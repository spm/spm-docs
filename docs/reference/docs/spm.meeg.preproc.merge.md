# Merging  
Merge EEG/MEG data.   

* **File Names** (select files)  
Select the M/EEG mat files.   

* **Condition label recoding rules** (create a list of items)  
Specify the rules for translating condition labels from the original files to the merged file. Multiple rules can be specified. The later rules have precedence. Trials not matched by any of the rules will keep their original labels. 

    * **Recoding rule**   
    Recoding rule. The default means that all trials will keep their original label.   

        * **Files to which the rule applies** (enter text)  
        Regular expression to match the files to which the rule applies (default - all)   

        * **Original labels to which the rule applies** (enter text)  
        Regular expression to match the original condition labels to which the rule applies (default - all)   

        * **New label for the merged file** (enter text)  
        New condition label for the merged file. Special tokens can be used as part of the name. \#file\# will be replaced by the name of the original file, \#labelorg\# will be replaced by the original condition labels.   

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the merged dataset. Default prefix is 'm'.   
