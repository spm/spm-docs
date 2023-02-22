## SPM8, SPM12

There are no plans to provide SPM8 or SPM12 MEX files for Solaris SPARC
(32 or 64 bit) as these platforms are not supported by MathWorks any
more, see [MATLAB
Roadmap](http://www.mathworks.com/support/sysreq/roadmap.html).

## SPM5

Precompiled MEX files for Solaris SPARC (\*.mexsol and \*.mexs64) are
available in the latest update.

### Installation

Download
[spm5.zip](http://www.fil.ion.ucl.ac.uk/spm/software/download.html) in
your home directory then type the following in a Terminal:
\<pre\>\<nowiki\> cd /home/login unzip spm5.zip \</nowiki\>\</pre\>
Start MATLAB and add SPM into your path, either using *File \> Set Path
\> Add Folder\...* or typing \<pre\>\<nowiki\> addpath /home/login/spm5
\</nowiki\>\</pre\> in MATLAB\'s workspace.

### Compilation

In a Terminal, from the *src* folder of your SPM5 installation, type:
\<pre\>\<nowiki\> cd /home/login/spm5/src make distclean make && make
install \</nowiki\>\</pre\>

## SPM2

The following is a description of how to compile SPM2 to run nicely with
MATLAB 7 (service pack 1) on a SUN running Solaris 9 with the gcc
compiler installed. The standard Makefile that came with SPM2 didn\'t
quite work; partly because it was expecting you to use the cc compiler
that comes with Solaris.

#### Setting up compiling for MATLAB7

Copy the \"gccopts.sh\" file from \$MATLAB_HOME_DIR/bin to the SPM2
directory and call it \"mexopts.sh\". This enables you to easily enter
options for mex to use ONLY for compiling SPM2. Lower down you make the
changes to the Makefile to tell it to use this mexopts.sh file instead
of the default. Make sure this file sets variables like CC=\'gcc\'. You
may also need to edit the line for MLIBS to:

` MLIBS="-L/opt/sfw/gcc-3/lib -L/opt/sfw/gcc-3/lib/gcc-lib -L$TMW_ROOT/bin/$Arch -lg2c -lmx -lmex -lmat -lm"`

(everything between the quotes should be on one line) Note the extra
directories to search for libraries and the \"-lg2c\" option are new. To
be on the safe side also try adding:

` EXTRA_LIBDIR="/opt/sfw/gcc-3/lib"`

Also changing the optimization flags may boost performance. For an
UltraSPARCIII cpu you could try the following:

` COPTIMFLAGS='-O3 -funroll-loops -mcpu=v9 -DNDEBUG -DBIGENDIAN'`

Getting the linker set up properly needed some searching; try adding the
following in the mexopts.sh file

` LDFLAGS="-shared -Wl,-M,$TMW_ROOT/extern/lib/$Arch/$MAPFILE,-R,$GCC_LIBDIR,-R,$EXTRA_LIBDIR"`

#### Editing the Makefile

Try changing the Makefile from:

`SunOS:`  
`       make all SUF=mexsol  CC="cc -xO5" MEX="mex COPTIMFLAGS=-xO5"`

to:

`SunOS:`  
`       make all SUF=mexsol  CC="cc -xO5 -DBIGENDIAN" MEX="mex COPTIMFLAGS=-xO5 -DBIGENDIAN"`  
`SunOS.gcc:`  
`# Assumes that gccopts.sh has been copied to mexopts.sh in the current directory`  
`       make all SUF=mexsol  CC="gcc -O3 -funroll-loops -DBIGENDIAN -fPIC"\`  
`               MEX="mex -f ./mexopts.sh"`

Again you could also try adding \"-mcpu=v9\" as an option in the CC
variable for UltraSPARCIII cpu You may also need to edit the line

`all: verb.$(SUF) $(SPMMEX)`

to:

`all: verb.$(SUF) archive $(SPMMEX)`

As this recompiles the spm_vol_utils.a archive that may otherwise not be
compatible with your newly compiled mexsol routines.

and then compiling using:

`   make SunOS.gcc`

You may have to do:

`   make very_clean`

first to get this to work. Without doing this you may get a long list of
errors when linking.

In order to find out where things might be going wrong you can edit the
line above from:

`MEX="mex -f ./mexopts.sh"`

to:

`MEX="mex -n -f ./mexopts.sh"`

This will not compile the SPM routines but will tell you want commands
mex was going to issue to gcc.
