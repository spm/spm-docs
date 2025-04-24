# Conversion  
Convert EEG/MEG data.  

* **File Name** (select files)  
Select data set file.  

* **Reading mode** (choose an option)  
Select whether you want to convert to continuous or epoched data.  

    * **Continuous** (choose an option)  
      

        * **Time window** (enter text)  
        start and end of epoch  

        * **Read all**   
          

    * **Epoched** (choose an option)  
      

        * **Trials defined in data**   
          

        * **Trial File** (select files)  
          

        * **Define trial**   
          

            * **Time window** (enter text)  
            start and end of epoch  

            * **Trial definitions** (create a list of items)  
              

                * **Trial**   
                  

                    * **Condition label** (enter text)  
                      

                    * **Event type** (enter text)  
                      

                    * **Event value** (enter text)  
                      

    * **Header only**   
      

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
      

* **Output filename** (enter text)  
Choose filename. Leave empty to add 'spmeeg_' to the input file name.  

* **Event padding** (enter text)  
in sec - the additional time period around each trial  
for which the events are saved with the trial (to let the  
user keep and use for analysis events which are outside  
trial borders). Default - 0  

* **Block size** (enter text)  
size of blocks used internally to split large files default ~100Mb.  

* **Check trial boundaries** (choose from the menu)  
Check if there are breaks in the file and do not read  
across those breaks (recommended) or ignore them.  

* **Save original header** (choose from the menu)  
Keep the original data header which might be useful for some later analyses.  

* **Input data format** (enter text)  
Force the reader to assume a particular data format (usually not necessary).  
