# fMRI data preprocessing

## Introduction to fMRI preprocessing

fMRI data is inherently noisy and in order to make sense of it, we first have to clean it. The process of cleaning fMRI data is referred to as **preprocessing** and its goal is to minimise the impact of variables unrelated to your imaging protocol on the data. These extraneous variables typically include participant's head movements, individual differences in brain anatomy, scanner or equipment-related artefacts, physiological processes such as breathing and heartbeats. In simple terms, preprocessing aims to separate the BOLD signal from other signals acquired during scanning.

This page goes through an example pipeline for fMRI data preprocessing. 

!!! attention "Customising your pipeline"
    Although some steps of fMRI preprocessing will always be used, other steps may vary. This is dependent on the way your data was acquired and/or the type of analysis you plan to perform. Additionally, the parameters used in each processing step should be tailored to the acquisition parameters of your dataset. The tutorial presented here shows an example pipeline and should not be used as a template but rather as a guide. 

    **If you're not sure of the preprocessing steps or parameters appropriate for your data, please consult an experienced fMRI analyst.**

    | Always used       | Sometimes used           |
    | :---------------: | :----------------------: |
    | Realignment       | Removal of dummy scans   |
    | Coregistration    | Slice timing correction  |
    | Normalisation     | Segmentation             |
    |                   | Smoothing                |
