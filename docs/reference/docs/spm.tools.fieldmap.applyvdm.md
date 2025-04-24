# Apply VDM   
Apply VDM (voxel displacement map) to resample voxel values in selected image(s). This allows a VDM to be applied to any images which are assumed to be already realigned (e.g. including EPI fMRI time series and DTI data).
The VDM can be created from a field map acquisition using the FieldMap toolbox and comprises voxel shift values which describe geometric distortions occuring as a result of magnetic susceptbility artefacts. Distortions along any single dimension can be corrected therefore input data may have been acquired with phase encode directions in X, Y (most typical) and Z.
The selected images are assumed to be realigned to the first in the time series (e.g. using Realign: Estimate) but do not need to be resliced. The VDM is assumed to be in alignment with the images selected for resampling (note this can be achieved via the FieldMap toolbox). The resampled images are written to the input subdirectory with the same (prefixed) filename.
e.g. The typical processing steps for fMRI time series would be 1) Realign: Estimate, 2) FieldMap to create VDM, 3) Apply VDM.
Note that this routine is a general alternative to using the VDM in combination with Realign & Unwarp which estimates and corrects for the combined effects of static and movement-related susceptibility induced distortions. Apply VDM can be used when dynamic distortions are not (well) modelled by Realign & Unwarp (e.g. for fMRI data acquired with R->L phase-encoding direction, high field fMRI data or DTI data).

* **Data** (create a list of items)  
Subjects or sessions for which VDM file is being applied to images.

    * **Session**   
    Data for this session.

        * **Images** (select files)  
        Select scans for this session. These are assumed to be realigned to the first in the time series (e.g. using Realign: Estimate) but do not need to be resliced

        * **Fieldmap (vdm* file)** (select files)  
        Select VDM (voxel displacement map) for this session (e.g. created via FieldMap toolbox). This is assumed to be in alignment with the images selected for resampling (note this can be achieved via the FieldMap toolbox).

* **Reslice Options**   
Apply VDM reslice options

    * **Distortion direction** (choose from the menu)  
    In which direction are the distortions? Any single dimension can be corrected therefore input data may have been acquired with phase encode directions in Y (most typical), X or Z

    * **Resliced images** (choose from the menu)  
    All Images (1..n) 
      This applies the VDM and reslices all the images. 
    All Images + Mean Image 
       This applies the VDM reslices all the images and creates a mean of the resliced images.

    * **Interpolation** (choose from the menu)  
    The method by which the images are sampled when being written in a different space. Nearest Neighbour is fastest, but not recommended for image realignment. Trilinear Interpolation is probably OK for PET, but not so suitable for fMRI because higher degree interpolation generally gives better results. Although higher degree methods provide better interpolation, but they are slower because they use more neighbouring voxels.

    * **Wrapping** (choose from the menu)  
    This indicates which directions in the volumes the values should wrap around in.  For example, in MRI scans, the images wrap around in the phase encode direction, so (e.g.) the subject's nose may poke into the back of the subject's head. These are typically:
        No wrapping - for PET or images that have already been spatially transformed. Also the recommended option if you are not really sure.
        Wrap in  Y  - for (un-resliced) MRI where phase encoding is in the Y direction (voxel space) etc.

    * **Masking** (choose from the menu)  
    Because of subject motion, different images are likely to have different patterns of zeros from where it was not possible to sample data. With masking enabled, the program searches through the whole time series looking for voxels which need to be sampled from outside the original images. Where this occurs, that voxel is set to zero for the whole set of images (unless the image format can represent NaN, in which case NaNs are used where possible).

    * **Filename Prefix** (enter text)  
    Specify the string to be prepended to the filenames of the distortion corrected image file(s). Default prefix is 'u'.
