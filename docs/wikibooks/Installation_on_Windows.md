## Preamble

Installing SPM on Windows 32 bit (XP, Vista, 7) should be relatively
straightforward. Common pitfalls can be found in the description of the
[64bit Windows
installation](SPM/Installation_on_64bit_Windows#Preamble "wikilink").

## SPM12

### Installation

- Download
  [spm12.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html).
- Unzip spm12.zip in a folder of your choice, such as
  C:\Users\\*login*\Documents\MATLAB\spm12).
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\Users\`*`login`*`\Documents\MATLAB\spm12`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

You might have to install the VC++ 2005 and 2008 Redistributable
Packages (*vcredist_x86.exe*) from Microsoft:

- <http://support.microsoft.com/kb/2019667>

### Update

If you have just downloaded the spm12.zip archive, it already contains
the latest set of updates. To update SPM when a new version is released:

- Download
  [spm12_updates_rxxxx.zip](http://www.fil.ion.ucl.ac.uk/spm/download/spm12_updates/)
- Unzip spm12_updates_rxxxx.zip on top of the folder containing your SPM
  installation so that newer files overwrite existing files.

Alternatively, you can use the spm_update.m function:

`>> spm_update`

If a new version is available, it can be applied to your local
installation by typing:

`>> spm_update update`

### Compilation

Precompiled MEX files (\*.mexw32) are provided with SPM12 and you
shouldn\'t need to recompile them by yourself.

## SPM8

### Installation

- Download
  [spm8.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html)
  and its updates
  [spm8_updates_rxxxx.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm8_updates/)
- Unzip spm8.zip in a folder of your choice, such as C:\spm\spm8, and
  unzip spm8_updates_rxxxx.zip on top of it so that newer files
  overwrite existing files.
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\spm\spm8`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

### Compilation

Precompiled MEX files (\*.mexw32) are provided with SPM8 and you
shouldn\'t need to recompile them by yourself.

See [Compilation of SPM8 MEX files on
Windows](SPM/SPM8_MEX_Compilation_on_Windows "wikilink") if needed.

## SPM5

### Installation

- Download
  [spm5.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html).
- Unzip spm5.zip in a folder of your choice, such as C:\spm\spm5.
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\spm\spm5`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

### Compilation

Precompiled MEX files (\*.mexw32) are provided with SPM5 and you
shouldn\'t need to recompile them by yourself.

See [Compilation of SPM5 MEX files on
Windows](SPM/SPM5_MEX_Compilation_on_Windows "wikilink") if needed.

## SPM2

### Installation

- Download
  [spm2.tar.gz](http://www.fil.ion.ucl.ac.uk/spm/software/download.html)
  and its updated MEX files
  [SPM2_R2007a_XP_MEX.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm2_updates/SPM2_R2007a_XP_MEX.zip)
- Unzip spm2.tar.gz in a folder of your choice, such as C:\spm\spm2, and
  unzip SPM2_R2007a_XP_MEX.zip on top of it so that newer files
  overwrite existing files.
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\spm\spm2`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

### Compilation

Precompiled MEX files (\*.mexw32) are provided with SPM2 and you
shouldn\'t need to recompile them by yourself.

See [Compilation of SPM2 MEX files on
Windows](SPM/SPM2_MEX_Compilation_on_Windows "wikilink").
