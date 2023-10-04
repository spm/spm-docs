# fMRI tutorial

## About the data

The dataset used in this tutorial can be [downloaded from here](https://www.fil.ion.ucl.ac.uk/spm/download/data/MoAEpilot/MoAEpilot.bids.zip). 

The data consists of 96 volumes (TR = 7s, 64 contiguous slices, 3$\times$3$\times$3mm voxels) from a single participant, in blocks of six, giving sixteen 42s blocks. Due to the lack of lead-in dummy scans, the first 12 volumes have been discarded leaving 84 volumes. 

The condition for successive blocks alternated between rest and auditory stimulation, starting with rest. Auditory stimulation was bi-syllabic words presented binaurally at a rate of 60 per minute. 

The functional data `sub-01_task-auditory_bold.nii` and task onsets `sub-01_task-auditory_events.tsv` are stored in `sub-01/func/`. Accompanying structural image `sub-01_T1w.nii` is stored in `sub-01/anat/`. 

!!! info "A bit more background about the data"
    This experiment was conducted by Geraint Rees under the direction of [Karl Friston](https://www.fil.ion.ucl.ac.uk/~karl/) and the [FIL Methods Group](https://www.fil.ion.ucl.ac.uk/method/modelling-and-analysis/). The purpose was to explore equipment and techniques in the early days of our fMRI experience. As such, it has not been formally written up and is freely available for personal education and evaluation purposes. 

    This dataset was the first ever collected and analysed in the Functional Imaging Laboratory (FIL) and is known locally as the Mother of All Experiments (MoAE). This dataset comprises whole brain BOLD/EPI images acquired on a modified 2T Siemens MAGNETOM Vision system. 


--8<-- "addons/abbreviations.md"
