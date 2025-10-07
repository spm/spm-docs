# More Linear Algebra

## Singular Value Decomposition

Singular value decomposition of a matrix $\mathbf{X}$ involves decomposing it into three other matrices,
which will be referred to as $\mathcal{U}$, $\mathcal{S}$ and $\mathcal{V}$ (you may recall an ``svd`` function from earlier).
The properties of these matrices are that the columns of both $\mathbf{U}$ and $\mathbf{V}$ are orthonormal (which will be described later),
and $\mathbf{S}$ is a diagonal matrix, with nonnegative values along the diagonal.
The original $\mathbf{X}$ can be reconstructed from $\mathbf{U} \mathbf{S} \mathbf{V}^T$.

```matlab

% Create a matrix of random numbers
X = randn(4,3)

% Do the SVD
[U,S,V] = svd(X,0)

% Orthonormal is when U'*U is an identity matrix
U'*U
V'*V

X_recon = U*S*V'
```

### Pseudoinverse
Singular value decomposition is often used for computing pseudoinverses because:
* The pseudoinverse of an orthonormal matrix is equal to its transpose.
* The inverse of a diagonal matrix is simply another diagonal matrix where the elements along the diagonal are the reciprocal of the values in the original.
From the previous example, we can compute the pseudoinverse by:
```
P = V*diag(1./diag(S))*U'
pinv(X)
```

However, things become trickier when matrices are no longer full rank.
If we do an SVD of the example from earlier,
then the resulting ``S`` matrix has zeros (or values very close to zero) on its diagonal, which would lead to infinities from ``diag(1./diag(S))``.
```
X = [1 2; 2 4];
[U,S,V] = svd(X);
disp(S)
```

To avoid this, computing a pseudoinverse is done by setting the result to zero for any values on the diagonal below some predefined tolerence:
```
tol = 1e-9;
s = diag(S);
sp = zeros(size(s));
sp(s>tol) = 1./s(s>tol);
Sp = diag(sp);
P = V*Sp*U'
```

The rank of a matrix is computed from how many of these diagonal elements of ``S`` are above the tolerence.

### Principal Component Analysis
Principal component analysis (PCA) is a linear dimensionality reduction technique that can be useful for summarising what is in fMRI data.

To illustrate the method, we will do an SVD of an image.
A quick way to get an image in MATLAB is to use one of the "[Easter eggs](https://blogs.mathworks.com/steve/2006/10/17/the-story-behind-the-matlab-default-image/)" in the code.
```matlab
X = pow2(get(0,'DefaultImageCData'),47);
imagesc(X);
colormap(gray)
axis image
figure(gcf)
```

We can do the SVD, and show the results:
```matlab
[U,S,V] = svd(X);

subplot(2,2,1);
imagesc(X);
axis image
title('Original')

subplot(2,2,2);
imagesc(U);
axis image
title('U')

subplot(2,2,3);
imagesc(V');
axis image
title('V^T')

subplot(2,2,4);
plot(diag(S))
title('S')
```

The diagonal of ``S`` shows how much variance is explained by each of the columns of ``U`` and ``V``.
The results of the SVD are organised such that the first columns explain the most variance, the second explains the second most, and so on.
The original image can be reconstructed with different numbers of components:
```matlab
for i=1:16
    subplot(4,4,i);
    n = i;
    R = U(:,1:n)*S(1:n,1:n)*V(:,1:n)';
    imagesc(R);
    title([num2str(n) ' components']);
    axis image off
end
figure(gcf)
```

We can do something similar with fMRI data.  First the 4D data would be reshaped into a 2D matrix,
where each row would be the time series from a voxel and each column would be a vectorised 3D scan.
This can then be decomposed into the components that best summarise the data.
Each component ``i`` would have a time series(``V(:,i)``), and a 3D image associated with that time series (from reshaping ``U(:,i)*S(i,i)``).

When you come to analyse your first fMRI dataset, you could try pasting the following code into MATLAB:
```matlab
P   = spm_select(Inf,'nifti'); % Select a 4D image
Nii = nifti(P);                % Read the header
d   = size(Nii.dat);           % Dimensions of the 4D image

% Read the data and reshape into a matrix
X   = reshape(Nii.dat(:,:,:,:),prod(d(1:3)),d(4));

[U,S,V] = svd(X,0); % Reduced size SVD

m = 35;              % Plane to show
to_view = [1 2 3 4]; % Components to show

for i=1:4 % Show the first four components
    j = to_view[i];

    % The jth component image (in 3D)
    uj = reshape(U(:,j),d(1:3))*S(j,j);

    % Display the image
    subplot(4,2,(i-1)*2+1);
    imagesc(uj(:,:,m)');
    axis image xy off

    % Show the corresponding time course
    subplot(4,2,(i-1)*2+2);
    plot(V(:,j));
end

figure(gcf)
```
In practice, much of the variance will arise from motion artifacts, although this is reduced
slightly in data that has been motion corrected.

## Weighted Least Squares
The least-squares fitting that we have seen previously assumes that the aim is to minimise the simple sum of squares difference between the data and the model fit.
Sometimes though, we may know that there is more noise on some data points than on others.
In this situation, we might want to do a weighted least squares fit so that noisy data points contribute less to the parameter estimation.
Rather than find the value of ${\beta}$ that minimises:

$$
(\mathbf{X} {\beta} - \mathbf{y})^T (\mathbf{X} {\beta} - \mathbf{y})
$$

A weighted least squares fit would minimise:

$$
(\mathbf{X} {\beta} - \mathbf{y})^T \mathbf{L} (\mathbf{X} {\beta} - \mathbf{y})
$$

Here, $\mathbf{L}$ would be some form of weighting matrix, which is symmetric and positive semidefinite.
A symmetric matrix means that ``L(i,j) == L(j,i)`` for all elements of the matrix.
Positive semidefinite means that for any column vector ``r`` that has the same length as the dimention of the matrix, the value of ``r'*L*r >= 0``.

For a simple weighted least squares fit, ``L`` would be a diagonal matrix, with positive values (or zero) along the diagonal.
As you will discover for fMRI, sometimes the weighting matrix needs to also include non-zero values on the off-diagonal elements.
The reason for this is that there are temporal correlations in the noise.
When it is known, or can be estimated, the weighting matrix is the inverse of the covariance matrix of the noise.

The least-squares estimate of ``beta`` can be computed from ``beta = (X'*L*X)\X'*L*y``.
One way to achieve the same result is to pre-whiten both the data ``y`` and the design matrix ``X``.
This is done by pre-multiplying both ``X`` and ``y`` with a matrix ``K``, such that ``K'*K == L``.
This means that we would be computing ``beta = (X'*K'*K*X)\X'*K'*K*y``, which is equivalent.

Because ``L`` is symmetric and positive definite, we can use SVD to derive a suitable ``K`` matrix.
This is because SVD of a symmetric positive definite matrix gives ``U`` and ``V`` matrices that are the same.
The following shows a simulation to illustrate the idea.
```matlab
% Simulate a symmetric positive definite matrix L.
L = randn(5);
L = L'*L;

% SVD
[U,S,V] = svd(L);

% Compute the matrix square root
K = U*diag(sqrt(diag(S)))*U';

% Test the result to see if the answer is almost zero
ss = sum(sum((K*K - L).^2))
```

The ``K`` matrix is the matrix square root of ``L``.
A similar approach can also be used for computing matrix logarithms, matrix exponentials, and various other matrix functions - providing the matrix is symmetric positive definite.
When this is not the case, a slightly different strategy is needed (which involves eigenvalues and eigenvectors - if you must know).
Alternatively, we could just use MATLAB's ``sqrtm`` function to compute matrix square roots.

```matlab
N = 100;

% Simulate a covariance matrix
Sig = randn(N);
Sig = Sig'*Sig/N;

% Simulate a design matrix
X = [ones(N,1) randn(N,2)];

% Simulate some data
beta_gt = rand(3,1);
y = X*beta_gt + sqrtm(Sig)*randn(N,1);

% L should be (proportional to) the inverse of the covariance
L = inv(Sig);

% Estimate beta one way
beta = (X'*L*X)\X'*L*y

% Estimate beta from re-whitened X and y
W = sqrtm(L);
Xw = W*X;
yw = W*y;
beta = Xw\yw

% Notice that the results differ from naive least squares estimation,
% which (on average) gives a solution that is further from the ground truth
beta_wrong = X\y
```


## Toeplitz Matrices
Correlated noise in fMRI data is normally modelled by toeplitz matrices, which describe the covariance of autocorrelated noise.
Rather than describe what a toeplitz matrix is, it might be easier to simply show you.
Try pasting the following examples of toeplitz matrices into MATLAB:
```matlab
T1 = toeplitz([1 0.75 0.25 0.125 0 0 0])
t2 = toeplitz([1 0.75 0.25 0.125 0 0 0],[1 -1 0 0 0 0 0])
```

Later, you will encounter convolution, which can be conceptualised as multiplication with a toeplitz matrix, as illustrated in 1D here:
```matlab
% Generate a Toeplitz matrix
f = gampdf(0:99,5,1);
T = toeplitz(f,[f(1) zeros(1,99)]);

% Simulate some delta functions
y = zeros(100,1);
y(20:30:80) = 1;

% Show the original and convolved
clf
plot([y T*y]);
legend('Original','Convolved')
figure(gcf)
```

In 2D, images are smoothed by convolving them with a Gaussian.  This is illustrated with this code snippet:
```matlab
G = toeplitz(exp(-(0:99).^2/(2*10))/sqrt(2*pi*10));
Y = zeros(100);
ind = ceil(rand(100,1)*100^2);
Y(ind) = 1;
subplot(2,2,1);
imagesc(Y)
axis image
title('random 1s');

subplot(2,2,2);
imagesc(G);
axis image
title('Convolution matrix')

subplot(2,2,3);
imagesc(G*Y*G')
axis image
title('Convolved');

colormap(gray)
figure(gcf)
```

