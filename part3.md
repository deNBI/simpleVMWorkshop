## Part 3: Inspect your generated data via a research environment

We now want to start a new VM. This time we would like to use an IDE like RStudio or VScode 
in order to inspect and visualize our results.

### 3.1 Create a VM based on a Research Environment

1. Start a new VM. This time select again the de.NBI default flavor since
   we do not need that much resources anymore.

2. In the image tab you can either start Rstudio (`RStudio-ubuntu1804`) or VSCode (`VSCode-ubuntu1804`).
   
3. In the volume tab please choose the volume you created
   in the previous part of the workshop.
   Please use again `/vol/data` as mountpath.
   ![](figures/reuseVolume.png)

4. Confirm all checkboxes and click on start.

5. On the instance overview, select `How to connect` of the newly started VM 
   and click on the URL. A tab should be opened up in your browser.

6. If you decided to use RStudio you can continue to chapter 3.2. If clicked
   on VSCode, please go to chapter 3.3.

### 3.2 RStudio

1. Login credentials for the RStudio user login are.
   ```
   Username: ubuntu  
   Password: simplevm
   ```

2. If you have chosen RStudio then please open a Terminal first by selecting
   `Tools` -> `Terminal` -> `New Terminal`.

3. Download the Script by running wget:
   
   
1. You might have to install markdown as indicated by the warning in the upper
   part of the editor. Just click on `install`.
2. Further you have to install the necessary libraries: 
   ```
   install.packages(c("ggplot2"))
   install.packages(c("RColorBrewer"))
   ```
3. You can now run the R notebook.

### 3.3 VSCode

1. If you have chosen VSCode

### 3.4 Todo: Create final image and volume for the reviewer