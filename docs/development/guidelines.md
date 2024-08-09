# Style and testing for SPM

These guidelines are designed to make it as easy as possible to get involved.
If you have any questions that are not discussed in our documentation, or if it is
difficult to find what you are looking for, please let us know by opening an
[issue](https://github.com/spm/spm/issues).


## Coding Style

In short the general formatting guidelines are:

* Use whitespace to make the code more readable.
* No whitespace at the end of a line (trailing whitespace).
* Comments are good, especially when they explain the algorithm.
* Try to adhere to a 76 character line length limit.
* Use lower case with underscores for function names.
* avoid use of varargin and nargin
* Avoid padding brackets with spaces. For example, `spm_fcn(value)` is preferred over `spm_fcn( value )`.

New functions should follow this overall format:

```
function [R1,...] = spm_fcn(P1,...)
% <- H1 line : Single line description using the imperative ->
% FORMAT [R1,...] = spm_fcn(P1,...)
%
% P1 - <-Argument/parameter description, including ranges->
%
% R1 - <-Description of returned values->
%__________________________________________________________________________
%
% HELP text
% Long description of function, including i/o details, algorithms and refs.
%__________________________________________________________________________

% AUTHOR
% Copyright (C) YEAR Wellcome Centre for Human Neuroimaging


%==========================================================================
% - B A N N E R   S E C T I O N   H E A D I N G
%==========================================================================

%-MAJOR section heading
%==========================================================================

%-MINOR section heading
%--------------------------------------------------------------------------

```
## Dependencies
SPM is designed to be as designed to rely on as few extra toolboxes as possible. As such we advise against including code that is not available in base MATLAB. There is an exception to this rule for M/EEG development as SPM bundles Fieldtrip with in its distribution, so developers may call Fieldtrip functions. 
 