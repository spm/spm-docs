# fMRI data preprocessing

## Introduction to fMRI preprocessing

fMRI data is inherently noisy and in order to make sense of it, we first have to clean it. The process of cleaning fMRI data is referred to as **preprocessing** and its goal is to minimise the impact of variables unrelated to your imaging protocol on the data. These extraneous variables typically include participant's head movements, individual differences in brain anatomy, scanner or equipment-related artefacts, physiological processes such as breathing and heartbeats. In simple terms, preprocessing aims to separate the BOLD signal from other signals acquired during scanning.

This page goes through an example pipeline for fMRI data preprocessing. 

!!! attention "Customising your pipeline"
    Although some steps of fMRI preprocessing will always be used, other steps may vary. This is dependednt on the way your data was acquired and/or the type of analysis you plan to perform. Additionally, the parameters used in each processing step should be tailored to the acquisition parameters of your dataset. The tutorial presented here shows an example pipeline and should not be used as a template but rather as a guide. 

    **If you're not sure of the preprocessing steps or parameters appropriate for your data, please consult an experienced fMRI analyst.**

    | Always used       | Sometimes used           |
    | :---------------: | :----------------------: |
    | Realignment       | Removal of dummy scans   |
    | Coregistration    | Slice timing correction  |
    | Normalisation     | Segmentation             |
    |                   | Smoothing                |

## About the data

The dataset used in this tutorial can be [downloaded from here](https://www.fil.ion.ucl.ac.uk/spm/download/data/MoAEpilot/MoAEpilot.bids.zip). 

The data consists of 96 volumes (TR = 7s, 64 contiguous slices, 3×3×3mm voxels) from a single participant, in blocks of six, giving sixteen 42s blocks. Due to the lack of lead-in dummy scans, the first 12 volumes have been discarded leaving 84 volumes. 

The condition for successive blocks alternated between rest and auditory stimulation, starting with rest. Auditory stimulation was bi-syllabic words presented binaurally at a rate of 60 per minute. 

The functional data `sub-01_task-auditory_bold.nii` and task onsets `sub-01_task-auditory_events.tsv` are stored in `sub-01/func`. Accompanying structural image `sub-01_T1w.nii` is stored in `sub-01/anat`. 

!!! info "A bit more background about the data"
    This experiment was conducted by [Geraint Rees](https://www.fil.ion.ucl.ac.uk/~grees/) under the direction of [Karl Friston](https://www.fil.ion.ucl.ac.uk/~karl/) and the [FIL Methods Group](https://www.fil.ion.ucl.ac.uk/Friston/). The purpose was to explore equipment and techniques in the early days of our fMRI experience. As such, it has not been formally written up and is freely available for personal education and evaluation purposes. 

    This dataset was the first ever collected and analysed in the Functional Imaging Laboratory (FIL) and is known locally as the Mother of All Experiments (MoAE). This dataset comprises whole brain BOLD/EPI images acquired on a modified 2T Siemens MAGNETOM Vision system. 


--8<-- "addons/abbreviations.md"