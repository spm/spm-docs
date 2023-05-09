## Spatial pre-processing

### Display

Display eg. the first functional image using the "Display" button. Note
orbitofrontal and inferior temporal drop-out and ghosting. This can be
seen more clearly by selecting "Brighten" from the "Effects" menu in the
"Colours" menu from the "SPM Figure" tab at the top of the Graphics
window.

<figure id="dropout">
<div class="center">
<img src="../../../assets/figures/manual/faces/dropout.png" style="width:100mm" />
</div>
<figcaption><em>Signal dropout in EPI images. <span id="dropout"
label="dropout"></span></em></figcaption>
</figure>

### Realignment

Under the spatial pre-processing section of the SPM base window select
<span class="smallcaps">Realign (Est & Res)</span> from the
<span class="smallcaps">Realign</span> pulldown menu. This will call up
a realignment job specification in the batch editor window. Then

- Highlight data, select "New Session", then highlight the newly created
  "Session" option.

- Select "Specify Files" and use the SPM file selector to choose all of
  your functional images eg. `sM03953_0005_*.img`. You should select 351
  files.

- Save the job file as eg. `DIR/jobs/realign.mat`.

- Press the `Run` button in the batch editor window (green triangle).

This will run the realign job which will write realigned images into the
directory where the functional images are. These new images will be
prefixed with the letter "`r`". SPM will then plot the estimated time
series of translations and rotations shown in
Figure <a href="#face_realign" data-reference-type="ref"
data-reference="face_realign">1.5</a>. These data, the realignment
parameters, are also saved to a file eg. `rp_sM03953_0005_0006.txt`, so
that these variables can be used as regressors when fitting GLMs. This
allows movements effects to be discounted when looking for brain
activations.

SPM will also create a mean image eg. `meansM03953_0005_0006.{hdr,img}`
which will be used in the next step of spatial processing -
coregistration.

<figure id="face_realign">
<div class="center">
<img src="../../../assets/figures/manual/faces/realign.png" style="width:100mm" />
</div>
<figcaption><em><strong>Realignment of face data</strong>: Movement less
than the size of a voxel, which for this data set is 3mm, is not
considered problematic. <span id="face_realign"
label="face_realign"></span></em></figcaption>
</figure>

### Slice timing correction

Press the <span class="smallcaps">Slice timing</span> button. This will
call up the specification of a slice timing job in the batch editor
window. Note that these data consist of N=24 axial slices acquired
continuously with a TR=2s (ie TA = TR - TR/N, where TA is the time
between the onset of the first and last slice of one volume, and the TR
is the time between the onset of the first slice of one volume and the
first slice of next volume) and in a descending order (ie, most superior
slice was sampled first). The data however are ordered within the file
such that the first slice (slice number 1) is the most inferior slice,
making the slice acquisition order \[24 23 22 \... 1\].

- Highlight "Data" and select "New Sessions"

- Highlight the newly create "Sessions" option, "Specify Files" and
  select the 351 realigned functional images using the filter `^r.*`.

- Select "Number of Slices" and enter 24.

- Select TR and enter 2.

- Select TA and enter 1.92 (or 2 - 2/24).

- Select "Slice order" and enter 24:-1:1.

- Select "Reference Slice", and enter 12.

- Save the job as `slice_timing.mat` and press the "Run" button.

SPM will write slice-time corrected files with the prefix "`a`" in the
functional data directory.

### Coregistration

Select <span class="smallcaps">Coregister (Estimate)</span> from the
`Coregister` pulldown menu. This will call up the specification of a
coregistration job in the batch editor window.

- Highlight "Reference Image" and then select the mean functional image
  `meansM03953_0005_0006.img`.

- Highlight "Source Image" and then select the structural image eg.
  `sM03953_0007.img`.

- Press the "Save" button and save the job as `coreg.job`

- Then press the "Run" button.

SPM will then implement a coregistration between the structural and
functional data that maximises the mutual information. The image in
figure <a href="#face_coreg" data-reference-type="ref"
data-reference="face_coreg">1.6</a> should then appear in the Graphics
window. SPM will have changed the header of the source file which in
this case is the structural image `sM03953_0007.hdr`.

<figure id="face_coreg">
<div class="center">
<img src="../../../assets/figures/manual/faces/coreg.png" style="width:100mm" />
</div>
<figcaption><em>Mutual Information Coregistration of Face data.<span
id="face_coreg" label="face_coreg"></span></em></figcaption>
</figure>

### Segmentation

Press the <span class="smallcaps">Segment</span> button. This will call
up the specification of a segmentation job in the batch editor window.
Highlight the "Volumes" field in "Data $>$ Channels" and then select the
subjects coregistered anatomical image eg. `sM03953_0007.img`. Change
"Save Bias Corrected" so that it contains "Save Bias Corrected" instead
of "Save Nothing". At the bottom of the list, select "Forward" in
"Deformation Fields". Save the job file as `segment.mat` and then press
the `Run` button. SPM will segment the structural image using the
default tissue probability maps as priors. SPM will create, by default,
gray and white matter images and bias-field corrected structral image.
These can be viewed using the CheckReg facility as described in the
previous section. Figure <a href="#face_gray" data-reference-type="ref"
data-reference="face_gray">1.7</a> shows the gray matter image,
`c1sM03953_0007.nii`, along with the original structural[^2].

<figure id="face_gray">
<div class="center">
<img src="../../../assets/figures/manual/faces/gray.png" style="width:100mm" />
</div>
<figcaption><em>Gray matter (top) produced by segmentation of structural
image (below). <span id="face_gray"
label="face_gray"></span></em></figcaption>
</figure>

SPM will also write a spatial normalisation deformation field file eg.
`y_sM03953_0007.nii` file in the original structural directory. This
will be used in the next section to normalise the functional data.

### Normalise

Select <span class="smallcaps">Normalise (Write)</span> from the
<span class="smallcaps">Normalise</span> pulldown menu. This will call
up the specification of a normalise job in the batch editor window.

- Highlight "Data", select "New Subject".

- Open "Subject", highlight "Deformation field" and select the
  `y_sM03953_0007.nii` file that you created in the previous section.

- Highlight "Images to write" and select all of the slice-time
  corrected, realigned functional images `arsM*.img`. Note: This can be
  done efficiently by changing the filter in the SPM file selector to
  `^ar.*`. You can then right click over the listed files, choose
  "Select all". You might also want to select the mean functional image
  created during realignment (which would not be affected by slice-time
  correction), i.e, the `meansM03953_0005_006.img`. Then press "Done".

- Open "Writing Options", and change "Voxel sizes" from \[2 2 2\] to \[3
  3 3\][^3].

- Press "Save", save the job as normalise.mat and then press the `Run`
  button.

SPM will then write spatially normalised files to the functional data
directory. These files have the prefix "`w`".

If you wish to superimpose a subject's functional activations on their
own anatomy[^4] you will also need to apply the spatial normalisation
parameters to their (bias-corrected) anatomical image. To do this

- Select <span class="smallcaps">Normalise (Write)</span>, highlight
  'Data', select "New Subject".

- Highlight "Deformation field", select the `y_sM03953_0007.nii` file
  that you created in the previous section, press "Done".

- Highlight "Images to Write", select the bias-corrected structural eg.
  `msM03953_0007.nii`, press "Done".

- Open "Writing Options", select voxel sizes and change the default \[2
  2 2\] to \[1 1 1\] which better matches the original resolution of the
  images \[1 1 1.5\].

- Save the job as `norm_struct.mat` and press `Run` button.

### Smoothing

Press the <span class="smallcaps">Smooth</span> button[^5]. This will
call up the specification of a smooth job in the batch editor window.

- Select "Images to Smooth" and then select the spatially normalised
  files created in the last section eg. `war*.img`.

- Save the job as smooth.mat and press `Run` button.

This will smooth the data by (the default) 8mm in each direction, the
default smoothing kernel width.

<figure id="face_smooth">
<div class="center">
<img src="../../../assets/figures/manual/faces/smooth.png" style="width:100mm" />
</div>
<figcaption><em>Functional image (top) and 8mm-smoothed functional image
(bottom). These images were plotted using SPM’s "CheckReg" facility.
<span id="face_smooth" label="face_smooth"></span></em></figcaption>
</figure>
