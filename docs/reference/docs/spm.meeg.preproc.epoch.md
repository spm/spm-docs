# Epoching  
Epoch continuous EEG/MEG data.   

* **File Name** (select files)  
Select the M/EEG mat file.   

* **How to define trials** (choose an option)  
Choose one of the two options how to define trials.   

    * **Trial definition file** (select files)  
    Select the trialfile mat file.   

    * **Define trial**   


        * **Time window** (enter text)  
        Start and end of epoch [ms].   

        * **Trial definitions** (create a list of items)  


            * **Trial**   


                * **Condition label** (enter text)  


                * **Event type** (enter text)  


                * **Event value** (enter text)  


                * **Shift** (enter text)  
                shift the triggers by a fixed amount (ms)   
                (e.g. projector delay).   

    * **Arbitrary trials**   
    Epoch the data in arbitrary fixed length trials (e.g. for spectral analysis).   

        * **Trial length** (enter text)  
        Arbitary trial length [ms].   

        * **Condition label** (enter text)  


* **Baseline correction** (choose from the menu)  
Perform baseline correction when epoching, or not.   

* **Event padding** (enter text)  
In seconds: the additional time period around each trialfor which the events are saved with the trial (to let the user keep and use for analysis events which are outside trial borders). Default is 0 s.   

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the epoched dataset. Default prefix is 'e'.   
