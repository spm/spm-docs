# Normalise: Write  
Apply previously estimated warps (stored in ``y_''imagename``_sn.mat'' files) to series of images.  

* **Data** (create a list of items)  
List of subjects. Images of each subject should be warped differently.  

    * **Subject**   
    Data for this subject. The same parameters are used within subject.  

        * **Deformation Field** (select files)  
        Deformations can be thought of as vector fields, and represented by three-volume images.  In SPM, deformation fields are saved in NIfTI format, with dimensions xdim x ydim x zdim x 1 x 3. Each voxel contains the x, y and z mm coordinates of where the deformation points.  

        * **Images to Write** (select files)  
        These are the images for warping according to the estimated parameters.  
        They can be any images that are in register with the image used to generate the deformation.  

* **Writing Options**   
Various options for writing normalised images.  

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

    * **Filename Prefix** (enter text)  
    Specify the string to be prepended to the filenames of the normalised image file(s). Default prefix is 'w'.  
