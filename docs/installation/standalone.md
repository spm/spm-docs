# Standalone SPM

## Introduction

!!! note "What is Standalone SPM?"
    A standalone SPM is a version of SPM that has been compiled using the [MATLAB Compiler](https://www.mathworks.com/products/compiler.html) so that it does not require the availability of a MATLAB licence (you might want to check first [here](https://www.mathworks.com/academia/tah-support-program/eligibility.html) whether you have access to a MATLAB campus license).

A standalone version of SPM is [available for download from the SPM
website](https://www.fil.ion.ucl.ac.uk/spm/software/download/) (select
`SPM Standalone` as `SPM Version`).

## Installation

Following your request, you will be given instructions to download the
following components:

- The **MATLAB Runtime**: it contains a set of libraries that enables
  the execution of compiled MATLAB applications. You need to install the
  one that corresponds to your platform (Windows, Linux or macOS) and to
  the version that was used to compile SPM. The default SPM standalone
  requires the MATLAB Compiler Runtime (MCR) 7.13 of MATLAB R2010a
  available from the `MCR` folder from the link you will have been
  given. Otherwise more recent MATLAB Runtimes are downloadable from
  [here](https://www.mathworks.com/products/compiler/matlab-runtime.html).
- The **SPM Standalone** itself containing the compiled SPM, as a ZIP
  file.

### Installation of the MATLAB Runtime

You need to do this operation only once, even if you use several
versions of standalone SPM.

Run the MCRInstaller executable you downloaded and follow the
instructions.

Windows installation requires to have administrative privileges and you
might also have to install the Visual C++ Redistributable Package
(`vcredist_*.exe`), that can be found in the same folder as the
MCRInstaller. We also recommend you restart Windows at this point.

If you run into problems, or want to learn more, see:

- <https://www.mathworks.com/help/compiler/install-the-matlab-runtime.html>

To install the MCR on Linux from the command line, type:

```
chmod 755 MCRInstaller.bin
./MCRInstaller.bin -P bean421.installLocation="MCR" -silent
```

### Installation of the SPM Standalone

- Unzip the `spm12_rxxx.zip` archive containing the SPM Standalone in an
  appropriate folder. It contains a number of small executables for each
  supported platform and a large `spm12.ctf` file containing the
  compiled version of SPM12 itself.

- The first time the standalone application will be executed, the CTF
  file will be unpacked in a subfolder so if you installed the CTF in a
  folder that requires administrative permission for write access, you
  should execute the application once under those privileges -- see
  below.

- On `macOS`, you also need to unzip the `spm12_maci64.zip` archive.

## Usage

To start SPM graphical user interface:

- on Windows: double-click on `spm12_wxx.exe`
- on Linux, type

```
./run_spm12.sh /usr/local/MATLAB/MATLAB_Compiler_Runtime/v713/
```

where the argument is the path to your MCR installation.

- on macOS, type

```
./run_spm12.sh /Applications/MATLAB/MATLAB_Compiler_Runtime/v713/
```

where the argument is the path to your MCR installation.

The first execution should take longer to start as the CTF file will be
unpacked. When installing SPM system-wide, you should do the first
unpacker execution as root, i.e.

```
./run_spm12.sh /usr/local/MATLAB/MATLAB_Compiler_Runtime/v713/ quit
```

On Linux/Mac, you can edit the Shell script `run_spm12.sh` to hardcode the
location of the MCR installation, thus removing the need of providing it
on the command line.

The other arguments that can be used are the modality (as in `spm
fmri`) or the keyword `batch` to start directly the batch system
window, e.g.:

```
./run_spm12.sh /Applications/MATLAB/MATLAB_Compiler_Runtime/v713/ fmri
./run_spm12.sh /Applications/MATLAB/MATLAB_Compiler_Runtime/v713/ batch
```

Furthermore, `batch` followed by a batch filename (`*.mat` or
`*.m`) will start SPM, execute the batch and quit:

```
./run_spm12.sh /Applications/MATLAB/MATLAB_Compiler_Runtime/v713/ batch mybatch.mat
```

## Troubleshooting

!!! failure "Why do I receive an error regarding missing `mclmcrrt7x.dll`?"

    The first thing to try is restarting your computer, if you haven't already, since Windows can fail to find this dll even when the path is correctly set if it hasn't been restarted. If you still have problems, see: <http://www.mathworks.com/support/solutions/en/data/1-1IW46N/>

!!! failure "What to do if I get the error `Unable to obtain exclusive lock on   CTF directory because of a file access error.`?"

    You need to set variable `MCR_INHIBIT_CTF_LOCK` to `1`, see [this](http://www.mathworks.com/matlabcentral/answers/109-why-does-the-project_mcr-folder-extracted-from-a-ctf-file-compiled-with-matlab-7-5-r2007b-keep-g).

!!! failure "What to do if I get the error `readlink: illegal option -- f` on a Mac?"

    Change the last line of `run_spm12.sh` so that it reads:
    ```
    dirname $0`/${MACAPP}spm12_${MWE_ARCH} $*
    ```

!!! failure "How to have the standalone version using a single computational   thread?"

    All recent versions of the standalone SPM are compiled with the `-R -singleCompThread` flag in `mcc` so that they will use a single thread at runtime.

    Previous version r4290, was not using this compilation option so you might want to have your MATLAB script to start with `maxNumCompThreads(1);` to have the same effect.

!!! failure "Why do I get a warning `Process manager already initialized -- can't fully enable headless mode`followed by a crash on macOS?"

    Try installing [XQuartz](http://xquartz.macosforge.org/landing/) (you must logout and back in again after installing XQuartz).

!!! failure "What to do if I get the error `Error: libXp.so.6: cannot open shared   object file: No such file or directory`?"

    You need to install the package containing libXp on your Linux platform:
    ```
    yum install libXp.x86_64
    ```
    If you don't find this package on Ubuntu, try installing `libXp` manually (compilation might require to install packages `autoconf`, `autogen`, `xutils-dev` and `x11proto-print-dev`):

    ```
    git clone [git://anongit.freedesktop.org/xorg/lib/libXp](git://anongit.freedesktop.org/xorg/lib/libXp)
    cd libXp
    ./autogen.sh
    ./configure
    make
    sudo make install
    ```

    Alternatively you can find packages for [Ubuntu](http://packages.ubuntu.com/trusty/amd64/libxp6/download) or [Debian](https://packages.debian.org/jessie/amd64/libxp6/download) that can be installed after download with `dpkg -i libxp6_1.0.2-2_amd64.deb`.

!!! failure "What to do if I get the error `This process is attempting to exclude an item from Time Machine by path without administrator privileges.  This is not supported`?"

    Run the standalone as root once, i.e. start the command line with `sudo`.

!!! failure "Why do I get an error `Library not loaded: /usr/X11/lib/libXext.6.dylib` on Mac?"

    Have a look [here](https://www.mathworks.com/matlabcentral/answers/98267), you probably have to reinstall [XQuartz](http://xquartz.macosforge.org/landing/).

!!! failure "Why does standalone SPM crash when starting the GUI?"

    This is discussed [here](../development/compilation/linux.md) and can be fixed by adding the following in `run_spm12.sh`:
    ```
    SPM_HTML_BROWSER=0
    ```

## Frequently Asked Questions

!!! question "Will it run faster?"

    No, see <https://www.mathworks.com/matlabcentral/answers/94695>

!!! question "Is it possible to add other SPM toolboxes?"

    No: only core SPM toolboxes are available, contributed SPM toolboxes are not present and cannot be added without a whole recompilation.

!!! question "How to compile the SPM standalone by myself?"

    Have a look at `config/spm_make_standalone.m` and `spm_standalone.m` in your SPM installation.

    Open MATLAB, at the command line, addpath the SPM directory (if not done already), run `spm_jobman('initcfg')`, then run `spm_make_standalone`. The compilation process takes several minutes. By default, the newly compiled standalone SPM will be saved in a `standalone` directory.

!!! question "How to create a shortcut on your Desktop for a faster launch?"

    On Linux, for achieving menu entries, you can install an appropriate `.desktop` file and an icon file for SPM. For example, save the following as `spm12.desktop` (modify according to your needs/MCR version):
    ```
    [Desktop Entry]
    Name=spm12
    GenericName=SPM12
    Comment=Statistical Parametric Mapping
    Exec=/usr/local/SPM/spm12/run_spm12.sh /usr/local/MATLAB/MATLAB_Compiler_Runtime/v713
    Icon=spm
    Terminal=false
    Type=Application
    Categories=Education;Science;
    ```

    Additionally download the [SPM icon](https://www.fil.ion.ucl.ac.uk/spm/favicon.ico) and convert it to a PNG file, name it `spm.png` (this is the one referred to in the `.desktop` file as icon).

    Then (package `xdg-utils` required) execute in a terminal:
    ```
    xdg-desktop-menu install --novendor spm12.desktop
    xdg-icon-resource install --novendor --size 32 spm.png
    ```

    To uninstall desktop and icon file run:
    ```
    xdg-icon-resource uninstall --size 32 spm
    xdg-desktop-menu uninstall spm12.desktop
    ```

## Documentation

The MATLAB Compiler Toolbox presentation:

- <https://www.mathworks.com/products/compiler/>

The MATLAB Compiler Toolbox documentation:

- <https://www.mathworks.com/access/helpdesk/help/toolbox/compiler/>

The MATLAB Compiler Support page:

- <https://www.mathworks.com/support/product/solutions.html?product=CO>
