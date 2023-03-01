# Installation

!!! info "Prerequisites"
    The SPM software is a collection of [MATLAB](https://www.mathworks.com/products/matlab.html) functions and thus requires the MATLAB software to be installed on your computer in order to run. SPM requires only core MATLAB to run (no special toolboxes are required - unless stated otherwise). 

    Each SPM version was written for a particular MATLAB version and will not work with earlier versions. MATLAB versions released after SPM can have some peculiarities but SPM developers try to provide compatibility fixes in the updates. 
    
    [GNU Octave](https://www.octave.org/) is a free open-source numerical analysis software similar to MATLAB. See SPM/Octave for more details about compatibility between SPM and Octave. 

=== "Windows"

    ## Installation 

    1. Download spm12.zip.
    2. Unzip spm12.zip in a folder of your choice, such as `C:\Users\login\Documents\MATLAB\spm12`.
    3. Start MATLAB and add SPM into your path, either using:

        `File` :material-arrow-right-bold: `Set Path` :material-arrow-right-bold: `Add Folder...` 

        or typing:

        ```
        addpath C:\Users\login\Documents\MATLAB\spm12
        ```
        
        in MATLAB's workspace.

    4. Launch SPM by typing `spm`.

    ## Update

    If you have just downloaded the `spm12.zip` archive, it already contains the latest set of updates. To update SPM when a new version is released:

    1. [Download `spm12_updates_rxxxx.zip`](https://www.fil.ion.ucl.ac.uk/spm/software/download/).
    2. Uncompress `spm12_updates_rxxxx.zip` on top of the folder containing your SPM installation so that newer files overwrite existing files.

    Alternatively, you can use the `spm_update.m` function:

    ```
    spm_update
    ```

    If a new version is available, it can be applied to your local installation by typing:

    ```
    spm_update update
    ```

=== "Mac"
    ## Installation

    Mac Intel with 64bit MATLAB is a supported SPM platform. Precompiled MEX files (`*.mexmaci64`) are included in the SPM distribution.

    1. [Download `spm12.zip`](https://www.fil.ion.ucl.ac.uk/spm/software/download/) and its updates `spm12_updates_rxxxx.zip` in your home directory.

    2. Uncompress the archive by typing the following in a terminal:
        ```
        cd /Users/login
        unzip spm12.zip
        unzip -o spm12_updates_rxxxx.zip -d spm12
        ```

    3. Start MATLAB and add SPM to your path, either using: 
        `File` :material-arrow-right-bold: `Set Path` :material-arrow-right-bold: `Add Folder...` 
        
        or typing:

        ```
        addpath /Users/login/spm12
        savepath
        ```
        
        in MATLAB's workspace. 

    ??? failure "Troubleshooting"
        **macOS Catalina**
        
        If you have issues with MEX files on macOS Catalina such as:
        
        "`*.mexmaci64` cannot be opened because the developer cannot be verified. macOS cannot verify that this app is free from malware" 
        
        or:
        
        "Code signature not valid for use in process using Library Validation: library load disallowed by system policy"
        
        Open a terminal, `cd` to the SPM directory and type:
        
        ```
        find . -name "*.mexmaci64" -exec xattr -d com.apple.quarantine {} \;
        ```
        
        **macOS Big Sur**
        
        If you have issues with MEX files on macOS Big Sur such as: 
        
        "`*.mexmaci64` cannot be opened because the developer cannot be verified. macOS cannot verify that this app is free from malware"
        
        or: 
        
        "Code signature not valid for use in process using Library Validation: library load disallowed by system policy"
        
        Open a terminal, and type the following whilst replacing `SPM_PATH` with the path of your spm installation:

        ```
        sudo xattr -r -d com.apple.quarantine SPM_PATH
        sudo find SPM_PATH -name "*.mexmaci64" -exec spctl --add {} \;
        ```
        
        **Java Exception and abrupt exit on Mac OS X version 10.10 Yosemite**

        If you get MATLAB issues with Yosemite, see [this bug report](http://www.mathworks.com/support/bugreports/1098655) for a patch.

        **"This is pdfTeX, Version ..." error**

        If you get an error message such as:

        ```
        mex -O -c spm_vol_utils.c -DSPM_UNSIGNED_CHAR 
        mex: unrecognized option `-O'
        mex: unrecognized option `-c'
        This is pdfTeX, Version 3.1415926-2.3-1.40.12 (TeX Live 2011)
        restricted \write18 enabled.
        entering extended mode
        (./spm_vol_utils.c
        This is MeX  Version 1.05  18 XII 1993  (B. Jackowski & M. Ry\'cko)
        ! You can't use `macro parameter character #' in vertical mode.
        ```
        
        This is due to a conflict between MATLAB mex and a LaTeX command with the same name. Edit `src/Makefile.var` and mention the full path when referring to MEXBIN.
        
        ```
        MEXBIN = /Applications/MATLAB_R2012a.app/bin/mex
        ```
        
        or change PATH accordingly. 

=== "Unix/Linux"

    ## Installation

    1. Download `spm12.zip` and its updates `spm12_updates_rxxxx.zip` in your home directory.

    2. Uncompress the archive by typing the following in a terminal:

        ```
        cd /home/login
        unzip spm12.zip
        unzip -o spm12_updates_rxxxx.zip -d spm12
        ```

    3. Start MATLAB and add SPM into your path, either using:

        `File` :material-arrow-right-bold: `Set Path` :material-arrow-right-bold: `Add Folder...` 
        
        or typing:

        ```
        addpath /home/login/spm12
        ```

        in MATLAB's workspace.
        
    ## Compilation

    In a Terminal, from the src folder of your SPM installation, type:

    ```
    cd /home/login/spm12/src
    make distclean
    make && make install
    make external-distclean
    make external && make external-install
    ```

    ??? failure "Troubleshooting"
        **"bad image handle dimensions" error**

        The error "bad image handle dimensions" when trying to display an image is an indication that you need to (re)compile the SPM MEX files.
        
        **"cannot find -lstdc++" error**

        If you get the error message "cannot find -lstdc++", try installing the build-essential package using your package manager (i.e. sudo apt-get install build-essential on Ubuntu), or the specific libstdc++5 package (replacing 5 with the version of your system) with sudo apt-get install libstdc++5.

        If, after the install of libstdc++5, the lstdc++ error persists, just add a symbolic link with:
        
        ```
        sudo ln -s /usr/lib64/libstdc++.so.6 /usr/lib64/libstdc++.so 
        ```

        and it should work. If not, see next.

        For old versions of Ubuntu and MATLAB, have a look at [these instructions](https://web.archive.org/web/20160307232613/http://www.lukedodd.com/compiling-gcc-4-3-4-under-ubuntu-11-10-for-matlab/).

        **Other problems with lstdc++**

        If you get the above error and cannot install libstdc++ (e.g. you do not have root/admin access), or if you get other problems such as "... version `GCC_4.2.0' not found (required by /usr/lib64/libstdc++.so.6", then another option is to edit your mexopts.sh config file and delete or disable the -lstdc++ compiler flags, for example simply use find-and-replace to delete all instances of -lstdc++. SPM will compile properly without this.

        It's best to edit a local copy of mexopts.sh (the original is found in the bin directory with your matlab and mex executables). If you run `mex -setup` it will interactively create a copy for you, under `~/.matlab/RELEASE`, for example:

        ```
        ~/.matlab/R14SP3/mexopts.sh
        ~/.matlab/R2008b/mexopts.sh
        ```

        or similar, and you may then further edit that copy if required. If you match the release, mex will find your copy of `mexopts.sh`. Note that MATLAB 
        
        ```
        version
        ```

        should show you the release, but with parentheses around it, e.g. `"(R14SP3)"`.

        **"mex: command not found" error**

        If you get the error "mex: command not found" check that mex is in your path. Either add the MATLAB binary directory (usually `/usr/local/matlab/bin`) to your path or create a link to mex somewhere already in the path (usually `/usr/local/bin)`.

        Adding the path:
        ```
        PATH=/usr/local/matlab/bin:$PATH
        ```

        Creating a Link:
        ```
        cd /usr/local/bin
        ln -s /usr/local/matlab/bin/mex
        ```

        Another solution is to edit the Makefile (`spm8/src/Makefile.var` for SPM8 or `spm5/src/Makefile` for SPM5) and mention the full path when referring to MEXBIN:

        ```
        MEXBIN = /usr/local/matlab/mex
        ```

        **"This is pdfTeX, Version ... (TeX Live 2011)" error**

        If you get the error:
        ```
        mex: unrecognized option `-O'
        mex: unrecognized option `-c'
        This is pdfTeX, Version ... (TeX Live 2011)
        ```

        This is due to a conflict between MATLAB mex and a LaTeX command (from the texlive package) with the same name. Change `PATH` or `MEXBIN` as described in the previous solution.

        **Missing files**

        If you get build errors such as "`math.h` does not exist" when trying to execute the command `make && make install`, then you probably need to install the `build-essentials` package. 
        
        At the command line type:
        ```
        sudo aptitude install build-essential" 
        ```
        If you have sudo privileges or simply log in as root or obtain superuser status and type: 
        ```
        aptitude install build-essential
        ```
        
        **"plugin needed to handle lto object" error**

        Edit `spm12/src/Makefile.var` at the line defining AR so that it reads:

        ```
        AR           = gcc-ar rcs
        ```

        **Crash at startup**

        The following concerns a situation where MATLAB segfaults when opening the SPM interface with errors like: "
        ```
        BadWindow (invalid Window parameter)``` 
        ```serial 20133 error_code 3 request_code 20 minor_code 0
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

        This is likely to be an issue with the display of the welcoming message in the graphics window as a web document.

        [One fix is to comment one line in spm.m.](https://github.com/spm/spm12/blob/r7771/spm.m#L352)

        [Another fix not requiring to modify the SPM source code is to define an environment variable.](https://github.com/spm/spm12/blob/r7771/spm_browser.m#L54)
        ```
        setenv('SPM_HTML_BROWSER','0')
        ```