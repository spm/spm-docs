# Normalise: Estimate  
Spatial normalisation performed via the segmentation routine.  
.  
The algorithm is essentially the same as that described in the Unified Segmentation paper, except for (i) a slightly different treatment of the mixing proportions, (ii) the use of an improved registration model, (iii) the ability to use multi-spectral data, (iv) an extended set of tissue probability maps, which allows a different treatment of voxels outside the brain.  
.  
If you encounter problems with spatial normalisation, it is advisable to use **Check Reg** to see how well aligned the original data are with the MNI-space templates released with SPM.  If misalignment is greater than about 3 cm and 15 degrees, you could try to manually re-position the images prior to attempting to align them.  This may be done using the Display button.  

* **Data** (create a list of items)  
List of subjects. Images of each subject should be warped differently.  

    * **Subject**   
    Data for this subject. The same parameters are used within subject.  

        * **Image to Align** (select files)  
        The image that the template (atlas) data is warped into alignment with.  
        The result is a set of warps, which can be applied to this image, or any other image that is in register with it.  

* **Estimation Options**   
Various settings for estimating deformations.  

    * **INU regularisation** (choose from the menu)  
    MR images are usually corrupted by a smooth, spatially varying artifact that modulates the intensity of the image (intensity nonuniformity - INU). These artifacts, although not usually a problem for visual inspection, can impede automated processing of the images.  
    .  
    An important issue relates to the distinction between intensity variations that arise because of INU due to the physics of MR scanning, and those that arise due to different tissue properties.  The objective is to model the latter by different tissue classes, while modelling the former with an INU field. We know a priori that intensity variations due to MR physics tend to be spatially smooth, whereas those due to different tissue types tend to contain more high frequency information. A more accurate estimate of an INU field can be obtained by including prior knowledge about the distribution of the fields likely to be encountered by the correction algorithm. For example, if it is known that there is little or no intensity non-uniformity, then it would be wise to penalise large values for the intensity non-uniformity parameters. This regularisation can be placed within a Bayesian context, whereby the penalty incurred is the negative logarithm of a prior probability for any particular pattern of non-uniformity.  
    Knowing what works best should be a matter of empirical exploration.  For example, if your data has very little intensity non-uniformity artifact, then the INU regularisation should be increased.  This effectively tells the algorithm that there is very little INU in your data, so it does not try to model it.  

    * **INU FWHM** (choose from the menu)  
    FWHM of Gaussian smoothness of the intensity nonuniformity (INU). If your intensity non-uniformity is very smooth, then choose a large FWHM. This will prevent the algorithm from trying to model out intensity variation due to different tissue types. The model for intensity non-uniformity is one of i.i.d. Gaussian noise that has been smoothed by some amount, before taking the exponential. Note also that smoother INU fields need fewer parameters to describe them. This means that the algorithm is faster for smoother intensity non-uniformities.  

    * **Tissue probability map** (select files)  
    Select the tissue probability atlas. These should contain probability maps of all the various tissues found in the image data (such that probabilities are greater than or equal to zero, and they sum to one at each voxel. A nonlinear deformation field is estimated that best overlays the atlas on the individual subjects' image.  

    * **Affine Regularisation** (choose from the menu)  
    The procedure is a local optimisation, so it needs reasonable initial starting estimates. Images should be placed in approximate alignment using the Display function of SPM before beginning. A Mutual Information affine registration with the tissue probability maps (D'Agostino et al, 2004) is used to achieve approximate alignment. Note that this step does not include any model for intensity non-uniformity. This means that if the procedure is to be initialised with the affine registration, then the data should not be too corrupted with this artifact.If there is a lot of intensity non-uniformity, then manually position your image in order to achieve closer starting estimates, and turn off the affine registration.  
    .  
    Affine registration into a standard space can be made more robust by regularisation (penalising excessive stretching or shrinking).  The best solutions can be obtained by knowing the approximate amount of stretching that is needed (e.g. ICBM templates are slightly bigger than typical brains, so greater zooms are likely to be needed). For example, if registering to an image in ICBM/MNI space, then choose this option.  If registering to a template that is close in size, then select the appropriate option for this.  

    * **Warping Regularisation** (enter text)  
    The objective function for registering the tissue probability maps to the image to process, involves minimising the sum of two terms. One term gives a function of how probable the data is given the warping parameters. The other is a function of how probable the parameters are, and provides a penalty for unlikely deformations. Smoother deformations are deemed to be more probable. The amount of regularisation determines the tradeoff between the terms. Pick a value around one.  However, if your normalised images appear distorted, then it may be an idea to increase the amount of regularisation (by an order of magnitude). More regularisation gives smoother deformations, where the smoothness measure is determined by the bending energy of the deformations.   

    * **Smoothness** (enter text)  
    For PET or SPECT, set this value to about 5 mm, or more if the images have smoother noise.  For MRI, you can usually use a value of 0 mm.  This is used to derive a fudge factor to account for correlations between neighbouring voxels.  Smoother data have more spatial correlations, rendering the assumptions of the model inaccurate.  

    * **Sampling distance** (enter text)  
    This encodes the approximate distance between sampled points when estimating the model parameters. Smaller values use more of the data, but the procedure is slower and needs more memory. Determining the ``best'' setting involves a compromise between speed and accuracy.  
