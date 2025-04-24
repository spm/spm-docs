# Generate Scalar Momenta  
Generate spatially smoothed *scalar momenta*  cite{singh2010multivariate,singh2012genetic}  in a form suitable for using with pattern recognition. In principle, a Gaussian Process model can be used to determine the optimal (positive) linear combination of kernel matrices.  The idea would be to combine a kernel matrix derived from these, with a kernel derived from the velocity fields. Such a combined kernel should then encode more relevant information than the individual kernels alone.  The scalar momentum fields that are generated contain a number of volumes equal to the number of sets of ``rc*`` images used (equal to the number of volumes in the template - 1).   See Figures 10 and 11 of for examples of scalar momenta (Jacobian scaled residuals) for simulated data.   

* **Template** (select files)  
Residual differences are computed between the warped images and template. These are then scaled by the Jacobian determinants at each point, and spatially smoothed.  

* **Images** (create a list of items)  
Multiple sets of images are used here. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.  The number of sets of images must be the same as was used to generate the template.  

    * **Images** (select files)  
    Select tissue class images (one per subject).  

* **Deformation fields** (select files)  
Select the deformation fields for each subject.  

* **Jacobian determinant fields** (select files)  
Select the Jacobian determinant fields for each subject.  Residual differences are computed between the warped images and template. These are then scaled by the Jacobian determinants at each point, and spatially smoothed.  

* **Smoothing** (choose from the menu)  
The scalar momenta can be smoothed with a Gaussian to reduce dimensionality. More smoothing is recommended if there are fewer training images or if more channels of data were used for driving the registration. From preliminary experiments, a value of about 10mm seems to work reasonably well.  
