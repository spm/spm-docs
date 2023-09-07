# SPM Installation with MATLAB

!!! info "Prerequisites"
    The SPM software is a collection of [MATLAB](https://www.mathworks.com/products/matlab.html) functions and thus requires the MATLAB software to be installed on your computer in order to run. SPM requires only core MATLAB to run (no special toolboxes are required - unless stated otherwise).

    Each SPM version was written for a particular MATLAB version and will not work with earlier versions. MATLAB versions released after SPM can have some peculiarities but SPM developers try to provide compatibility fixes in the updates.

## Installation

=== "Windows"

    1. Download `spm12.zip` from the [SPM website](https://www.fil.ion.ucl.ac.uk/spm/software/download/).

    2. Unzip `spm12.zip` in a folder of your choice, such as `C:\Users\login\Documents\MATLAB\spm12`.

    3. Start MATLAB and add SPM to your path, either using:

        `Home` :material-arrow-right-bold: `Set Path` :material-arrow-right-bold: `Add Folder...`: select the directory containing your SPM installation then click on `Save` and `Close`.

        or type the following at the MATLAB prompt:

        ```matlab
        addpath('C:\Users\login\Documents\MATLAB\spm12')
        savepath % if you want to save the current MATLAB path
        ```

        If you are using MATLAB without its desktop, you can open the Set Path dialog box by typing `pathtool` at the MATLAB prompt.

    !!! danger
        If using the graphical interface, make sure to use the `Add Folder...` button and not `Add with Subfolders...`. SPM will automatically add the appropriate subfolders to the MATLAB path at runtime.

=== "macOS"

    1. [Download  `spm12.zip` from the [SPM website](https://www.fil.ion.ucl.ac.uk/spm/software/download/) in your home directory.

    2. Uncompress the archive by typing the following in a Terminal:
        ```bash
        cd /Users/<login>
        unzip spm12.zip
        ```

    3. Start MATLAB and add SPM to your path, either using: 
        `File` :material-arrow-right-bold: `Set Path` :material-arrow-right-bold: `Add Folder...` 
        
        or typing the following at the MATLAB prompt:

        ```matlab
        addpath /Users/login/spm12
        savepath % if you want to save the current MATLAB path
        ```

    ??? failure "Troubleshooting"
        **macOS Catalina**
        
        If you have issues with MEX files on macOS Catalina such as:
        
        "`*.mexmaci64` cannot be opened because the developer cannot be verified. macOS cannot verify that this app is free from malware" 
        
        or:
        
        "Code signature not valid for use in process using Library Validation: library load disallowed by system policy"
        
        Open a terminal, `cd` to the SPM directory and type:
        
        ```bash
        find . -name "*.mexmaci64" -exec xattr -d com.apple.quarantine {} \;
        ```
        
        **macOS Big Sur**
        
        If you have issues with MEX files on macOS Big Sur such as: 
        
        "`*.mexmaci64` cannot be opened because the developer cannot be verified. macOS cannot verify that this app is free from malware"
        
        or: 
        
        "Code signature not valid for use in process using Library Validation: library load disallowed by system policy"
        
        Open a terminal, and type the following whilst replacing `SPM_PATH` with the path of your spm installation:

        ```bash
        sudo xattr -r -d com.apple.quarantine SPM_PATH
        sudo find SPM_PATH -name "*.mexmaci64" -exec spctl --add {} \;
        ```
        
        **Java Exception and abrupt exit on Mac OS X version 10.10 Yosemite**

        If you get MATLAB issues with Yosemite, see [this bug report](http://www.mathworks.com/support/bugreports/1098655) for a patch.

        **"This is pdfTeX, Version ..." error**

        If you get an error message such as:

        ```bash
        mex -O -c spm_vol_utils.c -DSPM_UNSIGNED_CHAR 
        mex: unrecognized option '-O'
        mex: unrecognized option '-c'
        This is pdfTeX, Version 3.1415926-2.3-1.40.12 (TeX Live 2011)
        restricted \write18 enabled.
        entering extended mode
        (./spm_vol_utils.c
        This is MeX  Version 1.05  18 XII 1993  (B. Jackowski & M. Ry\'cko)
        ! You can't use 'macro parameter character #' in vertical mode.
        ```
        
        This is due to a conflict between MATLAB mex and a LaTeX command with the same name. Edit `src/Makefile.var` and mention the full path when referring to MEXBIN.
        
        ```bash
        MEXBIN = /Applications/MATLAB_R2012a.app/bin/mex
        ```
        
        or change PATH accordingly. 

=== "Linux"

    1. Download `spm12.zip` from the [SPM website](https://www.fil.ion.ucl.ac.uk/spm/software/download/) in your home directory.

    2. Uncompress the archive by typing the following in a terminal:

        ```bash
        cd /home/login
        unzip spm12.zip
        ```

    3. Start MATLAB and add SPM into your path, either using:

        `File` :material-arrow-right-bold: `Set Path` :material-arrow-right-bold: `Add Folder...` 
        
        or typing the following at the MATLAB prompt:

        ```matlab
        addpath /home/login/spm12
        savepath % if you want to save the current MATLAB path
        ```

![](../../../assets/figures/matlab_setpath.png)

You can then launch SPM by typing `spm` at the MATLAB prompt.

## Update

If you have just downloaded the `spm12.zip` archive, it already contains the latest set of updates. To update SPM when a new version is released:

1. [Download `spm12_updates_rxxxx.zip`](https://www.fil.ion.ucl.ac.uk/spm/download/spm12_updates/).
2. Uncompress `spm12_updates_rxxxx.zip` on top of the folder containing your SPM installation so that newer files overwrite existing files.

Alternatively, you can use the `spm_update.m` function:

```matlab
spm_update
```

If a new version is available, it can be applied to your local installation by typing:

```matlab
spm_update update
```
