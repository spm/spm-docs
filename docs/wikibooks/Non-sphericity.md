# Non-sphericity specification in SPM5

## GUI

In batch mode for factorial design specification, one can specify
whether error variances are unequal across the levels of a factor and
whether error variances are correlated between different levels of the
same factor.

## Script, SPM.mat

After the design has been specified, SPM.xVi.Vi will hold a cell array
of non-sphericity component matrices. Each matrix should have as many
rows and columns as you have images in your design. A **1** in a certain
row *i* and column *j* means that images *i* and *j* are assumed to have
dependent error covariance. Note the special case if *i==j*, this
encodes for the error variance of image *i*.

If you are not satisfied with SPMs suggestions for error covariances,
you can insert your own list of Vi components after design
specification, before you estimate the model.

## Verification of covariance specifications

Covariance components can be verified using SPMs **Review Design**
facility - in the Menu there is an item called *Explore Covariance
structure*.
