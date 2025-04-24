# Kernel from Images  
Generate a kernel matrix from images. In principle, this same function could be used for generating kernels from any image data (e.g. "modulated" grey matter). If there is prior knowledge about some region providing more predictive information (e.g. the hippocampi for AD), then it is possible to weight the generation of the kernel accordingly. The matrix of dot products is saved in a variable ``K``, which can be loaded from the dp_*.mat file. The "kernel trick" can be used to convert these dot products into distance measures for e.g. radial basis function approaches.   

* **Data** (select files)  
Select images to generate dot products from.   

* **Weighting image** (select files)  
The kernel can be generated so that some voxels contribute to the similarity measures more than others.  This is achieved by supplying a weighting image, which each of the component images are multiplied before the dot products are computed. This image needs to have the same dimensions as the component images, but orientation information (encoded by matrices in the headers) is ignored. If left empty, then all voxels are weighted equally.   

* **Dot product Filename** (enter text)  
Enter a filename for results (it will be prefixed by ``dp_`` and saved in the current directory).   
