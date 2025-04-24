# Montage  
Apply a montage (linear transformation) to EEG/MEG data.  

* **File Name** (select files)  
Select the EEG mat file.  

* **Mode** (choose an option)  
Choose between writing a new dataset or online montage operation  

    * **Write**   
    Apply montage and write out to a new dataset.  

        * **Montage specification** (choose an option)  
        Choose how to specify the montage  

            * **Load montage from file**   
              

                * **Montage file name** (select files)  
                Select a montage file.  

                * **Keep other channels** (choose from the menu)  
                Specify whether you want to keep channels that are not contributing to the new channels  

            * **Montage name** (enter text)  
            Specify the name of existing montage  

            * **Montage index** (enter text)  
            Specify the index of existing montage  

        * **Block size** (enter text)  
        size of blocks used internally to split large files default ~20Mb  

        * **Filename Prefix** (enter text)  
        Specify the string to be prepended to the filenames of the montaged   
        dataset (if generated). Default prefix is 'M'.  

    * **Switch**   
    Switch to new online montage without writing out a new file  

        * **Montage specification** (choose an option)  
        Choose how to specify the montage  

            * **Load montage from file**   
              

                * **Montage file name** (select files)  
                Select a montage file.  

                * **Keep other channels** (choose from the menu)  
                Specify whether you want to keep channels that are not contributing to the new channels  

            * **Montage name** (enter text)  
            Specify the name of existing montage  

            * **Montage index** (enter text)  
            Specify the index of existing montage  

    * **Add**   
    Add montage to the set of online montages without applying  

        * **Load montage from file**   
          

            * **Montage file name** (select files)  
            Select a montage file.  

            * **Keep other channels** (choose from the menu)  
            Specify whether you want to keep channels that are not contributing to the new channels  

    * **Clear**   
    Clear all online montages and revert to the original state.  
