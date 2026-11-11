# SPM and Python

## Installation

SPM-Python is a python package that _binds_ against the SPM MATLAB code.
This means that when we're running an SPM function from python, such as
`spm.spm('fmri')`, python transfers the input argument (here `'fmri'`)
to MATLAB, which runs the MATLAB function `spm`. In order to run this
MATLAB code, we need the MATLAB _Runtime_ to be installed. Do not worry,
this Runtime does not require a MATLAB license (in other words, it is free
to use), and we provide a script to ease its installation and setup.

---

#### First, let's install SPM-Python

If you are familiar with Python, you should also be familiar with `pip`,
the python installer.

!!! tip
    Hopefully, you are also familiar with virtual environments and
    their managers, such as
    [`conda`](https://docs.conda.io),
    [`mamba`](https://mamba.readthedocs.io),
    [`venv`](https://docs.python.org/3/library/venv.html),
    [`virtualenv`](https://virtualenv.pypa.io),
    [`pixi`](https://pixi.sh),
    [`uv`](https://docs.astral.sh/uv/), or
    [`poetry`](https://python-poetry.org)
    If not, it's probably time to take a break and search for something
    like "a beginner's guide de python environments" in your favourite
    search engine (or chatbot).

After activating your favourite python environment, you can type

```console
$ pip install spm-python
```

This will install:

- [`spm-python`](https://github.com/spm/spm-python),
  which defines pythonic signatures of each SPM function or class;
- [`spm-runtime`](https://github.com/spm/spm-runtime),
  which contains a compiled version of SPM;
- [`mpython-core`](https://github.com/MPython-Package-Factory/mpython-core),
  which implements the interface between Python and MATLAB,
  along with a [MATLAB-like type system for Python](datatypes.md);
- [`matlab-runtime`](https://github.com/balbasty/matlab-runtime),
  which provided an installation script for the MATLAB runtime, and
  properly sets the python environment such that the MATLAB runtime can
  be found and used.

!!! note

    Not all versions of MATLAB are compatible with all versions of Python.
    When installing `spm-python`, `pip` will select a version of the SPM
    runtime that was compiled with a the most recent version of MATLAB that
    is compatible with the current version of Python.

    As of October 2026, this is how version selection works:

    | Python version | Default MATLAB Runtime version |
    | -------------- | ------------------------------ |
    | 3.6            | R2020b                         |
    | 3.7            | R2021b                         |
    | 3.8            | R2023a                         |
    | 3.9            | R2025a                         |
    | 3.10 - 3.13    | R2025b                         |

!!! tip
    Instead, it is possible to install a version of SPM-Python compiled
    with a specific version of MATLAB (assuming that it is compatible
    with the current Python) by specifying an _extra_ tag:

    ```console
    $ pip install spm-python[R2024a]
    ```

    This can be useful if you know that you already have a specific version
    of MATLAB (or the MATLAB runtime) installed.

!!! failure
    If the installation fails with an error of the like:

    ```
    ERROR: Ignored the following versions that require a different python
    version: 25.1.2 Requires-Python <3.12,>=3.9; 25.1.2 Requires-Python <3.12,>=3.9
    ERROR: Could not find a version that satisfies the requirement spm-runtime-r2024a (from versions: none)
    ERROR: No matching distribution found for spm-runtime-r2024a
    ```

    This means that you are trying to install a version of the SPM runtime
    that is not compatible with your version of Python. While we try to cover
    as many versions of Python as possible, each version of MATLAB is only
    compatible with a subset of Python versions. For this reason, we only
    support Python >= 3.6. Furthermore, Python and MATLAB releases are not
    synchronised, which means that the most recent version(s) of Python
    may not be supported by _any_ MATLAB runtime yet. If this is your case,
    you may need to downgrade to a slightly older version of Python.

---

#### Now that this is done, we can install the MATLAB runtime

=== "via `spm`"

    Once `spm-python` is installed, the most straightforward way to install
    the corresponding MATLAB runtime is simply to type

    ```console
    $ spm
    ```

    This will attempt to run the standalone SPM software and, if the runtime
    is missing, it will ask whether it should be installed:

    ```console
    No MATLAB Runtime R2024b found. Install it now? ([yes]/no)
    ```

    It will further ask for your consent before each installation step:

    ```console
    No MATLAB Runtime R2024b found. Install it now? ([yes]/no)
    ```

    ```console
    Download installer from https://.../MATLAB_Runtime_R2024b_Update_10_maca64.zip?
    ([yes]/no)
    ```

    ```console
    Downloading from https://.../MATLAB_Runtime_R2024b_Update_10_maca64.zip ...
    100 %
    done -> /.../MATLAB_Runtime_R2024b_Update_10_maca64.zip
    Unzip /.../MATLAB_Runtime_R2024b_Update_10_maca64.zip?
    ([yes]/no)
    ```

    ```console
    Unzipping /.../MATLAB_Runtime_R2024b_Update_10_maca64.zip ...
    done -> /.../install
    BY ENTERING 'YES', YOU ACCEPT THE TERMS OF THE MATLAB RUNTIME LICENSE,
    LINKED BELOW. THE MATLAB RUNTIME INSTALLER WILL BE RUN WITH THE ARGUMENT
    `-agreeToLicense yes`. IF YOU ARE NOT WILLING TO DO SO, ENTER 'NO' AND
    THE INSTALLATION WILL BE ABORTED.	/.../matlabruntime_license_agreement.pdf
    ([yes]/no)
    ```

    ```console
    License agreed.
    Running the MATLAB installer requires signing off its binaries,
    which requires sudo:
    Password:
    ```

    !!! note "Limitations"
        When using this method, the installer is called with its default options,
        which means that the runtime gets installed in a default location,
        which often requires administrative rights. That is:

        - MacOS: `/Applications/MATLAB/MATLAB_Runtime/`
        - Linux: `/usr/local/MATLAB/MATLAB_Runtime/`
        - Windows: `C:\Program Files\MATLAB\MATLAB Runtime\`

        If this is problematic, the runtime installer provides additional
        flexibility.
        See [_via_ `install_matlab_runtime`](#via-install_matlab_runtime).

=== "via `install_matlab_runtime`"

    Let's first determine the version of the Runtime that we need for the
    current SPM-Python:

    ```console
    $ python -c "import spm_runtime; print(spm_runtime.__matlab_release__)"
    ```

    This will print something like `R2024b`. We can now run the helper script

    ```console
    $ install_matlab_runtime -v $MATLAB_VERSION
    ```

    !!! tip
        - With bash, the two previous commands can be combined into

        ```console
        $ install_matlab_runtime -v $( python -c "import spm_runtime; print(spm_runtime.__matlab_release__)" )
        ```

        - To avoid having to enter `yes` to each question, run the script with
        the `--yes` option:

        ```console
        $ install_matlab_runtime -v $MATLAB_VERSION --yes
        ```

        - To specify the installation directory, run the script with the `--prefix`
        option:

        ```console
        $ install_matlab_runtime -v $MATLAB_VERSION --prefix $INSTALL_PATH
        ```

        In this case, you should define the environment variable
        `MATLAB_RUNTIME_PATH` in your shell profile.

    !!! failure
        - `command not found: install_matlab_runtime`: if you've just installed
        spm-python, you may need to open a new shell for the script to be
        visible on your path.

        - On windows, you may need to run this script with administrative privileges.

=== "via MATLAB"

    Alternatively, if you have MATLAB installed, and a license for the MATLAB
    Compiled, you may prefer to install the runtime as part of the MATLAB compiler.
    In MATLAB, click on **"Add-Ons"**, then search **"Compiler"** and select
    **"MATLAB Compiler"**, then **"Install"**. If your MATLAB is not installed
    in a standard location, you will need to set the environment variable
    `MATLAB_PATH` in your shell profile.

---

#### Let's test that everything is properly installed

Run SPM standalone:

```console
$ spm
```

Simply print the SPM version:

```console
$ spm -v
WARNING: package sun.awt.X11 not in java.desktop
WARNING: package sun.awt.X11 not in java.desktop
SPM25, version 25.01.rc3-80-g86bcaac86 (standalone)
MATLAB, version 24.2.0.2863752 (R2024b) Update 5
 ___  ____  __  __
/ __)(  _ \(  \/  )
\__ \ )___/ )    (   Statistical Parametric Mapping
(___/(__)  (_/\/\_)  SPM25 - https://www.fil.ion.ucl.ac.uk/spm/
```

Run any spm function:

```console
$ python -c "from spm import *; spm('AsciiWelcome')"
Initializing Matlab Runtime...
WARNING: package sun.awt.X11 not in java.desktop
WARNING: package sun.awt.X11 not in java.desktop
 ___  ____  __  __
/ __)(  _ \(  \/  )
\__ \ )___/ )    (   Statistical Parametric Mapping
(___/(__)  (_/\/\_)  SPM25 - https://www.fil.ion.ucl.ac.uk/spm/
```

!!! warning "MacOS"

    On MacOS, SPM-Python can only be used through the MathWorks Python
    interpreter, named `mwpython`. We provide our own wrapper of this
    interpreter, named `mpython`, with SPM-Python. To run the previous
    example, type:

    ```console
    $ mpython -c "from spm import *; spm('AsciiWelcome')"
    Initializing Matlab Runtime...
    WARNING: package sun.awt.X11 not in java.desktop
    WARNING: package sun.awt.X11 not in java.desktop
     ___  ____  __  __
    / __)(  _ \(  \/  )
    \__ \ )___/ )    (   Statistical Parametric Mapping
    (___/(__)  (_/\/\_)  SPM25 - https://www.fil.ion.ucl.ac.uk/spm/
    ```

    There are a number of limitations caused by the use of this MATLAB-specific
    Python interpreter. See the [troubleshooting section](troubleshooting.md).
