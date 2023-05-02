# DCM for Induced Responses 

This chapter presents an example of Dynamic Causal Modelling for Induced Responses (DCM-IR) (Chen, Kiebel, and Friston 2008), based on the analysis described by Chen et al. (Chen et al. 2009). The analysis aims to examine the effective connectivity between cortical areas involved in face processing, focusing on non-linearities in connections expressed as cross-frequency coupling.

DCM-IR is a phenomenological DCM, not a physiological one. The advantage of this approach is that it can directly model a specific feature extracted from the data, namely event-related spectral perturbations, which have been widely studied in neuroscience literature. However, since computing event-related power requires discarding phase information, it is impossible to model this feature with a physiologically realistic model, such as the one used in DCM for evoked responses.

A key feature of DCM for induced responses is that it models the full time-frequency spectrum, differing from typical approaches where only a few specific frequency bands are selected *a priori*. DCM-IR models spectral dynamics in terms of a mixture of frequency modes (obtained with singular value decomposition), with the dynamics of each mode encoded by the evolution of a state. This multi-state vector, for each source, captures how the energy in different frequencies interacts, either linearly or non-linearly, among sources.

## Data

The epoched and merged MEG face-evoked dataset[^1] will be used, saved in the files:

```
    cdbespm12_SPM_CTF_MEG_example_faces1_3D.mat
    cdbespm12_SPM_CTF_MEG_example_faces1_3D.dat
```

DCM-IR also requires a head model and coregistration. If you have performed "Imaging" reconstruction of differential power and saved the results, the head model should already be defined. Otherwise, you will be asked to define the head model while configuring the DCM (see below).

## Getting Started

Start SPM and toggle "EEG" as the modality (bottom-right of SPM main window), or start SPM with `spm eeg`. Ensure that the main SPM directory is on your MATLAB path.

## Setting up DCM

After calling `spm eeg`, you will see SPM's graphical user interface in the top-left window. The button for calling the DCM-GUI is found in the second partition from the top, on the right-hand side. Pressing the button brings up the GUI (Figure <a href="#dcm-ir:fig:1" data-reference-type="ref" data-reference="dcm-ir:fig:1">1.1</a>). The GUI is divided into five parts, going from the top to the bottom. The first part deals with loading and saving existing DCMs and selecting the type of model. The second part is for selecting data, the third for specifying the spatial forward model, the fourth for defining connectivity, and the last row of buttons allows you to estimate parameters and view results.

<figure id="dcm-ir:fig:1">
<div class="center">
<img src="../../../assets/figures/manual/dcm_ir/irfigure1.png" style="width:100mm" />
</div>
<figcaption><em>DCM window configured for analysing induced responses and the FnBn model specified.<span id="dcm-ir:fig:1" label="dcm-ir:fig:1"></span></em></figcaption>
</figure>

Select the data first and specify the model in a fixed order (data selection $>$ spatial model $>$ connectivity model). This order is necessary because dependencies among the three parts would be difficult to resolve if input could be entered in any order. You can switch back and forth between parts at any time, and within each part, you can specify information in any order you prefer.

### Load, save, select model type

At the top of the GUI, you can load an existing DCM or save the one you are currently working on. You can `save` and `load` during model specification at any time. You can also switch between different DCM analyses (the left menu). The default is "ERP" (DCM for evoked responses). Switch to "IND" for DCM-IR. The right-hand side menu lets you choose the neuronal model. Once you switch to "IND", it will be disabled, as neuronal models are not relevant for DCM-IR, a phenomenological DCM.

### Data and design

In this part, you select the data and model between-trial effects. Press "new data" and select the data file `cdbespm12_SPM_CTF_MEG_example_faces1_3D.mat`. The data file should be an epoched file with multiple trials per condition in SPM-format. On the right-hand side, enter trial indices of the evoked responses in the SPM-file. For example, to model the first and second condition within an SPM-file, specify indices `1` and `2`. Type:

```matlab
    D = spm_eeg_load('cdbespm12_SPM_CTF_MEG_example_faces1_3D.mat');D.condlist
```

in the command line to see the list of condition labels in the order that corresponds to these indices. This order is defined in the dataset and can be modified by selecting "Sort Conditions" from the "Other" submenu in the main SPM window (*spm_eeg_sort_conditions*). SPM should echo:

```
    ans = 

        'faces'    'scrambled'
```

meaning that index `1` corresponds to presentation of faces and index `2` to presentation of scrambled faces. The box below the list of indices allows specifying experimental effects on connectivity. The specification can be quite generic, as in the design matrix for a General Linear Model (GLM). In our simple case, we have a baseline condition ("scrambled") and want to know how the "faces" condition differs from it. Enter:

```
    1 0
```

in the first row of the box, meaning that there will be some additive modulation of connections for "faces" (some coefficient multiplied by 1) and this modulation will not be there for "scrambled" (the same coefficient multiplied by 0). Clicking outside the box assigns a default name to this effect - "effect1". It is possible to change this name to something else, e.g. "face".

Now, select the peristimulus time window to model. Enter `-50` in the left box and `300` in the right box for the segment `-50` to `300` ms relative to the visual stimulus presentation.

Choose whether to remove low-frequency drifts of the data at sensor level. If not, select `1` for "detrend", to just remove the mean. Otherwise, select the number of discrete cosine transform terms you want to remove. You can also subsample your data (prior to computing the time-frequency decomposition) using the "subsample" option. Generally, filter out drifts and downsample the data during preprocessing. The options here are for exploration, data cleanup, or reduction without running additional processing steps outside DCM.

Press "Display" to view the selected data. You will see the evoked responses for the two conditions (Figure <a href="#dcm-ir:fig:2" data-reference-type="ref" data-reference="dcm-ir:fig:2">1.2</a>), which help you assess your choice of time window. You can change the "detrend" and "subsample" values or the time window and press "Display" again to see the effects of these changes.

A crucial parameter for DCM-IR is the number of modes, which are the frequency modes mentioned earlier. The main features of the time-frequency image can be represented by a limited number of components with fixed frequency profiles modulated over time. These components can be determined automatically using Singular Value Decomposition (SVD). Generally, SVD retains information from the original time-frequency image, producing as many components as there are frequency bins. However, usually, only the first few components are physiologically relevant, while the rest are noise. Using fewer components significantly accelerates DCM model inversion. It is impossible to predict the optimal number of components for your data. One approach is to start with a relatively large number (e.g., 8) and then assess the time and frequency profiles of the later components (in the Results view, as described later) to determine their importance. You can then reduce the number and try again. For this example, using 4 modes is sufficient, so change the number in "modes" from 8 to 4.

<figure id="dcm-ir:fig:2">
<div class="center">
<img src="../../../assets/figures/manual/dcm_ir/irfigure2.png" style="width:160mm" />
</div>
<figcaption><em>Averaged evoked responses after configuring the 'Data and design' section.<span id="dcm-ir:fig:2" label="dcm-ir:fig:2"></span></em></figcaption>
</figure>

If you are satisfied with your data selection, subsampling, and detrending terms, click on the $>$ (forward) button, advancing to the next stage, *electromagnetic model*. From this section, you can press the red $<$ button to return to the data and design part.

### Electromagnetic model

With DCM-IR, you have two options for extracting the source data for time-frequency analysis. Firstly, you can use 3 orthogonal single equivalent current dipoles (ECD) for each source and invert the resulting source model to get source waveforms. This option is suitable for multichannel EEG or MEG data. Alternatively, you can treat each channel as a source (LFP option). This is appropriate when the channels already contain source data either recorded directly with intracranial electrodes or extracted (e.g., using a beamformer).

Note that a difference with DCM for evoked responses is that the parameters of the spatial model are not optimized. This means that DCM-IR will project the data into source space using the spatial locations you provide.

We will use the ECD option. This requires specifying a list of source names in the left large text box and a list of MNI coordinates for the sources in the right large text box. Enter the following in the left box:

```
    lOFA
    rOFA
    lFFA
    rFFA
```

Now enter in the right text box:

```
    -39 -81 -15
     42 -81 -15
    -39 -51 -24
     42 -45 -27
```

These correspond to left Occipital Face Area, right Occipital Face Area, left Fusiform Face Area, and right Fusiform Face Area, respectively. See (Chen, Kiebel, and Friston 2008) for more details.

The onset-parameter determines when the stimulus, presented at 0 ms peri-stimulus time, is assumed to trigger the cortical induced response. In DCM, we usually do not model the rather small early responses, but start modeling at the first large deflection. Because the propagation of the stimulus impulse through the input nodes causes a delay, we found that the default value of 60 ms onset time is a good value for many responses where the first large deflection is seen around 100 ms. However, this value is a prior, i.e., the inversion routine can adjust it. The prior mean should be chosen according to the specific responses of interest. This is because the time until the first large deflection is dependent on the paradigm or the modality you are working in, e.g., audition or vision. You may also find that changing the onset prior has an effect on how your data are fitted. This is because the onset time has strongly nonlinear effects (a delay) on the data, which might cause differences in the maximum found at convergence, for different prior values. Note, that it is possible to enter more than one number in the "onset\[s\] (ms)" box. This will add several inputs to the model. These inputs can then be connected to different nodes and/or their timing and frequency profiles can be optimized separately.

When you want to proceed to the next model specification stage, hit the $>$ (forward) button and proceed to the *neuronal model*. If you have not used the input dataset for 3D source reconstruction before, you will be asked to specify the parameters of the head model at this stage.

## Neuronal model

There are 4 (or more) matrices which you need to specify by button presses. In the first row, there are matrices that define the connectivity structure of the model and in the second row, there are matrices that specify which connections are affected by experimental effects. All the matrices except one are square. In each of these square matrices, you specify a connection *from* a source area *to* a target area. For example, switching on the element $(2,\;1)$ means that you specify a connection from area 1 to 2 (in our case from lOFA to rOFA). Some people find the meaning of each element slightly counter-intuitive, because the column index corresponds to the source area, and the row index to the target area. This convention is motivated by the direct correspondence between the matrices of buttons in the GUI and connectivity matrices in DCM equations and should be clear to anyone familiar with matrix multiplication.

The leftmost matrix in the first row specifies the *linear* connections. These are the connections where frequency dynamics in one source affect the dynamics at the same frequencies in another source. Note that all connections in the model should be at least linear, so if you think some connection should be present in the model, the corresponding button in this matrix should be on. Also, the buttons on the leading diagonal of the matrix are always on because each node in the model has a linear intrinsic connection with a negative sign. This means that the activity has a tendency to dissipate. To the right of the linear connectivity matrix, there is a *nonlinear* connectivity matrix. The idea here is the same, just remember to enable the corresponding linear connection as well. When a connection is nonlinear, a frequency mode in the source node can affect all the frequency modes in the target node. Intrinsic connections can be made non-linear as well. It is actually recommended to always make the intrinsic connections non-linear unless there is a good theoretical reason not to do it. Since we are mainly interested in non-linearities in the extrinsic connections, we would like to be over-conservative and first explain away anything that can be explained by non-linearities in the intrinsic connections.

The rightmost matrix in the first row is the input matrix. It is usually not square, and in the case of a single input, as we have here, is reduced to a column vector. The entries of this vector specify which areas receive the external input (whose onset time we specified above). In the case of several inputs, the input matrix will have several columns.

The matrix (matrices) in the second row specify which of the connections defined in the first row can be modified by experimental effects. A connection which is not modified will have the same value for all conditions. If you don't allow modification of any of the connections, then exactly the same model will be fitted to all conditions. For the purpose of allowing modification by experimental effects, it does not matter whether a connection is linear or non-linear. Hence, there is one modulation matrix per experimental effect (defined in the "Data and design" panel). In our case, there is only one effect - faces vs. scrambled faces. Also, self connections can be modified by experimental effects, thus the diagonal entries of the second row matrices can also be toggled.

Figure 1.3 is taken from the paper of Chen et al. (Chen et al. 2009) and shows several alternative models that could apply to the data. We will start by specifying the model with nonlinear forward and backward connections (FnBn) and with the effect of condition on these connections. The corresponding button configuration is shown in Figure 1.4. Compare the depiction of FnBn model in Figure 1.3 with the specification in Figure 1.4 to see the correspondence. Note that the effect of condition is not shown in Figure 1.3. Now copy the specification to the DCM GUI.

![Four different DCM-IR models proposed by Chen et al. (Chen et al. 2009)](../../../assets/figures/manual/dcm_ir/irfigure3.png)

*Four different DCM-IR models proposed by Chen et al. (Chen et al. 2009)*

![Connectivity configuration for the FnBn model](../../../assets/figures/manual/dcm_ir/irfigure4.png)

*Connectivity configuration for the FnBn model*

At the bottom of this panel, there are additional radio buttons for options that are not relevant for DCM-IR. Below these buttons there are controls for specifying the parameters of the wavelet transform for computing the time-frequency decomposition. We will keep the default frequency window 4 to 48 Hz and increase the number of wavelet cycles to 7. You can press the *Wavelet transform* button to preview the time-frequency plots and optimize the parameters if necessary before inverting the model.

## Estimation

When you have finished model specification, you can hit the *invert DCM* button in the lower left corner. DCM will now estimate the model parameters. You can follow the estimation process by observing the model fit in the output window. Note that in DCM-IR there is no difference between the hidden states and the predicted responses because the dynamics of the hidden states fit directly the time course of frequency modes (shown as dotted lines in the middle plot). This is different from DCM for ERP where the hidden states correspond to neural dynamics and a subset of the hidden states (activation of pyramidal cells) are projected via the forward model to generate predictions of sensor data. In the MATLAB command window, you will see each iteration print an expectation-maximization iteration number, free energy $F$, and the predicted and actual change of $F$ following each iteration step. At convergence, DCM saves the results in a DCM file, by default named `DCM_*.mat` where `*` corresponds to the name of the original SPM MEG file you specified. You can save to a different name, e.g. if you are estimating multiple models, by pressing 'save' at the top of the GUI and writing to a different name.

## Results

After estimation is finished, you can assess the results by choosing from the pull-down menu at the bottom (middle).

### Frequency modes

This will display the frequency profiles of the modes, identified using singular value decomposition of spectral dynamics in source space (over time and sources).

### Time modes

This will display the observed time courses of the frequency modes (dashed lines) and the model predictions (solid lines). Here you can also see whether the activity picked up by the minor modes is noise, which is helpful for optimizing the number of modes.

### Time-Frequency

This will display the observed time-frequency power data for all pre-specified sources (upper panel) and the fitted data features (lower panel).

### Coupling (A-Hz)

This will display the coupling matrices representing the coupling strength from source to target frequencies. These matrices are obtained by multiplying the between-mode matrices estimated with the frequency profiles of the modes (see (Chen, Kiebel, and Friston 2008)). The arrangement of the matrices corresponds to arrangements of the buttons in the connectivity matrices above.

### Coupling (B-Hz)

This presentation of results is similar to the above and reports modification of coupling by condition (eg. in our example it shows which frequency couplings are different for faces as opposed to scrambled faces).

### Coupling (A-modes)

This will display the coupling matrices between modes and the conditional probabilities that the coefficients are different from zero. This representation is useful for diagnostics when something is wrong with the inversion, but the physiological interpretation is less straightforward.

### Coupling (B-Hz)

This presentation is similar to the above and reports the modification of coupling by condition.

### Input (C-Hz)

This shows the frequency profiles of the inputs estimated. This is again a multiplication between the mode-specific coefficients and the frequency profiles of the modes.

### Input (u-ms)

This shows the time courses of the inputs.

### Dipoles

This shows the positions of the sources as specified in the "Electromagnetic model" section.

### Save as img

Here you can save the cross-frequency coupling matrices as images. If you are analyzing a group of subjects you can then enter these images into parametric statistical tests to find common features in coupling and coupling changes across subjects. The image names will include identifiers like "A12" or "B31" which relate to the source connection matrices; either the basic (A) or experimental effects (B).
## Model comparison

You can now compare the fully nonlinear model with alternative models (eg. those shown in [Figure 1.3](#dcm-ir:fig:3)). You can start by saving the DCM you have already specified under a different name using the *Save* button. Then just modify the connectivity matrices and reinvert the DCM by pressing the "Estimated" button (but not using previous posterior or prior estimates). As an exercise, you can specify the other models from [Figure 1.3](#dcm-ir:fig:3) yourself. If in doubt look at [Figure 1.5](#dcm-ir:fig:5) for the three alternative models. Once you have specified and inverted the three additional models, you can perform Bayesian model comparison.

![Connectivity configurations for the alternative models. Left to right: FlBl, FlBn, FnBl.](../../../assets/figures/manual/dcm_ir/irfigure5_FlBl.png)

Press the <span class="smallcaps">BMS</span> button. This will open the SPM batch tool for model selection. Specify a directory to write the output file to. For the "Inference method" select "Fixed effects" (see (Stephan et al. 2009) for additional explanations). Then click on "Data" and in the box below click on "New: Subject". Click on "Subject" and in the box below on "New: Session". Click on models and in the selection window that comes up select the DCM mat files for all the models (remember the order in which you select the files as this is necessary for interpreting the results). Then run the model comparison by pressing the green "Run" button. You will see, at the top, a bar plot of the log-model evidences for all models [1.6](#dcm-ir:fig:6). At the bottom, you will see the posterior probability, for each model, given the data. By convention, a model can be said to be the best among a selection of other models, with strong evidence, if its log-model evidence exceeds all other log-model evidences by at least 3. In our case the FnBn model is superior to the other models as was found in the original paper (Chen et al. 2009) for a different group of subjects.

![Bayesian comparison of the four DCM-IR models shown in Figure 1.3.](../../../assets/figures/manual/dcm_ir/irfigure6.png)

### References

Chen, C. C., R. N. A. Henson, K. E. Stephan, J. Kilner, and K. J. Friston. 2009. "Forward and Backward Connections in the Brain: A DCM Study of Functional Asymmetries." *NeuroImage* 45 (2): 453--62. <https://doi.org/10.1016/j.neuroimage.2008.12.041>.

Chen, C. C., S. J. Kiebel, and K. J. Friston. 2008. "Dynamic Causal Modelling of Induced Responses." *NeuroImage* 41 (4): 1293--1312. <https://doi.org/10.1016/j.neuroimage.2008.03.026>.

Stephan, K. E., W. D. Penny, J. Daunizeau, R. Moran, and K. J. Friston. 2009. "Bayesian Model Selection for Group Studies." *
