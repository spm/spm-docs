# Compilation of SPM on Windows

This page describes the compilation of SPM12 MEX files on **Windows 10**
with **MATLAB R2018b (9.5)** and **MSYS2 MinGW-w64 GCC**. It also provides instructions to compile SPM for the Octave package of MSYS2.

!!! tip
    If you use other versions of MATLAB and/or compiler, make sure that they are compatible by checking the list of [supported compilers](https://www.mathworks.com/support/requirements/previous-releases.html) and adjust the MATLAB path accordingly in the commands below.

## Installation of MSYS2/MinGW-w64

Download and install MSYS2 from [`https://www.msys2.org/`](https://www.msys2.org/) in directory `C:\msys64`.

Then, from MSYS2, type:

```
pacman -Syu
pacman -Su  
pacman -S --needed base-devel mingw-w64-x86_64-toolchain
```

## Configuration of MATLAB compilation environment

Start MATLAB and type:

```matlab
setenv('MW_MINGW64_LOC', 'C:\msys64\mingw64')
mex -setup
% MEX configured to use 'MinGW64 Compiler (C)' for C language compilation.
```

Do not worry if there is a warning:

```
Warning: The MATLAB C and Fortran API has changed to support MATLAB
```

## Compilation

In MSYS2, move to the SPM12 source directory with

```
cd  /c/Documents and Settings/`*`login`*`/Documents/MATLAB/spm12/src
```

then set the PATH appropriately

```bash
PATH=/c/Program\ Files/MATLAB/R2018b/bin/win64:${PATH} 
export MW_MINGW64_LOC=/c/msys64/mingw64/
PATH=/c/msys64/mingw64/bin/:${PATH}
```

and finally type the following to start the compilation process

```bash
make distclean
make && make install
make external-distclean
make external && make external-install
```

## Notes for compilation with MSYS2 for Octave

Install
[mingw-w64-octave](https://packages.msys2.org/base/mingw-w64-octave)

```bash
pacman -S mingw64/mingw-w64-x86_64-octave
```

then start Octave with:

```bash
/mingw64/bin/octave --gui
```

and compile with:

```bash
cd('/c/Documents and Settings/<login>/Documents/MATLAB/spm12/src');
system('make distclean PLATFORM=octave');
system('make PLATFORM=octave && make install PLATFORM=octave');
system('make external-distclean PLATFORM=octave');
system('make external PLATFORM=octave && make external-install PLATFORM=octave');
```
