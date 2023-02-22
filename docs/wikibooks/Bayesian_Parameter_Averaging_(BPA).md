# Bayesian Parameter Averaging (BPA)

Bayesian Parameter Averaging \<Ref
name=\"PennyBPA\"\><http://www.fil.ion.ucl.ac.uk/~wpenny/publications/parameter_averaging.pdf%3C/Ref%3E>;
is a fixed effects average of parameters over several DCMs. The models
need to have the same structure, but can be of different data - e.g. you
could take an average of the same model fitted to several subjects\'
data. Being a fixed effects average, it is appropriate to use this
approach when one assumes that all subjects have the same underlying
model.

## Theory

Let\'s say we have \<math\>N\</math\> subjects and we wish to average
the parameters across one model from each subject. For each model
\<math\>i\</math\> we have a posterior covariance matrix
\<math\>\Sigma_i\</math\>, which contains the variance (on the diagonal)
and the covariance (off-diagonal) of the parameter estimates. The
inverse of this is the precision matrix \<math\>\Lambda_i\</math\>,
which we\'ll use more often than the covariance. Each model also has a
vector of estimated parameter means, \<math\>\mu_i\</math\>. The priors
are assumed to be the same for all models, with prior precision matrix
\<math\>\Lambda\_{0}\</math\> and vector of prior means
\<math\>\mu\_{0}\</math\>.

Bayes law for Gaussians says that precisions are combined by addition,
whereas means are combined by summing each mean weighted by its
(normalized) precision:

\<math\> \Lambda = (\Lambda_i + \Lambda_0) \</math\>

\<math\> \mu = \Lambda^{-1} \Big\[(\Lambda_i \mu_i) + (\Lambda_0
\mu_0)\Big\] \</math\>

The Bayesian Parameter Average across models is derived from this. It
has posterior precision:

\<math\>\Lambda=\Big(\sum\limits\_{i=1}^N \Lambda_i\Big) - (N-1)
\Lambda_0\</math\>

And posterior mean:

\<math\>\mu=\Lambda^{-1}\Bigg\[\Big(\sum\limits\_{i=1}^N
\Lambda_i\mu_i\Big) - (N-1) \Lambda_0 \mu_0\Bigg\]\</math\>

For a full derivation of these formulae, see the technical note by Will
Penny \<Ref
name=\"PennyBPA\"\><http://www.fil.ion.ucl.ac.uk/~wpenny/publications/parameter_averaging.pdf%3C/Ref%3E>;.

## Practice

**GUI:** Click \"Dynamic Causal Modelling\", then click \"Average\",
then \"BPA\". Select the models you wish to include and a name for the
output.

**Scripting:** Use the Matlab function

\<syntaxhighlight
lang=\"matlab\"\>spm_dcm_average(P,name)\</syntaxhighlight\>

where \'P\' is a cell array of model filenames and \'name\' is the name
with which to save the result.

With either method, a new model will be saved with the name
\"DCM_avg_name.mat\". The contents of this averaged model are identical
to those of the first model you selected as input, except for the
following:

- DCM.Ep (averaged posterior means, \<math\>\mu\</math\>)
- DCM.Cp (averaged posterior covariance matrix,
  \<math\>\Lambda^{-1}\</math\>)
- DCM.Vp (averaged posterior variance vector of each parameter - the
  diagonal of DCM.Cp)
- DCM.Pp (averaged posterior probability of each parameter).

## Frequently Asked Questions

**Why are my estimated parameter means larger / smaller than I expect?**

First, bear in mind that this is a bayesian average, so the priors will
have an impact on the outcome. If there were two parameters which both
had prior mean 0 and posterior mean 0.7, you shouldn\'t be surprised if
the average is not exactly 0.7, but rather is closer to the prior, 0.
The extent to which the average is drawn towards the prior will depend
on the prior\'s precision.

Second, some very unintuitive results can be found when parameters are
correlated, as described by Kasess and colleagues \<ref
name=\"KasessStephan2010\"\>\</ref\>. The precision matrices in the
formulae above reflect not only the variance of each parameter
individually, but also the (off-diagonal) covariance between parameters.
If the posterior covariances are non-zero, then the posterior mean of
the BPA will deviate from the arithmetic mean. In fact, with certain
combinations of covariances, the average of a parameter in two models
could be outside the range of each parameter mean separately.

Kasess et al. \<ref name=\"KasessStephan2010\"/\> compared BPA (as
described above) against \'Posterior Variance Weighted Averaging\'
(PVWA), which is the same process but with the covariance between
parameters set to zero before averaging. This avoids the unintuitive
result described above, but at the cost of not taking the covariance
structure into account. They found that with signal-to-noise ratios of
1-2, which may be typical for fMRI data, the two approaches gave similar
results. Anyone wishing to use this in SPM can call spm_dcm_average with
the third parameter (\'nocond\') set to true:

\<syntaxhighlight lang=\"matlab\"\>nocond=true;
spm_dcm_average(P,name,nocond)\</syntaxhighlight\>

**What\'s the difference between Bayesian Parameter Averaging (BPA) and
Bayesian Model Averaging (BMA)?**

BPA averages parameters across models without any regard for the
probability of the models themselves. BMA, by contrast, weights
parameter estimates by the probability of the model which contains them.
As such, it can be used to average over models of different structure as
well as across subjects. BMA can be used for random-effects analysis,
whereas BPA cannot.

## References

\<references/\>
