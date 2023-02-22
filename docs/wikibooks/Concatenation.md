A session (also referred to as a \'run\') is a period of data collection
in the scanner. Between sessions, data acquisition is paused, typically
to give the participant a rest and enable them to communicate with the
experimenter. SPM by default specifies multi-session GLMs for univariate
inference. However, you may want to concatenate all sessions into one
for later (esp. connectivity) analyses.

## Why concatenate?

Concatenation is not necessary for GLM-based univariate inferences.
Moreover, it could lead to different estimates compared to the
multi-session \'full model\' because 1) if a condition is too close to
the end of a session, after being convolved with an HRF it could wrongly
model data from the subsequent session (acquiring additional scans
before the end of each session can effectively mitigate this effect) and
2) It disables condition × session interactions by giving a single beta
to each condition throughout the concatenated session.

However, [SPM timeseries
extraction](SPM/Timeseries_extraction "wikilink") operates on a
per-session basis. It is common, therefore, to concatenate and extract
timeseries from multiple sessions for connectivity analyses such as PPI
or DCM.

## Procedure (fMRI)

### Preparations

The trickiest part of concatenation is ensuring your onsets, collated
across sessions, are correct. If you have obtained onsets within each
session, in order to append these onsets for your new concatenated GLM,
you need to add to these onsets the length of sessions before the
session. For example, suppose you have 3 sessions and corresponding
onsets 1-3, and each session has 60, 55, 44 volume scans respectively:

\<syntaxhighlight lang=\"matlab\"\> scans = \[60 55 40\];

% If onsets are in scans onsets_concat = \[onsets1,onsets2 + 60,
onsets3 + 115\];

% If onsets are in seconds TR = 2; % Replace with the actual value
according to your scanning protocol onsets_concat = \[onsets1,onsets2 +
60\*TR, onsets3 + 115\*TR\]; \</syntaxhighlight\>Other regressors (e.g.
head motion regressors) should be similarly concatenated.

### Specifying the concatenated GLM

You can then specify your single-session first-level design matrix.
Instead of having multiple sessions, you should now specify only one
session which includes image volumes from all sessions and appended
onsets and other regressors. Run the batch to specify the model, but do
not estimate for now.

### By-session adjustment and estimation

Before estimating, you still need to tell SPM which scans belong to
which original session so that it can adjust for its effects. This will
add to your single-session GLM block effect regressors (replacing the
usual mean column in the design matrix), and correct the high-pass
filter and temporal non-sphericity calculations to account for the
original session lengths.

Run the following code in the main Matlab window. Again, \'scans\' here
denotes the number of volumes in each session in the original
timeseries.

\<syntaxhighlight lang=\"matlab\"\> scans = \[60 55 40\];
spm_fmri_concatenate(\'SPM.mat\', scans); \</syntaxhighlight\>

The adjusted GLM (SPM.mat) will replace the GLM you specified before
(its copy will be saved as SPM_backup.mat). Now you can estimate the GLM
and add contrasts (most importantly, an effect of interest F contrast
for timeseries extraction) in the normal way.

Now you may use SPM Volumes of Interest utility to [extract
timeseries](SPM/Timeseries_extraction "wikilink") from the single
session.
