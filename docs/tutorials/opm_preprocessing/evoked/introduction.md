# Evoked response pipeline

## Introduction

This page goes through an example preprocessing pipeline for a sensor-level evoked response analysis. 

## Data import 

Currently SPM supports a file format that has the raw data stored in a binary file and all the metadata stored in `TSV` and `JSON` files. This is designed to be as aligned as possible to the BIDS file structure. Data from Cerca Magnetics is stored in a similar format and we will analyse such a dataset in this tutorial. The necessary input arguments are the path to the binary file(`S.data`) and the path to the `TSV` file that contains the position information(`S.positions`)

```matlab
S = [];
S.data ='20230417_114328_meg_001.cMEG';
S.positions= '20230417_114328_HelmConfig.tsv';
D = spm_opm_create(S);

```

!!! note
    If you would like your file format to be supported by SPM please [open an issue on GitHub](https://github.com/spm/spm/issues).

## Power Spectral Density

Once OPM data is imported the first step should be to look at the PSD (power spectral density). This plot tells us if any channels are deviating notably from their manufacturer specifications or are clear outlier channels. 


```matlab
S=[];
S.triallength = 3000; 
S.plot=1;
S.D=D;
S.channels='MEG';
spm_opm_psd(S);
ylim([1,1e5])
```

<figure markdown>
  <div class="center">
    <img src="../../../../assets/figures/opm/raw_psd.png" style="width:160mm" />
  </div>
  <figcaption>Power Spectral Density</figcaption>
</figure>


## Filtering 

We can also filter our data to remove very low frequency interference and very high frequency interference. 
The key arguments are `S.freq` which sets the cut-off frequency and `S.band` which sets the type of filter (high-pass or low-pass)

```matlab
S=[];
S.D=cD;
S.freq=[10];
S.band = 'high';
fD = spm_eeg_ffilter(S);

S = [];
S.D = fD;
S.freq = [70];
S.band = 'low';
fD = spm_eeg_ffilter(S);

```
We can also optionally apply a stop-band filter for removing the line noise by changing the `S.band` argument but we won't run that here. 

```matlab
S = [];
S.D = fD;
S.freq = [48,52];
S.band = 'stop';
fD = spm_eeg_ffilter(S);
```


## Harmonic Models of OPM data

While OPM sensors are sensitive to brain signal they are also incredibly sensitive to environmental interference. Unlike modern MEG systems OPMs come with no built-in interference rejection. As such, all interference must be removed post-hoc using software. Here we will review some principled approaches to removing environmental interference based on solutions to Laplace's equation. 

### Spatial models of interference

By solving Laplace's equation in spherical coordinates we can model OPM interference as a linear combination of vector spherical harmonics. The key arguments are the M/EEG object argument `S.D` and the order argument `S.L`. The order argument reflects how complicated the model of interference is. The number of regressors used in the interference model scales as `S.L^2+2*S.L`. As such, for this method it is recommended to have many more channels than this number. It is generally not recommended to increase the order above `S.L=1` for radial samplings of the magnetic field. It is strongly recommended that higher orders are only used for multi-axis OPM systems.

```matlab
S = [];
S.D = fD;
S.L = 1;
[hfD] = spm_opm_hfc(S);
``` 
### Spatial models of interference and brain signal

If you have an OPM array with more than 120 channels it is possible to not only fit a model of the interference but also a compact model of the brain signal using spheroidal harmonics. The advantage of fitting this additional brain signal  model is that as you spatially oversample the data the white noise component of your data will reduce increasing the SNR of the data. Fitting the brain signal model also helps minimise the impact of interference that may be common to just a few channels. 

```matlab
S = [];
S.D = fD;
mD = spm_opm_amm(S);

``` 
### Spatio-temporal models of interference and brain signal 

If the source of magnetic interference is quite near the array then the low order spatial models will not be able to remove the interference. This problem is further exacerbated by the presence of sensor non-linearities such as calibration and orientation errors. This kind of interference can be identified using a temporal subspace intersection (implemented using CCA). This step requires setting the argument `S.corrLim` which is the the threshold(value between 0 and 1) at which one considers two subspaces to have intersected. 

??? info "How does subspace intersection work?" 
	`spm_opm_amm` defines 3 subspaces. A brain space using internal spheroidal harmonics (of default order `S.li=9`), an interference space using external spheroidal harmonics (of default order `S.li=2`) and an intermediate space that contains the data that is not well modelled by the internal or external harmonics. If the origin of magnetic interference is spatially close by it will not be well modelled by the external harmonics and will be present in the intermediate space. However, due to partial spatial correlation between complex interference and brain signal part of this interference will also be present in the brain signal space. If we assume that over long time scales (5-10s) brain signals(as observed by OPMs) are poorly temporally correlated any high temporal correlations (temporal subspace intersection) between the brain space and the intermediate space are most likely a reflection of magnetic interference. This interference is then readily removed using linear regression. 


```matlab
S = [];
S.D = fD;
S.corrLim = .98;
mD = spm_opm_amm(S);
``` 
??? info "How do I pick a value for S.corrLim?"
	Setting a lower value for `S.corrLim` will result in more aggressive cleaning of the data but will increase the risk of removing interesting brain signal. The factors that influence this decision are the order of the internal harmonics (`S.li`), the SNR of the brain response, the time scale at which physiological correlations exist, the length of the time window analysed and the number of channels in your array. As such it is not possible to extrapolate any heuristics one might have developed from using other temporal subspace intersection methods such as TSSS. However, the code has been developed so that  values greater than `0.98` are generally safe for arrays of less than 200 channels. 


!!! note "How do I choose which model to use?"
	1. If you have less than 120 channels use `spm_opm_hfc`.
	2. If you have radial only samplings do not increase `S.L` beyond 1.
	3. If you have more than 120 channels use `spm_opm_amm`.
	4. If you have more than 120 channels and have spatially complex interference  use `spm_opm_amm` with `S.corrLim` set between `0.95` and `1`.
	
## Examining processed data 

We can now also examine what the impact of our preprocessing has been by creating another PSD. 

```matlab
S=[];
S.triallength = 3000; 
S.plot=1;
S.D=mD;
S.channels=chans;
[~,freq]=spm_opm_psd(S);
ylim([1,1e5])
```

<figure markdown>
  <div class="center">
    <img src="../../../../assets/figures/opm/processed_psd.png" style="width:160mm" />
  </div>
  <figcaption>processed PSD</figcaption>
</figure>


	

If you can spot any channels in the PSD that are behaving differently to other channels we can interactively identify them by clicking on the plot. We can then set the bad channels with the following code. 

<figure markdown>
  <div class="center">
    <img src="../../../../assets/figures/opm/badchannel_psd.png" style="width:160mm" />
  </div>
  <figcaption>Bad Channels</figcaption>
</figure>

As all channels appear to be behaving fairly similarly we won't mark any as bad but the code should you wish to do so is below.
```matlab 
mD = badchannels(mD,[88],1);
mD.save();
```
!!! note
    If you change an M/EEG object(`D`) you should call the `save` method to make your changes permanent.


	
## Epoching and baseline correction
Now that we have modelled the interference in our data we can cut our data up into the time periods we are interested in. To do this we require that a trigger channel exists in our data set  when some stimuli was presented to a participant. In this case we are interested in the 50ms before the stimuli up until 200ms after the stimuli. This time window is specified with the `S.timewin` argument. The channel that encodes when the stimuli is presented is channel `Trigger 6 Z` and we can specify this with the `S.triggerChannels` argument.

```matlab
S =[];
S.D=mD;
S.timewin=[-50 200];
S.triggerChannels ={'Trigger 6 Z'};
eD= spm_opm_epoch_trigger(S);

S=[];
S.D = eD;
S.timewin = [-50 0];
eD = spm_eeg_bc(S);
``` 
!!! note
    SPM automatically baseline corrects data that has a negative time window. if you do not have a negative time window you will need to do baseline correction separately( see `spm_eeg_bc`).

## Averaging

We can now average across our trials and compute our t-statistic. The function `spm_eeg_average` has 1 key argument: `S.D`. We can then plot the average response for all channels trivially. 

```matlab
S=[];
S.D=eD;
muD = spm_eeg_average(S);


MEGind = indchantype(eD,'MEGMAG');
used = setdiff(MEGind,badchannels(muD));
pl =muD(used,:,:)';
figure();
plot(muD.time(),pl)
xlabel('Time (s)')
ylabel('B (fT)')
grid on
ax = gca; % current axes
ax.FontSize = 13;
ax.TickLength = [0.02 0.02];
fig= gcf;
fig.Color=[1,1,1];
xlim([0,.250])
```
<figure markdown>
  <div class="center">
    <img src="../../../../assets/figures/opm/evoked_field.png" style="width:160mm" />
  </div>
  <figcaption>Evoked field</figcaption>
</figure>



## Topoplots

Displaying OPM topographies is non trivial for 2 reasons:
  1. For small channel arrays 2D representations of 3D may not look very good.
  2. OPMs measure multiple components of the magnetic field at the same point in space and the sampled  field pattern is not necessarily smooth. 
  
 As such for now we will only display the radial component of the magnetic field measurement(which is a smooth interpretable field). We can do this with `spm_opm_plotScalpData` and we will initially look at the peak around 70ms. 
  
!!! note
    If you think OPM scalp data should be displayed differently please [open an issue on GitHub](https://github.com/spm/spm/issues).

```matlab

S=[];
S.D = muD;
S.T=.066
%S.display='NORM';
spm_opm_plotScalpData(S);
caxis([-150, 150])
``` 
<figure markdown>
  <div class="center">
    <img src="../../../../assets/figures/opm/90ms.png" style="width:160mm" />
  </div>
  <figcaption>Radial evoked field pattern</figcaption>
</figure>

--8<-- "addons/abbreviations.md"
