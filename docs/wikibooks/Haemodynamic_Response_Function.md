Generally speaking, cognitive processing is associated with increases in
neuronal firing rates. The increased neural activity leads to increased
metabolic requirements for the neurons. The onset of neural activity
leads to a systematic series of physiological changes in the local
network of blood vessels that include changes in the cerebral blood
volume per unit of brain tissue (CBV), changes in the rate of cerebral
blood flow (CBF), and changes in the concentration of oxyhaemoglobin and
deoxyhaemoglobin.

There are different fMRI techniques that can pick up a functional signal
corresponding to changes in each of the previously mentioned components
of the haemodynamic response. The most common functional imaging signal
is the Blood Oxygenation Level Dependent signal (BOLD), which primarily
corresponds to the concentration of deoxyhaemoglobin. In simple terms,
the magnetic resonance signal comes from exciting hydrogen nuclei with a
radiofrequency pulse, and detecting the radio waves emitted as the
nuclei return to a lower-energy configuration. Deoxyhaemoglobin has
different magnetic properties than oxyhaemoglobin - it is paramagnetic,
which means that it will make the local magnetic field over a
microscopic domain inhomogeneous. This has the effect of dephasing the
signal emitted by the nuclei in this domain, causing destructive
interference in the observed MR signal. Over a macroscopic domain (i.e.,
one functional voxel) greater amounts of deoxyhaemoglobin lead to less
signal. The functional BOLD signal is seen as an increase in the MR
signal that corresponds to a decrease in the concentration of
deoxyhaemoglobin. The decrease of deoxy-Hb concentration is seen because
the increase in CBF following neural activity more than accounts for the
effect of increased uptake of oxygen.

<figure markdown>
<div class="center">
<img src="../../../assets/figures/wikibooks/SPM_hemodynamic_response_function.png" />
</div>
<figcaption>SPM Haemodynamic Response Function</figcaption>
</figure>

For the purposes of estimating the BOLD signal in an experimental
paradigm, SPM makes use of a canonical haemodynamic response function
(HRF). This function is assumed to be the response of the system (as
reflected by the MR signal) to a brief, intense period of neural
stimulation. The SPM HRF is shown in the graph on the right, and
exhibits a rise peaking at 5 sec, followed by an undershoot that
persists for a considerable period (with a peak around 15 sec). The code
for this graph is below.

`>> RT = 0.5; hrf = spm_hrf(RT); plot(0:RT:32, hrf);`

In this graph, the y-axis is in arbitrary units. A common way to plot
the impulse response is in units of percent signal change from a
baseline condition. A very robust stimulus (such as a contrast taken
between a flickering visual stimulus and no visual stimulus) may produce
changes on the order of 2%-4% in the BOLD signal. The change observed in
contrasts involving higher-level cognitive processes is typically much
smaller.
