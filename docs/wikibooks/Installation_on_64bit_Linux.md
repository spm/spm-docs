All versions of SPM are supported under Linux 64 bit and the
installation should be fairly the same with any version. You might have
to recompile SPM MEX files (\*.mexa64) if those provided with the SPM
distribution do not appear to be compatible with your system.

## SPM12

### Installation

Download
[spm12.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updates
[spm12_updates_rxxxx.zip](http://www.fil.ion.ucl.ac.uk/spm/download/spm12_updates/)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login unzip spm12.zip unzip -o
spm12_updates_rxxxx.zip -d spm12 \</nowiki\>\</pre\> Start MATLAB and
add SPM into your path, either using *File \> Set Path \> Add
Folder\...* or typing \<pre\>\<nowiki\> addpath /home/login/spm12
\</nowiki\>\</pre\> in MATLAB\'s workspace.

### Compilation

In a Terminal, from the *src* folder of your SPM12 installation, type:
\<pre\>\<nowiki\> cd /home/login/spm12/src make distclean make && make
install make external-distclean make external && make external-install
\</nowiki\>\</pre\>

## SPM8

### Installation

Download
[spm8.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updates
[spm8_updates_rxxxx.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm8_updates/)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login unzip spm8.zip unzip -o
spm8_updates_rxxxx.zip -d spm8 \</nowiki\>\</pre\> Start MATLAB and add
SPM into your path, either using *File \> Set Path \> Add Folder\...* or
typing \<pre\>\<nowiki\> addpath /home/login/spm8 \</nowiki\>\</pre\> in
MATLAB\'s workspace.

### Compilation

In a Terminal, from the *src* folder of your SPM8 installation, type:
\<pre\>\<nowiki\> cd /home/login/spm8/src make distclean make && make
install make toolbox-distclean make toolbox && make toolbox-install make
external-distclean make external && make external-install
\</nowiki\>\</pre\>

Required development packages: make, gcc.

If you get an error message regarding \"ft_getopt\" then please download
a [maintenance version of
SPM8](https://github.com/spm/spm8/archive/maint.zip) and skip the
compilation steps above entirely. All the necessary files including mex
files for windows, linux and mac should be already compiled. This
distribution gives compile time error for nanmean.c though. (ref:
<https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=spm;9d67787a.1802>)

## SPM5

### Installation

Download
[spm5.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) and
its updated MEX files
[SPM5_Matlab7.9_LINUX64_MEX.zip](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm5_updates/SPM5_Matlab7.9_LINUX64_MEX.zip)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login unzip spm5.zip unzip -o
SPM5_Matlab7.9_LINUX64_MEX.zip -d spm5 \</nowiki\>\</pre\> Start MATLAB
and add SPM into your path, either using *File \> Set Path \> Add
Folder\...* or typing \<pre\>\<nowiki\> addpath /home/login/spm5
\</nowiki\>\</pre\> in MATLAB\'s workspace.

### Compilation

In a Terminal, from the *src* folder of your SPM5 installation, type:
\<pre\>\<nowiki\> cd /home/login/spm5/src make distclean make && make
install \</nowiki\>\</pre\>

If you receive the following error message: mex: link of \'
\"spm_bwlabel.mexa64\"\' failed, then edit spm_bwlabel.c at line 364
such that it reads:

`   plhs[1] = mxCreateDoubleScalar(nl);`

Ref: McGeown, William. \"Re: mex files SPM5 64bit,\" JISCMail. 13 Apr
2015. Last Accessed 18 Sep 2018.

## SPM2

### Installation

Download
[spm2.tar.gz](http://www.fil.ion.ucl.ac.uk/spm/software/download.html)
and its updated MEX files
[SPM2_LINUX64_MEX.tar.gz](ftp://ftp.fil.ion.ucl.ac.uk/spm/spm2_updates/SPM2_LINUX64_MEX.tar.gz)
in your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login tar xvfz spm2.tar.gz tar xvfz
SPM2_LINUX64_MEX.tar.gz -C spm2 \</nowiki\>\</pre\> Start MATLAB and add
SPM into your path, either using *File \> Set Path \> Add Folder\...* or
typing \<pre\>\<nowiki\> addpath /Users/login/spm2 \</nowiki\>\</pre\>
in MATLAB\'s workspace.

### Compilation

In a Terminal, type: \<pre\>\<nowiki\> cd /home/login/spm2 make
clean.Linux.A64 make Linux.A64 \</nowiki\>\</pre\>

## Troubleshooting

### \"bad image handle dimensions\" error

The error \"bad image handle dimensions\" when trying to display an
image is an indication that you need to (re)compile the SPM MEX files.

### \"cannot find -lstdc++\" error

If you get the error message \"cannot find -lstdc++\", try installing
the *build-essential* package using your package manager (i.e. *sudo
apt-get install build-essential* on Ubuntu), or the specific
*libstdc++5* package (replacing *5* with the version of your system)
with *sudo apt-get install libstdc++5*.

If, after the install of libstdc++5, the lstdc++ error persists, just
add a symbolic link with:

`sudo ln -s /usr/lib64/libstdc++.so.6 /usr/lib64/libstdc++.so `

and it should work. If not, see next.

For old versions of Ubuntu and MATLAB, have a look at [these
instructions](https://web.archive.org/web/20160307232613/http://www.lukedodd.com/compiling-gcc-4-3-4-under-ubuntu-11-10-for-matlab/).

### Other problems with lstdc++

If you get the above error and cannot install libstdc++ (e.g. you do not
have root/admin access), or if you get other problems such as \"\...
version \`GCC_4.2.0\' not found (required by
/usr/lib64/libstdc++.so.6\", then another option is to edit your
mexopts.sh config file and delete or disable the -lstdc++ compiler
flags, for example simply use find-and-replace to delete all instances
of *-lstdc++*. SPM will compile properly without this.

It\'s best to edit a local copy of mexopts.sh (the original is found in
the bin directory with your matlab and mex executables). If you run
\<code\>mex -setup\</code\> it will interactively create a copy for you,
under \<code\>~/.matlab/RELEASE\</code\> for example

`~/.matlab/R14SP3/mexopts.sh`  
`~/.matlab/R2008b/mexopts.sh`

or similar, and you may then further edit that copy if required. If you
match the release, mex will find your copy of mexopts.sh. Note that from
MATLAB

`version`

should show you the release, but with parentheses around it, e.g.
\"(R14SP3)\".

### \"mex: command not found\" error

If you get the error \"mex: command not found\" check that mex is in
your path. Either add the MATLAB binary directory (usually
/usr/local/matlab/bin) to your path or create a link to mex somewhere
already in the path (usually /usr/local/bin).

- Adding the path:
  - Type: \<code\>PATH=/usr/local/matlab/bin:\$PATH\</code\>
- Creating a Link:
  - Enter your local binary directory: \<code\>cd
    /usr/local/bin\</code\>
  - Create the link: \<code\>ln -s /usr/local/matlab/bin/mex\</code\>
  - (Note: Creating a link in /usr/local/bin requires root privileges).

Another solution is to edit the Makefile
(\<code\>spm8/src/Makefile.var\</code\> for SPM8 or
\<code\>spm5/src/Makefile\</code\> for SPM5) and mention the full path
when referring to MEXBIN:

`MEXBIN = /usr/local/matlab/mex`

### \"This is pdfTeX, Version \... (TeX Live 2011)\" error

If you get the error:

`` mex: unrecognized option `-O' ``  
`` mex: unrecognized option `-c' ``  
`This is pdfTeX, Version ... (TeX Live 2011)`

This is due to a conflict between MATLAB mex and a LaTeX command (from
the texlive package) with the same name. Change PATH or MEXBIN as
described in the previous solution.

### Missing files

If you get build errors such as \<code\>math.h\</code\> does not exist
when trying to execute the command \"make && make install\", then you
probably need to install the build-essentials package. At the command
line type: \"sudo aptitude install build-essential\" (for Ubuntu). if
you have sudo privileges or simply log in as root or obtain superuser
status and type: \"aptitude install build-essential\".

### \"plugin needed to handle lto object\" error

Edit \<code\>spm12/src/Makefile.var\</code\> at the line defining AR so
that it reads:

`AR           = gcc-ar rcs`

### Crash at startup

The following concerns a situation where MATLAB segfaults when opening
the SPM interface with errors like \"\'BadWindow (invalid Window
parameter)\", \"serial 20133 error_code 3 request_code 20 minor_code
0\", \"Pango-CRITICAL \*\*: pango_font_description_from_string:
assertion \'str != NULL\' failed\", \"GLib-CRITICAL \*\*:
g_once_init_leave: assertion \'result != 0\' failed\",
\"GLib-GObject-CRITICAL \*\*: g_type_register_dynamic: assertion
\'parent_type \> 0\' failed\"

This is likely to be an issue with the display of the welcoming message
in the Graphics window as a web document.

One fix is to comment one line in \<code\>spm.m\</code\>:
<https://github.com/spm/spm12/blob/r7771/spm.m#L352>

Another fix not requiring to modify the SPM source code is to define an
environment variable, see:
<https://github.com/spm/spm12/blob/r7771/spm_browser.m#L54>

`setenv('SPM_HTML_BROWSER','0')`

## Compilation notes for SPM2:

### Updates for *Makefile*

In order to make the recompilation process work, the **Makefile**
provided in the **spm2.tar.gz** package needs to be patched. Also the
file **spm_platform.m** needs an update.

The updated **Makefile** adds a new architecture, **Linux.a64**:

`Linux.a64:<br>`  
`  make all SUF=mexa64  CC="gcc -O3 -funroll-loops -fPIC -march=x86-64" \<br>`  
`  MEX="mex COPTIMFLAGS='-O3 -funroll-loops -fPIC -march=x86-64'"`

Plus an appropriate section for **clean** and **verb.mexa64**.

The **-fPIC** option is necessary to allow linking on 64bit Linux.\<br\>
**-march=x86-64** provides generic optimisations for both **Opteron**
and **64bit Xeon**. If the code is running on a **64bit Xeon** you can
change it to **-march=nocona** (the first revision of the Xeon processor
supporting EM64T had codename **nocona**), if you have an **Opteron** or
**Athlon64**, change it to **-march=opteron**. This might give you an
extra performance boost.

### Updates for *spm_platform.m*

Matlab for 64bit Linux identifies itself as **GLNXA64** as opposed to
**GLNX86** when running 32bit Matlab. *Endian-ness* doesn\'t change, so
I added this to the **PDefs** cell list:

`                'GLNXA64',      'unx',  0;...`

and added **GLNXA64** to the list of other unix platforms at the font
configuration.
