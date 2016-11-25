# DCM models for fMRI

This page gives an overview of the different neural models available for analysing fMRI data.

| Model | Description |
| --- | --- |
| One-state DCM | The basic DCM model for fMRI |

---

## One-state DCM

This is the most basic neuronal model for DCM for fMRI. Activity in each region is represented by a single number \(hidden state\).

### The model

We summarise the level of neural activity in each brain region by a single number $$z_i$$ and collate these into a vector $$z$$. This *state vector* represents hidden neural activity - hidden in the sense that we cannot directly measure it. The change in z over time $$\dot{z}$$ is then function:
$$
\dot{z}=F(z,u,\theta^n)
$$In words, the rate of change in each brain region's activity is a function of its current activity $$z$$, experimental inputs $$u$$ and connection parameters $$\theta^n$$. The definition of $$F$$ depends on the DCM model being used.

In the one-state DCM model, $$F$$ is written as follows:


$$
 \dot{z} = (A + \sum{u_jB^j})z + Cu
$$
This state equation is a function of three sets of parameters: matrix $$A$$ \(the baseline or average connectivity\), matrix $$B^j$$ \(the modulatory influence of experimental input $$j$$ on each connection\) and matrix $$C$$ \(the driving input of each experimental input on each region\). Matrix $$u$$ contains the experimental timeseries (e.g. boxcar on-off regressors) which are hypothesised to drive or modulate the network. 

### Parameters

Given n regions and u experimental inputs:

| Parameter | Dimension | Directory |
| --- | --- | --- |
| A | n x n | Average or baseline connectivity within and between regions.  |
| B | n x n x u | The modulatory influence of each experimental input on each connection. |
| C | n x u | The driving effect of each experimental input on each region. |

#### Matrix A

Matrix A represents the causal influence of one neural population on another, i.e. how the rate of change in neural activity depends on the neural activity itself:
$$
A = \frac{\partial\dot{z}}{\partial z}
$$
Entries on the diagonal of the A-matrix are self connections and the off-diagonal entries are between-region connections.

The units for A,B and C are Hz (rates of change). To force the self-connections to always be negative, the values of the self-connections on A and B are scaling parameters, which scale the prior of -0.5Hz. The values in the A and B matrices undergo the following transformation to give the strength of the self-connection $$S_{i}$$ for region $$i$$:
$$
S_{i}=-0.5*exp(A_{ii} + B_{ii})
$$

#### Matrix B
Matrix $$B^j$$ is the change in the strength of particular connections caused by the $$j^{th}$$ experimental input. i.e. 
$$
B^j = \frac{\partial A}{\partial u_j}
$$
In words, matrix $$B^j$$ is the change in the effective connectivity $$A$$ caused by the $$j^{th}$$ experimental input.

This often represents the context of the experimental manipulation. For instance, at certain time points, the participant may be instructed to pay attention. The entries of the B-matrix would then be used to represent the influence of attention on the coupling between specific regions or the self-connections within regions.


