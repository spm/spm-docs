# DCM

Dynamic Causal Modelling \(DCM\) is a framework for specifying, estimating and comparing models of neuroimaging data. It is particularly used for studying effective connectivity - the causal influences between brain regions.

A DCM analysis generally consists of three steps: [model specification](#model-specification), [model inversion](#model-inversion) and [inference](#inference). These are outlined below in brief and are detailed in the follow chapters.

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

[Read more about the forward models in DCM](/theory/dcm/models/dcm-models.md)

---

##### Model inversion

A forward model allows us to investigate things we cannot directly measure. In neuroimaging, we are particularly interested in  parameters $$\theta^{(n)}$$, which represent some interesting quantity about the brain, such as the strengths of connections or the sensitivity of neural populations to experimental inputs. To estimate these parameters, we run the model in reverse. We start from the measured data $$y$$, and ask which particular settings of the parameters $$\theta=(\theta^{(n)},\theta^{(h)})$$ best explain the data. This process is called model estimation or model inversion. The "inverse problem" is generally more challenging than specifying the forward model, because it is often ill-posed - different possible neural configurations could give rise to the same observed timeseries.

![](/theory/dcm/forward_inverse_problems.png)

\(Brain image adapted from image by [IsaacMao](https://www.flickr.com/photos/isaacmao/19245594/in/photolist-2GD3A-6MaCW8-dmktpf-64zrPn-64zrPt-9UwYi-4AkYYV-84cP5K-sq4RNt-NDMUU-cgJcUs-8bFv9f-dMPrVr-J8bQCu-vKCLx-dcVGb3-645D1o-gayZDq-jypVk8-wTEZDo-xbQUur-5vGNkE-bPewqD-qDbwbV-9UwYp), [CC By 2.0](https://creativecommons.org/licenses/by/2.0/)\)

DCM addresses the inverse problem using Bayesian or probabilistic methods. This means that all parameters have prior probability distributions, which represent our beliefs about these parameters before collecting any data. These priors beliefs are combined with experimental data to give updated \(posterior\) beliefs. Bayes theorem states:


$$
 p(\theta|y,m) = \frac{p(y|\theta,m)p(\theta|m)}{p(y|m)}
$$


Where $$p(\theta|y,m)$$ are the posterior beliefs about the parameters after seeing the data, $$p(y|\theta,m)$$ is the forward model's prediction of what data we would expect to measure given a particular setting of the parameters, $$p(\theta|m)$$ are the prior beliefs about the parameters, and $$p(y|m)$$ is how probable the observed data are, given the model \(after collapsing over possible settings of the parameters\). This last term is called the model evidence, and it's particularly useful because it quantifies how good the model is, in terms of balancing accuracy and complexity. \(In these equations, $$m$$ refers to the particular model being used, i.e. the set of equations representing the DCM forward model.\)

DCM estimates the posterior distribution of the parameters $$p(\theta|y,m)$$ and an approximation of the \(log\) model evidence $$\ln{p(y|m)}$$, using an estimation scheme called variational Laplace.

Read more about model inversion in DCM

---

##### Inference

The purpose of DCM is to test hypotheses. This can be achieved by specifying two or more models with different priors $$p(\theta|m)$$. For instance, one model's priors could say, with absolute confidence, that a particular connection between brain regions does not exist. A second model could have priors that do allow this connection to exist. After inverting each model, the models can be compared in terms of their log model evidence. The model with the stronger evidence can be declared the best available explanation for the data. Additionally, the estimated parameters of the model\(s\) can be examined and reported, for instance to identify which connections are strong and which are weak.

In practice, studies generally have multiple subjects and aim to characterise brain connectivity at the group level. We therefore need to collate our parameters and model evidences across subjects. There are several mechanisms for doing this, which are described in the page on group DCM analysis.

Read more about inference with DCM

---

[Back to top](#dcm)

