# fMRI Signal

## Haemodynamic Response Function
Increased brain activity does not immediately result in increased BOLD signal in the fMRI scans.
The increased activity follows the haemodynamic response function (HRF), which in SPM is modelled using
the ``spm_hrf()`` function.  The form of the HRF depends on the repeat time of the scans, which for simplicity, we assume is 1 second.
We can plot the default representation of the HRF by:
```
h = spm_hrf(1);
plot(h,'k.-')
xlabel('Time (seconds)')
ylabel('BOLD activity');
title('Haemodynamic Response Function')
```

The code allows the HRF to be varied according to seven parameters.
For information about what these parameters mean, you can type:
```
help spm_hrf
```

We can plot some examples of variability that the HRF can be modelled with by:
```
plot([spm_hrf(1,[6 16 1 1 6 0]) spm_hrf(1,[7 16 1 1 6 0]) ...
      spm_hrf(1,[6 18 1 1 6 0]) spm_hrf(1,[6 16 2 1 6 0]) ...
      spm_hrf(1,[6 16 1 2 6 0]) spm_hrf(1,[6 16 1 1 7 0])])
xlabel('Time (seconds)')
ylabel('BOLD activity');
title('Haemodynamic Response Functions')
```

Now we can consider how the HRF is used in practice.
Consider the activity in a single voxel over 200 seconds, from a subject stimulated briefly at 50, 100, 150 seconds.
```
N  = 200;
y0 = zeros(N,1);    % 200 seconds of scanning
y0([50 100 150]) = 1; % Stimuli at 50, 100 and 150 seconds

y  = convn(y0,h);     % Convolve stimuli with the HRF
y  = y(1:N); % Remove the trailing time points

% Plot the data
plot([y0 y])
legend('Stimuli','Expected BOLD response')
xlabel('Time (seconds)')
ylabel('Signal')
title('Simulated BOLD Responses');
```

The duration of stimuli can be variable, and the HRFs they elicit often overlap.
Copy/paste the following to 
```
y0 = zeros(N,1);    % 200 seconds of scanning
y0(round(rand(30,1)*(N-1)+1)) = 1; % Stimuli at 50, 100 and 150 seconds

y  = convn(y0,h);     % Convolve stimuli with the HRF
y  = y(1:length(y0)); % Remove the trailing time points

% Plot the data
plot([y0 y])
legend('Stimuli','Expected BOLD response')
xlabel('Time (seconds)')
ylabel('Signal')
title('Simulated BOLD Responses');
```

## Simulating a Design Matrix
An fMRI experiment may consist of several different tasks, and interactions among the tasks are often modelled.
We can simulate a design matrix for such an experiment by:
```
X = zeros(200,4);
X(round(rand(10,1)*(N-1)+1),1) = 1;
X(round(rand(10,1)*(N-1)+1),2) = 1;
X(round(rand(10,1)*(N-1)+1),3) = 1;
X(round(rand(10,1)*(N-1)+1),4) = 1;
X = convn(X,h);
X = [X(1:N,:) spm_dctmtx(N,5)];
imagesc(X)
colormap(gray)
```

In the above matrix, the first four columns model different types of task, white the final 10 columns would model smooth drifts in the overall BOLD signal.


## Simulating an fMRI Time Series
The following could be used to simulate a BOLD response.
```
beta_gt = [10 5 5 10  100 randn(1,4)*13]';
y0 = X*beta_gt;
```

We also need to consider noise, which will be correlated.
If the noise has a covariance of ``V``, then we can simulate this correlated noise with:
```
% Simulate some form of covariance matrix
D = diag([0; -ones(N-1,1)])+diag([0; ones(N-2,1)],1);
L = D'*D + eye(N)*0.1;
V = inv(L);

% Add random noise drawn from the distribution modelled by the covariance matrix.
e0 = randn(200,1);
e = sqrtm(V)*e0;
y = y0 + e;

% Plot the data
plot([y0 y]);
legend('Noise-free','With noise')
title('Simulated BOLD');
xlabel('Time (seconds)');
ylabel('Signal')
```

Because of the temporal correlations in the data,
both the data and design matrix are "pre-whitened" before doing the statistics by dividing by the matrix square root of the covariance matrix.
In SPM, the form of the covariance matrix is estimated from the data itself using restricted maximum likelihood (REML).
You don't need to know the details of how this works, but it's useful to be aware of it.

There are four different tasks/stimuli modelled by the design matrix.
When computing t statistics, we define how we would like to interpret the data in terms of a contrast vector.
The design matrix has nine columns, so the contrast vector should have nine elements.
In the SPM software, you could enter a smaller number of elements and it would pad the contrast vector with zeros to make it the right length.
For this example, we'll use a contrast vector of:
```
c = [-1 -1 1 1  0 0 0 0 0];
```
This would identify for statistically significant differences where tasks 3 and 4 cause higher BOLD signal than tasks 1 and 2.

```
% Compute ``S``, which is the inverse of the matrix square root of the covariance matrix ``V``.
S    = inv(sqrtm(V));

% Use this to "pre-whiten" the design matrix and data.
ya   = S*y;
Xa   = S*X;

% Compute the t statistic using the pre-whitened ``Xa`` and ``ya``.
beta = Xa\ya;
nu   = size(Xa,1) - rank(Xa);
r    = ya - Xa*beta;
v    = sum(r.^2)/nu;
t    = (c * beta) / sqrt(v * c * inv(Xa'*Xa) * c')
p    = 1-spm_Tcdf(t,nu)
```

In the above example, p should typically not indicate statistical significance.
However, specifying the following contrast vector, and then copy/pasting the above code into MATLAB, should give small p values:
```
c = [1 -1 -1 1  0 0 0 0 0];
```



