# SPM for MEG/EEG overview <span id="Chap:eeg:overview" label="Chap:eeg:overview"></span>

## Welcome to SPM for M/EEG

SPM functionality for M/EEG data analysis consists of three major parts.

- Statistical analysis of voxel-based images. For statistical analysis,
  we use exactly the same routines as SPM for fMRI users would. These
  are robust and validated functions based on the General Linear
  Model[^1] (GLM) and Random Field Theory[^2] (RFT). The statistical
  methods are equally applicable to multi- (or single-) subject M/EEG
  studies.

- Source Reconstruction [^3] . Our group has invested heavily in
  establishing Bayesian approaches to the source reconstruction of M/EEG
  data. Good source reconstruction techniques are vital for the M/EEG
  field, otherwise it would be very difficult to relate sensor data to
  neuroanatomy or findings from other modalities like fMRI. Bayesian
  source reconstruction provides a principled way of incorporating prior
  beliefs about how the data were generated, and enables principled
  methods for model comparison. With the use of priors and Bayesian
  model comparison, M/EEG source reconstruction is a very powerful
  neuroimaging tool, which has a unique macroscopic view on neuronal
  dynamics.

- Dynamic Causal Modelling[^4] (DCM), which is a spatio-temporal network
  model to estimate effective connectivity in a network of sources. For
  M/EEG, DCM is a powerful technique, because the data are highly
  resolved in time and this makes the identifiability of
  neurobiologically inspired network models feasible. This means that
  DCM can make inferences about temporal precedence of sources and can
  quantify changes in feedforward, backward and lateral connectivity
  among sources on a neuronal time-scale of milliseconds.

In order to make it possible for the users to prepare their data for SPM
analyses we also implemented a range of tools for the full analysis
pipeline starting with raw data from the MEG or EEG machine.  
Our overall goal is to provide an academic M/EEG analysis software
package that can be used by everyone to apply the most recent methods
available for the analysis of M/EEG data. Although SPM development is
focusing on a set of specific methods pioneered by our group, we aim at
making it straightforward for the users to combine data processing in
SPM and other software packages. We have a formal collaboration with the
excellent FieldTrip package (head developer: Robert Oostenveld, F.C.
Donders centre in Nijmegen/Netherlands)[^5] on many analysis issues. For
example, SPM and FieldTrip share routines for converting data to ,
forward modelling for M/EEG source reconstruction and the SPM
distribution contains a version of FieldTrip so that one can combine
FieldTrip and SPM functions in custom scripts. SPM and FieldTrip
complement each other well, as SPM is geared toward specific analysis
tools, whereas FieldTrip is a more general repository of different
methods that can be put together in flexible ways to perform a variety
of analyses. This flexibility of FieldTrip, however, comes at the
expense of accessibility to a non-expert user. FieldTrip does not have a
graphical user interface (GUI) and its functions are used by writing
custom MATLAB scripts. By combining SPM and FieldTrip the flexibility of
FieldTrip can be complemented by SPM's GUI tools and batching system.
Within this framework, power users can easily and rapidly develop
specialized analysis tools with GUIs that can then also be used by
non-proficient MATLAB users. Some examples of such tools are available
in the MEEG toolbox distributed with SPM. We will also be happy to
include in this toolbox new tools contributed by other users as long as
they are of general interest and applicability.

## Changes from SPM8 to SPM12

SPM8 introduced major changes to the initial implementation of M/EEG
analyses in SPM5. The main change was a different data format that used
an object to ensure internal consistency and integrity of the data
structures and provide a consistent interface to the functions using
M/EEG data. The use of the object substantially improved the stability
and robustness of SPM code. The changes in data format and object
details from SPM8 to SPM12 are relatively minor. The aims of those
changes were to rationalise the internal data structures and object
methods to remove some 'historical' design mistakes and inconsistencies.
For instance, the methods meegchannels, eogchannels, ecgchannels from
SPM8 have been replaced with method indchantype that accepts as an
argument the desired channel type and returns channel indices.
indchantype is one of several methods with similar functionality, the
others being indsample, indchannel, indtrial (that replaces
pickconditions) and indfrequency.  
Another major change in data preprocessing functionality was removal of
interactive GUI elements and switch to the use of SPM batch system. This
should make it easy to build processing pipelines for performing
complete complicated data analyses without programming. The use of batch
has many advantages but can also complicate some of the operations
because a batch must be configured in advance and cannot rely on
information available in the input file. For instance, the batch tool
cannot know the channel names for a particular dataset and thus cannot
generate a dialog box for the user to choose the channels. To facilitate
the processing steps requiring this kind of information additional
functionalities have been added to the 'Prepare' tool under 'Batch
inputs' menu. One can now make the necessary choices for a particular
dataset using an unteractive GUI and then save the results in a mat file
and use this file as an input to batch.  
The following chapters go through all the EEG/MEG related functionality
of SPM. Most users will probably find the tutorial (chapter
<a href="#Chap:data:mmn" data-reference-type="ref"
data-reference="Chap:data:mmn">[Chap:data:mmn]</a>) useful for a quick
start. A further detailed description of the conversion, preprocessing
functions, and the display is given in chapter
<a href="#Chap:eeg:preprocessing" data-reference-type="ref"
data-reference="Chap:eeg:preprocessing">[Chap:eeg:preprocessing]</a>. In
chapter <a href="#Chap:eeg:sensoranalysis" data-reference-type="ref"
data-reference="Chap:eeg:sensoranalysis">[Chap:eeg:sensoranalysis]</a>,
we explain how one would use SPM's statistical machinery to analyse
M/EEG data. The 3D-source reconstruction routines, including dipole
modelling, are described in chapter
<a href="#Chap:eeg:imaging" data-reference-type="ref"
data-reference="Chap:eeg:imaging">[Chap:eeg:imaging]</a>. Finally, in
chapter <a href="#Chap:eeg:DCM" data-reference-type="ref"
data-reference="Chap:eeg:DCM">[Chap:eeg:DCM]</a>, we describe the
graphical user interface for dynamical causal modelling, for evoked
responses, induced responses, and local field potentials.

[^1]: GLM:
    <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/GLM.html>

[^2]: RFT:
    <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/RFT.html>

[^3]: Source Reconstruction:
    <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/EEG.html>

[^4]: Dynamic Causal Modelling:
    <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/DCM.html>

[^5]: FieldTrip: <http://fieldtrip.fcdonders.nl/>
