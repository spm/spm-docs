# Bayesian Parameter Averaging (BPA)

Bayesian Parameter Averaging[^1] is a fixed effects average of
parameters over several DCMs. The models need to have the same
structure, but can be of different data - e.g. you could take an
average of the same model fitted to several subjects' data.
Being a fixed effects average, it is appropriate to use this
approach when one assumes that all subjects have the same underlying
model.

## Theory

Let\'s say we have $N$ subjects and we wish to average
the parameters across one model from each subject. For each model
$i$ we have a posterior covariance matrix
$\Sigma_i$, which contains the variance (on the diagonal)
and the covariance (off-diagonal) of the parameter estimates. The
inverse of this is the precision matrix $\Lambda_i$,
which we'll use more often than the covariance. Each model also has a
vector of estimated parameter means, $\mu_i$. The priors
are assumed to be the same for all models, with prior precision matrix
$\Lambda_{0}$ and vector of prior means $\mu_{0}$.

Bayes law for Gaussians says that precisions are combined by addition,
whereas means are combined by summing each mean weighted by its
(normalized) precision:

$$ \Lambda = (\Lambda_i + \Lambda_0)$$

$$ \mu = \Lambda^{-1} \Big[(\Lambda_i \mu_i) + (\Lambda_0
\mu_0)\Big]$$

The Bayesian Parameter Average across models is derived from this. It
has posterior precision:

$$\Lambda=\Big(\sum_{i=1}^N \Lambda_i\Big) - (N-1) \Lambda_0$$

And posterior mean:

$$\mu=\Lambda^{-1}\Bigg[\Big(\sum_{i=1}^N
\Lambda_i\mu_i\Big) - (N-1) \Lambda_0 \mu_0\Bigg]$$

For a full derivation of these formulae, see the technical note by Will
Penny[^1].

## Practice

**GUI:** Click "Dynamic Causal Modelling", then click "Average",
then "BPA". Select the models you wish to include and a name for the
output.

**Scripting:** Use the MATLAB function

```matlab
spm_dcm_average(P,name)
```

where `P` is a cell array of model filenames and `name` is the name
with which to save the result.

With either method, a new model will be saved with the name
`DCM_avg_name.mat`. The contents of this averaged model are identical
to those of the first model you selected as input, except for the
following:

- `DCM.Ep` (averaged posterior means, $\mu$)
- `DCM.Cp` (averaged posterior covariance matrix, $\Lambda^{-1}$)
- `DCM.Vp` (averaged posterior variance vector of each parameter - the
  diagonal of `DCM.Cp`)
- `DCM.Pp` (averaged posterior probability of each parameter).

## Frequently Asked Questions

**Why are my estimated parameter means larger / smaller than I expect?**

First, bear in mind that this is a bayesian average, so the priors will
have an impact on the outcome. If there were two parameters which both
had prior mean 0 and posterior mean 0.7, you shouldn't be surprised if
the average is not exactly 0.7, but rather is closer to the prior, 0.
The extent to which the average is drawn towards the prior will depend
on the prior's precision.

Second, some very unintuitive results can be found when parameters are
correlated, as described by Kasess and colleagues[^2].
The precision matrices in the
formulae above reflect not only the variance of each parameter
individually, but also the (off-diagonal) covariance between parameters.
If the posterior covariances are non-zero, then the posterior mean of
the BPA will deviate from the arithmetic mean. In fact, with certain
combinations of covariances, the average of a parameter in two models
could be outside the range of each parameter mean separately.

Kasess et al.[^2] compared BPA (as
described above) against 'Posterior Variance Weighted Averaging'
(PVWA), which is the same process but with the covariance between
parameters set to zero before averaging. This avoids the unintuitive
result described above, but at the cost of not taking the covariance
structure into account. They found that with signal-to-noise ratios of
1-2, which may be typical for fMRI data, the two approaches gave similar
results. Anyone wishing to use this in SPM can call spm_dcm_average with
the third parameter (`nocond`) set to true:

```matlab
nocond=true;
spm_dcm_average(P,name,nocond)
```

**What's the difference between Bayesian Parameter Averaging (BPA) and
Bayesian Model Averaging (BMA)?**

BPA averages parameters across models without any regard for the
probability of the models themselves. BMA, by contrast, weights
parameter estimates by the probability of the model which contains them.
As such, it can be used to average over models of different structure as
well as across subjects. BMA can be used for random-effects analysis,
whereas BPA cannot.

## References

[^1]: [https://www.fil.ion.ucl.ac.uk/~wpenny/publications/parameter_averaging.pdf](https://www.fil.ion.ucl.ac.uk/~wpenny/publications/parameter_averaging.pdf)

[^2]: Kasess, Christian Herbert; Stephan, Klaas Enno; Weissenbacher, Andreas; Pezawas, Lukas; Moser, Ewald; Windischberger, Christian (2010). "Multi-subject analyses with dynamic causal modeling". NeuroImage 49 (4): 3065–3074. doi:10.1016/j.neuroimage.2009.11.037. ISSN 10538119.
