# Realign: Reslice  
Reslice a series of registered images such that they match the first image selected voxel-for-voxel.
The resliced images are named the same as the originals, except that they are prefixed by ``r``.

* **Images** (select files)  
Select scans to reslice to match the first.

* **Reslice Options**   
Various reslicing options. If in doubt, simply keep the default values.

    * **Resliced images** (choose from the menu)  
    Specify the images to reslice.
        1. **All Images (1..n)**:  This reslices all the images - including the first image selected - which will remain in its original position.
        2. **Images 2..n**:  Reslices images 2..n only. Useful for if you wish to reslice (for example) a PET image to fit a structural MRI, without creating a second identical MRI volume.
        3. All **Images + Mean Image**:  In addition to reslicing the images, it also creates a mean of the resliced image.
        4. **Mean Image Only**:  Creates the mean resliced image only.

    * **Interpolation** (choose from the menu)  
    The method by which the images are sampled when being written in a different space. Nearest Neighbour is fastest, but not recommended for image realignment. Trilinear Interpolation is probably OK for PET, but not so suitable for fMRI because higher degree interpolation generally gives better results. Although higher degree methods provide better interpolation, but they are slower because they use more neighbouring voxels. Fourier Interpolation is another option, but note that it is only implemented for purely rigid body transformations.  Voxel sizes must all be identical and isotropic.

    * **Wrapping** (choose from the menu)  
    This indicates which directions in the volumes the values should wrap around in. For example, in MRI scans, the images wrap around in the phase encode direction, so (e.g.) the subject's nose may poke into the back of the subject's head. These are typically:
        * **No wrapping** - for PET or images that have already been spatially transformed.
        * **Wrap in  Y**  - for (un-resliced) MRI where phase encoding is in the Y direction (voxel space).

    * **Masking** (choose from the menu)  
    Because of subject motion, different images are likely to have different patterns of zeros from where it was not possible to sample data. With masking enabled, the program searches through the whole time series looking for voxels which need to be sampled from outside the original images. Where this occurs, that voxel is set to zero for the whole set of images (unless the image format can represent NaN, in which case NaNs are used where possible).

    * **Filename Prefix** (enter text)  
    Specify the string to be prepended to the filenames of the resliced image file(s). Default prefix is ``r``.
