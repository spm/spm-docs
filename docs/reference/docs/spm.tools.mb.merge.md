# Merge tissues  
This option is for merging template tissues together and extracting intensity priors for later use when running **Fit Multi-Brain model**. This is typically used for re-ordering tissue classes or combining multiple classes (e.g. from air, which has a non-Gaussian intensity distribution that is often has multiple "tissues" fitted to it) into one. Generated templates also usually have a large field of view, so it is often desirable to trim them down so the field of view covers a smaller region of anatomy.   

* **MB results file** (select files)  
Specify the results file obtained from running **Fit Multi-Brain model**. This will be named ``mb_fit_*.mat`` and contain a link to where any resulting template may be found.   

* **Indices** (enter text)  
Specify indices. For example, if the original model had K=9 and you wish to combine the final three classes, then enter ``1 2 3 4 5 6 7 7 7``. Note that K refers to the total number of tissue maps -- including the implicit background.   

* **Bounding box** (enter text)  
The bounding box (in voxels) of the merged template volume. Non-finite values indicate to use original template dimensions.   

* **Output name** (enter text)  
Specify a key string for inclusion within all the output file names.   

* **Output directory** (select files)  
All output is written to the specified directory. If this is not specified, the current working directory will used by default.   
