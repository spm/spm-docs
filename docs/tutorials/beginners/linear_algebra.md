# Introduction to Linear Algebra in MATLAB

## Matrix Multiplication
Matrices can be added and subtracted in a straightforward way.  Providing their dimensions are the same, then the corresponding elements are simply added or subtracted. Multiplication is a bit more complicated. Given two matrices $\mathbf{A}$ and $\mathbf{B}$ of sizes $M \times K$ and $K \times N$, then the product of the matrices has size $M \times N$. If $\mathbf{C} = \mathbf{A} \mathbf{B}$, then $c_{mn} = \sum_{k=1}^K a_{mk} b_{kn}$. Here is a quick MATLAB example, where $M=3$, $K=2$ and $N=3$.
```matlab
A = [1 2
     3 4
     5 6]

B = [11 12 13
     14 15 16]

C = A*B
```
The computations here are:

$$
\mathbf{C} = \left[\begin{array}{ccc}
a_{11} b_{11} + a_{12} b_{21}, & a_{11} b_{12} + a_{12} b_{22}, & a_{11} b_{13} + a_{12} b_{23}\\
a_{21} b_{11} + a_{22} b_{21}, & a_{21} b_{12} + a_{22} b_{22}, & a_{21} b_{13} + a_{22} b_{23}\\
a_{31} b_{11} + a_{32} b_{21}, & a_{31} b_{12} + a_{32} b_{22}, & a_{31} b_{13} + a_{32} b_{23}
\end{array}\right]
$$

Recall that matrix multiplication was briefly encountered in the tutorial about ``for`` loops.
The following extends the code from that section into a function, which could be saved as ``matmul.m``:
```matlab
function C = matmul(A,B)
% Matrix multiplication
% FORMAT C = matmul(A,B)

% Error checking
if ndims(A)>2 || ndims(B)>2, error('Too many dimensions.'); end
if size(A,2) ~= size(B,1),   error('Incompatible dimensions.'); end

M = size(A,1);
K = size(A,2);
N = size(B,2);
C = zeros(M,N);
for m=1:M
    for n=1:N
        for k=1:K
            C(m,n) = C(m,n) + A(m,k)*B(k,n);
        end
    end
end
```
You can test the code with the following, and compare it against MATLAB's much more efficient implementation:
```matlab
A = randn(2,3);
B = randn(3,4);
C_matmul = matmul(A,B)
C_mtimes = A*B
disp(sum(sum((C_matmul - C_mtimes).^2)))
```
In MATLAB, a vector is simply a matrix where one of the dimensions is 1.

Computing the sums (or means) over the columns of a matrix can be treated as a matrix multiplication:
```matlab
X = rand(4,3)
w = ones(4,1);
s = w'*X
sum(X)
```

Similarly, computing the sum of squares of a vector can also be conceptualised this way:
```
x = randn(4,1)
ss = x'*x
sum(x.^2)
```

## Transpose
Sometimes, we swap around the rows and columns of a matrix. This is called transposing. In maths, we would denote transposing matrix $\mathbf{A}$ by $\mathbf{A}^T$. In MATLAB, we would write
```matlab
A = [1 2
     3 4
     5 6];
A'
```
which gives the answer
```
     1     3     5
     2     4     6
```

## Matrix Inverse
Before explaining what a matrix inverse is, we first look at some simultaneous equations.

### Simultaneous equations
Consider these simultaneous equations in $a$ and $b$:

$$
\begin{matrix}
2a + 3b + 4 & = 26 \\
 a +  b - 2 & = 7
 \end{matrix}
$$

This can easily be simplified to:

$$
\begin{matrix}
2a + 3b & = 22 \\
 a +  b & = 9
  \end{matrix}
$$

We can tackle this by rearranging $a + b = 9$ into:

$$
a = 9 - b
$$

and substituting this into $2a + 3b = 22$ to give:

$$
\begin{matrix}
2(9 - b) + 3b & = 22 \\
\end{matrix}
$$

Solving this gives $b = 4$, which can be substituted into $a = 9 - b$ to give:

$$
a = 9 - 4 = 5
$$

### Matrix Solutions
Solving equations like this with two parameters ($a$ and $b$ in the above example) is pretty straightforward.
Finding their solution becomes harder with more equations and unknowns.
Although it might seem abstract, this type of problem is relevant to a wide variety of tasks in imaging neuroscience, from image reconstruction (where there are tens of thousands of unknowns) to fitting fMRI time series. 

Now try solving the following set of equations:

$$
\begin{matrix}
 3a - 2b +  c & =  20 \\
-5a +  b - 4c & = -45 \\
  a -  b +  c & =   8
\end{matrix}
$$

To do this in MATLAB, we can arrange 20, -45 and 8 into a column vector that we call $\mathbf{y}$:
```matlab
y = [20; -45; 8]
```

The values that $a$, $b$ and $c$ are multiplied by can be arranged into a square matrix $\mathbf{X}$:
```matlab
X = [ 3 -2  1
     -5  1 -4
      1 -1  1]
```

If we consider the unknowns as a vector ${\beta}$, we essentially want to find the values in ${\beta}$ where $\mathbf{X} {\beta} = \mathbf{y}$.

In MATLAB, the above simultaneous equations can be solved to give a solution $\mathbf{\beta}$ using:
```matlab
beta = X\y
```

Alternatively, we could compute the matrix inverse ($\mathbf{P}$) and do a matrix-vector multiplication of this by $\mathbf{y}$:
```matlab
P    = inv(X)
beta = P*y
```

We can check the answers with:
```matlab
X*beta
```

This simply means:
```matlab
[X(1,1)*beta(1) + X(1,2)*beta(2) + X(1,3)*beta(3)
 X(2,1)*beta(1) + X(2,2)*beta(2) + X(2,3)*beta(3)
 X(3,1)*beta(1) + X(3,2)*beta(2) + X(3,3)*beta(3)]
```

## Matrix Pseudoinverse
Matrices only have inverses if they are square.
Sometimes, the number of unknowns may not match the number of equations, in which case the problem is under- or overdetermined.
Obtaining solutions to problems like this can be done using a pseudoinverse.

This simple illustation is a case where there is not enough information to obtain a unique solution for both variables:

$$
a + 2b = 5
$$

Extending this illustration slightly, we could also have these equations to solve:

$$
\begin{matrix}
a + 2b & = 5 \\
2a + 4b & = 10
\end{matrix}
$$

Again, there is no unique solution.
The problem is rank deficient, because the rows/columns of the matrix would be linearly dependont on each other.
In MATLAB, we can obtain the rank (i.e. the number of linearly independent rows or columns) of a matrix by:
```matlab
X = [1 2; 2 4];
rank(X)
```

Another type of example, which is similar to what you are likely to encounter later is:
```matlab
X = [[ones(3,1); zeros(3,1)] [zeros(3,1); ones(3,1)] ones(6,1)]
r = rank(X)
```
In this example, it should be clear that any column of the matrix can be constructed from a linear combination of the other columns.

An inverse $\mathbf{P}$ of a matrix $\mathbf{X}$ is defined when $\mathbf{P} \mathbf{X} = \mathbf{X} \mathbf{P} = \mathbf{I}$, where $\mathbf{I}$ is an identity matrix (i.e., all zeros, but with 1 along its diagonal).
The "pseudoinverse" (``pinv``) of a matrix has a more general definition, whereby $\mathbf{X} \mathbf{P} \mathbf{X} = \mathbf{X}$.
We can try these two definitions in MATLAB by:
```matlab
pinv(X)*X - eye(size(X,2))
X*pinv(X)*X - X
```

### Least Squares Fitting
In SPM, we work with over-determined problems, where there are more knowns than unknowns, and where there may be inconsistencies among them.
For example, consider the following set of equations:

$$
\begin{matrix}
a +  b & =  5 \\
a + 2b & =  7.5 \\
a + 3b & =  9 \\
a + 4b & = 11.5 \\
a + 5b & = 13
\end{matrix}
$$

There is no actual solution to the above set of equations because they are not consistent with each other.
Instead, the aim is usually to find the solution that minimises the sum of squared errors (which can be considered as a squared distance according to Pythagoras' theorem).
In this example, the equations are set up for fitting a straight line through some data, where the line is modelled by an intercept ($a$) and slope ($b$).

We have a design matrix ``X``:
```matlab
X = [1 1
     1 2
     1 3
     1 4
     1 5]
```
and a data vector ``y``:
```matlab
y = [5 7.5 9 11.5 13]'
```

The least squares solution can be found in MATLAB by:
```matlab
beta = X\y
```

However, we can not do the following because $\mathbf{X}$ is not square, so does not have an inverse:
```matlab
P    = inv(X)
beta = P*y
```

Instead, we could compute a pseudo-inverse $\mathbf{P} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T$ by
```matlab
P    = inv(X'*X)*X'
beta = P*y
```

In the above, `` X'*X `` means do a matrix multiplication of the transposed version of $\mathbf{X}$ with itself not transposed.

The residuals ($\mathbf{r}$) can be computed from
```matlab
r = X*beta - y
```

In MATLAB, ``size(X,1)`` and ``size(X,2)`` give the number of rows and columns of ``X``, respectively.
The mean squared residuals (correcting for loss of degrees of freedom because we estimate two parameters $b_1$ and $b_2$) is then by:
```matlab
v = sum(r.^r)/(size(X,1)-rank(X))
```
In the above, the `` .* `` denotes element-by element multiplication. Another way of achieving the same result ($(r_1^2 + r_2^2 + r_3^2 + r_4^2 + r_5^2)/3$) is by:
```matlab
v = r'*r/(size(X,1)-size(X,2))
```
The `` v `` above is an estimate of the variance on the data.

## Basic Plotting
Plotting is easy in MATLAB. We can create a figure window by:
```matlab
figure
```

We can then plot the data and model fit. In the following, `` X(:,2) '' means the second column of $\mathbf{X}$. The `` 'r.' `` means use red dots and the `` 'b-' `` means use a blue line:
```matlab
plot(X(:,2), y, 'r.', X(:,2), X*b, 'b-')
```
We can also annotate the plot with axis labels, a title and a legend.
```matlab
xlabel('Time (seconds)')
ylabel('Value')
title('Simulated data and linear fit')
legend('Data','Model fit')
figure(gcf)
```

In the above, the ``figure(gcf)`` command causes MATAB to display the current figure window. If you want to create a new figure window, you can simply type ``figure``.
For more information about commands/functions in MATLAB, you can type e.g. for the `` plot `` command:
```matlab
help plot
```

## Showing images
In SPM, matrices are often visualised as images.
This can be done by:
```matlab
imagesc(X)
axis image
colormap(gray)
figure(gcf)
```
Notice that a grey-scale image is just a 2D array of numbers.
