# Spatially normalise using Multi-Brain  
Spatially normalise by matching a pre-computed template to each image. The template is not in ICBM space, but a deformation is provided for composing with the inverse of the estimated deformations in order to obtain a suitable spatially normalising deformation. Note that the algorithm may need to download additional data from Figshare:
.
    * https://figshare.com/articles/dataset/Deformation_field_mapping_from_ICMB_to_mu_X_nii/17143796/1
    * https://figshare.com/articles/dataset/Head_Tissue_Template/17143289
.
This approach is described in:
.
    * Brudfors, M., Balbastre, Y., Flandin, G., Nachev, P. and Ashburner, J., 2020. Flexible Bayesian modelling for nonlinear image registration. In Medical Image Computing and Computer Assisted Intervention–MICCAI 2020: 23rd International Conference, Lima, Peru, October 4–8, 2020, Proceedings, Part III 23 (pp. 253-263). Springer International Publishing.
    * Blaiotta, C., Freund, P., Cardoso, M.J. and Ashburner, J., 2018. Generative diffeomorphic modelling of large MRI data sets for probabilistic template construction. NeuroImage, 166, pp.117-134.

* **Scans** (select files)  
Select one NIfTI format scan for each subject.
