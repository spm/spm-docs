
## Data
The entire original dataset is from 19 participants (8F & 11M, ages 23-37 years) and described in the paper by Wakeman et al (2015).
This tutorial only uses a subset of the original data.
Originally, there were nine runs of fMRI for each participant, but we will use only the first two.

For subject `XX`, the data you should have are:
- `sub-XX/anat/sub-XX-T1w.nii`. A T1-weighted MPRAGE MRI scan (TR 2,250 ms, TE 2.98 ms, TI 900 ms, 190 Hz/pixel; flip angle 9°) of 1 mm isotropic resolution.
- `sub-XX/func/sub-XX_ses-mri_task-facerecognition_run-01_bold.nii`. The first run of functional data, acquired using an EPI sequence (TR 2000 ms, TE 30 ms, flip angle 78°), with 208 volumes.  Slices were acquired interleaved, with odd then even numbered slices (where slice 1 was the most inferior slice).
- `sub-XX/func/sub-XX_ses-mri_task-facerecognition_run-01_events.nii`. A MATLAB file containing the onset times of the three types of stimuli for the first fMRI run. The file contains three variables: `famous`, `unfamiliar` and `scrambled`.
- `sub-XX/func/sub-XX_ses-mri_task-facerecognition_run-02_bold.nii`. The second run of functional data, acquired using an EPI sequence, with 208 volumes.
- `sub-XX/func/sub-XX_ses-mri_task-facerecognition_run-02_events.nii`. A MATLAB file containing the onset times of the three types of stimuli for the second fMRI run.

## Experimental design
For the sake of this analysis, three types of stimuli (trial-types) were used
* **Famous** Faces. Grey-scale photographs of people the participants are likely to recognise.
* **Unfamiliar** Faces. Grey-scale photographs of random people, but with similar properties to the famous faces.
* **Scrambled** Faces. Essentially just slightly smooth random noise, but with some properties similar to that of the face data.

Participants saw the pictures projected onto a screen in front of them (subtending horizontal and vertical angles of 3.66° and 5.38°), against a black background and with a white fixation cross in the center.
Between stimuli, participants saw a central white circle.
The fixation cross appeared between 0.4 and 0.6 seconds before each picture, and these were shown for between 0.8 and 1 seconds.

Participants were told to fixate on the circle or cross throughout the fMRI runs, and not to blink while the cross-hairs or pictures were visible.
To sustain participants' attention, they were asked to press a key with either their left or right index finger, depending on whether they regarded each image as more or less bilaterally symmetric than average.
.

## Analysis
Start up MATLAB and type `spm fmri` at the
MATLAB prompt. SPM will then open in fMRI mode with three windows (1)
the top-left or "Menu" window, (2) the bottom-left or "Interactive"
window and (3) the right-hand or "Graphics" window. Analysis then takes
place in three major stages (i) spatial pre-processing, (ii) model
specification, review and estimation and (iii) inference. These stages
organise the buttons in SPM's base window.

<figure>
<div class="center">
<img src="../../../assets/figures/manual/faces/command.png" style="width:100mm" />
</div>
<figcaption>The SPM base window comprises three sections (i) spatial
pre-processing, (ii) model specification, review and estimation and
(iii) inference.</figcaption>
</figure>

!!! info "Reference"
    [Wakeman, D., Henson, R. A multi-subject, multi-modal human neuroimaging dataset. Sci Data 2, 150001 (2015).](https://doi.org/10.1038/sdata.2015.1)

--8<-- "addons/abbreviations.md"
