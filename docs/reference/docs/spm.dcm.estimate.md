# DCM estimation  
Estimate the parameters and free energy (log model evidence) of first level DCMs for fMRI. Models are assembled into a Subjects x Models array and saved in group GCM_*.mat file   

* **Select DCMs** (choose an option)  
Select one DCM per subject, multiple DCMs per subject or an existing group DCM file.   
If multiple DCMs are selected per subject, then the first DCM for each subject should a 'full' model containing all connections of interest. Subsequent (nested) DCMs will have certain connections switched off.   

    * **Per model** (create a list of items)  
    Select DCM.mat files per model   

        * **Model**   
        Corresponding model for each subject   

            * **Select DCM_*.mat** (select files)  
            Select DCM_*.mat files.   

    * **Per subject** (create a list of items)  
    Create the subjects and select the models for each   

        * **Subject**   
        Subject with one or more models.   

            * **Select DCM_*.mat** (select files)  
            Select DCM_*.mat files.   

    * **Select GCM_*.mat** (select files)  
    Select GCM_*.mat files.   

* **Output** (choose an option)  
How to store the estimated DCMs. The options are:    
1. Create group GCM_*.mat file - this will create a single file containing a cell array, with one row per subject and one column per DCM, containing the filenames of the DCMs. The original DCM files will be overwritten with the outcome of the estimation. (Alternatively, if the input is a GCM file containing DCM structures rather than filenames, then the output will also be an array containing DCM structures.)   
2. Overwrite existing GCM/DCM files - as above but with with an existing GCM filename.   
3. Only save individual DCM files - does not change the GCM file and simply overwrites each subject's DCM with estimated values.   

    * **Create group GCM_*.mat file**   
    Creates a single group-level DCM file containing a subjects x models cell array.   

        * **Directory** (select files)  
        Select the directory where the output will be written.   

        * **Name** (enter text)  
        Specify a name for the group DCM file. The prefix GCM_and suffix .mat are automatically added   

    * **Overwrite existing GCM/DCM files**   
    Overwrites existing group-level DCM file with estimated models.   

    * **Only save individual DCM files**   
    Updated existing individual DCM.mat files   

* **Estimation type** (choose from the menu)  
Full + BMR: Estimates the full (first) model for each subject then uses Bayesian Model Reduction (BMR) to rapidly infer the evidence / parameters for any subsequent nested models.   
Full + BMR PEB: Iteratively estimates the full (first) model for each subject, then sets the priors on each parameter to the group mean (from a PEB model) then re-estimates. This improves estimation by overcoming local optima, but takes longer.   
Full: Estimates all models individually. Provided for backward compatibility.   
None: Creates a group level DCM file without performing estimation   

* **MRI specific options**   
MRI specific options   

    * **Analysis** (choose from the menu)  
    Whether to analyse in the time domain (for task-based studies or stochastic DCM) or in the frequency domain (for resting state analysis with DCM for CSD   
