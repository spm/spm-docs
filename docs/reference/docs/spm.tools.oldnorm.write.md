# Old Normalise: Write  
Allows previously estimated warps (stored in ``*_sn.mat`` files) to be applied to series of images.   
All normalised images are written to the same subdirectory as the original images, prefixed with a ``w``.  The details of the transformations are displayed in the results window, and the parameters are saved in the "*_sn.mat" file.   

* **Data** (create a list of items)  
List of subjects. Images of each subject should be warped differently.   

    * **Subject**   
    Data for this subject.  The same parameters are used within subject.   

        * **Parameter File** (select files)  
        Select the ``_sn.mat`` file containing the spatial normalisation parameters for that subject.   

        * **Images to Write** (select files)  
        These are the images for warping according to the estimated parameters. They can be any images that are in register with the "source" image used to generate the parameters.   

* **Writing Options**   
Various options for writing normalised images.   

    * **Preserve** (choose from the menu)  
        1. Preserve Concentrations: Spatially normalised images are not "modulated". The warped images preserve the intensities of the original images.   
        2. Preserve Total: Spatially normalised images are "modulated" in order to preserve the total amount of signal in the images. Areas that are expanded during warping are correspondingly reduced in intensity.   

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

    * **Wrapping** (choose from the menu)  
    These are typically:   
        No wrapping: for PET or images that have already been spatially transformed.    
        Wrap in  Y: for (un-resliced) MRI where phase encoding is in the Y direction (voxel space).   

    * **Filename Prefix** (enter text)  
    Specify the string to be prepended to the filenames of the normalised image file(s). Default prefix is ``w``.   
