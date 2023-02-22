# Compilation of SPM5 on Windows XP

SPM MEX-files can be compiled using [MinGW](http://www.mingw.org/)
(tools that allow you to compile C code) and
[MSYS](http://www.mingw.org/wiki/msys) (a minimal shell system that
allows you to use Makefile). The compiled MEX files come already shipped
with the SPM5 package but you might need to recompile them to have them
running faster (tuned for your exact platform) or because you developed
your own routine relying on the core SPM5 functions.

## Installation of MSYS

First, download the most recent MSYS package (*MSYS-1.0.10.exe* at time
of writing) and install it in *\<install_dir\>\msys\\*.

If you get an error saying *error creating INI entry file
c:\windows\MSYS.INI*, you can safely ignore it. You can also answer *n*
when asked for a post-installation.

## Installation of MinGW

Second, download the most recent MinGW package (*MinGW-4.1.10.exe* at
time of writing) and install it in *\<install_dir\>\msys\mingw\\* (this
directory should already have be automatically created when installing
MSYS). Choose to do a full installation with selecting:

- current/runtime
- w32api
- binutils
- gcc-core
- ming32-make

You could have as well chosen to download these packages separately and
uncompressing them in the same directory (the package names are
*mingw-runtime*, *w32api*, *binutils*, *gcc-core*, *mingw32-make*).

## Installation of GnuMEX

[GnuMEX](http://gnumex.sourceforge.net/) need also to be installed. Just
specify that you want to compile using MinGW (MinGW linking).

## Compilation

You can then launch MSYS by double-clicking on
*\<install_dir\>\msys\msys.bat*. Move to your SPM5 directory with

`% cd /c/spm/src`

and then type make to start the compilation process

`% make`

This will create several MEX files (*\*.dll* or *\*.mexw32*) that are
copied to their right location in SPM using the command

`% make install`

You can then remove all the files that have been created during the
compilation with

`% make distclean`
