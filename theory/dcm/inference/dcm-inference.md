# Inference

In this chapter we look at how to test hypotheses by comparing models and examining parameters. The methods described here are generic and can be to any Bayesian models. After introducing these important basics, we move onto how we deal with analyses across multiple subjects.



Bayes Factors



As described in the model inversion chapter, estimating a model provides an approximation of its log model evidence: $$log p\(y\|m\)$$. The approximation used in SPM is the negative variational free energy, or free energy for short. The free energy can be decomposed into two terms: the accuracy of the model minus its complexity. It's a tremendously useful quantity for the experimenter, because it's a single number which scores how good a model is relative to some other model. By embodying several hypotheses as models, the hypothesis with strongest evidence can be identified.



Two or more models can easily be compared by computing the Bayes factor. This is the ratio of the model evidence of one model relative to the other:



$$Bf = p\(y\|m1\) / p\(y\|m2\)$$



If this number is greater than 1, we can conclude that model 1 is better than model 2. How much larger? A common practice is to take a bayes factor of 20 as 'strong evidence' that model 1 is better.



Because we are working with the free energy which approximates the log of the model evidence, we generally work with log of the bayes factor:



$$log bf = log p\(y\|m1\) - log p\(y\|m2\) = F\_1 - F\_2$$



With logarithms, division becomes subtraction. So to compare two models, we simply subtract their free energies, which gives us the log bayes factor. The threshold for 'strong evidence' becomes log\(20\)=3, so with a difference of 3 or more, we can confidently conclude that one model is better than the other.



This method generalises to more than 2 models by computing the Bayes factor for each model relative to a reference model. Generally the worst model is selected to be the reference, so if we have a vector of free energies F then:



logBF = F - min\(F\)



These can then be plotted \(see figure below, left\). The worst model has a value of zero and any difference greater than 3 is strong evidence of a difference.



We can also plot these results as the probability that one model is better than another.



...



Parameters 



By estimating a Bayesian model using SPM, we are supplied with a multivariate normal distribution of the estimated parameters. This consists of the expected value of each parameter and a covariance matrix, which quantifies the uncertainty about the parameters. The leading diagonal of the covariance matrix is the variance of each parameter and the off-diagonal elements are the covariances between each pair of parameters.



As shown in the figure, parameters can conveniently be plotted by displaying their expected values \(gray bars\) and computing their 95% confidence intervals \(pink bars\). The confidence interval is:



...



We can also compute the probability that these parameters have deviated from their prior expectations \(which was zero for all parameters\):



...



Although useful for inspecting the parameters, these calculations don't give the full picture, because they ignore the covariances between parameters. To visualise the covariances we can convert the covariance matrix to a correlation matrix \(figure, right panel\).



...



Therefore, to ask a question like "is this parameter contributing to my model" it is not enough to compare its expected value against zero.





