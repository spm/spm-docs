# DCM for fNIRS <span id="Chap:data:fnirs" label="Chap:data:fnirs"></span>

Functional near-infrared spectroscopy (fNIRS) is a noninvasive method
for monitoring hemodynamic changes in the brain via measurements of
optical absorption changes (Jobsis 1977). As neuronal process requires
extra delivery of oxygen, this hemodynamic change provides a marker of
underlying neuronal activity.  
As compared to fMRI, fNIRS provides a more direct measure of changes in
oxygenated, deoxygenated, and total hemoglobin (HbO, HbR, and HbT) with
high temporal resolution. However, due to complex relationships between
neurodynamic and hemodynamic processes, functional connectivity often
differs with the type of hemodynamic changes (e.g., HbO and HbR), while
underlying interactions between neuronal populations do not vary. This
highlights the importance of estimating causal influences among neuronal
populations, referring to a biophysical model of how neuronal activity
is transformed into fNIRS measurements.  
DCM for fNIRS allows for making inference about changes in directed
connectivity at the neuronal level from the measurement of optical
density changes (Tak et al. 2015). DCM is a framework for fitting
differential equation models of neuronal activity to brain imaging data
using Bayesian inference (Friston, Harrison, and Penny 2003; Friston et
al. 2007; Penny et al. 2010). Because the Bayesian estimation is the
same as the one used for DCMs for other imaging modalities (Friston et
al. 2007; Penny et al. 2010), the main differences are a generative
model describing how observed fNIRS data are caused by the interactions
among hidden neuronal states.  
In particular, the neurodynamic and hemodynamic models used for DCM-fMRI
analysis (Friston, Harrison, and Penny 2003) are extended to
additionally include the following components:

- In the hemodynamic model, *the rate of HbT changes* $\dot{p}_j$, (Cui,
  Bray, and Reiss 2010), is modeled as: $$\begin{aligned}
  \tau_j \dot{p}_j  =  \left(f_{j,in} - f_{j,out}\right)\frac{p_j}{v_j},
  \end{aligned}$$ where $j$ denotes cortical source region, $f_{j,in}$
  is inflow, $f_{j,out}$ is outflow, $v_j$ is blood volume, and $\tau_j$
  is the transit time.

- *An optics model* relates the hemodynamic sources to measurements of
  optical density changes (Delpy et al. 1988; Arridge 1999). Optical
  density changes at channel $i$ and wavelength $\lambda$,
  $y_i(\lambda)$, are described as a linear combination of light
  absorption changes due to hemoglobin oxygenation: $$\begin{aligned}
  \label{eq:optics} 
  y_i (\lambda) = \frac{1}{\omega_{i,H}} \sum_{i=1}^{N} S_{i,j}(\lambda)\epsilon_H(\lambda) \Delta H_{j,c} +  \frac{1}{\omega_{i,Q}} \sum_{i=1}^{N} S_{i,j}(\lambda)\epsilon_Q(\lambda) \Delta Q_{j,c}
  \end{aligned}$$ where $\Delta H_{j,c}$ and $\Delta Q_{j,c}$ are the
  HbO and HbR changes in the cortical source region $j$; $S(\lambda)$ is
  the sensitivity matrix at wavelength $\lambda$; $\epsilon_H$ and
  $\epsilon_Q$ are the extinction coefficients for HbO and HbR;
  $\omega = \frac{\mathrm{cortical}}{\mathrm{cortical} + \mathrm{pial}}$
  is a factor for correcting the effect of pial vein oxygenation changes
  on fNIRS measurements (Gagnon et al. 2012).

- *Spatially extended hemodynamic sources*. In the optics model
  (Eq. <a href="#eq:optics" data-reference-type="ref"
  data-reference="eq:optics">[eq:optics]</a>), each hemodynamic signal
  is modelled as a point source which produces a hemodynamic response at
  a single specified anatomical location. However, the hemodynamic
  response can be spatially extended. Hemodynamic activity at a source
  location is then specified by convolving the temporal response with a
  Gaussian spatial kernel.

## Example: Motor Execution and Imagery Data

This section presents an illustrative DCM analysis using fNIRS data
acquired during the motor execution and motor imagery tasks. This data
was collected by Agnieszka Kempny and Alex Leff in Royal Hospital for
Neuro-disability, and is available from the SPM website[^1].

### Experimental protocol

- In the first session, the subject was instructed to squeeze and
  release a ball with her right hand during task blocks. In the second
  session, the subjected was instructed to perform kinesthetic imagery
  of the same hand movement, but without moving the hand.

- For both sessions, there were 5 second blocks of tasks where the cue
  was presented with an auditory beep, interspersed with 25 second rest
  blocks.

- A previous study found consistent activation in the supplementary
  motor area (SMA) and premotor cortex during motor execution and
  imagery, and reduced activation in primary motor cortex (M1) during
  motor imagery (Hanakawa et al. 2003). Moreover, a recent study has
  revealed, using DCM for fMRI (Kasess et al. 2008), that coupling
  between SMA and M1 may serve to attenuate the activation of M1 during
  motor imagery.

- In this manual, using DCM-fNIRS, we investigate (i) how the motor
  imagery condition affects the directed connections between SMA and M1,
  and (ii) how these interactions are associated with the regional
  activity in M1 and SMA during motor execution and imagery.

## SPM Analysis

Prior to DCM analysis, brain regions whose dynamics are driven by
experimental conditions are identified using a standard SPM
analysis[^2]. Using motor execution and imagery data, we can find that
SMA is significantly activated during both motor execution and motor
imagery, whereas M1 is only activated during motor execution. The most
significantly activated voxels within SMA and M1 are then selected as
the source positions for DCM analysis: The MNI coordinates are: SMA,
$[51, 4, 55]$; and M1, $[44, 16, 65]$.

## Specifying and Estimating the DCM

Now we will generate a connectivity model of how observed fNIRS data are
caused by interactions among hidden neuronal states. Specifically, we
will determine (i) which regions receive driving input, (ii) which
regions are connected in the absence of experimental input, and (iii)
which connections are modulated by input. In this example, we will
generate the best model (selected using Bayesian model comparison) in
which task input could affect regional activity in both M1 and SMA, and
motor imagery modulates both extrinsic and intrinsic connections.

1.  After starting SPM in the EEG modality, add the DCM for fNIRS
    toolbox to the MATLAB path:  
    \>\> `addpath(fullfile(spm(’Dir’),’toolbox’,’dcm_fnirs’))`

2.  Type `spm_dcm_fnirs_specify` at the MATLAB command window.  
    \>\> `spm_dcm_fnirs_specify`;

3.  Select the `SPM.mat` files that were created when specifying the GLM
    (two files).  
    eg, `fnirs_data/ME/SPM.mat`, and `fnirs_data/MI/SPM.mat`.

4.  Highlight 'name for DCM???.mat' and enter a DCM file name you want
    to create eg, `fully_connected`.

5.  Select a directory, eg. `fnirs_data`, in which to place the results
    of your DCM analysis.

6.  **Preprocess time series of optical density changes**

    1.  Highlight 'temporal filtering? \[Butterworth IIR/No\]'.  
        Select the Butterworth IIR button to reduce very low-frequency
        confounds, and physiological noise eg, respiration and cardiac
        pulsation.

        - Highlight 'stopband frequencies Hz \[start end\]', and then
          accept the default value '\[0 0.008; 0.12 0.35; 0.7 1.5\]'.

    2.  Highlight 'average time series over trials?'.  
        Select 'yes' to average the optical density signal across
        trials, and then specify averaging window length \[secs\] for
        each experimental condition.

        - Highlight 'for motor execution:' and change the default values
          30.2683 to 30.

        - Highlight 'for motor imagery:' and change the default values
          30.6624 to 30.

7.  **Specify confounding effects**

    1.  Highlight 'regressors? \[DCT/User/None\]'.  
        Select 'DCT' to model confounding effects (eg, Mayer waves)
        using a discrete cosine transform set.

        - Highlight 'periods \[sec\]', and then accept the default value
          '\[8 12\]'.

    2.  Highlight 'correction of pial vein contamination? \[yes/no'\].  
        Select 'yes' to correct pial vein contamination of fNIRS
        measurements.

8.  **Specify fNIRS channels to be used in DCM**.

    1.  Highlight 'use all sensor measurements? \[yes/no\]'.  
        Select 'no' to specify sensor (channel) measurements to be used
        in DCM analysis.

        - Highlight 'sensors of interest', and enter eg, \[1 2 3 4 9 10
          11 12\]  
          Only fNIRS data from the left hemisphere is used in this
          study. You can refer to sensor positions shown in 'Figure 1:
          Optode and Channel Positions'.

9.  **Specify hemo/neurodynamic source positions**.

    1.  Enter total number of sources to be estimated using DCM eg, 2.

        - Enter a name of source 1 eg, M1

        - Enter MNI coordinates of source 1 eg, \[$-$<!-- -->44
          $-$<!-- -->16 65\]

        - Enter a name of source 2 eg, SMA

        - Enter MNI coordinates of source 2 eg, \[$-$<!-- -->51
          $-$<!-- -->4 55\]

    2.  Highlight 'type of hemodynamic source \[Point/Distributed\]'  
        Select 'Distributed' to use spatially extended hemodynamic
        source rather than point source.

        - Highlight 'radius of distributed source \[mm\]', and accept
          the default value '4'.

10. Select a file containing Green's function of the photon fluence
    precomputed using the mesh-based Monte Carlo simulation software
    (MMC, (Fang 2010)[^3] eg, `fnirs_data/greens/est_G.mat`.

11. **Input Specification**  
    Specify experimental input that modulates regional activity and
    connectivity.

    1.  Highlight 'number of conditions/trials', and then enter '2'.

    2.  Highlight 'name for condition/trial1?', and enter 'motor
        task'.  
        'motor task' encodes the occurrence of motor execution and motor
        imagery tasks.

        - Highlight 'onsets - motor task \[sec\]', and enter '\[0 30\]'.

        - Highlight 'duration\[s\] \[sec\]', and enter '5'.

    3.  Highlight 'name for condition/trial2?', and enter 'motor
        imagery'.

        - Highlight 'onsets - motor task \[sec\]', and enter '30'.

        - Highlight 'duration\[s\] \[sec\]', and enter 'Inf'.

12. **Specify endogenous (fixed) connections**

    - Define connections in the absence of input: M1 to SMA, SMA to
      M1.  
      Your connectivity matrix should look like the one in
      Fig. <a href="#fig:gui_con" data-reference-type="ref"
      data-reference="fig:gui_con">1.1</a>.

13. **Specify regions receiving driving input**

    - Highlight 'Effects of motor tasks on regions and connections', and
      specify motor tasks as a driving input into M1 and SMA. See
      Fig. <a href="#fig:gui_con" data-reference-type="ref"
      data-reference="fig:gui_con">1.1</a>.

14. **Specify connections modulated by input**

    - Highlight 'Effects of motor imagery on regions and connections.  
      Specify motor imagery to modulate intrinsic connections on M1 and
      SMA; extrinsic connections from M1 to SMA, and from SMA to M1. See
      Fig. <a href="#fig:gui_con" data-reference-type="ref"
      data-reference="fig:gui_con">1.1</a>.

    A polite "Thank You" completes the model specification process. A
    file called `DCM_fully_connected.mat` will have been generated.

15. You can now estimate the model parameters by typing  
    \>\> `spm_dcm_fnirs_estimate`;

16. Select the DCM.mat file created in the last step eg,
    `fnirs_data/DCM_fully_connected.mat`. After convergence of
    estimation of DCM parameters, results will appear in a window, as
    shown in Fig. <a href="#fig:fit" data-reference-type="ref"
    data-reference="fig:fit">1.2</a>.

    - Also, you can display DCM analysis results, including connectivity
      parameters, estimated neural responses, and DCM fit, by typing  
      \>\> `spm_dcm_fnirs_viewer_result`;

    - Select the DCM.mat file eg, `fnirs_data/`, and then results will
      appear in a window, as shown in
      Figs. <a href="#fig:est_conn" data-reference-type="ref"
      data-reference="fig:est_conn">1.3</a> and
      <a href="#fig:est_neural" data-reference-type="ref"
      data-reference="fig:est_neural">1.4</a>.

<figure id="fig:gui_con">
<div class="center">

</div>
<figcaption>Specification of model depicted in Fig.7 <span
class="citation" data-cites="tak2015dynamic">(Tak et al. 2015)</span>.
In this model, motor task input affects regional activity in both M1 and
SMA, and motor imagery modulates both extrinsic and intrinsic
connections. Left: Endogenous (fixed) connections. Middle: Regions
receiving driving input (eg, motor tasks encoding the occurrence of
motor execution and motor imagery tasks). Right: Connections modulated
input (eg, motor imagery task).<span id="fig:gui_con"
label="fig:gui_con"></span></figcaption>
</figure>

<figure id="fig:fit">
<div class="center">

</div>
<figcaption>Results of estimation of DCM parameters. Top: Predicted and
observed response, after convergence. Bottom: Conditional expectation of
the parameters.<span id="fig:fit" label="fig:fit"></span></figcaption>
</figure>

<figure id="fig:est_conn">
<div class="center">

</div>
<figcaption>Estimated parameters of effective connectivity. These
results indicate that while all motor stimuli positively affect
theregional activity in primarymotor cortex (M1), motor imagery
negativelymodulates connection fromsupplementarymotor area (SMA) to M1,
resulting in the suppressive influence of SMA on M1. See <span
class="citation" data-cites="tak2015dynamic">(Tak et al. 2015)</span>
for more details. <span id="fig:est_conn"
label="fig:est_conn"></span></figcaption>
</figure>

<figure id="fig:est_neural">
<div class="center">

</div>
<figcaption>Estimated neural responses. Duringmotor imagery, neural
activity in primary motor cortex (M1) is significantly reduced, while
neural activity in supplementary motor area (SMA) is relatively
consistent, compared with activity during motor execution.<span
id="fig:est_neural" label="fig:est_neural"></span></figcaption>
</figure>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-arridge1999optical" class="csl-entry">

Arridge, Simon R. 1999. "<span class="nocase">Optical tomography in
medical imaging</span>." *Inverse Probl.* 15: R41--93.

</div>

<div id="ref-cui10" class="csl-entry">

Cui, X, S Bray, and A. L. Reiss. 2010. "<span class="nocase">Functional
near infrared spectroscopy (NIRS) signal improvement based on negative
correlation between oxygenated and deoxygenated hemoglobin
dynamics</span>." *NeuroImage* 49: 3039--46.

</div>

<div id="ref-delpy1988estimation" class="csl-entry">

Delpy, D. T., M. Cope, P. Van der Zee, S. Arridge, S. Wray, and J.
Wyatt. 1988. "Estimation of Optical Pathlength Through Tissue from
Direct Time of Flight Measurement." *Phys. Med. Biol.* 33 (12):
1433--42.

</div>

<div id="ref-fang2010mesh" class="csl-entry">

Fang, Qianqian. 2010. "<span class="nocase">Mesh-based Monte Carlo
method using fast ray-tracing in Pl<span class="nocase">ü</span>cker
coordinates</span>." *Biomed. Opt. Express* 1: 165--75.

</div>

<div id="ref-dcm" class="csl-entry">

Friston, K. J., L. Harrison, and W. D. Penny. 2003. "Dynamic Causal
Modelling." *NeuroImage* 19 (4): 1273--1302.

</div>

<div id="ref-karl_vb_laplace" class="csl-entry">

Friston, K. J., J. Mattout, N. Trujillo-Bareto, J. Ashburner, and W. D.
Penny. 2007. "Variational Free Energy and the Laplace Approximation."
*NeuroImage* 34 (1): 220--34.
<https://doi.org/10.1016/j.neuroimage.2006.08.035>.

</div>

<div id="ref-gagnon2012quantification" class="csl-entry">

Gagnon, Louis, Meryem A Yücel, Mathieu Dehaes, Robert J Cooper,
Katherine L Perdue, Juliette Selb, Theodore J Huppert, Richard D Hoge,
and David A Boas. 2012. "<span class="nocase">Quantification of the
cortical contribution to the NIRS signal over the motor cortex using
concurrent NIRS-fMRI measurements</span>." *Neuroimage* 59 (4):
3933--40.

</div>

<div id="ref-hanakawa2003functional" class="csl-entry">

Hanakawa, Takashi, Ilka Immisch, Keiichiro Toma, Michael A Dimyan, Peter
Van Gelderen, and Mark Hallett. 2003. "<span class="nocase">Functional
properties of brain areas associated with motor execution and
imagery</span>." *J. Neurophysiol.* 89: 989--1002.

</div>

<div id="ref-jobsis1977noninvasive" class="csl-entry">

Jobsis, Frans F. 1977. "Noninvasive, Infrared Monitoring of Cerebral and
Myocardial Oxygen Sufficiency and Circulatory Parameters." *Science* 198
(4323): 1264--67.

</div>

<div id="ref-kasess2008suppressive" class="csl-entry">

Kasess, Christian H, Christian Windischberger, Ross Cunnington, Rupert
Lanzenberger, Lukas Pezawas, and Ewald Moser. 2008.
"<span class="nocase">The suppressive influence of SMA on M1 in motor
imagery revealed by fMRI and dynamic causal modeling</span>."
*Neuroimage* 40 (2): 828--37.

</div>

<div id="ref-dcm_families" class="csl-entry">

Penny, W. D., K. E. Stephan, J. Daunizeau, M. J. Rosa, K. J. Friston, T.
M.Schofield, and A. P. Leff. 2010. "Comparing Families of Dynamic Causal
Models." *PLoS Comput Biol* 6 (3): e1000709.
<https://doi.org/10.1371/journal.pcbi.1000709>.

</div>

<div id="ref-tak2015dynamic" class="csl-entry">

Tak, Sungho, A. M. Kempny, K. J. Friston, A. P. Leff, and W. D. Penny.
2015. "Dynamic Causal Modelling for Functional Near-Infrared
Spectroscopy." *NeuroImage* 111: 338--49.
<https://doi.org/10.1016/j.neuroimage.2015.02.035>.

</div>

</div>

[^1]: <http://www.fil.ion.ucl.ac.uk/spm/data/fnirs/>

[^2]: The first-level fNIRS analysis. including preprocessing, artefact
    removal, general linear modelling, spatial registration were all
    implemented using the SPM for fNIRS toolbox. This is available from
    <https://www.nitrc.org/projects/spm_fnirs/.> See the toolbox manual
    for more details.

[^3]: We provide a code for estimating Green's function based on the MMC
    software. The code currently only runs on Mac OS.
