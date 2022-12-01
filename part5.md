## Part 5: Scale further up your analysis 

In this part of the tutorial you will scale your cluster horizontally by using a SimpleVM Cluster.
A SimpleVM Cluster consists of a master and multiple worker nodes.
In the following you will configure a cluster and submit your tools
to a SLURM job scheduler.

TODO: Write about SLURM workload manager

### 5.1 Create a Cluster

1. Click on "New Cluster" on the left menu.
   If you can not see the "New Cluster" item then reload the page.

2. Since your master node is just used for submitting jobs, please select *de.NBI mini* as flavor and
   the snapshot you created in the previous parts as image.
   ![](./figures/clusterMasterImage.png)

3. The worker nodes will run the actual tools, so we need a flavor wir more cores then the one that the master node is
   using. Therefore, please select *de.NBI large* as flavor and the same snapshot as in the previous part.

4. Now click on Start! Thats it! Just with a few clicks you started your own cluster.

### 5.2 Investigate your cluster setup

1. Click on the Clusters tab (Overview -> Clusters). After you have initiated the start-up of the cluster,
   you should have been automatically redirected there. Now open the "How to connect"
   dropdown of your machine. Click on the Theia ide URL which opens a new browser tab.

2. Click on `Terminal` in the upper menu and select `New Terminal`.
   ![](figures/terminal.png)

3. Check how many nodes are part of your cluster by using `sinfo`

```
sinfo
```
which will produce the following example output
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      2   idle bibigrid-worker-1-1-us6t5hdtlklq7h9,bibigrid-worker-1-2-us6t5hdtlklq7h9
```

The important columns here are `STATE` which tells you if the worker nodes are processing jobs
or are just in `idle` state and the column `NODELIST` which is just a list of nodes.

4. You could now submit a job and test if your cluster is working as expected.
   `/vol/spool` is the folder which is shared between all nodes. You should always submit jobs
   from that directory.
   ```
   cd /vol/spool
   ```
   
5. Download the following script via `wget`.
   ```
   wget https://openstack.cebitec.uni-bielefeld.de:8080/simplevm-workshop/genomes.msh
   ```
   
   The script contains the following content:
   ```
   #!/bin/bash
   
   #Do not do anything for 30 seconds 
   sleep 30
   #Print out the name of the machine where the job was executed
   hostname
   ```
   where
    * `sleep 30` will delay the process for 30 seconds.
    * `hostname` reports the name of the worker node.

6. You could now submit the job to the SLURM scheduler by using `sbatch` and directly after that
   check if your SLURM is executing your script with `squeue`.

   sbatch:
   ```
   sbatch basic.sh
   ```
   
   squeue:
   ```
   squeue
   ```
   which will produce the following example output:
   ```
   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   212     debug basic.sh   ubuntu  R       0:03      1 bibigrid-worker-1-1-us6t5hdtlklq7h9
   ```
   Squeue tells you the state of your jobs and which nodes are actually executing them.
   In this example you should see that `bibigrid-worker-1-1-us6t5hdtlklq7h9` is running (`ST` column) your job
   with the name `basic.sh`.

7. Once the job has finished you should see a slurm output file in your directory (Example: `slurm-212.out`)
   which will contain the name of the worker node which executed your script.
   Open the file with the following command:
   ```
   cat slurm-*.out
   ```
   Example output:
   ```
   bibigrid-worker-1-1-us6t5hdtlklq7h9
   ```

8. One way to distribute jobs is to use so-called array jobs. With array jobs you specify how many times
   your script should be executed. Every time the script is executed, a number between 0 and the number of times
   you want the script to be executed is assigned to the script execution. The specific number is saved in a
   variable (`SLURM_ARRAY_TASK_ID`). If you specify `--array=0-100` then your script is 100 times executed and
   the `SLURM_ARRAY_TASK_ID` variable will get a value between 0 and 100. SLURM will distribute the
   jobs on your cluster.

   Please fetch the modified script
   Todo: Add link

   Which is simply reading out the `SLURM_ARRAY_TASK_ID` variable and placing them in a file in an
   output directory:

   ```
   #!/bin/bash
   
   # Create output directory in case it was not created so far
   mkdir -p ouptput_array
   
   #Do not do anything for 10 seconds 
   sleep 10
   
   #Create a file with the name of SLURM_ARRAY_TASK_ID content. 
   touch output_array/${SLURM_ARRAY_TASK_ID}
   ```
 
   You can execute this script a 100 times with the following command 
   ```
   sbatch --array=0-100 basic_array.sh
   ```
   
   If you now check the `output_array` folder, you should see numbers from 0 to 100.
   ```
   ls output_array
   ```

### 5.3 Scan the SRA for genomes

1. We can now reuse the `search` function of the third part and submit an array job with the number of 
datasets we want to scan.

Please download the updated script which looks like this:

```
#!/bin/bash

eval "$(conda shell.bash hook)"
conda activate denbi
# Add S3 SRA OpenStack Config
mc config host add sra https://openstack.cebitec.uni-bielefeld.de:8080 "" ""

# Search function
search(){
   left_read=$(echo $1 | cut -d ' '  -f 1);  
   right_read=$(echo $1 | cut -d ' ' -f 2);
   sra_id=$(echo ${left_read} | rev | cut -d '/' -f 1 | rev | cut -d '_' -f 1 | cut -d '.' -f 1);
   mc cat $left_read $right_read | mash screen -p 3 genomes.msh - \
        | sed "s/^/${sra_id}\t/g"  \
        | sed 's/\//\t/' > output/${sra_id}.txt ;
}

LINE_NUMBER=${SLURM_ARRAY_TASK_ID}
LINE=$(sed "${LINE_NUMBER}q;d" reads.tsv)

search ${LINE} 
```
TODO where
  * `eval "$(conda shell.bash hook)"` reuses 

TODO Download the input file.

2. Run the actual analysis with

TODO:
```
sbatch...
```

