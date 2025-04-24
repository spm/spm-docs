# Image Calculator  
The image calculator is for performing user-specified algebraic manipulations on a set of images.
The result is being written out as an image. The user is prompted to supply images to work on, a filename for the output image, and the expression to evaluate. The expression should be a standard MATLAB expression, within which the images should be referred to as ``i1``, ``i2``, ``i3``,... etc.

* **Input Images** (select files)  
These are the images that are used by the calculator.  They are referred to as ``i1``, ``i2``, ``i3``, etc in the order that they are specified.

* **Output Filename** (enter text)  
The output image is written to current working directory unless a valid full pathname is given. If a path name is given here, the output directory setting will be ignored.
If the field is left empty then the name of the 1st input image, preprended with 'i', is used (change this letter in the spm_defaults if necessary).

* **Output Directory** (select files)  
Files produced by this function will be written into this output directory. If no directory is given, images will be written to current working directory. If both output filename and output directory contain a directory, then output filename takes precedence.

* **Expression** (enter text)  
Example expressions:
    * Mean of six images (select six images)
          ``(i1+i2+i3+i4+i5+i6)/6``
    * Make a binary mask image at threshold of 100
          ``i1>100``
    * Make a mask from one image and apply to another
          ``i2.*(i1>100)``
      [here the first image is used to make the mask, which is applied to the second image]
    * Sum of n images
          ``i1 + i2 + i3 + i4 + i5 + ...``
    * Sum of n images (when reading data into a data-matrix - use the Data Matrix option)
          ``sum(X)``

* **Additional Variables** (create a list of items)  
Additional variables which can be used in expression.

    * **Variable**   
    Additional variable which can be used in expression.

        * **Name** (enter text)  
        Variable name used in expression.

        * **Value** (enter text)  
        Value of the variable.

* **Options**   
Options for image calculator

    * **Data matrix** (choose from the menu)  
    If this flag is set, then images are read into a data matrix ``X`` (rather than into separate variables ``i1``, ``i2``, ``i3``,...). The data matrix  should be referred to as ``X``, and contains images in rows. Computation is plane by plane, so in data-matrix mode, ``X`` is an NxK matrix, where N is the number of input images, and K is the number of voxels per plane.

    * **Masking** (choose from the menu)  
    For data types without a representation of ``NaN``, implicit zero masking assumes that all zero voxels are to be treated as missing, and treats them as ``NaN``. ``NaN``s are written as zero (by spm_write_plane), for data types without a representation of ``NaN``.

    * **Interpolation** (choose from the menu)  
    With images of different sizes and orientations, the size and orientation of the first is used for the output image. A warning is given in this situation. Images are sampled into this orientation using the interpolation specified by the hold parameter.
    The method by which the images are sampled when being written in a different space.
        0. **Nearest Neighbour**
           Fastest, but not normally recommended.
        1. **Trilinear Interpolation**
           OK for PET, or realigned fMRI.
        2. **Sinc Interpolation**
           Better quality (but slower) interpolation, especially with higher degrees.

    * **Data Type** (choose from the menu)  
    Data-type of output image
