# Artefact detection  
Detect artefacts in epoched M/EEG data.

* **File Name** (select files)  
Select the M/EEG mat file.

* **Mode** (choose from the menu)  
Action mode reject - to set trials and channels as bad
mark - just create artefact events, set channels as bad if mostly artefactual.

* **Bad channel threshold** (enter text)  
Fraction of trials with artefacts 
above which an M/EEG channel is declared as bad.

* **Append** (choose from the menu)  
Append new artefacts to already marked or overwrite.

* **How to look for artefacts** (create a list of items)  
Choose channels and methods for artefact detection.

    * **Method**   
    .

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

        * **Detection algorithm** (choose an option)  
        .

            * **Reject based on events**   
            .

                * **What events to use?** (choose an option)  
                .

                    * **All artefact events**   
                    .

                    * **Load event list** (select files)  
                    Select events list file.

            * **Eyeblinks**   
            .

                * **Threshold** (enter text)  
                Threshold to reject things that look like eye-blinks but probably aren't.

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each eyeblink, 0 to not mark data as bad.

            * **Flat segments**   
            .

                * **Threshold** (enter text)  
                Threshold for difference between adjacent samples.

                * **Flat segment length** (enter text)  
                Minimal number of adjacent samples with the same value to reject.

            * **Heart beats**   
            .

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each heart beat, 0 to not mark data as bad.

            * **Difference between adjacent samples**   
            .

                * **Threshold** (enter text)  
                Threshold value to apply to all channels

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each jump (for mark mode only), 0 - do not mark data as bad.

            * **Detect NaNs**   
            .

            * **Peak to peak amplitude**   
            .

                * **Threshold** (enter text)  
                Threshold value to apply to all channels.

            * **Saccades**   
            .

                * **Threshold** (enter text)  
                Threshold to reject things that look like saccades but probably aren't.

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each saccade, 0 to not mark data as bad.

            * **Threshold channels**   
            .

                * **Threshold** (enter text)  
                Threshold value to apply to all channels.

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each jump (for mark mode only), 0 - do not mark data as bad.

            * **Threshold z-scored data**   
            .

                * **Threshold** (enter text)  
                Threshold value (in stdev)

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each event (for mark mode only), 0 - do not mark data as bad.

            * **Threshold z-scored difference data**   
            .

                * **Threshold** (enter text)  
                Threshold value (in stdev).

                * **Excision window** (enter text)  
                Window (in ms) to mark as bad around each event (for mark mode only), 0 - do not mark data as bad.

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the output dataset. Default prefix is 'a'.
