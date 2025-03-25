# Dynamic Causal Modelling for M/EEG <span id="Chap:eeg:DCM" label="Chap:eeg:DCM"></span>

## Introduction

Dynamic Causal Modelling (DCM) is based on an idea initially developed
for fMRI data: The measured data are explained by a network model
consisting of a few sources, which are interacting dynamically. This
network model is inverted using a Bayesian approach, and one can make
inferences about connections between sources, or the modulation of
connections by task.

For M/EEG data, DCM is a powerful technique for inferring about
parameters that one doesn't observe with M/EEG directly. Instead of
asking 'How does the strength of the source in left superior temporal
gyrus (STG) change between condition A and B?', one can ask questions
like 'How does the backward connection from this left STG source to left
primary auditory cortex change between condition A and B?'. In other
words, one isn't limited to questions about source strength as estimated
using a source reconstruction approach, but can test hypotheses about
what is happening between sources, in a network.

As M/EEG data is highly resolved in time, as compared to fMRI, the
inferences are about more neurobiologically plausible parameters. These
relate more directly to the causes of the underlying neuronal dynamics.

The key DCM for M/EEG methods paper appeared in 2006, and the first DCM
studies about mismatch negativity came out in 2007/2008. At its heart
DCM for M/EEG is a source reconstruction technique, and for the spatial
domain we use exactly the same leadfields as other approaches. However,
what makes DCM unique, is that is combines the spatial forward model
with a biologically informed temporal forward model, describing e.g. the
connectivity between sources. This critical ingredient not only makes
the source reconstruction more robust by implicitly constraining the
spatial parameters, but also allows one to infer about connectivity.  
  
Our methods group is continuing to work on further improvements and
extensions to DCM. In the following, we will describe the usage of DCM
for evoked responses (both MEG and EEG), DCM for induced responses
(i.e., based on power data in the time-frequency domain), and DCM for
local field potentials (measured as steady-state responses). All three
DCMs share the same interface, as many of the parameters that need to be
specified are the same for all three approaches. Therefore, we will
first describe DCM for evoked responses, and then point out where the
differences to the other two DCMs lie.  
  
This manual provides only a procedural guide for the practical use of
DCM for M/EEG. If you want to read more about the scientific background,
the algorithms used, or how one would typically use DCM in applications,
we recommend the following reading. The two key methods contributions
can be found in (David et al. 2006) and (Kiebel, David, and Friston
2006). Two other contributions using the model for testing interesting
hypotheses about neuronal dynamics are described in (Kiebel, Garrido,
and Friston 2007) and (Fastenrath, Friston, and Kiebel 2008). At the
time of writing, there were also three application papers published
which demonstrate what kind of hypotheses can be tested with DCM
(Garrido, Kilner, Kiebel, Stephan, et al. 2007; Garrido, Kilner, Kiebel,
and Friston 2007; Garrido et al. 2008). Another good source of
background information is the recent SPM book (Friston et al. 2007),
where Parts 6 and 7 cover not only DCM for M/EEG but also related
research from our group. The DCMs for induced responses and steady-state
responses are covered in (Chen, Kiebel, and Friston 2008; Chen et al.
2009) and (Moran et al. 2009, 2008, 2007). Also note that there is a DCM
example file, which we put onto the webpage
http://www.fil.ion.ucl.ac.uk/spm/data/eeg_mmn/. After downloading
DCMexample.mat, you can load (see below) this file using the DCM GUI,
and have a look at the various options, or change some, after reading
the description below.

## Overview

In summary, the goal of DCM is to explain measured data (such as evoked
responses) as the output of an interacting network consisting of a
several areas, some of which receive input (i.e., the stimulus). The
differences between evoked responses, measured under different
conditions, are modelled as a modulation of selected DCM parameters,
e.g. cortico-cortical connections (David et al. 2006). This
interpretation of the evoked response makes hypotheses about
connectivity directly testable. For example, one can ask, whether the
difference between two evoked responses can be explained by top-down
modulation of early areas (Garrido, Kilner, Kiebel, Stephan, et al.
2007). Importantly, because model inversion is implemented using a
Bayesian approach, one can also compute Bayesian model evidences. These
can be used to compare alternative, equally plausible, models and decide
which is the best (Kiebel et al. 2008).

DCM for evoked responses takes the spatial forward model into account.
This makes DCM a spatiotemporal model of the full data set (over
channels and peri-stimulus time). Alternatively, one can describe DCM
also as a spatiotemporal source reconstruction algorithm which uses
additional temporal constraints given by neural mass dynamics and
long-range effective connectivity. This is achieved by parameterising
the lead-field, i.e., the spatial projection of source activity to the
sensors. In the current version, this can be done using two different
approaches. The first assumes that the leadfield of each source is
modelled by a single equivalent current dipole (ECD) (Kiebel, David, and
Friston 2006). The second approach posits that each source can be
presented as a 'patch' of dipoles on the grey matter sheet (Daunizeau,
Kiebel, and Friston 2009). This spatial model is complemented by a model
of the temporal dynamics of each source. Importantly, these dynamics not
only describe how the intrinsic source dynamics evolve over time, but
also how a source reacts to external input, coming either from
subcortical areas (stimulus), or from other cortical sources.

The GUI allows one to enter all the information necessary for specifying
a spatiotemporal model for a given data set. If you want to fit multiple
models, we recommend using a batch script. An example of such a script
(*DCM_ERP_example*), which can be adapted to your own data, can be found
in the *man/example_scripts/* folder of the distribution. You can run
this script on example data provided by via the SPM webpage
(http://www.fil.ion.ucl.ac.uk/spm/data/eeg_mmn/). However, you first
have to preprocess these data to produce an evoked response by going
through the preprocessing tutorial (chapter
<a href="#Chap:data:mmn" data-reference-type="ref"
data-reference="Chap:data:mmn">[Chap:data:mmn]</a>) or by running the
`history_subject1.m` script in the `example_scripts` folder.

## Calling DCM for ERP/ERF

After calling *spm eeg*, you see SPM's graphical user interface, the
top-left window. The button for calling the DCM-GUI is found in the
second partition from the top, on the right hand side. When pressing the
button, the GUI pops up. The GUI is partitioned into five parts, going
from the top to the bottom. The first part is about loading and saving
existing DCMs, and selecting the type of model. The second part is about
selecting data, the third is for specification of the spatial forward
model, the fourth is for specifying connectivity, and the last row of
buttons allows you to estimate parameters and view results.

You have to select the data first and specify the model in a fixed order
(data selection $>$ spatial model $>$ connectivity model). This order is
necessary, because there are dependencies among the three parts that
would be hard to resolve if the input could be entered in any order. At
any time, you can switch back and forth from one part to the next. Also,
within each part, you can specify information in any order you like.

## load, save, select model type

At the top of the GUI, you can load an existing DCM or save the one you
are currently working on. In general, you can *save* and *load* during
model specification at any time. You can also switch between different
DCM analyses (the left menu). The default is 'ERP' which is DCM for
evoked responses described here. Currently, the other types are
cross-spectral densities (CSD), induced responses (IND) and phase
coupling (PHA) described later in this chapter. The menu on the
right-hand side lets you choose the neuronal model. 'ERP' is the
standard model described in most of our older papers, e.g. (David et al.
2006). 'SEP' uses a variant of this model, however, the dynamics tend to
be faster (Marreiros et al. 2008). 'NMM' is a nonlinear neural mass
model based on a first-order approximation, and 'MFM', is also nonlinear
and is based on a second-order approximation. 'NMDA' is a variant of the
'NMM' model which also includes a model of NMDA receptor. 'CMC' and
'CMM' are canonical microcircuit models  (Bastos et al. 2012) used in
the more recent paper to link models of neurophysiological phenomena
with canonical models of cortical processing based on the idea of
predictive coding.

## Data and design

In this part, you select the data and model between-trial effects. The
data can be either event-related potentials or fields. These data must
be in the SPM-format. On the right-hand side you can enter trial indices
of the evoked responses in this SPM-file. For example, if you want to
model the second and third evoked response contained within an SPM-file,
specify indices 2 and 3. The indices correspond to the order specified
by the *condlist* method (see
<a href="#Chap:eeg:preprocessing" data-reference-type="ref"
data-reference="Chap:eeg:preprocessing">[Chap:eeg:preprocessing]</a>).
If the two evoked responses, for some reason, are in different files,
you have to merge these files first. You can do this with the SPM
preprocessing function *merge* (*spm_eeg_merge*), see
<a href="#Chap:eeg:preprocessing" data-reference-type="ref"
data-reference="Chap:eeg:preprocessing">[Chap:eeg:preprocessing]</a>.
You can also choose how you want to model the experimental effects (i.e.
the differences between conditions). For example, if trial `1` is the
standard and trial `2` is the deviant response in an oddball paradigm,
you can use the standard as the baseline and model the differences in
the connections that are necessary to fit the deviant. To do that type
`0 1` in the text box below trial indices. Alternatively, if you type
`-1 1` then the baseline will be the average of the two conditions and
the same factor will be subtracted from the baseline connection values
to model the standard and added to model the deviant. The latter option
is perhaps not optimal for an oddball paradigm but might be suitable for
other paradigms where there is no clear 'baseline condition'. When you
want to model three or more evoked responses, you can model the
modulations of a connection strength of the second and third evoked
responses as two separate experimental effects relative to the first
evoked response. However, you can also choose to couple the connection
strength of the first evoked response with the two gains by imposing a
linear relationship on how this connection changes over trials. Then you
can specify a single effect (e.g. `-1 0 1`). This can be useful when one
wants to add constraints on how connections (or other DCM parameters)
change over trials. A compelling example of this can be found in
(Garrido et al. 2008). For each experimental effect you specify, you
will be able to select the connections in the model that are affected by
it (see below).

Press the button `’data file’` to load the M/EEG dataset. Under 'time
window (ms)' you have to enter the peri-stimulus times which you want to
model, e.g. 1 to 200 ms.

You can choose whether you want to model the mean or drifts of the data
at sensor level. Select 1 for 'detrend' to just model the mean.
Otherwise select the number of discrete cosine transform terms you want
to use to model low-frequency drifts ($> 1$). In DCM, we use a
projection of the data to a subspace to reduce the amount of data. The
type of spatial projection is described in (Fastenrath, Friston, and
Kiebel 2008). You can select the number of modes you wish to keep. The
default is 8.

You can also choose to window your data, along peri-stimulus time, with
a hanning window (radio button). This windowing will reduce the
influence of the beginning and end of the time-series.

If you are happy with your data selection, the projection and the
detrending terms, you can click on the $>$ (forward) button, which will
bring you to the next stage *electromagnetic model*. From this part, you
can press the red $<$ button to get back to the data and design part.

## Electromagnetic model

With the present version of DCM, you have three options for how to
spatially model your evoked responses. Either you use a single
equivalent current dipole (ECD) for each source, or you use a patch on
the cortical surface (IMG), or you don't use a spatial model at all
(local field potentials (LFP)). In all three cases, you have to enter
the source names (one name in one row). For ECD and IMG, you have to
specify the prior source locations (in mm in MNI coordinates). Note that
by default DCM uses uninformative priors on dipole orientations, but
tight priors on locations. This is because tight priors on locations
ensure that the posterior location will not deviate to much from its
prior location. This means each dipole stays in its designated area and
retains its meaning. The prior location for each dipole can be found
either by using available anatomical knowledge or by relying on source
reconstructions of comparable studies. Also note that the prior location
doesn't need to be overly exact, because the spatial resolution of M/EEG
is on a scale of several millimeters. You can also load the prior
locations from a file ('load'). You can visualize the locations of all
sources when you press 'dipoles'.

The onset-parameter determines when the stimulus, presented at 0 ms
peri-stimulus time, is assumed to activate the cortical area to which it
is connected. In DCM, we usually do not model the rather small early
responses, but start modelling at the first large deflection. Because
the propagation of the stimulus impulse through the input nodes causes a
delay, we found that the default value of 60 ms onset time is a good
value for many evoked responses where the first large deflection is seen
around 100 ms. However, this value is a prior, i.e., the inversion
routine can adjust it. The prior mean should be chosen according to the
specific responses of interest. This is because the time until the first
large deflection is dependent on the paradigm or the modality you are
working in, e.g. audition or vision. You may also find that changing the
onset prior has an effect on how your data are fitted. This is because
the onset time has strongly nonlinear effects (a delay) on the data,
which might cause differences in which maximum was found at convergence,
for different prior values. It is also possible to type several numbers
in this box (identical or not) and then there will be several inputs
whose timing can be optimized separately. These inputs can be connected
to different model sources. This can be useful, for instance, for
modelling a paradigm with combined auditory and visual stimulation.The
'duration (sd)' box makes it possible to vary the width of the input
volley, separately for each of the inputs. This can be used to model
more closely the actual input structure (e.g. a long tone or extended
presentation of a visual input). By combining several inputs with
different durations one can approximate an even more complex input
waveform (e.g. speech).

When you want to proceed to the next model specification stage, hit the
$>$ (forward) button and proceed to the *neuronal model*.

## Neuronal model

There are five (or more) matrices which you need to specify by button
presses. The first three are the connection strength parameters for the
first evoked response. There are three types of connections, *forward*,
*backward* and *lateral*. In each of these matrices you specify a
connection *from* a source area *to* a target area. For example,
switching on the element $(2,\;1)$ in the intrinsic forward connectivity
matrix means that you specify a forward connection from area 1 to 2.
Some people find the meaning of each element slightly counter-intuitive,
because the column index corresponds to the source area, and the row
index to the target area. This convention is motivated by direct
correspondence between the matrices of buttons in the GUI and
connectivity matrices in DCM equations and should be clear to anyone
familiar with matrix multiplication.

The one or more inputs that you specified previously can go to any area
and to multiple areas. You can select the receiving areas by selecting
area indices in the *C input* vector.

The *B* matrix contains all gain modulations of connection strengths as
set in the $A$-matrices. These modulations model the difference between
the first and the other modelled evoked responses. For example, for two
evoked responses, DCM explains the first response by using the
$A$-matrix only. The 2nd response is modelled by modulating these
connections by the weights in the $B$-matrix.

## Estimation

When you are finished with model specification, you can hit the
*estimate* button in the lower left corner. If this is the first
estimation and you have not tried any other source reconstructions with
this file, DCM will build a spatial forward model. You can use the
template head model for quick results. DCM will now estimate model
parameters. You can follow the estimation process by observing the model
fit in the output window. In the matlab command window, you will see
each iteration printed out with expected-maximization iteration number,
free energy $F$, and the predicted and actual change of $F$ following
each iteration step. At convergence, DCM saves the results in a DCM
file, by default named 'DCM_ERP.mat'. You can save to a different name,
eg. if you are estimating multiple models, by pressing 'save' at the top
of the GUI and writing to a different name.

## Results

After estimation is finished, you can assess the results by choosing
from the pull-down menu at the bottom (middle).

With *ERPs (mode)* you can plot, for each mode, the data for both evoked
responses, and the model fit.

When you select *ERPs (sources)*, the dynamics of each area are plotted.
The activity of the pyramidal cells (which is the reconstructed source
activity) are plotted in solid lines, and the activity of the two
interneuron populations are plotted as dotted lines.

The option *coupling (A)* will take you to a summary about the posterior
distributions of the connections in the $A$-matrix. In the upper row,
you see the posterior means for all intrinsic connectivities. As above,
element $(i,\; j)$ corresponds to a connection from area $j$ to $i$. In
the lower row, you'll find, for each connection, the probability that
its posterior mean is different from the prior mean, taking into account
the posterior variance.

With the option *coupling(B)* you can access the posterior means for the
gain modulations of the intrinsic connectivities and the probability
that they are unequal to the prior means. If you specified several
experimental effects, you will be asked which of them you want to look
at.

With *coupling(C)* you see a summary of the posterior distribution for
the strength of the input into the input receiving area. On the left
hand side, DCM plots the posterior means for each area. On the right
hand side, you can see the corresponding probabilities.

The option *Input* shows you the estimated input function. As described
by (David et al. 2006), this is a gamma function with the addition of
low-frequency terms.

With *Response*, you can plot the selected data, i.e. the data, selected
by the spatial modes, but back-projected into sensor space.

With *Response (image)*, you see the same as under Results but plotted
as an image in grey-scale.

And finally, with the option *Dipoles*, DCM displays an overlay of each
dipole on an MRI template using the posterior means of its 3 orientation
and 3 location parameters. This makes sense only if you have selected an
ECD model under *electromagnetic model*.

Before estimation, when you press the button 'Initialise' you can assign
parameter values as initial starting points for the free-energy gradient
ascent scheme. These values are taken from another already estimated
DCM, which you have to select.

The button *BMS* allows you do Bayesian model comparison of multiple
models. It will open the SPM batch tool for model selection. Specify a
directory to write the output file to. For the "Inference method" you
can choose between "Fixed effects" and "Random effects" (see (Stephan et
al. 2009) for additional explanations). Choose "Fixed effects" if you
are not sure. Then click on "Data" and in the box below click on "New:
Subject". Click on "Subject" and in the box below on "New: Session".
Click on models and in the selection window that comes up select the DCM
mat files for all the models (remember the order in which you select the
files as this is necessary for interpreting the results). Then run the
model comparison by pressing the green "Run" button. You will see, at
the top, a bar plot of the log-model evidences for all models. At the
bottom, you will see the probability, for each model, that it produced
the data. By convention, a model can be said to be the best among a
selection of other models, with strong evidence, if its log-model
evidence exceeds all other log-model evidences by at least 3.

## Cross-spectral densities

### Model specification

DCM for cross-spectral densities can be applied to M/EEG or intracranial
data.

The top panel of the DCM for ERP window allows you to toggle through
available analysis methods. On the top left drop-down menu, select
'CSD'. The second drop-down menu in the right of the top-panel allows
you to specify whether the analysis should be performed using a model
which is linear in the states, for this you can choose ERP or CMC.
Alternatively you may use a conductance based model, which is non-linear
in the states by choosing, 'NMM', 'MFM' or 'NMDA'. (see (Marreiros et
al. 2008) for a description of the differences).

The steady state (frequency) response is generated automatically from
the time domain recordings. The time duration of the frequency response
is entered in the second panel in the time-window. The options for
detrending allow you to remove either 1st, 2nd, 3rd or 4th order
polynomial drifts from channel data. In the subsampling option you may
choose to downsample the data before constructing the frequency
response. The number of modes specifies how many components from the
leadfield are present in channel data. The specification of between
trial effects and design matrix entry is the same as for the case of
ERPs, described above.

### The Lead-Field

The cross-spectral density is a description of the dependencies among
the observed outputs of these neuronal sources. To achieve this
frequency domain description we must first specify the likely sources
and their location. If LFP data are used then only source names are
required. This information is added in the third panel by selecting
'LFP'. Alternatively, x,y,z coordinates are specified for ECD or IMG
solutions.

### Connections

The bottom panel then allows you to specify the connections between
sources and whether these sources can change from trial type to trial
type.

On the first row, three connection types may be specified between the
areas. For NMM and MFM options these are Excitatory, Inhibitory or Mixed
excitatory and inhibitory connections. When using the ERP option the
user will specify if connections are 'Forward', 'Backward' or 'Lateral'.
To specify a connection, switch on the particular connection matrix
entry. For example to specify an Inhibitory connection from source 3 to
source 1, turn on the 'Inhib' entry at position (3,1).

On this row the inputs are also specified. These are where external
experimental inputs enter the network.

The matrix on the next row allows the user to select which of the
connections specified above can change across trial types. For example
in a network of two sources with two mixed connections (1,2) and (2,1),
you may wish to allow only one of these to change depending on
experimental context. In this case, if you wanted the mixed connection
from source 2 to source 1 to change depending on trial type, then select
entry (2,1) in this final connection matrix.

### Cross Spectral Densities

The final selection concerns what frequencies you wish to model. These
could be part of a broad frequency range e.g. like the default 4 - 48
Hz, or you could enter a narrow band e.g. 8 to 12 Hz, will model the
alpha band in 1Hz increments.

Once you hit the 'invert DCM' option the cross spectral densities are
computed automatically (using the spectral-toolbox). The data for
inversion includes the auto-spectra and cross-spectra between channels
or between channel modes. This is computed using a multivariate
autoregressive model, which can accurately measure periodicities in the
time-domain data. Overall the spectra are then presented as an
upper-triangular, s x s matrix, with auto-spectra on the main diagonal
and cross-spectra in the off-diagonal terms.

### Output and Results

The results menu provides several data estimates. By examining the
'spectral data', you will be able to see observed spectra in the matrix
format described above. Selecting 'Cross-spectral density' gives both
observed and predicted responses. To examine the connectivity estimates
you can select the 'coupling (A)' results option, or for the modulatory
parameters, the 'coupling (B)' option. Also you can examine the input
strength at each source by selecting the 'coupling (C)' option, as in
DCM for ERPs. The option 'trial-specific effects' shows the change in
connectivity parameter estimates (from B) from trial to trial relative
to the baseline connection (from A). To examine the spectral input to
these sources choose the 'Input' option; this should look like a mixture
of white and pink noise. Finally the 'dipoles' option allows
visualisation of the a posteriori position and orientation of all
dipoles in your model.

## Induced responses

DCM for induced responses aims to model coupling within and between
frequencies that are associated with linear and non-linear mechanisms
respectively. The procedure to do this is similar to that for DCM for
ERP/ERF. In the following, we will just point out the differences in how
to specify models in the GUI. Before using the technique, we recommend
reading about the principles behind DCM for induced responses (Chen,
Kiebel, and Friston 2008).

### Data

The data to be modelled must be single trial, epoched data. We will
model the entire spectra, including both the evoked (phase-locked to the
stimulus) and induced (non-phase-locked to the stimulus) components.

### Electromagnetic model

Currently, DCM for induced responses uses only the ECD method to capture
the data features. Note that a difference to DCM for evoked responses is
that the parameters of the spatial model are not optimized. This means
that DCM for induced responses will project the data into source space
using the spatial locations provided by you.

### Neuronal model

This is where you specify the connection architecture. Note that in DCM
for induced responses, the $A$-matrix encodes the linear and nonlinear
coupling strength between sources.

### Wavelet transform

This function can be called below the connectivity buttons and allows
one to transfer data into the time-frequency domain using a Morlet
Wavelet transform as part of the feature extraction. There are two
parameters: The frequency window defines the desired frequency band and
the wavelet number specifies the temporal-frequency resolution. We
recommend values greater than 5 to obtain a stable estimation.

### Results

#### Frequency modes

This will display the frequency modes, identified using singular value
decomposition of spectral dynamics in source space (over time and
sources).

#### Time-Frequency

This will display the observed time-frequency power data for all
pre-specified sources (upper panel) and the fitted data (lower panel).

#### Coupling (A-Hz)

This will display the coupling matrices representing the coupling
strength from source to target frequencies.

## Phase-coupled responses

DCM for phase-coupled responses is based on a weakly coupled oscillator
model of neuronal interactions.

### Data

The data to be modeled must be multiple trial, epoched data. Multiple
trials are required so that the full state-space of phase differences
can be explored. This is achieved with multiple trials as each trial is
likely to contain different initial relative phase offsets. Information
about different trial types is entered as it is with DCM for ERP ie.
using a design matrix. DCM for phase coupling is intended to model
dynamic transitions toward synchronization states. As these transitions
are short it is advisable to use short time windows of data to model and
the higher the frequency of the oscillations you are interested in, the
shorter this time window should be. DCM for phase coupling will probably
run into memory problems if using long time windows or large numbers of
trials.

### Electromagnetic model

Currently, DCM for phase-coupled responses will work with either ECD or
LFP data. Note that a difference to DCM for evoked responses is that the
parameters of the spatial model are not optimized. This means that DCM
for phase-coupled responses will project the data into source space
using the spatial locations you provide.

### Neuronal model

This is where you specify the connection architecture for the weakly
coupled oscillator model. If using the GUI, the Phase Interaction
Functions are given by $a_{ij} sin (\phi_i-\phi_j)$ where $a_{ij}$ are
the connection weights that appear in the $A$-matrix and $\phi_i$ and
$\phi_j$ are the phases in regions $i$ and $j$. DCM for phase coupling
can also be run from a MATLAB script. This provides greater flexibility
in that the Phase Interaction Functions can be approximated using
arbitrary order Fourier series. Have a look in the `example_scripts` to
see how.

### Hilbert transform

Pressing this button does two things. First, source data are bandpass
filtered into the specified range. Second, a Hilbert transform is
applied from which time series of phase variables are obtained.

### Results

#### Sin(Data) - Region i

This plots the sin of the data (ie. sin of phase variable) and the
corresponding model fit for the $i$th region.

#### Coupling (A),(B)

This will display the intrinsic and modulatory coupling matrices. The
$i,j$th entry in $A$ specifies how quickly region $i$ changes its phase
to align with region $j$. The corresponding entry in $B$ shows how these
values are changed by experimental manipulation.

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-Bastos2012" class="csl-entry">

Bastos, A. M., W. M. Usrey, R. A. Adams, G. R. Mangun, P. Fries, and K.
J. Friston. 2012. "Canonical Microcircuits for Predictive Coding."
*Neuron* 76 (4): 695--711.
<https://doi.org/10.1016/j.neuron.2012.10.038>.

</div>

<div id="ref-cc_asymm" class="csl-entry">

Chen, C. C., R. N. A. Henson, K. E. Stephan, J. Kilner, and K. J.
Friston. 2009. "Forward and Backward Connections in the Brain: A DCM
Study of Functional Asymmetries." *NeuroImage* 45 (2): 453--62.
<https://doi.org/10.1016/j.neuroimage.2008.12.041>.

</div>

<div id="ref-cc_induced" class="csl-entry">

Chen, C. C., S. J. Kiebel, and K. J. Friston. 2008. "Dynamic Causal
Modelling of Induced Responses." *NeuroImage* 41 (4): 1293--1312.
<https://doi.org/10.1016/j.neuroimage.2008.03.026>.

</div>

<div id="ref-jean_distributed" class="csl-entry">

Daunizeau, J., S. J. Kiebel, and K. J. Friston. 2009. "Dynamic Causal
Modelling of Distributed Electromagnetic Responses." *NeuroImage*.
<https://doi.org/10.1016/j.neuroimage.2009.04.062>.

</div>

<div id="ref-od_dcm_erp" class="csl-entry">

David, O., S. J. Kiebel, L. Harrison, J. Mattout, J. Kilner, and K. J.
Friston. 2006. "Dynamic Causal Modelling of Evoked Responses in EEG and
MEG." *NeuroImage* 30: 1255--72.
[https://doi.org/doi:10.1016/j.neuroimage.2005.10.045
](https://doi.org/doi:10.1016/j.neuroimage.2005.10.045 ).

</div>

<div id="ref-matthias_dcm_constraints" class="csl-entry">

Fastenrath, Matthias, K. J. Friston, and S. J. Kiebel. 2008. "Dynamical
Causal Modelling for M/EEG: Spatial and Temporal Symmetry Constraints."
*NeuroImage*. <https://doi.org/10.1016/j.neuroimage.2008.07.041>.

</div>

<div id="ref-spm_book" class="csl-entry">

Friston, K. J., J. Ashburner, S. J. Kiebel, T. E. Nichols, and W. D.
Penny, eds. 2007. *Statistical Parametric Mapping: The Analysis of
Functional Brain Images*. Academic Press.
<http://store.elsevier.com/product.jsp?isbn=9780123725608>.

</div>

<div id="ref-marta_mmndcm" class="csl-entry">

Garrido, M. I., K. J. Friston, K. E. Stephan, S. J. Kiebel, T. Baldeweg,
and J. Kilner. 2008. "The Functional Anatomy of the MMN: A DCM Study of
the Roving Paradigm." *NeuroImage* 42 (2): 936--44.
<https://doi.org/10.1016/j.neuroimage.2008.05.018>.

</div>

<div id="ref-mg_feedback" class="csl-entry">

Garrido, M. I., J. Kilner, S. J. Kiebel, and K. J. Friston. 2007.
"Evoked Brain Responses Are Generated by Feedback Loops." *PNAS* 104
(52): 20961--66. [https://doi.org/10.1073/pnas.0706274105
](https://doi.org/10.1073/pnas.0706274105 ).

</div>

<div id="ref-mg_dcm_repro" class="csl-entry">

Garrido, M. I., J. Kilner, S. J. Kiebel, K. E. Stephan, and K. J.
Friston. 2007. "Dynamic Causal Modelling of Evoked Potentials: A
Reproducibility Study." *NeuroImage* 36: 571--80.
<https://doi.org/10.1016/j.neuroimage.2007.03.014>.

</div>

<div id="ref-sjk_dcm_erp" class="csl-entry">

Kiebel, S. J., O. David, and K. J. Friston. 2006. "Dynamic Causal
Modelling of Evoked Responses in EEG/MEG with Lead-Field
Parameterization." *NeuroImage* 30: 1273--84.
<https://doi.org/doi:10.1016/j.neuroimage.2005.12.055>.

</div>

<div id="ref-sjk_dcm_intrinsic" class="csl-entry">

Kiebel, S. J., M. I. Garrido, and K. J. Friston. 2007. "Dynamic Causal
Modelling of Evoked Responses: The Role of Intrinsic Connections."
*NeuroImage* 36: 332--45.
<https://doi.org/10.1016/j.neuroimage.2007.02.046>.

</div>

<div id="ref-stefan_neurodynamics" class="csl-entry">

Kiebel, S. J., M. I. Garrido, R. Moran, and K. J. Friston. 2008.
"Dynamic Causal Modelling for EEG and MEG." *Cognitive Neurodynamics* 2
(2): 121--36. <https://doi.org/10.1007/s11571-008-9038-0>.

</div>

<div id="ref-andre_sigmoid" class="csl-entry">

Marreiros, A. C., J. Daunizeau, S. J. Kiebel, and K. J. Friston. 2008.
"Population Dynamics: Variance and the Sigmoid Activation Function."
*NeuroImage* 42 (1): 147--57.
<https://doi.org/10.1016/j.neuroimage.2008.04.239>.

</div>

<div id="ref-rm_spectralnmm" class="csl-entry">

Moran, R., S. J. Kiebel, N. Rombach, W. T. O'Connor, K. J. Murphy, R. B.
Reilly, and K. J. Friston. 2008. "Bayesian Estimation of Synaptic
Physiology from the Spectral Responses of Neural Masses." *NeuroImage*
42 (1): 272--84. <https://doi.org/10.1016/j.neuroimage.2008.01.025>.

</div>

<div id="ref-rm_massmodelspectral" class="csl-entry">

Moran, R., S. J. Kiebel, K. E. Stephan, R. B. Reilly, J. Daunizeau, and
K. J. Friston. 2007. "A Neural Mass Model of Spectral Responses in
Electrophysiology." *NeuroImage* 37 (3): 706--20.
[https://doi.org/10.1016/j.neuroimage.2007.05.032
](https://doi.org/10.1016/j.neuroimage.2007.05.032 ).

</div>

<div id="ref-dcm_ssr" class="csl-entry">

Moran, R., K. E. Stephan, T. Seidenbecher, H. C. Pape, R. Dolan, and K.
J. Friston. 2009. "Dynamic Causal Models of Steady-State Responses."
*NeuroImage* 44 (3): 796--811.
<https://doi.org/10.1016/j.neuroimage.2008.09.048>.

</div>

<div id="ref-klaas_bms" class="csl-entry">

Stephan, K. E., W. D. Penny, J. Daunizeau, R. Moran, and K. J. Friston.
2009. "Bayesian Model Selection for Group Studies." *NeuroImage* 46 (3):
1004--10174. <https://doi.org/10.1016/j.neuroimage.2009.03.025>.

</div>

</div>
