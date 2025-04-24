# SCOPE  
Utility to correct susceptibility distortions in EPI images using a re-implementation of FSL's topup (J.L.R. Andersson, S. Skare, J. Ashburner, 2003. How to correct susceptibility distortions in spin-echo echo-planar images: application to diffusion tensor imaging). This implementation requires two EPI images acquired with opposite traversals along the phase-encode direction, which is assumed to be in the y-direction (in voxel-space).  

* **Same PE direction image(s)** (select files)  
Select an image, or set of images, where the acquisition traversed the phase-encode direction of K-space in the *same* way as the fMRI data to be corrected (and could even be the same data as the fMRI to be corrected). Note that if multiple images are specified, then they will be motion corrected and their average used for registering the phase-encoding direction reversed images. If these images were acquired before the oposite PE images, then it is advised to select the final volume before the others, otherwise simply select them in the usual chronological order.  

* **Opposite PE direction image(s)** (select files)  
Select an image, or set of images, where the acquisition traversed the phase-encode direction of K-space in the *opposite* way to the fMRI data to be corrected. Note that if multiple images are specified, then they will be motion corrected and their average used for registering the phase-encoding direction reversed images .If these images were acquired before the same PE images, then it is advised to select the final volume before the others, otherwise simply select them in the usual chronological order.  

* **FWHM** (enter text)  
Spatial scales (expressed as FWHM of Gaussian kernels used to smooth input images during SCOPE estimation.  

* **Regularisation** (enter text)  
Regularisation settings (see spm_field).  
The three expected values are:  
    1. Penalty on absolute values.  
    2. Penalty on the *membrane energy*.  
    3. Penalty on the *bending energy*.  

* **Interpolation** (choose from the menu)  
Degree of B-spline (from 0 to 7) along different dimensions (see ``spm_diffeo``).  

* **Jacobian scaling** (choose from the menu)  
Option to include Jacobian scaling in the registration model.  
It includes in the process the changes of intensities due to stretching and compression.  

* **vdm Filename Prefix** (enter text)  
Specify the string to be prepended to the voxel displacement map (vdm) file. Default prefix is ``vdm5_``.  

* **Output directory** (select files)  
All created files (voxel displacement map and unwarped images) are written in the specified directory. The voxel displacement map is saved to disk as a vdm file (``vdm5_*.nii``)  
