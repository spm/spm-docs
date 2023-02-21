# Dynamic Causal Modelling for resting state fMRI <span id="Chap:DCM_rsfmri" label="Chap:DCM_rsfmri"></span>

This chapter provides an extension to the framework of Dynamic Causal
Modelling (DCM) for modelling intrinsic dynamics of a resting state
network (Friston et al. 2014; Razi et al. 2015). This DCM estimates the
effective connectivity among coupled populations of neurons, which
subtends the observed functional connectivity in the frequency domain.
We refer to this as spectral DCM (spDCM).

## Theoretical background

Spectral DCM uses a neuronally plausible power-law model of the coupled
dynamics of neuronal populations to generate complex cross spectra among
measured responses. Spectral DCM is distinct from stochastic DCM (sDCM)
(Li et al. 2011) as it eschews the estimation of random fluctuations in
(hidden) neural states; rendering spectral DCM essentially deterministic
in nature. These models are similar to conventional deterministic DCM
for fMRI (Friston, Harrison, and Penny 2003) but model endogenous
activity that would reproduce the functional connectivity (correlations)
observed in resting state fMRI. DCMs for resting state data are also
slightly simpler; given that most resting state designs compare groups
of subjects (e.g. patient cohorts vs. controls), spDCMs do not usually
require the bilinear term (accounting for condition-specific effects on
effective connection strengths). In other words, spectral DCM is
intended to simply compare endogenous coupling between groups of
subjects (e.g. patients vs. healthy controls).

In modelling resting state activity, it is necessary to augment the
ordinary differential equations used in standard DCM, with a stochastic
term to model endogenous neuronal fluctuations. This renders the
equations of the motion stochastic. The stochastic generative model for
the resting state fMRI time series, like any other DCM, comprises of two
equations: the Langevin form of evolution equation (motion) is written
as:

$$\label{spdcm_eq1}
\dot{z}= f(z,u,\theta)+ v$$ and the observation equation, which is a
static nonlinear mapping from the hidden physiological states in
Eq. <a href="#spdcm_eq1" data-reference-type="ref"
data-reference="spdcm_eq1">[spdcm_eq1]</a> to the observed BOLD activity
and is written as: $$\label{spdcm_eq2}
y= h(z,u,\phi)+ e$$ where $\dot{z}$ is the rate in change of the
neuronal states $z$, $\theta$ are unknown parameters (i.e. the effective
connectivity) and $v$ (resp. $e$) is the stochastic process -- called
the state noise (resp. the measurement or observation noise) --
modelling the random neuronal fluctuations that drive the resting state
activity. In the observation equations, $\phi$ are the unknown
parameters of the (haemodynamic) observation function and $u$ represents
any exogenous (or experimental) inputs -- that are usually absent in
resting state designs. For resting state activity,
Eq. <a href="#spdcm_eq1" data-reference-type="ref"
data-reference="spdcm_eq1">[spdcm_eq1]</a> takes on a very simple linear
form: $$\label{spdcm_eq3}
\dot{z}= Az + Cu + v$$ where $A$ is the Jacobian describing the
behaviour -- i.e. the effective connectivity -- of the system near its
stationary point ($f(z_{o})=0$) in the absence of the fluctuations $v$.
It is to be noted that we can still include exogenous (or experimental)
inputs, $u$ in our model. These inputs drive the hidden states -- and
are usually set to zero in resting state models. It is perfectly
possible to have external, (non-modulatory) stimuli, as in the case of
conventional functional neuroimaging studies. For example, in (Friston,
Harrison, and Penny 2003) we used an attention to visual motion paradigm
to illustrate this point.

Inverting the stochastic DCM of the form given by
Eq. <a href="#spdcm_eq3" data-reference-type="ref"
data-reference="spdcm_eq3">[spdcm_eq3]</a> in the time domain, which
includes state noise, is rather complicated because such models require
estimation of not only the model parameters (and any hyperparameters
that parameterise the random fluctuations), but also the hidden states,
which become random (probabilistic) variables. Hence the unknown
quantities to be estimated under a stochastic DCM are
$\psi=\{z,\phi,\theta,\sigma\}$, where $\sigma$ refers to any
hyperparameters (precisions or inverse covariances) defining the
neuronal fluctuations. In terms of temporal characteristics, the hidden
states are time-variant, whereas the model parameters (and
hyperparameters) are time-invariant. There are various variational
schemes in literature that can invert such models. For example, dynamic
expectation maximization (DEM) (Friston, Trujillo-Bareto, and Daunizeau
2008) and generalized filtering (GF) (Friston et al. 2010).

Although the stochastic models in
Eq. <a href="#spdcm_eq1" data-reference-type="ref"
data-reference="spdcm_eq1">[spdcm_eq1]</a> and their inversion in time
domain provide a useful means to estimate effective connectivity they
also require us to estimate hidden states. This poses a difficult
inverse problem that is computationally demanding; especially when the
number of hidden states becomes large. To finesse this problem, we
furnish a constrained inversion of the stochastic model by
parameterising the neuronal fluctuations. This parameterisation also
provides an opportunity to compare parameters encoding the neuronal
fluctuations among groups. The parameterisation of endogenous
fluctuations means that the states are no longer probabilistic; hence
the inversion scheme is significantly simpler, requiring estimation of
only the parameters (and hyperparameters) of the model.

Spectral DCM simply estimates the time-invariant parameters of their
cross spectra. In other words, while stochastic DCMs model the observed
BOLD timeseries of each node, spectral DCMs model the observed
functional connectivity between nodes. Effectively, this is achieved by
replacing the original timeseries with their second-order statistics
(i.e., cross spectra), under stationarity assumptions. This means,
instead of estimating time varying hidden states, we are estimating
their covariance, which does not change with time. This means we need to
estimate the covariance of the random fluctuations; where a scale free
(power law) form for the state noise (resp. observation noise) that can
be motivated from previous work on neuronal activity: $$\begin{aligned}
\label{spdcm_eq4}
& g_{v}(\omega,\theta) & = \alpha_{v}\omega^{-\beta_{v}}\\ \nonumber
& g_{e}(\omega,\theta) & = \alpha_{e}\omega^{-\beta_{e}}
\end{aligned}$$ Here, $\{\alpha,\beta\}\subset \theta$ are the
parameters controlling the amplitudes and exponents of the spectral
density of the neural fluctuations. This models neuronal noise with a
generic $1/f^{\gamma}$ spectra, which characterizes fluctuations in
systems that are at nonequilibrium steady-state. Using the model
parameters, $\theta \supseteq \{A,C,\alpha, \beta\}$, we can simply
generate the expected cross spectra: $$\begin{aligned}
\label{spdcm_eq5}
& y & = \kappa \ast v + e\\ \nonumber
& \kappa & = \partial_z h \mathrm{exp} (t \partial_z f ) \\ \nonumber
& g_{y}(\omega,\theta) & = |K(\omega)|^2 g_{v}(\omega,\theta) + g_{e}(\omega,\theta)
\end{aligned}$$ where $K(\omega)$ is the Fourier transform of the
system's (first order) Volterra kernels $\kappa$, which are a function
of the Jacobian or effective connectivity. The unknown quantities
$\psi=\{\phi,\theta,\sigma\}$ of this deterministic model can now be
estimated using standard Variational Laplace procedures (Friston et al.
2007). Here $g_{y} (\omega,\theta)$ represents the predicted cross
spectra that can be estimated, for example, using autoregressive (AR)
model. Specifically, we use a fourth-order autoregressive model to
ensure smooth sample cross spectra of the sort predicted by the
generative model. The frequencies usually considered for fMRI range from
1/128 Hz to 0.1 Hz in 32 evenly spaced frequency bins.

## Practical example

Data used for this example can be downloaded from the SPM website. This
dataset[^1] consists of an exemplar subject from the full dataset
available from the FC1000 project website[^2] and was used in (Razi et
al. 2015) to interrogate the information integration in default mode
network (DMN) -- a distinct brain system that is activated when an
individual engages in introspection like mindwandering or daydreaming.
The DMN comprises part of the medial prefrontal cortex (mPFC), posterior
cingulate cortex (PCC) and parts of inferior parietal lobe and superior
frontal regions.

The archive contains the smoothed, spatially normalised, realigned,
slice-time corrected images in the directory `func`. The directory
`anat` contains a spatially normalised T1 structural image and the
directory `GLM` contains the file `rp_rest0000.txt` containing six head
motion parameters. All preprocessing took place using SPM12.

### Defining the GLM

First, we need to set up the GLM analysis and extract our time series
from the results. For resting state fMRI because there is no task so
first we need to generate `SPM.mat` so that we can extract the time
series. This can be done by following the steps below.

Let's set up a batch that will specify the model and estimate it.

1.  The analysis directory you have downloaded should include:

    1.  A directory named `func`, which includes the preprocessed fMRI
        volumes.

    2.  A directory named `anat`, which includes a normalised T1
        structural volume.

    3.  A directory named `GLM`, which include file `rp_rest0000.txt`
        containing the movement regressors from the realignment step.

2.  In MATLAB type

        >> cd GLM
        >> spm fmri

3.  From the main SPM window, click on the
    <span class="smallcaps">Batch</span> button.

4.  From the SPM menu at the top of the Batch Editor, select "Stats $>$
    fMRI model specification".

5.  Click <span class="smallcaps">Directory</span> and choose the `GLM`
    directory that you made above.

6.  <span class="smallcaps">Units for design</span>
    \[<span class="smallcaps">scans</span>\]

7.  <span class="smallcaps">Interscan interval</span> \[2\]

8.  Click <span class="smallcaps">Data & Design</span>, Choose
    <span class="smallcaps">New \"Subject/Session\"</span>

9.  Click <span class="smallcaps">Scans</span> and choose all the
    functional scans `swrestxxxx.img`. There should be 175 `*.img`
    files.

10. From the SPM menu at the top of the Batch Editor, select "Stats $>$
    model estimation".

11. For <span class="smallcaps">Select SPM.mat</span>, click on the
    <span class="smallcaps">Dependency</span> button and choose the
    proposed item (the output from the previous module).

12. You should now be able to press the
    <span class="smallcaps">Run</span> green arrow at the top of the
    Batch Editor window. This will specify and estimate the GLM.

We will also need to extract signal from CSF and white matter (WM) to be
used as confound. Here is a step-by-step example for extracting the WM
(Pons) time series which we will use as one of the nuisance variable:

1.  From the main SPM window, click on the
    <span class="smallcaps">Batch</span> button.

2.  From the SPM menu at the top of the Batch Editor, select "Util $>$
    Volume of interest"

3.  Select the `SPM.mat` file (generated during the previous section).

4.  Adjust data: `NaN`

5.  Which session: `1`

6.  Name of VOI: `WM`

7.  Select 'Region(s) of Interest' $>$ Sphere

8.  Centre: `[0 -24 -33]`

9.  VOI radius (mm): e.g.`6` mm

10. Select 'Movement of Centre' $>$ Fixed

11. Select 'Region of Interest' $>$ Mask Image

12. Image file: select `mask.nii` (in `GLM` folder)

13. Expression: `i1&i2`

14. Now you should be able to press the green arrow button. This would
    extract the WM time series and save this as `VOI_WM_1.mat` in the
    working directory.

Do the same thing with to extract CSF (from one of the ventricles)
signal with a sphere centred on `[0 -40 -5]`. This will create files
`VOI_CSF_1.mat`. Next we need to adjust the SPM with the covariates. Do
the following procedure:

1.  From the main SPM window, click on the
    <span class="smallcaps">Batch</span> button.

2.  From the SPM menu at the top of the <span class="smallcaps">Batch
    Editor</span>, select "Stats $>$ fMRI model specification".

3.  Click Directory and choose the `GLM` directory that you made above.

4.  <span class="smallcaps">Units for design</span>
    \[<span class="smallcaps">scans</span>\]

5.  <span class="smallcaps">Interscan interval</span> \[2\]

6.  Click <span class="smallcaps">Data & Design</span>, Choose
    <span class="smallcaps">New \"Subject/Session\"</span>

7.  Click <span class="smallcaps">Scans</span> and choose all the
    functional scans `swrestxxxx.img`. There should be 175 `*.img`
    files.

8.  Click on Multiple Regressors. And then select the `VOI_CSF_1.mat`t,
    `VOI_WM_1.mat` and `rp_rest000.txt`. Leave the rest of the fields as
    default.

9.  From the SPM menu at the top of the Batch Editor, select "Stats $>$
    model estimation".

10. For <span class="smallcaps">Select SPM.mat</span>, click on the
    <span class="smallcaps">Dependency</span> button and choose the
    proposed item (the output from the previous module).

11. You should now be able to press the
    <span class="smallcaps">Run</span> green arrow at the top of the
    Batch Editor window (press 'continue' when asked for overwriting
    existing <span class="smallcaps">SPM.mat</span> file). This will
    specify and estimate the GLM.

### Extracting time series

Once you have specified and estimated the GLM, here is now a
step-by-step example for extracting the PCC time series:

1.  From the main SPM window, click on the
    <span class="smallcaps">Batch</span> button.

2.  From the SPM menu at the top of the Batch Editor, select "Util $>$
    Volume of interest"

3.  Select the `SPM.mat` file (generated during the previous section).

4.  Adjust data: `NaN`

5.  Which session: `1`

6.  Name of VOI: `PCC`

7.  Select 'Region(s) of Interet' $>$ Sphere

8.  Centre: `[0 -52 26]`

9.  VOI radius(mm): e.g.` 8` mm

10. Select 'Movement of Centre' $>$ Fixed

11. Select 'Region of Interest' $>$ Mask Image

12. Image file: select `mask.nii` (in `GLM` folder)

13. Expression: `i1&i2`

14. Now you should be able to press the green arrow button. This would
    extract the PCC time series and save this as `VOI_PCC_1.mat` in the
    working directory.

SPM now computes the first principal component of the time series from
all voxels included in the sphere. The result is stored (together with
the original time series) in a file named `VOI_PCC_1.mat` in the working
directory (the "1" refers to session 1). Do the same for the rest of the
VOIs: mPFC (`[3 54 -2]`), LIPC (\[-50 -63 32\]) and RIPC
(`[48 -69 35]`).

### Specifying and estimating the DCM

Now we have extracted the time series, we are ready to build the DCM. We
will look at a simplified version of the model described in (Razi et al.
2015). In our example here, we will model a fully connected system
comprising PCC, mPFC and bilateral IPC. This DCM is shown schematically
in Figure <a href="#spdcm_full" data-reference-type="ref"
data-reference="spdcm_full">1.1</a>, and can be made as follows:

1.  Press the large `Dynamic Causal Modelling` button.

2.  Choose <span class="smallcaps">specify</span>.

3.  Select the `SPM.mat` file you just created when specifying the GLM.

4.  `DCM_???.mat`: e.g. `DMN`.

5.  Select all VOIs in order `VOI_PCC_1`, `VOI_mPFC_1`, `VOI_LIPC_1` and
    `VOI_RIPC_1`.

6.  Specify slice timings for each area. The default values are set to
    the last slice of the data, which was the default in the original
    DCM version. For sequential (as opposed to interleaved) data, this
    modelling option allows to use DCM in combination with any TR (slice
    timing differences). Here, we proceed with the default values.

7.  Enter `0.04` for "Echo Time, TE\[s\]".

8.  Define the fully connected model. Your connectivity matrix should
    look like the one in See
    Figure <a href="#spdcm_Fig1" data-reference-type="ref"
    data-reference="spdcm_Fig1">1.2</a>.

A polite "Thank you" completes the model specification process. A file
called `DCM_DMN.mat` will have been generated.

You can now estimate the model parameters, either by pressing the DCM
button again and choosing <span class="smallcaps">estimate
(cross-spectra)</span>, or by typing

    >> spm_dcm_estimate('DCM_DMN');

from the MATLAB command line.

Once this is completed, you can review the results as follows:

1.  Press the DCM button.

2.  Choose <span class="smallcaps">review</span>.

3.  Select `DCM_DMN.mat`

By clicking "review\..." you can now select from multiple options, e.g.
you can revisit the fit of the model ("Cross-spectra (BOLD)"), shown in
Figure <a href="#spdcm_Fig2" data-reference-type="ref"
data-reference="spdcm_Fig2">1.3</a> or look at the parameter estimates
for the endogenous coupling ("Coupling (A)") as shown in
Figure <a href="#spdcm_Fig3" data-reference-type="ref"
data-reference="spdcm_Fig3">1.4</a>. Of course, you can also explore the
model results at the level of the MATLAB command line by loading the
model and inspecting the parameter estimates directly. These can be
found in `DCM.Ep.A` (endogenous coupling) and `DCM.Ep.a `(neuronal
parameters).

<figure id="spdcm_full">
<div class="center">
<img src="../../../assets/figures/manual/dcm_rs/dcm_mod_full.png" style="width:100mm" />
</div>
<figcaption><em>DCM with fully connected model.<span id="spdcm_full"
label="spdcm_full"></span></em></figcaption>
</figure>

<figure id="spdcm_Fig1">
<div class="center">
<img src="../../../assets/figures/manual/dcm_rs/Fig1.png" style="width:100mm" />
</div>
<figcaption><em>Specification of model depicted in Fig <a
href="#spdcm_Fig1" data-reference-type="ref"
data-reference="spdcm_Fig1">1.2</a>. Filled circles define the structure
of the extrinsic connections A such that all circles are filled since we
are using a fully connected model here. <span id="spdcm_Fig1"
label="spdcm_Fig1"></span></em></figcaption>
</figure>

<figure id="spdcm_Fig2">
<div class="center">
<img src="../../../assets/figures/manual/dcm_rs/Fig2.png" style="width:140mm" />
</div>
<figcaption><em>Plot of predicted and observed cross spectral densities
after convergence in Fig <a href="#spdcm_Fig2" data-reference-type="ref"
data-reference="spdcm_Fig2">1.3</a>. The dotted lines are measured cross
spectra and solid lines are its predictions. The lower left panel shows
the self cross spectra for the four regions. The rest of the graphics
show both self and cross spectra for the four regions. <span
id="spdcm_Fig2" label="spdcm_Fig2"></span></em></figcaption>
</figure>

<figure id="spdcm_Fig3">
<div class="center">
<img src="../../../assets/figures/manual/dcm_rs/Fig3.png" style="width:140mm" />
</div>
<figcaption><em>This Fig <a href="#spdcm_Fig3" data-reference-type="ref"
data-reference="spdcm_Fig3">1.4</a> shows the estimated fixed A matrix
(top and lower left panels). The posterior probabilities of these
effective connectivity parameters are shown on the lower right panel.
The red dashed line depicts the 95% threshold. <span id="spdcm_Fig3"
label="spdcm_Fig3"></span></em></figcaption>
</figure>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-dcm" class="csl-entry">

Friston, K. J., L. Harrison, and W. D. Penny. 2003. "Dynamic Causal
Modelling." *NeuroImage* 19 (4): 1273--1302.

</div>

<div id="ref-rsDCM2014" class="csl-entry">

Friston, K. J., J. Kahan, B. Biswal, and A. Razi. 2014. "A DCM for
Resting State fMRI." *NeuroImage* 94: 396--407.
<https://doi.org/10.1016/j.neuroimage.2013.12.009>.

</div>

<div id="ref-karl_vb_laplace" class="csl-entry">

Friston, K. J., J. Mattout, N. Trujillo-Bareto, J. Ashburner, and W. D.
Penny. 2007. "Variational Free Energy and the Laplace Approximation."
*NeuroImage* 34 (1): 220--34.
<https://doi.org/10.1016/j.neuroimage.2006.08.035>.

</div>

<div id="ref-karl_generalised_filtering" class="csl-entry">

Friston, K. J., K. E. Stephan, Baojuan Li, and J. Daunizeau. 2010.
"Generalised Filtering." *Mathematical Problems in Engineering* 621670.
<https://doi.org/10.1155/2010/621670>.

</div>

<div id="ref-karl_DEM" class="csl-entry">

Friston, K. J., N. Trujillo-Bareto, and J. Daunizeau. 2008. "DEM: A
Variational Treatment of Dynamic Systems." *NeuroImage* 41 (3): 849--85.
<https://doi.org/10.1016/j.neuroimage.2008.02.054>.

</div>

<div id="ref-stocDCM2011" class="csl-entry">

Li, Baojuan, J. Daunizeau, K. E. Stephan, W. D. Penny, D. Hu, and K. J.
Friston. 2011. "Generalised Filtering and Stochastic DCM for fMRI."
*NeuroImage* 58 (2): 442--57.
<https://doi.org/10.1016/j.neuroimage.2011.01.085>.

</div>

<div id="ref-rsDCM2015" class="csl-entry">

Razi, A., J. Kahan, G. Rees, and K. J. Friston. 2015. "Construct
Validation of DCM for Resting State fMRI." *NeuroImage* 106: 1--14.
<https://doi.org/10.1016/j.neuroimage.2014.11.027>.

</div>

</div>

[^1]: Resting state DCM dataset:
    <http://www.fil.ion.ucl.ac.uk/spm/data/spDCM/>

[^2]: "1000 Functional Connectomes" Project,:
    <http://fcon_1000.projects.nitrc.org/fcpClassic/FcpTable.html>
