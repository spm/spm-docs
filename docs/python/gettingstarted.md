# SPM and Python

## Getting started

### Cheat sheet

SPM-python implements types that are fully equivalent with MATLAB types.
This is a short reminder of conversion rules -- more details are available
at the [data types](datatypes.md) page.

!!! example "General rules"

    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        cell
        struct
        single, double
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        spm.Cell
        spm.Struct
        spm.Array, numpy.ndarray
        ```
    </div>
    </section>

    <!-- | MATLAB             | Python                         |
    | ------------------ | ------------------------------ |
    | `cell`             | `spm.Cell`                     |
    | `struct`           | `spm.Struct`                   |
    | `single`, `double` | `spm.Array` or `numpy.ndarray` | -->

!!! example "Examples"

    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:40%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        A = cell(1, 3);
        B = {"VTA", "M1", "vmPFC"};
        C = struct;
        C.A = A;
        C.array = [1 2; 3 4];
        ```
    </div>
    <div markdown="1" style="width:58%; margin-left:1%">
    !!! python "Python"
        ```python
        A = spm.Cell(1, 3)
        B = spm.Cell.from_any(["VTA", "M1", "vmPFC"])
        C = spm.Struct()
        C.A = A
        C.array = np.array([[1, 2], [3, 4]])
        ```
    </div>
    </section>

    <!-- | MATLAB                        | Python                                          |
    | ----------------------------- | ----------------------------------------------- |
    | `A = cell(1, 3);`             | `A = spm.Cell(1, 3)`                            |
    | `B = {"VTA", "M1", "vmPFC"};` | `B = spm.Cell.from_any(["VTA", "M1", "vmPFC"])` |
    | `C = struct;`                 | `C = spm.Struct()`                              |
    | `C.A = A;`                    | `C.A = A`                                       |
    | `C.array = [1 2; 3 4];`       | `C.array = np.array([[1, 2], [3, 4]])`          | -->

### Minimal example

Here is a minimal set of examples showcasing changes to do to use existing
Matlab code in Python (taken from the OPM tutorial).

!!! example "1. Importing and setting up SPM"
    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        spm('defaults', 'eeg');
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        from spm import *
        spm('defaults', 'eeg')
        ```
    </div>
    </section>

!!! example "2. Constructing object"
    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        S = [];
        S.data = 'OPM_meg_001.cMEG';
        S.positions = 'OPM_HelmConfig.tsv';
        D = spm_opm_create(S);
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        S = Struct()
        S.data = 'OPM_meg_001.cMEG'
        S.positions = 'OPM_HelmConfig.tsv'
        D = spm_opm_create(S)
        ```
    </div>
    </section>

    Here, D will be a meeg object which contains a virtual representation
    of the Matlab object. Class methods should work as expected, e.g.:

    ```python
    D.fullfile()
    >>> './OPM_meg_001.mat'
    ```

    Note that the alternative call that exist in Matlab
    (i.e., `fullfile(D)`) will not work.

!!! example "3. Creating and handling figures"
    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        S = [];
        S.triallength = 3000;
        S.plot = 1;
        S.D = D;
        S.channels = 'MEG';
        spm_opm_psd(S);
        ylim([1,1e5])
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        S = Struct()
        S.triallength = 3000
        S.plot = 1
        S.D = D
        S.channels = 'MEG'
        spm_opm_psd(S)
        ```
    </div>
    </section>

    This opens a Matlab figure, but we do not have the possibility of
    manipulating it yet (e.g., calling ylim). As of now, we can view the
    figures, have GUI interactions, but cannot manipulate figures with Python code.

!!! example "4. Variable number of output arguments"
    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        S = [];
        S.triallength = 3000;
        S.plot = 1;
        S.D = mD;
        [~,freq] = spm_opm_psd(S);
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        S = Struct()
        S.triallength = 3000
        S.plot = 1
        S.D = mD
        [_, freq] = spm_opm_psd(S, nargout=2)
        ```
    </div>
    </section>

    In Python, the number of output arguments must be specified by the
    `nargout` keyword argument.

!!! example "5. Other examples"
    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        S=[];
        S.D=D;
        S.freq=[10];
        S.band = 'high';
        fD = spm_eeg_ffilter(S);
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        S = Struct()
        S.D = D
        S.freq = 10
        S.band = 'high'
        fD = spm_eeg_ffilter(S)
        ```
    </div>
    </section>
    <section markdown="1" style="display: flex; width: 100%">
    <div markdown="1" style="width:49%; margin-right:1%">
    !!! matlab "MATLAB"
        ```matlab
        S = [];
        S.D = fD;
        S.freq = [70];
        S.band = 'low';
        fD = spm_eeg_ffilter(S);
        ```
    </div>
    <div markdown="1" style="width:49%; margin-left:1%">
    !!! python "Python"
        ```python
        S = Struct()
        S.D = fD
        S.freq = 70
        S.band = 'low'
        fD = spm_eeg_ffilter(S)
        ```
    </div>
    </section>
