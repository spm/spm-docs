# Batch tutorial

Details about the algorithms used for data processing are given in the
other sections of this manual. This section explains how a sequence of
processing steps can be run at once without MATLAB programming. SPM12
includes `matlabbatch`[^1] which has been derived from the SPM5 batch
system, but is also available as a separate package.

In `matlabbatch`, each data processing step is called "module". There
are e.g. modules for spatial processing of MRI data (realignment,
normalisation, smoothing), statistics (fMRI or factorial design
specification, model estimation, contrast specification). A batch
describes which modules should be run on what kind of data and how these
modules depend on each other.

Compared to running each processing step interactively, batches have a
number of advantages:

Documentation  
Each batch can be saved as a MATLAB script. All parameters (including
default settings) are included in this script. Thus, a saved batch
contains a full description of the sequence of processing steps and the
parameter settings used.

Reproducibility  
Batches can be saved, even if not all parameters have been set. For a
multi-subject study, this allows to create template batches. These
templates contain all settings which do not vary across subjects. For
each subject, they can be loaded and only subject-specific parts need to
be completed.

Unattended execution  
Instead of waiting for a processing step to complete before entering the
results in the next one, all processing steps can be run in the
specified order without any user interaction.

Multiple batches  
Multiple batches can be loaded and executed together.

Error reporting  
If a batch fails to complete, a standardised report will be given in the
MATLAB command window. When running a batch from the GUI, this can be
saved to an error `.mat` file.

## Single subject

In this tutorial we will develop a batch for spatial processing and fMRI
statistics of a single subject of the "Face" example dataset (see
chapter <a href="#Chap:data:faces" data-reference-type="ref"
data-reference="Chap:data:faces">[Chap:data:faces]</a>). To follow this
tutorial, it is not necessary to download the example dataset, except
for the last step (entering subject dependent data).

To create a batch which can be re-used for multiple subjects in this
study, it is necessary to collect/define

- study specific input data (e.g. MRI measurement parameters, time
  constants of the functional experiment, number of sessions),

- necessary processing steps,

- data flow between processing steps.

Subject specific input data (original functional and structural MRI
data, subject specific experiment parameters) should be entered after
the batch template has been saved.

### Study specific input data

This dataset consists of fMRI data acquired in a single session and a
structural MRI. See
section <a href="#sec:batch_interface_advanced" data-reference-type="ref"
data-reference="sec:batch_interface_advanced">1.2</a> to learn how to
deal efficiently with multi-session data. MRI parameters and experiment
details are described in
chapter <a href="#Chap:data:faces" data-reference-type="ref"
data-reference="Chap:data:faces">[Chap:data:faces]</a>.

### Necessary processing steps

#### Helper modules

Some SPM modules produce graphics output which is captured in a
PostScript file in the current working directory. Also, a new directory
needs to be created for statistics. The "BasicIO" menu provides a
collection of modules which are useful to organise a batch. We will need
the following modules:

- Named directory selector
  (`BasicIO > File/Dir Operations > Dir Operations > Named Directory Selector`)

- Change directory
  (`BasicIO > File/Dir Operations > Dir Operations > Change Directory`)

- Make directory
  (`BasicIO > File/Dir Operations > Dir Operations > Make Directory`)

#### SPM processing

For a classical SPM analysis, the following processing steps are
necessary:

- Realignment (`SPM > Spatial > Realign > Realign: Estimate & Reslice`)

- Slice timing correction (`SPM > Temporal > Slice Timing`)

- Coregistration (`SPM > Spatial > Coregister > Coregister: Estimate`)

- Segmentation (`SPM > Tools > Old Segment`)

- Normalisation (`SPM > Tools > Old Normalise > Old Normalise: Write`)

- Smoothing (`SPM > Spatial > Smooth`)

- fMRI design (`SPM > SPM > Stats > fMRI model specification`)

- Model estimation (`SPM > Stats > Model estimation`)

- Contrasts (`SPM > Stats > Contrast Manager`)

- Results report (`SPM > Stats > Results Report`)

Note that this examplar analysis pipeline is ancient and the
`SPM > Tools > Old Segment` and
`SPM > Tools > Old Normalise > Old Normalise: Write` modules could be
replaced by a single `SPM > Spatial > Normalise: Estimate & Write` one.

### Add modules to the batch

The helper modules and the SPM processing modules can be assembled using
the GUI. Click the "Batch" button in the SPM Menu window. First, add the
helper modules, followed by the SPM modules in the order listed above.
Do not configure any details until you have selected all modules.

<figure id="fig:basicio_spm">
<p><img src="../../../assets/figures/manual/batch/batch_spm.png" alt="image" /> <img
src="batch/batch_basicio.png" alt="image" /></p>
<figcaption>SPM and BasicIO application menus</figcaption>
</figure>

### Configure subject-independent data

Now, go through the batch and configure all settings that are
subject-independent (e.g. the name of the analysis directory, slice
timing parameters) as described in
chapter <a href="#Chap:data:faces" data-reference-type="ref"
data-reference="Chap:data:faces">[Chap:data:faces]</a>. Do not enter any
data that is specific for a certain subject. The values that need to be
entered are not repeated here, please refer to the corresponding
sections in chapter <a href="#Chap:data:faces" data-reference-type="ref"
data-reference="Chap:data:faces">[Chap:data:faces]</a>.

The file `man/batch/face_single_subject_template_nodeps.m` contains the
batch after all modules have been added and subject-independent data has
been entered.

#### Named Directory Selector

Input Name  
Give this selection a name (e.g. "Subject directory") - this name will
be shown in the dependency list of this batch.

Directories  
Add a new directory selector, but do not enter a directory itself.

#### Change Directory

Nothing to enter now.

#### Make Directory

New Directory Name  
"categorical" - the name of the analysis directory. This directory will
be created at batch run-time in the subject directory.

#### Realign: Estimate & Reslice

Data  
Add a new "Session" item. Do not enter any files for this session now.

#### Slice Timing

Data  
Add a new "Session" item. Do not enter any files for this session now.

Timing options  
Enter data for "Number of slices", "TR", "TA", "Slice order", "Reference
slice".

#### Coreg: Estimate

Nothing to enter now.

#### Segment

Nothing to enter now.

#### Normalise: Write

Data  
Add a new "Subject". Do not enter any files now.

Writing Options  
Adjust bounding box, voxel sizes, interpolation

#### Smooth

FWHM  
Enter FWHM

#### fMRI model specification

Enter all data which is constant across subjects.

Timing parameters  
Enter values for "Units for design", "Interscan interval", "Microtime
resolution", "Microtime onset"

Data & Design  
Add a new "Session" item. Do not enter scans, conditions or regressors
yet. They will be added as dependencies or subject specific inputs. If
you want to make sure to remember this, you can highlight "Multiple
conditions" and select "Clear Value" from the "Edit" menu. Do the same
for "Multiple regressors". This will mark both items with an `<-X`,
indicating that something must be entered there.

Factorial design  
Enter the specification for both factors.

Basis functions  
Select the basis function and options you want to use.

#### Model estimation

Nothing to be entered yet for classical estimation.

#### Contrast manager

If you have selected the "Factorial design" option as described above,
SPM will automatically create some contrasts for you. Here, you can
create additional T- or F-contrasts. As an example, we will add an
"Effects of interest" F-contrast.

Contrast session  
Add a new "F-contrast" item.

Name  
Enter a name for this contrast, e.g. "Effects of interest".

Contrast vectors  
Add a new "Contrast vector" item. F-contrasts can have multiple rows.
You can either enter a contrast matrix in an "F contrast vector" entry,
or enter them row by row. To test for the effects of interest (1 basis
function and 2 derivatives for each of the four conditions) enter
`eye(12)` as F contrast vector.

Replicate over sessions  
This design does not have multiple sessions, so it is safe to say "No"
here.

#### Results report

Reviewing individual results for a large number of subjects can be very
time consuming. Results report will print results from selected
contrasts to a PostScript file.

Contrast(s)  
Enter `Inf` to print a report for each of the defined contrasts.

### Data flow

In chapter <a href="#Chap:data:faces" data-reference-type="ref"
data-reference="Chap:data:faces">[Chap:data:faces]</a>, each processing
step was performed on its own. In most cases, output data was simply
passed on from one module to the next. This scheme is illustrated in
figure <a href="#fig:flow" data-reference-type="ref"
data-reference="fig:flow">1.2</a>. Only the coloured items at the top of
the flow chart are subject specific and need to be entered in the final
batch. All arrow connections are subject-independent and can be
specified in the batch template.

<figure id="fig:flow">
<img src="../../../assets/figures/manual/batch/flow.png" />
<figcaption>Flow chart for batch</figcaption>
</figure>

#### Add dependencies

Based on the data flow in
figure <a href="#fig:flow" data-reference-type="ref"
data-reference="fig:flow">1.2</a>, modules in the batch can now be
connected. The batch containing all dependencies can be found in
`man/batch/face_single_subject_template.m`.

<figure id="fig:batch_dependency">
<img src="../../../assets/figures/manual/batch/batch_dependencies.png" />
<figcaption>Dependency selection</figcaption>
</figure>

Again, start editing at the top of the batch:

#### Named Directory Selector

Nothing to enter now.

#### Change Directory

Directory  
Press "Dependency" and select "Subject directory(1)". At run time, SPM
will change to this directory before batch processing continues.

#### Make Directory

Parent Directory  
Press "Dependency" and select "Subject directory(1)". The "categorial"
directory will be created in this directory.

#### Realign: Estimate & Reslice

Nothing to enter now.

#### Slice Timing

Session  
Press "Dependency" and select "Resliced Images (Sess 1)".

#### Coreg: Estimate

Reference Image  
Press "Dependency" and select "Mean Image".

#### Segment

Data  
Press "Dependency" and select "Coregistered Images". At run time, this
will resolve to the coregistered anatomical image.

#### Normalise: Write

Parameter File  
Press "Dependency" and select "Deformation Field Subj$\rightarrow$MNI
(Subj 1)".

Images to Write  
Press "Dependency" and select "Slice Timing Corr. Images (Sess 1)".

#### Smooth

Images to Smooth  
Press "Dependency" and select "Normalised Images (Subj 1)"

#### fMRI model specification

Directory  
Press "Dependency" and select "Make Directory 'categorical' "

Scans  
Press "Dependency" and select "Smoothed Images". Note: this works
because there is only one session in our experiment. In a multisession
experiments, images from each session may be normalised and smoothed
using the same parameters, but the smoothed images need to be split into
sessions again. See
section <a href="#sec:batch_interface_advanced" data-reference-type="ref"
data-reference="sec:batch_interface_advanced">1.2</a> how this can be
done.

Multiple regressors  
Press "Dependency" and select "Realignment Param File (Sess 1)".

#### Model estimation

Select SPM.mat  
Press "Dependency" and select "SPM.mat File (fMRI Design&Data)".

#### Contrast manager

Select SPM.mat  
Press "Dependency" and select "SPM.mat File (Estimation)".

#### Results report

Select SPM.mat  
Press "Dependency" and select "SPM.mat File (Contrasts)".

### Entering subject-specific data

Now, only 4 modules should have open inputs left (marked with `<-X`).
These inputs correspond to data which vary over the subjects in your
study:

Named Directory Selector  
Subject directory

Realign: Estimate & Reslice  
Raw EPI data for the fMRI session

Coreg: Estimate  
Anatomical image to be coregistered to mean EPI

fMRI model specification  
Names, conditions and onsets of your experimental conditions, specified
in a multiple conditions .mat file.

Using the GUI, you can now perform these steps for each subject:

1.  load the template batch

2.  enter subject-specific data

3.  save batch under a subject specific name.

After that, all batches for all subjects can be loaded and run at once.

This process can be automated using some basic MATLAB scripting. See
section <a href="#sec:batch_interface_cmd_cfg_serial" data-reference-type="ref"
data-reference="sec:batch_interface_cmd_cfg_serial">1.2.3.2</a> for
details.

<figure id="fig:batch_stages">
<img src="../../../assets/figures/manual/batch/batch_single_subject_template_nodeps.png" />
<img src="../../../assets/figures/manual/batch/batch_single_subject_template.png" />
<img src="../../../assets/figures/manual/batch/batch_single_subject.png" />
<figcaption>All stages of batch entry</figcaption>
</figure>

## Advanced features

### Multiple sessions

If an fMRI experiment has multiple sessions, some processing steps need
to take this into account (slice timing correction, realignment, fMRI
design), while others can work on all sessions at once (normalisation,
smoothing).

Two modules in BasicIO help to solve this problem:

Named File Selector  
Files can be entered here session by session. Note that this file
selector selects all files (not restricted to images) by default. To
select only images, set the filter string to something like `.*nii$` or
`.*img$`.

File Set Split  
This module splits a list of files based on an index vector. Named file
selector provides such an index vector to split the concatenation of all
selected images into individual sessions again.

### Processing multiple subjects in GUI

There are different ways to process multiple subjects in the batch
editor:

- Add the necessary processing steps when creating the job.

- Create a per-subject template, save it and load it multiple times
  (i.e. in the file selector, add the same file multiple times to the
  list of selected files).

- Use "Run Batch Jobs" from "BasicIO"

<figure id="fig:batch_multi_subject_template">
<img src="../../../assets/figures/manual/batch/batch_multi_subject_template.png" />
<figcaption>Using "Run Batch Jobs"</figcaption>
</figure>

In all cases, the data for all subjects has to be entered through the
GUI, and computation will be done for all subjects at once after all
data is entered. There is an example job `face_multi_subject_template.m`
that demonstrates the usage of "Run Batch Jobs" to run the single
subject template job described above. Note that the order and type of
inputs in the single subject template is important. Also, consistency
checks are limited. If inconsistent data is entered, the job will fail
to execute and return an error message.

To run this job for multiple subjects, simply repeat the "Runs" item as
many times as necessary and fill in the required data.

### Command line interface

The command line interface is especially useful to run multiple jobs at
once without user interaction, e.g. to process multiple subjects or to
combine separate processing steps. There is a "high-level" interface
using `spm_jobman`, which combines "low-level" callbacks to `cfg_util`.

#### SPM startup in command line mode

During normal startup, SPM performs important initialisation steps.
Without initialisation, SPM and its batch system will not function
properly. Consequently, an initialisation sequence needs to be run
before any batch job can be submitted.

MATLAB has several command line options to start without its GUI
(`-nodesktop`) or even without any graphics output to a screen
(`-nodisplay`). See MATLAB documentation for details.

To run SPM in `-nodisplay` mode, the file `spm_defaults.m` has to be
modified. The line `defaults.cmdline = 0;` must be changed to
`defaults.cmdline = true;`. In command line mode, SPM will not open any
figure window except the "Graphics" window.

Within MATLAB, the following commands are sufficient to set up SPM

1.  `spm('defaults', MODALITY)` where `MODALITY` has to be replaced by
    the desired modality (e.g. `'fmri'`)

2.  `spm_jobman('initcfg')`

After executing these commands, any SPM functions and batch jobs can be
run in the same MATLAB session.

#### Complete and run a pre-specified job

`spm_jobman('run', job[, input1, input2 ...])`

This interface takes a job and asks for the input to any open
configuration items one after another. If a list of appropriate inputs
is supplied, these will be filled in. After all inputs are filled, the
job will be run. Note that only items without a pre-set value will be
filled (marked with `<-X` in the GUI). To force a item to to be filled,
use "Edit:Clear Value" in the GUI or set its value to `'<UNDEFINED>'` in
the harvested job.

The job argument is very flexible, it can e.g. be a job variable, the
name of a script creating a job variable, even a cell list of any
mixture of variables and scripts. All job snippets found will be
concatenated into a single job, the missing inputs will be filled and
the resulting job will be run.

The batch system can generate a script skeleton for any loaded job. From
the batch GUI, this feature is accessible via "File:Save Batch and
Script". This skeleton consists of a commented list of necessary inputs,
a `for` loop to enter inputs for multiple runs or subjects and the code
to initialise and run the job. An example is available in
`face_single_subject_script.m`:

    % List of open inputs
    % Named Directory Selector: Directory - cfg_files
    % Realign: Estimate & Reslice: Session - cfg_files
    % Coreg: Estimate: Source Image - cfg_files
    % fMRI model specification: Multiple conditions - cfg_files
    nrun = X; % enter the number of runs here
    jobfile = {fullfile(spm('dir'),'man','batch','face_single_subject_template.m')};
    jobs = repmat(jobfile, 1, nrun);
    inputs = cell(4, nrun);
    for crun = 1:nrun
        % Named Directory Selector: Directory - cfg_files
        inputs{1, crun} = MATLAB_CODE_TO_FILL_INPUT;
        % Realign: Estimate & Reslice: Session - cfg_files
        inputs{2, crun} = MATLAB_CODE_TO_FILL_INPUT; 
        % Coreg: Estimate: Source Image - cfg_files
        inputs{3, crun} = MATLAB_CODE_TO_FILL_INPUT; 
        % fMRI model specification: Multiple conditions - cfg_files
        inputs{4, crun} = MATLAB_CODE_TO_FILL_INPUT; 
    end
    spm('defaults','fmri');
    spm_jobman('run',jobs,inputs{:});

The skeleton needs to be adapted to the actual data layout by adding
MATLAB code which specifies the number of runs and the input data in the
`for` loop.

Another example script and batch is available for the multimodal
dataset, called `multimodal_fmri_script.m` and
`multimodal_fmri_template.m`.

### Modifying a saved job

In some cases, instead of using the serial interface it may be more
appropriate to modify the fields of a saved or harvested job. By
default, jobs are saved as MATLAB `.mat` files, but they can also be
saved as `.m` files. These files contain a number of MATLAB commands,
which will create a variable `matlabbatch`. The commands can be modified
to set different values, add or remove options.

[^1]: <https://sourceforge.net/projects/matlabbatch>
