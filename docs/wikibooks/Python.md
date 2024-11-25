## SPM and Python

### Python interface to SPM 

![PyPI - Python Version](https://img.shields.io/pypi/pyversions/spm-python)
![PyPI - License](https://img.shields.io/pypi/l/spm-python)
![PyPI - Version](https://img.shields.io/pypi/v/spm-python)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/spm/spm-python/.github%2Fworkflows%2Frun_unit_tests.yml)

We have developed a Python interface that enable performing SPM analyses in Python. The package provides an intuitive Python interface to SPM. Under the hood, the package does not reimplement SPM functions but rather relies on Matlab Runtime to execute the SPM routines - guaranteeing that the results are numerically consistent with established Matlab routines. 

This project is supported by the SPM team and hosted on Github [here](https://github.com/spm/spm-python). 

> [!WARNING]
> This project is **currently under construction** and might contain bugs. **If you experience any issues, please [let us know](https://github.com/spm/spm-python/issues)!**

<br/>

---

### External projects
#### Nipype

[Nipype](https://nipype.readthedocs.io/en/latest/) (Neuroimaging in
Python) has an [SPM
interface](https://nipype.readthedocs.io/en/latest/api/generated/nipype.interfaces.spm.html).

Tutorials can be found at:

- <https://miykael.github.io/nipype_tutorial/>
- <https://pythonhosted.org/nipype/users/examples/fmri_spm_auditory.html>
- 
#### spm1d

[spm1d](http://www.spm1d.org/) is a package for one-dimensional
Statistical Parametric Mapping.

#### pymdp

[pymdp](https://github.com/infer-actively/pymdp) is Python package for simulating Active Inference agents in Markov Decision Process environments --- building on MDP implementations in the DEM toolbox. 

#### dempy 

[dempy](https://github.com/johmedr/dempy) is Python implementation of the Dynamic Expectation Maximization algorithm. 


