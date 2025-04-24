# Tissue Volumes  
Compute total tissue volumes (in litres) from segmentation results.  
.  
Only the seg8.mat files are required, but if modulated warped segmentations (mwc*) are found they will be reused, saving time (and allowing you to use non-default values for MRF and/or clean-up options if you wish).  

* **Segmentation mat-files** (select files)  
Select the 'seg8.mat' files containing the segmentation parameters.  

* **Maximum tissue class index** (enter text)  
Specify the maximum tissue class index, T, where tissues [1:T] will be measured.  
The default of 3 corresponds to GM, WM and CSF for the default tissue prior probability maps 'TPM.nii,1' to 'TPM.nii,3'  
.  
The sum of these tissues will also be computed, which by default is the total intracranial volume (known as TIV or ICV). If T=2, the sum will by default be the total parenchymal brain volume (known as TBV or PBV), which is also often of interest.  

* **Mask image** (select files)  
Optional binary mask image in same space as TPMs (e.g. MNI).  
Only voxels inside this mask will count for the total volume.  
.  
The default mask excludes the eyes, which might otherwise be counted in the fluid tissue class (that includes cerebrospinal, aqueous, and vitreous fluids/humours).  

* **Output file** (enter text)  
Filename for saving results.  
Segmentation filenames and volumes will be stored in CSV format (comma-separated variables).  
.  
This can be empty; results will appear in the MATLAB command window.  
