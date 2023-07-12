# Multimodal, Multisubject data fusion 

## Overview

This dataset contains EEG, MEG, functional MRI and structural MRI data
from 16 subjects who undertook multiple runs of a simple task performed
on a large number of Famous, Unfamiliar and Scrambled faces. It will be
used to demonstrate:

1.  batching and scripting of preprocessing of multiple subjects/runs of
    combined MEG and EEG data,

2.  creation of trial-averaged evoked responses,

3.  3D scalp-time statistical mapping of evoked responses across trials
    within one subject,

4.  2D time-frequency statistical mapping of time-frequency data across
    subjects,

5.  preprocessing and group analysis of fMRI data from the same subjects
    and paradigm,

6.  source-reconstruction of the "N/M170" face component (using
    structural MRI for forward modelling),

7.  individual and group-based fusion of EEG and MEG during source
    reconstruction,

8.  statistical mapping across subjects of cortical power in a
    time-frequency window, using the functional MRI results as spatial
    priors.

For the formal basis of these steps, see SPM publications, most
specifically [Henson et al. (2011)](https://doi.org/10.3389/fnhum.2011.00076).

The raw data can be found here (see `README.txt` there for more
details):

- <ftp://ftp.mrc-cbu.cam.ac.uk/personal/rik.henson/wakemandg_hensonrn/>

??? tip "Data acquisition"

    The MEG data consist of 102 magnetometers and 204 planar gradiometers
    from an Elekta VectorView system. The same system was used to
    simultaneously record EEG data from 70 electrodes (using a nose
    reference), which are stored in the same "FIF" format file. The above
    FTP site includes a raw FIF file for each run/subject, but also a second
    FIF file in which the MEG data have been "cleaned" using Signal-Space
    Separation as implemented in MaxFilter 2.1. We use the latter here. A
    Polhemus digitizer was used to digitise three fiducial points and a
    large number of other points across the scalp, which can be used to
    coregister the M/EEG data with the structural MRI image. Six runs
    (sessions) of approximately 10mins were acquired for each subject, while
    they judged the left-right symmetry of each stimulus (face or
    scrambled), leading to nearly 300 trials in total for each of the 3
    conditions.

    The MRI data were acquired on a 3T Siemens TIM Trio, and include a
    1$\times$<!-- -->1$\times$<!-- -->1mm $T_1$-weighted structural MRI
    (sMRI) as well as a large number of
    3$\times$<!-- -->3$\times$$\sim$<!-- -->4mm $T^*_2$-weighted functional
    MRI (fMRI) EPI volumes acquired during 9 runs of the same task
    (performed by same subjects with different set of stimuli on a separate
    visit). (The FTP site also contains ME-FLASH data from the same
    subjects, plus DWI data from a subset, which could be used for improved
    head modelling for example, but these are not used here.) For full
    description of the data and paradigm, see [Wakeman and Henson
    (2015)](https://doi.org/10.1038/sdata.2015.1).

Versions of the SPM12 batch job files and scripts used in this tutorial
can be found here:

- <ftp://ftp.mrc-cbu.cam.ac.uk/personal/rik.henson/SPMScripts/>

!!! note

    It should be noted that the pipeline described below is just one
    possible sequence of processing steps, designed to illustrate some of
    the options available in SPM12. It is not necessarily the optimal
    preprocessing sequence, which really depends on the question being asked
    of the data.

## Getting Started

Download the data from above FTP site. There are over 100GB of data in
total, so you can start by just downloading one subject (e.g, Subject 15
that is used in the first demo below), and perhaps just their `MEEG`
sub-directory initially (though you will need the `T1` and `BOLD`
sub-directories later)[^3]. Within the `MEEG` sub-directory, you will
need all the MaxFiltered files (`run_0[1-6]_sss.fif`), the
`bad_channels.mat` file and the `Trials` sub-directory. It will be much
easier if you maintain this directory structure for your copy of the
data.

Open SPM12 and ensure it is set to the EEG modality. To do this, type
`spm eeg` into the MATLAB command window. For this to work, SPM12 root
folder must be in your MATLAB path.

Open the batch editor window by pressing ++"Batch"++ from the SPM Menu
window. This opens the window shown below.

<figure>
<div class="center">
<img src="../../../../assets/figures/manual/multi/figure1.png" style="width:120mm" />
</div>
<figcaption><em>Screenshot of the Batch Editor. The Module List, Current
Module Window and Current Item Windows are identified. The cursor
highlights how to save the pipeline as a batch and script.</em></figcaption>
</figure>

--8<-- "addons/abbreviations.md"
