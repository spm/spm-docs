# Fit Multi-Brain model  
This is where the model fitting is actually done. The outputs from the model fitting are an initial velocity field and deformation field for each subject. In addition, fitting the model may also generate a representation of a set of average shaped tissue probability maps that can serve as a template for future model fitting. The model  builds on a number of previous works and has the objective of achieving diffeomorphic alignment of a wide variety of medical image modalities into a common anatomical space. This involves the ability to construct a "tissue probability template" from one or more populations of scans through group-wise alignment.   
Diffeomorphic deformations are computed within a geodesic shooting framework, which is optimised with a Gauss-Newton strategy that uses a multi-grid approach to solve the system of linear equations. Variability among image contrasts is modelled using a more sophisticated version of the Gaussian mixture model with bias correction framework originally proposed in the "Unified Segmentation" paper, and which has been extended to account for known variability of the intensity distributions of different tissues. This model has been shown to provide a good model of the intensity distributions of different imaging modalities.  
This work was funded by the EU Human Brain Project's Grant Agreement No 785907 (SGA2).  

* **Template** (choose an option)  
The model can be run using a pre-computed template, or it can implicitly create an average shaped template from the population(s) of scan data. Here, the user chooses whether to create a template or use an existing one. Templates are named ``mu_*.nii``.  

    * **Create template**   
    A tissue probability template will be constructed from all the aligned images. The algorithm alternates between re-computing the template and re-aligning all the images with this template. The user chooses the voxel size of the template and the number of tissue types it encodes.  

        * **Number of classes** (enter text)  
        Specify K, the number of tissue classes encoded by the template. This value is ignored if it is incompatible with the specified data.  

        * **Voxel size** (enter text)  
        Specify the voxel size of the template to be created (mm). The algorithm will automatically attempt to determine suitable settings for its orientation and field of view.  

    * **Existing template** (select files)  
    The model can be fit using a previously computed template, which is not updated. Note that the template contains K-1 volumes within it, and that K should be compatible with various aspects of the data to which the model is fit. The template does not actually encode the tissue probabilities, but rather these probabilities can be generated from the template using a softmax function (${\bf p} = \frac{\exp {\boldsymbol{\mu}}}{1 + \sum_k \exp \mu_k}$).  

* **Affine** (choose from the menu)  
Specify the type of affine transform to use in the model, which may be either none, translations only (T(3)) or rigid body (SE(3)). The fitting begins with affine registration, before continuing by interleaving affine and diffeomorphic registrations over multiple spatial scales.Note that the "Affine" option is likely to throw up lots of warnings about ``QFORM0 representation has been rounded``, which can mostly be ignored.  

* **Shape regularisation** (enter text)  
Specify the regularisation settings for the diffeomorphic registration. These consist of a vector of five values, which penalise different aspects of the warps:  
    1. Absolute displacements need to be penalised by a tiny amount. The first element encodes the amount of penalty on these.  Ideally, absolute displacements should not be penalised, but it is usually  necessary for technical reasons.  
    2. The *membrane energy* of the deformation is penalised, usually by a relatively small amount. This penalises the sum of squares of the derivatives of the velocity field (i.e., the sum of squares of the elements of the Jacobian tensors).  
    3. The *bending energy* is penalised (3rd element). This penalises the sum of squares of the 2nd derivatives of the velocity.  
    4. Linear elasticity regularisation is also included. This parameter ($\mu$) is similar to that for linear elasticity, except it penalises the sum of squares of the Jacobian tensors after they have been made symmetric (by averaging with the transpose). This term essentially penalises length changes, without penalising rotations.  
    5. The final term also relates to linear elasticity, and is the weight that denotes how much to penalise changes to the divergence of the velocities ($\lambda$). This divergence is a measure of the rate of volumetric expansion or contraction.  
  
The default settings work reasonably well for most cases.  

* **Output name** (enter text)  
Specify a key string for inclusion within all the output file names.  

* **Output directory** (select files)  
All output is written to the specified directory. If this is not specified, the current working directory will used by default.  

* **Classes** (create a list of items)  
Images might have been segmented previously into a number of tissue classes. This framework allows such pre-segmented images to be included in the model fitting, in a similar way to the old Dartel toolbox for SPM. The user sets up a series of tissue class types (e.g., grey matter and white matter).  

    * **Class** (select files)  
    For each of the tissue class types, the user should specify the data to be included within the model fitting by selecting the files. It is important that the subject ordering of the files is the same across all classes.  

* **Populations** (create a list of items)  
Multiple populations of subjects may be combined. For example, there may be T1-weighted scans and manually defined labels for one population, whereas another population may have T2-weighted and PD-weighted scans without labels. Yet another population might have CT scans. All subject's data would be subdivided into the same tissue classes, although the intensity distributions of these tissues is likely to differ across populations.  

    * **Pop. of scans**   
    Information about a population of subjects that all have the same set of scans.  

        * **Channels** (create a list of items)  
        Multiple image channels may be specified. For example, two channels may be used to contain the T2-weighted and PD-weighted scans of the subjects.  

            * **Channel**   
            There may be multiple scans of different modalities for each subject. These would be entered into different channels. Note that all scans within a channel should be the same modality.  

                * **Scans** (select files)  
                Select one NIfTI format scan for each subject. Subjects must be in the same order if there are multiple channels. Image dimensions can differ over subjects, but (if there are multiple channels) the scans of each subject must all have the same dimensions and orientations.  

                * **Intensity nonuniformity**   
                Specify the intensity nonuniformity (INU) settings for the current channel, which consist of a regularisation setting and a cutoff.  

                    * **Regularisation** (enter text)  
                    Specify the bending energy penalty on the estimated intensity nonuniformity (INU) fields (bias fields). Larger values give smoother INU fields.  

                    * **Cut off** (choose from the menu)  
                    Specify the cutoff (mm) of the intensity nonuniformity (INU) correction (bias correction). Larger values use fewer parameters to encode the INU field. Note that a global intensity rescaling correction, without INU correction, can also be specified. For quantitative images, it may be better not to use any correction.  

                * **Modality** (choose from the menu)  
                Specify the modality of the scans in this channel. The main reason this is done is so that CT files can have a constant value of 1000 added to them to account for the way Hounsfield units are defined.  

        * **Labels?** (choose an option)  
        Specify whether or not there are any pre-defined label maps for (all) the subjects in the current population.  

            * **Has labels**   
            If subjects have corresponding label maps to guide the segmentation, these need to be specified along with a confusion matrix that relates values in the label maps to which tissue classes they correspond with.  

                * **Label maps** (select files)  
                Label maps are NIfTI images containing integer values, which must have the same dimensions and orientations as the scans of the corresponding subjects. Voxels of each value in the label map may be included in one or more tissue classes. For example, a label map showing the location of brain will include voxels that can be in grey or white matter classes. This information is specified in the confusion matrix.  

        * **Intensity prior**   
        Intensity distributions of each tissue class are modelled by a Gaussian distribution. Prior knowledge about these distributions can make the model fitting more robust.  

            * **Definition** (select files)  
            Knowledge of Gaussian-Wishart priors for the intensity distributions of each cluster can help to inform the segmentation. When available, this information is specified in MATLAB prior*.m files. These files currently need to be hand-crafted. Unless you understand what you are doing, it is advised that you do not specify and intensity prior definition.  

            * **Optimise** (choose from the menu)  
            Specify whether the Gaussian-Wishart priors should be updated at each iteration. Enabling this can slow down convergence if there are small numbers of subjects. If only one subject is to be modelled (using a pre-computed template), then definitely turn off this option. However, if generating a template from a population of scans then this procedure is much more robust when the intensity priors are optimised.  
