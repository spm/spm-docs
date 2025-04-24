# Slice-to-volume alignment  
Slice-to-volume alignment of fMRI time series.
The rigid-body alignment assumption is a bit limited for fMRI data, which is normally acquired a slice at a time. Motion during the acquisition of a volume is not properly accounted for with volume-based alignment, so this slice-to-volume alignment is an approach to try to deal with this.
Note that the method is a little slower than conventional motion correction, because it uses more of the data in order to estimate rather more parameters.
Also note that certain types of motion can cause the slices to move such that some of the data in the resliced volume is missing. This is why smoothing options are included here.

* **Images** (select files)  
Select fMRI data to correct.

* **Slice Ordering** (choose from the menu)  
Motion parameter estimates are regularised so that slices adjacent in time have to move in a similar way to each other. Slice ordering is therefore used to determine temporal adjacency.

* **Motion sd** (enter text)  
Motion parameters are regularised according to how much motion (mm) might typically be expected from one slice to the next. This is to prevent overfitting because there are many parameters to be estimated.

* **FWHM** (enter text)  
Full width at half maximum (FWHM) of the isotropic Gaussian smoothing kernel in mm. This is used to smooth the motion corrected images.
