# Prepare  
Prepare EEG/MEG data.

* **File Name** (select files)  
Select the M/EEG mat file.

* **Select task(s)** (create a list of items)  
Select task(s).

    * **Set channel types to default**   
    Reset all channel types to default.

    * **Set channel types from BIDS tsv** (select files)  
    Select BIDS tsv file and set channel types based on it.

    * **Set channel type**   
    Select the new channel type to set.

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

        * **New channel type** (choose from the menu)  
        Select the new channel type to set.

    * **Load MEG sensors**   
    Reload MEG sensor representation from raw dataset.

        * **Select MEG dataset** (select files)  
        Select the MEG dataset to copy sensors from.

    * **Load MEG fiducials\headshape**   
    Load MEG fiducials or headshape from a file.

        * **Select MEG headshape file** (select files)  
        Select the file to read MEG headshape from.

        * **Fiducials** (create a list of items)  
        Specify at least 3 matching pairs of fiducials to coregister the surface points to the sensors.

            * **Matching pair**   
            Specify a matching pair of labeled point in the dataset and surface points.

                * **MEG fiducial label** (enter text)  
                Label of a fiducial point as specified in the MEG dataset.

                * **Surface point label** (enter text)  
                Label of a fiducial point as specified in the surface points file.

    * **Assign default EEG sensors**   
    Set EEG sensor locations to SPM template.

        * **Specify MEG fiducial labels**   
        ONLY for multimodal datasets the labels of MEG fiducials should be specified.

            * **Nasion** (enter text)  
            Enter the nasion fiducial label.

            * **Left** (enter text)  
            Enter the left fiducial label.

            * **Right** (enter text)  
            Enter the right fiducial label.

    * **Load EEG sensors**   
    Load EEG electrode locations.

        * **Select EEG sensors file** (select files)  
        Select the file with EEG electrode coordinates (e.g. Polhemus or SFP).

        * **Match fiducials to MEG** (choose an option)  
        Match EEG fiducials to MEG fiducials (only for multimodal datasets).

            * **Not necessary**   
            .

            * **Fiducials** (create a list of items)  
            Specify at least 3 matching pairs of fiducials to coregister the surface points to the sensors.

                * **Matching pair**   
                Specify a matching pair of labeled point in the dataset and surface points.

                    * **MEG fiducial label** (enter text)  
                    Label of a fiducial point as specified in the MEG dataset.

                    * **Surface point label** (enter text)  
                    Label of a fiducial point as specified in the surface points file.

    * **Define EEG referencing** (choose an option)  
    Define the way EEG channels were derived from sensors.

        * **Select reference sensors**   
        Select the sensors to which the EEG recording was referenced
        (select 'All' for average reference).

            * **Channel selection** (create a list of items)  
            Channel selection.

                * **All**   
                .

                * **Custom channel** (enter text)  
                Enter a single channel name.

                * **Regular expression** (enter text)  
                Enter a regular expression for matching multiple channel labels.

                * **Channel file** (select files)  
                .

        * **Select montage file** (select files)  
        Select montage file that specifies the referencing.

    * **Project EEG sensors to 2D**   
    Project EEG sensor locations to 2D.

    * **Project MEG sensors to 2D**   
    Project MEG sensor locations to 2D.

    * **Load channel template file** (select files)  
    Specify 2D channel locations by loading a template file.

    * **Set/unset bad channels**   
    Set or clear bad flag for channels.

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

        * **Status to set** (choose from the menu)  
        Select the new channel type to set.

    * **Set bad channels from BIDS tsv** (select files)  
    Select BIDS tsv file and set channel status based on it.

    * **Create average reference montage**   
    Create an average reference montage for the specific dataset, 
    taking into account bad channels.

        * **Output file name** (enter text)  
        .

    * **Sort conditions** (choose an option)  
    Specify conditions order.

        * **Specify conditions list** (create a list of items)  
        Specify the conditions order manually as a list.

            * **Condition label** (enter text)  
            .

        * **Read from conditions file** (select files)  
        Select a mat-file with cell array of condition labels.

    * **Load events from BIDS tsv file**   
    Load events from BIDS tsv file.

        * **BIDS tsv file** (select files)  
        File to load events from

        * **Replace existing events** (choose from the menu)  
        Replace existing events or add to them
