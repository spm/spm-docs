# Bayesian analysis

## Specification

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

## Estimation

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

## Inference

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

--8<-- "addons/abbreviations.md"