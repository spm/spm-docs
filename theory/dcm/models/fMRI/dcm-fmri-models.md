
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
 The rate of change in neuronal activity with respect to time $$\dot{z}$$ is controlled by three sets of parameters: matrix A (connections within and between regions, representing baseline or average connectivity), matrix $$B^j$$ (the modulatory influence of experimental input $$j$$ on each connection) and matrix $$C$$ (the driving input of each experimental input on each region). Matrix $$u$$ contains the experimental timeseries. These could be boxcar on-off times or parametric regressors representing the level of some experimental manipulation.
 
 

