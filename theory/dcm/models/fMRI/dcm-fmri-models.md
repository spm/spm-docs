# DCM models for fMRI

This page gives an overview of the different neural models available for analysing fMRI data.

| Model | Description |
| --- | --- |
| One-state DCM | The basic DCM model for fMRI |

---

### One-state DCM

This is the most basic neuronal model for DCM for fMRI. Activity in each region is represented by a single number \(hidden state\).

#### The model

The state equation, which describes how experimental inputs give rise to brain activity, is written as follows:


$$
 \dot{z} = (A + \sum{u_jB^j})z + Cu
$$


Where $$\dot{z}$$ is a vector with one element per brain region, representing the rate of change in neuronal activity with respect to time. It is a function of three sets of parameters: matrix $$A$$ \(the baseline or average connectivity\), matrix $$B^j$$ \(the modulatory influence of experimental input $$j$$ on each connection\) and matrix $$C$$ \(the driving input of each experimental input on each region\). Matrix $$u$$ contains the experimental manipulations which are hypothesised to drive or modulate the network. These are usually boxcar regressors representing the on-off times for each experimental condition, but they can also include parametric regressors representing the level of some experimental manipulation or the output of a behavioural model.

#### Parameters

The model has three sets of parameters:

| Parameter | Dimension | Directory |
| —- | —- | —- |
| A | n x n matrix where n is the number of regions | Average or baseline connectivity within and between regions. Entries on the diagonal of the matrix represent self connections and the off-diagonal entries represent between-region connections. |
| B | n x n x u matrix where u is the number of experimental inputs | The modulatory influence of each experimental input on each connection. This often represents the context of the experimental manipulation. For instance, at certain time points, the participant may be instructed to pay attention. The entries of the B-matrix would then be used to represent the influence of attention on the coupling between regions or the self-connections within regions. |
| C | n x u matrix | The driving effect of each experimental input on each region. |
