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

## Automatic recompilation on GitHub Actions

!!! info "How is the recompilation triggered?"

    If new `.c`, `.cpp`, `.h` or `.hpp` files or changes of those are pushed to any branch, the [compile MEX](https://github.com/spm/spm/actions/workflows/compile_mex.yml) action will automatically run and recompile all MEX files. Changes in `external/` are ignored. It can also be triggered manually.

The compiled MEX files will be available as zip a file under the [compile MEX](https://github.com/spm/spm/actions/workflows/compile_mex.yml) action by selecting the corresponding run of the action and downloading the `spm-mex-all.zip` file. It contains compiled MEX files for Windows, Linux, MacOS (Intel) and MacOS (Apple Silicon). Each is built with the oldest MATLAB release the action can install on that platform (R2020b on Linux and MacOS (Intel), R2022a on Windows, R2023b on MacOS (Apple Silicon)), so that the MEX files also load in newer releases.

The Linux MEX files are compiled inside a [manylinux2014](https://github.com/pypa/manylinux) container, so they only require `glibc` 2.17 and load on every Linux distribution supported by MATLAB. The run summary reports the `glibc` and `libstdc++` versions they require. MEX files compiled locally require at least the `glibc` version of the machine they were built on, so only add the automatically compiled Linux MEX files to the repository.

The suggested steps after changing or adding new c code are:

1. Confirm that the [automatic tests](https://github.com/spm/spm/actions/workflows/matlab.yml) still succeed
2. Download the [automatically compiled MEX files](https://github.com/spm/spm/actions/workflows/compile_mex.yml)
3. Manually add the MEX files of the corresponding change to the SPM repository (.mexa64, .mexmaca64, .mexmaci64, .mexw64)

Note: Currently, the action doesn't compile the external c dependencies. This is because of a problem of compiling the fieldtrip dependency on windows.
