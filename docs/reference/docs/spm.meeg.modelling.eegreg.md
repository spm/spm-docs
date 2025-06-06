# GLM regressors  
Generate regressors for GLM analysis of M/EEG data.   

* **File Name** (select files)  
Select the M/EEG mat file.   

* **Generation method** (create a list of items)  
Generation method   

    * **Channel data**   

        * **Regressor dataset name** (select files)  
        Select the M/EEG mat file containing regressor data   

        * **Channel selection** (create a list of items)  
        Channel selection.   

            * **All**   

            * **Select channels by type** (choose from the menu)  
            Select channels by type.   

            * **Custom channel** (enter text)  
            Enter a single channel name.   

            * **Regular expression** (enter text)  
            Enter a regular expression for matching multiple channel labels.   

            * **Channel file** (select files)  

        * **Time window** (enter text)  
        Start and stop of the time window [ms]. (Used only for the epoched case)   

    * **CTF head movements**   

        * **Movement dataset name** (select files)  
        Select the M/EEG mat file containing continuous head localisation data.   
        This might or might not be the same as the dataset being analysed.   

    * **Time-frequency phase**   

        * **TF phase dataset name** (select files)  
        Select the M/EEG mat file containing TF phase data   

        * **Channel selection** (create a list of items)  
        Channel selection.   

            * **All**   

            * **Select channels by type** (choose from the menu)  
            Select channels by type.   

            * **Custom channel** (enter text)  
            Enter a single channel name.   

            * **Regular expression** (enter text)  
            Enter a regular expression for matching multiple channel labels.   

            * **Channel file** (select files)  

        * **Time window** (enter text)  
        Start and stop of the time window [ms]. (Used only for the epoched case)   

        * **Frequency window** (enter text)  
        Start and stop of the frequency window (Hz).   

        * **Average over frequency** (choose from the menu)  
        Average over the frequency window to produce one regressor   
        or output each frequency as a separate regressor   

        * **Standardize** (choose from the menu)  
        Standardize (zscore) regressor values   

        * **Regressor name** (enter text)  
        Specify the string to be used as regressor name.   

    * **Time-frequency power**   

        * **TF power dataset name** (select files)  
        Select the M/EEG mat file containing TF power data   

        * **Channel selection** (create a list of items)  
        Channel selection.   

            * **All**   

            * **Select channels by type** (choose from the menu)  
            Select channels by type.   

            * **Custom channel** (enter text)  
            Enter a single channel name.   

            * **Regular expression** (enter text)  
            Enter a regular expression for matching multiple channel labels.   

            * **Channel file** (select files)  

        * **Time window** (enter text)  
        Start and stop of the time window [ms]. (Used only for summarised epoched cases)   

        * **Frequency window** (enter text)  
        Start and stop of the frequency window (Hz).   

        * **Average over frequency** (choose from the menu)  
        Average over the frequency window to produce one regressor   
        or output each frequency as a separate regressor   

        * **Standardize** (choose from the menu)  
        Standardize (zscore) regressor values   

        * **Regressor name** (enter text)  
        Specify the string to be used as regressor name.   

* **What to do for epoched inputs** (choose from the menu)  
For epoched files the output can either be summarised (one number per epoch)   
or concatenated (the same number of samples as all epochs combined)   

* **Output filename** (enter text)  
Choose file name for a mat file with regressors.   
