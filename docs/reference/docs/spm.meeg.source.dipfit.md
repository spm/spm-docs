# Bayesian Dipole fit  
Run imaging source reconstruction  

* **M/EEG datasets** (select files)  
Select the M/EEG mat files.  

* **Inversion index** (enter text)  
Index of the cell in D.inv where the forward model can be found and the results will be stored.  

* **What conditions to include?** (choose an option)  
What conditions to include?  

    * **All**   
      

    * **Conditions** (create a list of items)  
    Specify the labels of the conditions to be included in the inversion  

        * **Condition label** (enter text)  
          

* **Fit current dipole (in volume conductor) or magnetic dipole (in free space) ** (choose from the menu)  
Fitting field due to an electrical current within the head or a coil (magnetic dipole) outside of the head ?  

* **Time window of interest** (enter text)  
Time window to include in the inversion (ms)  

* **Prior Source locations** (enter text)  
Input source locations as n x 3 matrix  

* **Coords (and outputs) specified in MNI or Native space ?** (choose from the menu)  
Specify coordinate frame of locations/orientations in MNI space or Native MRI space  

* **Prior Source location variance** (enter text)  
Source location variance as n x 3 matrix. So [100 100 100] means approx 10mm uncertainty in any direction  

* **Prior Source moments** (enter text)  
Input source moments as n x 3 matrix. If unsure of orientation leave this as zeros  

* **Prior Source moment variance** (enter text)  
Source moment variance as n x 3 matrix. So [100 100 100] means that expect to have magnitudes of around 10nAm  

* **Estimated amplitude SNR** (enter text)  
Estimate amplitude SNR as a ratio (1 for equal signal and noise amplitude)  

* **Number of fit iterations** (enter text)  
Number of restarts of algorithm to avoid local extrema  

* **Select modalities** (choose from the menu)  
Select modalities for the inversion (only relevant for multimodal datasets).  
