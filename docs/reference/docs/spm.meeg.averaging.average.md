# Averaging  
Average epoched EEG/MEG data.  

* **File Name** (select files)  
Select the M/EEG mat file.  

* **Averaging type** (choose an option)  
choose between using standard and robust averaging.  

    * **Standard**   
      

    * **Robust**   
      

        * **Offset of the weighting function** (enter text)  
        Parameter determining the how far the values should be from the median,   
        to be considered outliers (the larger, the farther).  

        * **Compute weights by condition** (choose from the menu)  
        Compute weights for each condition separately or for all conditions together.  

        * **Save weights** (choose from the menu)  
        Save weights in a separate dataset for quality control.  

        * **Remove bad data** (choose from the menu)  
        Replace data marked as bad by NaNs before averaging  
        Warning: beware if there is no good data, NaNs may stay in the average.  

* **Compute phase-locking value** (choose from the menu)  
Compute phase-locking value rather than average the phase  
This option is only relevant for TF-phase datasets.  

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the averaged dataset. Default prefix is 'm'.  
