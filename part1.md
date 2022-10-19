## Part 1: Big things start small

Since you are new to SimpleVM, it is resource-saving to start with a VM that
has a few cores and a small amount of RAM.

You start this tutorial from your profile page (https://cloud.denbi.de/portal/).

### 1.1 Set an SSH key in your account

1. Click on the `New Instance` tab.

2. If you have no SSH key set so far, just click on generate key and save the
private key. During this workshop you will not need this file because 
you will access all VMs via the browser. However, for your future work using
SimpleVM, we highly recommend to read our de.NBI Cloud wiki regarding
SSH keys: https://cloud.denbi.de/wiki/portal/user_information/#ssh-key

### 1.2 Start a VM

1. Choose a name for your VM.
2. Select **de.NBI default** flavor and **TheiaIDE-ubuntu1804** image.
3. Select the Conda tab and select the following tools for installation: 
   * ncbi-genome-download
   * mash
   * csvtk
   * entrez-direct
   * jq
   ![](figures/bioconda.png)
4. Select a URL path for Theia. You will access Theia via this URL.
   ![](figures/researchenvironment_url.png)
5. Confirm the checkboxes and click on Start.