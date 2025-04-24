# Coregister: Estimate & Reslice  
Within-subject registration using a rigid-body model and image reslicing.  
  
Because the registration algorithm uses objective functions derived from joint intensity histograms, often works much better using scans that have been intensity non-uniformity (INU) corrected. Skull-stripping can also help.  INU corrected scans can be obtained as a by-product of SPM's segmentation. These can be easily skull-stripped by masking them with the grey matter grey matter, white matter and CSF maps that the segmentation generates (e.g., using SPM's ImCalc, and evaluating an expression like ``i1.*((i2+i3+i4)>0.5)``, where ``i1`` corresponds to the INU corrected scan, and ``i2``, ``i3`` and ``i4`` correspond to the tissue maps).  
  
The registration method used here is based on work by Collignon et al. The original interpolation method described in this paper has been changed in order to give a smoother cost function.  The images are also smoothed slightly, as is the histogram.  This is all in order to make the cost function as smooth as possible, to give faster convergence and less chance of local minima.  
  
At the end of coregistration, the voxel-to-voxel affine transformation matrix is displayed, along with the histograms for the images in the original orientations, and the final orientations.  The registered images are displayed at the bottom.  
  
Please note that Coreg only attempts rigid alignment between the images. fMRI tend to have large distortions, which are not corrected by rigid-alignment alone. There is not yet any functionality in the SPM software that is intended to correct this type of distortion when aligning distorted fMRI with relatively undistorted anatomical scans (e.g. MPRAGE).  
  
Registration parameters are stored in the headers of the "moved" and the "other" images. These images are also resliced to match the fixed image voxel-for-voxel. The resliced images are named the same as the originals except that they are prefixed by ``r``.  

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

* **Reslice Options**   
Various reslicing options.  

    * **Interpolation** (choose from the menu)  
    The method by which the images are sampled when being written in a different space.  Nearest Neighbour is fastest, but not normally recommended. It can be useful for re-orienting images while preserving the original intensities (e.g. an image consisting of labels). Trilinear Interpolation is OK for PET, or realigned and re-sliced fMRI. If subject movement (from an fMRI time series) is included in the transformations then it may be better to use a higher degree approach. Note that higher degree B-spline interpolation is slower because it uses more neighbours.  

    * **Wrapping** (choose from the menu)  
    This indicates which directions in the volumes the values should wrap around in. These are typically:  
        * **No wrapping** - for PET or images that have already been spatially transformed.  
        * **Wrap Y**  - for (un-resliced) MRI where phase encoding is in the Y direction (voxel space).  

    * **Masking** (choose from the menu)  
    Because of subject motion, different images are likely to have different patterns of zeros from where it was not possible to sample data. With masking enabled, the program searches through the whole time series looking for voxels which need to be sampled from outside the original images. Where this occurs, that voxel is set to zero for the whole set of images (unless the image format can represent NaN, in which case NaNs are used where possible).  

    * **Filename Prefix** (enter text)  
    String to be prepended to the filenames of the resliced image file(s). Default prefix is ``r``.  
