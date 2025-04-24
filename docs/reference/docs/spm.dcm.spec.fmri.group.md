# Specify group  
Duplicates template DCM(s) for each subject.
Before running this, create one or more example DCMs for one subject using the Dynamic Causal Modelling button in the main SPM window. Then use this batch to duplicate the DCM(s) for each subject. The output is a file containing a cell array, with one row per subject and one column per DCM (named GCM_<name>.mat).

* **Output**   
Select where the group DCM (GCM) file containing all the DCMs' filenames will be stored.

    * **Output directory** (select files)  
    Select the directory where the output will be written.

    * **Output name** (enter text)  
    Specify a name for the group DCM file. The prefix 'GCM_' and suffix '.mat' are added automatically.

* **Template DCMs**   
Template DCMs to replicate over subjects.

    * **Full DCM** (select files)  
    Select a DCM .mat file, specified using the GUI for a single subject. This should be a 'full' model with all parameters of interest (e.g. connections) enabled.

    * **Alternative DCMs** (select files)  
    Select alternative DCMs, specified using the GUI for a single subject. These should be reduced or nested versions of the full template DCM, e.g. with some connections switched off (fixed at their priors).

* **Design & data**   
Experimental timing and timeseries for the DCMs.

    * **SPM.mat files** (select files)  
    Select all subjects' SPM.mat files

    * **Which session** (enter text)  
    Enter the session (run) number.

    * **Regions of interest** (create a list of items)  
    Select the regions of interest (VOIs) for all subjects

        * **Region (VOI files)** (select files)  
        Select all subjects' VOI_*.mat for this region.
