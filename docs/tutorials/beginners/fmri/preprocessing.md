# Spatial preprocessing

## Display

Display one of the functional image volumes using the `Display` button. Note
the orbitofrontal and inferior temporal drop-out and ghosting. This can be
seen more clearly by selecting `Brighten` from the `Effects` menu in the
`Colours` menu from the `SPM Figure` tab at the top of the Graphics
window.


## Realignment

Under the spatial pre-processing section of the SPM base window select
`Realign (Est & Res)` from the
`Realign` pull-down menu. This will call up
a realignment job specification in the batch editor window. Then

- Highlight data, select `New Session`, then highlight the newly created
  `Session` option. There are two fMRI runs, so you need to do this twice.

- For the first session, select `Specify Files` and use the SPM file
  selector to choose all of the functional image volumes for the first run.
  To select all the volumes in a file, change the field that says `1` to `NaN`.

- For the second session, select `Specify Files` and use the SPM file
  selector to choose all of the functional image volumes for the second run.

- The other options in the user interface can be left as they are.

- Save the job file as e.g. `realign_job.m`. When saved as a `.m` file, the jobs are human readable text that can serve as a starting point for creating scripts.

- Press the `Run` button in the batch editor window (green triangle).

This will run the realign job which will write realigned images into the
folder where the functional images are. These new images will be
prefixed with the letter "`r`". SPM will then plot the estimated time
series of translations and rotations. These data, the realignment
parameters for each run, are also saved to a file called `func/rp_sub-XX_ses-mri_task-facerecognition_run-XX_bold.txt`, so
that these variables can be used as regressors when fitting GLMs. This
allows movements effects to be discounted when looking for brain
activations.

SPM will also create a mean image (`func/meansub-XX_ses-mri_task-facerecognition_run-01_bold.nii`)
which will be used in the next step of spatial processing -
coregistration.


## Coregistration

Select `Coregister (Estimate)` from the
`Coregister` pull-down menu. This will call up the specification of a
coregistration job in the batch editor window.

- Highlight `Fixed Image` and then select the mean functional image
  `func/meansub-XX_ses-mri_task-facerecognition_run-01_bold.nii`.

- Highlight `Moved Image` and then select the anatomical image
  `anat/sub-XX-T1w.nii`.

- Press the `Save` button and save the job as `coreg_job.m`

- Then press the `Run` button.

SPM will then coregister the anatomical image so that it matches the
functional data (as far as possible using a rigid-body transform).
SPM will have changed the header of the moved (anatomical) image to reflect its new
position and orientation.


## Segmentation

Press the `Segment` button. This will call
up the specification of a segmentation job in the batch editor window.
Highlight the `Volumes` field in `Data > Channels` and then select the
subject's coregistered anatomical image `anat/sub-XX-T1w.nii`. Change
`Save INU corrected` so that it contains `Save INU corrected` instead
of `Save nothing`. At the bottom of the list, select `Forward` in
`Deformation Fields`. Save the job file as `segment_job.m` and then press
the `Run` button. SPM will segment the structural image using the
default tissue probability maps as priors. SPM will create, by default,
gray and white matter images and an intensity non-uniformity corrected version of the image.
These can be viewed using the `Check Reg` facility.

SPM will also write a spatial normalisation deformation field file 
`anat/y_sub-XX-T1w.nii` in the original `anat` folder. This
will be used in the next section to spatially normalise the functional data.

## Normalise

Select `Normalise (Write)` from the
`Normalise` pull-down menu. This will call
up the specification of a normalise job in the batch editor window.

- Highlight `Data`, select `New Subject`.

- Open `Subject`, highlight `Deformation field` and select the
  `anat/y_sub-XX-T1w.nii` file that you created in the previous section.

- Highlight `Images to write` and select all of the 
  realigned functional images (`func/sub-XX_ses-mri_task-facerecognition_run-01_bold.nii` and `func/sub-XX_ses-mri_task-facerecognition_run-02_bold.nii`).

- Press `Save`, save the job as `norm_func_job.m` and then press the `Run`
  button.

SPM will then write spatially normalised versions of the images to the `func` folder.
These files have the prefix `w`.

If you wish to superimpose a subject's functional activations on their
own anatomy [^3] you will also need to apply the spatial normalisation
parameters to their (intensity non-uniformity corrected) anatomical image. To do this

- Select `Normalise (Write)`, highlight `Data`, select `New Subject`.

- Highlight `Deformation field`, select the `anat/y_sub-XX-T1w.nii` file
  that you created previously and press `Done`.

- Highlight `Images to Write`, select the intensity non-uniformity corrected anatomical MRI
  `anat/msub-XX-T1w.nii` and press `Done`.

- Open `Writing Options`, select voxel sizes and change the default \[`2
  2 2`\] to \[`1 1 1`\] which better matches the original resolution of the
  images.

- Save the job as `norm_struct_job.m` and press the `Run` button.

## Smoothing

Press the `Smooth` button. This will
call up the specification of a smooth job in the batch editor window.

- Select `Images to Smooth` and then select the spatially normalised
  volumes created in the last section (`func/wrsub-XX_ses-mri_task-facerecognition_run-01_bold.nii` and `func/wrsub-XX_ses-mri_task-facerecognition_run-02_bold.nii`).

- Save the job as `smooth_job.m` and press the `Run` button.

This will smooth the data by (the default) 8 mm in each direction.

--8<-- "addons/abbreviations.md"
