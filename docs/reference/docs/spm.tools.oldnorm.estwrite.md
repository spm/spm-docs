# Old Normalise: Estimate & Write  
Computes the warp that best registers a source image (or series of source images) to match a template, saving it to the file ``*_sn.mat``. This option also allows the contents of the ``*_sn.mat`` files to be applied to a series of images.
All normalised images are written to the same subdirectory as the original images, prefixed with a ``w``.  The details of the transformations are displayed in the results window, and the parameters are saved in the "*_sn.mat" file.

* **Data** (create a list of items)  
List of subjects. Images of each subject should be warped differently.

    * **Subject**   
    Data for this subject.  The same parameters are used within subject.

        * **Source Image** (select files)  
        The image that is warped to match the template(s).  The result is a set of warps, which can be applied to this image, or any other image that is in register with it.

        * **Source Weighting Image** (select files)  
        Optional weighting images (consisting of pixel values between the range of zero to one) to be used for registering abnormal or lesioned brains.  These images should match the dimensions of the image from which the parameters are estimated, and should contain zeros corresponding to regions of abnormal tissue.

        * **Images to Write** (select files)  
        These are the images for warping according to the estimated parameters. They can be any images that are in register with the "source" image used to generate the parameters.

* **Estimation Options**   
Various settings for estimating warps.

    * **Template Image** (select files)  
    Specify a template image to match the source image with. The contrast in the template must be similar to that of the source image in order to achieve a good registration.  It is also possible to select more than one template, in which case the registration algorithm will try to find the best linear combination of these images in order to best model the intensities in the source image. The template images supplied with SPM conform to the space defined by the ICBM (NIH P-20 project).

    * **Template weighting image** (select files)  
    Applies a weighting mask to the template(s) during the parameter estimation.  With the default brain mask, weights in and around the brain have values of one whereas those clearly outside the brain are zero.  This is an attempt to base the normalisation purely upon the shape of the brain, rather than the shape of the head (since low frequency basis functions can not really cope with variations in skull thickness).
    An option is available for a user specified weighting image. This should have the same dimensions and mat file as the template images, with values in the range of zero to one.

    * **Source Image Smoothing** (enter text)  
    Smoothing to apply to a copy of the source image. The template and source images should have approximately the same smoothness. Remember that the templates supplied with SPM have been smoothed by 8mm, and that smoothnesses combine by Pythagoras' rule.

    * **Template Image Smoothing** (enter text)  
    Smoothing to apply to a copy of the template image. The template and source images should have approximately the same smoothness. Remember that the templates supplied with SPM have been smoothed by 8mm, and that smoothnesses combine by Pythagoras' rule.

    * **Affine Regularisation** (choose from the menu)  
    Affine registration into a standard space can be made more robust by regularisation (penalising excessive stretching or shrinking).  The best solutions can be obtained by knowing the approximate amount of stretching that is needed (e.g. ICBM templates are slightly bigger than typical brains, so greater zooms are likely to be needed). If registering to an image in ICBM/MNI space, then choose the first option.  If registering to a template that is close in size, then select the second option.  If you do not want to regularise, then choose the third.

    * **Nonlinear Frequency Cutoff** (enter text)  
    Cutoff of DCT bases.  Only DCT bases of periods longer than the cutoff are used to describe the warps. The number used will depend on the cutoff and the field of view of the template image(s).

    * **Nonlinear Iterations** (enter text)  
    Number of iterations of nonlinear warping performed.

    * **Nonlinear Regularisation** (enter text)  
    The amount of regularisation for the nonlinear part of the spatial normalisation. Pick a value around one.  However, if your normalised images appear distorted, then it may be an idea to increase the amount of regularisation (by an order of magnitude) - or even just use an affine normalisation. The regularisation influences the smoothness of the deformation fields.

* **Writing Options**   
Various options for writing normalised images.

    * **Preserve** (choose from the menu)  
        1. Preserve Concentrations: Spatially normalised images are not "modulated". The warped images preserve the intensities of the original images.
        2. Preserve Total: Spatially normalised images are "modulated" in order to preserve the total amount of signal in the images. Areas that are expanded during warping are correspondingly reduced in intensity.

    * **Bounding box** (enter text)  
    The bounding box (in mm) of the volume which is to be written (relative to the anterior commissure).

    * **Voxel sizes** (enter text)  
    The voxel sizes (x, y & z, in mm) of the written normalised images.

    * **Interpolation** (choose from the menu)  
    The method by which the images are sampled when being written in a different space. (Note that Inf or NaN values are treated as zero, rather than as missing data)
        Nearest Neighbour:
          - Fastest, but not normally recommended.
        Trilinear Interpolation:
          - OK for PET, realigned fMRI, or segmentations
        B-spline Interpolation:
          - Better quality (but slower) interpolation, especially with higher degree splines. Can produce values outside the original range (e.g. small negative values from an originally all positive image).

    * **Wrapping** (choose from the menu)  
    These are typically:
        No wrapping: for PET or images that have already been spatially transformed. 
        Wrap in  Y: for (un-resliced) MRI where phase encoding is in the Y direction (voxel space).

    * **Filename Prefix** (enter text)  
    Specify the string to be prepended to the filenames of the normalised image file(s). Default prefix is ``w``.
