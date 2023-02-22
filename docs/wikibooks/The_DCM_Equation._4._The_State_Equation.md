We\'ve seen that the following linear dynamic equation can represent the
flow of information through a network over time:

\<center\> \<math\> z(t + 1) = Az(t)\\ \</math\> \</center\>

If we add a few more terms in to this equation, we can model some
additional details of a brain network. Here is the DCM state equation:

\<center\> \<math\> \dot{z} = (A + \sum{u_jB^j})z + Cu \</math\>
\</center\>

To explain each term in the equation, we\'ll consider the following
example model:

<figure>
<img src="ExampleDCMGraph.png" title=" center" />
<figcaption> center</figcaption>
</figure>

We\'ve got three brain regions represented by grey circles. Each region
has a certain level of output, denoted \<math\> z_1\\ \</math\>,
\<math\> z_2\\ \</math\> and \<math\> z_3\\ \</math\>. Between the three
regions are fixed connections shown by black arrows, in this example
going in just one direction. Coming into our system, via Region 1, we
have some kind of visual information as an external input. Additionally,
the connection between Region 2 and Region 3 is *modulated* by
attention. This means that attention only has an effect if this
connection is in use.

This is how the equation above can be used to describe this brain model.

## A- and B- Matrices

As before, \<math\> z\\ \</math\> is a column vector representing the
output of each brain region. Let\'s start off by saying that only the
first region is currently producing some output:

\<center\> \<math\> z = \left\[ \begin{array}{c}

`1 \\`  
`0 \\`  
`0  \\`  
`\end{array}`

\right\] \</math\> \</center\>

The equation gives us \<math\> \dot{z} \</math\>, which is a column
vector of the rate of change in \<math\> z\\ \</math\> with respect to
time - i.e. the rate of change of each brain region\'s output.

As before we also have the binary connectivity matrix \<math\> A\\
\</math\>, which records which regions connect to which other(s). For
this example, the A-matrix would look like this:

\<center\> \<math\> A = \left\[ {\begin{array}{ccc}

`0 & 0 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 1 & 0  \\`  
`\end{array} } \right]`

\</math\> \</center\>

Rather than just multiplying \<math\> A\\ \</math\> by the vector
\<math\> z\\ \</math\> directly, as we did before, \<math\> A\\
\</math\> is now firstly summed with an additional term:

\<center\> \<math\> A + \sum{u_jB^j} \</math\> \</center\>

Coming into our network we have \<math\> j\\ \</math\> *modulatory*
inputs. These are inputs which modify the connections between regions;
in our example, j = 1, attention. For each modulatory input we have a
\<math\> B\\ \</math\> matrix, which is a binary connectivity matrix
similar to A, but the 1\'s and 0\'s denote where the modulatory input
connects to the network. Each of these B-matrices is multiplied by a
vector of the modulatory inputs\' strengths, \<math\> u\\ \</math\>.

For this example, the B-matrix will look like this:

\<center\> \<math\> B_1 = \left\[ {\begin{array}{ccc}

`0 & 0 & 0 \\`  
`0 & 0 & 0 \\`  
`0 & 1 & 0  \\`  
`\end{array} } \right]`

\</math\> \</center\>

And the vector of input strengths:

\<center\> \<math\> u = \left\[ \begin{array}{c}

`1 \\`  
`1`  
`\end{array}`

\right\] \</math\> \</center\>

Let\'s decide that the first input, \<math\> u_1\\ \</math\>, is the
strength of our modulatory input (attention) and the second input,
\<math\> u_2\\ \</math\>, is the strength of our external input
(vision). For this part of the equation, which deals with modulatory
input, we\'ll only use the first value of \<math\> u\\ \</math\>:

\<center\> \<math\> u_1 = \left\[ \begin{array}{c}

`1`  
`\end{array}`

\right\] \</math\> \</center\>

Let\'s plug each of the terms we\'ve discussed so far into the DCM
equation:

\<center\> \<math type=\"eqnarray\"\> \begin{array}{lll} \dot{z} & = &
(A + \sum{u_jB^j})z + Cu \\\\ & = & \left( \left\[ {\begin{array}{ccc}

`0 & 0 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 1 & 0  \\`  
`\end{array} } \right] + \left[`

\begin{array}{c}

`1`  
`\end{array}`

\right\] \* \left\[ {\begin{array}{ccc}

`0 & 0 & 0 \\`  
`0 & 0 & 0 \\`  
`0 & 1 & 0  \\`  
`\end{array} } `

\right\]\right)\left\[ \begin{array}{c}

`1 \\`  
`0 \\`  
`0  \\`  
`\end{array}`

\right\] + Cu \\\\ & = & \left( \left\[ {\begin{array}{ccc}

`0 & 0 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 2 & 0  \\`  
`\end{array} } \right] \right) \left[`

\begin{array}{c}

`1 \\`  
`0 \\`  
`0  \\`  
`\end{array}`

\right\] + Cu \\\\ & = & \left\[ \begin{array}{c}

`0 \\`  
`1 \\`  
`0  \\`  
`\end{array}`

\right\] + Cu \end{array} \</math\> \</center\>

We can already see that the combination of fixed connectivity (A) and
modulatory connectivity (B) has given us a vector, \[0 1 0\], which
means that the second region\'s activity is going to increase in this
step.

Let\'s now finish our overview of the equation by adding in the last
term, involving the C matrix.

## The C- Matrix

The C-Matrix determines which external input connects to which region.
It is a matrix with one row per region, and one column per input
contained in the vector \<math\> u \</math\>:

\<center\> \<math\> C = \left\[ {\begin{array}{ccc}

`0 & 1 \\`  
`0 & 0 \\`  
`0 & 0  \\`  
`\end{array} } \right]`

\</math\> \</center\>

So this says that only the second input is an external input, and it is
connected to the first region.

Let\'s now plug this into the DCM equation we\'ve been working on:

\<center\> \<math\> \begin{array}{lll} \dot{z} & = & \left\[
\begin{array}{c}

`0 \\`  
`1 \\`  
`0  \\`  
`\end{array}`

\right\] + Cu \\\\ & = & \left\[ \begin{array}{c}

`0 \\`  
`1 \\`  
`0  \\`  
`\end{array}`

\right\] + \left\[ {\begin{array}{ccc}

`0 & 1 \\`  
`0 & 0 \\`  
`0 & 0  \\`  
`\end{array} } \right] * \left[`

\begin{array}{c}

`1 \\`  
`1`  
`\end{array}`

\right\] \\\\ & = & \left\[ \begin{array}{c}

`1 \\`  
`1 \\ `  
`0`  
`\end{array}`

\right\] \end{array} \</math\> \</center\>

### Summary

So after one iteration of the DCM state equation, Region 2 is active
because it receives activation from Region 1 (thanks to the first half
of the DCM equation) and Region 1 is active because of its external
visual input (thanks to the second half of the DCM equation).

Phew!

## What\'s the Point of all this?

The Dynamic Causal Model tells us how information entering a brain
network will behave over time. But, we need to know whether our choice
of the A, B and C matrices provide us with a good model of brain
activity. To do this, we need to:

- Combine our neural model with a haemodynamic model, so we know what
  we\'d expect to see in the fMRI scanner if the neural model were a
  good representation of real life.
- Test how well our neural model fits the measured fMRI signal, compared
  to other models, so we can choose the best model out of a choice of
  several.

The next pages explain how we do this.
