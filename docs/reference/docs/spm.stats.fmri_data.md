# fMRI data specification  
Select data and optional explicit mask for a specified fMRI design   

* **Scans** (select files)  
Select the fMRI scans for this session.   
They must all have the same image dimensions, orientation, voxel size etc.   

* **Select SPM.mat** (select files)  
Select the SPM.mat file containing the specified design matrix.   

* **Explicit mask** (select files)  
Specify an image for explicitly masking the analysis.   
A sensible option here is to use a segmention of structural images to specify a within-brain mask. If you select that image as an explicit mask then only those voxels in the brain will be analysed. This both speeds the estimation and restricts SPMs/PPMs to within-brain voxels. Alternatively, if such structural images are unavailable or no masking is required, then leave this field empty.   
