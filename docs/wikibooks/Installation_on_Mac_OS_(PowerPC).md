## SPM12

SPM12 is not supported on Mac PowerPC.

## SPM8

SPM8 is not officially supported on Mac PowerPC, as this platform is
about to be phased out (see [MATLAB Platform
Roadmap](http://www.mathworks.com/support/sysreq/roadmap.html)). This
means that precompiled MEX files (\*.mexmac) are not included in the SPM
distribution. It should however be relatively straightforward to compile
them by yourself.

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

You need to have Apple\'s development environment
[Xcode](http://developer.apple.com/tools/xcode/) installed. It should be
available on your Mac OS X installation DVD.

You also need to have the *mex* executable in your system path. To do
so, type the following in a Terminal: \<pre\>\<nowiki\> export
PATH=\$PATH:/Applications/MATLAB/bin \</nowiki\>\</pre\> with the
appropriate path where MATLAB is installed.

In a Terminal, from the *src* folder of your SPM8 installation, type:
\<pre\>\<nowiki\> cd /Users/login/spm8/src make distclean make && make
install make toolbox-distclean make toolbox && make toolbox-install make
external-distclean make external && make external-install
\</nowiki\>\</pre\>

## SPM5

Precompiled MEX files for Mac PowerPC (\*.mexmac) are available in the
latest update.

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
type: \<pre\>\<nowiki\> cd /Users/login/spm5/src make distclean make &&
make install \</nowiki\>\</pre\>

## SPM2

Precompiled MEX files for Mac PowerPC (\*.mexmac) are available in the
latest update.

### Compilation

NB. Some of the following commands require that you log in as root or
use the \"sudo\" mode.

Assuming that your Matlab and SPM2 folders (say, \"MATLAB\" and
\"spm2\") are in your Applications folder:

\(1\) make sure you use a version of gcc earlier than 3.3 (compiling
spm2 with gcc3.3 fails when mex is invoked; there apparently is a
conflict of library):

`   (sudo) gcc_select 3`

\(2\) put a symbolic link to the \"mex\" file found in MATLAB/bin into
/usr/sbin/ (!!! not in usr/local/bin):

`   (sudo) ln -s /.../Applications/MATLAB/bin/mex   /usr/sbin/mex`

\(3\) recompile SPM2:

`   cd /.../Applications/spm2`  
`   make MAC`

You should see something like the following lines:

`   make all SUF=mexmac RANLIB="ranlib spm_vol_utils.mexmac.a"`  
`   _________________________________________________`  
`   Unix compile for MacOS X`  
`   _________________________________________________`  
`   mex -O -c spm_mapping.c`  
`   mv spm_mapping.o spm_mapping.mexmac.o`  
`   rm -f spm_vol_utils.mexmac.a`  
`   ar rcv spm_vol_utils.mexmac.a utils_uchar.mexmac.o utils_short.mexmac.o`  
`   utils_int.mexmac.o utils_schar.mexmac.o utils_ushort.mexmac.o `  
`   utils_uint.mexmac.o utils_float.mexmac.o utils_double.mexmac.o `  
`   utils_short_s.mexmac.o utils_int_s.mexmac.o utils_ushort_s.mexmac.o`  
`   utils_uint_s.mexmac.o utils_float_s.mexmac.o utils_double_s.mexmac.o `  
`   spm_make_lookup.mexmac.o spm_getdata.mexmac.o spm_vol_access.mexmac.o `  
`   spm_mapping.mexmac.o `  
`   a - utils_uchar.mexmac.o`  
`   a - utils_short.mexmac.o`  
`   ... (a long long list of files)`  
`   _________________________________________________`  
`   FINISHED`  
`   _________________________________________________`
