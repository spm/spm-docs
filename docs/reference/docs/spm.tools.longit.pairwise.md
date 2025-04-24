# Pairwise Longitudinal Registration  
Longitudinal registration of pairs of anatomical MRI scans.  It is based on pairwise inverse-consistent alignment between the first and second scan of each subject, and incorporates an intensity nonuniformity (INU) correction .  Prior to running the registration, the scans should already be in very rough alignment, although because the model incorporates a rigid-body transform, this need not be extremely precise.  Note that there are a bunch of hyper-parameters to be specified.  If you are unsure what values to take, then the defaults should be a reasonable guess of what works.  Note that changes to these hyper-parameters will impact the results obtained.   
.   
The alignment assumes that all scans have similar resolutions and dimensions, and were collected on the same (or very similar) MR scanner using the same pulse sequence.  If these assumption are not correct, then the approach will not work as well.   

* **Time 1 Volumes** (select files)  
Select first time point scans of each subject.   

* **Time 2 Volumes** (select files)  
Select second time point scans of each subject. Note that the order that the first and second time points are specified should be the same.  The algorithm does not incorporate any magical way of figuring out which scans go together.   

* **Time Difference** (enter text)  
Specify the time difference between the scans in years.  This can be a single value (if it is the same for all subjects) or a vector of values (if it differs among subjects).   

* **Noise Estimate** (enter text)  
Specify the standard deviation of the noise in the images.  If a scalar is entered, all images will be assumed to have the same level of noise.  For any non-finite values, the algorithm will try to estimate the noise from fitting a mixture of two Rician distributions to the intensity histogram of each of the images, and assuming that the Rician with the smaller overall intensity models the intensity distribution of air in the background. This works reasonably well for simple MRI scans, but less well for derived images (such as averages) and it fails badly for scans that are skull-stripped.  The assumption used by the registration is that the residuals, after fitting the model, are i.i.d. Gaussian. The assumed standard deviation of the residuals is derived from the estimated Rician distribution of the air.   

* **Warping Regularisation** (enter text)  
Registration involves simultaneously minimising two terms.  One of these is a measure of similarity between the images (mean-squared difference in the current situation), whereas the other is a measure of the roughness of the deformations.  This measure of roughness involves the sum of the following terms:   
    1. Absolute displacements need to be penalised by a tiny amount.  The first element encodes the amount of penalty on these.  Ideally, absolute displacements should not be penalised, but it is often necessary for technical reasons.   
    2. The *membrane energy* of the deformation is penalised (2nd element), usually by a relatively small amount. This penalises the sum of squares of the derivatives of the velocity field (ie the sum of squares of the elements of the Jacobian tensors).   
    3. The *bending energy* is penalised (3rd element). This penalises the sum of squares of the 2nd derivatives of the velocity.   
    4. Linear elasticity regularisation is also included (4th and 5th elements).  The first parameter ($\mu$) is similar to that for *membrane energy*, except it penalises the sum of squares of the Jacobian tensors after they have been made symmetric (by averaging with the transpose).  This term essentially penalises length changes, without penalising rotations.   
    5. The final term also relates to linear elasticity, and is the weight that denotes how much to penalise changes to the divergence of the velocities ($\lambda$).  This divergence is a measure of the rate of volumetric expansion or contraction.   
Note that regularisation is specified based on what is believed to be appropriate for a year of growth.  The specified values are divided by the number of years time difference.   

* **INU Regularisation** (enter text)  
MR images are usually corrupted by a smooth, spatially varying artifact that modulates the intensity of the image (intensity nonuniformity). These artifacts, although not usually a problem for visual inspection, can impede automated processing of the images.   
.   
An important issue relates to the distinction between variations in the difference between the images that arise because of the differential INU artifact due to the physics of MR scanning, and those that arise due to shape differences.  The objective is to model the latter by deformations, while modelling the former with an INU field. We know a priori that intensity variations due to MR physics tend to be spatially smooth. A more accurate estimate of an INU field can be obtained by including prior knowledge about the distribution of the fields likely to be encountered by the correction algorithm. For example, if it is known that there is little or no intensity non-uniformity, then it would be wise to penalise large estimates of the intensity non-uniformity.   
Knowing what works best should be a matter of empirical exploration, as it depends on the scans themselves.  For example, if your data has very little of the artifact, then the INU regularisation should be increased.  This effectively tells the algorithm that there is very little INU in your data, so it does not try to model it.   

* **Save Mid-point average** (choose from the menu)  
Do you want to save the mid-point average template image? This is likely to be useful for groupwise alignment, and is prefixed by ``avg_`` and written out in the same directory of the first time point data.   

* **Save Jacobian Rate** (choose from the menu)  
Do you want to save a map of the differences between the Jacobian determinants, divided by the time interval?  Some consider these useful for morphometrics (although the divergences of the initial velocities may be preferable). The difference between two Jacobian determinants is computed and this is divided by the time interval. One original Jacobian map is for the deformation from the mid point to the first scan, and the other is for the deformation from the mid point to the second scan.  Each of these encodes the relative volume (at each spatial location) between the scan and the mid-point average. Values less than 0 indicate contraction (over time), whereas values greater than zero indicate expansion.  These files are prefixed by ``jd_`` and written out in the same directory of the first time point data.   

* **Save Divergence Rate** (choose from the menu)  
Do you want to save a map of divergence of the velocity field?  This is useful for morphometrics, and may be considered as the rate of volumetric expansion.  Negative values indicate contraction. These files are prefixed by ``dv_`` and written out in the same directory of the first time point data. Note that the divergences written out have been divided by the time interval between scans   

* **Deformation Fields** (choose from the menu)  
Deformation fields can be saved to disk, and used by the Deformations Utility. Deformations are saved as ``y_*.nii`` files, which contain three volumes to encode the x, y and z coordinates.  They are written in the same directory as the corresponding image.   
