# Face fMRI data <span id="Chap:data:faces" label="Chap:data:faces"></span>

As another, more sophisticated example, consider the data from a
repetition priming experiment performed using event-related fMRI.
Briefly, this is a 2$\times$<!-- -->2 factorial study with factors
"fame" and "repetition" where famous and non-famous faces were presented
twice against a checkerboard baseline (for more details, see (Henson et
al. 2002)). The subject was asked to make fame judgements by making key
presses. There are thus four event-types of interest; first and second
presentations of famous and non-famous faces, which we denote N1, N2, F1
and F2. The experimental stimuli and timings of events are shown in
Figures <a href="#face_stim" data-reference-type="ref"
data-reference="face_stim">1.1</a> and
<a href="#face_timing" data-reference-type="ref"
data-reference="face_timing">1.2</a>.

<figure id="face_stim">
<div class="center">
<img src="../../../assets/figures/manual/faces/face_stim.png" style="width:120mm" />
</div>
<figcaption><em><strong>Face repetition paradigm</strong>: There were 2
presentations of 26 Famous and 26 Nonfamous Greyscale photographs, for
0.5s each, randomly intermixed. The minimal Stimulus Onset Asynchrony
(SOA)=4.5s, with probability 2/3 (ie 1/3 null events). The subject made
one of two right finger key presses denoting whether or not the subject
thought the face was famous. <span id="face_stim"
label="face_stim"></span></em></figcaption>
</figure>

<figure id="face_timing">
<div class="center">
<img src="../../../assets/figures/manual/faces/face_timing.png" style="width:140mm" />
</div>
<figcaption><em>Time series of events. <span id="face_timing"
label="face_timing"></span></em></figcaption>
</figure>

Images were acquired using continuous Echo-Planar Imaging (EPI) with
TE=40ms, TR=2s and 24 descending slices (64$\times$<!-- -->64
3$\times$<!-- -->3 mm$^2$), 3mm thick with a 1.5mm gap. The data archive
is available from the SPM website[^1]. This contains 351 Analyze format
functional images `sM03953_0005_*.{hdr,img}` of dimension
64$\times$<!-- -->64$\times$<!-- -->24 with
3$\times$<!-- -->3$\times$<!-- -->4.5 mm$^3$ voxels. A structural image
is also provided in Analyze format (`sM03953_0007.{hdr,img}`).

To analyse the data, first create a new directory `DIR` eg.
`C:`$\backslash$`data`$\backslash$`face_rep`, in which to place the
results of your analysis. Then create 4 subdirectories (i) `jobs`, (ii)
`categorical`, (iii) `parametric` and (iv) `bayesian`. As the analysis
proceeds these directories will be filled with job-specification files,
design matrices and models estimated using classical or Bayesian
methods.

As well as the classical/Bayesian distinction we will show how this data
can be analysed from a parametric as well as a categorical perspective.
We will look at the main effects of fame and repetition and in the
parameteric analysis we will look at responses as a function of "lag",
that is, the number of faces intervening between repetition of a
specific face.

Start up MATLAB, enter your jobs directory and type `spm fmri` at the
MATLAB prompt. SPM will then open in fMRI mode with three windows (1)
the top-left or "Menu" window, (2) the bottom-left or "Interactive"
window and (3) the right-hand or "Graphics" window. Analysis then takes
place in three major stages (i) spatial pre-processing, (ii) model
specification, review and estimation and (iii) inference. These stages
organise the buttons in SPM's base window.

<figure id="command">
<div class="center">
<img src="../../../assets/figures/manual/faces/command.png" style="width:100mm" />
</div>
<figcaption><em>The SPM base window comprises three sections (i) spatial
pre-processing, (ii) model specification, review and estimation and
(iii) inference. <span id="command"
label="command"></span></em></figcaption>
</figure>

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

## Modelling categorical responses

Before setting up the design matrix we must first load the Stimulus
Onsets Times (SOTs) into MATLAB . SOTs are stored in the `sots.mat` file
in a cell array such that eg. `sot{1}` contains stimulus onset times in
TRs for event type 1, which is N1. Event-types 2, 3 and 4 are N2, F1 and
F2.[^6]

- At the MATLAB command prompt type `load sots`

Now press the <span class="smallcaps">Specify 1st-level</span> button.
This will call up the specification of a fMRI specification job in the
batch editor window. Then

- For "Directory", select the "categorical" folder you created earlier,

- In the "Timing parameters" option,

- Highlight "Units for design" and select "Scans",

- Highlight "Interscan interval" and enter 2,

- Highlight "Microtime resolution" and enter 24,

- Highlight "Microtime onset" and enter 12. These last two options make
  the creating of regressors commensurate with the slice-time correction
  we have applied to the data, given that there are 24 slices and that
  the reference slice to which the data were slice-time corrected was
  the 12th (middle slice in time).

- Highlight "Data and Design" and select "New Subject/Session".

- Highlight "Scans" and use SPM's file selector to choose the 351
  smoothed, normalised, slice-time corrected, realigned functional
  images ie `swarsM.img`. These can be selected easily using the
  `^swar.*` filter, and select all. Then press "Done".

- Highlight "Conditions" and select "New condition"[^7].

- Open the newly created "Condition" option. Highlight "Name" and enter
  "N1". Highlight "Onsets" and enter `sot{1}`. Highlight "Durations" and
  enter 0.

- Highlight "Conditions" and select "Replicate condition".

- Open the newly created "Condition" option (the lowest one). Highlight
  "Name" and change to "N2". Highlight "Onsets" and enter `sot{2}`.

- Highlight "Conditions" and select "Replicate condition".

- Open the newly created "Condition" option (the lowest one). Highlight
  "Name" and change to "F1". Highlight "Onsets" and enter `sot{3}`.

- Highlight "Conditions" and select "Replicate condition".

- Open the newly created "Condition" option (the lowest one). Highlight
  "Name" and change to "F2". Highlight "Onsets" and enter `sot{4}`.

- Highlight "Multiple Regressors" and select the realignment parameter
  file `rp_sM03953_0005_0006.txt` file that was saved during the
  realignment preprocessing step in the folder containing the fMRI
  data[^8].

- Highlight "Factorial Design", select "New Factor", open the newly
  created "Factor" option, highlight "Name" and enter "Fam", highlight
  "Levels" and enter 2.

- Highlight "Factorial Design", select "New Factor", open the newly
  created "Factor" option, highlight "Name" and enter "Rep", highlight
  "Levels" and enter 2[^9].

- Open "Canonical HRF" under "Basis Functions". Select "Model
  derivatives" and select "Time and Dispersion derivatives".

- Highlight "Directory" and select the `DIR/categorical` directory you
  created earlier.

- Save the job as `categorical_spec.mat` and press the `Run` button.

SPM will then write an `SPM.mat` file to the `DIR/categorical`
directory. It will also plot the design matrix, as shown in
Figure <a href="#cat_design" data-reference-type="ref"
data-reference="cat_design">1.9</a>.

<figure id="cat_design">
<div class="center">
<img src="../../../assets/figures/manual/faces/cat_design.png" style="width:100mm" />
</div>
<figcaption><em><strong>Design matrix</strong>. <span id="cat_design"
label="cat_design"></span></em></figcaption>
</figure>

At this stage it is advisable to check your model specification using
SPM's review facility which is accessed via the "Review" button. This
brings up a "Design" tab on the interactive window clicking on which
produces a pulldown menu. If you select the first item "Design Matrix"
SPM will produce the image shown in
Figure <a href="#cat_design" data-reference-type="ref"
data-reference="cat_design">1.9</a>. If you select "Explore" then
"Session 1" then "N1", SPM will produce the plots shown in
Figure <a href="#cat_explore" data-reference-type="ref"
data-reference="cat_explore">1.10</a>.

<figure id="cat_explore">
<div class="center">
<img src="../../../assets/figures/manual/faces/cat_explore.png" style="width:100mm" />
</div>
<figcaption><em><strong>Exploring the design matrix in Figure <a
href="#cat_design" data-reference-type="ref"
data-reference="cat_design">1.9</a></strong>. This shows the time series
of the "N1" regressor (top left), the three basis functions used to
convert assumed neuronal activity into hemodynamic activity (bottom
left), and a frequency domain plot of the three regressors for the basis
functions in this condition (top right). The frequency domain plot shows
that the frequency content of the "N1" condition is generally above the
set frequencies that are removed by the High Pass Filter (HPF) (these
are shown in gray - in this model we accepted the default HPF cut-off of
128s or 0.008Hz). <span id="cat_explore"
label="cat_explore"></span></em></figcaption>
</figure>

### Estimate

Press the <span class="smallcaps">Estimate</span> button. This will call
up the specification of an fMRI estimation job in the batch editor
window. Then

- Highlight the "Select SPM.mat" option and then choose the `SPM.mat`
  file saved in the `DIR/categorical` directory.

- Save the job as `categorical_est.job` and press `Run` button.

SPM will write a number of files into the selected directory including
an `SPM.mat` file.

### Inference for categorical design

Press "Results" and select the SPM.mat file from `DIR/categorical`. This
will again invoke the contrast manager. Because we specified that our
model was using a "Factorial design" a number of contrasts have been
specified automatically, as shown in
Figure <a href="#cat_contrasts" data-reference-type="ref"
data-reference="cat_contrasts">1.11</a>.

<figure id="cat_contrasts">
<div class="center">
<img src="../../../assets/figures/manual/faces/cat_contrasts.png" style="width:100mm" />
</div>
<figcaption><em>Contrast Manager containing default contrasts for
categorical design. <span id="cat_contrasts"
label="cat_contrasts"></span></em></figcaption>
</figure>

- Select contrast number 5. This is a t-contrast
  `Positive effect of condition_1` This will show regions where the
  average effect of presenting faces is significantly positive, as
  modelled by the first regressor (hence the `_1`), the canonical HRF.
  Press 'Done''.

- *Apply masking ? \[None/Contrast/Image\]*

- Specify None.

- *p value adjustment to control: \[FWE/none\]*

- Select FWE

- *Corrected p value(family-wise error)*

- Accept the default value, 0.05

- *Extent threshold {voxels} \[0\]*

- Accept the default value, 0.

SPM will then produce the MIP shown in
Figure <a href="#cat5_volume" data-reference-type="ref"
data-reference="cat5_volume">1.12</a>.

### Statistical tables

To get a summary of local maxima, press the "whole brain" button in the
p-values section of the interactive window. This will list all clusters
above the chosen level of significance as well as separate
($>$<!-- -->8mm apart) maxima within a cluster, with details of
significance thresholds and search volume underneath, as shown in
Figure <a href="#cat5_volume" data-reference-type="ref"
data-reference="cat5_volume">1.12</a>

<figure id="cat5_volume">
<div class="center">
<img src="../../../assets/figures/manual/faces/cat5_volume.png" style="width:100mm" />
</div>
<figcaption><em><strong>MIP and Volume table for Canonical HRF</strong>:
Faces <span class="math inline">&gt;</span> Baseline. <span
id="cat5_volume" label="cat5_volume"></span></em> </figcaption>
</figure>

The columns in volume table show, from right to left:

- **x, y, z (mm)**: coordinates in MNI space for each maximum.

- **peak-level**: the chance (p) of finding (under the null hypothesis)
  a peak with this or a greater height (T- or Z-statistic), corrected
  (FWE or FDR)/ uncorrected for search volume.

- **cluster-level**: the chance (p) of finding a cluster with this
  many(ke) or a greater number of voxels, corrected (FWE or FDR)/
  uncorrected for search volume.

- **set-level**: the chance (p) of finding this (c) or a greater number
  of clusters in the search volume.

Right-click on the MIP and select "goto global maximum". The cursor will
move to \[39 -70 -14\]. You can view this activation on the subject's
normalised, bias-corrected structural (`wmsM03953_0007i̇mg`), which gives
best anatomical precision, or on the normalised mean functional
(`wmeansM03953_0005_0006.nii`), which is closer to the true data and
spatial resolution (including distortions in the functional EPI data).

If you select "plot" and choose "Contrast of estimates and 90% C.I"
(confidence interval), and select the "Average effect of condition"
contrast, you will see three bars corresponding to the parameter
estimates for each basis function (summed across the 4 conditions). The
BOLD impulse response in this voxel loads mainly on the canonical HRF,
but also significantly (given that the error bars do not overlap zero)
on the temporal and dispersion derivatives (see next Chapter).

### F-contrasts

To assess the main effect of repeating faces, as characterised by both
the hrf *and* its derivatives, an F-contrats is required. This is really
asking whether repetition changes the *shape* of the impulse response
(e.g, it might affect its latency but not peak amplitude), at least the
range of shapes defined by the three basis functions. Because we have
told SPM that we have a factorial design, this required contrast will
have been created automatically - it is number 3.

- Press "Results" and select the `SPM.mat` file in the `DIR/categorical`
  directory.

- Select the "F-contrast" toggle and the contrast number 3, as shown in
  Figure <a href="#cat3_contrast" data-reference-type="ref"
  data-reference="cat3_contrast">1.13</a>. Press "Done".

- *Apply masking ? \[None/Contrast/Image\]*.

- Specify "Contrast".

- Select contrast 5 - `Positive effect of condition_1` (the T-contrast
  of activation versus baseline, collapsed across conditions, that we
  evaluated above)

- *uncorrected mask p-value ?*

- Change to 0.001

- *nature of mask?*

- Select 'inclusive'

- *p value adjustment to control: \[FWE/none\]*

- Select none

- *threshold (F or p value)*

- Accept the default value, 0.001

- *Extent threshold {voxels} \[0\]*

- Accept the default value, 0

A MIP should then appear, the top half of which should look like
Figure <a href="#cat3_psth" data-reference-type="ref"
data-reference="cat3_psth">1.14</a>.

<figure id="cat3_contrast">
<div class="center">
<img src="../../../assets/figures/manual/faces/cat3_contrast.png" style="width:100mm" />
</div>
<figcaption><em>Contrast manager showing selection of the first contrast
"Main effect of Rep" (repetition: F1 and N1 vs F2 and N2)<span
id="cat3_contrast" label="cat3_contrast"></span></em> </figcaption>
</figure>

<figure id="cat3_psth">
<div class="center">
<img src="../../../assets/figures/manual/faces/cat3_psth.png" style="width:100mm" />
</div>
<figcaption><em>MIP for Main effect of Rep, masked inclusively with
Canonical HRF: Faces <span class="math inline">&gt;</span> Baseline at
p<span class="math inline">&lt;</span>.001 uncorrected. Shown below are
the best-fitting responses and peri-stimulus histograms (PSTH) for F1
and F2. <span id="cat3_psth" label="cat3_psth"></span></em>
</figcaption>
</figure>

Note that this contrast will identify regions showing any effect of
repetition (e.g, decreased or increased amplitudes) *within* those
regions showing activations (on the canonical HRF) to faces versus
baseline (at p$<$.05 uncorrected). Select "goto global max", which is in
right ventral temporal cortex \[42 -64 -8\].

If you press plot and select "Event-related responses", then "F1", then
"fitted response and PSTH", you will see the best fitting linear
combination of the canonical HRF and its two derivatives (thin red
line), plus the "selectively-averaged" data (peri-stimulus histogram,
PSTH), based on an FIR refit (see next Chapter). If you then select the
"hold" button on the Interactive window, and then "plot" and repeat the
above process for the "F2" rather than "F1" condition, you will see two
estimated event-related responses, in which repetition decreases the
peak response (ie F2$<$F1), as shown in
Figure <a href="#cat3_psth" data-reference-type="ref"
data-reference="cat3_psth">1.14</a>.

You can explore further F-contrasts, which are a powerful tool once you
understand them. For example, the MIP produced by the "Average effect of
condition" F-contrast looks similar to the earlier T-contrast, but
importantly shows the areas for which the sums across conditions of the
parameter estimates for the canonical hrf *and/or* its temporal
derivative *and/or* its dispersion derivative are different from zero
(baseline). The first row of this F-contrast (\[1 0 0 1 0 0 1 0 0 1 0
0\]) is also a two-tailed version of the above T-contrast, ie testing
for both activations and deactivations versus baseline. This also means
that the F-contrasts \[1 0 0 1 0 0 1 0 0 1 0 0\] and \[-1 0 0 -1 0 0 -1
0 0 -1 0 0\] are equivalent. Finally, note that an F- (or t-) contrast
such as \[1 1 1 1 1 1 1 1 1 1 1\], which tests whether the mean of the
canonical hrf AND its derivatives for all conditions are different from
(larger than) zero is not sensible. This is because the canonical hrf
and its temporal derivative may cancel each other out while being
significant in their own right. The basis functions are really quite
different things, and need to represent separate rows in an F-contrast.

### F-contrasts for testing effects of movement

To assess movement-related activation

- Press "Results", select the `SPM.mat` file, select "F-contrast" in the
  Contrast Manager. Specify e.g. "Movement-related effects" (name) and
  in the "contrasts weights matrix" window, or "1:12 19" in the "columns
  for reduced design" window.

- Submit and select the contrast, specify "Apply masking?" (none),
  "corrected height threshold" (FWE), and "corrected p-value" (accept
  default).

- When the MIP appears, select "sections" from the "overlays" pulldown
  menu, and select the normalised structural image
  (`wmsM03953_0007.nii`).

You will see there is a lot of residual movement-related artifact in the
data (despite spatial realignment), which tends to be concentrated near
the boundaries of tissue types (eg the edge of the brain; see
Figure <a href="#movements" data-reference-type="ref"
data-reference="movements">1.15</a>). (Note how the MIP can be
misleading in this respect, since though it appears that the whole brain
is affected, this reflects the nature of the (X-ray like) projections
onto each orthogonal view; displaying the same datae as sections in 3D
shows that not every voxel is suprathreshold.) Even though we are not
interested in such artifact, by including the realignment parameters in
our design matrix, we "covary out" (linear components) of subject
movement, reducing the residual error, and hence improve our statistics
for the effects of interest.

<figure id="movements">
<div class="center">
<img src="../../../assets/figures/manual/faces/movements.png" style="width:100mm" />
</div>
<figcaption><em>Movement-related activations. These spurious
‘activations’ are due to residual movement of the head during scanning.
These effects occur at tissue boundaries and boundaries between brain
and non-brain, as this is where contrast differences are greatest.
Including these regressors in the design matrix means these effects
cannot be falsely attributed to neuronal activity. <span id="movements"
label="movements"></span></em> </figcaption>
</figure>

## Modelling parametric responses

Before setting up the design matrix, we must first load into MATLAB the
Stimulus Onsets Times (SOTs), as before, and also the "Lags", which are
specific to this experiment, and which will be used as parametric
modulators. The Lags code, for each second presentation of a face (N2
and F2), the number of other faces intervening between this (repeated)
presentation and its previous (first) presentation. Both SOTs and Lags
are represented by Matlab cell arrays, stored in the `sots.mat` file.

- At the MATLAB command prompt type `load sots`. This loads the stimulus
  onset times and the lags (the latter in a cell array called `itemlag`.

Now press the <span class="smallcaps">Specify 1st-level</span> button.
This will call up the specification of a fMRI specification job in the
batch editor window. Then

- Press "Load" and select the `categorical_spec.mat` job file you
  created earlier.

- Open "Conditions" and then open the second "Condition".

- Highlight "Parametric Modulations", select "New Parameter".

- Highlight "Name" and enter "Lag", highlight values and enter
  `itemlag{2}`, highlight polynomial expansion and "2nd order".

- Now open the fourth "Condition" under "Conditions".

- Highlight "Parametric Modulations", select "New Parameter".

- Highlight "Name" and enter "Lag", highlight values and enter
  `itemlag{4}`, highlight polynomial expansion and "2nd order".

- Open "Canonical HRF" under "Basis Functions", highlight "Model
  derivatives" and select "No derivatives" (to make the design matrix a
  bit simpler for present purposes!).

- Highlight "Directory" and select `DIR/parametric` (having "unselected"
  the current definition of directory from the Categorical analysis).

- Save the job as `parametric_spec` and press the `Run` button.

This should produce the design matrix shown in
Figure <a href="#par_design" data-reference-type="ref"
data-reference="par_design">1.16</a>.

<figure id="par_design">
<div class="center">
<img src="../../../assets/figures/manual/faces/par_design.png" style="width:100mm" />
</div>
<figcaption><em><strong>Design matrix for testing repetition effects
parametrically.</strong> Regressor 2 indicates the second occurrence of
a nonfamous face. Regressor 3 modulates this linearly as a function of
lag (ie. how many faces have been shown since that face was first
presented), and regressor 4 modulates this quadratically as a function
of lag. Regressors 6,7 and 8 play the same roles, but for famous faces.
<span id="par_design" label="par_design"></span></em> </figcaption>
</figure>

### Estimate

Press the <span class="smallcaps">Estimate</span> button. This will call
up the specification of an fMRI estimation job in the batch editor
window. Then

- Highlight the "Select SPM.mat" option and then choose the `SPM.mat`
  file saved in the `DIR/parametric` directory.

- Save the job as `parametric_est.job` and press the `Run` button.

SPM will write a number of files into the selected directory including
an `SPM.mat` file.

### Plotting parametric responses

We will look at the effect of lag (up to second order, ie using linear
and quadratic terms) on the response to repeated Famous faces, within
those regions generally activated by faces versus baseline. To do this

- Press "Results" and select the `SPM.mat` file in the `DIR/parametric`
  directory.

- Press "Define new contrast", enter the name "Famous Lag", press the
  "F-contrast" radio button, enter "1:6 9:15" in the "columns in reduced
  design" window, press "submit", "OK" and "Done".

- Select the "Famous Lag" contrast.

- *Apply masking ? \[None/Contrast/Image\]*

- Specify "Contrast".

- Select the "Positive Effect of Condition 1" T contrast.

- Change to an 0.05 uncorrected mask p-value.

- Nature of Mask ? inclusive.

- *p value adjustment to control: \[FWE/none\]*

- Select None

- *Threshold {F or p value}*

- Accept the default value, 0.001

- *Extent threshold {voxels} \[0\]*

- Accept the default value, 0.

Figure <a href="#famous_lag_mip" data-reference-type="ref"
data-reference="famous_lag_mip">1.17</a> shows the MIP and an overlay of
this parametric effect using overlays, sections and selecting the
`wmsM03953_0007.nii` image.

<figure id="famous_lag_mip">
<div class="center">
<img src="../../../assets/figures/manual/faces/famous_lag_mip.png" style="width:100mm" />
</div>
<figcaption><em>MIP and overlay of parametric lag effect in parietal
cortex. <span id="famous_lag_mip" label="famous_lag_mip"></span></em>
</figcaption>
</figure>

The effect is plotted in the time domain in
figure <a href="#famous_lag" data-reference-type="ref"
data-reference="famous_lag">1.18</a>. This was obtained by

- Right clicking on the MIP and selecting "global maxima".

- Pressing Plot, and selecting "parametric responses" from the pull-down
  menu.

- Which effect ? select "F2".

This shows a quadratic effect of lag, in which the response appears
negative for short-lags, but positive and maximal for lags of about 40
intervening faces (note that this is a very approximate fit, since there
are not many trials, and is also confounded by time during the session,
since longer lags necessarily occur later (for further discussion of
this issue, see the SPM2 example analysis of these data on the webpage).

<figure id="famous_lag">
<div class="center">
<img src="../../../assets/figures/manual/faces/famous_lag.png" style="width:150mm" />
</div>
<figcaption><em>Response as a function of lag. <span id="famous_lag"
label="famous_lag"></span></em> </figcaption>
</figure>

## Bayesian analysis

### Specification

Press the <span class="smallcaps">Specify 1st-level</span> button. This
will call up an fMRI specification job in the batch editor window. Then

- Load the `categorical_spec.mat` job file created for the classical
  analysis.

- Open "Subject/Session", highlight "Scans".

- Deselect the smoothed functional images using the 'unselect all'
  option available from a right mouse click in the SPM file selector
  (bottom window).

- Select the unsmoothed functional images using the `^wa.*` filter and
  "select all" option available from a right mouse click in the SPM file
  selector (top right window). The Bayesian analysis uses a spatial
  prior where the spatial regularity in the signal is estimated from the
  data. It is therefore not necessary to create smoothed images if you
  are only going to do a Bayesian analysis.

- Press "Done".

- Highlight "Directory" and select the `DIR/bayesian` directory you
  created earlier (you will first need to deselect the `DIR/categorical`
  directory).

- Save the job as `specify_bayesian.mat` and press the `Run` button.

### Estimation

Press the <span class="smallcaps">Estimate</span> button. This will call
up the specification of an fMRI estimation job in the batch editor
window. Then

- Highlight the "Select SPM.mat" option and then choose the `SPM.mat`
  file saved in the `DIR/bayesian` subdirectory

- Highlight "Method" and select the "Choose Bayesian 1st-level" option.

- Save the job as `estimate_bayesian.job` and press the `Run` button.

<figure id="face_ar1">
<div class="center">
<img src="../../../assets/figures/manual/faces/face_ar1.png" style="width:100mm" />
</div>
<figcaption><em>Bayesian analysis: Estimated AR(1) coefficient image
indicating heterogeneity near the circle of Willis <span id="face_ar1"
label="face_ar1"></span></em> </figcaption>
</figure>

SPM will write a number of files into the output directory including

- An `SPM.mat` file.

- Images `Cbeta_k.nii` where $k$ indexes the $k$th estimated regression
  coefficient. These filenames are prefixed with a "`C`"' indicating
  that these are the mean values of the "Conditional" or "Posterior"
  density.

- Images of error bars/standard deviations on the regression
  coefficients `SDbeta_k.nii`.

- An image of the standard deviation of the error `Sess1_SDerror.nii`.

- An image `mask.nii` indicating which voxels were included in the
  analysis.

- Images `Sess1_AR_p.nii` where $p$ indexes the $p$th AR coefficient.
  See eg. Figure <a href="#face_ar1" data-reference-type="ref"
  data-reference="face_ar1">1.19</a>.

- Images `con_i.nii` and `con_sd_i.nii` which are the mean and standard
  deviation of the $i$th pre-defined contrast.

### Inference

After estimation, we can make a posterior inference using a PPM.
Basically, we identify regions in which we have a high probability
(level of confidence) that the response exceeds a particular size (eg, %
signal change). This is quite different from the classical inferences
above, where we look for low probabilities of the null hypothesis that
the size of the response is zero.

To determine a particular response size ("size threshold") in units of
PEAK % signal change, we first need to do a bit of calculation
concerning the scaling of the parameter estimates. The parameter
estimates themselves have arbitrary scaling, since they depend on the
scaling of the regressors. The scaling of the regressors in the present
examples depends on the scaling of the basis functions. To determine
this scaling, load the "SPM.mat" file and type in MATLAB
`sf = max(SPM.xBF.bf(:,1))/SPM.xBF.dt` (alternatively, press
"Design:Explore:Session 1" and select any of the conditions, then read
off the peak height of the canonical HRF basis function (bottom left)).

Then, if you want a size threshold of 1% peak signal change, the value
you need to enter for the PPM threshold (ie the number in the units of
the parameter estimates) is `1/sf` (which should be 4.75 in the present
case).[^10]

Finally, if we want to ask where is there a signal greater than 1% (with
a certain confidence) to faces versus baseline, we need to create a new
contrast that takes the AVERAGE of the parameter estimates for the
canonical HRF across the four conditions (N1 to F2), rather than the
default `Positive effect of condition_1` contrast, which actually
calculates the SUM of the parameter estimates for the canonical HRF
across conditions (the average vs sum makes no difference for the
classical statistics).

- Press "Results".

- Select the `SPM.mat` file created in the last section.

- Press "Define new contrast", enter the name "AVERAGE Canonical HRF:
  Faces $>$ Baseline", press the "T-contrast" radio button, enter the
  contrast \[1 0 0 1 0 0 1 0 0 1 0 0\]/4, press "submit", "OK" and
  "Done".

- *Apply masking ? \[None/Contrast/Image\]*

- Specify None

- *Effect size threshold for PPM*

- Enter the value

- *Log Odds Threshold for PPM*

- Enter the value 10

- *Extent threshold \[0\]*

- Accept the default value

SPM will then plot a map of effect sizes at voxels where it is 95% sure
that the effect size is greater than 1% of the global mean. Then use
overlays, sections, select the normalised structural image created
earlier and move the cursor to the activation in the left hemisphere.
This should create the plot shown in
Figure <a href="#face_bayes" data-reference-type="ref"
data-reference="face_bayes">1.20</a>.

<figure id="face_bayes">
<div class="center">
<img src="../../../assets/figures/manual/faces/face_bayes.png" style="width:100mm" />
</div>
<figcaption><em>Bayesian analysis: MIP and overlay of effect sizes at
voxels where PPM is 95% sure that the effect size is greater than 1% of
the global mean. The cursor is at the location <span
class="math inline"><em>x</em> = 30, <em>y</em> =  − 82, <em>z</em> =  − 17</span>mm<span
id="face_bayes" label="face_bayes"></span></em> </figcaption>
</figure>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-rnah_face_rep" class="csl-entry">

Henson, R. N. A., T. Shallice, M. L. Gorno-Tempini, and R. Dolan. 2002.
"Face Repetition Effects in Implicit and Explicit Memory Tests as
Measured by fMRI." *Cerebral Cortex* 12: 178--86.

</div>

</div>

[^1]: Face Repetition dataset:
    <http://www.fil.ion.ucl.ac.uk/spm/data/face_rep/>

[^2]: Segmentation can sometimes fail if the source (structural) image
    is not close in orientation to the MNI templates. It is generally
    advisable to manually orient the structural to match the template
    (ie MNI space) as close as possible by using the "Display" button,
    adjusting x/y/z/pitch/roll/yaw, and then pressing the "Reorient"
    button.

[^3]: This step is not strictly necessary. It will write images out at a
    resolution closer to that at which they were acquired. This will
    speed up subsequent analysis and is necessary, for example, to make
    Bayesian fMRI analysis computationally efficient.

[^4]: Beginners may wish to skip this step, and instead just superimpose
    functional activations on an "canonical structural image".

[^5]: The smoothing step is unnecessary if you are only interested in
    Bayesian analysis of your functional data.

[^6]: Unlike previous analyses of these data in SPM99 and SPM2, we will
    not bother with extra event-types for the (rare) error trials.

[^7]: It is also possible to enter information about all of the
    conditions in one go. This requires much less button pressing and
    can be implemented by highlighting the "Multiple conditions" option
    and then selecting the `all-conditions.mat` file, which is also
    provided on the webpage.

[^8]: It is also possible to enter regressors one by one by highlighting
    "Regressors" and selecting "New Regressor" for each one. Here, we
    benefit from the fact that the realignment stage produced a text
    file with the correct number of rows (351) and columns (6) for SPM
    to add 6 regressors to model (linear) rigid-body movement effects.

[^9]: The order of naming these factors is important - the factor to be
    specified first is the one that "changes slowest" ie. as we go
    through the list of conditions N1, N2, F1, F2 the factor
    "repetition" changes every condition and the factor "fame" changes
    every other condition. So "Fam" changes slowest and is entered
    first.

[^10]: Strictly speaking, this is the peak height of the canonical
    component of the best fitting BOLD impulse response: the peak of the
    complete fit would need to take into account all three basis
    functions and their parameter estimates.
