
## Temporal correlations
Suppose we were to double the lengths of ``y`` and ``X`` by replicating their values.
One way to do this is
```
T  = kron(eye(size(y,1)),[1 1]');
y2 = T*y;
X2 = T*X;
```
We can compute a t statistic from these by copy/pasting the following into MATLAB.
```
beta2 = X2\y2
nu2 = size(X2,1) - rank(X2);
r2  = y2 - X2*beta2;          % Residuals
v2  = sum(r2.^2)/nu2
t2 = (c * beta2) / sqrt(v2 * c * inv(X2'*X2) * c')
p2 = 1-spm_Tcdf(t2,nu)
```

You should notice that the new p value ``p2`` is much smaller than the one from the original t test.
We should not be able to get extra statistical significance like this for free, so what is wrong?

The answer is that the statistical analysis does not account for the noise no longer being i.i.d.
There is now covariance in the data.
If we know this covariance, then it can be included within the various matrix operations of the statistical analysis
to correct it.
In fMRI, there are temporal correlations in the time series noise, which needs to be accounted for when doing statistical analyses within subject.




You don't need to know or understand the details, but this could be done as follows:
```
V     = T*T';                                               % Covariance matrix
L     = pinv(V);                                            % Precision matrix
beta2 = (X2' * L * X2) \ X2'* L *y2                         % Parameter estimates
R     = L*(eye(size(X2,1)) - X2*inv(X2'*L*X2)*X2'*L);       % Residual forming matrix
nu2   = trace(R*V);                                         % Degrees of freedom
v2    = y2'*R*y2/nu2                                        % Variance
t2    = (c * beta2) / sqrt(v2 * c * inv(X2' * L * X2) * c') % t statistic
p2    = 1-spm_Tcdf(t2, nu2)                                 % p value
```
An easier way would be to "pre-whiten" the data and design matrix.
```
y3 = pinv(T)*y2;
X3 = pinv(T)*X2;
```
