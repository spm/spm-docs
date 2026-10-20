# Auditory fMRI data <span id="Chap:data:auditory" label="Chap:data:auditory"></span>

This experiment was conducted by Geraint Rees under the direction of
Karl Friston and the FIL methods group. The purpose was to explore
equipment and techniques in the early days of our fMRI experience. As
such, it has not been formally written up, and is freely available for
personal education and evaluation purposes.

This data set was the first ever collected and analysed in the
Functional Imaging Laboratory (FIL) and is known locally as the mother
of all experiments (MoAE).

This data set comprises whole brain BOLD/EPI images acquired on a
modified 2T Siemens MAGNETOM Vision system. Each acquisition consisted
of 64 contiguous slices (64$\times$<!-- -->64$\times$<!-- -->64
3$\times$<!-- -->3$\times$<!-- -->3 mm$^3$ voxels). Acquisition took
6.05s, with the scan to scan repeat time (TR) set arbitrarily to 7s.

96 acquisitions were made (TR=7s) from a single subject, in blocks of 6,
giving 16 42s blocks. The condition for successive blocks alternated
between rest and auditory stimulation, starting with rest. Auditory
stimulation was bi-syllabic words presented binaurally at a rate of 60
per minute. The functional data starts at acquisition 4, image
`fM00223_004.{hdr,img}`, and are stored in folder `fM00223`. Due to T1
effects it is advisable to discard the first few scans (there were no
"dummy" lead-in scans). A structural image was also acquired:
`sM00223_002.{hdr,img}`, stored in folder `sM00223`. These images are
stored in Analyze format (now superseded by the NIfTI format, but SPM
reads natively both formats and always saves images as NIfTI) and are
available from the SPM site [^1].

To analyse the data, first create a new directory `DIR`, eg.
`C:`$\backslash$`data`$\backslash$`auditory`, in which to place the
results of your analysis. Then create 3 subdirectories (i) `dummy`, (ii)
`jobs` and (iii) `classical`. As the analysis proceeds these directories
will be filled with dummy scans, job-specification files, design
matrices and models estimated using classical inference.

Start up MATLAB, enter your `jobs` directory and type `spm fmri` at the
MATLAB prompt. SPM will then open in fMRI mode with three windows (see
Figure <a href="#aud_command" data-reference-type="ref"
data-reference="aud_command">1.1</a>): (1) the top-left or "Menu"
window, (2) the bottom-left or "Interactive" window and (3) the
right-hand or "Graphics" window. Analysis then takes place in three
major stages (i) spatial pre-processing, (ii) model specification,
review and estimation and (iii) inference. These stages organise the
buttons in SPM's Menu window.

<figure id="aud_command">
<div class="center">
<img src="../../../assets/figures/manual/auditory/command.png" style="width:100mm" />
</div>
<figcaption><em>The SPM base window comprises three sections i) spatial
pre-processing, (ii) model specification, review and estimation and
(iii) inference. <span id="aud_command"
label="aud_command"></span></em></figcaption>
</figure>

## Preamble (dummy scans)

To avoid T1 effects in the initial scans of an fMRI time series we
recommend discarding the first few scans. To make this example simple,
we'll discard the first complete cycle (12 scans, `04-15`), leaving 84
scans, image files `16-99`. This is best done by moving these files to a
different directory, `dummy`, that we created earlier.

## Spatial pre-processing

### Realignment

Under the spatial pre-processing section of the SPM Menu window select
<span class="smallcaps">Realign (Est & Res)</span> from the
<span class="smallcaps">Realign</span> pulldown menu. This will call up
a realignment job specification in the batch editor. Then

- Highlight "Data", select "New Session", then highlight the newly
  created "Session" option.

- Press "Select Files" and use the SPM file selector to choose all of
  the functional images eg. ("`fM000*.img`"). There should be 84 files.

- Press "Resliced images" in the "Reslice Options" and select "Mean
  Image Only".

- Save the job file as eg.
  `DIR`$\backslash$`jobs`$\backslash$`realign.mat`.

- Press the `RUN` button in the batch editor (green arrow).

This will run the realign job which will estimate the 6 parameter (rigid
body) spatial transformation that will align the times series of images
and will modify the header of the input images (`*.hdr`), such that they
reflect the relative orientation of the data after correction for
movement artefacts. SPM will then plot the estimated time series of
translations and rotations shown in
Figure <a href="#aud_realign" data-reference-type="ref"
data-reference="aud_realign">1.2</a>. These data are also saved to a
file eg. `rp_fM00223_016.txt`, so that these variables can be later used
as regressors when fitting GLMs. This allows movements effects to be
discounted when looking for brain activations.

SPM will also create a mean image eg. `meanfM00223_016.img` which will
be used in the next step of spatial processing - coregistration.

<figure id="aud_realign">
<div class="center">
<img src="../../../assets/figures/manual/auditory/realign.png" style="width:125mm" />
</div>
<figcaption><em>Realignment of Auditory data.<span id="aud_realign"
label="aud_realign"></span></em></figcaption>
</figure>

### Coregistration

Select <span class="smallcaps">Coregister (Estimate)</span> from the
<span class="smallcaps">Coregister</span> pulldown. This will call up
the specification of a coregistration job in the batch editor.

- Highlight "Reference Image" and then select the mean fMRI scan from
  realignment eg. `meanfM00223_016.img`.

- Highlight "Source Image" and then select the structural image eg.
  `sM00223_002.img`.

- Press the Save button and save the job as
  `DIR`$\backslash$`jobs`$\backslash$`coregister.mat`.

- Then press the `RUN` button.

SPM will then implement a coregistration between the structural and
functional data that maximises the mutual information. The image in
figure <a href="#aud_coreg" data-reference-type="ref"
data-reference="aud_coreg">1.3</a> should then appear in the Graphics
window. SPM will have changed the header of the source file which in
this case is the structural image `sM00223_002.hdr`.

<figure id="aud_coreg">
<div class="center">
<img src="../../../assets/figures/manual/auditory/coreg.png" style="width:125mm" />
</div>
<figcaption><em>Mutual Information Coregistration of Auditory data.<span
id="aud_coreg" label="aud_coreg"></span></em></figcaption>
</figure>

The <span class="smallcaps">Check Reg</span> facility is useful here, to
check the results of coregistration. Press the
<span class="smallcaps">Check Reg</span> button in the lower section of
the Menu window and then select the "Reference" and "Source" Images
specified above ie `meanfM00223_016.img` and `sM00223_002.img`. SPM will
then produce an image like that shown in
Figure <a href="#aud_checkreg" data-reference-type="ref"
data-reference="aud_checkreg">1.4</a> in the Graphics window. You can
then use your mouse to navigate these images to confirm that there is an
anatomical correspondence.

<figure id="aud_checkreg">
<div class="center">
<img src="../../../assets/figures/manual/auditory/checkreg.png" style="width:125mm" />
</div>
<figcaption><em>Checking registration of functional and "registered"
structural data. <span id="aud_checkreg"
label="aud_checkreg"></span></em></figcaption>
</figure>

### Segmentation

Press the <span class="smallcaps">Segment</span> button. This will call
up the specification of a segmentation job in the batch editor.
Highlight the "Volumes" field and then select the subject's registered
anatomical image eg. `sM00223_002.img`. Highlight "Save INU corrected"
and select "Save INU corrected". Highlight "Deformation Fields" t͡he
bottom of the list and select "Forward". Save the job file as
`segment.mat` and then press `RUN`. SPM will segment the structural
image using the default tissue probability maps as priors (Ashburner and
Friston 2005).

SPM will create gray and white matter images and an intensity nonuniformity corrected
structural image. These can be viewed using the
<span class="smallcaps">CheckReg</span> facility as described in the
previous section. Figure <a href="#aud_gray" data-reference-type="ref"
data-reference="aud_gray">1.5</a> shows the gray matter image,
`c1sM0023_002.nii` along with the original structural.
Figure <a href="#aud_bias" data-reference-type="ref"
data-reference="aud_bias">1.6</a> shows the structural and
intensity nonuniformity corrected image, `msM0023_002.nii`.

<figure id="aud_gray">
<div class="center">
<img src="../../../assets/figures/manual/auditory/gray.png" style="width:125mm" />
</div>
<figcaption><em>Gray matter image and "registered" structural
image.<span id="aud_gray" label="aud_gray"></span></em></figcaption>
</figure>

<figure id="aud_bias">
<div class="center">
<img src="../../../assets/figures/manual/auditory/bias.png" style="width:125mm" />
</div>
<figcaption><em>Structural image (top) and intensity nonuniformity (INU) corrected structural
image (bottom). Notice that the original structural is darker at the top
than at the bottom. This non-uniformity has been removed in the
INU corrected image.<span id="aud_bias"
label="aud_bias"></span></em></figcaption>
</figure>

SPM will also write a deformation field, file `y_sM00223_002.nii` in the
original structural directory. It contains 3 volumes to encode the x, y
and z coordinates. Given that the structural and functional data are in
alignment, this can be used to spatially normalise the functional data.

### Normalise

Select <span class="smallcaps">Normalise (Write)</span> from the
<span class="smallcaps">Normalise</span> pulldown menu. This will call
up the specification of a normalise job in the batch editor.

- Highlight "Data", select New "Subject",

- Highlight "Deformation Field" and select the `y_sM00223_002.nii` file
  that you created in the previous section,

- Highlight "Images to Write" and select all of the realigned functional
  images `fM000*.img`. You can right click over the listed files, choose
  "Select all" and press "Done".

- In the "Writing Options", change "Voxel sizes" from \[2 2 2\] to \[3 3
  3\]. This step is not strictly necessary: it will write images out at
  a resolution closer to that at which they were acquired.

- Press "Save", save the job as `normalise_functional.mat` and then
  press the `RUN` button.

SPM will then write spatially normalised files to the functional data
directory. These files have the prefix `w`.

If you wish to superimpose a subject's functional activations on their
own anatomy[^2] you will also need to apply the spatial normalisation
parameters to their (intensity nonuniformity corrected) anatomical image. To do this

- Select <span class="smallcaps">Normalise (Write)</span>, highlight
  "Data", select "New Subject".

- Highlight "Deformation Field", select the `y_sM00223_002.nii` file
  that you created in the previous section, press "Done".

- Highlight "Images to Write", select the intensity nonuniformity corrected structural eg.
  `msM00223_002.nii`, press "Done".

- Open "Writing Options", select voxel sizes and change the default \[2
  2 2\] to \[1 1 3\] which corresponds to the original resolution of the
  images.

- Save the job as `normalise_structural.mat` and press the `RUN` button.

### Smoothing

Press the <span class="smallcaps">Smooth</span> button. This will call
up the specification of a smooth job in the batch editor.

- Select "Images to Smooth" and then select the spatially normalised
  files created in the last section eg. `wf*.img`. This can be done
  efficiently by changing the filter in the SPM file selector to
  `^wf.*`. SPM will then only list those files beginning with letters
  `wf` ie. those that have been spatially normalised.

- Highlight "FWHM" and change \[8 8 8\] to \[6 6 6\]. This will smooth
  the data by 6mm in each direction.

- Save the job as `smooth.mat` and press the `Run` button.

An example of functional image and its smoothed version is displayed on
Figure <a href="#aud_smooth" data-reference-type="ref"
data-reference="aud_smooth">1.7</a>.

<figure id="aud_smooth">
<div class="center">
<img src="../../../assets/figures/manual/auditory/smooth.png" style="width:125mm" />
</div>
<figcaption><em>Functional image (top) and 6mm-smoothed functional image
(bottom). These images were obtained using SPM’s "CheckReg" facility.
<span id="aud_smooth" label="aud_smooth"></span></em></figcaption>
</figure>

## Model specification, review and estimation

Press the "Specify 1st-level" button. This will call up the
specification of an fMRI specification job in the batch editor. Then

- Open the "Timing parameters" option.

- Highlight "Units for design" and select "Scans".

- Highlight "Interscan interval" and enter 7. That's the TR in seconds.

- Highlight "Data and Design" and select "New Subject/Session". Then
  open the newly created "Subject/Session" option.

- Highlight "Scans" and use SPM's file selector to choose the 84
  smoothed, normalised functional images ie `swfM00223_016.img` to
  `swfM00223_099.img`. These can be selected easily using the `^sw.*’`
  filter, and select all. Then press "Done".

- Highlight "Condition" and select "New condition".

- Open the newly created "Condition" option. Highlight "Name" and enter
  "listening". Highlight "Onsets" and enter "6:12:84". Highlight
  "Durations" and enter "6".

- Highlight "Directory" and select the `DIR/classical` directory you
  created earlier.

- Save the job as `specify.mat` and press the `Run` button.

SPM will then write an `SPM.mat` file to the `DIR/classical` directory.
It will also plot the design matrix, as shown in
Figure <a href="#aud_design" data-reference-type="ref"
data-reference="aud_design">1.8</a>.

<figure id="aud_design">
<div class="center">
<img src="../../../assets/figures/manual/auditory/design.png" style="width:125mm" />
</div>
<figcaption><em><code>Design matrix</code>: The filenames on the
right-hand side of the design matrix indicate the scan associated with
each row.<span id="aud_design"
label="aud_design"></span></em></figcaption>
</figure>

At this stage it is advisable to check your model specification using
SPM's review facility which is accessed via the "Review" button. This
brings up a "design" tab on the interactive window clicking on which
produces a pulldown menu. If you select the first item "Design Matrix"
SPM will produce the image shown in
Figure <a href="#aud_design" data-reference-type="ref"
data-reference="aud_design">1.8</a>. If you select "Explore" then
"Session 1" then "listening", SPM will produce the plots shown in
Figure <a href="#aud_explore" data-reference-type="ref"
data-reference="aud_explore">1.9</a>.

<figure id="aud_explore">
<div class="center">
<img src="../../../assets/figures/manual/auditory/explore.png" style="width:125mm" />
</div>
<figcaption><em><code>Exploring the design matrix in Figure </code><a
href="#aud_design" data-reference-type="ref"
data-reference="aud_design">1.8</a>: This shows the time series of the
"listening" regressor (top left), a frequency domain plot of the
"listening" regressor (top right) and the basis function used to convert
assumed neuronal activity into hemodynamic activity. In this model we
used the default option - the canonical basis function. The frequency
domain plot shows that the frequency content of the "listening"
regressor is above the set frequencies that are removed by the High Pass
Filter (HPF) (these are shown in gray - in this model we accepted the
default HPF cut-off of 128s or 0.008Hz). <span id="aud_explore"
label="aud_explore"></span></em></figcaption>
</figure>

If you select the second item on the "Design" tab, "Design
Orthogonality", SPM will produce the plot shown in
Figure <a href="#aud_orth" data-reference-type="ref"
data-reference="aud_orth">1.10</a>. Columns $x_1$ and $x_2$ are
orthogonal if the inner product $x_1^T x_2=0$. The inner product can
also be written $x_1^T x_2 = |x_1||x_2| cos \theta$ where $|x|$ denotes
the length of $x$ and $\theta$ is the angle between the two vectors. So,
the vectors will be orthogonal if $cos \theta=0$. The upper-diagonal
elements in the matrix at the bottom of
figure <a href="#aud_orth" data-reference-type="ref"
data-reference="aud_orth">1.10</a> plot $cos\theta$ for each pair of
columns in the design matrix. Here we have a single entry. A degree of
non-orthogonality or collinearity is indicated by the gray shading.

<figure id="aud_orth">
<div class="center">
<img src="../../../assets/figures/manual/auditory/aud_orth.png" style="width:80mm" />
</div>
<figcaption><em><code>Design Orthogonality</code>: The description above
the first column in the design matrix <span>Sn(1)Listening*bf(1)</span>
means that this column refers to the first session of data (in this
analysis there is only 1 session), the name of this condition/trial is
‘listening’ and the trial information has been convolved with the first
basis function (the canonical hemodynamic response). The constant
regressor for session 1 is referred to as <span>Sn(1)Constant</span>.
The orthogonality matrix at the bottom indicates a degree of
collinearity between regressors. <span id="aud_orth"
label="aud_orth"></span></em></figcaption>
</figure>

### Estimate

Press the <span class="smallcaps">Estimate</span> button. This will call
up the specification of an fMRI estimation job in the batch editor. Then

- Highlight the "Select SPM.mat" option and then choose the `SPM.mat`
  file saved in the `classical` subdirectory.

- Save the job as `estimate.mat` and press the `Run` button.

SPM will write a number of files into the selected directory including
an `SPM.mat` file.

## Inference

After estimation:

- Press "Results".

- Select the `SPM.mat` file created in the last section.

<figure>
<div class="center">
<img src="../../../assets/figures/manual/auditory/con_man.png" style="width:75mm" />
</div>
<figcaption><em>The contrast manager</em></figcaption>
</figure>

This will invoke the contrast manager.

### Contrast manager

The contrast manager displays the design matrix (surfable) in the right
panel and lists specified contrasts in the left panel. Either
"t-contrast" or "F-contrast" can be selected. To examine statistical
results for condition effects

- Select "Define new contrast"

<figure>
<p><img src="../../../assets/figures/manual/auditory/con_man2.png" style="width:75mm" alt="image" />
<img src="../../../assets/figures/manual/auditory/con_man3.png" style="width:75mm" alt="image" /></p>
<figcaption><em>Left: A contrast is entered by specifying the numeric
values in the lower window and the name in the upper window. Right:
After contrasts have been specified they can be
selected.</em></figcaption>
</figure>

One sided main effects for the listening condition (i.e., a one-sided
t-test) can be specified (in this example) as "1" (listening $>$ rest)
and "-1" (rest $>$ listening). SPM will accept estimable contrasts only.
Accepted contrasts are displayed at the bottom of the contrast manager
window in green, incorrect ones are displayed in red. To view a contrast

- Select the contrast name e.g., "listening $>$ rest".

- Press "Done".

### Masking

You will then be prompted with

- *Apply masking ? \[none/contrast/image\]*.

- "Specify none".

Masking implies selecting voxels specified by other contrasts. If "yes",
SPM will prompt for (one or more) masking contrasts, the significance
level of the mask (default *p* = 0.05 uncorrected), and will ask whether
an inclusive or exclusive mask should be used. Exclusive will remove all
voxels which reach the default level of significance in the masking
contrast, inclusive will remove all voxels which do not reach the
default level of significance in the masking contrast. Masking does not
affect *p*-values of the "target" contrast, it only includes or excludes
voxels.

### Thresholds

You will then be prompted with

- *p value adjustment to control: \[FWE/none\]*.

  - Select "FWE".

- *p value(family-wise error)*.

  - Accept the default value, 0.05.

A Family Wise Error (FWE) is a false positive anywhere in the SPM. Now,
imagine repeating your experiment many times and producing SPMs. The
proportion of SPMs containing FWEs is the FWE rate. A value of 0.05
implies that on average 1 in 20 SPMs contains one or more false
positives somewhere in the image.

If you choose the "none" option above this corresponds to making
statistical inferences at the "voxel level". These use "uncorrected" *p*
values, whereas FWE thresholds are said to use "corrected" *p*-values.
SPM's default uncorrected *p*-value is *p*=0.001. This means that the
probability of a false positive at each voxel is 0.001. So if, you have
50,000 voxels you can expect $50,000 \times 0.001 = 50$ false positives
in each SPM.

You will then be prompted with

- *Extent Threshold {voxels} \[0\]*.

  - Accept the default value, "0".

Entering a value $k$ here will produce SPMs with clusters containing at
least $k$ voxels. SPM will then produce the SPM shown in
Figure <a href="#aud_spm1" data-reference-type="ref"
data-reference="aud_spm1">1.11</a>.

<figure id="aud_spm1">
<div class="center">
<img src="../../../assets/figures/manual/auditory/spm1.png" style="width:100mm" />
</div>
<figcaption><em>SPM showing bilateral activation of auditory cortex.
<span id="aud_spm1" label="aud_spm1"></span></em></figcaption>
</figure>

### Files

A number of files are written to the working directory at this time.
Images containing weighted parameter estimates are saved as
`con_0001.nii`, `con_0002.nii`, etc. in the working directory. Images of
T-statistics are saved as `spmT_0001.nii`, `spmT_0002.nii` etc., also in
the working directory.

### Maximum Intensity Projections

SPM displays a Maximum Intensity Projection (MIP) of the statistical map
in the Graphics window. The MIP is projected on a glass brain in three
orthogonal planes. The MIP is surfable: right-clicking in the MIP will
activate a pulldown menu, left-clicking on the red cursor will allow it
to be dragged to a new position.

<figure>
<div class="center">
<img src="../../../assets/figures/manual/auditory/interactive.png" style="width:100mm" />
</div>
<figcaption><em>SPM’s Interactive window during results assessment. The
"<em>p</em>-values" section is used to produce tables of statistical
information. The visualisation section is used to plot responses at a
voxel or to visual activations overlaid on anatomical images. The
"Multivariate" section, ie. the "eigenvariate" button, is used to
extract data for subsequent analyses such as assessment of
PsychoPhysiological Interactions (PPIs) or Dynamic Causal Models
(DCMs).</em></figcaption>
</figure>

### Design matrix

SPM also displays the design matrix with the selected contrast. The
design matrix is also surfable: right-clicking will show parameter
names, left-clicking will show design matrix values for each scan.

In the SPM Interactive window (lower left panel) a button box appears
with various options for displaying statistical results (p-values panel)
and creating plots/overlays (visualisation panel). Clicking "Design"
(upper left) will activate a pulldown menu as in the "Explore design"
option.

### Statistical tables

To get a summary of local maxima, press the "whole brain" button in the
*p*-values section of the Interactive window. This will list all
clusters above the chosen level of significance as well as separate
($>$<!-- -->8mm apart) maxima within a cluster, with details of
significance thresholds and search volume underneath, as shown in
Figure <a href="#aud_volume" data-reference-type="ref"
data-reference="aud_volume">1.12</a>

<figure id="aud_volume">
<div class="center">
<img src="../../../assets/figures/manual/auditory/volume.png" style="width:100mm" />
</div>
<figcaption><em>Volume table for "listening <span
class="math inline">&gt;</span> rest" effect. This table of values was
created by pressing the SPM Figure <span class="math inline">&gt;</span>
Results Table option at the top of the Graphics window and then pressing
the "whole brain" button. This displays the table of results in a
separate window. <span id="aud_volume"
label="aud_volume"></span></em></figcaption>
</figure>

The columns in volume table show, from right to left:

- **x, y, z (mm)**: coordinates in MNI space for each maximum.

- **peak-level**: the chance (*p*) of finding (under the null
  hypothesis) a peak with this or a greater height (T- or Z-statistic),
  corrected (FWE or FDR)/ uncorrected for search volume.

- **cluster-level**: the chance (*p*) of finding a cluster with this
  many (*k*) or a greater number of voxels, corrected (FWE or FDR)/
  uncorrected for search volume.

- **set-level**: the chance (*p*) of finding this (*c*) or a greater
  number of clusters in the search volume.

It is also worth noting that:

- The table is surfable: clicking a row of cluster coordinates will move
  the pointer in the MIP to that cluster, clicking other numbers will
  display the exact value in the MATLAB window (e.g. 0.000 =
  6.1971e-07).

- To inspect a specific cluster (e.g., in this example data set, the
  right auditory cortex), either move the cursor in the MIP (by
  left-clicking and dragging the cursor, or right-clicking the MIP
  background which will activate a pulldown menu).

- Alternatively, click the cluster coordinates in the volume table, or
  type the coordinates in the coordinates section of the Interactive
  window.

It is also possible to produce tables of statistical information for a
single cluster of interest rather than for the whole volume. Firstly,
select the relevant cluster in the MIP and then press the "current
cluster" button in the *p*-values section of the Interactive window.
This will show coordinates and voxel-level statistics for local maxima
($>$<!-- -->4mm apart) in the selected cluster. This table is also
surfable.

### Plotting responses at a voxel

A voxel can be chosen with coordinates corresponding to those in the
Interactive window. The responses at this voxel can then be plotted
using the "Plot" button in the visualisation section of the Interactive
window. This will provide you with five further options:

1.  Contrast estimates and 90% CI: SPM will prompt for a specific
    contrast (e.g., listening$>$rest). The plot will show effect size
    and 90% confidence intervals. See eg.
    Figure <a href="#aud_contrast" data-reference-type="ref"
    data-reference="aud_contrast">1.13</a>.

2.  Fitted responses: Plots adjusted data and fitted response across
    session/subject. SPM will prompt for a specific contrast and
    provides the option to choose different ordinates ("an explanatory
    variable", "scan or time", or "user specified"). If "scan or time",
    the plot will show adjusted or fitted data with errors added as
    shown in Figure <a href="#aud_fitted" data-reference-type="ref"
    data-reference="aud_fitted">1.14</a>.

3.  Event-related responses: Plots adjusted data and fitted response
    across peri-stimulus time.

4.  Parametric responses.

5.  Volterra kernels.

<figure id="aud_contrast">
<div class="center">
<img src="../../../assets/figures/manual/auditory/contrast.png" style="width:60mm" />
</div>
<figcaption><em>Estimated effect size. <span id="aud_contrast"
label="aud_contrast"></span></em></figcaption>
</figure>

<figure id="aud_fitted">
<div class="center">
<img src="../../../assets/figures/manual/auditory/fitted.png" style="width:60mm" />
</div>
<figcaption><em>Fitted responses. <span id="aud_fitted"
label="aud_fitted"></span></em></figcaption>
</figure>

For plotting event-related responses SPM provides three options

1.  Fitted response and PSTH (peri-stimulus time histogram): plots mean
    regressor(s) (ie. averaged over session) and mean signal +/- SE for
    each peri-stimulus time bin.

2.  Fitted response and 90% CI: plots mean regressor(s) along with a 90%
    confidence interval.

3.  Fitted response and adjusted data: plots regressor(s) and individual
    data (note that in this example the data are shown in columns due to
    the fixed TR/ISI relationship).

Its worth noting that

- The values for the fitted response across session/subject for the
  selected plot can be displayed and accessed in the window by typing
  "Y". Typing "y" will display the adjusted data.

- "Adjusted" data = adjusted for confounds (e.g., global flow) and high-
  and low pass filtering.

### Overlays

The visualisation section of the Interactive window also provides an
overlay facility for anatomical visualisation of clusters of activation.
Pressing "Overlays" will activate a pulldown menu with several options
including:

1.  **Slices**: overlay on three adjacent (2mm) transaxial slices. SPM
    will prompt for an image for rendering. This could be a canonical
    image (see `spm_templates.man`) or an individual T1/mean EPI image
    for single-subject analyses. Beware that the left-right convention
    in the display of that option will depend on how your data are
    actually stored on disk.

2.  **Sections**: overlay on three intersecting (sagittal, coronal,
    axial) slices. These renderings are surfable: clicking the images
    will move the crosshair.

3.  **Render**: overlay on a volume rendered brain.

Thresholded SPMs can be saved as NIfTI image files in the working
directory by using the "Save" button in the Interactive window. In
Figures <a href="#aud_slices" data-reference-type="ref"
data-reference="aud_slices">1.15</a>,
<a href="#aud_sections" data-reference-type="ref"
data-reference="aud_sections">1.16</a> and
<a href="#aud_render" data-reference-type="ref"
data-reference="aud_render">1.17</a> the 'listening $>$ rest' activation
has been superimposed on the spatially normalised, intensity nonuniformity corrected
anatomical image `wmsM00223_002.nii` created earlier.

<figure id="aud_slices">
<div class="center">
<img src="../../../assets/figures/manual/auditory/slices.png" style="width:100mm" />
</div>
<figcaption><em>Slices.</em> <span id="aud_slices"
label="aud_slices"></span> </figcaption>
</figure>

<figure id="aud_sections">
<div class="center">
<img src="../../../assets/figures/manual/auditory/sections.png" style="width:100mm" />
</div>
<figcaption><em>Sections.</em> <span id="aud_sections"
label="aud_sections"></span> </figcaption>
</figure>

For the "Render" option we first created a rendering for this subject.
This was implemented by

- "Normalise (Write)" the two images `c1sM00223_002.nii` and
  `c2sM00223_002.nii` using the "Deformation Field" `y_sM00223_002.nii`
  and a voxel size of \[1 1 1\].

- Selecting "Extract Surface" from the "Render" pulldown menu.

- Selecting the gray and white matter images `wc1sM00223_002.nii` and
  `wc2sM00223_002.nii` created in the first step.

- Saving the results using the default options (Rendering and Surface).

SPM plots the rendered anatomical image in the graphics window and saves
it as `render_wc1sM00223_002.mat`. The surface image is saved as
`surf_wc1sM00223_002.mat`.

<figure id="aud_render">
<div class="center">
<img src="../../../assets/figures/manual/auditory/render.png" style="width:100mm" />
</div>
<figcaption><em>Render.</em> <span id="aud_render"
label="aud_render"></span> </figcaption>
</figure>

It is also possible to project and display the results on a surface
mesh, we are going to use here one of the canonical mesh distributed
with SPM (in MNI space). Press "Overlays" and choose "Render", then go
in the `canonical` folder of your SPM installation and select file
`cortex_20484.surf.gii` (this is a surface mesh stored using the GIfTI
format) and you will obtain a figure similar to
<a href="#aud_render2" data-reference-type="ref"
data-reference="aud_render2">1.18</a>.

<figure id="aud_render2">
<div class="center">

</div>
<figcaption><em>3D Rendering using canonical mesh.</em> <span
id="aud_render2" label="aud_render2"></span></figcaption>
</figure>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-ashburner05" class="csl-entry">

Ashburner, J., and K. J. Friston. 2005. "Unified Segmentation."
*NeuroImage* 26: 839--51.
<https://doi.org/doi:10.1016/j.neuroimage.2005.02.018>.

</div>

</div>

[^1]: Auditory fMRI dataset:
    <http://www.fil.ion.ucl.ac.uk/spm/data/auditory/>

[^2]: Beginners may wish to skip this step, and instead just superimpose
    functional activations on an "average structural image".
