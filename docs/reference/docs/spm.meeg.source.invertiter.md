# Source inversion, iterative  
Run imaging source reconstruction   

* **M/EEG datasets** (select files)  
Select the M/EEG mat files.   

* **Inversion index** (enter text)  
Index of the cell in D.inv where the forward model can be found and the results will be stored.   

* **What conditions to include?** (choose an option)  
.   

    * **All**   
    .   

    * **Conditions** (create a list of items)  
    Specify the labels of the conditions to be included in the inversion   

        * **Condition label** (enter text)  
        .   

* **Inversion parameters** (choose an option)  
Choose whether to use standard or custom inversion parameters.   

    * **Standard**   
    Use default settings for the inversion   

    * **Custom**   
    Define custom settings for the inversion   

        * **Inversion function** (choose from the menu)  
        Current code allows multiple subjects and modalities; classic is for single subject single modality but has no additional scaling factors   

        * **Inversion type** (choose from the menu)  
        Select the desired inversion type   

        * **Time window of interest** (enter text)  
        Time window to include in the inversion (ms)   

        * **Frequency window of interest** (enter text)  
        Frequency window (the same as high-pass and low-pass in the GUI)   

        * **PST Hanning window** (choose from the menu)  
        Multiply the time series by a Hanning taper to emphasize the central part of the response.   

        * **Patch definition** (choose an option)  
        Choose whether to use random or fixed patch centres.   

            * **Random Patches**   
            Define random patches   

                * **Number of randomly selected patches** (enter text)  
                Number of randomly centred patches (priors) on each iteration   

                * **Number of iterations** (enter text)  
                Number of iterations   

            * **Fixed Patches**   
            Define fixed patches   

                * **Patch definition file ** (select files)  
                Select patch definition file (mat file with variable Ip: rows are iterations columns are patch indices    

                * **Rows from fixed patch file** (enter text)  
                Select first and last row of patch sets to use from file   

        * **Patch smoothness** (enter text)  
        Width of priors in cortex arb units (see inverse.smoothmm to see FWHM mm   

        * **Selction of winning model** (choose from the menu)  
        How to get the final current density estimate from multiple iterations   

        * **Number of spatial modes** (enter text)  
        Number of spatial modes. Zero for auto-select   

        * **File containing spatial mode definition** (select files)  
        File (SPMU..) containing spatail mode definition, if no file is supplied bases these modes on lead fields   

        * **Number of temporal modes** (enter text)  
        Number of temporal modes. Zero for auto-select.   

        * **Source priors**   
        Restrict solutions to pre-specified VOIs   

            * **Priors file** (select files)  
            Select a mask or a mat file with priors.   

            * **Prior image space** (choose from the menu)  
            Space of the mask image.   

        * **Restrict solutions**   
        Restrict solutions to pre-specified VOIs   

            * **Source locations** (enter text)  
            Input source locations as n x 3 matrix   

            * **Radius of VOI (mm)** (enter text)  
            .   

        * **Output prefix to save a copy of inv structure** (enter text)  
        If this is supplied the original dataset will still be updated but new inverse structure with this name created   

* **Select modalities** (choose from the menu)  
Select modalities for the inversion (only relevant for multimodal datasets).   

* **Cross validation parameters** (enter text)  
Percentage of cross validation test trials (eg 10 for 10 percent test trials, 0 for no cross val), number of cross val permutations. If number permutations*percentage equals 100, then make unique, non-repeating, selection of channels.   
