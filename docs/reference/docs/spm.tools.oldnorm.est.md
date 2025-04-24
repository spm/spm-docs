# Old Normalise: Estimate  
Computes the warp that best registers a source image (or series of source images) to match a template, saving it to a file ``*_sn.mat``.  
Generally, the algorithms work by minimising the sum of squares difference between the image which is to be normalised, and a linear combination of one or more template images.  For the least squares registration to produce an unbiased estimate of the spatial transformation, the image contrast in the templates (or linear combination of templates) should be similar to that of the image from which the spatial normalisation is derived.  The registration simply searches for an optimum solution.  If the starting estimates are not good, then the optimum it finds may not find the global optimum.  
The first step of the normalisation is to determine the optimum 12-parameter affine transformation.  Initially, the registration is performed by matching the whole of the head (including the scalp) to the template.  Following this, the registration proceeded by only matching the brains together, by appropriate weighting of the template voxels.  This is a completely automated procedure (that does not require "scalp editing") that discounts the confounding effects of skull and scalp differences.   A Bayesian framework is used, such that the registration searches for the solution that maximises the a posteriori probability of it being correct .  i.e., it maximises the product of the likelihood function (derived from the residual squared difference) and the prior function (which is based on the probability of obtaining a particular set of zooms and shears).  
The affine registration is followed by estimating nonlinear deformations, whereby the deformations are defined by a linear combination of three dimensional discrete cosine transform (DCT) basis functions .  The default options result in each of the deformation fields being described by 1176parameters, where these represent the coefficients of the deformations in three orthogonal directions.  The matching involved simultaneously minimising the membrane energies of the deformation fields and the residual squared difference between the images and template(s).  

* **Data** (create a list of items)  
List of subjects. Images of each subject should be warped differently.  

    * **Subject**   
    Data for this subject.  The same parameters are used within subject.  

        * **Source Image** (select files)  
        The image that is warped to match the template(s).  The result is a set of warps, which can be applied to this image, or any other image that is in register with it.  

        * **Source Weighting Image** (select files)  
        Optional weighting images (consisting of pixel values between the range of zero to one) to be used for registering abnormal or lesioned brains.  These images should match the dimensions of the image from which the parameters are estimated, and should contain zeros corresponding to regions of abnormal tissue.  

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
