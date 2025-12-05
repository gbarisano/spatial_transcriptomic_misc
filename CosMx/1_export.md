To start working with CosMx data, you will need to export them from the AtoMx portal to your working station or cluster.\
Once a flow cell has been created and is available in the AtoMx portal, you can look at it and visually assess the segmentation of cells.
Click on the icon indicated below to have the cell boundaries overlaid on your image.

https://github.com/user-attachments/assets/920fd36b-1f40-4d5d-aa65-cad1b9a90c13

If the segmentation does not look good, then click on "Segment Cells" in the FOV menu within the flow cell (right-most blue button in left panel shown in the video above). Here you will be able to access the parameters to change for improving the segmentation. These parameters will strongly depend on the type of tissue that you are analyzing, and you can have a look at the [CosMx SMI Data Analysis User Manual](https://university.nanostring.com/cosmx-smi-data-analysis-user-manual) for details. In my experience with human kidney samples, the initial default segmentation was not sufficiently good, and required the use of Model A, with the channel U (DNA) set up for nuclei and the channel Y (membrane) set up for morphology.\
After the update of CosMx to version 2.1 in summer 2025, I noted improvements in the default segmentation for human kidney samples, and no additional re-segmentation was required.

When the segmentation looks good, go back to the Home page, select that flow cell, and click on "create a study". In my experience, this usually takes 15-20 minutes and you should receive an email once the process is completed.\
Once the study is created, you can access your study folder and click on the 'export' option (blue button on the top left). In my experience, this usually takes between 1 and 2 hours.\
If you do not have storage issues, tick all the files and click on 'Export' on the bottom right. At the minimum, I would say you will need either the Seurat object or the flat CSV files.\

Once the export successfully finished, you can click on "Show export details" to retrieve the information needed to access and export the data.
You will need the following 3 information:
```
Hostname: na.export.atomx.nanostring.com
Username: youremail@stanford.edu
Output Folder Name: yourstudy_DD_MM_YYYY_HH_MM_SS_XXX
```
Please note that in some instances (for example when the study name includes dashes), the Output Folder Name shown may not correspond to the actual output folder name. For example, if the study name includes a dash, the dash may have been removed and the real output folder name may be without the dash. You can verify the actual Output Folder Name by connecting directly to the AtoMx server with the following ```bash``` command:\
```sftp youremail@stanford.edu@na.export.atomx.nanostring.com```\
and then use the command ```ls``` to list the name of the output folder(s) available to you.

After ensuring you have ```Hostname```, ```Username```, and ```Output Folder Name```available, you can either copy the folder from the AtoMx server to your local machine with the following ```bash``` command:
```scp -r youremail@stanford.edu@na.export.atomx.nanostring.com:/yourstudy_DD_MM_YYYY_HH_MM_SS_XXX /your/output/directory```

Or you can use tools like [rclone](https://www.sherlock.stanford.edu/docs/software/using/rclone/) to transfer files from the AtoMx server to your own cloud. This way you can save storage on your local machine.
You will need to set up the AtoMx server in your [rclone configuration](https://www.sherlock.stanford.edu/docs/software/using/rclone/#rclone-config).
The ```bash``` command to export the folder from AtoMx server with ```rclone``` would be something like the following:\
```rclone copy -v atomx:/yourstudy_DD_MM_YYYY_HH_MM_SS_XXX your_cloud:/your/output/directory```\
The transfer of 1 study with 1 TMA via ```rclone``` in my experience typically takes between 30 minutes and 1 hour. The other advantage of using ```rclone``` to export is that you can submit the transfer as a job if you are in a cluster environment. This way you will not need to remain online during the transfer.




