## What is a dynamic equation?

To understand DCM, you\'ll need to a know what a dynamic equation is.
It\'s extremely simple. A dynamic equation describes how a process (a
system) changes in time or space.

Here\'s a couple of examples - one from the real world, and one from the
world of maths. (The latter is considerably more exciting.)

Example 1: Let\'s say the bank gives you 3% interest on your savings.
We\'re now at the end of year zero, and your extremely successful
business has made you £50. How much will you have next year? We can work
out the answer with a dynamic equation:

\<center\> \<math\> x(1) = 1.03 \* x(0)\\ \</math\> \</center\>

Or more generally:

\<center\> \<math\> x(t) = 1.03 \* x(t-1)\\ \</math\> \</center\>

Where {{#tag:math\|t}} is time and {{#tag:math\|x}} is your bank
balance. You can apply this equation over and over again to see how your
bank balance will develop. In reality, you probably know that there\'s a
one-off equation to calculate compound interest for any number of years,
but the point of this example is that the *state equation* is a simple
rule describing how the system (your bank account) changes over time.

Example 2: A dynamic equation may represent how a system changes in
space, rather than time. Take these three equations, which describe the
rates of change of three numbers:

\<center\>

  
\<math\>

\begin{array}{lcl} \dfrac{dx}{dt} & = & \sigma (y - x) \\ \\
\dfrac{dy}{dt} & = & x (\rho - z) - y \\ \\ \dfrac{dz}{dt} & = & xy -
\beta z \end{array}\\ \</math\> \</center\>

These equations give you the rate of change of variables
{{#tag:math\|x}}, {{#tag:math\|y}} and {{#tag:math\|z}} over time,
whilst {{#tag:math\|\sigma}}, {{#tag:math\|\rho}} and
{{#tag:math\|\beta}} are numbers selected in advance - they are the
*parameters* of the system, which fine tune it. Don\'t worry about what
they mean.

Together these equations form the *Lorenz Attractor*, and if you plot
them on a graph, you get something which is not only crucial to chaos
theory, but something quite pretty.

So we\'ve seen that repeatedly applying a short dynamic equation to its
own output can describe the change of a system over time or space. As
we\'ll explore next, such an equation forms the basis of Dynamic Causal
Modelling.
