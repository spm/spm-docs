
# DCM models for fMRI
This page gives an overview of the different neural models available for analysing fMRI data.

| Model | Description |
| -- | -- |
| [Deterministic DCM](#Deterministic-DCM) | The basic DCM model for fMRI |

## Deterministic DCM

This is the most basic neuronal model for DCM for fMRI. Activity in each region is represented by a single number (hidden state).

### The model

The state equation, which describes how experimental inputs give rise to brain activity, is formed as follows:

$$
\dot{z} = (A + \sum{u_jB^j})z + Cu
$$
 Here, the change in neuronal activity with respect to time $$\dot{z}$$ is controlled by three sets of parameters: matrix A (connections within and between regions, representing baseline or average connectivity), matrix $$B^j$$ (representing the modulatory influence of experimental input $$j$$ on each connection) and matrix $$C$$ (representing the driving input of each experimental input on each region.



