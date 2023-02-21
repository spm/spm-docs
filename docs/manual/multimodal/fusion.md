# Multimodal fusion<span id="multimodal:fusion" label="multimodal:fusion"></span>

Here, we will illustrate here two types of multimodal integration:

1.  "Fusion" of the EEG and MEG data (Henson et al, 2009b),

2.  Use of the fMRI data as spatial priors on the MEG/EEG data (Henson
    et al, in press).

## EEG and MEG fusion <span id="multimodal:fusion:eegmeg:fusion" label="multimodal:fusion:eegmeg:fusion"></span>

Make a new directory called "Fused', and change into it.

### Merging the EEG and MEG datafiles <span id="multimodal:fusion:eegmeg:merge" label="multimodal:fusion:eegmeg:merge"></span>

The first step is to combine the MEG and EEG data into a single SPM
file. We will use the (weighted) averaged files for each modality.

Press "Fuse" from the "Others" pulldown menu, and select the
`wmcdbespm8_SPM_CTF_MEG_example_faces1_3D.mat` in the MEG directory and
the `wmaceMdspm8_faces_run1.mat` in the EEG directory. This will create
a new file called `uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D.mat` in the
new Fused directory. Note that the two files need to have the same
number of trials, conditions, samples, etc. Display the new file, and
you will see the EEG and MEG data within their respective tabs.

We have to do one extra bit of "preparation" for the EEG data. Because
in general, one might want to merge more than one EEG file, integrating
all their locations could be tricky. So the simple answer is to clear
all locations and force the user to re-specify them. So (as in earlier
EEG section), select <span class="smallcaps">Prepare</span> from the
"Other" menu and select `uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D.mat`.
Then in the SPM Interactive window, on the "Sensors" submenu, choose
"Load EEG sensors"/"Convert locations file", and select the
`electrode_locations_and_headshape.sfp` file (in the original EEG
directory). Then from the "2D projection" submenu select "Project 3D
(EEG)". A 2D channel layout will appear in the Graphics window. Select
"Apply" from "2D Projection" and "Save" from "File" submenu.

### 3D fused "imaging" reconstruction <span id="multimodal:fusion:eegmeg:3D" label="multimodal:fusion:eegmeg:3D"></span>

Now we can demonstrate simultaneous reconstruction of the MEG and EEG
data, as described in Henson et al (2009b). This essentially involves
scaling each type of data and gain matrix, concatenating them, and
inverting using the normal methods, but with separate sensor error
covariance terms for each modality.

- Press the "3D source reconstruction" button, and load the
  `uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D.mat` file. Type a label (eg
  "N/M170").

- Press the "MRI" button, select the `smri.img` file within the `sMRI`
  sub-directory, and select "normal" for the cortical mesh. Because this
  MRI was normalised previously, this step should not take long,
  finishing with display of the cortex (blue), inner skull (red), outer
  skull (orange) and scalp (pink) meshes.

- Press the "Co-register" button. Press "type" and for "nas", enter \[0
  91 -28\] for "lpa" press "type" and enter \[-72 4 -59\] for "rpa"
  press "type" and enter \[71 -6 -62\]. Finally, answer "no" to "Use
  headshape points?". Then select either "EEG" or "MEG" to display
  corresponding data registration. Note that the MEG data will have been
  coregistered to the EEG data in the headspace. If you want to display
  the other modality afterwards, just press the "display" button below
  the "Co-register" button.

- Press "Forward Model", and select "EEG BEM" for the EEG and "Local
  Spheres" for the MEG. Then select either "EEG" or "MEG" to display
  corresponding forward model. (If you want to display the other
  modality afterwards, just press the "display" button below the
  "Forward Model" button). In the Graphics window the meshes will be
  displayed with the sensors marked with green asterisks.

- Press "save" to save progress.

- Press "Invert", select "Imaging". Because the fMRI data (see below)
  already come from a contrast of faces versus scrambled faces, we will
  only invert the differential ERP/ERFs. So press "no" to the question
  about invert "all conditions or trials", press "yes" to invert the
  Difference (between faces and scrambled) but "no" to invert the Mean
  (of faces and scrambled versus baseline).

  Then press "Standard" to use the default inversion settings (MSP), and
  then to select both the "EEG" and "MEG" modalities in the new "Select
  modalities" window, in order to fuse them (simultaneously invert
  both).

  Lead fields will first be computed for all the mesh vertices and saved
  in the file
  `SPMgainmatrix_uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D_1.mat`. This
  will take some time. Then the actual MSP algorithm will run and the
  summary of the solution will be displayed in the Graphics window.

- Press "save" to save the results. You can now explore the results via
  the 3D reconstruction window. If you type 180 into the box in the
  bottom right (corresponding to the time in ms) and press "mip", you
  should see an output similar to
  Figure <a href="#multimodal:fusion:fig:1" data-reference-type="ref"
  data-reference="multimodal:fusion:fig:1">1</a>.

<figure id="multimodal:fusion:fig:1">
<div class="center">
<img src="../../../assets/figures/manual/multimodal/fused_eeg_meg_msp.png"
style="width:90mm" />
</div>
<figcaption><em>Graphical output of an MSP estimation of the
differential evoked response between faces and scrambled faces at 180ms,
after fusing both EEG and MEG data. <span id="multimodal:fusion:fig:1"
label="multimodal:fusion:fig:1"></span></em></figcaption>
</figure>

Note that because we have inverted only the differential ERP/ERF, these
results cannot be compared directly to the unimodal inversions in the
previous sections of this chapter. For a fairer comparison:

- Press the "new" button and type "N170" as the label, press "Invert"
  again (note that all forward models are copied by default from the
  first inversion) and select the same options as above, except that
  when asked "Select modalities", select just "EEG". This should produce
  the results in
  Figure <a href="#multimodal:fusion:fig:2" data-reference-type="ref"
  data-reference="multimodal:fusion:fig:2">2</a>. Notice the more
  posterior maxima.

  <figure id="multimodal:fusion:fig:2">
  <div class="center">
  <img src="../../../assets/figures/manual/multimodal/fused_eeg_msp.png" style="width:90mm" />
  </div>
  <figcaption><em>Graphical output of an MSP estimation of the
  differential evoked response between faces and scrambled faces at 180ms,
  after inverting just EEG data. <span id="multimodal:fusion:fig:2"
  label="multimodal:fusion:fig:2"></span></em></figcaption>
  </figure>

- Press the "new" button and type "M170" as the label, press "Invert"
  again and select the same options as above, except that when asked
  "Select modalities", select just "MEG" this time. This should produce
  the results in
  Figure <a href="#multimodal:fusion:fig:3" data-reference-type="ref"
  data-reference="multimodal:fusion:fig:3">3</a>. Notice the more
  anterior and medial activity.

  <figure id="multimodal:fusion:fig:3">
  <div class="center">
  <img src="../../../assets/figures/manual/multimodal/fused_meg_msp.png" style="width:90mm" />
  </div>
  <figcaption><em>Graphical output of an MSP estimation of the
  differential evoked response between faces and scrambled faces at 180ms,
  after inverting just MEG data. <span id="multimodal:fusion:fig:3"
  label="multimodal:fusion:fig:3"></span></em></figcaption>
  </figure>

By comparing these figures, you can see that the multimodal fused
inversion (first inversion) combines aspects of both the unimodal
inversions. Unfortunately one cannot simply compare the multi-modal vs
uni-modal reconstructions via the log-evidence, because the data differs
in each case (rather, one could use an estimate of the conditional
precision of the sources, as in Henson et al, 2009b). With multiple
subjects though, one could compare statistics across subjects using
either multimodal or unimodal inversions.

## EEG, MEG and fMRI fusion <span id="multimodal:fusion:fmri" label="multimodal:fusion:fmri"></span>

Now we can examine the effects of using the fMRI data in
Section <a href="#multimodal:data:fMRI" data-reference-type="ref"
data-reference="multimodal:data:fMRI">[multimodal:data:fMRI]</a> as
spatial priors on the sources (Henson et al, in press). But first we
need to create an 3D volumetric image of the clusters that we wish to
use as spatial priors. These clusters can be defined by thresholding an
SPM for a given fMRI contrast: here we will use the contrast in
Section <a href="#multimodal:data:fMRI" data-reference-type="ref"
data-reference="multimodal:data:fMRI">[multimodal:data:fMRI]</a> of
faces versus scrambled faces (using all three basis functions). So press
"Results" and select the "SPM.mat" file from the "fMRI/Stats" directory.
Select the previous faces vs scrambled F-contrast, do not mask or change
title, use FWE correction at 0.05 and a 60-voxel extent to reproduce the
SPM{F} shown in
Figure <a href="#multimodal:fig:22" data-reference-type="ref"
data-reference="multimodal:fig:22">[multimodal:fig:22]</a> (if you are
still in SPM's "EEG" mode, you will also be asked the type of images,
for which you reply "Volumetric 2D/3D").

Now press the "save" button in the Interactive window and enter a
filename like `FacesVsScrambled_FWE05_60`. This will produce a 3D image
(which you can display as normal) in which all subthreshold voxels are
set to zero (ie, where only 6 clusters containing non-zero voxel values
are left).

Now we can use this cluster image in a new inversion:

- Press the "new" button to create a fourth inversion, and type
  "N/M170+fMRI" as the label.

- Press "Invert", select "Imaging", press "no" for "all conditions or
  trials", and select only the Difference (not Mean), as before \...

- \... but this time, press "Custom" rather than "Standard" to get more
  flexibility in the inversion settings. Select "GS" for the type of
  inversion (the default MSP with a Greedy Search), enter default time
  window of "-200 to 600", "yes" to a Hanning window, "0" for the
  highpass and "48" for the lowpass, and then press "yes" to the
  question of "Source priors?"\...

- \... select the `FacesVsScrambled_FWE05_60.img` file in the
  "fMRI/Stats" directory, and select "MNI" for the "Image space"
  (because the fMRI images were spatially normalised).

- Say "No" to "Restrict solutions", and then select both the "EEG" and
  "MEG" modalities in the "Select modalities" window, in order to fuse
  them (together with the fMRI priors).

  Note that, once the inversion has finished, a new image will be
  created (in the "fMRI/Stats" directory) called
  `cluster_FacesVsScrambled_FWE05_60.img`, which contains the six binary
  priors, as will a GIfTI version called
  `priors_uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D_4.func.gii`. If you
  want to display each spatial prior on the cortical mesh, first make
  sure you save the current reconstruction, and then type in the MATLAB
  window:

        D = spm_eeg_load('uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D.mat');
        val = 4;    % Fourth inversion; assuming you have followed the above steps
        G = gifti(D.inv{val}.mesh.tess_mni);
        P = gifti(D.inv{val}.inverse.fmri.texture);
        for i=1:size(P.cdata,2);
          figure, plot(G,P,i);
        end

  Finally, a new MATLAB file called
  `priors_uwmcdbespm8_SPM_CTF_MEG_example_faces1_3D_4.mat` will also be
  saved to the current directory, which contains the information
  necessary to construct the covariance components for subsequent
  inversion. So if you want to use these fMRI priors in another
  inversion, next time you are prompted for the "Source priors?", rather
  than selecting an image ("img" file) as we did above, you can select
  this "mat" file, so SPM will not need to recreate the covariance
  matrices from the image file, but can use the covariance matrices
  directly.

- Again, type 180 into the box in the bottom right and press "mip". This
  should produce the output in
  Figure <a href="#multimodal:fusion:fig:4" data-reference-type="ref"
  data-reference="multimodal:fusion:fig:4">4</a>. Notice how the more
  posterior midfusion clusters (particularly on the left) have become
  stronger (where there were fMRI priors). (Note also that fMRI priors
  have generally been found to have a greater effect on IID or COH
  inversions, given the implicit flexibility of MSP priors, Henson et
  al, in press).

  <figure id="multimodal:fusion:fig:4">
  <div class="center">
  <img src="../../../assets/figures/manual/multimodal/fused_eeg_meg_msp_fmri.png"
  style="width:90mm" />
  </div>
  <figcaption><em>Graphical output of an MSP estimation of the
  differential evoked response between faces and scrambled faces at 180ms,
  after fusing EEG, MEG and fMRI data. <span id="multimodal:fusion:fig:4"
  label="multimodal:fusion:fig:4"></span></em></figcaption>
  </figure>

- Press "save".

You can repeat the above steps to use the common fMRI effect of faces
and scrambled faces versus baseline (though at a higher threshold
perhaps) as an alternative set of spatial priors for inverting either
the differential evoked MEG/EEG response, or the mean evoked MEG/EEG
response vs baseline.
