# Dynamic Causal Modeling for fMRI <span id="Chap:DCM_fmri" label="Chap:DCM_fmri"></span>

## Theoretical background

Dynamic Causal Modelling (DCM) is a method for making inferences about
neural processes that underlie measured time series, e.g. fMRI data. The
general idea is to estimate the parameters of a reasonably realistic
neuronal system model such that the predicted blood oxygen level
dependent (BOLD) signal, which results from converting the modeled
neural dynamics into hemodynamic responses, corresponds as closely as
possible to the observed BOLD time series. This section gives a short
introduction to the theoretical background of DCM for fMRI; details can
be found in (Friston, Harrison, and Penny 2003). Note that DCMs can be
formulated, in principle, for any measurement technique. Depending on
the spatio-temporal properties of a given measurement technique, one
needs to define an adequate state equation and an observation model. See
Fig <a href="#dcm_fig1" data-reference-type="ref"
data-reference="dcm_fig1">1.1</a> for a summary of the differences
between DCM implementations for fMRI and Event-Related Potentials
(ERPs). For a gentle introduction to DCM, written for non-technical
imaging researchers, see (Kahan and Foltynie 2013).

<figure id="dcm_fig1">
<div class="center">
<img src="../../../assets/figures/manual/dcm/Fig1.png" style="width:100mm" />
</div>
<figcaption><em>A schematic overview of the differences between between
the DCM implementations for fMRI and ERPs (as measured by EEG or MEG).
Whereas the state equation of DCM for fMRI is bilinear and uses only a
single state variable per region, that for ERPs is more complex and
requires 8 state variables per region. Moreover, DCM for ERPs models the
delays of activity propagation between areas. At the level of the
observation model, DCM for fMRI is more complex than DCM for ERPs. While
the former uses a non-linear model of the hemodynamic response that
contains a cascade of differential equations with five state variables
per region, the latter uses a simple linear model for predicting
observed scalp data.<span id="dcm_fig1"
label="dcm_fig1"></span></em></figcaption>
</figure>

As in state-space models, two distinct levels constitute a DCM (see
Figure <a href="#dcm_fig2" data-reference-type="ref"
data-reference="dcm_fig2">1.2</a>). The hidden level, which cannot be
directly observed using fMRI, represents a simple model of neural
dynamics in a system of $k$ coupled brain regions. Each system element
$i$ is represented by a single state variable $z_i$, and the dynamics of
the system is described by the change of the neural state vector over
time.

The neural state variables do not correspond directly to any common
neurophysiological measurement (such as spiking rates or local field
potentials) but represent a summary index of neural population dynamics
in the respective regions. Importantly, DCM models how the neural
dynamics are driven by external perturbations that result from
experimentally controlled manipulations. These perturbations are
described by means of external inputs $u$ that enter the model in two
different ways: they can elicit responses through direct influences on
specific regions ("driving" inputs, e.g. evoked responses in early
sensory areas) or they can change the strength of coupling among regions
("modulatory" inputs, e.g. during learning or attention).

Overall, DCM models the temporal evolution of the neural state vector,
i.e. , as a function of the current state, the inputs $u$ and some
parameters that define the functional architecture and interactions
among brain regions at a neuronal level ($n$ denotes "neural"):
$$\left[ \begin{array}{l}
  \dot{z}_1 \\
  \dot{z}_2 \\
  .. \\
  \dot{z}_k \\  \end{array} \right] = \dot{z}= \frac{dz}{dt} = F(z,u,\theta^n)$$
In this neural state equation, the state $z$ and the inputs $u$ are
time-dependent whereas the parameters are time-invariant. In DCM, $F$
has the bilinear form $$\dot{z}=Az+\sum_{j=1}^m u_j B_j z + Cu$$ The
parameters of this bilinear neural state equation,
$\theta^n=\{A,B_1,...,B_m,C\}$, can be expressed as partial derivatives
of $F$: $$\begin{aligned}
A & = & \frac{\partial F}{\partial z} = \frac{\partial \dot{z}}{\partial z} \\ \nonumber
B_j & = & \frac{\partial^2 F}{\partial z \partial u_j} = \frac{\partial}{\partial u_j}\frac{\partial \dot{z}}{\partial z}  \\ \nonumber
C & = & \frac{\partial F}{\partial u}
\end{aligned}$$ These parameter matrices describe the nature of the
three causal components which underlie the modeled neural dynamics: (i)
context-independent effective connectivity among brain regions, mediated
by anatomical connections ($k \times k$ matrix $A$), (ii)
context-dependent changes in effective connectivity induced by the $j$th
input $u_j$ ($k \times k$ matrices $B_1$, \..., $B_m$), and (iii) direct
inputs into the system that drive regional activity ($k \times m$ matrix
$C$). As will be demonstrated below, the posterior distributions of
these parameters can inform us about the impact that different
mechanisms have on determining the dynamics of the model. Notably, the
distinction between "driving" and "modulatory" is neurobiologically
relevant: driving inputs exert their effects through direct synaptic
responses in the target area, whereas modulatory inputs change synaptic
responses in the target area in response to inputs from another area.
This distinction represents an analogy, at the level of large neural
populations, to the concept of driving and modulatory afferents in
studies of single neurons.

<figure id="dcm_fig2">
<div class="center">
<img src="../../../assets/figures/manual/dcm/Fig2.png" style="width:130mm" />
</div>
<figcaption><em>Schematic summary of the conceptual basis of DCM. The
dynamics in a system of interacting neuronal populations (orange boxes),
which are not directly observable by fMRI, is modeled using a bilinear
state equation (grey box). Integrating the state equation gives
predicted neural dynamics (<span class="math inline"><em>z</em></span>)
that enter a model of the hemodynamic response (<span
class="math inline"><em>λ</em></span>) to give predicted BOLD responses
(<span class="math inline"><em>y</em></span>) (green boxes). The
parameters at both neural and hemodynamic levels are adjusted such that
the differences between predicted and measured BOLD series are
minimized. Critically, the neural dynamics are determined by
experimental manipulations. These enter the model in the form of
"external" or "driving" inputs. Driving inputs (<span
class="math inline"><em>u</em><sub>1</sub></span>; e.g. sensory stimuli)
elicit local responses directly that are propagated through the system
according to the endogenous (or latent) coupling. The strengths of these
connections can be changed by modulatory inputs (<span
class="math inline"><em>u</em><sub>2</sub></span>; e.g. changes in
cognitive set, attention, or learning).<span id="dcm_fig2"
label="dcm_fig2"></span></em></figcaption>
</figure>

DCM combines this model of neural dynamics with a biophysically
plausible and experimentally validated hemodynamic model that describes
the transformation of neuronal activity into a BOLD response. This
so-called "Balloon model" was initially formulated by Buxton and
colleagues and later extended by (Friston et al. 2000). Briefly
summarized, it consists of a set of differential equations that describe
the relations between four hemodynamic state variables, using five
parameters ($\theta^h$). More specifically, changes in neural activity
elicit a vasodilatory signal that leads to increases in blood flow and
subsequently to changes in blood volume $v$ and deoxyhemoglobin content
$q$. The predicted BOLD signal $y$ is a non-linear function of blood
volume and deoxyhemoglobine content. Details of the hemodynamic model
can be found in other publications (Friston et al. 2000). By combining
the neural and hemodynamic states into a joint state vector x and the
neural and hemodynamic parameters into a joint parameter vector
$\theta=[\theta^n, \theta^h]^T$, we obtain the full forward model that
is defined by the neural and hemodynamic state equations
$$\begin{aligned}
\dot{x} & = & F(x,u,\theta) \\ \nonumber
y & = & \lambda(x)
\end{aligned}$$ For any given set of parameters $\theta$ and inputs $u$,
the joint state equation can be integrated and passed through the output
nonlinearity $\lambda$ to give a predicted BOLD response $h(u,\theta)$.
This can be extended to an observation model that includes observation
error $e$ and confounding effects $X$ (e.g. scanner-related
low-frequency drifts): $$y = h(u,\theta) + X \beta + e$$ This
formulation is the basis for estimating the neural and hemodynamic
parameters from the measured BOLD data, using a fully Bayesian approach
with empirical priors for the hemodynamic parameters and conservative
shrinkage priors for the neural coupling parameters.

Details of the parameter estimation scheme, which rests on a Fisher
scoring gradient ascent scheme with Levenburg-Marquardt regularisation,
embedded in an expectation maximization (EM) algorithm, can be found in
the original DCM publication (Friston, Harrison, and Penny 2003). In
brief, under Gaussian assumptions about the posterior distributions,
this scheme returns the posterior expectations $\eta_{\theta | y}$ and
posterior covariance $C_{\theta | y}$ for the parameters as well as
hyperparameters for the covariance of the observation noise, $C_e$.

After fitting the model to measured BOLD data, the posterior
distributions of the parameters can be used to test hypotheses about the
size and nature of effects at the neural level. Although inferences
could be made about any of the parameters in the model, hypothesis
testing usually concerns context-dependent changes in coupling (i.e.
specific parameters from the $B$ matrices; see
Fig. <a href="#dcm_fig4" data-reference-type="ref"
data-reference="dcm_fig4">1.5</a>). As will be demonstrated below, at
the single-subject level, these inferences concern the question of how
certain one can be that a particular parameter or, more generally, a
contrast of parameters, $c^T \eta_{\theta | y}$, exceeds a particular
threshold $\gamma$ (e.g. zero).

Under the assumptions of the Laplace approximation, this is easy to test
($\Phi_N$ denotes the cumulative normal distribution):
$$p(c^T \eta_{\theta | y} > \gamma) = \Phi_N \left(\frac{c^T \eta_{\theta | y} - \gamma}{c^T C_{\theta | y} c} \right)$$
For example, for the special case $c^T \eta_{\theta | y} = \gamma$ the
probability is $p(c^T \eta_{\theta | y} > \gamma)=0.5$, i.e. it is
equally likely that the parameter is smaller or larger than the chosen
threshold $\gamma$. We conclude this section on the theoretical
foundations of DCM by noting that the parameters can be understood as
rate constants (units: $1/s = Hz$) of neural population responses that
have an exponential nature. This is easily understood if one considers
that the solution to a linear ordinary differential equation of the form
$\dot{z}=Az$ is an exponential function (see Fig.
 <a href="#dcm_fig3" data-reference-type="ref"
data-reference="dcm_fig3">1.3</a>).

<figure id="dcm_fig3">
<div class="center">
<img src="../../../assets/figures/manual/dcm/Fig3.png" style="width:120mm" />
</div>
<figcaption><em>A short mathematical demonstration, using a simple
linear first-order differential equation as an example, explaining why
the coupling parameters in a DCM are inversely proportional to the
half-life of the modelled neural responses and are therefore in units of
1/s = Hertz.<span id="dcm_fig3"
label="dcm_fig3"></span></em></figcaption>
</figure>

## Bayesian model selection <span id="mc" label="mc"></span>

A generic problem encountered by any kind of modeling approach is the
question of model selection: given some observed data, which of several
alternative models is the optimal one? This problem is not trivial
because the decision cannot be made solely by comparing the relative fit
of the competing models. One also needs to take into account the
relative complexity of the models as expressed, for example, by the
number of free parameters in each model.

Model complexity is important to consider because there is a trade-off
between model fit and generalizability (i.e. how well the model explains
different data sets that were all generated from the same underlying
process). As the number of free parameters is increased, model fit
increases monotonically whereas beyond a certain point model
generalizability decreases. The reason for this is "overfitting": an
increasingly complex model will, at some point, start to fit noise that
is specific to one data set and thus become less generalizable across
multiple realizations of the same underlying generative process.

Therefore, the question "What is the optimal model?" can be reformulated
more precisely as "What is the model that represents the best balance
between fit and complexity?". In a Bayesian context, the latter question
can be addressed by comparing the evidence, $p(y|m)$, of different
models. According to Bayes theorem
$$p(\theta|y,m) = \frac{p(y|\theta,m)p(\theta|m)}{p(y|m)}$$ the model
evidence can be considered as a normalization constant for the product
of the likelihood of the data and the prior probability of the
parameters, therefore
$$p(y|m) = \int p(\theta|y,m) p(\theta|m) d\theta$$ Here, the number of
free parameters (as well as the functional form) are considered by the
integration. Unfortunately, this integral cannot usually be solved
analytically, therefore an approximation to the model evidence is
needed. One such approximation used by DCM, and many other models in
SPM, is to make use of the Laplace approximation [^1].

As shown in (Penny et al. 2004), this yields the following expression
for the natural logarithm ($ln$) of the model evidence (
$\eta_{\theta | y}$ denotes the posterior mean, $C_{\theta | y}$ is the
posterior covariance of the parameters, $C_e$ is the error covariance,
$\theta_p$ is the prior mean of the parameters, and $C_p$ is the prior
covariance): $$\begin{aligned}
ln p(y|m) & = & accuracy(m) - complexity (m) \\ \nonumber
& = & \left[ -\frac{1}{2} ln |C_e| - \frac{1}{2} (y-h(u,\eta_{\theta | y}))^T C_e^{-1} (y-h(u,\eta_{\theta | y}))\right] \\ \nonumber
& - & \left[ \frac{1}{2} ln |C_p| -\frac{1}{2}ln |C_{\theta | y}| + \frac{1}{2} (\eta_{\theta | y}-\theta_p)^T C_p^{-1} (\eta_{\theta | y}-\theta_p) \right]
\end{aligned}$$

This expression properly reflects the requirement, as discussed above,
that the optimal model should represent the best compromise between
model fit (accuracy) and model complexity. The complexity term depends
on the prior density, for example, the prior covariance of the intrinsic
connections.

Two models can then be compared using the Bayes factor:
$$BF_{ij} = \frac{p(y|m_i)}{p(y|m_j)}$$ Given uniform priors over
models, the posterior probability for model $i$ is greater 0.95 if
$BF_{ij}$ is greater than twenty.

This results in a robust procedure for deciding between competing
hypotheses represented by different DCMs. These hypotheses can concern
any part of the structure of the modeled system, e.g. the pattern of
endogenous connections (A-matrix) or which inputs affect the system and
where they enter (C-matrix). Note, however, that this comparison is only
valid if the data $y$ are identical in all models. This means that in
DCM for fMRI, where the data vector results from a concatenation of the
time series of all areas in the model, only models can be compared that
contain the same areas. Therefore, model selection cannot be used to
address whether or not to include a particular area in the model. In
contrast, in DCM for ERPs, the data measured at the sensor level are
independent of how many neuronal sources are assumed in a given model.
Here, model selection could also be used to decide which sources should
be included.

## Practical example

The following example refers to the "attention to visual motion" data
set available from the SPM web site[^2]. This data set was obtained by
Christian Buchel and is described in (Buchel and Friston 1997).

The archive contains the smoothed, spatially normalised, realigned,
slice-time corrected images in the directory `functional`. The directory
`structural` contains a spatially normalised structural image. All
processing took place using SPM99, but the image files have been
converted into NIfTI format.

Making a DCM requires two ingredients: (i) a design matrix and (ii) the
time series, stored in VOI files. The regressors of the design matrix
define the inputs for the DCM. Note that this means that the design
matrix that is optimal for a given DCM is often somewhat different than
the one for the corresponding GLM. DCM does not require the design
matrix to be part of an estimated model, however. It just needs to be
defined.

### Defining the GLM

The present experiment consisted of 4 conditions: (i) fixation (F), (ii)
static (S, non-moving dots), (iii) no attention (N, moving dots but no
attention required), (iv) attention (A). The GLM analyses by Christian
showed that activity in area V5 was not only enhanced by moving stimuli,
but also by attention to motion. In the following, we will try to model
this effect in V5, and explain it as a context-dependent modulation or
"enabling" of V5 afferents, using a DCM. First, we need to set up the
GLM analysis and extract our time series from the results. In this
example, we want to use the same design matrix for GLM and DCM,
therefore we recombine the above regressors to get the following three
conditions:

1.  **photic**: this comprises all conditions with visual input, i.e. S,
    N, and A.

2.  **motion**: this includes all conditions with moving dots, i.e. N
    and A.

3.  **attention**: this includes the attention-to-motion (A) condition
    only.

Now we need to define and estimate the GLM. This is not the main topic
of this chapter so you should already be familiar with these procedures,
see <a href="#Chap:fmri_spec" data-reference-type="ref"
data-reference="Chap:fmri_spec">[Chap:fmri_spec]</a> and
<a href="#Chap:fmri_est" data-reference-type="ref"
data-reference="Chap:fmri_est">[Chap:fmri_est]</a> for more information.
Here are the relevant details for this data set that you need to set up
the GLM:

- The onsets for the conditions can be found in the file `factors.mat`.
  They are named `stat` (static), `natt` (no attention) and `att`
  (attention) and are defined in scans (not seconds). They are blocks of
  10 TRs each.

- The TR is 3.22 seconds.

- There are 360 scans.

Let's specify a batch that will specify the model and estimate it.

1.  The analysis directory you have downloaded should include

    1.  A directory named `functional`, which includes the preprocessed
        fMRI volumes.

    2.  A directory named `structural`, which includes a normalised T1
        structural volume

    3.  File `factors.mat`.

    4.  You will also need to make a new directory called `GLM` that
        will contain the analysis.

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

7.  <span class="smallcaps">Interscan interval</span> \[3.22\]

8.  Click <span class="smallcaps">Data & Design</span>, Choose
    <span class="smallcaps">New \"Subject/Session\"</span>

9.  Click <span class="smallcaps">Scans</span> and choose all the
    functional scans `snffM00587_00xx.img`. There should be 360 `*.img`
    files.

10. Go back to the main MATLAB workspace and load the MAT-file
    containing the experimental conditions:

        >> load ../factors.mat

    You can look at the loaded variables by typing the variable names.
    (`stat` = stationary, `natt` = no attention, `att` = attention)

        >> stat
        >> natt
        >> att

11. Return to the batch editor. Click
    <span class="smallcaps">Conditions</span> then double click
    <span class="smallcaps">New: Condition</span> three times. Enter the
    following details for each:

    - Condition 1: <span class="smallcaps">Name</span> = `Photic`,
      <span class="smallcaps">Onsets</span> = `[att natt stat]` and
      <span class="smallcaps">Durations</span> = `10`.

    - Condition 2: <span class="smallcaps">Name</span> = `Motion`,
      <span class="smallcaps">Onsets</span> = `[att natt]` and
      <span class="smallcaps">Durations</span> = `10`.

    - Condition 3: <span class="smallcaps">Name</span> = `Attention`,
      <span class="smallcaps">Onsets</span> = `att` and
      <span class="smallcaps">Durations</span> = `10`.

12. From the SPM menu at the top of the Batch Editor, select "Stats $>$
    model estimation".

13. For <span class="smallcaps">Select SPM.mat</span>, click on the
    <span class="smallcaps">Dependency</span> button and choose the
    proposed item (the output from the previous module).

14. You should now be able to press the
    <span class="smallcaps">Run</span> green arrow at the top of the
    Batch Editor window. This will specify and estimate the GLM.

### Adding contrasts

Once you have specified and estimated the GLM, you should define
t-contrasts that test for photic, motion, and attention, respectively.
These serve to locate areas that show "photic" effects due to visual
stimulation (e.g. in V1), motion (e.g. V5) and attention (e.g. V5 and
superior parietal cortex, SPC). You will also need to add an "effects of
interest" F-contrast, which is required for mean-correcting the
extracted time series. Any experimental effects not covered by this
contrast will be regressed out of the signal.

1.  From the main SPM window, click on the
    <span class="smallcaps">Batch</span> button.

2.  Add a module "SPM $>$ Stats $>$ Contrast manager".

3.  For <span class="smallcaps">Select SPM.mat</span>, enter the one
    that has been created in the previous step.

4.  Under <span class="smallcaps">Contrast Sessions</span>, choose one
    <span class="smallcaps">New: F-contrast</span> and three
    <span class="smallcaps">New: T-contrast</span> and enter

    - F-contrast: <span class="smallcaps">Name</span> =
      `Effects of interest`, <span class="smallcaps">F contrast
      vector</span> = `eye(3)`.

    - T-contrast: <span class="smallcaps">Name</span> = `Photic`,
      <span class="smallcaps">T contrast vector</span> = `[1 0 0]`.

    - T-contrast: <span class="smallcaps">Name</span> = `Motion`,
      <span class="smallcaps">T contrast vector</span> = `[0 1 0]`.

    - T-contrast: <span class="smallcaps">Name</span> = `Attention`,
      <span class="smallcaps">T contrast vector</span> = `[0 0 1]`.

5.  Press the <span class="smallcaps">Run</span> green arrow at the top
    of the Batch Editor window. This will specify and estimate these 4
    contrasts.

### Extracting time series

The next step is to extract one representative fMRI time series from
each brain region of interest: V5, V1 and SPC.

#### Extracting time series from V5

To locate visual area V5, we will identify voxels that respond to both
visual motion AND attention to motion. This is referred to as "masking"
one contrast by another. We will then extract time series from voxels
identified by this contrast, restricted to a sphere positioned in the V5
region:

1.  Press <span class="smallcaps">Results</span>.

2.  Select the `SPM.mat` file.

3.  Choose the t-contrast for the `Motion` condition.

4.  Apply masking: contrast

5.  Choose the t-contrast for the `Attention` condition.

6.  Uncorrected mask p-value $p \leq 0.05$ and nature of mask:
    inclusive.

7.  p value adjustment to control: none with a threshold of 0.001 and
    extent 0

8.  To overlay these results on a structural scan, click "overlays\..."
    in the SPM Results window, then click "sections". Navigate to the
    structural folder and select the file named "nsM00587_0002.img".

9.  At the bottom of the results window, enter the following
    coordinates, which are V5-ish: `[-36 -87 -3]`.

10. Press the <span class="smallcaps">eigenvariate</span> button.

11. Name of region: `V5`

12. Adjust data for: `Effects of interest` (this effectively
    mean-corrects the time series)

13. VOI definition: `sphere`

14. VOI radius(mm): e.g. `8` mm

SPM now computes the first principal component of the time series from
all super-threshold voxels included in the sphere. The result is stored
(together with the original time series) in a file named `VOI_V5_1.mat`
in the working directory (the "1" refers to session 1).

#### Extracting time series from V1

We will next identify visual area V1 (primary visual cortex) using the
Photic contrast. This is the same procedure as for V5 above, except we
will only by using one contrast (not masking one contrast by another):

1.  Press <span class="smallcaps">Results</span>.

2.  Select the `SPM.mat` file.

3.  Choose the t-contrast for the `Photic` condition.

4.  Apply masking: none

5.  p value adjustment to control: none with a threshold of 0.001 and
    extent 0

6.  To overlay these results on a structural scan, click "overlays\..."
    in the SPM Results window, then click "previous sections".

7.  Set the coordinates at the bottom of the Results window to
    `[0 -93 18]`.

8.  Press the <span class="smallcaps">eigenvariate</span> button.

9.  Name of region: `V1`

10. Adjust data for: `Effects of interest` (this effectively
    mean-corrects the time series)

11. VOI definition: `sphere`

12. VOI radius(mm): e.g. `8` mm

This will create the file `VOI_V1_1.mat`.

#### Extracting time series from SPC

Finally, we will extract time series from superior parietal cortex
(SPC). We will identify voxels in this region using the Attention
contrast:

1.  Press <span class="smallcaps">Results</span>.

2.  Select the `SPM.mat` file.

3.  Choose the t-contrast for the `Attention` condition.

4.  Apply masking: none

5.  p value adjustment to control: none with a threshold of 0.001 and
    extent 0

6.  To overlay these results on a structural scan, click "overlays\..."
    in the SPM Results window, then click "previous sections".

7.  Set the coordinates at the bottom of the Results window to
    `[-27 -84 36]`.

8.  Press the <span class="smallcaps">eigenvariate</span> button.

9.  Name of region: `SPC`

10. Adjust data for: `Effects of interest` (this effectively
    mean-corrects the time series)

11. VOI definition: `sphere`

12. VOI radius(mm): e.g. `8` mm

### Specifying and estimating the DCM

Now we have defined the inputs (via the design matrix) and the time
series, we are ready to build the DCM. We will look at a simplified
version of the model described in (Friston, Harrison, and Penny 2003).
In our example here, we will model a hierarchically connected system
comprising V1, V5 and SPC, i.e. reciprocal connections between V1-V5 and
V5-SPC, but not between V1-SPC. We will assume that (i) V1 is driven by
any kind of visual stimulation (direct input "photic"), (ii)
motion-related responses in V5 can be explained through an increase in
the influence of V1 onto V5 whenever the stimuli are moving (i.e.
"motion" acts as modulatory input onto the $V1 \rightarrow V5$
connection) and (iii) attention enhances the influence of SPC onto V5
(i.e. "attention" acts as modulatory input onto the $SPC \rightarrow V5$
connection). This DCM is shown schematically in
Figure <a href="#bwd" data-reference-type="ref" data-reference="bwd">1.4</a>,
and can be made

<figure id="bwd">
<div class="center">
<img src="../../../assets/figures/manual/dcm/dcm_mod_bwd.png" style="width:100mm" />
</div>
<figcaption><em>DCM with attentional modulation of backwards connection.
Dotted lines denote modulatory connections.<span id="bwd"
label="bwd"></span></em></figcaption>
</figure>

as follows:

1.  Press the large `Dynamic Causal Modelling` button.

2.  Choose <span class="smallcaps">specify</span>.

3.  Select the `SPM.mat` file you just created when specifying the GLM.

4.  Name for `DCM_???.mat`: e.g. `mod_bwd` (for "attentional modulation
    of backward connection").

5.  Select all VOIs in order `VOI_V1_1`, `VOI_V5_1`, `VOI_SPC_1`.

6.  Include `Photic`: Yes

7.  Include `Motion`: Yes

8.  Include `Attention`: Yes

9.  Specify slice timings for each area. The default values are set to
    the last slice of the data, which was the default in the original
    DCM version. For sequential (as opposed to interleaved) data, this
    modelling option allows to use DCM in combination with any TR (slice
    timing differences) (Kiebel et al. 2007). Here, we proceed with the
    default values.

10. Enter `0.04` for "Echo Time, TE\[s\]".

11. Modulatory effects: `bilinear`

12. States per region: `one`

13. Stochastic effects: `no`

14. Centre input: `no`

15. Fit timeseries or CSD: `timeseries`

16. Define the following extrinsic connections: V1 to V5, V5 to V1, V5
    to SPC, SPC to V5, i.e. a hierarchy with reciprocal connections
    between neighbouring areas. Note that the columns specify the source
    of the connection and the rows specify its target. Your connectivity
    matrix should look like the one in
    Fig. <a href="#dcm_fig4" data-reference-type="ref"
    data-reference="dcm_fig4">1.5</a>.

17. Specify Photic as a driving input into V1. See
    Fig. <a href="#dcm_fig4" data-reference-type="ref"
    data-reference="dcm_fig4">1.5</a>

18. Specify Motion to modulate the connection from V1 to V5. See
    Fig. <a href="#dcm_fig4" data-reference-type="ref"
    data-reference="dcm_fig4">1.5</a>

19. Specify Attention to modulate the connection from SPC to V5. See
    Fig. <a href="#dcm_fig4" data-reference-type="ref"
    data-reference="dcm_fig4">1.5</a>

A polite "Thank you" completes the model specification process. A file
called `DCM_mod_bwd.mat` will have been generated.

<figure id="dcm_fig4">
<div class="center">
<p><img src="../../../assets/figures/manual/dcm/Fig4.png" style="width:70mm" /> <img
src="../../../assets/figures/manual/dcm/Fig5.png" style="width:70mm" /> <img
src="../../../assets/figures/manual/dcm/Fig6.png" style="width:70mm" /> <img
src="../../../assets/figures/manual/dcm/Fig7.png" style="width:70mm" /></p>
</div>
<figcaption><em>Specification of model depicted in Fig <a href="#bwd"
data-reference-type="ref" data-reference="bwd">1.4</a>. <strong>Top
left:</strong> Filled circles define the structure of the extrinsic
connections <span class="math inline"><em>A</em></span> such that eg.
there are no connections from V1 to SPC or from SPC to V1. <strong>Top
right:</strong> The filled circle specifies that the input
<code>Photic</code> connects to region V1. <strong>Bottom left:</strong>
The filled circle indicates that the input <code>Motion</code> can
modulate the connection from V1 to V5. This specifies a "modulatory"
connection. <strong>Bottom right:</strong> The filled circle indicates
that <code>Attention</code> can modulate the connection from SPC to V5.
<span id="dcm_fig4" label="dcm_fig4"></span></em></figcaption>
</figure>

You can now estimate the model parameters, either by pressing the DCM
button again and choosing <span class="smallcaps">estimate
(time-series)</span>, or by typing

    >> spm_dcm_estimate('DCM_mod_bwd');

from the MATLAB command line.

<figure id="fwd" markdown>
<div class="center">
<img src="../../../assets/figures/manual/dcm/dcm_mod_fwd.png" style="width:100mm" />
</div>
<figcaption><em>DCM with attentional modulation of forwards connection.
Dotted lines denote modulatory connections.<span id="fwd"
label="fwd"></span></em></figcaption>
</figure>

<figure id="dcm_fig8" markdown>
<div class="center">
<img src="../../../assets/figures/manual/dcm/Fig8.png" style="width:140mm" />
</div>
<figcaption><em>Plot of predicted and observed response, after
convergence and conditional expectation of the parameters.<span
id="dcm_fig8" label="dcm_fig8"></span></em></figcaption>
</figure>

Once this is completed, you can review the results as follows:

1.  Press the DCM button.

2.  Choose <span class="smallcaps">review</span>.

3.  Select `DCM_mod_bwd.mat`

By clicking "review\..." you can now select from multiple options, e.g.
you can revisit the fit of the model ("Outputs") or look at the
parameter estimates for the endogenous coupling ("Intrinsic
connections") or for the parameters associated with the driving or
modulatory inputs ("Effects of Photic", "Effects of Motion", "Effects of
Attention").

Also, you can use the "Contrasts" option to determine how confident you
can be that a contrast of certain parameter estimates exceeds the
threshold you chose in step 4. Of course, you can also explore the model
results at the level of the MATLAB command line by loading the model and
inspecting the parameter estimates directly. These can be found in
`DCM.Ep.A` (endogenous coupling), `DCM.Ep.B` (modulatory inputs) and
`DCM.Ep.C` (driving inputs).

### Comparing models

Let us now specify an alternative model and compare it against the one
that we defined and estimated above. The change that we are going to
make is to assume that attention modulates the $V1 \rightarrow V5$
connection (as opposed to the $SPC \rightarrow V5$ connection in the
previous model). For defining this model, you repeat all the steps from
the above example, the only difference being that the model gets a new
name (e.g. `mod_fwd`) and that attention now acts on the forward
connection. This DCM is shown schematically in
Figure <a href="#fwd" data-reference-type="ref" data-reference="fwd">1.6</a>.

Once you have specified this new model, estimate it as above. You can
then perform a Bayesian model comparison as follows:

1.  Press the "DCM" button.

2.  Choose <span class="smallcaps">Compare</span>.

3.  In the Batch Editor window that opened, fill in the boxes as
    follows:

    1.  `Directory`: choose current directory by clicking the small dot
        on the right hand side of the file selector then click Done,

    2.  `Data`: add a New Subject with a New Session and select the two
        models, e.g. in the order `DCM_mod_bwd.mat` and
        `DCM_mod_fwd.mat`,

    3.  `Inference method`: choose "Fixed effects (FFX)".

4.  Press Run (the green triangle in the Batch Editor).

The Graphics window, Fig. <a href="#dcm_fig9" data-reference-type="ref"
data-reference="dcm_fig9">1.8</a>, now shows a bar plot of the model
evidence. You can see that our second model is better than the first
one.

<figure id="dcm_fig9" markdown>
<div class="center">
<img src="../../../assets/figures/manual/dcm/Fig9.png" style="width:140mm" />
</div>
<figcaption><em>Model 2 (shown in Fig <a href="#fwd"
data-reference-type="ref" data-reference="fwd">1.6</a>) is preferred to
model 1 (shown in Fig <a href="#bwd" data-reference-type="ref"
data-reference="bwd">1.4</a>).<span id="dcm_fig9"
label="dcm_fig9"></span></em></figcaption>
</figure>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-buchel97" class="csl-entry">

Buchel, C., and K. J. Friston. 1997. "Modulation of Connectivity in
Visual Pathways by Attention: <span class="nocase">c</span>ortical
Interactions Evaluated with Structural Equation Modelling and fMRI."
*Cerebral Cortex* 7: 768--78.

</div>

<div id="ref-dcm" class="csl-entry">

Friston, K. J., L. Harrison, and W. D. Penny. 2003. "Dynamic Causal
Modelling." *NeuroImage* 19 (4): 1273--1302.

</div>

<div id="ref-balloon" class="csl-entry">

Friston, K. J., A. Mechelli, R. Turner, and C. J. Price. 2000.
"Nonlinear Responses in fMRI: The Balloon Model, Volterra Kernels and
Other Hemodynamics." *NeuroImage* 12: 466--77.

</div>

<div id="ref-Kahan2013" class="csl-entry">

Kahan, J., and T. Foltynie. 2013. "Understanding DCM: Ten Simple Rules
for the Clinician." *NeuroImage* 83: 542--49.
<https://doi.org/10.1016/j.neuroimage.2013.07.008>.

</div>

<div id="ref-sjk_dcm_slicetiming" class="csl-entry">

Kiebel, S. J., S. Klöppel, N. Weiskopf, and K. J. Friston. 2007.
"Dynamic Causal Modeling: A Generative Model of Slice Timing in fMRI."
*NeuroImage* 34: 1487--96.
<https://doi.org/10.1016/j.neuroimage.2006.10.026>.

</div>

<div id="ref-cdcm" class="csl-entry">

Penny, W. D., K. E. Stephan, A. Mechelli, and K. J. Friston. 2004.
"Comparing Dynamic Causal Models." *NeuroImage* 22 (3): 1157--72.

</div>

</div>

[^1]: This should perhaps more correctly be referred to as a fixed-form
    variational approximation, where the fixed form is chosen to be a
    Gaussian. The model evidence is approximated by the negative free
    energy, $F$.

[^2]: Attention to visual motion dataset:
    <https://www.fil.ion.ucl.ac.uk/spm/data/attention/>
