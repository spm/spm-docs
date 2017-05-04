# DCM

Dynamic Causal Modelling \(DCM\) is a framework for specifying, estimating and comparing models of neuroimaging data. It is particularly used for studying effective connectivity - the causal influences between brain regions.

A DCM analysis generally consists of three steps:

---

##### Model specification

The forward or generative model is a mathematical description of how experimental stimuli causes changes in brain activity, and how brain activity gives rise to the signals measured using imaging devices \(e.g. fMRI, MEG, EEG\).

![](/theory/dcm/stim-neural-observation.png)

The forward model is split into two parts: a neuronal model and an observation model. The neuronal model $$f$$ specifies how brain activity changes over time:


$$
\dot{x}=f(x,u,\theta^{(n)})
$$


Where $$x$$ is the neural activity of each  brain region, $$\dot{x}$$ is the change in $$x$$ over time \(its temporal derivative\), $$u$$ is the experimental input and $$\theta^{(n)}$$ are the parameters \(for instance, the connection strengths\). The observation model $$g$$ predicts the timeseries one would expect to measure given this brain activity:


$$
y = g(x,\theta^{(h)})
$$


Where $$\theta^{(h)}$$ are the parameters of the observation model. The particular implementation of $$f$$ and $$g$$ depend on the model, and are explained in more detail in the following chapters.

Having specified a forward model which describes the chain of events from stimulation, to brain activity to observations, the next step is to fit the model to the data.

---

##### Model inversion

Having a forward model allows us to investigate things which we cannot directly measure. In neuroimaging, we are interested in parameters $$\theta^{(n)}$$, which represent some interesting quantity about neural activity, such as connection strengths or sensitivity of neural populations to experimental inputs. To estimate these parameters, we start from the measured data, and ask which particular settings of the parameters best explain the data. This is called model estimation or model inversion.

DCM is Bayesian or probabilistic framework. As such, all parameters $$\theta=(\theta^{(n)},\theta^{(h)})$$ have prior probability distributions, which represent our beliefs about the parameters before collecting any data. These priors beliefs are combined with experimental data to give updated \(posterior\) beliefs. Bayes theorem states:


$$
 p(\theta|y,m) = \frac{p(y|\theta,m)p(\theta|m)}{p(y|m)}
$$
Where $$p(\theta|y,m)$$ are the posterior beliefs about the parameters after seeing the data, $$p(y|\theta,m)$$ is the prediction of the forward model about the data given a particular setting of the parameters, $$p(\theta|m)$$ are the prior beliefs about the parameters, and $$p(y|m)$$ is the probability of the data given the model, collapsed over possible settings of the parameters. This is a particularly useful part of the equation called the model evidence. Here, $$m$$ is the model, for instance a DCM or a GLM.



