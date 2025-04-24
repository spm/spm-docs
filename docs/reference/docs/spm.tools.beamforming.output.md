# Output  
Compute output measures  

* **BF.mat file** (select files)  
Select BF.mat file.  

* **Output method** (choose an option)  
  

    * **Phase Lag Index**   
      

        * **Summary method** (choose from the menu)  
        How to summarise sources in the reference ROI  

        * **Filter reconstructed sources**   
          

            * **Highpass cutoff** (enter text)  
            Highpass filter cutoff (4th order butterworth)  

            * **Lowpass cutoff** (enter text)  
            Lowpass filter cutoff (4th order butterworth)  

        * **Redefine reference VOIs** (create a list of items)  
        Select the source VOI from which you would like to compute the PLI  

            * **VOI**   
              

                * **Label** (enter text)  
                Label for the reference VOI  

                * **MNI coordinates** (enter text)  
                Locations for the reference VOI in MNI coordinates  

                * **Radius** (enter text)  
                Radius (in mm) for the reference VOIs (leave 0 for single point)  

            * **Mask VOI**   
              

                * **Label** (enter text)  
                Label for the reference VOI  

                * **MNI mask** (select files)  
                Select a reference mask image  

            * **Source file**   
            Select an M/EEG file with channels corresponding to reference sources. Otherwise this file must exactly correspond to the file on which the BF was computed  

                * **Label** (enter text)  
                Label for the reference VOI  

                * **Source file** (select files)  
                Select the M/EEG data file  

    * **cross-frequency GLM image**   
      

        * **What conditions to include?** (choose an option)  
          

            * **All**   
              

            * **Conditions** (create a list of items)  
            Specify the labels of the conditions to be included in the inversion  

                * **Condition label** (enter text)  
                  

        * **Trials same as for filters** (choose from the menu)  
        Take the same trials as used for filter computation  
        This is useful for bootstrap.  

        * **Time window of interest** (enter text)  
        Time windows (in ms)  

        * **Phase frequencies** (enter text)  
        Frequencies to compute phase for (as a vector)  

        * **Phase resolution** (enter text)  
        Frequency resolution for phase computation  

        * **Low Amplitude resolution** (enter text)  
        Frequency resolution for amplitude computation at low frequencies  

        * **Amplitude frequencies** (enter text)  
        Frequencies to compute amplitude for (as a vector)  

        * **Amplitude resolution** (enter text)  
        Frequency resolution for amplitude computation  

        * **Reference type** (choose an option)  
          

            * **Within source.**   
            Within source PAC (no reference)  

            * **Reference channel**   
              

                * **Channel name** (enter text)  
                Reference channel name.  

                * **Reference feature** (choose from the menu)  
                What to take from the reference  

        * **Modality** (choose from the menu)  
        Specify modality  

        * **Number of dipole orientations for AMPLITUDE** (enter text)  
        Number of dipole orientations for each MEG source for amplitude  

        * **Number of dipole orientations for PHASE** (enter text)  
        Number of dipole orientations for each MEG source for phase  

        * **Name output images** (enter text)  
        To specify details that will be added to the output images file names.  

    * **DICS image**   
      

        * **Reference type** (choose an option)  
          

            * **Power (no reference)**   
            Compute power image  

            * **Reference channel**   
              

                * **Channel name** (enter text)  
                Reference channel name.  

                * **Shuffle** (choose from the menu)  
                Shuffle the reference channel to produce the null case.  

            * **Reference source** (enter text)  
            Location of the reference in MNI coordinates  

        * **Power summary method** (choose from the menu)  
        How to summarise the power for vector beamformer  

        * **What conditions to include?** (choose an option)  
          

            * **All**   
              

            * **Conditions** (create a list of items)  
            Specify the labels of the conditions to be included in the inversion  

                * **Condition label** (enter text)  
                  

        * **Trials same as for filters** (choose from the menu)  
        Take the same trials as used for filter computation  
        This is useful for bootstrap.  

        * **Time windows of interest** (enter text)  
        Time windows (in ms)  

        * **Time contrast** (enter text)  
          

        * **Take log of power** (choose from the menu)  
        Take the log of power before computing time contrast  
        This is equivalent to log of the ratio.  

        * **Frequency band of interest** (enter text)  
        Frequency window within which to compute CSD over (Hz)  

        * **Taper** (choose from the menu)  
        Save taper as well as power  

        * **What to output** (choose from the menu)  
        Specify output type.  

        * **Unit-noise-gain** (choose from the menu)  
        Scale power by norm of the filters (unit-noise-gain)  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Filter correlations image**   
      

        * **Seed specification** (choose an option)  
          

            * **Seed MNI coordinates** (enter text)  
            Locations for the seed in MNI coordinates (closest point is chosen  

            * **Label** (enter text)  
            Label for source of interest  

        * **Correlation type** (choose from the menu)  
        Whether to correlate filters with other filters of with other leadfields  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Gain image**   
      

        * **Output type** (choose from the menu)  
        The calculation that should be performed  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Kurtosis image**   
      

        * **What conditions to include?** (choose an option)  
          

            * **All**   
              

            * **Conditions** (create a list of items)  
            Specify the labels of the conditions to be included in the inversion  

                * **Condition label** (enter text)  
                  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Mv image**   
      

        * **Design matrix parameters** (choose an option)  
        Choose whether to load custom design  

            * **design matrix** (select files)  
            Select the design matrix  

            * **Custom**   
            Define custom settings for the inversion  

                * **What conditions to include?** (choose an option)  
                  

                    * **All**   
                      

                    * **Conditions** (create a list of items)  
                    Specify the labels of the conditions to be included in the inversion  

                * **Time windows of interest** (enter text)  
                Time windows (in ms)  

        * **Features** (choose from the menu)  
        Data features of interest  

        * **Frequency bands of interest** (enter text)  
        Freq bands (in Hz)  

        * **What to output** (choose from the menu)  
        Specify output type  

        * **Trials same as for filters** (choose from the menu)  
        Take the same trials as used for filter computation  
        This is useful for bootstrap.  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **PAC image**   
      

        * **What conditions to include?** (choose an option)  
          

            * **All**   
              

            * **Conditions** (create a list of items)  
            Specify the labels of the conditions to be included in the inversion  

                * **Condition label** (enter text)  
                  

        * **Trials same as for filters** (choose from the menu)  
        Take the same trials as used for filter computation  
        This is useful for bootstrap.  

        * **Shuffle** (enter text)  
        Shuffle the reference channel to produce the null case.  
        Specify the number of shufflings  

        * **Time window of interest** (enter text)  
        Time windows (in ms)  

        * **Phase frequencies** (enter text)  
        Frequencies to compute phase for (as a vector)  

        * **Phase resolution** (enter text)  
        Frequency resolution for phase computation  

        * **Amplitude frequencies** (enter text)  
        Frequencies to compute amplitude for (as a vector)  

        * **Amplitude resolution** (enter text)  
        Frequency resolution for amplitude computation  

        * **Reference type** (choose an option)  
          

            * **Within source.**   
            Within source PAC (no reference)  

            * **Reference channel**   
              

                * **Channel name** (enter text)  
                Reference channel name.  

                * **Reference feature** (choose from the menu)  
                What to take from the reference  

        * **Modality** (choose from the menu)  
        Specify modality  

        * **Number of dipole orientations for AMPLITUDE** (enter text)  
        Number of dipole orientations for each MEG source for amplitude  

        * **Number of dipole orientations for PHASE** (enter text)  
        Number of dipole orientations for each MEG source for phase  

        * **Name output images** (enter text)  
        To specify details that will be added to the output images file names.  

    * **Power correlations image**   
      

        * **What conditions to include?** (choose an option)  
          

            * **All**   
              

            * **Conditions** (create a list of items)  
            Specify the labels of the conditions to be included in the inversion  

                * **Condition label** (enter text)  
                  

        * **Trials same as for filters** (choose from the menu)  
        Take the same trials as used for filter computation  
        This is useful for bootstrap.  

        * **Shuffle** (enter text)  
        Shuffle the reference channel to produce the null case.  
        Specify the number of shufflings  

        * **Time window of interest** (enter text)  
        Time windows (in ms)  

        * **Reference channel** (enter text)  
        Reference channel name.  

        * **Reference frequencies** (enter text)  
        Frequencies in the reference channel  

        * **Reference resolutions** (enter text)  
        Frequency resolution for reference frequencies. Single value or vector  

        * **Data frequencies** (enter text)  
        First set of frequencies  

        * **Data resolutions** (enter text)  
        Frequency resolution for data frequencies. Single value or vector  

        * **Moving average window** (enter text)  
        Time window for moving average of power envelope (ms).  
        Specify 0 to not average  

        * **Subsample** (enter text)  
        Set to N to subsample the power to every Nth sample  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Power image**   
      

        * **What conditions to include?** (choose an option)  
          

            * **All**   
              

            * **Conditions** (create a list of items)  
            Specify the labels of the conditions to be included in the inversion  

                * **Condition label** (enter text)  
                  

        * **Trials same as for filters** (choose from the menu)  
        Take the same trials as used for filter computation  
        This is useful for bootstrap.  

        * **Time windows of interest** (enter text)  
        Time windows (in ms). N.B. Make sure these windows lie within the covariance window  

        * **Frequency bands of interest** (enter text)  
        Frequency windows within which to power changes over (Hz) .N.B. Check this lies within covariance window  

        * **Time contrast** (enter text)  
          

        * **Take log of power** (choose from the menu)  
        Take the log of power before computing time contrast  
        This is equivalent to log of the ratio.  

        * **What to output** (choose from the menu)  
        Specify output type.  

        * **Unit-noise-gain** (choose from the menu)  
        Scale power by norm of the filters (unit-noise-gain)  

        * **Power summary method** (choose from the menu)  
        How to summarise the power for vector beamformer  

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Sensitivity image**   
      

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
              

        * **Modality** (choose from the menu)  
        Specify modality  

    * **Source montage**   
      

        * **Summary method** (choose from the menu)  
        How to summarise sources in the ROI  

        * **Redefine VOIs** (create a list of items)  
        This makes it possible to define new VOIs when the original source space was mesh or grid.  
        Only the sources present in the original source space can be used at this stage  

            * **VOI**   
              

                * **Label** (enter text)  
                Label for the VOI  

                * **MNI coordinates** (enter text)  
                Locations for the VOI in MNI coordinates  

                * **Radius** (enter text)  
                Radius (in mm) for the VOIs (leave 0 for single point)  

            * **Mask VOI**   
              

                * **Label** (enter text)  
                Label for the VOI  

                * **MNI mask** (select files)  
                Select a mask image  

    * **Source data (robust)**   
      

        * **Summary method** (choose from the menu)  
        How to summarise sources in the ROI  
