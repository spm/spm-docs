SPM (Statistical Parametric Mapping) refers to the construction and
assessment of spatially extended statistical processes used to test
hypotheses about functional imaging data. These ideas have been
instantiated in software that is called SPM.

The SPM software package has been designed for the analysis of brain
imaging data sequences. The sequences can be a series of images from
different cohorts, or time-series from the same subject. The current
release is designed for the analysis of fMRI, PET, SPECT, EEG and MEG.

SPM is made freely available to the \[neuro\]imaging community, to
promote collaboration and a common analysis scheme across laboratories.
The software represents the implementation of the theoretical concepts
of Statistical Parametric Mapping in a complete analysis package, as a
suite of [MATLAB](https://www.mathworks.com/products/matlab.html)
functions and subroutines with some externally compiled
[C](https://www.mathworks.com/help/matlab/external-language-interfaces.html)
routines.

## Overview

- [SPM website](https://www.fil.ion.ucl.ac.uk/spm/) at the [Wellcome
  Centre for Human Neuroimaging](https://www.fil.ion.ucl.ac.uk/),
  [UCL](https://www.ucl.ac.uk), UK.
- [Overview of SPM](w:Statistical_parametric_mapping "wikilink") from
  [Wikipedia](http://en.wikipedia.org/)
- [Overview of
  SPM](http://www.scholarpedia.org/article/Statistical_parametric_mapping_(SPM))
  from [Scholarpedia](http://www.scholarpedia.org/)
- [Introduction](https://www.fil.ion.ucl.ac.uk/spm/doc/intro/) to
  Statistical Parametric Mapping
- [History](https://www.fil.ion.ucl.ac.uk/spm/doc/history.html) of
  Statistical Parametric Mapping in functional neuroimaging

## Installation

Overview of SPM installation:

- [Download](SPM/Download "wikilink")

System Requirements (hardware and software):

- [MATLAB version](SPM/MATLAB "wikilink")
- [Advice on hardware
  selection](SPM/Advice_on_hardware_selection "wikilink")

Detailed installation and compilation on:

- [Windows 64bit](SPM/Installation_on_64bit_Windows "wikilink")
- [Linux 64bit](SPM/Installation_on_64bit_Linux "wikilink")
- [macOS 64bit](SPM/Installation_on_64bit_Mac_OS_(Intel) "wikilink")

Instructions for older platforms: [Windows
32bit](SPM/Installation_on_Windows "wikilink"), [Linux
32bit](SPM/Installation_on_Linux "wikilink"), [MacOS
(Intel)](SPM/Installation_on_Mac_OS_(Intel) "wikilink"), [MacOS
(PowerPc)](SPM/Installation_on_Mac_OS_(PowerPC) "wikilink"), [SunOS /
Solaris](SPM/Installation_on_SunOS "wikilink").

Standalone version using the MATLAB Compiler:

- [Standalone SPM](SPM/Standalone "wikilink")

Optimising your installation:

- [Optimising MATLAB/SPM](SPM/Faster_SPM "wikilink")

Containerisation:

- [Docker and Singularity](SPM/Docker "wikilink")

Miscellaneous:

- [GNU Octave](SPM/Octave "wikilink")
- [Julia](SPM/Julia "wikilink")
- [Python](SPM/Python "wikilink")
- [Continuous Integration](SPM/CI "wikilink")

## Experimental Design for fMRI

- [Design efficiency](SPM/Design_efficiency "wikilink")
- [Block design](SPM/Block_design "wikilink")
- Event related design

## Data Formats

- [Importing data from the
  scanner](SPM/Importing_data_from_the_scanner "wikilink")
- [DICOM Import](SPM/DICOM "wikilink")

## Preprocessing

- [Slice Timing](SPM/Slice_Timing "wikilink")
- [Normalisation](SPM/Normalisation "wikilink")

## Modelling

- [Haemodynamic Response
  Function](SPM/Haemodynamic_Response_Function "wikilink")
- [Basis functions](SPM/Basis_functions "wikilink")
- [General Linear Model](w:General_linear_model "wikilink")
- [Correlation and
  Regression](SPM/Correlation_and_Regression "wikilink")
- [Covariance](SPM/Covariance "wikilink")
- [Autocorrelation](SPM/Autocorrelation "wikilink")
- [Non-sphericity](SPM/Non-sphericity "wikilink")
- [Session concatenation](SPM/Concatenation "wikilink")
- [Group analysis](SPM/Group_Analysis "wikilink")

## Statistical Inference

- [F and T tests](SPM/F_and_T_tests "wikilink")
- [Contrasts](SPM/Contrasts "wikilink")
- [Inference](SPM/Inference "wikilink")
- [Power Analysis](SPM/Power_Analysis "wikilink")
- [Correlation](SPM/Correlation "wikilink")
- [Information to include in
  papers](https://www.humanbrainmapping.org/i4a/pages/index.cfm?pageid=3728)
- Calculating [Percentage signal
  change](SPM/Percentage_signal_change "wikilink")
- [Comparing a single patient versus a group of
  controls](SPM/Single_Case_Study "wikilink")
- [Timeseries extraction](SPM/Timeseries_extraction "wikilink")

## Voxel Based Morphometry

- [VBM](SPM/VBM "wikilink") (Voxel Based Morphometry)

## Connectivity Analysis

- [Structural Equation Modelling](SPM/SEM "wikilink")
- [Psychophysiological Interactions (PPI)](SPM/PPI "wikilink")
- Dynamic Causal Modelling (DCM)
  - [/The DCM Equation. 1.
    Motivation/](/The_DCM_Equation._1._Motivation/ "wikilink")
  - [/The DCM Equation. 2. Dynamical
    Systems/](/The_DCM_Equation._2._Dynamical_Systems/ "wikilink")
  - [/The DCM Equation. 3. Networks and
    Matrices/](/The_DCM_Equation._3._Networks_and_Matrices/ "wikilink")
  - [/The DCM Equation. 4. The State
    Equation/](/The_DCM_Equation._4._The_State_Equation/ "wikilink")
  - [Two-State DCM for fMRI](/Two_State_DCM "wikilink")
  - [/Bayesian Parameter Averaging
    (BPA)/](/Bayesian_Parameter_Averaging_(BPA)/ "wikilink")
  - [/Parametric Empirical Bayes
    (PEB)/](/Parametric_Empirical_Bayes_(PEB)/ "wikilink")

## Misc

- [How-tos](SPM/How-to "wikilink")
- [Atlases](SPM/Atlases "wikilink")
- [Datasets](SPM/Datasets "wikilink")
- [BIDS](SPM/BIDS "wikilink")
- [Working with 4D data](SPM/Working_with_4D_data "wikilink")
- [Programming intro](SPM/Programming_intro "wikilink")
- [Writing batch scripts](SPM/Batch "wikilink")
- [No Display Mode](SPM/Headless "wikilink")
- [Learning SPM](SPM/Learning_SPM "wikilink"): Courses, books and
  websites

## Other tools

- [Cogent](http://www.vislab.ucl.ac.uk/cogent.php) a MATLAB-based
  stimulus presentation software
- [MRIcron](http://www.mccauslandcenter.sc.edu/mricro/mricron/) and
  [MRIcroGL](http://www.mccauslandcenter.sc.edu/mricrogl/) medical
  images viewers
- [MarsBaR](http://marsbar.sourceforge.net/) region of interest toolbox
  for SPM
- [SnPM](http://warwick.ac.uk/snpm) Statistical nonParametric Mapping
- [Anatomy](http://www.fz-juelich.de/ime/spm_anatomy_toolbox) SPM
  Anatomy toolbox
- [AAL](http://www.gin.cnrs.fr/AAL) Anatomical Automatic Labeling
- [WFU_PickAtlas](http://fmri.wfubmc.edu/software/PickAtlas) a region of
  interest toolbox for SPM based on the [Talairach Daemon
  Database](http://www.talairach.org/)
- [Physiological noise correction](SPM/Physio "wikilink")
- [Diffusion tools](SPM/DTI "wikilink")

## The physics of imaging technologies

- [functional Magnetic Resonance Imaging](w:fMRI "wikilink") (fMRI)
- [Positron Emission
  Tomography](w:Positron_Emission_Tomography "wikilink") (PET)
- [Single Photon Emission Computed
  Tomography](w:Single_photon_emission_computed_tomography "wikilink")
  (SPECT)
- [Electroencephalography](w:Electroencephalography "wikilink") (EEG)
- [Magnetoencephalography](w:Magnetoencephalography "wikilink") (MEG)
