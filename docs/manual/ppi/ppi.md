# Psychophysiological Interactions (PPI)<span id="Chap:data:ppi" label="Chap:data:ppi"></span>

## Theoretical background

Psychophysiological interactions (PPI) and the related technique of
physiophysiological interactions $(\Phi$PI) are based on extensions to
statistical models of factorial designs. Table 1 illustrates a classic
$2 \times 2$ factorial design.

<table>
<caption>2 x 2 factorial design in Table format</caption>
<tbody>
<tr class="odd">
<td style="text-align: left;"><p>&amp;</p>
<p>&amp;</p></td>
<td style="text-align: left;"></td>
<td style="text-align: left;"></td>
<td style="text-align: left;"></td>
</tr>
</tbody>
</table>

2 x 2 factorial design in Table format

The equation for factorial design is given by
<a href="#eq:ppi1" data-reference-type="ref"
data-reference="eq:ppi1">[eq:ppi1]</a>.

$$\mbox{$y=(A_{2}-A_{1})\beta_{1}+(B_{2}-B_{1})\beta_2+(A_{2}-A_{1})(B_{2}-B_{1})\beta_{3}+G\beta_{4} +\epsilon\quad$}
    \label{eq:ppi1}$$

Notice that this equation includes both of the main effects terms
$(A_{2}-A_{1})\beta_{1}$ for factor A, and $(B_{2}-B_{1})\beta_{2}$ for
factor B, as well as the interaction term
$(A_{2}-A_{1})(B_{2}-B_{1})\beta_{3}$. It also contains a term for the
confounds $G\beta_{4}$ such as movement parameters, session effects,
etc. The inclusion of main effects when estimating interactions is very
important, and their inclusion in the design cannot be stressed enough.
If the main effects are not included, then we cannot be sure that
estimates of the interaction term are not confounded by main effects.

To extend the concept of factorial designs to PPI's the basic idea is to
substitute (neural) activity from one cerebral region for one of the
factors. Equation <a href="#eq:ppi2" data-reference-type="ref"
data-reference="eq:ppi2">[eq:ppi2]</a> illustrates this concept after
substituting activity in area V1 for factor A.

$$\mbox{$y=V1\beta_{1}+(B_{2}-B_{1})\beta_2+(V1\times(B_{2}-B_{1}))\beta_3+G\beta_4 +\epsilon\quad$}
    \label{eq:ppi2}$$

Similarly, for psychophysiological interactions activity from 2 cerebral
regions (V1 and posterior parietal (PP)) are used as the main effects,
as shown in equation <a href="#eq:ppi3" data-reference-type="ref"
data-reference="eq:ppi3">[eq:ppi3]</a>

$$\mbox{$y=V1\beta_{1}+PP\beta_2+(V1\times PP)\beta_3+G\beta_4 +\epsilon\quad$}
    \label{eq:ppi3}$$

Again, notice that all 3 equations
<a href="#eq:ppi1" data-reference-type="ref"
data-reference="eq:ppi1">[eq:ppi1]</a>,
<a href="#eq:ppi2" data-reference-type="ref"
data-reference="eq:ppi2">[eq:ppi2]</a> and
<a href="#eq:ppi3" data-reference-type="ref"
data-reference="eq:ppi3">[eq:ppi3]</a> have 3 terms (aside from
confounds and error) -- the two main effects and the interaction.
Therefore, the design matrix must include at least 3 columns, one for
each main effect and one for the interaction. A basic design matrix for
PPI's is shown in Figure <a href="#fig:ppi1" data-reference-type="ref"
data-reference="fig:ppi1">1.1</a>.

<figure id="fig:ppi1">
<img src="../../../assets/figures/manual/ppi/Fig1.png" style="width:85mm" />
<figcaption><em>Example design matrix for a PPI (or <span
class="math inline">(<em>Φ</em></span>PI)). The main effects are BOLD
activity from area V1, in column 2, and a psychological vector, e.g.,
attention vs. no attention (P), in column 3. Inference would typically
focus on the interaction term, in column 1, using a contrast vector of
<span class="smallcaps">[1 0 0 0]</span>. In <span
class="math inline"><em>Φ</em></span>PIs the third column would be BOLD
activity from a second source region rather than the psychological
factor.</em></figcaption>
</figure>

Both PPIs and $\Phi$PIs can be conceived of as models of "contribution".
PPIs occupy middle-ground between between models of functional vs.
effective connectivity (Friston et al. 1997). Functional connectivity
(FC) is defined as the temporal correlation between spatially separated
neurophysiological events (Friston et al. 1997). FC analyses are
typically model-free and do not specify a direction of influence, i.e.,
the influence of A on B is indistinguishable from the influence of B on
A. In contrast, PPI's are based on regression models, and therefore a
direction of influence is chosen based on the model. Effective
connectivity (EC) is defined as the influence one neural system has on
another (Friston 1995). PPIs are closely related to EC models, but
because PPIs are generally very simple (i.e., 1 source region and 1
experimental factor, or 2 source regions in the case of $\Phi$PIs) they
are very limited models of EC.

The interaction between the source region and experimental context (or
two source regions) can be interpreted in 2 different ways: 1) as
demonstrating how the contribution of one region to another is altered
by the experimental context or task, or 2) as an example of how an
area's response to an experimental context is modulated by input from
another region, Figure <a href="#fig:ppi2" data-reference-type="ref"
data-reference="fig:ppi2">1.2</a>.  

<figure id="fig:ppi2">
<img src="../../../assets/figures/manual/ppi/Fig2.png" style="width:120mm" />
<figcaption>Two alternative interpretations of PPI effects. A) The
contribution of one area (k) to another (i) is altered by the
experimental (psychological) context. B) The response of an area (i) to
an experimental (psychological) context due to the contribution of
region (k). (Adapted from <span class="citation"
data-cites="ppi">(Friston et al. 1997)</span>)</figcaption>
</figure>

## Psycho-Physiologic Interaction Analysis: Summary of Steps

Mechanistically, a PPI analysis involves the following steps.

1.  Performing a standard GLM analysis.

2.  Extracting BOLD signal from a source region identified in the GLM
    analysis.

3.  Forming the interaction term (source signal x experimental
    treatment)

4.  Performing a second GLM analysis that includes the interaction term,
    the source region's extracted signal and the experimental vector in
    the design. The inclusion of the source region's signal and the
    experimental vector is analogous to including the main effects in an
    ANOVA in order to make an inference on the interaction.

Forming the proper interaction term turns out to be a challenge because
of the unique characteristics of fMRI (BOLD) data in which the
underlying neural signal is convolved with a hemodynamic response
function. However, interactions in the brain take place at the neural
and not the hemodynamic level. Therefore, appropriate models of the
interactions require the neural signal, which is not measured directly,
but instead must be derived by deconvolving the HRF. The PPI software
(`spm_peb_ppi.m`) was developed in order to provide robust deconvolution
of the HRF and the proper derivation of the interaction term (Gitelman
et al. 2003).

## Practical example

The dataset in this exercise is from one subject who was studied in the
(Buchel et al. 1998) report and refers to the "attention to motion"
dataset available from the SPM website[^1]. It has already been
described in the previous chapter for DCM.

The goal is to use PPI to examine the change in effective connectivity
between V2 and V5 while the subject observes visual motion (radially
moving dots) under the experimental treatments of attending vs. not
attending to the speed of the dots. The psychophysiologic interaction
can be conceived of as looking for a significant difference in the
regression slopes of V1 vs. V5 activity under the influence of the
different attentional states (Friston et al. 1997).

### GLM analysis - Design setup and estimation

This dataset has already been preprocessed (coregistered, normalised and
smoothed) using an earlier version of SPM.

1.  The analysis directory should include

    1.  A directory named `functional`, which includes the preprocessed
        fMRI volumes.

    2.  A directory named `structural`, which includes a T1 structural
        volume

    3.  Files: `factors.mat`, `block_regressors.mat`,
        `multi_condition.mat` and `multi_block_regressors.mat`.

    4.  You will also need to make 2 empty directories called `GLM` and
        `PPI` for performing the analyses.

2.  In MATLAB type

        >> cd GLM
        >> spm fmri

3.  Start the Batch system by clicking the
    <span class="smallcaps">Batch</span> button.

4.  From the <span class="smallcaps">SPM</span> menu in the Batch
    window, click <span class="smallcaps">Stats</span> and then select
    the modules <span class="smallcaps">fMRI Model Specification</span>,
    <span class="smallcaps">Model Estimation</span> and
    <span class="smallcaps">Contrast Manager</span>,
    Figure <a href="#fig:ppi3" data-reference-type="ref"
    data-reference="fig:ppi3">1.3</a>.

    <figure id="fig:ppi3">
    <img src="../../../assets/figures/manual/ppi/Fig3.png" style="width:120mm" />
    <figcaption>Batch Editor showing the <span class="smallcaps">fMRI Model
    Specification</span>, <span class="smallcaps">Model Estimation</span>
    and <span class="smallcaps">Contrast Manager</span>
    modules.</figcaption>
    </figure>

    **Fill in the <span class="smallcaps">fMRI Model
    Specification</span>**

5.  Click <span class="smallcaps">Directory</span> and choose the `GLM`
    directory that you made above.

6.  <span class="smallcaps">Units for design</span>
    \[<span class="smallcaps">scans</span>\]

7.  <span class="smallcaps">Interscan interval</span> \[3.22\]

8.  Click <span class="smallcaps">Data & Design</span>. Then in the
    <span class="smallcaps">Current Item</span> box click
    <span class="smallcaps">New: Subject/Session</span>,
    Figure <a href="#fig:ppi4" data-reference-type="ref"
    data-reference="fig:ppi4">1.4</a>.

    <figure id="fig:ppi4">
    <img src="../../../assets/figures/manual/ppi/Fig4.png" style="width:120mm" />
    <figcaption><em>Fill in the Data &amp; Design</em></figcaption>
    </figure>

9.  Click <span class="smallcaps">Scans</span> and choose all the
    functional scans `snffM00587_00xx.img`. There should be 360 `*.img`
    files.

10. The experimental conditions can be defined either individually or
    using a multiple condition `mat`-file. This exercise shows both
    methods for educational purposes. When doing an actual analysis you
    can just follow one of the two approaches below.  
      
    **Define conditions individually**

11. Load the mat file containing the individual conditions:

        >> load factors.mat

    You can look at the loaded variables by typing the variable names. (
    `stat` = stationary, `natt` = no attention, `att` = attention)

        >> stat
        >> natt
        >> att

12. Click <span class="smallcaps">Conditions</span> then in the
    <span class="smallcaps">Current Item</span> box click
    <span class="smallcaps">New: Condition</span> 3 times,
    Figure <a href="#fig:ppi5" data-reference-type="ref"
    data-reference="fig:ppi5">1.5</a>.

    <figure id="fig:ppi5">
    <img src="../../../assets/figures/manual/ppi/Fig5.png" style="width:120mm" />
    <figcaption><em><span class="smallcaps">Current Module</span> section of
    the <span class="smallcaps">Batch Editor</span> showing 3 Conditions to
    be filled in.</em></figcaption>
    </figure>

13. Condition 1: Name = `Stationary`,
    <span class="smallcaps">Onsets</span> = `stat`,
    <span class="smallcaps">Durations</span> = 10.

14. Condition 2: Name = `No-attention`,
    <span class="smallcaps">Onsets</span> = `natt`,
    <span class="smallcaps">Durations</span> = 10.

15. Condition 3: Name = `Attention`,
    <span class="smallcaps">Onsets</span> = `att`,
    <span class="smallcaps">Durations</span> = 10.

16. Next you will enter 3 regressors to model block effects. This will
    account for the fact that the experiment took place over 4 runs that
    have been concatenated into a single session to make the PPI
    analysis easier. *Note: Only 3 of the 4 sessions need to be modeled
    by block regressors because the fourth one is modeled by the mean
    column of the design matrix.*

    First load the regressors:

        >> load block_regressor.mat

17. Click <span class="smallcaps">Regressors</span> then click
    <span class="smallcaps">New: Regressor</span> 3 times in the
    <span class="smallcaps">Current Item</span> box,
    Figure <a href="#fig:ppi6" data-reference-type="ref"
    data-reference="fig:ppi6">1.6</a>.

    <figure id="fig:ppi6">
    <img src="../../../assets/figures/manual/ppi/Fig6.png" style="width:120mm" />
    <figcaption><em><span class="smallcaps">Current Module</span> section of
    the <span class="smallcaps">Batch Editor</span> showing 3 Regressors to
    be filled in.</em></figcaption>
    </figure>

18. Regressor 1: <span class="smallcaps">Name</span> = `Block 1`,
    <span class="smallcaps">Value</span> = `block1`

19. Regressor 2: <span class="smallcaps">Name</span> = `Block 2`,
    <span class="smallcaps">Value</span> = `block2`

20. Regressor 3: <span class="smallcaps">Name</span> = `Block 3`,
    <span class="smallcaps">Value</span> = `block3`  
      
    **Define conditions using multiple condition and multiple regressor
    files**

21. If you would like to look at the organization of the variables in
    the multiple condition file, first load it.

        >> load multi_condition.mat
        >> names
        >> onsets
        >> durations

    The variables in a multiple condition file must always be named:
    'names', 'onsets', and 'durations'. Notice that these three
    variables are cell arrays. *(Note: You only need to do this step if
    you want to look at the organization of the variables. In contrast
    to defining conditions individually, as shown above, when using a
    multiple condition file you do not have to load the file in order to
    enter it into the design.)*  

22. To use the multiple conditions file in the design, click
    <span class="smallcaps">Multiple Conditions</span>, then
    <span class="smallcaps">Specify Files</span> in the Options box and
    choose the `multi_condition.mat` file.

23. Next you will enter the 3 regressors to model block effects by using
    a multiple regressor file. To look at the organization of the
    multiple regressor variable, first load it. *(Again you do not have
    to load the multiple regressor file in order to use it. This step is
    just to allow you to examine the file and the variables it
    contains.)*

        >> load multi_block_regressor.mat
        >> R

    Notice that this file contains a single variable, `R`, which is a
    360 x 3 matrix. The number of rows is equal to the number of scans,
    and each regressor is in a separate column.

24. To use the multiple regressor file, click
    <span class="smallcaps">Multiple Regressors</span> then select the
    `multi_block_regressor.mat` file.  
      
    **Complete the design setup**

25. <span class="smallcaps">High-pass filter</span> \[192\] (Note: most
    designs will use a high-pass filter value of 128. However, this
    dataset requires a longer high-pass filter in order not to lose the
    low frequency components of the design.)

26. <span class="smallcaps">Factorial design</span> is not used

27. The <span class="smallcaps">Basis function</span> is the
    <span class="smallcaps">canonical HRF</span> as shown and
    <span class="smallcaps">Model derivatives</span>
    \[<span class="smallcaps">No derivatives</span>\]

28. <span class="smallcaps">Model Interactions (Volterra)</span>:
    \[<span class="smallcaps">Do not model interactions</span>\]

29. <span class="smallcaps">Global normalisation</span>
    \[<span class="smallcaps">None</span>\]

30. <span class="smallcaps">Explicit mask</span>
    \[<span class="smallcaps">None</span>\]

31. <span class="smallcaps">Serial correlations</span>
    \[<span class="smallcaps">AR(1)</span>\]  
      
    **Model Estimation**

32. Under <span class="smallcaps">Model estimation</span> click
    <span class="smallcaps">Select `SPM.mat`</span> then click the
    <span class="smallcaps">Dependency</span> button and choose
    <span class="smallcaps">fMRI model specification: SPM.mat
    File</span>. The <span class="smallcaps">Method</span> should be
    left as Classical.  
      
    **Contrast Manager**

33. Under <span class="smallcaps">Contrast Manager</span> click
    <span class="smallcaps">Select `SPM.mat`</span> then click the
    <span class="smallcaps">Dependency</span> button and choose
    <span class="smallcaps">Model estimation: SPM.mat File</span>

34. Click <span class="smallcaps">Contrast Sessions</span> then click
    <span class="smallcaps">New: F-contrast</span> once, and
    <span class="smallcaps">New: T-contrast</span> twice from the
    <span class="smallcaps">Current Item</span> box.

35. Select <span class="smallcaps">Weights matrix</span> underneath
    <span class="smallcaps">F-contrast</span>.

36. The F weights matrix (named "effects of interest") can be entered as
    \[eye(3), zeros(3,4)\], which will produce:

        1 0 0 0 0 0 0
        0 1 0 0 0 0 0
        0 0 1 0 0 0 0

37. For the first T-contrast, <span class="smallcaps">Name</span> is
    `Attention`, and the <span class="smallcaps">T weights vector</span>
    is ` 0 -1 1 0 0 0 0` (Note the order of the conditions in the design
    matrix is: Stationary, NoAttMot and AttMot).

38. For the second T-contrast <span class="smallcaps">Name</span> is
    `Motion`, and the <span class="smallcaps">T weights vector</span>
    is: `-2 1 1 0 0 0 0`.

39. Click the <span class="smallcaps">Save</span> icon on the toolbar
    and save the batch file.  
    **Design estimation**  

40. If everything has been entered correctly the
    <span class="smallcaps">Run</span> button should now be green. Click
    <span class="smallcaps">Run</span> to estimate the design.

41. The design matrix should look as shown in
    Figure <a href="#fig:ppi7" data-reference-type="ref"
    data-reference="fig:ppi7">1.7</a>, below.

    <figure id="fig:ppi7">
    <img src="../../../assets/figures/manual/ppi/Fig7.png" style="width:85mm" />
    <figcaption><em>Design matrix</em></figcaption>
    </figure>

### GLM analysis - Results

1.  Click <span class="smallcaps">Results</span> and select the
    `SPM.mat` file.

2.  Choose the `Attention` contrast

3.  Apply masking \[None\]

4.  p value adjustment to control \[None\]

5.  threshold T or p value \[0.0001\]

6.  & extent threshold voxels \[10\]

7.  You should see an SPM that looks like the one shown below,
    Figure <a href="#fig:ppi8" data-reference-type="ref"
    data-reference="fig:ppi8">1.8</a>. Note the Superior Parietal and
    Dorso-Lateral Prefrontal activations, among others. By selecting
    <span class="smallcaps">overlays</span> $\rightarrow$
    <span class="smallcaps">sections</span>, and selecting the
    normalised structural image, you should be able to identify the
    anatomy more accurately.

    <figure id="fig:ppi8">
    <img src="../../../assets/figures/manual/ppi/Fig8.png" style="width:85mm" />
    <figcaption><em>Statistical parametric map for the
    <code>Attention</code> contrast</em></figcaption>
    </figure>

8.  To look at the `Motion` contrast where `Attention` is greater than
    `No Attention`, click <span class="smallcaps">Results</span>, choose
    the `SPM.mat` file and choose the `Motion` contrast.

9.  apply masking \[Contrast\]

10. Select contrast for masking: Choose the `Attention` contrast

11. Uncorrected mask p-value \[0.01\]

12. Nature of Mask: \[inclusive\]

13. p value adjustment to control \[FWE\]

14. threshold T or p value \[0.05\]

15. & extent threshold voxels \[3\]

16. The masked `motion` contrast on the glass brain is shown below in
    Figure <a href="#fig:ppi9" data-reference-type="ref"
    data-reference="fig:ppi9">1.9</a>.

    <figure id="fig:ppi9">
    <img src="../../../assets/figures/manual/ppi/Fig9.png" style="width:85mm" />
    <figcaption><em>Statistical parametric map for the <code>Motion</code>
    contrast inclusively masked by the Attention contrast</em></figcaption>
    </figure>

## GLM analysis - Extracting VOIs

1.  First select the `Motion` contrast, but do not include masking. Use
    a p-value adjustment of FWE with height threshold of 0.05 and a
    cluster threshold of 3.

2.  Go to point \[15 -78 -9\]

3.  Press `eigenvariate`

4.  Name of region \[V2\]

5.  Adjust data for \[effects of interest\]

6.  VOI definition \[sphere\]

7.  VOI radius(mm) \[6\]

This saves the extracted VOI data in the file `VOI_V2_1.mat` in the
working directory, and displays
Figure <a href="#fig:ppi10" data-reference-type="ref"
data-reference="fig:ppi10">1.10</a>, below. The left side shows the
location on a standard brain. The right side shows the first
eigenvariate of the extracted BOLD signal.

<figure id="fig:ppi10">
<img src="../../../assets/figures/manual/ppi/Fig10.png" style="width:85mm" />
<figcaption><em>First eigenvariate of the extracted BOLD signal in
V2</em></figcaption>
</figure>

## PPI analysis - Create PPI variable<span id="create_ppi" label="create_ppi"></span>

1.  PPIs can be calculated either by pressing the
    <span class="smallcaps">PPIs</span> button in the
    <span class="smallcaps">SPM Menu</span> window, or by selecting the
    <span class="smallcaps">Physio/Psycho-Physiologic</span> menu item
    from the SPM $\rightarrow$ Stats menu of the
    <span class="smallcaps">Batch Editor</span>. This example uses the
    <span class="smallcaps">Batch Editor</span>,
    Figure <a href="#fig:ppi11" data-reference-type="ref"
    data-reference="fig:ppi11">1.11</a>.

    <figure id="fig:ppi11">
    <img src="../../../assets/figures/manual/ppi/Fig11.png" style="width:100mm" />
    <figcaption><em>Physio/Psycho-Physiologic module in the Batch
    Editor</em></figcaption>
    </figure>

2.  Choose the <span class="smallcaps">SPM.mat</span> file in the `GLM`
    directory.

3.  Type of analysis: Choose <span class="smallcaps">Psycho-Physiologic
    interaction</span>,
    Figure <a href="#fig:ppi12" data-reference-type="ref"
    data-reference="fig:ppi12">1.12</a>.

    <figure id="fig:ppi12">
    <img src="../../../assets/figures/manual/ppi/Fig12.png" style="width:100mm" />
    <figcaption><em>Specifying a Psycho-Physiologic
    interaction.</em></figcaption>
    </figure>

4.  Select VOI: Choose `VOI_V2_1.mat`

5.  Input variables and contrast weights: Must be specified as an n x 3
    matrix, where n is the number of conditions included in the PPI. The
    first column of the matrix indexes `SPM.Sess.U(i)`. The second
    column indexes `SPM.Sess.U(i).name{ii}`. It will generally be a 1
    unless there are parametric effects. The third column is the
    contrast weight. In order to include Attention - No-attention in the
    PPI, recall that the conditions were entered as: Stationary,
    No-attention, Attention, therefore the matrix should be.

        [2 1 -1; 3 1 1]

6.  Name of PPI \[ V2x(Att-NoAtt) \]

7.  Display results: Yes

After a few seconds the PPI will be calculated and a graphical window
will appear, Figure <a href="#fig:ppi13" data-reference-type="ref"
data-reference="fig:ppi13">1.13</a>. In the upper left, the details of
the PPI setup calculation are given including the name of the PPI, the
chosen VOI file, and the included conditions and their contrast weights.
The main central graph shows the original BOLD signal (actually the
eigenvariate) in blue and the neuronal or deconvolved signal in green.
These will look quite similar for block design data. The graph in the
lower left shows the task condition plot, dotted green line, and the
convolved task conditions (psych variable). In the lower right the PPI
interaction term is plotted.

<figure id="fig:ppi13">
<img src="../../../assets/figures/manual/ppi/Fig13.png" style="width:100mm" />
<figcaption><em>PPI output graphics</em></figcaption>
</figure>

The PPI calculation will create a file `PPI_V2x(Att-NoAtt).mat` in the
current working directory. It contains the variable `PPI.ppi` (the
interaction term), `PPI.Y` (the original VOI eigenvariate) and `PPI.P`
(the `Attention - No Attention` task vector). You will use these vectors
in setting up your psychophysiologic interaction GLM analysis. See
`spm_peb_ppi` for a full description of the `PPI` data structure.

### PPI GLM analysis - Design setup and estimation

1.  Copy the file `PPI_V2x(Att-NoAtt)`
    <span class="smallcaps">Mat</span>-file to the `PPI` directory that
    you created at the start of this exercise.

2.  Change directory to the new one, i.e. `cd PPI`

3.  At the MATLAB prompt type

        >> load PPI_V2x(Att-NoAtt)

4.  In the <span class="smallcaps">Batch Editor</span> setup another GLM
    analysis by choosing the modules <span class="smallcaps">fMRI Model
    Specification</span>, <span class="smallcaps">Model
    Estimation</span> and <span class="smallcaps">Contrast
    Manager</span> as you did above, and fill it in as follows.

5.  Directory: Choose the `PPI` directory

6.  Units for design \[scans\]

7.  Interscan interval \[3.22\]

8.  Add a <span class="smallcaps">New: Subject/Session</span> under
    <span class="smallcaps">Data & Design</span>

9.  Click <span class="smallcaps">Scans</span> and choose all the
    functional scans `snffM00587_00xx.img`. There should be 360 `*.img`
    files.

10. Click <span class="smallcaps">New: Regressor</span> and add 6
    regressors.

11. Regressor 1: <span class="smallcaps">Name</span> =
    `PPI-interaction`, <span class="smallcaps">Value</span> = `PPI.ppi`

12. Regressor 2: <span class="smallcaps">Name</span> = `V2-BOLD`,
    <span class="smallcaps">Value</span> = `PPI.Y`

13. Regressor 3: <span class="smallcaps">Name</span> =
    `Psych_Att-NoAtt`, <span class="smallcaps">Value</span> = `PPI.P`

14. Regressor 4: <span class="smallcaps">Name</span> = `Block 1`,
    <span class="smallcaps">Value</span> = `block1`

15. Regressor 5: <span class="smallcaps">Name</span> = `Block 2`,
    <span class="smallcaps">Value</span> = `block2`

16. Regressor 6: <span class="smallcaps">Name</span> = `Block 3`,
    <span class="smallcaps">Value</span> = `block3`

17. High Pass Filter \[192\]  
      
    **Model Estimation**

18. Under <span class="smallcaps">Model estimation</span> click
    <span class="smallcaps">Select `SPM.mat`</span> then click the
    <span class="smallcaps">Dependency</span> button and choose
    <span class="smallcaps">fMRI model specification: SPM.mat
    File</span>. The <span class="smallcaps">Method</span> should be
    left as Classical.  
      
    **Contrast Manager**

19. Under <span class="smallcaps">Contrast Manager</span> click
    <span class="smallcaps">Select `SPM.mat`</span> then click the
    <span class="smallcaps">Dependency</span> button and choose
    <span class="smallcaps">Model estimation: SPM.mat File</span>

20. Click <span class="smallcaps">Contrast Sessions</span> then click
    <span class="smallcaps">New: T-contrast</span>

21. T-contrast, <span class="smallcaps">Name</span>: `PPI-Interaction`,
    vector: `1 0 0 0 0 0 0`

22. Save the batch file.

23. Run

The design matrix is shown below,
Figure <a href="#fig:ppi14" data-reference-type="ref"
data-reference="fig:ppi14">1.14</a>.

<figure id="fig:ppi14">
<img src="../../../assets/figures/manual/ppi/Fig14.png" style="width:85mm" />
<figcaption><em>Design matrix for the PPI analysis</em></figcaption>
</figure>

### PPI analysis - Results

1.  Press the <span class="smallcaps">Results</span> button and select
    the `SPM.mat` file in the PPI directory.

2.  Choose the `PPI-Interaction` contrast

3.  apply masking \[No\]

4.  p value adjustment to control \[None\]

5.  threshold T or p value \[0.01\]

6.  & extent threshold voxels \[10\]

7.  You should see an SPM that looks the same as the one shown below in
    the top part of
    Figure <a href="#fig:ppi15" data-reference-type="ref"
    data-reference="fig:ppi15">1.15</a>. The resulting SPM shows areas
    showing differential connectivity to V2 due to the effect of
    attention vs. no attention conditions. The effect in this subject is
    weak.

<figure id="fig:ppi15">
<img src="../../../assets/figures/manual/ppi/Fig15.png" style="width:100mm" />
<figcaption><em>PPI results</em></figcaption>
</figure>

### PPI analysis - Plotting

1.  One region showing the psychophysiologic interaction is the
    V5region, which is located at \[39 -72 0\] in this subject. Move the
    cursor to this point to view the area of activation, as shown below,
    in the bottom half of
    Figure <a href="#fig:ppi15" data-reference-type="ref"
    data-reference="fig:ppi15">1.15</a>.

2.  In order to plot the PPI graph showing the effect of attention, you
    need to extract a VOI from the V5 region. To do this, you will
    return to the original GLM analysis.

3.  Click Results, then select the GLM analysis `SPM.mat` file and the
    `Motion` contrast.

4.  apply masking \[No\]

5.  p value adjustment to control \[None\]

6.  threshold T or p value \[0.001\]

7.  & extent threshold voxels \[3\]

8.  Go to point \[39 -72 0\]

9.  Press eigenvariate

10. Name of region \[V5\]

11. Adjust data for \[effects of interest\]

12. VOI definition \[sphere\]

13. VOI radius(mm) \[6\]

14. Now you will create 4 PPIs (Follow the steps under
    section <a href="#create_ppi" data-reference-type="ref"
    data-reference="create_ppi">[create_ppi]</a>, Create PPI Variable,
    above). By using the PPI software machinery to create the
    interaction vectors, rather than just multiplying the extracted V2
    and V5 eigenvariates by the behavioral vectors, the PPI vectors will
    be formed properly.

15. `V2xNoAttention` (Use the V2 VOI and include `No-Attention` with a
    contrast weight of 1, do not include `Stationary`, `Attention`)

16. `V2xAttention` (Use the V2 VOI and include `Attention` with a
    contrast weight of 1, do not include `Stationary`, `No-Attention`)

17. `V5xNoAttention` (Use the V5 VOI and include `No-Attention` with a
    contrast weight of 1, do not include `Stationary`, `Attention`)

18. `V5xAttention` (Use the V5 VOI and include `Attention` with a
    contrast weight of 1, do not include `Stationary`, `No-Attention`)

19. Load the PPIs you just created with the following commands at the
    MATLAB prompt:

        >> v2noatt = load('PPI_V2xNoAttention');
        >> v2att   = load('PPI_V2xAttention.mat');
        >> v5noatt = load('PPI_V5xNoAttention.mat');
        >> v5att   = load('PPI_V5xAttention.mat');

20. Plot the PPI datapoints with the following commands at the MATLAB
    prompt:

        >> figure
        >> plot(v2noatt.PPI.ppi,v5noatt.PPI.ppi,'k.');
        >> hold on
        >> plot(v2att.PPI.ppi,v5att.PPI.ppi,'r.');

21. To plot the best fit lines type the following first for
    `NoAttention`

        >> x  = v2noatt.PPI.ppi(:);
        >> x  = [x, ones(size(x))];
        >> y  = v5noatt.PPI.ppi(:);
        >> B  = x\y;
        >> y1 = B(1)*x(:,1)+B(2);
        >> plot(x(:,1),y1,'k-');

22. Then for `Attention`

        >> x = v2att.PPI.ppi(:);
        >> x = [x, ones(size(x))];
        >> y = v5att.PPI.ppi(:);
        >> B = x\y;
        >> y1 = B(1)*x(:,1)+B(2);
        >> plot(x(:,1),y1,'r-');
        >> legend('No Attention','Attention')
        >> xlabel('V2 activity')
        >> ylabel('V5 response')
        >> title('Psychophysiologic Interaction')

<figure id="fig:ppi16">
<img src="../../../assets/figures/manual/ppi/Fig16.png" style="width:85mm" />
<figcaption><em>Graph demonstrating PPI interaction.</em></figcaption>
</figure>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-buchel1998" class="csl-entry">

Buchel, C., O. Josephs, G. Rees, R. Turner, C. Frith, and K. J. Friston.
1998. "The Functional Anatomy of Attention to Visual Motion. A
Functional MRI Study." *Brain* 121: 1281--94.
<http://brain.oxfordjournals.org/cgi/reprint/121/7/1281>.

</div>

<div id="ref-func1" class="csl-entry">

Friston, K. J. 1995. "Functional and Effective Connectivity in
Neuroimaging: A Synthesis." *Human Brain Mapping* 2: 56--78.

</div>

<div id="ref-ppi" class="csl-entry">

Friston, K. J., C. Buchel, G. R. Fink, J. Morris, E. Rolls, and R.
Dolan. 1997. "Psychophysiological and Modulatory Interactions in
Neuroimaging." *NeuroImage* 6: 218--29.
<https://doi.org/10.1006/nimg.1997.0291>.

</div>

<div id="ref-gitelman_03" class="csl-entry">

Gitelman, D. R., W. D. Penny, J. Ashburner, and K. J. Friston. 2003.
"Modeling Regional and Psychophysiologic Interactions in
<span class="nocase">fMRI</span>: The Importance of Hemodynamic
Deconvolution." *NeuroImage* 19: 200--207.
<https://doi.org/10.1016/S1053-8119(03)00058-2>.

</div>

</div>

[^1]: <http://www.fil.ion.ucl.ac.uk/spm/data/attention/>
