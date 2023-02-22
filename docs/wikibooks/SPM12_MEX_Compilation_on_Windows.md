# Compilation of SPM12 on Windows 10

This page describes the compilation of SPM12 MEX files on **Windows 10**
with **MATLAB R2018b (9.5)** and **MSYS2 MinGW-w64 GCC**.

If you use other versions of MATLAB and/or compiler, make sure that they
are compatible: [Supported
compilers](https://www.mathworks.com/support/sysreq/previous_releases.html)

## Installation of MSYS2/MinGW-w64

Download and install MSYS2 from:

[`https://www.msys2.org/`](https://www.msys2.org/)

in directory *C:\msys64*. Then, from MSYS2, type:

`pacman -Syu`  
`pacman -Su`  
`pacman -S --needed base-devel mingw-w64-x86_64-toolchain`

## Configuration of MATLAB compilation environment

Start MATLAB and type:

`>> setenv('MW_MINGW64_LOC', 'C:\msys64\mingw64')`  
`>> mex -setup`  
`MEX configured to use 'MinGW64 Compiler (C)' for C language compilation.`

Do not worry if there is a warning:

` Warning: The MATLAB C and Fortran API has changed to support MATLAB ...`

## Compilation

In MSYS2, move to the SPM12 source directory with

`$ cd  /c/Documents and Settings/`*`login`*`/Documents/MATLAB/spm12/src`

then set the PATH appropriately

`$ PATH=/c/Program\ Files/MATLAB/R2018b/bin/win64:${PATH}`  
`$ export MW_MINGW64_LOC=/c/msys64/mingw64/`  
`$ PATH=/c/msys64/mingw64/bin/:${PATH}`

and then type the following to start the compilation process

`$ make distclean`  
`$ make && make install`  
`$ make external-distclean`  
`$ make external && make external-install`

## Notes for compilation with MSYS2 for Octave

Install
[mingw-w64-octave](https://packages.msys2.org/base/mingw-w64-octave)

`$ pacman -S mingw64/mingw-w64-x86_64-octave`

then start Octave with:

`$ /mingw64/bin/octave --gui`

and compile with:

`$ cd('/c/Documents and Settings/<login>/Documents/MATLAB/spm12/src');`  
`$ system('make distclean PLATFORM=octave');`  
`$ system('make PLATFORM=octave && make install PLATFORM=octave');`  
`$ system('make external-distclean PLATFORM=octave');`  
`$ system('make external PLATFORM=octave && make external-install PLATFORM=octave');`
