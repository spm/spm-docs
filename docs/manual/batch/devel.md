# Developer's guide

## SPM and Matlabbatch code organisation

This is a short overview describing code organisation and interfaces
between SPM and the batch system.

### Code organisation

Most features are implemented in:

- `fullfile(spm('dir'),'matlabbatch')`: core batch system.

- `fullfile(spm('dir'),'config')`: SPM config files.

- `spm_jobman.m` and `spm_select.m`: wrappers to Matlabbatch.

Some assignments to configuration items are guarded by validity checks.
Usually, there will be a warning issued if a wrong value is supplied.
Special care needs to be taken for `.prog`, `.vfiles`, `.vout`, `.check`
functions or function handles. The functions referenced here must be on
MATLAB path before they are assigned to one of these fields. For
toolboxes, this implies that toolbox paths must be added at the top of
the configuration file.

### Interfaces between SPM and Matlabbatch

Configuration files:  
Configuration items are defined as objects. Structs of type `<type>` in
SPM5 are represented as objects of class `cfg_<type>`. There is a class
`cfg_exbranch` which is used for branches that have a `.prog` field.

Dependencies:  
Dependencies require computations to return a single output argument
(e.g. a cell, struct). Parts of this output argument can be passed on to
new inputs at run-time.

Interface to the batch system:  
- `cfg_util` Configuration management, job management, job execution,

- `cfg_serial` A utility to fill missing inputs and run a job
  (optionally with a GUI input function),

- `cfg_ui` Graphical User Interface.

## Configuration Code Details

Configuration code is split into two files per configuration:

spm_cfg\_\*.m  
Configuration classes, `.check`, `.vout` subfunctions

spm_run\_\*.m  
Run-time code, takes job structure as input and returns output structure
as specified in `.vout`.

In a few cases (where there was no additional magic in the code),
run-time code has been integrated into the main SPM code. This may be
useful to run test batches without using the configuration/batch system.

### Virtual Outputs

Virtual outputs are described by arrays of `cfg_dep` objects. These
objects contain a "source" and a "target" part. Functions may have more
than one virtual output (e.g. one output per session, a collection of
results variables). One `cfg_dep` object has to be created for each
output.

Only two fields in the "source" part need to be set in a `.vout`
callback:

sname  
A display name for this output. This will appear in the dependencies
list and should describe the contents of this dependency.

src_output  
A subscript reference that can be used to address this output in the
variable returned at run-time.

tgt_spec (optional)  
A description on what kind of inputs this output should be displayed as
dependency. This is not very convenient yet, the `match` and
`cfg_findspec` methods are very restrictive in the kind of expressions
that are allowed.

The `.vout` callback will be evaluated once the configuration system
thinks that enough information about the *structure* of the outputs is
available. This condition is met, once all in-tree nodes
`cfg_(ex)branch`, `cfg_choice`, `cfg_repeat` have the required number of
child nodes.

The `.vout` callback is called with a job structure as input, but its
code *should not rely* on the evaluation of any contents of this
structure (or at least provide a fallback). The contents of the leaf
nodes may not be set or may contain a dependency object instead of a
value during evaluation of `.vout`.

The "target" part will be filled by the configuration classes, the
`src_exbranch` field is set in `cfg_util`.

### SPM Startup

The top level configuration file for SPM is `spm_cfg.m`. It collects SPM
core configuration files and does toolbox autodetection. If a toolbox
directory contains `*_cfg_*.m` files, they will be loaded.

### Defaults Settings

In Matlabbatch, there are different ways to set defaults:

1.  <span id="def:cf" label="def:cf"></span>in the configuration file
    itself,

2.  <span id="def:cd" label="def:cd"></span>in a defaults file, which
    has a structure similar to a harvested job,

3.  <span id="def:def" label="def:def"></span>using a `.def` field for
    leaf items.

Defaults set using option <a href="#def:cf" data-reference-type="ref"
data-reference="def:cf">[def:cf]</a> or
<a href="#def:cd" data-reference-type="ref"
data-reference="def:cd">[def:cd]</a> will only be updated at
SPM/matlabbatch startup. Defaults set using option
<a href="#def:def" data-reference-type="ref"
data-reference="def:def">[def:def]</a> will be set once a new job is
started. These defaults take precedence over the other defaults.

In core SPM, these defaults refer to `spm_get_defaults`, which accesses
`spm_defaults`. Toolboxes may use their own callback functions.

Toolboxes should set their defaults using the `.def` fields, using a
mechanism similar to `spm_get_defaults`. This allows for flexibility
without interfering with SPMs own defaults.

## Utilities

### Batch Utilities

Matlabbatch is designed to support multiple applications. A standard
application "BasicIO" is enabled by default. Among other options, it
contains file/file selection manipulation utilities which can be used as
as dependency source if multiple functions require the same set of files
as input argument. For debugging purposes, "Pass Output to Workspace"
can be used to assign outputs of a computation to a workspace variable.

The `cfg_confgui` folder contains an application which describes all
configuration items in terms of configuration items. It is not enabled
by default, but can be added to the batch system using
`cfg_util('addapp'...)`. This utility can be used generate a batch
configuration file with the batch system itself.

### MATLAB Code Generation

The `gencode` utility generates MATLAB `.m` file code for any kind of
MATLAB variable. This is used to save batch files as well as to generate
configuration code.

### Configuration Management

The backend utility to manage the configuration data is `cfg_util`. It
provides callbacks to add application configurations, and to load,
modify, save or run jobs. These callbacks are used by two frontends:
`cfg_ui` is a MATLAB GUI, while `cfg_serial` can be used both as a GUI
and in script mode. In script mode, it will fill in job inputs from an
argument list. This allows to run predefined jobs with e.g. subject
dependent inputs without knowing the exact details of the job structure.
