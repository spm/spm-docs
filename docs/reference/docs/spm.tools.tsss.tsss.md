# TSSS denoising  
TSSS clean-up for Neuromag data  

* **File Name** (select files)  
Select the M/EEG mat file.  

* **Do temporal denoising** (choose from the menu)  
Determines whether to use temporal denoising (tSSS)  
if not, just SSS is done.  

* **Realign head location**   
Realign head location to another dataset  

    * **Reference dataset** (select files)  
    Select the M/EEG mat file.  
    Leave empty to realign within dataset  

    * **Reference index** (enter text)  
    Index of the reference sensors within dataset  
    Normally should be 1. Set to 0 for not realigning a composite dataset  

* **Time window** (enter text)  
Time window (in sec) for temporal correlation  
Ignored for epoched data  

* **Correlation limit** (enter text)  
Correlation limit parameter for tSSS  

* **Inner dimension** (enter text)  
Order if the inner SSS basis  

* **Outer dimension** (enter text)  
Order if the outer SSS basis  

* **Condition number threshold** (enter text)  
Threshold on condition number applied for basis regularisation  

* **Output space** (choose from the menu)  
Determines whether the output file is in sensor space  
or has virtual montage trasforming to SSS space.  

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the output dataset. Default prefix is 'tsss_'.  
