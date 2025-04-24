# Output  
When **Fit Multi-Brain model** is run, the resulting model fit contains information that allows a lot of other derived images to be generated. For example, the results file encodes the INU fields, which allows images to be INU corrected. It contains information about intensity distributions, which (when combined with other extracted information) enables tissue segmentation to be achieved. And of course, the estimated deformations allow spatially normalised versions of these results to be generated.
The **Output** functionality allows a range of derived data to be generated from these parameter estimates. When it is executed, it generates the derived data from the images originally entered into **Fit Multi-Brain model**. In order to do this, it needs to assume that the images, as well as the results files, have not been moved from their original locations. If they can not be fund, then **Output** will crash out in a not very elegant way.

* **MB results file** (select files)  
Specify the results file obtained from previously running **Fit Multi-Brain model**.

* **Images** (choose from the menu)  
Specify whether versions of the original images, but with missing values filled in, should be written out.

* **INU corrected** (choose from the menu)  
Specify whether INU corrected versions of the original images (with missing values filled in) should be written out.

* **Warped images** (choose from the menu)  
Specify whether spatially normalised versions of the INU corrected images (missing values filled in) should be written out.

* **Warped INU corrected** (choose from the menu)  
.

* **INU** (choose from the menu)  
Specify whether the estimated INU fields should be written out.

* **Tissues** (enter text)  
Specify the indices of any native-space tissue class images to be written.

* **Warped tissues** (enter text)  
Specify the indices of any spatially normalised tissue class images to be written.

* **Warped mod. tissues** (enter text)  
Specify the indices of any spatially normalised and Jacobian-scaled ("modulated") tissue class images to be written.

* **Scalar momentum** (enter text)  
Specify the indices of any scalar momentums to be written.

* **MRF Parameter** (enter text)  
When tissue class images are written out, a few iterations of a simple Markov random field (MRF) cleanup procedure are run.  This parameter controls the strength of the MRF. Setting the value to zero will disable the cleanup.

* **Output smoothing** (enter text)  
Full width at half maximum (FWHM) of Gaussian smoothing kernel for smoothing of template space data: warped tissues, modulated warped tissues and scalar momentums.

* **Bounding box** (enter text)  
The bounding box of the template space output, as a 2x3 array. The specified values should be the minimum and maximum x coordinates, the minimum and maximum y coordinates, and the minimum and maximum z coordinates.

* **Voxel sizes** (enter text)  
The (isotropic) voxel size of the template space output, in mm.

* **Process responsibilities** (enter text)  
Function for processing native space responsibilities, given as a function handle ``@(x) foo(x)``. The argument (``x``) is of ``size(x) = [1, 4]``, where the first three dimensions are the size of the image and the last dimension is the number of segmentation classes (K + 1).

* **Output directory** (select files)  
All output is written to the specified directory. If this is not specified, the output directory of the run module will be used by default.
