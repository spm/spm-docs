# 3D source reconstruction: Imaging approach <span id="Chap:eeg:imaging" label="Chap:eeg:imaging"></span>

This chapter describes an Imaging approach to 3D source reconstruction.

## Introduction<span id="sec:imaginv_intro" label="sec:imaginv_intro"></span>

This chapter focuses on the imaging (or distributed) method for
implementing EEG/MEG source reconstruction in SPM. This approach results
in a spatial projection of sensor data into (3D) brain space and
considers brain activity as comprising a very large number of dipolar
sources spread over the cortical sheet, with fixed locations and
orientations. This renders the observation model linear, the unknown
variables being the source amplitudes or power.

Given epoched and preprocessed data (see chapter
<a href="#Chap:eeg:preprocessing" data-reference-type="ref"
data-reference="Chap:eeg:preprocessing">[Chap:eeg:preprocessing]</a>),
the evoked and/or induced activity for each dipolar source can be
estimated, for a single time-sample or a wider peristimulus time window.

The obtained reconstructed activity is in 3D voxel space and can be
further analyzed using mass-univariate analysis in SPM.

Contrary to PET/fMRI data reconstruction, EEG/MEG source reconstruction
is a non trivial operation. Often compared to estimating a body shape
from its shadow, inferring brain activity from scalp data is
mathematically ill-posed and requires prior information such as
anatomical, functional or mathematical constraints to isolate a unique
and most probable solution (Baillet, Mosher, and Leahy 2001).

Distributed linear models have been around for several decades now (Dale
and Sereno 1993) and the proposed pipeline in SPM for an imaging
solution is classical and very similar to common approaches in the
field. However, at least two aspects are quite original and should be
emphasized here:

- Based on an empirical Bayesian formalism, the inversion is meant to be
  generic in the sense it can incorporate and estimate the relevance of
  multiple constraints of varied nature; data-driven relevance
  estimation being made possible through Bayesian model
  comparison (Friston et al. 2002, 2005; Phillips et al. 2005; Mattout
  et al. 2006).

- The subject's specific anatomy is incorporated in the generative model
  of the data, in a fashion that eschews individual cortical surface
  extraction. The individual cortical mesh is obtained automatically
  from a canonical mesh in MNI space, providing a simple and efficient
  way of reporting results in stereotactic coordinates.

The EEG/MEG imaging pipeline is divided into four consecutive steps
which characterize any inverse procedure with an additional step of
summarizing the results. In this chapter, we go through each of the
steps that need completing when proceeding with a full inverse analysis:

1.  Source space modeling,

2.  Data co-registration,

3.  Forward computation,

4.  Inverse reconstruction.

5.  Summarizing the results of inverse reconstruction as an image.

Whereas the first three steps are part of the whole generative model,
the inverse reconstruction step consists in Bayesian inversion, and is
the only step involving actual EEG/MEG data.  

## Getting started

Everything which is described hereafter is accessible from the SPM
user-interface by choosing the "EEG" application,
`3D Source Reconstruction` button. When you press this button a new
window will appear with a GUI that will guide you through the necessary
steps to obtain an imaging reconstruction of your data. At each step,
the buttons that are not yet relevant for this step will be disabled.
When you open the window the only two buttons you can press are `Load`
which enables you to load a pre-processed SPM MEEG dataset and the
`Group inversion` button that will be described below. You can load a
dataset which is either epoched with single trials for different
conditions, averaged with one event related potential (ERP) per
condition, or grand-averaged. An important pre-condition for loading a
dataset is that it should contain sensors and fiducials. This will be
checked when you load a file and loading will fail in case of a problem.
You should make sure that for each modality present in the dataset as
indicated by channel types (either EEG or MEG) there is a sensor
description. If, for instance, you have an MEG dataset with some EEG
channels that you don't actually want to use for source reconstruction,
change their type to "*LFP*" or "*Other*" before trying to load the
dataset (the difference is that *LFP* channels will still be filtered
and available for artefact detection whereas *Other* channels won't).
MEG datasets converted by SPM from their raw formats will always contain
sensor and fiducial descriptions. In the case of EEG for some supported
channel setups (such as extended 10-20 or BioSemi) SPM will provide
default channel locations and fiducials that you can use for your
reconstruction. Sensor and fiducial descriptions can be modified using
the `Prepare` interface and in this interface you can also verify that
these descriptions are sensible by performing a coregistration (see
chapter <a href="#Chap:eeg:preprocessing" data-reference-type="ref"
data-reference="Chap:eeg:preprocessing">[Chap:eeg:preprocessing]</a> and
also below for more details about coregistration).

When you successfully load a dataset you are asked to give a name to the
present analysis cell. In SPM it is possible to perform multiple
reconstructions of the same dataset with different parameters. The
results of these reconstructions will be stored with the dataset if you
press the `Save` button. They can be loaded and reviewed again using the
3D GUI and also with the SPM EEG <span class="smallcaps">Review</span>
tool. From the command line you can access source reconstruction results
via the `D.inv` field of the `meeg` object. This field (if present) is a
cell array of structures and does not require methods to access and
modify it. Each cell contains the results of a different reconstruction.
In the GUI you can navigate between these cells using the buttons in the
second row. You can also create, delete and clear cells. The label you
input at the beginning will be attached to the cell for you to identify
it.

## Source space modeling

After entering the label you will see the `Template` and `MRI` button
enabled. The `MRI` button will create individual head meshes describing
the boundaries of different head compartments based on the subject's
structural scan. SPM will ask for the subject's structural image. It
might take some time to prepare the model as the image needs to be
segmented. The individual meshes are generated by applying the inverse
of the deformation field needed to normalize the individual structural
image to MNI template to canonical meshes derived from this template.
This method is more robust than deriving the meshes from the structural
image directly and can work even when the quality of the individual
structural images is low.

Presently we recommend the `Template` button for EEG and a head model
based on an individual structural scan for MEG. In the absence of
individual structural scan combining the template head model with the
individual headshape also results in a quite precise head model. The
`Template` button uses SPM's template head model based on the MNI brain.
The corresponding structural image can be found under
`canonical`$\backslash$`single_subj_T1.nii` in the SPM directory. When
you use the template, different things will happen depending on whether
your data is EEG or MEG. For EEG, your electrode positions will be
transformed to match the template head. So even if your subject's head
is quite different from the template, you should be able to get good
results. For MEG, the template head will be transformed to match the
fiducials and headshape that come with the MEG data. In this case having
a headshape measurement can be quite helpful in providing SPM with more
data to scale the head correctly. From the user's perspective the two
options will look quite similar.

No matter whether the `MRI` or `Template` button was used the cortical
mesh, which describes the locations of possible sources of EEG and MEG
signal, is obtained from a template mesh. In the case of EEG the mesh is
used as is, and in the case of MEG it is transformed with the head
model. Three cortical mesh sizes are available "coarse", "normal" and
"fine" (5124, 8196 and 20484 vertices respectively). It is advised to
work with the "normal" mesh. Choose "coarse" if your computer has
difficulties handling the "normal" option. "Fine" will only work on
64-bit systems and is probably an overkill.

## Coregistration

In order for SPM to provide a meaningful interpretation of the results
of source reconstruction, it should link the coordinate system in which
sensor positions are originally represented to the coordinate system of
a structural MRI image (MNI coordinates). In general, to link between
two coordinate systems you will need a set of at least 3 points whose
coordinates are known in both systems. This is a kind of *Rosetta stone*
that can be used to convert a position of any point from one system to
the other. These points are called "fiducials" and the process of
providing SPM with all the necessary information to create the *Rosetta
stone* for your data is called "coregistration".

There are two possible ways of coregistrating the EEG/MEG data into the
structural MRI space.

1.  A Landmark based coregistration (using fiducials only).  
    The rigid transformation matrices (Rotation and Translation) are
    computed such that they match each fiducial in the EEG/MEG space
    into the corresponding one in sMRI space. The same transformation is
    then applied to the sensor positions.

2.  Surface matching (between some headshape in MEG/EEG space and some
    sMRI derived scalp tessellation).  
    For EEG, the sensor locations can be used instead of the headshape.
    For MEG, the headshape is first coregistrated into sMRI space; the
    inverse transformation is then applied to the head model and the
    mesh.  
    Surface matching is performed using an Iterative Closest Point
    algorithm (ICP). The ICP algorithm (Besl and McKay 1992) is an
    iterative alignment algorithm that works in three phases:

    - Establish correspondence between pairs of features in the two
      structures that are to be aligned based on proximity;

    - Estimate the rigid transformation that best maps the first member
      of the pair onto the second;

    - Apply that transformation to all features in the first structure.
      These three steps are then reapplied until convergence is
      concluded. Although simple, the algorithm works quite effectively
      when given a good initial estimate.

In practice what you will need to do after pressing the `Coregister`
button is to specify the points in the sMRI image that correspond to
your M/EEG fiducials. If you have more fiducials (which may happen for
EEG as in principle any electrode can be used as a fiducial), you will
be ask at the first step to select the fiducials you want to use. You
can select more than 3, but not less. Then for each M/EEG fiducial you
selected you will be asked to specify the corresponding position in the
sMRI image in one of 3 ways.

- `select` - locations of some points such as the commonly used nasion
  and preauricular points and also CTF recommended fiducials for MEG (as
  used at the FIL) are hard-coded in SPM. If your fiducial corresponds
  to one of these points you can select this option and then select the
  correct point from a list.

- `type` - here you can enter the MRI coordinates in mm for your
  fiducial ($1 \times 3$ vector). If your fiducial is not on SPM's
  hard-coded list, it is advised to carefully find the right point on
  either the template image or on your subject's own image normalized to
  the template. You can do it by just opening the image using SPM's
  Display/images functionality. You can then record the MNI coordinates
  and use them in all coregistrations you need to do using the "type"
  option.

- `click` - here you will be presented with a structural image where you
  can click on the right point. This option is good for "quick and
  dirty" coregistration or to try out different options.

You will also have the option to skip the current fiducial, but remember
you can only do it if you eventually specify more than 3 fiducials in
total. Otherwise the coregistration will fail.

After you specify the fiducials you will be asked whether to use the
headshape points if they are available. For EEG it is advised to always
answer "yes". For MEG if you use a head model based on the subject's
sMRI and have precise information about the 3 fiducials (for instance by
doing a scan with fiducials marked by vitamin E capsules) using the
headshape might actually do more harm than good. In other cases it will
probably help, as in EEG.

The results of coregistration will be presented in SPM's graphics
window. It is important to examine the results carefully before
proceeding. In the top plot you will see the scalp, the inner skull and
the cortical mesh with the sensors and the fiducials. For EEG make sure
that the sensors are on the scalp surface. For MEG check that the head
position in relation to the sensors makes sense and the head does not
for instance stick outside the sensor array. In the bottom plot the
sensor labels will be shown in topographical array. Check that the top
labels correspond to anterior sensors, bottom to posterior, left to left
and right to right and also that the labels are where you would expect
them to be topographically.

## Forward computation (*forward*)

This refers to computing for each of the dipoles on the cortical mesh
the effect it would have on the sensors. The result is a $N \times M$
matrix where N is the number of sensors and M is the number of mesh
vertices (that you chose from several options at a previous step). This
matrix can be quite big and it is, therefore, not stored in the header,
but in a separate `*.mat` file which has `SPMgainmatrix` in its name and
is written in the same directory as the dataset. Each column in this
matrix is a so called "lead field" corresponding to one mesh vertex.

The lead fields are computed using the "forwinv" toolbox[^1] developed
by Robert Oostenveld, which SPM shares with FieldTrip. This computation
is based on Maxwell's equations and makes assumptions about the physical
properties of the head. There are different ways to specify these
assumptions which are known as "forward models".

The "forwinv" toolbox can support different kinds of forward models.
When you press `Forward Model` button (which should be enabled after
successful coregistration), you will have a choice of several head
models depending on the modality of your dataset. We presently recommend
using a single shell model for MEG and "EEG BEM" for EEG. You can also
try other options and compare them using model evidence (see below). The
first time you use the EEG BEM option with a new structural image (and
also the first time you use the `Template` option) a lengthy computation
will take place that prepares the BEM model based on the head meshes.
The BEM will then be saved in a quite large `*.mat` file with ending
`_EEG_BEM.mat` in the same directory with the structural image
("canonical" subdirectory of SPM for the template). When the head model
is ready, it will be displayed in the graphics window with the cortical
mesh and sensor locations you should verify for the final time that
everything fits well together.

The actual lead field matrix will be computed at the beginning of the
next step and saved. This is a time-consuming step and it takes longer
for high-resolution meshes. The lead field file will be used for all
subsequent inversions if you do not change the coregistration and the
forward model.

## Inverse reconstruction

To get started press the `Invert` button. The first choice you will see
is between `Imaging`, `VB-ECD` and `DCM`. For reconstruction based on an
empirical Bayesian approach to localize either the evoked response, the
evoked power or the induced power, as measured by EEG or MEG press the
`Imaging` button. The other options are explained in greater detail
elsewhere.

If you have trials belonging to more than one condition in your dataset
then the next choice you will have is whether to invert all the
conditions together or to choose a subset. It is recommended to invert
the conditions together if you are planning to later do a statistical
comparison between them. If you have only one condition, or after
choosing the conditions, you will get a choice between "Standard" and
"Custom" inversion. If you choose "Standard" inversion, SPM will start
the computation with default settings. These correspond to the multiple
sparse priors (MSP) algorithm (Friston et al. 2008) which is then
applied to the whole input data segment.

If you want to fine-tune the parameters of the inversion, choose the
"Custom" option. You will then have the possibility to choose between
several types of inversion differing by their hyperprior models (IID -
equivalent to classical minimum norm, COH - smoothness prior similar to
methods such as LORETA) or the MSP method .

You can then choose the time window that will be available for
inversion. Based on our experience, it is recommended to limit the time
window to the activity of interest in cases when the amplitude of this
activity is low compared to activity at other times. The reason is that
if the irrelevant high-amplitude activity is included, the source
reconstruction scheme will focus on reducing the error for
reconstructing this activity and might ignore the activity of interest.
In other cases, when the peak of interest is the strongest peak or is
comparable to other peaks in its amplitude, it might be better not to
limit the time window to let the algorithm model all the brain sources
generating the response and then to focus on the sources of interest
using the appropriate contrast (see below). There is also an option to
apply a hanning taper to the channel time series in order to downweight
the possible baseline noise at the beginning and end of the trial. There
is also an option to pre-filter the data. Finally, you can restrict
solutions to particular brain areas by loading a `*.mat` file with a
$K \times 3$ matrix containing MNI coordinates of the areas of interest.
This option may initially seem strange, as it may seem to overly bias
the source reconstructions returned. However, in the Bayesian inversion
framework you can compare different inversions of the same data using
Bayesian model comparison. By limiting the solutions to particular brain
areas you greatly simplify your model and if that simplification really
captures the sources generating the response, then the restricted model
will have much higher model evidence than the unrestricted one. If,
however, the sources you suggested cannot account for the data, the
restriction will result in a worse model fit and depending on how much
worse it is, the unrestricted model might be better in the comparison.
So using this option with subsequent model comparison is a way, for
instance, to integrate prior knowledge from the literature or from
fMRI/PET/DTI into your inversion. It also allows for comparison of
alternative prior models.

Note that for model comparison to be valid all the settings that affect
the input data, like the time window, conditions used and filtering
should be identical.

SPM imaging source reconstruction also supports multi-modal datasets.
These are datasets that have both EEG and MEG data from a simultaneous
recording. Datasets from the "Neuromag" MEG system which has two kinds
of MEG sensors are also treated as multimodal. If your dataset is
multimodal a dialogue box will appear asking to select the modalities
for source reconstruction from a list. If you select more than one
modality, multiomodal fusion will be performed. This option based on the
paper by Henson et al. (Henson, Mouchlianitis, and Friston 2009) uses a
heuristic to rescale the data from different modalities so that they can
be used together.

Once the inversion is completed you will see the time course of the
region with maximal activity in the top plot of the graphics window. The
bottom plot will show the maximal intensity projection (MIP) at the time
of the maximal activation. You will also see the log-evidence value that
can be used for model comparison, as explained above. Note that not all
the output of the inversion is displayed. The full output consists of
time courses for all the sources and conditions for the entire time
window. You can view more of the results using the controls in the
bottom right corner of the 3D GUI. These allow focusing on a particular
time, brain area and condition. One can also display a movie of the
evolution of neuronal activity.

## Summarizing the results of inverse reconstruction as an image

SPM offers the possibility of writing the results as 3D NIfTI images, so
that you can then proceed with GLM-based statistical analysis using
Random Field theory. This is similar to the 2nd level analysis in fMRI
for making inferences about region and trial-specific effects (at the
between subject level).

This entails summarizing the trial- and subject-specific responses with
a single 3-D image in source space. Critically this involves prompting
for a time-frequency contrast window to create each contrast image. This
is a flexible and generic way of specifying the data feature you want to
make an inference about (e.g., gamma activity around 300 ms or average
response between 80 and 120 ms). This kind of contrast is specified by
pressing the `Window` button. You will then be asked about the time
window of interest (in ms, peri-stimulus time). It is possible to
specify one or more time segments (separated by a semicolon). To specify
a single time point repeat the same value twice. The next question is
about the frequency band. If you just want to average the source time
course leave that at the default, zero. In this case the window will be
weighted by a Gaussian. In the case of a single time point this will be
a Gaussian with 8 ms full width half maximum (FWHM). If you specify a
particular frequency or a frequency band, then a series of Morlet
wavelet projectors will be generated summarizing the energy in the time
window and band of interest.

There is a difference between specifying a frequency band of interest as
zero, as opposed to specifying a wide band that covers the whole
frequency range of your data. In the former case the time course of each
dipole will be averaged, weighted by a gaussian. Therefore, if within
your time window this time course changes polarity, the activity can
average out and in an ideal case even a strong response can produce a
value of zero. In the latter case the power is integrated over the whole
spectrum ignoring phase, and this would be equivalent to computing the
sum of squared amplitudes in the time domain.

Finally, if the data file is epoched rather than averaged, you will have
a choice between "evoked", "induced" and "trials". If you have multiple
trials for certain conditions, the projectors generated at the previous
step can either be applied to each trial and the results averaged
(induced) or applied to the averaged trials (evoked). Thus it is
possible to perform localization of induced activity that has no
phase-locking to the stimulus. It is also possible to focus on frequency
content of the ERP using the "evoked" option. Clearly the results will
not be the same. The projectors you specified (bottom plot) and the
resulting MIP (top plot) will be displayed when the operation is
completed. "trials" option makes it possible to export an image per
trial which might be useful for doing within-subject statistics. The
images are exported as 4D-NIfTI with one file per condition including
all the trials for that condition.

The `Image` button is used to write out the contrast results. It is
possible to export them as either values on a mesh (GIfTI) or volumetric
3D images (NIfTI). Both formats are supported by SPM statistical
machinery. When generating an image per trial the images are exported as
4D-NIfTI with one file per condition including all the trials for that
condition. The values of the exported images are normalized to reduce
between-subject variance. Therefore, for best results it is recommended
to export images for all the time windows and conditions that will be
included in the same statistical analysis in one step. Note that the
images exported from the source reconstruction are a little peculiar
because of smoothing from a 2D cortical sheet into 3D volume. SPM
statistical machinery has been optimized to deal with these
peculiarities and get sensible results. If you try to analyze the images
with older versions of SPM or with a different software package you
might get different (less focal) results.

## Rendering interface

By pressing the `Render` button you can open a new GUI window which will
show you a rendering of the inversion results on the brain surface. You
can rotate the brain, focus on different time points, run a movie and
compare the predicted and observed scalp topographies and time series. A
useful option is "virtual electrode" which allows you to extract the
time course from any point on the mesh and the MIP at the time of
maximal activation at this point. Just press the button and click
anywhere in the brain.  
An additional tool for reviewing the results is available in the SPM
M/EEG <span class="smallcaps">Review</span> function.

## Group inversion

A problem encountered with MSP inversion is that sometimes it is "too
good", producing solutions that were so focal in each subject that the
spatial overlap between the activated areas across subjects was not
sufficient to yield a significant result in a between-subjects contrast.
This could be improved by smoothing, but smoothing compromises the
spatial resolution and thus subverts the main advantage of using an
inversion method that can produce focal solutions.

To circumvent this problem we proposed a modification of the MSP method
(Litvak and Friston 2008) that effectively restricts the activated
sources to be the same in all subjects with only the degree of
activation allowed to vary. We showed that this modification makes it
possible to obtain significance levels close to those of non-focal
methods such as minimum norm while preserving accurate spatial
localization.

The group inversion can yield much better results than individual
inversions because it introduces an additional constraint for the
ill-posed inverse problem, namely that the responses in all subjects
should be explained by the same set of sources. Thus it should be your
method of choice when analyzing an entire study with subsequent GLM
analysis of the images.

Group inversion works very similarly to what was described above. You
can start it by pressing the "Group inversion" button right after
opening the 3D GUI. You will be asked to specify a list of M/EEG data
sets to invert together. Then the routine will ask you to perform
coregistration for each of the files and specify all the inversion
parameters in advance. It is also possible to specify the contrast
parameters in advance. Then the inversion will proceed by computing the
inverse solution for all the files and will write out the output images.
The results for each subject will also be saved in the header of the
corresponding input file. It is possible to load this file into the 3D
GUI after the inversion and explore the results as described above.

## Batching source reconstruction

There is a possibility to run imaging source reconstruction using the
SPM batch tool. It can be accessed by pressing the "Batch" button in the
main SPM window and then going to "M/EEG source reconstruction" in the
"SPM" under "M/EEG". There are separate tools there for building head
models, computing the inverse solution and computing contrasts and
generating images. This makes it possible for instance to generate
images for several different contrasts from the same inversion. All the
three tools support multiple datasets as inputs. In the case of the
inversion tool group inversion will be done for multiple datasets.

## Appendix: Data structure

The MATLAB object describing a given EEG/MEG dataset in SPM is denoted
as *D*. Within that structure, each new inverse analysis will be
described by a new cell of sub-structure field *D.inv* and will be made
of the following fields:

- `method`: character string indicating the method, either "ECD" or
  "Imaging" in present case;

- `mesh`: sub-structure with relevant variables and filenames for source
  space and head modeling;

- `datareg`: sub-structure with relevant variables and filenames for
  EEG/MEG data registration into MRI space;

- `forward`: sub-structure with relevant variables and filenames for
  forward computation;

- `inverse`: sub-structure with relevant variable, filenames as well as
  results files;

- `comment`: character string provided by the user to characterize the
  present analysis;

- `date`: date of the last modification made to this analysis.

- `gainmat`: name of the gain matrix file.

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-Baillet01" class="csl-entry">

Baillet, S., J. C. Mosher, and R. M. Leahy. 2001. "Electromagnetic Brain
Mapping." *IEEE Sign. Proc. Mag.* 18: 14--30.

</div>

<div id="ref-Besl_McKay" class="csl-entry">

Besl, P. J., and N. D. McKay. 1992. "A Method for Registration of 3-d
Shapes." *IEEE Trans. Pat. Anal. And Mach. Intel.* 142: 239--56.

</div>

<div id="ref-Dale93" class="csl-entry">

Dale, A. M., and M. Sereno. 1993. "Improved Localization of Cortical
Activity by Combining EEG and MEG with MRI Surface Reconstruction: A
Linear Approach." *J. Cognit. Neurosci.* 5: 162--76.

</div>

<div id="ref-karl_msp" class="csl-entry">

Friston, K. J., L. Harrison, J. Daunizeau, S. J. Kiebel, C. Phillips, N.
Trujillo-Bareto, R. N. A. Henson, G. Flandin, and J. Mattout. 2008.
"Multiple Sparse Priors for the M/EEG Inverse Problem." *NeuroImage* 39
(3): 1104--20. <https://doi.org/10.1016/j.neuroimage.2007.09.048>.

</div>

<div id="ref-karl_induced" class="csl-entry">

Friston, K. J., R. N. A. Henson, C. Phillips, and J. Mattout. 2005.
"Bayesian Estimation of Evoked and Induced Responses." *Human Brain
Mapping* 27: 722--35. <https://doi.org/10.1002/hbm.20214>.

</div>

<div id="ref-peb1" class="csl-entry">

Friston, K. J., W. D. Penny, C. Phillips, S. J. Kiebel, G. Hinton, and
J. Ashburner. 2002. "Classical and Bayesian Inference in Neuroimaging:
Theory." *NeuroImage* 16: 465--83.

</div>

<div id="ref-rnah_fusion" class="csl-entry">

Henson, R. N. A., E. Mouchlianitis, and K. J. Friston. 2009. "MEG and
EEG Data Fusion: Simultaneous Localisation of Face-Evoked Responses."
*NeuroImage*. <https://doi.org/10.1016/j.neuroimage.2009.04.063>.

</div>

<div id="ref-vl_group" class="csl-entry">

Litvak, V., and K. J. Friston. 2008. "Electromagnetic Source
Reconstruction for Group Studies." *NeuroImage* 42 (4): 1490--98.
<https://doi.org/10.1016/j.neuroimage.2008.06.022>.

</div>

<div id="ref-jm_multiple" class="csl-entry">

Mattout, J., C. Phillips, W. D. Penny, M. Rugg, and K. J. Friston. 2006.
"MEG Source Localization Under Multiple Constraints: An Extended
Bayesian Framework." *NeuroImage* 30 (3): 753--67.
[https://doi.org/10.1016/j.neuroimage.2005.10.037
](https://doi.org/10.1016/j.neuroimage.2005.10.037 ).

</div>

<div id="ref-cp_empirical_eeg" class="csl-entry">

Phillips, C., J. Mattout, M. D. Rugg, P. Maquet, and K. J. Friston.
2005. "An Empirical Bayesian Solution to the Source Reconstruction
Problem in EEG." *NeuroImage* 24: 997--1011.
<https://doi.org/10.1016/j.neuroimage.2004.10.030>.

</div>

</div>

[^1]: forwinv: <http://fieldtrip.fcdonders.nl/development/forwinv>
