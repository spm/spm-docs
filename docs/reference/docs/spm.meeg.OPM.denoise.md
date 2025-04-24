# Synthetic Gradiometery  
Denoise will regress all channels of the selected type(s) from the input dataset. Optionally the derivatives of the selected type(s) can be used as well. This function wil automatically regress on a trial by trial basis or accross the whole sesison based on whether or not the dataset has been epoched.  

* **File Name** (select files)  
Select the M/EEG mat file.  

* **Confounds** (enter text)  
Labels of channel types to use for denoising. Will default to REF to use reference sensors for denoising  

* **Derivatives** (choose from the menu)  
Boolean to specify whether Derivatices should be used for denoising. Default is true  
