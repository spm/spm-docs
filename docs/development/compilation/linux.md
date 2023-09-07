# Compilation of SPM on Linux

This page describes the compilation of SPM MEX files on Linux. It requires the installation of a C/C++ compiler and related tools (e.g. `make`). This can be done on Debian/Ubuntu by installing the `build-essential` package (`sudo apt install build-essential`).

## Instructions

In a Terminal, ==from the `src` folder== of your SPM installation, type:

```
make distclean
make && make install
make external-distclean
make external && make external-install
```

This will generate `*.mexa64` MEX files.

## Troubleshooting
    
!!! failure "`cannot find -lstdc++` error"

    If you get the error message "`cannot find -lstdc++`", try installing the `build-essential` package using your package manager (i.e. `sudo apt install build-essential` on Ubuntu), or the specific `libstdc++5` package (replacing 5 with the version of your system) with `sudo apt-get install libstdc++5`.

    If, after the install of `libstdc++5`, the `lstdc++` error persists, just add a symbolic link with:
    
    ```
    sudo ln -s /usr/lib64/libstdc++.so.6 /usr/lib64/libstdc++.so 
    ```

    and it should work. If not, see next.

    For old versions of Ubuntu and MATLAB, have a look at [these instructions](https://web.archive.org/web/20160307232613/http://www.lukedodd.com/compiling-gcc-4-3-4-under-ubuntu-11-10-for-matlab/).

!!! failure "Other problems with `lstdc++`"

    If you get the above error and cannot install `libstdc++` (e.g. you do not have root/admin access), or if you get other problems such as "`... version `GCC_4.2.0' not found (required by /usr/lib64/libstdc++.so.6`", then another option is to edit your `mexopts.sh` config file and delete or disable the `-lstdc++` compiler flags, for example simply use find-and-replace to delete all instances of `-lstdc++`. SPM will compile properly without this.

    It's best to edit a local copy of `mexopts.sh` (the original is found in the `bin` directory with the `matlab` and `mex` executables). If you run `mex -setup` it will interactively create a copy for you, under `~/.matlab/RELEASE`, for example:

    ```
    ~/.matlab/R2020a/mexopts.sh
    ```

    or similar, and you may then further edit that copy if required. If you match the release, `mex` will find your copy of `mexopts.sh`.

!!! failure "`mex: command not found` error"

    If you get the error "`mex: command not found`" check that `mex` is in your path. Either add the MATLAB binary directory (usually `/usr/local/matlab/bin`) to your path or create a link to `mex` somewhere already in the path (usually `/usr/local/bin)`.

    Adding the path:
    ```
    PATH=/usr/local/matlab/bin:$PATH
    ```

    Creating a Link:
    ```
    cd /usr/local/bin
    ln -s /usr/local/matlab/bin/mex
    ```

    Another solution is to provide the full path to the `mex` executable in an environment variable called `MEXBIN`. It will be used by the `Makefile` instead of the default one.

    ```
    MEXBIN=/usr/local/matlab/bin/mex make
    ```

!!! failure "`This is pdfTeX, Version ... (TeX Live 2011)` error"

    If you get the error:
    ```
    mex: unrecognized option `-O'
    mex: unrecognized option `-c'
    This is pdfTeX, Version ... (TeX Live 2011)
    ```

    This is due to a conflict between MATLAB `mex` and a LaTeX command (from the texlive package) with the same name. Change `PATH` or `MEXBIN` as described in the previous solution to make sure that the MATLAB `mex` executable is called first.

!!! failure "Missing files"

    If you get build errors such as "`math.h` does not exist" when trying to execute the command `make && make install`, then you probably need to install the `build-essentials` package. 
    
    At the command line in a Terminal, type:
    ```
    sudo apt install build-essential
    ```
    
    or similar command to install a C/C++ compilation environment if you are not using a Debian-based Linux distribution.

!!! failure "`plugin needed to handle lto object` error"

    Edit `spm/src/Makefile.var` at the line defining `AR` so that it reads:

    ```
    AR           = gcc-ar rcs
    ```

!!! failure "`Crash at startup`"

    The following concerns a situation where MATLAB generates a segmentation fault when opening the SPM interface with errors like:
    ```
    BadWindow (invalid Window parameter)
    ``` 
    ```
    serial 20133 error_code 3 request_code 20 minor_code 0
    ```
    ```
    Pango-CRITICAL **: pango_font_description_from_string: assertion 'str != NULL' failed
    ```
    ```
    GLib-CRITICAL **: g_once_init_leave: assertion 'result != 0' failed
    ``` 
    ```
    GLib-GObject-CRITICAL **: g_type_register_dynamic: assertion 'parent_type > 0' failed
    ```

    This is likely to be an issue with the display of the welcoming message in the Graphics window as a web document.

    One fix is to [comment one line in spm.m.](https://github.com/spm/spm12/blob/r7771/spm.m#L352). Another fix not requiring to modify the SPM source code is to [define an environment variable.](https://github.com/spm/spm12/blob/r7771/spm_browser.m#L54):
    ```
    setenv('SPM_HTML_BROWSER','0')
    ```
