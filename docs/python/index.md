# SPM and Python

![PyPI - Python Version](https://img.shields.io/pypi/pyversions/spm-python)
![PyPI - License](https://img.shields.io/pypi/l/spm-python)
![PyPI - Version](https://img.shields.io/pypi/v/spm-python)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/spm/spm-python/.github%2Fworkflows%2Frun_unit_tests.yml)

We have developed a Python interface that enable performing SPM analyses in Python. The package provides an intuitive Python interface to SPM. Under the hood, the package does not reimplement SPM functions but rather relies on Matlab Runtime to execute the SPM routines - guaranteeing that the results are numerically consistent with established Matlab routines.

This project is supported by the SPM team and hosted on Github [here](https://github.com/spm/spm-python).


<div class="grid cards" markdown>

-   :material-lock-open:{ .lg .middle } __Installation__

    ---

    SPM-Python is available on `pip` and does not require a MATLAB license!

    [:octicons-arrow-right-24: **Installation** ](installation.md)

-   :material-run:{ .lg .middle } __Getting Started__

    ---

    We provide a series of short examples to get started with
    calling SPM functions, work with SPM objects, and write and run
    SPM batches.

    [:octicons-arrow-right-24: **Getting Started**](gettingstarted.md)

-   :material-connection:{ .lg .middle } __Data types__

    ---

    SPM-Python implements a Python type system that allows all MATLAB
    types to be interacted with. Get familiar with them.

    [:octicons-arrow-right-24: **Type System**](datatypes.md)

-   :material-shield-bug:{ .lg .middle } __Troubleshooting__

    ---

    Python and MATLAB have fundamentally different design philosophies;
    interfacing them can lead to issues and ambiguities. We have compiled
    a list of common problems encountered when using SPM-Python, and
    their solutions.

    [:octicons-arrow-right-24: **Troubleshooting**](troubleshooting.md)

-   :fontawesome-solid-graduation-cap:{ .lg .middle } __Tutorials__

    ---

    Read our tutorials for applying SPM-Python in different contexts
    (fMRI, VBM, EEG, MEG, _etc._).

    [:octicons-arrow-right-24: **Tutorials**](tutorials.md)

-   :material-book-cog:{ .lg .middle } __API__

    ---

    The complete SPM API documentation is available. All functions
    describe here can be accessed from MATLAB or from Python.

    [:octicons-arrow-right-24: **API**](api/spm/index.md)
</div>

!!! warning
    This project is **currently under construction** and might contain bugs.
    **If you experience any issues, please [let us know](https://github.com/spm/spm-python/issues)!**
