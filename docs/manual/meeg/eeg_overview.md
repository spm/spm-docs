# SPM for MEG/EEG Overview

## Welcome to SPM for M/EEG

SPM functionality for M/EEG data analysis consists of three major parts:

- Statistical analysis of voxel-based images: For statistical analysis, the same routines as SPM for fMRI users are used. These are robust and validated functions based on the General Linear Model[^1] (GLM) and Random Field Theory[^2] (RFT). The statistical methods are equally applicable to multi- (or single-) subject M/EEG studies.

- Source Reconstruction[^3]: Our group has invested heavily in establishing Bayesian approaches to the source reconstruction of M/EEG data. Good source reconstruction techniques are vital for the M/EEG field, otherwise, it would be very difficult to relate sensor data to neuroanatomy or findings from other modalities like fMRI. Bayesian source reconstruction provides a principled way of incorporating prior beliefs about how the data were generated and enables principled methods for model comparison. With the use of priors and Bayesian model comparison, M/EEG source reconstruction is a very powerful neuroimaging tool, which has a unique macroscopic view on neuronal dynamics.

- Dynamic Causal Modelling[^4] (DCM) is a spatio-temporal network model to estimate effective connectivity in a network of sources. For M/EEG, DCM is a powerful technique because the data are highly resolved in time, making the identifiability of neurobiologically inspired network models feasible. This means that DCM can make inferences about temporal precedence of sources and can quantify changes in feedforward, backward, and lateral connectivity among sources on a neuronal time-scale of milliseconds.

## FieldTrip in SPM
In order to make it possible for users to prepare their data for SPM analyses, we also implemented a range of tools for the full analysis pipeline, starting with raw data from the MEG or EEG machine. Our overall goal is to provide an academic M/EEG analysis software package that can be used by everyone to apply the most recent methods available for the analysis of M/EEG data. Although SPM development is focusing on a set of specific methods pioneered by our group, we aim at making it straightforward for users to combine data processing in SPM and other software packages. We have a formal collaboration with the excellent FieldTrip package (head developer: Robert Oostenveld, F.C. Donders Centre in Nijmegen/Netherlands)[^5] on many analysis issues. For example, SPM and FieldTrip share routines for converting data, digital filtering, spectral estimation, forward modelling for M/EEG source reconstruction, and the SPM distribution contains a version of FieldTrip so that one can combine FieldTrip and SPM functions in custom scripts. SPM and FieldTrip complement each other well, as SPM is geared towards specific analysis tools, whereas FieldTrip is a more general repository of different methods that can be put together in flexible ways to perform a variety of analyses. This flexibility of FieldTrip, however, comes at the expense of accessibility to a non-expert user. FieldTrip does not have a graphical user interface (GUI) and its functions are used by writing custom MATLAB scripts. By combining SPM and FieldTrip, the flexibility of FieldTrip can be complemented by SPM's GUI tools and batching system. Within this framework, power users can easily and rapidly develop specialised analysis tools with GUIs that can then also be used by non-proficient MATLAB users. Some examples of such tools are available in the MEEG toolbox distributed with SPM. We will also be happy to include in this toolbox new tools contributed by other users as long as they are of general interest and applicability.

The following chapters go through all the EEG/MEG related functionality of SPM. Most users will probably find the tutorial (chapter [Chap:data:mmn](#Chap:data:mmn)) useful for a quick start. A further detailed description of the conversion, preprocessing functions, and the display is given in chapter [Chap:eeg:preprocessing](#Chap:eeg:preprocessing). In chapter [Chap:eeg:sensoranalysis](#Chap:eeg:sensoranalysis), we explain how one would use SPM's statistical machinery to analyse M/EEG data. The 3D-source reconstruction routines, including dipole modelling, are described in chapter [Chap:eeg:imaging](#Chap:eeg:imaging). Finally, in chapter [Chap:eeg:DCM](#Chap:eeg:DCM), we describe the graphical user interface for dynamical causal modelling, for evoked responses, induced responses, and local field potentials.

[^1]: GLM: <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/GLM.html>

[^2]: RFT: <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/RFT.html>

[^3]: Source Reconstruction: <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/EEG.html>

[^4]: Dynamic Causal Modelling: <http://www.fil.ion.ucl.ac.uk/spm/doc/biblio/Keyword/DCM.html>

[^5]: FieldTrip: <http://fieldtrip.fcdonders.nl/>
