# Coregister: Reslice  
Reslice images to match voxel-for-voxel with an image defining some space. The resliced images are named the same as the originals except that they are prefixed by ``r``.

* **Image Defining Space** (select files)  
This is analogous to the fixed (reference) image. Images are resliced to match this image (providing they have been coregistered first).

* **Images to Reslice** (select files)  
These images are resliced to the same dimensions, voxel sizes, orientation etc as the space defining image.

* **Reslice Options**   
Various reslicing options.

    * **Interpolation** (choose from the menu)  
    The method by which the images are sampled when being written in a different space.  Nearest Neighbour is fastest, but not normally recommended. It can be useful for re-orienting images while preserving the original intensities (e.g. an image consisting of labels). Trilinear Interpolation is OK for PET, or realigned and re-sliced fMRI. If subject movement (from an fMRI time series) is included in the transformations then it may be better to use a higher degree approach. Note that higher degree B-spline interpolation is slower because it uses more neighbours.

    * **Wrapping** (choose from the menu)  
    This indicates which directions in the volumes the values should wrap around in. These are typically:
        * **No wrapping** - for PET or images that have already been spatially transformed.
        * **Wrap Y**  - for (un-resliced) MRI where phase encoding is in the Y direction (voxel space).

    * **Masking** (choose from the menu)  
    Because of subject motion, different images are likely to have different patterns of zeros from where it was not possible to sample data. With masking enabled, the program searches through the whole time series looking for voxels which need to be sampled from outside the original images. Where this occurs, that voxel is set to zero for the whole set of images (unless the image format can represent NaN, in which case NaNs are used where possible).

    * **Filename Prefix** (enter text)  
    String to be prepended to the filenames of the resliced image file(s). Default prefix is ``r``.
