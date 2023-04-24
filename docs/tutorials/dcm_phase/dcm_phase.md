# DCM for Phase Coupling <span id="Chap:data:dcm_phase" label="Chap:data:dcm_phase"></span>

This chapter presents an extension of the Dynamic Causal Modelling (DCM)
framework to the analysis of phase-coupled data. A weakly coupled
oscillator approach is used to describe dynamic phase changes in a
network of oscillators. The influence that the phase of one oscillator
has on the change of phase of another is characterised in terms of a
Phase Interaction Function (PIF) as described in (Penny et al. 2009).
SPM supports PIFs specified using arbitrary order Fourier series.
However, to simplify the interface, one is restricted to simple
sinusoidal PIFs with the GUI.

## Data

We will use the merged epoched MEG face-evoked dataset[^1] saved in the
files:

    cdbespm12_SPM_CTF_MEG_example_faces1_3D.mat
    cdbespm12_SPM_CTF_MEG_example_faces1_3D.dat

DCM-Phase requires a head model and coregistration. If you have been
following the previous chapters of this tutorial, these should already
be available in the dataset. Otherwise, you should perform the 'Prepare'
and '3D Source reconstruction' steps described earlier in the chapter,
with the latter comprising the MRI, Co-register, Forward and Save
sub-steps.

## Getting Started

You need to start SPM and toggle "EEG" as the modality (bottom-right of
SPM main window), or start SPM with `spm eeg`. In order for this to work
you need to ensure that the main SPM directory is on your MATLAB path.
After calling `spm eeg`, you see SPM's graphical user interface, the
top-left window. The button for calling the DCM-GUI is found in the
second partition from the top, on the right hand side. When pressing the
button, the GUI pops up
(Figure <a href="#dcm-ir:fig:1" data-reference-type="ref"
data-reference="dcm-ir:fig:1">[dcm-ir:fig:1]</a>).

## Data and design

You should switch the DCM model type to "PHASE" which is the option for
DCM-Phase. Press "new data" and select the data file
`cdbespm12_SPM_CTF_MEG_example_faces1_3D.mat`. This is an epoched data
file with multiple trials per condition. On the right-hand side enter
the trial indices

    1 2

for the 'face' and 'scrambled' evoked responses (we will model both
trial types). The box below this list allows for specifying experimental
effects on connectivity. Enter

    1 0

in the first row of the box. This means that "face" trial types can have
different connectivity parameters than "scrambled" trial types. If we
now click somewhere outside the box, a default name will be assigned to
this effect - "effect1". It will appear in the small text box next to
the coefficients box. It is possible to change this name to something
else e.g. "face". Now we can select the peristimulus time window we want
to model. Set it to:

    1 300

ms. Select `1` for "detrend", to remove the mean from each data record.
The `sub-trials` option makes it possible to select just a subset of
trials for the analysis (select 2 for every second trial, 3 - for every
third etc.). This is useful because DCM-Phase takes quite a long time to
invert for all the trials and you might want to first try a smaller
subset to get an idea about the possible results. Here we will assume
that you used all the trials (sub-trials was set to 1). You can now
click on the $>$ (forward) button, which will bring you to the next
stage *electromagnetic model*. From this part, you can press the red $<$
button to get back to the data and design part.

## Electromagnetic model

With DCM-Phase, there are two options for how to extract the source
data. Firstly, you can use 3 orthogonal single equivalent current
dipoles (ECD) for each source, invert the resulting source model to get
3 source waveforms and take the first principal component. This option
is suitable for multichannel EEG or MEG data. Alternatively, you can
treat each channel as a source (LFP option). This is appropriate when
the channels already contain source data either recorded directly with
intracranial electrodes or extracted (e.g. using a beamformer).

Note that a difference to DCM for evoked responses is that the
parameters of the spatial model are not optimized. This means that
DCM-Phase (like DCM-IR) will project the data into source space using
the spatial locations you provide.

We will use the ECD option and specify just two source regions. This
requires specifying a list of source names in the left large text box
and a list of MNI coordinates for the sources in the right large text
box. Enter the following in the left box:

    LOFA
    LFFA

Now enter in the right text box:

    -39 -81 -15
    -39 -51 -24

These correspond to left Occipital Face Area, and left Fusiform Face
Area. The onset-parameter is irrelevant for DCM-Phase. Now hit the $>$
(forward) button and proceed to the *neuronal model*. Generally, if you
have not used the input dataset for 3D source reconstruction before you
will be asked to specify the parameters of the head model at this stage.

## Neuronal model

We will now define a coupled oscillator model for investigating network
synchronization of alpha activity. To this end, we first enter the
values 8 and 12 to define the frequency window. The wavelet number is
irrelevant for DCM-Phase. After source reconstruction (using a
pseudo-inverse approach), source data is bandpass filtered and then the
Hilbert transform is used to extract the instantaneous phase. The
DCM-Phase model is then fitted used standard routines as described in
(Penny et al. 2009).

Figure  <a href="#dcm-phase:fig:1" data-reference-type="ref"
data-reference="dcm-phase:fig:1">1.1</a> shows the four models we will
apply to the M/EEG data. We will first fit model 4. This model proposes
that alpha activity in region LOFA changes its phase so as to
synchronize with activity in region LFFA. In this network LFFA is the
master and LOFA is the slave. Moreover, the connection from LFFA to LOFA
is allowed to be different for scrambled versus unscrambled faces.

<figure id="dcm-phase:fig:1">
<div class="center">
<img src="../../../assets/figures/manual/dcm_phase/phase_models.png" style="width:160mm" />
</div>
<figcaption><em>Four different DCM-Phase models <span
id="dcm-phase:fig:1" label="dcm-phase:fig:1"></span></em></figcaption>
</figure>

The connectivity for Model 4 can be set up by configuring the radio
buttons as shown in
Figure <a href="#dcm-phase:fig:2" data-reference-type="ref"
data-reference="dcm-phase:fig:2">1.2</a>. You can now press the
`Invert DCM` button. It can take up to an hour to estimate the model
parameters depending on the speed of your computer.

<figure id="dcm-phase:fig:2">
<div class="center">
<img src="../../../assets/figures/manual/dcm_phase/model4_conn.png" style="width:50mm" />
</div>
<figcaption><em>Radio button configurations for DCM-Phase model 4 <span
id="dcm-phase:fig:2" label="dcm-phase:fig:2"></span></em></figcaption>
</figure>

## Results

After estimation is finished, you can assess the results by choosing
from the pull-down menu at the bottom (middle). The `Sin(Data)-Region i`
option will show the sin of the phase data in region $i$, for the first
16 trials. The blue line corresponds to the data and the red to the
DCM-Phase model fit. The `Coupling(As)` and `Coupling(Bs)` buttons
display the estimated endogenous and modulatory activity shown in
Figure <a href="#dcm-phase:fig:3" data-reference-type="ref"
data-reference="dcm-phase:fig:3">1.3</a>.

<figure id="dcm-phase:fig:3">
<div class="center">
<img src="../../../assets/figures/manual/dcm_phase/modulatory.png" style="width:50mm" />
</div>
<figcaption><em>The figure shows the estimated parameters for endogenous
coupling (left column) and modulatory parameters (right column) for the
4th DCM. <span id="dcm-phase:fig:3"
label="dcm-phase:fig:3"></span></em></figcaption>
</figure>

If one fits all the four models shown in
Figure <a href="#dcm-phase:fig:1" data-reference-type="ref"
data-reference="dcm-phase:fig:1">1.1</a> then they can be formally
compared using Bayesian Model Selection. This is implemented by pressing
the `BMS` button. You will need to first create a directory for the
results to go in e.g. `BMS-results`. For 'Inference Method' select FFX
(the RFX option is only viable if you have models from a group of
subjects). Under 'Data', Select 'New Subject' and under 'Subject' select
'New Session'. Then under 'Models' select the `DCM.mat` files you have
created. Then press the green play button. This will produce the results
plot shown in
Figure <a href="#dcm-phase:fig:4" data-reference-type="ref"
data-reference="dcm-phase:fig:4">1.4</a>. This leads us to conclude that
LFFA and LOFA act in master slave arrangement with LFFA as the master.

<figure id="dcm-phase:fig:4">
<div class="center">
<img src="../../../assets/figures/manual/dcm_phase/bms_phase.png" style="width:50mm" />
</div>
<figcaption><em>Bayesian comparison of the four DCM-Phase models shown
in Figure <a href="#dcm-phase:fig:1" data-reference-type="ref"
data-reference="dcm-phase:fig:1">1.1</a>.<span id="dcm-phase:fig:4"
label="dcm-phase:fig:4"></span></em></figcaption>
</figure>

## Extensions

In the DCM-Phase model accessible from the GUI, it is assumed that the
phase interaction functions are of simple sinusoidal form ie.
$a_{ij} \sin(\phi_j - \phi_i)$. The coefficients $a_{ij}$ are the values
shown in the endogenous parameter matrices in eg.
Figure <a href="#dcm-phase:fig:3" data-reference-type="ref"
data-reference="dcm-phase:fig:3">1.3</a>. These can then be changed by
an amount $b_{ij}$ as shown in the modulatory parameter matrices. It is
also possible to specify and estimate DCM-Phase models using matlab
scripts. In this case it is possible to specify more generic phase
interaction functions, such as arbitrary order Fourier series. Examples
are given in (Penny et al. 2009).

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-dcm_phase" class="csl-entry">

Penny, W. D., V. Litvak, L. Fuentemilla, E. Duzel, and K. J. Friston.
2009. "Dynamic Causal Models for Phase Coupling." *Journal of
Neuroscience Methods* 183 (1): 19--30.

</div>

</div>

[^1]: Multimodal face-evoked dataset:
    <http://www.fil.ion.ucl.ac.uk/spm/data/mmfaces/>
