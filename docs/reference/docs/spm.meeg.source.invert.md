# Source inversion  
Run imaging source reconstruction  

* **M/EEG datasets** (select files)  
Select the M/EEG mat files.  

* **Inversion index** (enter text)  
Index of the cell in D.inv where the forward model can be found and the results will be stored.  

* **What conditions to include?** (choose an option)  
What conditions to include?  

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

        * **Inversion type** (choose from the menu)  
        Select the desired inversion type  

        * **Time window of interest** (enter text)  
        Time window to include in the inversion (ms)  

        * **Frequency window of interest** (enter text)  
        Frequency window (the same as high-pass and low-pass in the GUI)  

        * **PST Hanning window** (choose from the menu)  
        Multiply the time series by a Hanning taper to emphasize the central part of the response.  

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

            * **Mask image** (select files)  
            Select a mask image  

* **Select modalities** (choose from the menu)  
Select modalities for the inversion (only relevant for multimodal datasets).  
