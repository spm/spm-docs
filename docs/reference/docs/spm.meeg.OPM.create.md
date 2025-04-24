# Create OPM object  
Create/simulate OPM data. All arguments for this function are optional. It allows for either a conversion of raw data to valid MEG object or for the simulation of OPM data on a template brain or an individual brain with customisable sensor configurations.

* **Sensor Info**   
Input arguments required for sensor level analysis

    * **OPM dataset** (select files)  
    Select the meg.bin file. If left empty an empty dataset will be created according to the simulation parameters

    * **channels.tsv file** (select files)  
    Select the channels .tsv file. The format of this file should conform to the BIDS standard for channels.tsv files

    * **meg.json file** (select files)  
    Select the meg  json file.  The format of this file should conform to the BIDS standard for meg.json files

* **Simulation Parameters**   
Parameters for simulating OPM data. will be ignored if a dataset has already been supplied

    * **Sensor Coverage** (enter text)  
    If value is set to 1 then sensors will be placed all over scalp surface. 

    * **Desired Space between Sensors** (enter text)  
    This is only an approximation, units are mm

    * **Sensor Offset** (enter text)  
    Scalp to sensor Distance, units are mm

    * **Number of samples** (enter text)  

* **Source level info**   
Input arguments necessary for performing source space analysis

    * **Coordinate systems** (select files)  
    json file describing the coordinate systems of fiducials and sensors. The format of this file should conform to the BIDS standard for coordsystem.json files

    * **Sensor positins** (select files)  
    tsv file giving coordinates for labelled sensors

    * **Individual structural image** (select files)  
    Select the subject's structural image.

    * **Mesh resolution** (choose from the menu)  
    Specify the resolution of the cortical mesh

    * **Custom meshes**   
    Provide custom individual meshes as GIfTI files

        * **Custom cortical mesh** (select files)  
        Select the subject's cortical mesh. Leave empty for default

        * **Custom inner skull mesh** (select files)  
        Select the subject's inner skull mesh. Leave empty for default

        * **Custom outer skull mesh** (select files)  
        Select the subject's outer skull mesh. Leave empty for default

        * **Custom scalp mesh** (select files)  
        Select the subject's scalp mesh. Leave empty for default

    * **MEG head model** (choose from the menu)  
    Select the head model type to use for MEG (if present)

    * **Compute Lead Field** (enter text)  
    If value is set to 1 then a lead field will be compted and saved
