# DCM

Dynamic Causal Modelling \(DCM\) is a framework for specifying, estimating and comparing models of neuroimaging data. It is particularly used for studying effective connectivity - the causal influences between brain regions.

A DCM analysis generally consists of several steps:

##### Model specification

The forward or generative model is a mathematical description of how experimental stimuli causes changes in brain activity, and how brain activity gives rise to the signals measured using imaging devices \(e.g. fMRI, MEG, EEG\).



![](/theory/dcm/stim-neural-observation.png)

The forward model is split into two parts: a neuronal model and an observation model. The neuronal model $$f$$ specifies how brain activity changes over time:

$$\dot{x}=f(x,u)$$

Where $$x$$ is the neural activity of each  brain region, $$\dot{x}$$ is the change in $$x$$ over time \(its temporal derivative\) and $$u$$ is the experimental input. 

