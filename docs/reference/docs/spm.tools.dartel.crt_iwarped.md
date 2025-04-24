# Create Inverse Warped  
Create inverse normalised versions of some image(s). The image that is inverse-normalised should be in alignment with the template (generated during the warping procedure). Note that the results have the same dimensions as the "flow fields", but are mapped to the original images via the affine transformations in their headers.  

* **Flow fields** (select files)  
The flow fields store the deformation information. The same fields can be used for both forward or backward deformations (or even, in principle, half way or exaggerated deformations).  

* **Images** (select files)  
Select the image(s) to be inverse normalised.  These should be in alignment with the template image of the warping procedure (Run Dartel).  

* **Time Steps** (choose from the menu)  
The number of time points used for solving the partial differential equations.  Note that Jacobian determinants are not very accurate for very small numbers of time steps (less than about 16).  

* **Interpolation** (choose from the menu)  
The method by which the images are sampled when being written in a different space. (Note that Inf or NaN values are treated as zero, rather than as missing data)  
    Nearest Neighbour:  
      - Fastest, but not normally recommended.  
    Trilinear Interpolation:  
      - OK for PET, realigned fMRI, or segmentations  
    B-spline Interpolation:  
      - Better quality (but slower) interpolation, especially with higher degree splines. Can produce values outside the original range (e.g. small negative values from an originally all positive image).  
