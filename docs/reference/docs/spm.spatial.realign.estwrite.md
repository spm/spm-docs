# Realign: Estimate & Reslice  
Realign a time-series of images acquired from the same subject using a least squares approach and a 6 parameter (rigid body) spatial transformation.  
.  
The first image in the list specified by the user is used as a reference to which all subsequent scans are realigned. The reference scan does not have to be the first chronologically and it may be wise to chose a "representative scan" in this role.  
.  
The aim is primarily to remove movement artefact in fMRI and PET time-series (or more generally longitudinal studies). The headers are modified for each of the input images, such that. they reflect the relative orientations of the data. The details of the transformation are displayed in the results window as plots of translation and rotation. A set of realignment parameters are saved for each session, named rp_*.txt. After realignment, the images are resliced such that they match the first image selected voxel-for-voxel. The resliced images are named the same as the originals, except that they are prefixed by ``r``.  

* **Data** (create a list of items)  
Add new sessions for this subject. In the estimation step, the sessions are first realigned to each other, by aligning the first scan from each session to the first scan of the first session.  Then the images within each session are aligned to the first image of the session. The parameter estimation is performed this way because it is assumed (rightly or not) that there may be systematic differences in the images between sessions.  

    * **Session** (select files)  
    Select scans for this session. In the estimation step, the sessions are first realigned to each other, by aligning the first scan from each session to the first scan of the first session.  Then the images within each session are aligned to the first image of the session. The parameter estimation is performed this way because it is assumed (rightly or not) that there may be systematic differences in the images between sessions.  

* **Estimation Options**   
Various registration options. If in doubt, simply keep the default values.  

    * **Quality** (enter text)  
    Quality versus speed trade-off. Highest quality (1) gives most precise results, whereas lower qualities gives faster realignment. The idea is that some voxels contribute little to the estimation of the realignment parameters. This parameter is involved in selecting the number of voxels that are used.  

    * **Separation** (enter text)  
    The separation (in mm) between the points sampled in the reference image.  
    Smaller sampling distances gives more accurate results, but will be slower.  

    * **Smoothing (FWHM)** (enter text)  
    The FWHM of the Gaussian smoothing kernel (mm) applied to the images before estimating the realignment parameters.  

    * **Num Passes** (choose from the menu)  
    Register to first: Images are registered to the first image in the series.  Register to mean: A two pass procedure is used in order to register the images to the mean of the images after the first realignment.  

    * **Interpolation** (choose from the menu)  
    The method by which the images are sampled when estimating the optimum transformation.  
    Higher degree interpolation methods provide the better interpolation, but they are slower because they use more neighbouring voxels.  

    * **Wrapping** (choose from the menu)  
    Directions in the volumes the values should wrap around in. For example, in MRI scans, the images wrap around in the phase encode direction, so (e.g.) the subject's nose may poke into the back of the subject's head. These are typically:  
        * **No wrapping** - for PET or images that have already been spatially transformed. Also the recommended option if you are not really sure.  
        * **Wrap in  Y**  - for (un-resliced) MRI where phase encoding is in the Y direction (voxel space).  

    * **Weighting** (select files)  
    Optional weighting image to weight each voxel of the reference image differently when estimating the realignment parameters. The weights are proportional to the inverses of the standard deviations.This would be used, for example, when there is a lot of extra-brain motion - e.g., during speech, or when there are serious artifacts in a particular region of the images.  

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
