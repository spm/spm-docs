# Introduction

The [SPM](http://www.fil.ion.ucl.ac.uk/spm/) software is a suite of
[MATLAB](http://www.mathworks.com/products/matlab/) functions to
organise and interpret functional neuroimaging data.

[SPM](http://www.fil.ion.ucl.ac.uk/spm/) is made freely available to the
(neuro)imaging community, to promote collaboration and a common analysis
scheme across laboratories. The software represents the implementation
of the theoretical concepts of Statistical Parametric Mapping in a
complete analysis package.

# Licence

SPM (being the collection of files given in the manifest in the
Contents.m file included with each distribution) is free but copyright
software, distributed under the terms of the GNU General Public Licence
as published by the Free Software Foundation (either version 2, as given
in file spm_LICENCE.man, or at your option, any later version). Further
details on \"copyleft\" can be found at <http://www.gnu.org/copyleft/>.
In particular, SPM is supplied as is. No formal support or maintenance
is provided or implied.

# SPM Version

## SPM12

SPM12 was released in October 2014 and represents a major update to the
SPM software, containing substantial theoretical, algorithmic,
structural and interface enhancements over previous versions:

- <http://www.fil.ion.ucl.ac.uk/spm/software/spm12/>

## Previous versions

Previous versions are still available for download, however you should
prefer the most recent one:

- SPM8: <http://www.fil.ion.ucl.ac.uk/spm/software/spm8/>
- SPM5: <http://www.fil.ion.ucl.ac.uk/spm/software/spm5/>
- SPM2: <http://www.fil.ion.ucl.ac.uk/spm/software/spm2/>
- SPM99: <http://www.fil.ion.ucl.ac.uk/spm/software/spm99/>
- SPM96: <http://www.fil.ion.ucl.ac.uk/spm/software/spm96/>

# Prerequisites

You need the following to run
[SPM](http://www.fil.ion.ucl.ac.uk/spm/software/) on your computer:

- [MATLAB](SPM/MATLAB "wikilink"): MATLAB is a high level numerical
  mathematics environment developed by
  [MathWorks](http://www.mathworks.com/), Inc. Natick, MA, USA. SPM
  requires only core MATLAB to run (**no special toolboxes are
  required**). See [this page](SPM/MATLAB "wikilink") for more details.
- SPM also uses external C programs, linked to MATLAB as C-mex files, to
  perform some of the more computationally intensive operations.
  Pre-compiled binaries are provided with the distribution for Linux
  (*\*.mexglx*, *\*.mexa64*), Windows (*\*.mexw32*, *\*.mexw64*) and Mac
  (*\*.mexmac*, *\*.mexmaci*, *\*.mexmaci64*). Hence, most of the time,
  SPM will work immediately using these binaries; in some instances, you
  might have to recompile them for your own architecture, details are
  provided [here](SPM#Installation "wikilink") for each platform.
- SPM12 uses the [NIFTI-1](http://nifti.nimh.nih.gov/) file format for
  the image data. All images are written as
  [NIFTI-1](http://nifti.nimh.nih.gov/), but it will also read the old
  Analyze format used by SPM2. Tools are provided to import data from
  [DICOM](http://medical.nema.org/dicom/2004.html),
  [MINC](http://www.bic.mni.mcgill.ca/software/minc/minc.html) and
  ECAT7. SPM12 also uses the
  [GIfTI](http://www.nitrc.org/projects/gifti/) file format for
  surface-based data.

# Download

SPM is freely available, but you will be asked to complete a
[registration form](http://www.fil.ion.ucl.ac.uk/spm/software/download/)
prior to downloading. Having completed the form, you will be directed to
the download location.

# Installation

- On Unix/Linux, use *unzip SPM.zip* or *tar xvfz SPM.tar.gz*
- On Windows, use [7-Zip](http://www.7-zip.org/) (free and powerful file
  archiver) or any other archive software that you might already have
  installed.
- On Mac, double-click to unpack the archive

Then launch MATLAB and add SPM to MATLAB\'s path:

`>> addpath C:/software/matlab/spm`

SPM is now ready to use:

`>> spm`

## Managing your MATLAB path

To ensure SPM is automatically on your MATLAB path in the future, you
can either use the command

`>> pathtool`

and save the path, or (better, if you have multiple MATLAB versions) you
can edit your [MATLAB startup
file](http://www.mathworks.co.uk/help/techdoc/ref/startup.html) to
include the above *addpath* command.

(Expert tip: since the startup file can be a general MATLAB
script/function you could consider more elaborate things like bringing
up a GUI window with a choice of SPM versions to add to the path.)

# Updates

SPM updates are made from time to time and advertised on the SPM
[mailing list](SPM/Learning_SPM "wikilink"). They can be downloaded from
the following addresses:

- <http://www.fil.ion.ucl.ac.uk/spm/download/spm12_updates/>
- <http://www.fil.ion.ucl.ac.uk/spm/download/spm8_updates/>
- <http://www.fil.ion.ucl.ac.uk/spm/download/spm5_updates/>
- <http://www.fil.ion.ucl.ac.uk/spm/download/spm2_updates/>

On Unix systems (Linux, Mac), use this command line syntax to install
the updates

`unzip -o spm12_updates_rxxxx.zip -d spm12`

On other platforms, just unpack the update archive over your SPM
installation so that newer files overwrite existing files.

# Toolboxes

A list of SPM toolboxes and extensions is available on the SPM website:
<http://www.fil.ion.ucl.ac.uk/spm/ext/>.

To install one of them, follow the instructions probably provided with
the package. It usually consists in simply copying the functions into a
subdirectory of the *toolbox* directory of your SPM installation.
