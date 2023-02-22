## Overview

A common experimental aim is to test whether effective connectivity is
different between groups of subjects, or is different according to a
behavioural measure (e.g. test scores) within a group. One approach is
to take DCM connectivity parameters and apply a classical test (e.g.
t-tests, CVA). The disadvantage of this approach is that it throws away
the estimated uncertainty (variance) about the connection strengths.
Alternatively, one may construct a Bayesian hierarchical model over the
parameters - describing how group level effects constrain parameter
estimates on a subject-by-subject basis. SPM12 includes a Parametric
Empirical Bayes (PEB) model, which makes it possible to evaluate group
effects and between-subjects variability on parameters.

This page describes the main steps for performing a DCM+PEB analysis.
For a more detailed explanation, please see the tutorial papers: [Part 1
on DCM for fMRI](doi:10.1016/j.neuroimage.2019.06.031 "wikilink") and
[Part 2 on PEB](doi:10.1016/j.neuroimage.2019.06.032 "wikilink") which
come with an [Example fMRI
dataset](https://github.com/pzeidman/dcm-peb-example).

## First level analysis

#### **Specify an exemplar DCM for one subject**

Begin by specifying a single DCM for an example subject using SPM\'s
graphical user interface (you can find a worked fMRI example in Chapter
35 of the [SPM
manual](https://www.fil.ion.ucl.ac.uk/spm/doc/spm12_manual.pdf) or in
the tutorial papers mentioned above). In this model, make sure to switch
on all connections of interest. We\'ll refer to this as the \'full\'
model.

#### **Replicate the DCM over subjects and assemble them in a group DCM (GCM) file**

Next, replicate this DCM over subjects. You can do this using the batch.

For fMRI:

- Click Batch from the main SPM window to open the batch editor.
- Click SPM -\> DCM -\> DCM specification -\> DCM for fMRI -\> specify
  group

For M/EEG:

- Click Batch from the main SPM window to open the batch editor.
- Click SPM -\> DCM -\> DCM specification -\> DCM for M/EEG

The output will be a cell array with one column and one subject per row
(GCM_name.mat). The entries in this array can either be the filenames of
individual DCMs, or the DCM structures themselves.

#### Estimate the models

Estimate the models (i.e. fit them to the data, to get estimates of the
parameters). This can easily be done via the batch.

- Click Batch from the main SPM window to open the batch editor.
- Click SPM -\> DCM -\> DCM estimation.
- Select the GCM file you created above, which contains a cell array
  with all subjects\' DCMs (or their filenames)
- Click Output, then \"Overwrite existing GCM/DCM files\".
- Click the green play button.

Here\'s how to achieve the same result using underlying Matlab
functions:\<syntaxhighlight lang=\"matlab\"\>% Collate DCMs into a GCM
file GCM = {\'DCM_subject1_full_model.mat\'

`      'DCM_subject2_full_model.mat'};`

% Estimate each subject\'s model GCM = spm_dcm_fit(GCM);

% Alternatively, replace the line above with the following code to
alternate between estimating % DCMs and estimating group effects. This
is slower, but can draw subjects out % of local optima towards the group
mean. % GCM = spm_dcm_peb_fit(GCM);

% Write results save(\'GCM_example.mat\',\'GCM\');

\</syntaxhighlight\>

## Second level analysis

#### **Estimate a second level PEB (Parametric Empirical Bayes) model**

Having finished the first level analysis, we now create a second level
(group) general linear model over the parameters:

- In the batch editor select SPM -\> DCM -\> Second level -\> Specify /
  Estimate PEB.
- Give the analysis a name and select the GCM file created above.
- Under Covariates, add any between-subjects effects you\'d like to
  model. (This is like specifying a design matrix in a regular second
  level SPM analysis). The first regressor will model the group mean and
  is added to the design matrix automatically. Some of the PEB functions
  assume that the first covariate you add will be of experimental
  interest, whereas subsequent covariates are nuisance variables of no
  interest. If you leave the covariates option blank, only the group
  mean will be modelled.
- Under Fields, select which fields of the DCM to model at the group
  level. We recommend keeping this to a small number of parameters. If,
  for instance, you\'re interested in the A-matrix and the B-matrix,
  then run a separate PEB analysis on each.

This will create a group level model named PEB_name.mat. It contains
parameters representing each between-subjects effect on each DCM
connection. Here\'s the equivalent Matlab code using the underlying SPM
functions:\<syntaxhighlight lang=\"matlab\"\>% Specify PEB model
settings % The \'all\' option means the between-subject variability of
each connection will % be estimated individually M = struct(); M.Q =
\'all\';

% Specify design matrix for N subjects. It should start with a constant
column M.X = ones(N,1);

% Choose field field = {\'A\'};

% Estimate model PEB = spm_dcm_peb(GCM,M,field);

save(\'PEB_example.mat\',\'PEB\'); \</syntaxhighlight\>

#### **Compare the full PEB model to nested PEB models to test specific hypotheses**

The PEB model created above will have lots of parameters (the number of
group effects multiplied by the number of DCM connections). To test
hypotheses, you can compare this full PEB model to nested PEB models
with certain parameters switched off (fixed at their prior mean of
zero). For instance, you could switch off all group level effects,
including the group mean, on the connection from Region R1 to Region R2.
The difference in model evidence between the full PEB and this nested
PEB, with R1-\>R2 parameters switched off, will tell you the importance
of the R1-\>R2 at the group level.

To perform this analysis, first implement each of your hypotheses as a
DCM with certain connections switched on or off. For instance, if you
have five different connectivity hypotheses, you would specify five
different DCMs. These DCMs don\'t need to be estimated - they only serve
to tell the software which parameters to try switching off. You can do
this using the GUI or by writing a script to modify an existing model,
for example:

\<syntaxhighlight lang=\"matlab\"\> % Get an existing model. We\'ll use
the first subject\'s DCM as a template DCM_full = GCM{1};

% IMPORTANT: If the model has already been estimated, clear out the old
priors, or changes to DCM.a,b,c will be ignored if
isfield(DCM_full,\'M\')

`   DCM_full = rmfield(DCM_full ,'M');`

end

% Specify candidate models that differ in particular A-matrix
connections, e.g. DCM_model1 = DCM_full; DCM_model1.a(1,2) = 0; %
Switching off the connection from region 2 to region 1

DCM_model2 = DCM_full; DCM_model2.a(3,4) = 0; % Switching off the
connection from region 4 to region 3 \</syntaxhighlight\>

Whether you use the GUI or a script to specify the candidate models
above, you will need to write a simple script to collate them into a
cell array, with one column per model:

\<syntaxhighlight lang=\"matlab\"\> GCM = {DCM_full, DCM_model1,
DCM_model2}; save(\'GCM_templates.mat\', \'GCM\'); \</syntaxhighlight\>

To recap, you now have a PEB file with the group-level parameters, and a
GCM file with each of your connectivity hypotheses. You can now compare
the evidence for your different models. Using the batch:

- In the batch editor select SPM -\> DCM -\> Second level -\> Compare /
  Average PEB models.
- Select the PEB model you created in the previous step.
- For DCMs, select the GCM_templates.mat file.
- Click the green play button

Here\'s the equivalent Matlab code:\<syntaxhighlight lang=\"matlab\"\>%
Compare nested PEB models. Decide which connections to switch off based
on the % structure of each template DCM in the GCM cell array. This
should have one row % and one column per candidate model (it doesn\'t
need to be estimated). BMA = spm_dcm_peb_bmc(PEB, GCM);
\</syntaxhighlight\>Two windows will be created. One titled BMC
(\"Bayesian Model Comparison\") shows the results of comparing the full
PEB model against the nested PEB models. The other window, titled
\"PEB - Review Parameters\" shows the estimated connection strengths at
the group level, averaged over PEBs (a Bayesian Model Average).

#### **Search over nested PEB models**

Rather than compare specific hypotheses, as described above, you may
wish to simply prune away any parameters from the PEB which don\'t
contribute to the model evidence. This approach (previously referred to
as post-hoc search) can be performed as follows.

- In the batch editor select SPM -\> DCM -\> Second level -\> Search
  nested PEB models.
- Select the PEB file from earlier and the GCM file, which contains the
  first level DCMs.
- Press the green play button

Here\'s the equivalent Matlab code:\<syntaxhighlight lang=\"matlab\"\>%
Search over nested PEB models. BMA = spm_dcm_peb_bmc(PEB);
\</syntaxhighlight\>Again, two windows will be created. One titled BMC
(\"Bayesian Model Comparison\") shows the connections that were switched
off - both for the group mean (commonalities across subjects) and first
two between-subjects effects. The other window, titled \"PEB - Review
Parameters\" shows the estimated group-level connection strengths,
averaged over PEB models identified by the search (a Bayesian Model
Average).

#### **Review results**

To review the results of a BMA analysis (or a PEB model), use the PEB
review function: \<syntaxhighlight lang=\"matlab\"\>% Review results
spm_dcm_peb_review(BMA,GCM)\</syntaxhighlight\> Note that in this line
of code, we have provided the group DCM (GCM) to the review function,
which provides useful information such as the names of the connections.

## Interpreting the output

### The model

To recap, the PEB model is a general linear model:

\<math\>\theta^{(1)} = X\theta^{(2)}+\epsilon^{(2)}\</math\>

Where \<math\>\theta^{(1)}\</math\>is the vector of all connectivity
parameters from all the individual subjects\' DCMs. Design matrix
\<math\>X\</math\> has regressors (columns) for the group average
strength of each connection, as well as the effect of each covariate on
each connection. For example, for a particular connection, there may be
a regressor for the group average and another regressor for the effect
of subjects\' age on that connection. The corresponding parameters
\<math\>\theta^{(2)}\</math\>quantify the effect of each covariate on
each connection and these are estimated from the data. The residual
between-subject variability (random effects) form the vector
\<math\>\epsilon^{(2)}\</math\>.

### The parameters

The estimated PEB parameters \<math\>\theta^{(2)}\</math\> are shown as
bar charts in the spm_dcm_peb_review GUI, with one bar chart per
covariate. More specifically, when the PEB model is estimated, a
multivariate normal probability density over the parameters is computed,
with expected values stored in the field PEB.Ep and covariance matrix
stored in the field PEB.Cp. The height of the bars correspond to the
expected values (Ep) and the error bars are 90% Bayesian confidence
intervals (credible intervals), computed from the leading diagonal of
the covariance matrix (Cp).

Some of the parameters in \<math\>\theta^{(2)}\</math\>encode the
commonalities over subjects. These are either the group mean of the DCM
connections over subjects, or the baseline connectivity, depending on
whether or not the covariates in the design matrix were mean-centred
(see Example design matrices, below). A positive parameter estimate
indicates the connection is positively associated with the covariate.
Conversely, negative values indicate a negative relationship between the
connection and the covariate.

The units of the PEB parameters \<math\>\theta^{(2)}\</math\> depend on
the units of the underlying DCM connectivity parameters. Some
connectivity parameters are in units of hertz (Hz), because they are
rates of change (inverse time constants). Other connectivity
parameters - those with positivity or negativity constraints, such as
the self-connections in DCM for fMRI - are unitless log scaling
parameters. These scale some default connection strength, which is in
Hz. For more detail, please see the relevant paper for the specific DCM
model being used. For DCM for fMRI, a detailed explanation of the units
can be found in the tutorial paper - [Part 1 on DCM for
fMRI](doi:10.1016/j.neuroimage.2019.06.031 "wikilink").

### Posterior probabilities

In the PEB review GUI (spm_dcm_peb_review.m), a probability can be found
for each parameter by clicking on the relevant bar in the bar chart. A
drop-down menu provides two different ways of calculating these
probabilities.

- If the option **Probability that parameter \> 0** is selected, then
  each parameter is considered in isolation, and the probability is
  computed based on how much of the area under the (marginal) posterior
  probability distribution is greater than zero. In other words, the
  probability relates to the pink error bars that are displayed on the
  bar chart. Note this does not take into account covariance among
  parameters.

<!-- -->

- Alternatively, when reviewing the results of a Bayesian model
  comparison or search, the option **Free energy (with vs without)** is
  displayed, which should be used if available. When performing an
  automatic search over reduced PEB models, these probabilities are
  computed by comparing the evidence for all models in which the
  particular connection was switched on, versus the evidence for all
  models in which the connection was switched off (out of the best 256
  models from the search). The same approach to computing probabilities
  is used if pre-defined models are provided, except probabilities will
  only be computed for parameters relating to the commonalities and the
  first group difference (covariate).

The GUI also provides the option to threshold based on these
probabilities, which is discussed in more detail below.

### Significance and thresholding

There is no concept of significance in Bayesian analysis - there is
simply a posterior probability for each effect or model. For example,
you are perfectly entitled to report an effect with a posterior
probability of 53%, 86% or 99%.

[Kass and Raftery (1995)](doi:10.1080/01621459.1995.10476572 "wikilink")
suggested descriptive labels for levels of evidence, listed in the table
below. For example, consider two models m1 and m2, with free energies
F1=4 and F2=7 respectively (the free energy is an approximation of the
log model evidence). Then the ratio of log evidences, called the log
Bayes factor, is the difference in the free energies: log BF = F1-F2 =
3. Assuming flat priors across models, this would correspond to a
posterior probability for model m2 of 95% (this can be computed using a
softmax function of the free energies for each model). From the table, a
log Bayes factor of 3 is described as \"strong evidence\". Thus, in a
paper, one could write \"the posterior probability for model m2 was 95%,
which corresponds to strong evidence in favour of this model\".

| Bayes factor | Log (base e) Bayes factor | Posterior probability | Evidence level                     |
|--------------|---------------------------|-----------------------|------------------------------------|
| 1 to 3       | 0 to 1                    | 0.5 to 0.73           | Not worth more than a bare mention |
| 3 to 20      | 1 to 3                    | 0.73 to 0.95          | Positive                           |
| 20 to 150    | 3 to 5                    | 0.95 to 0.99          | Strong                             |
| \> 150       | \> 5                      | \> 0.99               | Very strong                        |

Categories for levels of evidence from [Kass and Raftery
(1995)](doi:10.1080/01621459.1995.10476572 "wikilink")

No thresholding of effects based on their probability is required, as
there is no concept of significance with Bayesian analysis.
Nevertheless, if there are many parameters, it can be helpful to focus
the reader\'s attention on the most probable effects, e.g. by only
plotting those with strong evidence. Options for doing this a provided
in the PEB review GUI. Nevertheless, remember that this is only for
display purposes and sub-threshold parameters can still make an
important contribution to the model.

## Leave-one-out cross validation

Having identified one or more group effects in the results of the PEB
analysis, you may wish to ask if the effect on a particular connection
would be large enough to predict whether a new subject was in a
particular group, or predict a continuous regressor, such as a clinical
score. You can do this as follows.

- In the batch editor select SPM -\> DCM -\> Second level -\> Predict
  (cross-validation).
- Specify the PEB model exactly as for step 3. This time, under Field,
  you may wish to include one or two connections you have previously
  found expresses a significant between-subjects effect.
- Press the green play button.

Alternatively, with Matlab code:\<syntaxhighlight lang=\"matlab\"\>%
Perform leave-one-out cross validation (GCM,M,field are as before)
spm_dcm_loo(GCM,M,field); \</syntaxhighlight\>A PEB model will now be
estimated while leaving out a subject, and will be used to predict the
first between-subjects effect (after the constant column) in the design
matrix, based on the specific connections chosen. The resulting plot
shows the predicted group effect for each subject as well as the
correlation between the predicted score and known score. If the effect
to be predicted is binary (e.g. patient or control), then the bottom
plots show on a subject-by-subject basis how confident one can be of the
predicted group membership. If it\'s a continuous variable, like a
clinical score, then the plot shows the prediction accuracy.

## Example design matrices

Here we give some examples of how to define the between-subjects design
matrix, M.X, for some typical experimental designs. All the same
principles for the general linear model (GLM) also apply in the PEB
scheme, the only restriction being that the PEB software expects the
design matrix to start with a column of ones. For each type of
experimental design, there are multiple options for how to encode the
design matrix. Part of the decision of which design to use will be based
on interpretability - for example, do you want parameters encoding the
difference between groups, or the mean of each group separately?
Additionally, different designs will have different degrees of
statistical efficiency. For example, if the regressors are orthogonal
(statistically independent), then efficiency will be maximised. If in
doubt about which design is best, perform a Bayesian model comparison by
trying different options and choosing the one which gives the most
positive free energy PEB.F.

#### One between-subjects factor (two groups)

This can be modelled with two regressors, encoding 1) the group mean and
2) group difference relative to the mean:

\<math\>\begin{bmatrix} 1 & -1 \\ 1 & -1 \\ 1 & 1 \\ 1 & 1
\end{bmatrix}\</math\>

Here, the rows are the four subjects and the columns are the regressors.
The first two subjects are in group 1 (indicated by a value of -1 in the
second regressor) and the second two subjects are in group 2 (indicated
by value 1 in the second regressor). Note that the second regressor must
have a mean of zero for this interpretation to hold. If the group
difference regressor doesn\'t have mean of zero, you can set this
manually:\<syntaxhighlight lang=\"matlab\"\> X(:,2:end) = X(:,2:end) -
mean(X(:,2:end)) \</syntaxhighlight\>The ensuing PEB model will have
parameters encoding the group mean of each connection strength (due to
the first regressor) as well as parameters encoding the deviation from
the mean due to the group difference (thanks to regressor 2). For the
group difference, positive estimated parameters indicate stronger
connectivity in group 2 than group 1 and negative parameters indicate
stronger connectivity in group 1 than group 2.

Alternatively, if the second regressor is not mean-centred, then the
regressors are: 1) the connectivity of group 1, and 2) the difference
between groups:

\<math\>\begin{bmatrix} 1 & 0 \\ 1 & 0 \\ 1 & 1 \\ 1 & 1
\end{bmatrix}\</math\>

#### **One between-subject factor (three groups)**

Consider three groups of subjects, with two subjects per group. If a
linear effect of group is hypothesised - i.e. that group 1 \> group 2 \>
group 3 - this could be modelled using two regressors: 1) the overall
mean and 2) the difference between groups 1 and 3:

\<math\>\begin{bmatrix} 1 & 1 \\ 1 & 1 \\ 1 & 0 \\ 1 & 0 \\ 1 & -1 \\ 1
& -1 \end{bmatrix}\</math\>

Alternatively, if the objective is to test for the commonalities and
between-group differences (without specifying a linear effect), three
regressors can be used to encode: 1) the overall mean 2) the difference
between the first and second groups, and 3) the difference between the
second and third groups:

\<math\>\begin{bmatrix} 1 & -1 & 0 \\ 1 & -1 & 0 \\ 1 & 1 & -1 \\ 1 & 1
& -1 \\ 1 & 0 & 1 \\ 1 & 0 & 1 \end{bmatrix}\</math\>

Or, to model each group separately, a group can be chosen to serve as
the baseline (here, group 1), and the regressors encode: 1) the first
group, 2) the additive effect of being in the second group relative to
the first group, and 3) the additive effect of being in the third group
relative to the first group:

\<math\>\begin{bmatrix} 1 & 0 & 0 \\ 1 & 0 & 0 \\ 1 & 1 & 0 \\ 1 & 1 & 0
\\ 1 & 0 & 1 \\ 1 & 0 & 1 \end{bmatrix}\</math\>

#### **Unbalanced design with two between-subject factors (three groups)**

Consider an experiment with three groups: A) patients receiving a drug
B) patients receiving a placebo, and C) a single control group. This is
an example of an unbalanced design. Although the designs listed in the
previous section could be used, it may be more intuitive to use the
following three regressors: 1) baseline (controls), 2) the additive
effect of being a patient, and 3) the difference between drug and
placebo. With 2 subjects per group:

\<math\>\begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & -1 \\ 1 & 1 &
-1 \\ 1 & 0 & 0 \\ 1 & 0 & 0 \end{bmatrix}\</math\>

Note that these regressors are not orthogonal, so this will be a less
efficient experimental design than a fully balanced factorial design,
which is described next.

#### **Balanced factorial design with two between-subject factors (four groups)**

Consider an experiment with four groups of subjects in a balanced
factorial design: 1) patients receiving a treatment, 2) patients
receiving a placebo, 3) controls receiving a treatment and 4) controls
receiving a placebo. The regressors will encode: 1) the overall mean, 2)
the main effect of being a patient, 3) the main effect of treatment and
4) the interaction, i.e. the difference in drug effect between patients
and controls. With two subjects per group:

\<math\>\begin{bmatrix} 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 \\ 1 & 1 & -1 &
-1 \\ 1 & 1 & -1 & -1 \\ 1 & -1 & 1 & -1 \\ 1 & -1 & 1 & -1 \\ 1 & -1 &
-1 & 1 \\ 1 & -1 & -1 & 1 \end{bmatrix}\</math\>

Here, the final column - the interaction - is generated by element-wise
multiplication of the two main effects (which must be mean-centred
first). Note that this design gives rise to regressors that are
orthogonal - making the factorial design optimally efficient.

#### Covariates

In addition to the regressors described above, it is common to have
covariates, e.g. age or clinical scores. These can be added to the
design matrix, and it is generally a good idea to mean-centre them
(across all subjects) to enable the first regressor to be interpretable
as the mean. Note that for every covariate added to the design matrix,
one parameter is added per DCM connection, so you can end up with a lot
of parameters. Therefore, it is a good idea to be conservative and keep
the number of covariates to a minimum.

## Hierarchical experimental designs

Consider an experiment with a factorial design at the between-run level.
For example, we will imagine a study with two groups of subjects, where
one group received a drug and the other received a placebo, and each
subject was scanned twice: pre- and post-treatment. This is a balanced
2x2 design with cells or experimental conditions:

- Group 1 pre-treatment (GCM1_pre)
- Group 1 post-treatment (GCM1_post)
- Group 2 pre-treatment (GCM2_pre)
- Group 2 post-treatment (GCM2_post)

We\'ll assume that a separate DCM was fitted for each of these four
experimental conditions for each subject. The names in brackets are
example variable names to store the DCMs for each condition. Each
variable is a cell array with one subject per row. Here are two ways to
model a design of this sort using PEB.

#### Option 1: A 2-level design (first level: DCM, second level: PEB)

Create a PEB model in which the design matrix X has the following
regressors:

1.  Overall mean
2.  Main effect of group (-1s for group 1, 1s for group 2, and then
    mean-corrected)
3.  Main effect of treatment (-1s for pre-treatment, 1s for
    post-treatment, and then mean-corrected)
4.  Interaction of group and treatment (the two mean-corrected main
    effects element-wise multiplied)

And fit this PEB model to all DCMs across subjects and time points (in
the right order to match the regressors you create), e.g:
\<syntaxhighlight lang=\"matlab\"\> GCM = {GCM1_pre; GCM1_post;
GCM2_pre; GCM2_post}; PEB = spm_dcm_peb(GCM, X); \</syntaxhighlight\>An
additional technical consideration arises if you wish to compare the
evidence for pre-defined alternative PEB models, because the evidence
for each model will only be compared in terms of the first two
regressors (in this example, the overall mean and the main effect of
group). To compare pre-defined models for all of these 4 four factors,
simply re-run the PEB analysis with design matrix re-ordered so that the
effect of interest is the second regressor. For example, to investigate
which model best explains the effect of treatment, re-order the columns
to: Mean, Treatment, Group, Interaction and then run spm_dcm_peb and
spm_dcm_peb_bmc (or use the batch).

#### Option 2: A 3-level design (first level: DCM, second and third levels: PEB)

Alternatively, the same design can be modelled in a 3-level hierarchy,
using the PEB-of-PEBs approach, where a PEB model is created for each
group separately. For group 1, this will have a design matrix X1 with
regressors:

1.  Mean of group 1
2.  Effect of treatment on group 1 (-1s for pre-treatment, 1s for
    post-treatment, and then mean-corrected)

And this will be fitted to all DCMs from group 1 in a single column
vector:\<syntaxhighlight lang=\"matlab\"\> GCM1 = \[GCM1_pre;
GCM1_post\]; PEB1 = spm_dcm_peb(GCM1, X1); \</syntaxhighlight\>For group
2, the PEB model will have a design matrix X2 with regressors:

1.  Mean of group 2
2.  Effect of treatment on group 2 (-1s for pre-treatment, 1s for
    post-treatment, and then mean-corrected)

And this will be fitted to the DCMs of group 2:\<syntaxhighlight
lang=\"matlab\"\> GCM2 = \[GCM2_pre; GCM2_post\]; PEB2 =
spm_dcm_peb(GCM2, X2); \</syntaxhighlight\>The ensuing PEB models PEB1
and PEB2 will have parameters encoding each of the the two effects (mean
and treatment) on each DCM connection. Now, take the parameters of the
PEB models up to the third level of the hierarchy, to identify
commonalities and differences across groups. The design matrix X3 will
contain regressors:

1.  Mean of all subjects
2.  Group difference (-1 for group 1, 1 for group 2, and then
    mean-corrected)

I.e., the design matrix will be of dimension 2x2, with values: \[1 -1; 1
1\]. This is fitted to the PEB parameters from the 2nd
level:\<syntaxhighlight lang=\"matlab\"\>

PEBs = {PEB1; PEB2}; PEB3 = spm_dcm_peb(PEBs,X3);
\</syntaxhighlight\>The final model 3rd level PEB model, named PEB3,
will include parameters for the group mean and group difference on each
of the 2nd level effects. i.e. for every connection there will be a
parameter for:

- The overall mean connectivity
- The main effect of treatment
- The main effect of group
- The interaction of group and treatment

To perform Bayesian model reduction and review the results, type:

\<syntaxhighlight lang=\"matlab\"\> BMA3 = spm_dcm_peb_bmc(PEB3);
spm_dcm_peb_review(BMA3); \</syntaxhighlight\>

NB: When prompted for a GCM array, click the cancel button, in order for
the PEB-of-PEB results to display correctly. The need for this step will
be resolved in a future release of SPM.

#### Which option to use?

Option 1 is simpler, but option 2 might offer a better model of between
subjects variability (random effects). To assess which is the better
option, you can try both and compare the free energy. To do this,
subtract the free energies of each model: PEB.F - PEB3.F . A positive
number indicates option 1 is better, a negative number indicates option
2 is better. And make sure to feed back to the SPM mailing list on what
you find - this will be useful for developing best practice.

## **Further reading**

- [Tutorial papers and step-by-step guide to group DCM studies with
  PEB](https://github.com/pzeidman/dcm-peb-example)
