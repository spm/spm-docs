# DCM

Dynamic Causal Modelling \(DCM\) is a framework for specifying, estimating and comparing models of neuroimaging data. It is particularly used for studying effective connectivity - the causal influences between brain regions.

A DCM analysis generally consists of several steps:

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

##### Model inversion



