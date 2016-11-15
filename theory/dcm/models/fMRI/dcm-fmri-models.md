
# DCM models for fMRI
This page gives an overview of the different neural models available for analysing fMRI data.

| Model | Description |
| -- | -- |
| [Deterministic DCM](#Deterministic-DCM) | The basic neural model for fMRI |


---


### Deterministic DCM

This is the most basic neuronal model for DCM for fMRI. Activity in each region is represented by a single number (hidden state).

#### The model

The state equation, which describes how experimental inputs give rise to brain activity, is written as follows:

$$
 \dot{z} = (A + \sum{u_jB^j})z + Cu 
$$
 Where $$\dot{z}$$ is a vector with one element per brain region, representing the rate of change in neuronal activity with respect to time. It is a function of three sets of parameters: matrix $$A$$ (the baseline or average connectivity), matrix $$B^j$$ (the modulatory influence of experimental input $$j$$ on each connection) and matrix $$C$$ (the driving input of each experimental input on each region). Matrix $$u$$ contains the experimental manipulations which are hypothesised to drive or modulate the network. These are usually boxcar regressors representing the on-off times for each experimental condition, but they can also include parametric regressors representing the level of some experimental manipulation or the output of a behavioural model.
 
 

