# Smooth  
Smooth (ie convolve) image volumes with a Gaussian kernel of a specified width.   
It is used as a preprocessing step to suppress noise and effects due to residual differences in functional and gyral anatomy during inter-subject averaging.   

* **Images to smooth** (select files)  
Specify the images to smooth.   
The smoothed images are written to the same subdirectories as the original images with a configurable prefix.   

* **FWHM** (enter text)  
Full width at half maximum (FWHM) of the Gaussian smoothing kernel in mm.   
Three values should be entered, denoting the FWHM in the x, y and z directions.   

* **Data Type** (choose from the menu)  
Data type of the output images.   
'SAME' indicates the same data type as the original images.   

* **Implicit masking** (choose from the menu)  
An "implicit mask" is a mask implied by a particular voxel value (0 for images with integer type, NaN for float images).   
If set to 'Yes', the implicit masking of the input image is preserved in the smoothed image.   

* **Filename prefix** (enter text)  
String to be prepended to the filenames of the smoothed image file(s). Default prefix is 's'.   
