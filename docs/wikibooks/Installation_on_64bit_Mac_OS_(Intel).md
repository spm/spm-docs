## Preamble

If you use drag-and-drop in *Finder* to install the updates, it will
actually perform a folder copy and not a folder merge so that updated
files will be overwritten but unchanged old files will be deleted (see
[this
thread](http://macosx.com/forums/mac-os-x-system-mac-software/303820-copy-folder-deletes-original-folder-contents-then-copies-new-ones.html)).
Using the command line option as described below will overcome this.

## SPM12

Mac Intel with 64bit MATLAB is a supported SPM12 platform. Precompiled
MEX files (\*.mexmaci64) are included in the SPM distribution.

### Installation

Download
[spm12.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updates
[spm12_updates_rxxxx.zip](http://www.fil.ion.ucl.ac.uk/spm/download/spm12_updates/)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /Users/login unzip spm12.zip unzip -o
spm12_updates_rxxxx.zip -d spm12 \</nowiki\>\</pre\> Start MATLAB and
add SPM into your path, either using *File \> Set Path \> Add
Folder\...* or typing \<pre\>\<nowiki\> addpath /Users/login/spm12
savepath \</nowiki\>\</pre\> in MATLAB\'s workspace.

### Compilation

`If you are using macOS Catalina, Big Sur, Monterey or Ventura, try this first: `[`fix for MEX files on recent macOS`](SPM/Installation_on_64bit_Mac_OS_(Intel)#macOS_Catalina,_Big_Sur,_Monterey,_Ventura "wikilink")` as you shouldn't need to recompile the MEX files.`

Should you want to compile SPM MEX files, you need to have Apple\'s
development environment [Xcode](http://developer.apple.com/tools/xcode/)
installed.

You also need to have the *mex* executable in your system path. To do
so, type the following in a Terminal: \<pre\>\<nowiki\> export
PATH=/Applications/MATLAB_R2017a.app/bin:\$PATH \</nowiki\>\</pre\> with
the appropriate path where MATLAB is installed

Then, in a Terminal, from the *src* folder of your SPM12 installation,
type: \<pre\>\<nowiki\> cd /Users/login/spm12/src make distclean make &&
make install make external-distclean make external && make
external-install \</nowiki\>\</pre\>

If you get errors such as *Bad : modifier in \$ (/)*, this is because
the instructions are given for a *bash* Terminal while you are using a
*tcsh* Terminal. The equivalent commands are: \<pre\>\<nowiki\> setenv
PATH /Applications/MATLAB_R2017a.app/bin:\${PATH} \</nowiki\>\</pre\>

If you get errors such as *xcrun: error: SDK \"macosx10.14.1\" cannot be
located* while compiling, execute the following: \<pre\>\<nowiki\> sudo
xcode-select \--switch /Applications/Xcode.app/ \</nowiki\>\</pre\>

## SPM8

Mac Intel with 64bit MATLAB (available since R2009b) is a supported SPM8
platform. Precompiled MEX files (\*.mexmaci64) are included in the SPM
distribution.

### Installation

Download
[spm8.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updates
[spm8_updates_rxxxx.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm8_updates/)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /Users/login unzip spm8.zip unzip -o
spm8_updates_rxxxx.zip -d spm8 \</nowiki\>\</pre\> Start MATLAB and add
SPM into your path, either using *File \> Set Path \> Add Folder\...* or
typing \<pre\>\<nowiki\> addpath /Users/login/spm8 \</nowiki\>\</pre\>
in MATLAB\'s workspace.

### Compilation

Should you want to compile SPM MEX files, you need to have Apple\'s
development environment [Xcode](http://developer.apple.com/tools/xcode/)
installed. It should be available on your Mac OS X installation DVD.

You also need to have the *mex* executable in your system path. To do
so, type the following in a Terminal: \<pre\>\<nowiki\> export
PATH=/Applications/MATLAB_R2009a.app/bin:\$PATH \</nowiki\>\</pre\> with
the appropriate path where MATLAB is installed

Then, in a Terminal, from the *src* folder of your SPM8 installation,
type: \<pre\>\<nowiki\> cd /Users/login/spm8/src export MACI64=1 make
distclean make && make install make toolbox-distclean make toolbox &&
make toolbox-install make external-distclean make external && make
external-install \</nowiki\>\</pre\>

If you get errors such as *Bad : modifier in \$ (/)*, this is because
the instructions are given for a *bash* Terminal while you are using a
*tcsh* Terminal. The equivalent commands are: \<pre\>\<nowiki\> setenv
PATH /Applications/MATLAB_R2009a.app/bin:\${PATH} setenv MACI64 1
\</nowiki\>\</pre\>

## SPM5

Precompiled MEX files for Mac Intel with 64bit MATLAB (\*.mexmaci64) are
available in the latest update.

### Installation

Download
[spm5.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) in
your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /Users/login unzip spm5.zip \</nowiki\>\</pre\>
Start MATLAB and add SPM into your path, either using *File \> Set Path
\> Add Folder\...* or typing \<pre\>\<nowiki\> addpath /Users/login/spm5
\</nowiki\>\</pre\> in MATLAB\'s workspace.

### Compilation

If you want to compile SPM5 MEX files by yourself, you need to have
Xcode installed and *mex* in your system path (see SPM8 for details).

Then, in a Terminal, from the *src* folder of your SPM5 installation,
type: \<pre\>\<nowiki\> cd /Users/login/spm5/src export MACI64=1 make
distclean make && make install \</nowiki\>\</pre\>

## SPM2

There are no plans to provide SPM2 64bit MEX files for Mac Intel.

## Troubleshooting

### macOS Catalina, Big Sur, Monterey, Ventura

If you have issues with MEX files on macOS (\"*\"\*.mexmaci64\" cannot
be opened because the developer cannot be verified. macOS cannot verify
that this app is free from malware*\" or \"*Code signature not valid for
use in process using Library Validation: library load disallowed by
system policy*\"), open a Terminal, *cd* to the SPM directory and type:

`find . -name "*.mexmaci64" -exec xattr -d com.apple.quarantine {} \;`

If It doesn\'t work, please try this equivalent alternative, replacing
SPM_PATH with the path of your SPM installation:

`sudo xattr -r -d com.apple.quarantine SPM_PATH`  
`sudo find SPM_PATH -name "*.mexmaci64" -exec spctl --add {} \;`

### Java Exception and abrupt exit on Mac OS X version 10.10 Yosemite

If you get MATLAB issues with Yosemite, see the following bug report for
a patch:

[`http://www.mathworks.com/support/bugreports/1098655`](http://www.mathworks.com/support/bugreports/1098655)

### \"This is pdfTeX, Version \...\" error

If you get an error message such as:

`mex -O -c spm_vol_utils.c -DSPM_UNSIGNED_CHAR `  
`` mex: unrecognized option `-O' ``  
`` mex: unrecognized option `-c' ``  
`This is pdfTeX, Version 3.1415926-2.3-1.40.12 (TeX Live 2011)`  
`restricted \write18 enabled.`  
`entering extended mode`  
`(./spm_vol_utils.c`  
`This is MeX  Version 1.05  18 XII 1993  (B. Jackowski & M. Ry\'cko)`  
`` ! You can't use `macro parameter character #' in vertical mode. ``

this is due to a conflict between MATLAB *mex* and a LaTeX command with
the same name. Edit *src/Makefile.var* and mention the full path when
referring to MEXBIN

`MEXBIN = /Applications/MATLAB_R2012a.app/bin/mex`

or change PATH accordingly.

### Slowdown when using graphical inputs

This can be due to a window manager for Mac called
[Magnet](https://magnet.crowdcafe.com/), see
[this](https://www.mathworks.com/matlabcentral/answers/475682).
