# Head model specification  
Specify M/EEG head model for forward computation   

* **M/EEG datasets** (select files)  
Select the M/EEG mat files.   

* **Inversion index** (enter text)  
Index of the cell in D.inv where the results will be stored.   

* **Comment** (enter text)  
User-specified information about this inversion   

* **Meshes**   
Create head meshes for building the head model   

    * **Mesh source** (choose an option)  

        * **Template**   

        * **Individual structural image** (select files)  
        Select the subject's structural image   

        * **Custom meshes**   
        Provide custom individual meshes as GIfTI files   

            * **Individual structural image** (select files)  
            Select the subject's structural image   

            * **Custom cortical mesh** (select files)  
            Select the subject's cortical mesh. Leave empty for default   

            * **Custom inner skull mesh** (select files)  
            Select the subject's inner skull mesh. Leave empty for default   

            * **Custom outer skull mesh** (select files)  
            Select the subject's outer skull mesh. Leave empty for default   

            * **Custom scalp mesh** (select files)  
            Select the subject's scalp mesh. Leave empty for default   

    * **Mesh resolution** (choose from the menu)  
    Specify the resolution of the cortical mesh   

* **Coregistration** (choose an option)  
Coregistration   

    * **Specify coregistration parameters**   

        * **Fiducials** (create a list of items)  
        Specify fiducials for coregistration (at least 3 fiducials need to be specified)   

            * **Fiducial**   
            Specify fiducial for coregistration   

                * **M/EEG fiducial label** (enter text)  
                Label of a fiducial point (as specified in the M/EEG dataset)   

                * **How to specify?** (choose an option)  

                    * **Select from a list** (choose from the menu)  
                    Select the corresponding fiducial point from a pre-specified list.   

                    * **Type MRI coordinates** (enter text)  
                    Type the coordinates (in MNI or native space depending on the MRI supplied) corresponding to the fiducial in the structural image.   

        * **Use headshape points?** (choose from the menu)  
        Use headshape points (if available)   

    * **Sensor locations are in MNI space already**   
    No coregistration is necessary because default EEG sensor locations were used   

    * **Coregistration based on BIDS json file**   
    Coregistration based on BIDS json file   

        * **MRI BIDS json file** (select files)  
        Select MRI BIDS json file with fiducials.   

        * **Use headshape points?** (choose from the menu)  
        Use headshape points (if available)   

* **Forward model**   
Forward model   

    * **EEG head model** (choose from the menu)  
    Select the head model type to use for EEG (if present)   

    * **MEG head model** (choose from the menu)  
    Select the head model type to use for MEG (if present)   
