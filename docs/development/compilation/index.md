# Compilation of SPM MEX files

!!! info "What are MEX files?"

    MEX files are MATLAB functions implemented in C/C++. Contrary to functions implemented in pure MATLAB, they need to be compiled in order for the MATLAB interpreter to be able to call them. Once compiled, they correspond to platform-specific files `*.mex*`.

## Instructions

Pre-compiled MEX files are provided with the SPM distribution so you shouldn't need to compile the MEX files yourself. We still provide here platform-specific instructions to compile all the MEX files for your machine if the shipped ones were incompatible with your OS version.

* [Windows](windows.md)
* [Linux](linux.md)
* [macOS](macos.md)

## Troubleshooting

Platform-specific throubleshooting is available with the links above. We list here error messages and solutions that are common across platforms. 

!!! failure "`bad image handle dimensions` error"

    The error "`bad image handle dimensions`" when trying to display an image is an indication that the MEX files are not compatible with your platform and should be recompiled.
