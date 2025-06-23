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
the python installer. Hopefully, you are also familiar with virtual
environments and their managers, such as
[`conda`](https://docs.conda.io),
[`mamba`](https://mamba.readthedocs.io),
[`venv`](https://docs.python.org/3/library/venv.html),
[`virtualenv`](https://virtualenv.pypa.io),
[`pixi`](https://pixi.sh),
[`uv`](https://docs.astral.sh/uv/), or
[`poetry`](https://python-poetry.org). If not, it's probably time to
take a break and search for something like
"a beginner's guide de python environments" in your favourite search
engine (or chatbot). Now that you do, you should have activated your python
environment and can type

```shell
pip install spm-python
```

This will install:

- `spm-python`, which defines pythonic signatures of each SPM function or
  class;
- `spm-runtime`, which contains a compiled version of SPM;
- `mpython-core`, which implements the interface between Python and MATLAB,
  along with a [MATLAB-like type system for Python](datatypes.md);
- `matlab-runtime`, which provided an installation script for the MATLAB
  runtime, and properly sets the python environment such that the MATLAB
  runtime can be found and used.

!!! note

    Not all versions of MATLAB are compatible with all versions of Python.
    When installing `spm-python`, `pip` will select a version of the SPM
    runtime that was compiled with a the most recent version of MATLAB that
    is compatible with the current version of Python.

    As of June 2025, this is how version selection works:

    - Python 3.6: MATLAB R2020b
    - Python 3.7: MATLAB R2021b
    - Python 3.8: MATLAB R2023a
    - Python 3.9-3.12: MATLAB R2025a

!!! tip
    Instead, it is possible to install a version of SPM-Python compiled
    with a specific version of MATLAB (assuming that it is compatible
    with the current Python) by specifying an _extra_ tag:

    ```shell
    pip install spm-python[R2024a]
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

Let's first determine the version of the Runtime that we need for the current
SPM-Python:

```shell
python -c "import spm_runtime; print(spm_runtime.__matlab_release__)"
```

This will print something like `R2024b`. We can now run the helper script

```shell
install_matlab_runtime -v $MATLAB_VERSION
```

!!! tip
    - With bash, the two previous commands can be combined into

    ```bash
    install_matlab_runtime -v $( python -c "import spm_runtime; print(spm_runtime.__matlab_release__)" )
    ```

    - To avoid having to enter `yes` to each question, run the script with
    the `--yes` option:

    ```shell
    install_matlab_runtime -v $MATLAB_VERSION --yes
    ```

    - To specify the installation directory, run the script with the `--prefix`
    option:

    ```shell
    install_matlab_runtime -v $MATLAB_VERSION --prefix $INSTALL_PATH
    ```

    In this case, you should define the environment variable
    `MATLAB_RUNTIME_PATH` in your shell profile.

!!! failure
    - `command not found: install_matlab_runtime`: if you've just installed
    spm-python, you may need to open a new shell for the script to be
    visible on your path.

    - On windows, you may need to run this script with administrative privileges.

Alternatively, if you have MATLAB installed, and a license for the MATLAB
Compiled, you may prefer to install the runtime as part of the MATLAB compiler.
In MATLAB, click on **"Add-Ons"**, then search **"Compiler"** and select
**"MATLAB Compiler"**, then **"Install"**. If your MATLAB is not installed
in a standard location, you will need to set the environment variable
`MATLAB_PATH` in your shell profile.

---

#### Let's test that everything is properly installed

Run SPM standalone:

```shell
spm
```

Simply print the SPM version:

```shell
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

```shell
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
    interpreter, named `mwpython2`, with SPM-Python. To run the previous
    example, type:

    ```shell
    $ mwpython2 -c "from spm import *; spm('AsciiWelcome')"
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
