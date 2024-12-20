# SPM version numbers
SPM has a **version** and a **release**. At the time of writing, the version is **SPM12** and the release is **dev**. 

Other toolboxes that depend on SPM use this information to check whether a compatible SPM version is installed. A common pattern is to split the version string into two parts, e.g. SPM12 becomes **SPM** and **12**, and then the latter part is compared to the desired version number.

## Transition to calendar versioning
SPM is moving towards [calendar versioning](https://calver.org/). 

The proposed new format is **YY.0M[.MICRO]**, where YY is the short year (e.g. 24 for the year 2024), 0M is the zero padded month (e.g. 06 for June or 12 for December) and .MICRO is an optional integer for patches.

For backward compatibility, the proposal is to set the version and release as follows:

-	**Version**: SPMYY, where YY is the year of the most recent release, e.g. SPM24 
-	**Release**: either 00.00 for the development version on Github or for a release: YY.0M[.MICRO], e.g. 24.07 for July 2024

## Where the SPM version number is stored
The SPM version is stored on the second line of the file Contents.m in the root directory, or Contents.txt when SPM is in deployed mode (i.e., compiled).

## Querying SPM’s version number
To obtain the version and release call:

```Matlab
[v, r] = spm('ver')
```

where v is the version and r is the release. For convenience, to return the version and release as a single string, instead call: 

```Matlab 
spm('version')
```
