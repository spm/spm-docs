# Compilation of SPM on macOS

This page describes the compilation of SPM MEX files on macOS.

!!! tip
    If you are using macOS Catalina, Big Sur, Monterey or Ventura and have apparent problems with the MEX files provided with SPM, check the [Troubleshooting section](#troubleshooting) first as it is very likely you do not need to recompile the MEX files.

To compile SPM MEX files, you need to have Apple's development environment [Xcode](http://developer.apple.com/tools/xcode/) installed.

You also need to have the `mex` executable in your system path. To do
so, type the following in a Terminal:
```
export PATH=/Applications/MATLAB_R2023b.app/bin:$PATH
```
with the appropriate path where MATLAB is installed

If you get errors such as `Bad : modifier in $ (/)`, this is because
the instructions are given for a `bash` Terminal while you are using a
`tcsh` Terminal. The equivalent commands are:
```
setenv PATH /Applications/MATLAB_R2023b.app/bin:${PATH}
```

An alternative is to use the `MEXBIN` environment variable when invoking `make`:
```
make MEXBIN=/Applications/MATLAB_R2023b.app/bin/mex
```

## Instructions

In a Terminal, from the *src* folder of your SPM installation,
type:
```
cd /Users/login/spm/src
make distclean
make && make install
make external-distclean
make external && make external-install
```

This will generate `*.mexmaci64` MEX files. To generate `*.mexmaca64` MEX files, add `PLATFORM=arm64` to the commands above.

## Troubleshooting

!!! failure "macOS Catalina, Big Sur, Monterey, Ventura"

    If you have issues with MEX files on macOS with one of these errors:

    ```
    "*.mexmaci64" cannot be opened because the developer cannot be verified.
    macOS cannot verify that this app is free from malware
    ```
    ```
    Code signature not valid for use in process using Library Validation: library load disallowed by system policy
    ```
    open a Terminal, `cd` to the SPM directory and type:

    ```
    find . -name "*.mexmaci64" -exec xattr -d com.apple.quarantine {} \;
    ```

    If it doesn't work, please try this equivalent alternative, replacing `SPM_PATH` with the path of your SPM installation:

    ```
    sudo xattr -r -d com.apple.quarantine SPM_PATH
    sudo find SPM_PATH -name "*.mexmaci64" -exec spctl --add {} \;
    ```

!!! failure "`xcrun: error: SDK "macosx10.14.1" cannot be located` error"

    If you get errors such as `xcrun: error: SDK "macosx10.14.1" cannot be located` while compiling, execute the following before compilation:
    ```
    sudo xcode-select --switch /Applications/Xcode.app/
    ```

!!! failure "Java Exception and abrupt exit on Mac OS X version 10.10 Yosemite"

    If you get MATLAB issues with Yosemite, see the following bug report for a patch:

    [`http://www.mathworks.com/support/bugreports/1098655`](http://www.mathworks.com/support/bugreports/1098655)

!!! failure "`This is pdfTeX, Version ...` error"

    If you get an error message such as:
    ```
    mex -O -c spm_vol_utils.c -DSPM_UNSIGNED_CHAR
      mex: unrecognized option `-O' 
      mex: unrecognized option `-c'
    This is pdfTeX, Version 3.1415926-2.3-1.40.12 (TeX Live 2011)
    restricted \write18 enabled.
    entering extended mode
    (./spm_vol_utils.c
    This is MeX Version 1.05 18 XII 1993 (B. Jackowski & M. Ry\'cko)  
      ! You can't use `macro parameter character #' in vertical mode.
    ```
    this is due to a conflict between MATLAB `mex` and a LaTeX command with
    the same name. Edit `src/Makefile.var` and mention the full path when
    referring to MEXBIN
    ```
    MEXBIN = /Applications/MATLAB_R2012a.app/bin/mex
    ```
    or change PATH accordingly.

!!! failure "Slowdown when using graphical inputs"

    This seems to happen when a window manager for macOS is installed ([Magnet](https://magnet.crowdcafe.com/) or [Tiles](https://freemacsoft.net/tiles/)), see also [this](https://www.mathworks.com/matlabcentral/answers/475682).
