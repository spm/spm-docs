**Spatial smoothing** is usually performed as a part of the
preprocessing of individual brain scans.

In SPM the spatial smoothing is performed with a spatially stationary
Gaussian filter where the user must specify the kernel width in mm
\"full width half max\". Kernel widths of up to 16mm are being used in
the literature.

The purpose of spatial smoothing is to cope with functional anatomical
variability that is not compensated by spatial normalization
(\"warping\") and to improve the signal to noise ratio. In other words:
smoothing increases statistical power. The less one smoothes, the less
likely it is to obtain significant results. Understanding the exact
impact of spatial smoothing on statistical power is not trivial:
smoothing not only increases the signal to noise ratio of each voxel, it
also reduces the number of resolution elements (\"resels\") that are
assumed to be independent and used for correction of multiple testing.
With respect to the signal to noise ratio, it is being argued that an
ideal filter kernel matches the size of the signal to detect \[1\].
After all, the level of smoothing that is optimal for real, measured
data can only be determined empirically \[2\] and it may make sense to
reanalyze the same data with several levels of smoothing \[3\].

The drawback from smoothing is a loss of spatial precision. Especially
when using larger smoothing kernels (and when separately estimating the
local variance for each smoothed voxel, which has been the standard in
SPM, including SPM2, \[4\]), one should keep in mind that the loss of
spatial precision is more dramatic in the resulting t-map than in the
images itself due to nonlinear effects on the voxel variances \[5\]. In
the worst case, the maximum t-value may be located in an area with no
signal at all (e.g. white matter).

## References

\[1\] Rosenfeld A, Kak AC (1982) Digital Picture Processing. New York:
Academic Press

\[2\] Hopfinger JB, Büchel C, Holmes AP, Friston KJ (2000) Study of
analysis parameters that influence the sensitivity of event-related fMRI
analyses. Neuroimage 11:326--33

\[3\] Worsley KJ, Marrett S, Neelin P, Evans AC (1996) Searching scale
space for activation in PET images.Hum Brain Mapping 4:74--90.

\[4\] Friston KJ, Frith CD, Liddle PF, Frackowiak RSJ (1991) Comparing
functional (PET) images: the assessment of significant change. J Cereb
Blood Flow Metab 11:690--699

\[5\] Reimold M, Slifstein M, Heinz A, Mueller-Schauenburg W, Bares R
(2006). Effect of spatial smoothing on t-maps: arguments for going back
from t-maps to masked contrast images. J Cereb Blood Flow Metab
26:751--759
