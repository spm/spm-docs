# Dynamic Causal Modelling - Units of Parameters

In general, the coupling parameters in DCM (matrices A,B,C) are in units
of hertz. They are rate constants, defining the rates of change within
or between regions. Certain parameters, however, are log scaling
parameters - this refers to a transformation used to force these
parameters to take positive or negative values.

The notion of rate constants is most easily illustrated for the
self-connections - the diagonal entries of the A matrix.

## Self-connections

Imagine a one region DCM with a single inhibitory self-connection (i.e.
a single value in matrix A). The level of activity in this region (its
state) is denoted \<math\>z\</math\>. This system can be modelled as a
first-order linear differential equation:

\<math\> \begin{align} \dfrac{dz}{dt} & = az \\ z(t) & = z_0 \cdot
e^{-at} \\ \end{align} \</math\>

The parameter \<math\>a\</math\> controls the rate of decay in the
region. To see why, we can consider the half-life of the system.

<figure markdown>
  <div class="center">
  <img src="../../../assets/figures/wikibooks/Exponential_decay_mechanism.svg" style="width:100mm" />
  </div>
  <figcaption>Exponential decay. A large 'a' parameter (red) will cause faster decay than a small 'a' parameter (green)</figcaption>
</figure>

### Relationship with half-life

The half-life \<math\>\tau\</math\> is the time at which activity in our
example region has fallen to half its starting level. From the
definition above, we know that we can calculate the activity at time
\<math\>\tau\</math\>:

\<math\> \begin{align} z(\tau) & = 0.5z_0 \\ & = z_0 \cdot e^{-a\tau}
\end{align} \</math\>

We can re-arrange this expression to find the value of
\<math\>\tau\</math\> and \<math\>a\</math\>:

\<math\> \begin{align} e^{-a\tau} & = \dfrac{0.5z_0}{z_0} \\ & = 0.5 \\
\\ -a\tau & = ln(0.5) \\ & = ln(1) - ln(2) \\ & = 0 - ln(2) \\ & =
-ln(2)\\ \\ \tau & = \dfrac{-ln(2)}{-a} \\ & = \dfrac{ln(2)}{a} \\ \\ a
& = \dfrac{ln(2)}{\tau} \end{align} \</math\>

Thus, \<math\>a\</math\> is inversely proportional to the half-life
\<math\>\tau\</math\>. The more negative the value of
\<math\>-a\</math\>, the quicker activity decays in this region.

## Constraining the sign

Sometimes we need to force the sign of a coupling parameter to be
positive or negative. For instance, we constrain the self-connection on
each region, \<math\>-A\</math\>, to be negative - ensuring that it
decays and the system reaches stability. Another example is two-state
DCM, which explicitly represents particular connections as being
excitatory or inhibitory. Typically, the approach taken is to treat the
parameters as being the logarithm of the coupling strength, rather than
the coupling strength itself. Then within the code of the model, its
value is exponentiated prior to use, and can be made positive:

```matlab
a = exp(A);
```

or negative:

```matlab
a = -exp(A);
```

It is important when interpreting the results, therefore, to know which
parameters are simple coupling strengths and which are log parameters.
This depends on which DCM model you use, and these are laid out below.

## Specific Models

### One-state DCM for fMRI

This is the standard or \'vanilla\' DCM for fMRI. All connections in the
A,B,C and D matrices are coupling parameters in units of Hz, with the
exception of the self-connections on the A-matrix, which are log scaling
parameters. The intention here is two-fold - to force these to always be
negative, and to set a default strength of -0.5Hz when the value in the
A-matrix is zero.

To achieve this, the values for the self-connections in the A-matrix go
through the following transformation within the model:

\<center\> \<math\> a = -\frac{e^A}{2} \</math\> \</center\>

Where \<math\>A\</math\> is the value that appears in the A-matrix and
\<math\>a\</math\> is the value used in the model.

### Two-state DCM for fMRI

In this model, all values in the A and B (but not C) matrix are log
scaling parameters.
