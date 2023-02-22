Using dynamic equations to study networks is common practice in a huge
range of fields - from engineering, to internet search engines, to
neuroscience. For DCM, we need to model what happens when brain regions
communicate with one another. We\'ll start by seeing how we represent
networks numerically, and then step through an example showing how a
linear dynamic equation can describe the activity of a network over
time.

## Networks as Matrices

### Representing a network as a matrix

A network, or graph, is a set of nodes connected by lines with arrows.
In DCM, these nodes represent brain regions, and an arrow between two
nodes denotes that one region causes a change in the activity of
another.

Here\'s an example network, and below it, the same network represented
by a *connectivity matrix*.

<figure markdown>
  <div class="center">
  <img src="../../../assets/figures/wikibooks/ThreeVertexGraph.png" style="width:100mm" />
  </div>
  <figcaption>SPM Display</figcaption>
</figure>

\<math\> A =
\left\[ {\begin{array}{ccc}
`0 & 1 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 1 & 0  \\`  
`\end{array} } \right]`

\</math\> \</center\> In the matrix representation of the network, each
column represents connections coming **from** a brain region, and each
row represents connections coming **into** a brain region. A one (1)
denotes a connection is present, a zero (0) denotes that there is no
connection.

For example, to find out whether region 2 connects to region 3, we would
look to see what number is in the second column and the third row of the
matrix. Here we find a number one, telling us that there is a connection
from region 2 to 3, and we can confirm this by looking at the diagram
above.

Representing networks as matrices makes it very simple to ask questions
about the network.

### Networks and Dynamic Equations

Let\'s say the network above represents three brain regions, and we
apply some external input to Region 1. What will happen? How will the
activity move through the network?

We need a model, in the form of a dynamic equation, which will tell us
how the network behaves. Our simple model will include just two things:

1.  A record of which brain region connects to which other region.
    We\'ll represent this in numbers, using the A-matrix shown above.
2.  The output of each region. We will store this in a column vector
    called z.

The model describes how z, the output of each region, changes from time
t to time t+1:

\<center\> \<math\> z(t + 1) = Az(t) \\ \</math\> \</center\>

Simple! So in this model, all we have to do is multiply the A-matrix by
the output of each region, and we get the new output of each region.
Let\'s try it.

We start off with only Region 1 producing activity, due to some kind of
stimulation that we\'re giving it. The other two regions are silent. We
represent this in the column vector, z:

\<center\> \<math\> z(t) = \left\[ \begin{array}{c}

`1 \\`  
`0 \\`  
`0  \\`  
`\end{array}`

\right\] \</math\> \</center\>

Now let\'s apply our dynamic equation to z, and see what happens.

**Iteration 1:**

\<center\> \<math\> z(t+1) = Az(t) = \left\[ \begin{array}{ccc}

`0 & 1 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 1 & 0`  
`\end{array}`

\right\] \left\[ \begin{array}{c}

`1 \\`  
`0 \\`  
`0  \\`  
`\end{array}`

\right\] = \left\[ \begin{array}{c}

`0 \\`  
`1 \\`  
`0  \\`  
`\end{array}`

\right\] \</math\> \</center\>

So we began with only the first region, Region 1, having activity. Now,
our new z-vector says that only Region 2 is active. Looking back at the
picture of the graph, this makes sense - the external input activated
Region 1, which in turn activated Region 2. We take our new z-vector and
repeat the process.

**Iteration 2**

\<center\> \<math\> z(t+1) = Az(t) = \left\[ \begin{array}{ccc}

`0 & 1 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 1 & 0`  
`\end{array}`

\right\] \left\[ \begin{array}{c}

`0 \\`  
`1 \\`  
`0  \\`  
`\end{array}`

\right\] = \left\[ \begin{array}{c}

`1 \\`  
`0 \\`  
`1  \\`  
`\end{array}`

\right\] \</math\> \</center\> Region 2 has activated regions 1 and 3.
We now re-apply the equation to the updated z-vector.

**Iteration 3**

\<center\> \<math\> z(t+1) = Az(t) = \left\[ \begin{array}{ccc}

`0 & 1 & 0 \\`  
`1 & 0 & 0 \\`  
`0 & 1 & 0`  
`\end{array}`

\right\] \left\[ \begin{array}{c}

`1 \\`  
`0 \\`  
`1  \\`  
`\end{array}`

\right\] = \left\[ \begin{array}{c}

`0 \\`  
`1 \\`  
`0  \\`  
`\end{array}`

\right\] \</math\> \</center\>

Only Region 2 is now active. The state of the system is now the same as
we used in Iteration 2, so we know that continuing further is
pointless - the system will loop forever between \[1 0 1\] and \[0 1
0\].

So the model, given above as a dynamic equation, tells us how the
activity of the network will change over time given certain input.

## Why This Works

To take a specific example of why \<math\> Az(t) \</math\> gives us the
change in network activity over time, consider the activity of our
example network at the end of Iteration 3. Why is only Region 2 active?

Region 2, which is the second element of the state vector, z, is
calculated by multiplying the second row of the A-matrix (which
represents all connections coming into Region 2) by the current output
of each region stored in the vector z. We are therefore adding up all
the activation entering Region 2, which written in full looks like:

\<center\> \<math\> z_2(t) = A(2,:)z(t) = \left\[A(2,1) \* z(t)\_1
\right\] + \left\[A(2,2) \* z(t)\_2 \right\] + \left\[A(2,3) \* z(t)\_3
\right\] = 1 + 0 + 0 = 1 \</math\> \</center\>

(Where the colon, :, means \"all columns\")

So, in conclusion, the new output for Region 2 is a weighted sum of its
inputs.

## Summary So Far

We\'ve seen how a simple dynamic equation can model communication across
a network of brain regions. We only have to add in a few extra details,
and we\'ve got the Dynamic Causal Model.
