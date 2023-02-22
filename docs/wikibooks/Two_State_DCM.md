# Two-State Dynamic Causal Modelling

Two-state DCM \<ref name=\"MarreirosKiebel2008\"\>\</ref\> is an
extension to the standard neuronal model in DCM for fMRI. Whereas the
standard neuronal model in DCM represents the activity in each region as
a single quantity, two-state DCM has an inhibitory and excitatory
population of neurons in each region. This gives an explicit model of
intrinsic connectivity within each region, and was adopted to be more
plausible and less constrained than the original model. Two-state DCM
imposes positivity constraints - all connections between regions are
excitatory, which conforms to the organisation of real cortical
hierarchies, where long-range connections are glutamatergic. With these
richer dynamics, two-state DCM may provide a better fit to fMRI data.
Furthermore, the inhibitory and excitatory populations add stability to
the model, allowing the priors on the connections to be relaxed, which
also may improve the model\'s ability to explain the data.

## The Two-State model

The model (see figure) involves recurrent connections between the
excitatory (E) and inhibitory (I) populations in each region. There is
an excitatory connection from E to I, denoted EI, and an inhibitory
connection from I to E, denoted IE. There are inhibitory
self-connections on E and I, which are referred to as SE and SI
respectively. The connections between regions (EE) link the excitatory
populations in each. In its current implementation in SPM12, the values
assigned to these connections are either estimated when fitting the
model to the data (EE and IE), or have fixed values (EI, SI, SE; see
table below). The EE extrinsic connections are based on the off-diagonal
of the A-matrix (plus modulatory input B if available), whereas the IE
self-inhibitory connections take their value from the diagonal of the
A-matrix (plus modulatory input B if available).

<figure>
<img src="Two_State_DCM_for_fMRI.png"
title="Illustration of the two-state neuronal model implemented in Dynamic Causal Model (DCM). Neuronal populations E and I are excitatory and inhibitory respectively. SE=self-excitation, SI=self-inhibition, EE=excitatory to excitatory, EI=excitatory to inhibitory, IE=inhibitory to excitatory." />
<figcaption>Illustration of the two-state neuronal model implemented in
Dynamic Causal Model (DCM). Neuronal populations E and I are excitatory
and inhibitory respectively. SE=self-excitation, SI=self-inhibition,
EE=excitatory to excitatory, EI=excitatory to inhibitory, IE=inhibitory
to excitatory.</figcaption>
</figure>

| Connection | Description                            | Value     |
|------------|----------------------------------------|-----------|
| IE         | intrinsic inhibitory to excitatory     | Estimated |
| EE         | extrinsic excitatory to excitatory     | Estimated |
| EI         | intrinsic excitatory to inhibitory     | 1         |
| SI         | intrinsic self-inhibition (inhibitory) | 1         |
| SE         | intrinsic self-inhibition (excitatory) | 0.5       |

## Interpreting the results

To enable certain connections to always have a positive effect and
others to always have a negative effect, the parameters of the
connections (A-matrix) and modulatory inputs (B) are log scaling
parameters that increase or decrease the prior values.

The between-regions EE (excitatory to excitatory) connection strength is
computed as follows:

\<math\> EE\_{ij} = 1/8 \* exp(J\_{ij}(t)) \</math\>

Where \<math\>J\_{ij}(t)\</math\> is the connection strength (summed
across A and B matrices) between a pair of regions i and j at time t:

\<math\> J\_{ij}(t) = A\_{ij} + B\_{ij}\*u(t) \</math\>

The inhibitory self connections IE are transformed in the same way, but
they are always negative, to ensure stability:

\<math\> IE\_{ij} = -1/8 \* exp(J\_{ij}(t)) \</math\>

The values in the A- and B-matrices therefore scale the prior connection
strength, 1/8Hz. A value of 0 in the A-matrix and 0 in the B-matrix for
a between-regions connection would equate to a connection strength of
1/8 \* exp(0 + 0) = 1/8Hz. Values of 0 for a self-connection would give
-1/8 \* exp(0+0) = -1/8Hz.

All this means that in order to inspect the results of model estimation,
one should first take the exponential of the A- and B-matrices, i.e.
exp(DCM.Ep.A) or exp(DCM.Ep.B). (This is done automatically if using the
Review tool in the GUI.) The number which results is a scaling factor,
which scales the prior. A value of 1 means no effect, values larger than
1 mean a larger amplitude effect than the prior, and values smaller than
1 mean a smaller amplitude effect than the prior. The values in the
C-matrix remain in units of Hz.

Here are some more examples of how to interpret the parameters.

### A-matrix between-region connections

A between-region connection exp(DCM.Ep.A(i,j)) larger than 1 would mean
the excitatory influence of region j on region i is larger than the
prior (1/8Hz). A value smaller than 1 would mean the excitatory
influence is smaller than the prior.

### A-matrix self connections

A self-connection exp(DCM.Ep.A(i,i)) larger than 1 would mean stronger
(more negative) self-inhibition in region i than the prior (-1/8Hz). A
self-connection smaller than 1 would mean weaker (less negative)
self-inhibition in region i than the prior.

### Modulatory (task) effects on the self-connections

An estimated parameter exp(DCM.Ep.B(i,i)) larger than 1 would mean an
increase in self-inhibition in region i caused by the task. Whereas, a
value smaller than 1 on this connection would mean a decrease in
self-inhibition caused by the task.

### Modulatory (task) effects on the between-region connections

An estimated parameter exp(DCM.Ep.B(i,j)) larger than 1 on a
between-regions connection would mean an increase in the connection
strength from region j to region i caused by the task. A value smaller
than 1 would mean a decrease in this connection due to the task.

## Difference between the paper and SPM12

The implementation of the model in SPM12, which is described here, has
certain differences from its description in the original scientific
paper by Marreiros and colleagues \<ref
name=\"MarreirosKiebel2008\"\>\</ref\>. In the paper, all possible
intrinsic connections between the excitatory and inhibitory states are
modulated and estimated explicitly. Therefore the matrices A and B have
size \[2xm,2xn\] as opposed to \[m,n\]. This implementation wasn't
adopted in SPM, in order to allow a more straightforward model
comparison (BMS) with the other single-state and non-linear DCM options.
As described above, the software uses a simplified scheme where an
estimated parameter larger than 1 on a self-connection on the B-matrix
contributes to the IE intrinsic inhibitory to excitatory connection, and
therefore an increase in self-inhibition in that region caused by the
task.

## References

\<references/\>
