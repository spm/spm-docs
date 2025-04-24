# Coregister: Estimate  
Within-subject registration using a rigid-body model.
.
Because the registration algorithm uses objective functions derived from joint intensity histograms, it often works much better using scans that have been intensity non-uniformity (INU) corrected. Skull-stripping can also help.  INU corrected scans can be obtained as a by-product of SPM's segmentation. These can be easily skull-stripped by masking them with the grey matter grey matter, white matter and CSF maps that the segmentation generates (e.g., using SPM's ImCalc, and evaluating an expression like ``i1.*((i2+i3+i4)>0.5)``, where ``i1`` corresponds to the INU corrected scan, and ``i2``, ``i3`` and ``i4`` correspond to the tissue maps).
.
The registration method used here is based on work by Collignon et al. The original interpolation method described in this paper has been changed in order to give a smoother cost function.  The images are also smoothed slightly, as is the histogram.  This is all in order to make the cost function as smooth as possible, to give faster convergence and less chance of local minima.
.
At the end of coregistration, the voxel-to-voxel affine transformation matrix is displayed, along with the histograms for the images in the original orientations, and the final orientations.  The registered images are displayed at the bottom.
.
Registration parameters are stored in the headers of the "moved" and the "other" images.

* **Fixed Image** (select files)  
This is the image that is assumed to remain stationary (previously known as the reference image), while the moved (previously known as the source) image is moved to match it. Although not ideal, note that slice-to-volume registration works better if the single slice is the fixed image.

* **Moved Image** (select files)  
This is the image that is jiggled about to best match the fixed (reference) image.

* **Other Images** (select files)  
These are any images that need to remain in alignment with the moved image.

* **Estimation Options**   
Various registration options, which are passed to the Powell optimisation algorithm.

    * **Objective Function** (choose from the menu)  
    Registration involves finding parameters that either maximise or minimise some objective function. For inter-modal registration, use Mutual Information, Normalised Mutual Information, or Entropy Correlation Coefficient. For within modality, you could also use Normalised Cross Correlation.

    * **Separation** (enter text)  
    The average distance between sampled points (in mm). Can be a vector to allow a coarse registration followed by increasingly fine ones.

    * **Tolerances** (enter text)  
    The accuracy for each parameter.  Iterations stop when differences between successive estimates are less than the required tolerance.

    * **Histogram Smoothing** (enter text)  
    Gaussian smoothing to apply to the 256x256 joint histogram. Other information theoretic coregistration methods use fewer bins, but Gaussian smoothing seems to be more elegant.
