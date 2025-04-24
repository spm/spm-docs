# Bayesian moment fit  
Run imaging source reconstruction  

* **M/EEG datasets** (select files)  
Select the M/EEG mat files.  

* **Inversion index** (enter text)  
Index of the cell in D.inv where the forward model can be found and the results will be stored.  

* **Fit current dipole (in volume conductor) or magnetic dipole (in free space) ** (choose from the menu)  
Fitting field due to an electrical current within the head or a coil (magnetic dipole) outside of the head ?  

* **What conditions to include?** (choose an option)  
What conditions to include?  

    * **All**   
    .  

    * **Conditions** (create a list of items)  
    Specify the labels of the conditions to be included in the inversion  

        * **Condition label** (enter text)  
        .  

* **Time window of interest** (enter text)  
Time window to include in the inversion (ms)  

* **Source locations** (enter text)  
Source locations as n x 3 matrix  

* **Source orientations as n x 3 matrix** (enter text)  
Source orientation as n x 3 matrix of unit vectors  

* **Prior mean of source moments** (enter text)  
Input source moments as n x 1 matrix (in nAm)  

* **Prior variance of source moments** (enter text)  
Prior source moment variance as n x 1 matrix (in (nAm) \^2)  

* **Estimated amplitude SNR** (enter text)  
Estimate amplitude SNR as a ratio (1 for equal signal and noise amplitude)  

* **Number of fit iterations** (enter text)  
Number of restarts of algorithm to avoid local extrema  

* **Select modalities** (choose from the menu)  
Select modalities for the inversion (only relevant for multimodal datasets).  
