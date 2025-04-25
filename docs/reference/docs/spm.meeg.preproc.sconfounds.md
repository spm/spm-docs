# Define spatial confounds  
Define spatial confounds for topography-based correction of artefacts.   

* **File Name** (select files)  
Select the EEG mat file.   

* **Mode** (create a list of items)  
Select methods for defining spatial confounds.   

    * **SVD**   
    Define confounds from SVD of artefact samples.   

        * **Time window** (enter text)  
        Time window (ms)   

        * **Conditions** (create a list of items)  
        Specify the labels of the conditions to include in the SVD.   

            * **Condition label** (enter text)  

        * **Threshold** (enter text)  
        Threshold for data amplitude after correction.   
        If defined this overrides number of components.   

        * **Number of components** (enter text)  
        Number of confound components to keep.   

    * **SPM M/EEG dataset**   
    Load confounds defined in a different dataset.   

        * **File name** (select files)  
        Select the EEG mat file.   

    * **BESA**   
    Load confounds defined in BESA from a .bsa file.   

        * **BESA confounds file** (select files)  
        Select the EEG mat file.   

    * **Eyes**   
    Defined confounds based on standard locations for the eyes   
    This is a very crude method, a data-driven method is preferable   

    * **Clear**   
    Clear previously defined spatial confounds.   
