---
hide:
  - navigation
---

# Statistical Parametric Mapping

## Overview
Statistical Parametric Mapping is the construction and assessment of spatially extended statistical processes used to test hypotheses about functional imaging data. These ideas have been instantiated in a free and open source software that is called **SPM**.

The SPM software package has been designed for the analysis of brain imaging data sequences. The sequences can be a series of images from different cohorts, or time-series from the same subject. The current release is designed for the analysis of **fMRI**, **PET**, **SPECT**, **EEG** and **MEG**.

<figure markdown>
  ![spm logo](assets/images/spm_front.png)
  <figcaption></figcaption>
</figure>

## Getting started

SPM is openly available and designed to work across all major operating systems. We recommend you use [the lastest version](https://github.com/spm/spm/releases/latest/). Older versions are available but no longer support:

- [SPM12](https://github.com/spm/spm12)

- [SPM8](https://github.com/spm/spm8)

- [SPM5](https://github.com/spm/spm5)

- [SPM99](https://github.com/spm/spm99)

=== ":fontawesome-brands-windows: Windows"

### Installation

    1. Download [the latest SPM release](https://github.com/spm/spm/releases/latest/) and uncompress it.

    2. Start MATLAB and add SPM to your path:

        ```matlab
        addpath('C:\Users\<login>\Documents\MATLAB\spm')
        savepath % if you want to save the current MATLAB path
        ```

    *For a detailed overview of the steps, please check our [installation page](./installation/index.md#windows).*

=== ":fontawesome-brands-apple: macOS"

    1. Download [the latest SPM release](https://github.com/spm/spm/releases/latest/) and uncompress it.

    2. Start MATLAB and add SPM to your path:

        ```matlab
        addpath /Users/<login>/spm
        savepath % if you want to save the current MATLAB path
        ```

    *For a detailed overview of the steps, please check our [installation page](./installation/index.md#mac).*

=== ":fontawesome-brands-linux: Linux"

    1. Download [the latest SPM release](https://github.com/spm/spm/releases/latest/) and uncompress it.

    2. Start MATLAB and add SPM to your path:

        ```matlab
        addpath /home/<login>/spm
        savepath % if you want to save the current MATLAB path
        ```

    *For a detailed overview of the steps, please check our [installation page](./installation/index.md#linux).*

!!! info inline end "Alternative installation options" 
    For other installation options, including versions not requiring MATLAB, please consult our [installation instructions](./installation/index.md)

### Usage

Once installed, you can use SPM via the graphical user interface or command line. Please see our [tutorials page](./tutorials/) for examples. 

## Citing SPM

Please acknowledge using SPM by citing:

Tierney et al., (2025). **SPM 25: open source neuroimaging analysis software**. Journal of Open Source Software, 10(110), 8103, https://doi.org/10.21105/joss.08103.

```
@article{Tierney2025, 
    title = {SPM 25: open source neuroimaging analysis software}, 
    author = {Tierney, Tim M. and Alexander, Nicholas A. and Ashburner, John and Avila, Nicole Labra and Balbastre, Yaël and Barnes, Gareth and Bezsudnova, Yulia and Brudfors, Mikael and Eckstein, Korbinian and Flandin, Guillaume and Friston, Karl and Jafarian, Amirhossein and Kowalczyk, Olivia S. and Litvak, Vladimir and Medrano, Johan and Mellor, Stephanie and O'Neill, George and Parr, Thomas and Razi, Adeel and Timms, Ryan and Zeidman, Peter}, 
    journal = {Journal of Open Source Software} 
    doi = {10.21105/joss.08103}, 
    url = {https://doi.org/10.21105/joss.08103}, 
    year = {2025}, 
    publisher = {The Open Journal}, 
    volume = {10}, 
    number = {110}, 
    pages = {8103}, 
}
```

## License
SPM is an open-source software available under GPL-2.0 license. For more details, see the [license](https://github.com/spm/spm?tab=GPL-2.0-1-ov-file#GPL-2.0-1-ov-file).

## Contributing 
The development of SPM takes place on [GitHub](https://github.com/spm/spm/). If you wish to contribute, please visit our [development page](./development/).

--8<-- "addons/abbreviations.md"
