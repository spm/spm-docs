# fMRI data preprocessing

## Bringing it all together - the batch interface

The SPM batch interface allows you to create workflows for executing multiple processing steps sequentially. This means that you can set up one batch file that will contain all preprocessing steps that you want to perform. 

1. From the SPM menu panel, select `Batch`. You will see a pop-up window of an empty batch interface looking like this:

    ![](../../assets/figures/batch.png)

2. Now we will start adding each of our preprocessing steps, starting with realignment. 
3. Within the batch window, select `SPM` :material-arrow-right-bold: `Spatial` :material-arrow-right-bold: `Realign` :material-arrow-right-bold: `Realign (estimate & reslice)`. 
