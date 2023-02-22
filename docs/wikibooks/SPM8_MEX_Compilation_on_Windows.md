# Compilation of SPM8 on Windows 7

This page describes the compilation of SPM8 MEX files on **Windows 7 SP1
32bit** with **MATLAB R2008a (7.6)** and **Microsoft Visual C++ 2005
(8.0) Express Edition**.

If you use other versions of MATLAB and/or compiler, make sure that they
are compatible: [Supported
compilers](http://www.mathworks.co.uk/support/sysreq/previous_releases.html)

## Installation of Microsoft Visual C++

Download and install Microsoft Visual C++ 2005 (8.0) Express Edition.

Download and install Microsoft Windows Server 2003 SP1 Platform SDK.

An environment variable called *MSSDk* has to be specified if absent:

- Go to *Control Panel*
- Go to *System*
- Go to *Advanced system settings*
- Go to *Advanced*
- Go to *Environment Variables\...*
- If a variable called *MSSDk* does not appear in *User variables for
  systems*:
  - Click on *New\...*
  - And enter:
    - *Variable name:* MSSDk
    - *Variable value:* C:\Program Files\Microsoft Platform SDK

## Installation of MinGW/MSYS

Download and install MinGW from:

[`http://www.mingw.org/wiki/Getting_Started#toc1`](http://www.mingw.org/wiki/Getting_Started#toc1)

in directory *C:\MinGW*, selecting component *MinGW Developer Toolkit
(including MSYS)* only.

## Configuration of MATLAB compilation environment

Start MATLAB and type:

`>> mex -setup`

and follow the instructions:

`Please choose your compiler for building external interface (MEX) files: `  
  
`Would you like mex to locate installed compilers [y]/n? `**`y`**  
  
`Select a compiler: `  
`[1] Lcc-win32 C 2.4.1 in C:\PROGRA~1\MATLAB\R2008a\sys\lcc\bin `  
`[2] Microsoft Visual C++ 2005 Express Edition in C:\Program Files\Microsoft Visual Studio 8 `  
  
`[0] None `  
  
`Compiler: `**`2`**  
  
`Please verify your choices: `  
  
`Compiler: Microsoft Visual C++ 2005 Express Edition  `  
`Location: C:\Program Files\Microsoft Visual Studio 8 `  
  
`Are these correct [y]/n? `**`y`**

If there is an error message:

`Error: The Microsoft Platform Software Development Kit (SDK) was not found. `

it means that the environment variable MSSDk was not set properly: check
again the installation of the compiler above.

Do not worry if there is a warning:

` Warning: The MATLAB C and Fortran API has changed to support MATLAB ...`

## Compilation

From the Start menu, choose:

- All Programs
  - Visual C++ 2005 Express Edition
    - Visual Studio Tools
      - Visual Studio 2005 Command Prompt

then launch MSYS from there:

`C:\MinGW\msys\1.0\msys.bat`

Move to the SPM8 source directory with

`$ cd /c/Users/`*`login`*`/Documents/MATLAB/spm8/src`

and then type the following to start the compilation process

`$ make distclean`  
`$ make && make install`  
`$ make toolbox-distclean`  
`$ make toolbox && make toolbox-install`  
`$ make external-distclean`  
`$ make external && make external-install`
