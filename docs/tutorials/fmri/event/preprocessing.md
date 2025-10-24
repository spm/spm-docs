# Spatial preprocessing

## Display

Display eg. the first functional image using the `Display` button. Note
orbitofrontal and inferior temporal drop-out and ghosting. This can be
seen more clearly by selecting `Brighten` from the `Effects` menu in the
`Colours` menu from the `SPM Figure` tab at the top of the Graphics
window.

<figure>
<div class="center">
<img src="../../../../assets/figures/manual/faces/dropout.png" style="width:100mm" />
</div>
<figcaption>Signal dropout in EPI images.</figcaption>
</figure>

## Realignment

Under the spatial pre-processing section of the SPM base window select
`Realign (Est & Res)` from the
`Realign` pulldown menu. This will call up
a realignment job specification in the batch editor window. Then

- Highlight data, select `New Session`, then highlight the newly created
  `Session` option.

- Select `Specify Files` and use the SPM file selector to choose all of
  your functional images eg. `sM03953_0005_*.img`. You should select 351
  files.

- Save the job file as eg. `DIR/jobs/realign.mat`.

- Press the `Run` button in the batch editor window (green triangle).

This will run the realign job which will write realigned images into the
directory where the functional images are. These new images will be
prefixed with the letter "`r`". SPM will then plot the estimated time
series of translations and rotations. These data, the realignment
parameters, are also saved to a file eg. `rp_sM03953_0005_0006.txt`, so
that these variables can be used as regressors when fitting GLMs. This
allows movements effects to be discounted when looking for brain
activations.

SPM will also create a mean image eg. `meansM03953_0005_0006.{hdr,img}`
which will be used in the next step of spatial processing -
coregistration.

<figure>
<div class="center">
<img src="../../../../assets/figures/manual/faces/realign.png" style="width:100mm" />
</div>
<figcaption><strong>Realignment of face data</strong>: Movement less
than the size of a voxel, which for this data set is 3mm, is not
considered problematic.</figcaption>
</figure>

## Slice timing correction

Press the `Slice timing` button. This will
call up the specification of a slice timing job in the batch editor
window. Note that these data consist of `N=24` axial slices acquired
continuously with a TR=2s (ie `TA = TR - TR/N`, where TA is the time
between the onset of the first and last slice of one volume, and the TR
is the time between the onset of the first slice of one volume and the
first slice of next volume) and in a descending order (ie, most superior
slice was sampled first). The data however are ordered within the file
such that the first slice (slice number 1) is the most inferior slice,
making the slice acquisition order \[`24 23 22 ... 1`\].

- Highlight `Data` and select `New Sessions`

- Highlight the newly create `Sessions` option, `Specify Files` and
  select the 351 realigned functional images using the filter `^r.*`.

- Select `Number of Slices` and enter `24`.

- Select `TR` and enter `2`.

- Select `TA` and enter `1.92` (or `2 - 2/24`).

- Select `Slice order` and enter `24:-1:1`.

- Select `Reference Slice`, and enter `12`.

- Save the job as `slice_timing.mat` and press the `Run` button.

SPM will write slice-time corrected files with the prefix `a` in the
functional data directory.

## Coregistration

Select `Coregister (Estimate)` from the
`Coregister` pulldown menu. This will call up the specification of a
coregistration job in the batch editor window.

- Highlight `Fixed Image` and then select the mean functional image
  `meansM03953_0005_0006.img`.

- Highlight `Moved Image` and then select the structural image eg.
  `sM03953_0007.img`.

- Press the `Save` button and save the job as `coreg.job`

- Then press the `Run` button.

SPM will then implement a coregistration between the structural and
functional data that maximises the mutual information. The image
displayed below should then appear in the Graphics
window. SPM will have changed the header of the moved file, which in
this case is the structural image `sM03953_0007.hdr`.

<figure>
<div class="center">
<img src="../../../../assets/figures/manual/faces/coreg.png" style="width:100mm" />
</div>
<figcaption>Mutual Information Coregistration of Face data.</figcaption>
</figure>

## Segmentation

Press the `Segment` button. This will call
up the specification of a segmentation job in the batch editor window.
Highlight the `Volumes` field in `Data > Channels` and then select the
subjects coregistered anatomical image eg. `sM03953_0007.img`. Change
`Save INU corrected` so that it contains `Save INU corrected` instead
of `Save nothing`. At the bottom of the list, select `Forward` in
`Deformation Fields`. Save the job file as `segment.mat` and then press
the `Run` button. SPM will segment the structural image using the
default tissue probability maps as priors. SPM will create, by default,
gray and white matter images and intensity nonuniformity corrected structural image.
These can be viewed using the `CheckReg` facility as described in the
previous section [^1].

<figure>
<div class="center">
<img src="../../../../assets/figures/manual/faces/gray.png" style="width:100mm" />
</div>
<figcaption>Grey matter (top) produced by segmentation of structural
image (below).</figcaption>
</figure>

SPM will also write a spatial normalisation deformation field file eg.
`y_sM03953_0007.nii` file in the original structural directory. This
will be used in the next section to normalise the functional data.

## Normalise

Select `Normalise (Write)` from the
`Normalise` pulldown menu. This will call
up the specification of a normalise job in the batch editor window.

- Highlight `Data`, select `New Subject`.

- Open `Subject`, highlight `Deformation field` and select the
  `y_sM03953_0007.nii` file that you created in the previous section.

- Highlight `Images to write` and select all of the slice-time
  corrected, realigned functional images `arsM*.img`. Note: This can be
  done efficiently by changing the filter in the SPM file selector to
  `^ar.*`. You can then right click over the listed files, choose
  `Select all`. You might also want to select the mean functional image
  created during realignment (which would not be affected by slice-time
  correction), i.e, the `meansM03953_0005_006.img`. Then press `Done`.

- Open `Writing Options`, and change `Voxel sizes` from \[`2 2 2`\] to \[`3
  3 3`\][^2].

- Press `Save`, save the job as normalise.mat and then press the `Run`
  button.

SPM will then write spatially normalised files to the functional data
directory. These files have the prefix `w`.

If you wish to superimpose a subject's functional activations on their
own anatomy [^3] you will also need to apply the spatial normalisation
parameters to their (intensity nonuniformity corrected) anatomical image. To do this

- Select `Normalise (Write)`, highlight `Data`, select `New Subject`.

- Highlight `Deformation field`, select the `y_sM03953_0007.nii` file
  that you created in the previous section, press `Done`.

- Highlight `Images to Write`, select the intensity nonuniformity corrected structural eg.
  `msM03953_0007.nii`, press `Done`.

- Open `Writing Options`, select voxel sizes and change the default \[`2
  2 2`\] to \[`1 1 1`\] which better matches the original resolution of the
  images \[`1 1 1.5`\].

- Save the job as `norm_struct.mat` and press `Run` button.

## Smoothing

Press the `Smooth` button [^4]. This will
call up the specification of a smooth job in the batch editor window.

- Select `Images to Smooth` and then select the spatially normalised
  files created in the last section eg. `war*.img`.

- Save the job as smooth.mat and press `Run` button.

This will smooth the data by (the default) 8mm in each direction, the
default smoothing kernel width.

<figure>
<div class="center">
<img src="../../../../assets/figures/manual/faces/smooth.png" style="width:100mm" />
</div>
<figcaption>Functional image (top) and 8mm-smoothed functional image
(bottom). These images were plotted using SPM’s "CheckReg" facility.
</figcaption>
</figure>

[^1]: Segmentation can sometimes fail if the source (structural) image
    is not close in orientation to the MNI templates. It is generally
    advisable to manually orient the structural to match the template
    (ie MNI space) as close as possible by using the "Display" button,
    adjusting x/y/z/pitch/roll/yaw, and then pressing the "Reorient"
    button.

[^2]: This step is not strictly necessary. It will write images out at a
    resolution closer to that at which they were acquired. This will
    speed up subsequent analysis and is necessary, for example, to make
    Bayesian fMRI analysis computationally efficient.

[^3]: Beginners may wish to skip this step, and instead just superimpose
    functional activations on an "canonical structural image".

[^4]: The smoothing step is unnecessary if you are only interested in
    Bayesian analysis of your functional data.

--8<-- "addons/abbreviations.md"
