# OPM data preprocessing

## Introduction to OPM preprocessing

OPM data is inherently noisy and in order to make sense of it, we first have to clean it. The process of cleaning OPM data is referred to as **preprocessing** and its goal is to minimise the impact of variables unrelated to your experimental protocol on the data. These extraneous variables typically include participant's head movements, equipment-related artefacts and physiological processes such as breathing and heartbeats. In simple terms, preprocessing aims to separate the neuronal signal from other signals acquired during the experiment.

This page goes through an example pipeline for OPM data preprocessing.


## Data import 

Curently SPM supports a file format that has the raw data stored in a binary file and all the metadata stored in 'TSV' and '.json' files. This is desiged to be as alligned as possible to the BIDS file sructure. Cerca magnetics data is stored in a simlar format and we will analyse such a dataset in this tutorial.


!!! note
    If yo would like your file format to be supported by SPM please open an issue on Github

## Identifying bad channels 

## Filtering 

## Harmonic Models of OPM data

## Epoching 

## Averaging 

## Topoplots




--8<-- "addons/abbreviations.md"