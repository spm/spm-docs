# Inference

This chapter covers how to test hypotheses by comparing models and inspecting parameters. The methods described here are generic and can be to any Bayesian models. After introducing the basics, we'll move onto performing analyses across multiple subjects.

##### Comparing models

As described in the chapter on model inversion, estimating a model provides an approximation of its log model evidence: $$\ln{p(y|m)}$$. The approximation used in SPM is the negative variational free energy, or free energy for short. The free energy can be decomposed into two terms: the accuracy of the model minus its complexity: 


$$
F \approx \ln{p(y|m)} = \text{accuracy} - \text{complexity}
$$


The free energy $$F$$ a tremendously useful quantity for the experimenter, because it's a single number which scores how good a model is for explaining the data. By embodying several hypotheses as models, the models can be compared and those with the strongest evidence identified.

Two or more models can easily be compared by computing the Bayes factor. This is the ratio of the model evidence of one model relative to the other:


$$
\text{bf} = \frac{p(y|m_1)}{p(y|m_2)}
$$


If the Bayes factor is greater than 1, we can conclude that model 1 is better than model 2. Common practice is to take a Bayes factor of 20 as being 'strong evidence' in favour of one model over the other. Because we are working with the log of the model evidence, we generally work with log of the Bayes factor. With logarithms, division becomes subtraction, so:


$$
\ln{\text{bf}} = \ln{ p(y|m_1)} - \ln{ p(y|m_2)} = F_1 - F_2
$$


Where $$F_1$$ is the free energy of model 1 and $$F_2$$ is the free energy of model 2. So to compare two models, we simply subtract their free energies, which gives us the log Bayes factor. The threshold for 'strong evidence' becomes $$\ln(20)=3$$, so with a difference in free energy of 3 or more, we can confidently conclude that one model is better than the other.

We can generalise this method to more than 2 models by computing the Bayes factor for each model, relative to a selected reference model. Generally the worst model is selected to be the reference, so if we have a vector of free energies F then in Matlab code:

```
logBF = F - min(F);
```

These can then be plotted \(see figure below, left\). The worst model has a value of zero and any difference greater than 3 is strong evidence of a difference.

We can also plot these results as the probability that one model is better than another.

...

##### Inference on parameters

By estimating a Bayesian model using SPM, we are supplied with a multivariate normal distribution of the estimated parameters. This consists of the expected value of each parameter and a covariance matrix, which quantifies the uncertainty about the parameters. The leading diagonal of the covariance matrix is the variance of each parameter and the off-diagonal elements are the covariances between each pair of parameters.

As shown in the figure, parameters can conveniently be plotted by displaying their expected values \(gray bars\) and computing their 95% confidence intervals \(pink bars\). The 95% confidence interval for the $$jth$$ parameter $$\text{ci}_j$$ with expected value $$\mu_j$$ and standard deviation $$\sigma_j$$ is defined by:


$$
\begin{aligned}c_j &= \text{icdf}(0.95) . \sigma_j \\ \text{ci}_j &= [\mu_j - c_j , \mu_j + c_j]\end{aligned}
$$


Where $$\text{icdf}$$ is the inverse normal cumulative distribution function.

We can also compute the probability that these parameters have deviated from their prior expectations \(which was zero for all parameters\):

...

Although useful for inspecting the parameters, these calculations don't give the full picture, because they ignore the covariances between parameters. To visualise the covariances we can convert the covariance matrix to a correlation matrix \(figure, right panel\).

...

Therefore, to ask a question like "is this parameter contributing to my model" it is not enough to compare its expected value against zero.

