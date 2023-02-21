# Localization of Equivalent Current Dipoles <span id="Chap:eeg:VBECD" label="Chap:eeg:VBECD"></span>

This chapter describes source reconstruction based on "Variational Bayes
Equivalent Current Dipoles" (VB-ECDs). For more details about the
implementation, please refer to the help and comments in the routines
themselves, as well as the original paper by (Kiebel et al. 2008).

## Introduction

3D imaging (or distributed) reconstruction methods consider all possible
source location simultaneously, allowing for large and widely spread
clusters of activity. This is to be contrasted with "Equivalent Current
Dipole" (ECD) approaches which rely on two different hypotheses:

- only a few (say less than 5) sources are active simultaneously, and

- those sources are very focal.

This leads to the ECD model where the observed scalp potential will be
explained by a handful of discrete current sources, i.e. dipoles,
located inside the brain volume.

In contrast to the 3D imaging reconstruction, the number of ECDs
considered in the model, i.e. the number of "active locations", should
be defined a priori. This is a crucial step, as the number of sources
considered defines the ECD model. This choice should be based on
empirical knowledge of the brain activity observed or any other source
of information (for example by looking at the scalp potential
distribution). In general, each dipole is described by 6 parameters: 3
for its location, 2 for its orientation and 1 for its amplitude. Once
the number of ECDs is fixed, a non-linear optimisation algorithm is used
to adjust the dipoles parameters (6 times the number of dipoles) to the
observed potential.

Classical ECD approaches use a simple best fitting optimisation using
"least square error" criteria. This leads to relatively simple
algorithms but presents a few drawbacks:

- constraints on the dipoles are difficult to include in the framework;

- the noise cannot be properly taken into account, as its variance
  should be estimated alongside the dipole parameters;

- it is difficult to define confidence intervals on the estimated
  parameters, which could lead to over-confident interpretation of the
  results;

- models with different numbers of dipoles cannot be compared except
  through their goodness-of-fit, which can be misleading.

As adding dipoles to a model will necessarily improve the overall
goodness of fit, one could erroneously be tempted to use as many ECDs as
possible and to perfectly fit the observed signal.  
Through using Bayesian techniques, however, it is possible to circumvent
all of the above limitations of classical approaches.

Briefly, a probabilistic generative model is built providing a
likelihood model for the data[^1]. The model is completed by a set of
priors on the various parameters, leading to a Bayesian model, allowing
the inclusion of user-specified prior constraints.

A "variational Bayes" (VB) scheme is then employed to estimate the
posterior distribution of the parameters through an iterative procedure.
The confidence interval of the estimated parameters is therefore
directly available through the estimated posterior variance of the
parameters. Critically, in a Bayesian context, different models can be
compared using their evidence or marginal likelihood. This model
comparison is superior to classical goodness-of-fit measures, because it
takes into account the complexity of the models (e.g., the number of
dipoles) and, implicitly, uncertainty about the model parameters. VB-ECD
can therefore provide an objective and accurate answer to the question:
Would this data set be better modelled by 2 or 3 ECDs?

## Procedure in SPM12

This section aims at describing how to use the VB-ECD approach in SPM12.

### Head and forward model

The engine calculating the projection of the dipolar sources on the
scalp electrode comes from Fieldtrip and is the same for the 3D imaging
or DCM. The head model should thus be prepared the same way, as
described in the chapter
<a href="#Chap:eeg:imaging" data-reference-type="ref"
data-reference="Chap:eeg:imaging">[Chap:eeg:imaging]</a>. For the same
data set, differences between the VB-ECD and imaging reconstructions
would therefore be due to the reconstruction approach only.

### VB-ECD reconstruction

To get started, after loading and preparing the head model, press the
'Invert' button[^2]. The first choice you will see is between 'Imaging',
'VB-ECD' and 'DCM'. The 'Imaging' reconstruction corresponds to the
imaging solution, as described in chapter
<a href="#Chap:eeg:imaging" data-reference-type="ref"
data-reference="Chap:eeg:imaging">[Chap:eeg:imaging]</a>, and 'DCM' is
described in chapter <a href="#Chap:eeg:DCM" data-reference-type="ref"
data-reference="Chap:eeg:DCM">[Chap:eeg:DCM]</a>. Then you are invited
to fill in information about the ECD model and click on buttons in the
following order:

1.  indicate the time bin or time window for the reconstruction, within
    the epoch length. Note that the data will be averaged over the
    selected time window! VB-ECD will thus always be calculated for a
    single time bin.

2.  enter the trial type(s) to be reconstructed. Each trial type will be
    reconstructed separately.

3.  <span id="add_dip" label="add_dip"></span> add a single (i.e.
    individual) dipole or a pair of symmetric dipoles to the model. Each
    "element" (single or pair) is added individually to the model.

4.  use "Informative" or "Non-informative" location priors.
    "Non-informative" means flat priors over the brain volume. With
    "Informative", you can enter the a priori location of the
    source[^3].

5.  use "Informative" or "Non-informative" moment priors.
    "Non-informative" means flat priors over all possible directions and
    amplitude. With "Informative", you can enter the a priori moment of
    the source[^4].

6.  go back to step <a href="#add_dip" data-reference-type="ref"
    data-reference="add_dip">[add_dip]</a> and add some more dipole(s)
    to the model, or stop adding dipoles.

7.  specify the number of iterations. These are repetitions of the
    fitting procedure with different initial conditions. Since there are
    multiple local maxima in the objective function, multiple iterations
    are necessary to get good results especially when non-informative
    location priors are chosen.

The routine then proceeds with the VB optimization scheme to estimate
the model parameters. There is graphical display of the intermediate
results. When the best solution is selected the model evidence will be
shown at the top of the SPM Graphics window. This number can be used to
compare solutions with different priors.  
Results are finally saved into the data structure `D` in the field
`.inv{D.val}.inverse` and displayed in the graphic window.

### Result display

The latest VB-ECD results can be displayed again through the function
`D = spm_eeg_inv_vbecd_disp`. If a specific reconstruction should be
displayed, then use: `spm_eeg_inv_vbecd_disp(’Init’,D, ind)`. In the GUI
you can use the `’dip’` button (located under the 'Invert' button) to
display the dipole locations.  
In the upper part, the 3 main figures display the 3 orthogonal views of
the brain with the dipole location and orientation superimposed. The
location confidence interval is described by the dotted ellipse around
the dipole location on the 3 views. It is not possible to click through
the image, as the display is automatically centred on the dipole
displayed. It is possible though to zoom into the image, using the
right-click context menu.

The lower left table displays the current dipole location, orientation
(Cartesian or polar coordinates) and amplitude in various formats.

The lower right table allows for the selection of trial types and
dipoles. Display of multiple trial types and multiple dipoles is also
possible. The display will center itself on the average location of the
dipoles.

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-stefan_vb_ecd" class="csl-entry">

Kiebel, S. J., J. Daunizeau, C. Phillips, and K. J. Friston. 2008.
"Variational Bayesian Inversion of the Equivalent Current Dipole Model
in EEG/MEG." *NeuroImage* 39 (2): 728--41.
<https://doi.org/10.1016/j.neuroimage.2007.09.005>.

</div>

</div>

[^1]: This includes an independent and identically distributed (IID)
    Normal distribution for the errors, but other distributions could be
    specified.

[^2]: The GUI for VB-ECD can also be launched directly from MATLAB
    command line with the instruction: `D = spm_eeg_inv_vbecd_gui`.

[^3]: For a pair of dipoles, only the right dipole coordinates are
    required.

[^4]: For a pair of dipoles, only the right dipole moment is required.
