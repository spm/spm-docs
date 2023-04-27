# Multimodal, Multisubject data fusion 

## Preprocessing M/EEG data

We will start by creating pipelines (using SPM's batch interface) for
preprocessing the M/EEG data for a single subject, and then scripting
these pipelines to repeat over multiple subjects. For no particular
reason, we will start with Subject 15.

### Convert (and epoch)

The first step is to convert raw M/EEG data from its native format
(which depends on the acquisition system) to the format used by SPM. In
the present example, the raw data are continuous. They can be converted
to continuous SPM data, but to save disk space and time, we can "cut
out" time windows (epochs) around the trials during the conversion step,
so that the resulting SPM file contains epoched data.

In the batch editor, select SPM on the top toolbar, and from the
dropdown menu select M/EEG. At the top of the new dropdown menu, select
"Conversion". Once selected, the Module List on the left of the batch
editor window will now list "Conversion" as the first (and only) step.
Within the main, Current Module window will be listed several variables.
The first variable listed is "File Name". On the right hand of this
pane, you will see "$<$-X", this indicates that you need to update this
field. To do so, click on "File Name", this will then open up your
current working directory. For this example we will be using the M/EEG
data from subject number 15 (in the `Sub15` directory). Select the file
named `run_01_sss.fif` and press "Done".

Many variables in the Current Module window have default values, but we
need to change some of them. For example, we want to epoch during
conversion, so highlight the "reading mode" and change from "continuous"
to "epoched". To epoch, we need to define the start and end of each
trial. This information normally comes from a trigger channel containing
pulses that encode the onset and type of trial. You can ask SPM to read
this channel and select the relevant trial codes by hand. However, here
we will use "trial definition" files that have been created in advance
(given the complexity of the trigger codes in this dataset; contact
authors for more information on trial definitions). So select "Trial
definition file" under the "epoched" option, click on "Specify" and
select the file `run_01_trldef.mat`. (The contents of this file conform
to the FieldTrip format, with one row per trial, each with three
columns, which represent the sample numbers from the start of the
continuous recording for 1) onset of the epoch, 2) offset of the epoch
and 3) pre-stimulus baseline period.) Each trial runs from -500ms to
+1200ms relative to the stimulus onset. (In fact, we only really care
about the period -100ms to +800ms in the analysis below, and will later
"crop" the trials to this period, but we first need the extra 400ms at
the start and end to provide "padding" to avoid "end-effects" in the
wavelet analysis done later.)

Another change to the defaults is that we do not want to convert all
channels in the original file (since many are extraneous), so will
select a subset by their type. We first need to delete the default
option to convert all channels. To do this, click "channel selection",
and scroll down until you can select the "Delete All(1)" option. Then
click the "channel selection" again, but this time choose the option
"New: Select channels by type". This will add "Select channels by type"
to the Current Module, and you will see "$<$-X" on the right hand side
of this, indicating the need for user input. Click the "$<$-X" and then
select "EEG" from the "Current Item" section. Repeat this process to
additionally include the "MEGMAG" and "MEGPLANAR" channels.

The remaining options for conversion can be left with their default
values (which includes the output filename, which defaults to the input
filename, simply prepended with `spmeeg_`). Once all variables are
specified, the "play" button on the top toolbar will turn green and the
batch could be run. However, for this example, we will continue to use
the current batch editor window, so do not press the "play" button yet.

### Prepare

The next step in the current pipeline is to update some other properties
of the data using the "Prepare" module. This is a general-purpose
"housekeeping" module that includes options like re-defining channel
names, types, locations, etc. as specific to the particular laboratory
set-up. In our case, some of channels currently labelled EEG were in
fact used to record EOG.

Select "Prepare", from the preprocessing menu. Highlight "Prepare" in
the Module list, this will open up the variables in the current module
window. Again we need to complete those variables indicated by the
"$<$-X". If we had already run the previous conversion stage, we could
select the new `spmeeg_run_01_sss.mat` file produced by that stage as
the input for this stage. Here however, we will create a pipeline in
which all stages are run in one go, in which case we need to tell SPM
that the output of the conversion step, even though not yet created,
will be the input of this preparation step. You can do this by selecting
the "Dependency" button located further down the window. This will open
up a new window, listing all the processing steps up to this point. So
far this is just one, the conversion step. Highlight this step and
select "OK".

The next variable to define is the "task(s)". Clicking this variable
will display a variety of options in the "current item" box. Within
this, select "New: Set channel type", then return to the current module
window. In here, highlight "Channel selection", which displays further
variables within the current item window. Select "New: Custom channel".
Now return to the current module box and select the variable with an
"$<$-X". This should be "Custom channel". Selecting this and clicking on
"Specify" will open up a small text window. Within this, type `EEG061`.
Create a new Custom channel and type `EEG062`; then select "new channel
type" and highlight "EOG". This is because channels EEG061 and EEG062 in
fact represented bipolar horizontal (HEOG) and vertical (VEOG)
electroculagrams respectively, which can be used to detect ocular
artifacts.

This process then needs to be repeated for two other new channel types.
First create a new channel type like above, label it `EEG063` and set
its channel type as "ECG". This channel represents the
electrocardiogram, which can be used to detect cardiac artifacts.
Second, create another new channel labelled `EEG064` and set its channel
type as "other" (this is just a free-floating electrode, and does not
contain data of interest).

One more job we need to do is specify "bad" channels. These only exist
for the EEG (MEG bad channels are assumed to be corrected by the prior
MaxFiltering step). Bad EEG channels were identified by hand by an
experienced EEG operator and saved in a file called `bad_channels.mat`
in each subject's directory. It is important to note that there are many
other ways that bad channels can be defined automatically by SPM, using
the "artefact" module for example, but the various options for this are
not explored in this chapter. To specify bad channels, add a second
Prepare module, select the "New: Set/unset bad channels", then under
"Channel selection", replace the default "All" with the option "New:
Channel file", and then select the `bad_channels.mat` file in this
subject's directory (note it is important that you delete the "All (1)"
option first). In fact, this subject does not have any bad channels, so
this step could be ignored if you only want to analyse this subject. But
if you want to use the preprocessing script that we will use below for
the remaining 15 subjects, you need to set-up this stage now (i.e, the
`bad_channels.mat` files for the other subjects are not empty).

This will complete the preparation stage, as indicated by the "play"
button turning green. (Note that you can also use this "Prepare" option
interactively, in order, for example, to create and save a bad channel
selection file).

### Downsample

The data were sampled at 1000Hz, but for the analyses below, we rarely
care about frequencies above 100Hz. So to save processing time and disk
space (by a factor of 5) we can therefore downsample the data to 200Hz
(which includes a lowpass filtering to avoid aliasing). Select
"downsampling" from the module list "SPM -- M/EEG -- Preprocessing",
click on "File Name", select "Dependency" and in the pop-up window,
select the prepared datafile at the bottom of the list. Next, set the
downsampling rate by clicking the "New sampling rate" variable within
the current module box. Type `200` into the pop-up window that appears
and use "OK" to set this value. The output file of this stage will be
prepended with a `d`.

### Baseline Correction

The next step in this preprocessing pipeline is to baseline correct the
data by subtracting from each channel the mean from -100ms to 0ms
relative to the stimulus onset. Select "Baseline Correct" from under the
"SPM -- M/EEG -- Preprocessing" menu. Select the input file name as
being dependent on the output of the downsampling by selecting the
"Dependency". To define the baseline, highlight the "Baseline" button in
the current module window and click on the "Specify" button. The
baseline needs to be entered as a 1-by-2 array. Enter `[-100 0]` (units
are milliseconds). The prefix of this file name will be `b`. Note, that
to be on the safe side it might be a good idea to perform baseline
correction before downsampling. The reason is that in the general case
there might be large DC offsets in the data leading to prolonged ringing
that even padding will not protect against. For our data, however, the
order shown here works equally well.

### Deleting intermediate steps (optional)

The four steps (modules) described above create a preprocessing pipeline
for the data. If this pipeline is run straight away, there will be four
different output files. If you are short of diskspace, you might want to
delete some of the intermediate files. To do this, select "SPM" from the
top toolbar of the batch editor window and choose "M/EEG -- Other --
Delete" several times. Then you will need to specify the File Names to
delete. Highlight each "Delete" module and set the File Name as the
output of the Prepare step using the "Dependency" button to delete any
output from the conversion/prepare step onward. In fact, all processing
steps up until the most recent step (Baseline-correction) can be
deleted. To do this, right click on "Delete" in the Module List and
select "Replicate Module", but change the dependency to the downsampled
file.

## Creating a script for combining pipelines within a subject.

Once you have created a linear pipeline, you might want to repeat it on
multiple sessions (runs) within a subject, or even across multiple
subjects. In the present case, there were 6 independent MEG runs
(separated only by a short period to give the subjects a rest), which
can all be processed identically. One option would be to save the batch
file, manually alter the "File Name" that is initially loaded into the
batch editor, and repeat this process separately for each run. A more
powerful approach is to create a script. To do this, select "File" from
the Batch Editor window, and select "Save Batch and Script". This will
produce two files: a batch file (same as that created when you save a
batch) but also a MATLAB script that calls that batch file. So if you
call the batch file `batch_preproc_meeg_convert`, you will get a batch
file called `batch_preproc_meeg_convert_job.m` and a script file called
`batch_preproc_meeg_convert.m`.

The script file `batch_preproc_meeg_convert.m` will automatically be
loaded into the MATLAB editor window, and should appear something like
this:

```matlab
% Conversion: File Name - cfg_files
% Conversion: Trial File - cfg_files
% Prepare: Channel file - cfg_files
nrun = X; % enter the number of runs here
jobfile = {'batch_preproc_meeg_convert_job.m'};
jobs = repmat(jobfile, 1, nrun);
inputs = cell(3, nrun);
for crun = 1:nrun
    inputs{1, crun} = MATLAB_CODE_TO_FILL_INPUT; % Conversion: File Name - cfg_files
    inputs{2, crun} = MATLAB_CODE_TO_FILL_INPUT; % Conversion: Trial File - cfg_files
    inputs{3, crun} = MATLAB_CODE_TO_FILL_INPUT; % Prepare: Channel file - cfg_files
end
spm('defaults', 'EEG');
spm_jobman('run', jobs, inputs{:}); 
```

At the top of this script is listed the variable `nrun = X`: replace `X`
with `6` for the six runs you wish to convert. You also need to complete
the missing MATLAB code needed for each run: here, 1) the raw input file
to convert, 2) the trial definition file for that run, and 3) the
channel file containing the bad channels (which is actually the same for
each run, but differs across subjects, which will matter later when we
extend the script to loop over subjects). In order to automate selection
of these files, you need to know some basic MATLAB . For example,
because the files are named systematically by run, we can complete the
relevant lines of the script with:

```matlab
inputs{1, crun} = cellstr(fullfile(rawpth,'Sub15','MEEG',sprintf('run_%02d_sss.fif',crun)));
     inputs{2, crun} = cellstr(fullfile(rawpth,'Sub15','MEEG','Trials',sprintf('run_%02d_trldef.mat',crun)));
     inputs{3, crun} = cellstr(fullfile(rawpth,'Sub15','MEEG','bad_channels.mat'));
```

where `rawpth` refers to your base directory, `Sub15` is the subject
directory that contains `MEG` and `Trials` sub-directories (assuming you
mirrored the directory structure from the FTP site) and `%02d` refers to
the run number, expressed as two digits.

The other file -- the batch or "job" file -- can be reloaded into the
batch editor at any time. It can also be viewed in the MATLAB editor. If
you type `edit batch_preproc_meeg_convert_job.m`, you will see your
selections from the earlier GUI steps. But we need to change those
selections that depend on the run number (so they can be passed from the
script instead). To make the batch file accept variables from the script
file, we need change three of the specifications to `’<UNDEFINED>’`
instead. So edit the following lines so they read:

```matlab
matlabbatch{1}.spm.meeg.convert.dataset = '<UNDEFINED>';
     matlabbatch{1}.spm.meeg.convert.mode.epoched.trlfile = '<UNDEFINED>';
     ...
     matlabbatch{2}.spm.meeg.preproc.prepare.task{4}.setbadchan.channels{1}.chanfile = '<UNDEFINED>';
```

Then save these edits (overwriting the previous batch file). If you're
unsure, the batch file should look like the
`batch_preproc_meeg_convert_job.m` file in the `SPM12batch` part of the
`SPMscripts` directory on the FTP site.

This completes the first part of the preprocessing pipeline. You can
then run this script by selecting the green play button on the upper
toolbar of the script MATLAB Editor window. The results will be 6 files
labelled `bdspmeeg_run_%02d_sss.mat`, where `%02d` refers to the run
number `1-6`. If you want to view any of these output files, press
"Display" on the main SPM menu pane, select "M/EEG", then select one of
these files. You will be able to review the preprocessing steps as a
pipeline from the "History" section of the "Info" tab, and can view
single trials by selecting one of the EEG, MEG (magnetometer) or MPLANAR
(gradiometer) tabs (see other chapters for how to use the buttons in the
M/EEG Review window).

### Merging (concatenating runs)

To analyse the data as one file, the six runs need to be merged. To do
this, select "Merging" from "SPM -- M/EEG -- Preprocessing -- Merging",
select "File Names", "specify", and select the 6 file names
`bdspmeeg_run_%02d_sss.mat`. If you were to run this stage now, the
output file would match the first input file, but be prepended with a
`c`, i.e, `cbdspmeeg_run_01_sss.mat`. However, we will wait to add some
more modules before running, as below. At this stage, you could also add
delete modules to delete all the previous individual run files (since
the concatenated file will contain all trials from all runs, i.e,
contain the same data).

### Prepare (a montage for re-referencing the EEG)

Below, we want to re-reference the EEG data to the average across
channels (as is sometimes conventional for ERP analyses; note the MEG
data have no reference). We can do this with the "montage" module below,
which is a general purpose module for creating new channel data from
linear combinations of existing channel data. However, we first need to
create a montage file, which includes a matrix that, when multiplied by
the existing data, creates the new channel data. There is another
sub-function (task) of the "Prepare" module that does this, so add
another "Prepare" module, select the dependency on the previous merged
file as the "FileName", but for the "task", select "Create average
reference montage" and enter `avref_montage.mat` as the output filename.
(If you want to look at this montage, you can run this module,
`load avref_montage.mat` into MATLAB and look at the `montage.tra`
matrix, where you can see that each new EEG channel is equal to the old
EEG channel minus the average of all other channels.) Note that this
montage will differ across subjects because different subjects had
different EEG channels marked as bad (from steps above), and bad
channels need to be excluded when estimating the average EEG signal
across channels.

At this point, we can also do one more bit of house-keeping within the
same "Prepare" module, which is simply to re-order the condition labels.
This only matters for the final stage of "Contrasting conditions" below,
where the contrast weights assume a certain order of the conditions. The
current order of conditions is based purely on the order they appear in
the raw data (e.g, if the first few trials of the first run were:
Scrambled, Unfamiliar, Unfamiliar, Scrambled, Famous\..., then the
condition labels will be ordered Scrambled-Unfamiliar-Famous), and this
may vary across subjects. To set the condition order to be invariant
across subjects, add a new task by selecting the "Sort conditions" task,
then "Specify conditions lists" add three "New: Condition labels", and
name them "Famous", "Unfamiliar" and "Scrambled" (in that order). Note
that this operation does not physically reorder the trials at this
stage, but just defines the order that will be used where required at
later steps.

### Montage

Now we have the montage file, we can apply it, in order to re-reference
the EEG data to the average. Select "Montage" from the "Preprocessing"
menu, and specify the "File Name" as being dependent on the output of
the "Merging" module above. For the "Montage file name", choose a
different dependency, namely the output of the "Prepare" module above.
Next, highlight "keep other channels" and select "yes" in the "Current
Item" box, in order to keep all the MEG channels (which are unchanged).
All other default values can remain the same. The output file will be
prepended with `M`.

As with the previous pipeline, if you are short of diskspace
(particularly if you later run all 16 subjects), the outputs produced
from the intermediate stages can be deleted using the "SPM -- M/EEG --
Other -- Delete" function (see earlier). However, refrain from deleting
the montaged data, as these will be used later in the Section on
Time-Frequency analysis.

### Save batch and review.

This completes the second part of the preprocessing pipeline. At this
point, you can run the batch. Alternatively, you can save and script,
and run the batch from a script. The resulting script can also be
combined with the previous script created (e.g., in the `SPMscripts` FTP
directory, scripts for all the stages are appended into a single master
script called `master_script.m`, which loops over each subject too).
Remember that, if you want to pass variables from a script to a batch,
you need to first ensure the relevant fields in the batch file are set
to `’<UNDEFINED>’` (see for example the `batch_preproc_meeg_merge_job.m`
file in the `SPM12batch` FTP directory).

To view the output, press "Display" on the main SPM menu pane, select
"M/EEG", then select `Mcbdspmeeg_run_01_sss.mat`. Again, you will be
able to review the preprocessing steps from the "History" section of the
"Info" tab.

--8<-- "addons/abbreviations.md"
