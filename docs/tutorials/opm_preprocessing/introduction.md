# OPM data preprocessing: Sensor level evoked responses

## Introduction

OPM data is inherently noisy and in order to make sense of it, we first have to clean it. The process of cleaning OPM data is referred to as **preprocessing** and its goal is to minimise the impact of variables unrelated to your experimental protocol on the data. These extraneous variables typically include participant's head movements, equipment-related artefacts and physiological processes such as breathing and heartbeats. In simple terms, preprocessing aims to separate the neuronal signal from other signals acquired during the experiment.

This page goes through an example preprocssing pipeline for a sensor-level evoked repsonse analysis. 


## Data import 

Currently SPM supports a file format that has the raw data stored in a binary file and all the metadata stored in 'TSV' and '.json' files. This is designed to be as aligned as possible to the BIDS file structure. Data from Cerca Magnetics is stored in a similar format and we will analyse such a dataset in this tutorial. The necesasry input arguments are the path to the binary file(`S.data`) and the path to the TSV file that contains the position information(`S.positions`)

```matlab
S = [];
S.data ='20230320_115853_meg_001.cMEG';
S.positions= '20230320_115853_HelmConfig.tsv';
D = spm_opm_create(S);
```

!!! note
    If you would like your file format to be supported by SPM please open an issue on Github

## Identifying bad channels

Once OPM data is imporated the first step should be to identify any bad channels by looking at the PSD (power spectral density). This plot tells us if any channels are deviating notably from their manafacturer specifications or are clear outlier channels. 

```matlab
S=[];
S.D=D;
S.triallength = 3000; % time in ms: longer windows provide more frequency resolution but are noisier
S.plot=1;
S.channels='MEG';     % select only MEG channels
spm_opm_psd(S);
ylim([1,1e5])         % set y axis limits between 1fT and 100,000 fT
```

We can now interactively identify the badchannels by clicking on the plot. We can then set then set the badchannels with the following code. 

```matlab 
D = badchannels(D,[51,52],1);
D.save();
```
!!! note
    If you change an M/EEG object(`D`) you should call the `save` method to make your changes permenant.

## Filtering 

We can also filter our data to remove very low frequency interference and very high frequency interference. 
The key arguments are `S.freq` which set the cutoff frequency and `S.band` which sets the tpye of filter(high-pass or low-pass)

```matlab
S=[];
S.D=D;
S.freq=[2];
S.band = 'high';
fD = spm_eeg_ffilter(S);

S=[];
S.D=fD;
S.freq=[70];
S.band = 'low';
fD = spm_eeg_ffilter(S);

```
We can also optionally apply a stop-band filter for removing the line noise by changing the `S.band` argument

```matlab
S=[];
S.D=fD;
S.freq=[48,52];
S.band = 'stop';
fD = spm_eeg_ffilter(S);
```


## Harmonic Models of OPM data


```matlab

``` 

## Epoching

```matlab

``` 

## Averaging 

```matlab

``` 

## Topoplots

```matlab

``` 


--8<-- "addons/abbreviations.md"