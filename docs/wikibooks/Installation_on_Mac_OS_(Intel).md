## Preamble

If you use drag-and-drop in *Finder* to install the updates, it will
actually perform a folder copy and not a folder merge so that updated
files will be overwritten but unchanged old files will be deleted (see
[this
thread](http://macosx.com/forums/mac-os-x-system-mac-software/303820-copy-folder-deletes-original-folder-contents-then-copies-new-ones.html)).
Using the command line option as described below will overcome this.

## SPM12

SPM12 is not officially supported on Mac Intel with 32 bit MATLAB, as
this platform is about to be phased out (see [MATLAB Platform
Roadmap](http://www.mathworks.com/support/sysreq/roadmap.html)). This
means that precompiled MEX files (\*.mexmaci) are not included in the
SPM distribution.

You should be able to compile the MEX files yourself provided you edit
spm12/src/Makefile.var so that it uses mexmaci instead of mexmaci64 in
the MacOS section.

## SPM8

Mac Intel with 32 bit MATLAB is a supported SPM8 platform. Precompiled
MEX files (\*.mexmaci) are included in the SPM distribution.

### Installation

Download
[spm8.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updates
[spm8_updates_rxxxx.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm8_updates/)
in your home directory then type the following in a Terminal:

`cd /Users/login`  
`unzip spm8.zip`  
`unzip -o spm8_updates_rxxxx.zip -d spm8`

Start MATLAB and add SPM into your path, either using *File \> Set Path
\> Add Folder\...* or typing

`addpath /Users/login/spm8`

in MATLAB\'s workspace.

### Compilation

Should you want to compile SPM MEX files (this can currently happen if
you are using Mac OS X 10.4 (Tiger)), there are two requirements:

- you need to have Apple\'s development environment
  [Xcode](http://developer.apple.com/tools/xcode/) installed. It should
  be available on the \"Mac OS X Install\" DVD that came with your Mac.
  Navigate to \"*Optional Installs*\" and then to \"*Xcode Tools*\" and
  double click the \"*Xcode Tools*\" package to install. You will know
  that you need to install this tool if *make* is a *command not found*
  later on.
- You also need to have the *mex* executable in your system path. To do
  so, type the following in a Terminal:

`export PATH=$PATH:/Applications/MATLAB74/bin`

with the appropriate path where MATLAB is installed. The exact syntax
might be different if you are using another shell.

Then, in a Terminal, from the *src* folder of your SPM8 installation,
type:

`cd /Users/login/spm8/src`  
`make distclean`  
`make && make install`  
`make toolbox-distclean`  
`make toolbox && make toolbox-install`  
`make external-distclean`  
`make external && make external-install`

Note: when compiling with Mac OS X 10.5 and wanting to keep
compatibility with Mac OS X 10.4, it is advised to set the preprocessor
macro:

`export MACOSX_DEPLOYMENT_TARGET=10.4`

See [Apple Development:Symbol
Variants](http://developer.apple.com/releasenotes/Darwin/SymbolVariantsRelNotes/index.html)

## SPM5

Precompiled MEX files for Mac Intel with 32bit MATLAB (\*.mexmaci) are
included in the SPM distribution.

### Installation

Download
[spm5.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) in
your home directory then type the following in a Terminal:

`cd /Users/login`  
`unzip spm5.zip`

Start MATLAB and add SPM into your path, either using *File \> Set Path
\> Add Folder\...* or typing

`addpath /Users/login/spm5`

in MATLAB\'s workspace.

### Compilation

If you want to compile SPM5 MEX files by yourself, you need to have
Xcode installed and *mex* in your system path (see SPM8 for details).

Then, in a Terminal, from the *src* folder of your SPM5 installation,
type:

`cd /Users/login/spm5/src`  
`make distclean`  
`make && make install`

## SPM2

### Installation

Follow the default installation for a UNIX system.

Precompiled MEX files for Mac Intel with 32bit MATLAB (\*.mexmaci) are
available as an extra package:

- [<ftp://ftp.fil.ion.ucl.ac.uk/spm/spm2_updates/SPM2_MAC_INTEL_MEX.zip>](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm2_updates/SPM2_MAC_INTEL_MEX.zip)

To run SPM2 on an Intel Mac, you will need to make a few changes to the
**spm_platform.m** file. These are relatively straightforward and have
to do with Intel Macs identifying themselves as MACI (instead of MAC) as
well as the big vs. little Endian issue.

First, add MACI to the list of platform definitions. In the
spm_platform.m file you will see a list that looks like this:

\<pre\> \<nowiki\> PDefs = { \'PCWIN\', \'win\', 0;\...

`       'PCWIN64',  'win',  0;...`  
`       'MAC',      'unx',  1;...`  
`       'SUN4',     'unx',  1;...`  
`       'SOL2',     'unx',  1;...`  
`       'HP700',    'unx',  1;...`  
`       'SGI',      'unx',  1;...`  
`       'SGI64',    'unx',  1;...`  
`       'IBM_RS',   'unx',  1;...`  
`       'ALPHA',    'unx',  0;...`  
`       'AXP_VMSG', 'vms',  Inf;...`  
`       'AXP_VMSIEEE',  'vms',  0;...`  
`       'LNX86',    'unx',  0;...`  
`       'GLNX86',   'unx',  0;...`  
`       'GLNXA64',      'unx',  0;...`  
`       'VAX_VMSG', 'vms',  Inf;...`  
`       'VAX_VMSD', 'vms',  Inf };`

\</nowiki\> \</pre\>

Simply add a line like this:

\<pre\>

`       'MACI',     'unx',  0;...`

\</pre\>

Finally, at the bottom of the spm_platform.m file you need to add MACI
in the list of supported platforms:

\<pre\> case
{\'SUN4\',\'SOL2\',\'HP700\',\'SGI\',\'SGI64\',\'IBM_RS\',\'ALPHA\',\'LNX86\',\'GLNX86\',\'GLNXA64\',\'MAC\',
\'MACI\'} \</pre\>

### Compilation

If you want to compile the MEX files by yourself, download an updated
Makefile and spm_platforms.m file from:

- [<http://web.me.com/markusnowak/SPM2_on_Intel_Macs>](http://web.me.com/markusnowak/SPM2_on_Intel_Macs)

and follow the instructions given there (they have been updated for Snow
Leopard).
