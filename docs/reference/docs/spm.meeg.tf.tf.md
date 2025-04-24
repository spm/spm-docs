# Time-frequency analysis  
Perform time-frequency analysis of epoched M/EEG data.   

* **File Name** (select files)  
Select the M/EEG mat file.   

* **Channel selection** (create a list of items)  
Channel selection.   

    * **All**   
    .   

    * **Select channels by type** (choose from the menu)  
    Select channels by type.   

    * **Custom channel** (enter text)  
    Enter a single channel name.   

    * **Regular expression** (enter text)  
    Enter a regular expression for matching multiple channel labels.   

    * **Channel file** (select files)  
    .   

* **Frequencies of interest** (enter text)  
Frequencies of interest (as a vector), if empty 1-48 with optimal frequency bins ~1 Hz or above resolution   

* **Time window** (enter text)  
Time window (ms)   

* **Spectral estimation** (choose an option)  
Spectral estimation   

    * **Hilbert transform**   
    .   

        * **Frequency resolution** (enter text)  
        Frequency resolution.   
        Note: 1 Hz resolution means plus-minus 1 Hz, i.e. 2 Hz badwidth   
        Either a single value or a vector of the same length as frequencies can be input   

        * **Filter**   
        .   

            * **Filter type** (choose from the menu)  
            Select the filter type.   

            * **Filter direction** (choose from the menu)  
            Select the filter direction.   

            * **Filter order** (enter text)  
            Butterworth filter order   
            Either a single value or a vector of the same length as frequencies can be input   

        * **Polynomial trend removal order** (choose from the menu)  
        Order of polynome for trend removal (0 for no detrending)   

        * **Subsample** (enter text)  
        Set to N to subsample the time axis to every Nth sample (to reduce the dataset size).   

    * **Morlet wavelet transform**   
    .   

        * **Number of wavelet cycles** (enter text)  
        Number of wavelet cycles (a.k.a. Morlet wavelet factor).   
        This parameter controls the time-frequency trade-off   
        Increasing it increases the frequency resolution at the expense of time resolution.   

        * **Fixed time window length** (enter text)  
        Fixed time window for all frequencies.   
        Specify time window length in ms.   
        Default valued of 0 specifies variable time window length.   

        * **Subsample** (enter text)  
        Set to N to subsample the time axis to every Nth sample (to reduce the dataset size).   

    * **Multi-taper**   
    .   

        * **Taper** (choose from the menu)  
        Save taper as well as power   

        * **Time resolution** (enter text)  
        Length of the sliding time window (in ms)   

        * **Time step** (enter text)  
        Step to slide the time window by (in ms)   

        * **Frequency resolution** (enter text)  
        Frequency resolution.   
        Note: 1 Hz resolution means plus-minus 1 Hz, i.e. 2 Hz badwidth   
        Either a single value or a vector of the same length as frequencies can be input   

    * **Spectrum**   
    .   

        * **Taper** (choose from the menu)  
        Save taper as well as power   

        * **Frequency resolution** (enter text)  
        Frequency resolution.   
        Note: 1 Hz resolution means plus-minus 1 Hz, i.e. 2 Hz badwidth   
        Either a single value or a vector of the same length as frequencies can be input   

* **Save phase** (choose from the menu)  
Save phase as well as power   

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the output dataset.   
