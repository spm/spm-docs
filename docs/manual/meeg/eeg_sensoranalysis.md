# Analysis in sensor space <span id="Chap:eeg:sensoranalysis" label="Chap:eeg:sensoranalysis"></span>

This chapter describes how to perform statistical analyses of EEG/MEG
data. This requires transforming data from SPM M/EEG format to image
files (NIfTI format). Once the data are in image format the analyses for
M/EEG are procedurally identical to 2nd level analyses for fMRI. We
therefore refer the reader to the fMRI section for further details of
this last step.

In the drop down "Images" menu, select the function `Convert to images`.
This will open the batch tool for conversion to images. You will need to
select the input dataset, that can be either a mat-file on disk or a
dependency from a previous processing step.  
Then you need to set the 'mode' of conversion. M/EEG data in general
case can be up to 5-dimensional (3 spatial dimensions, time and
frequency). SPM statistical machinery can only handle up to 3
dimensions. Although this is a purely implementational limitation and
the theory behind SPM methods can be extended to any dimensionality, in
practice high-dimensional statistical results can be very hard to
interpret not least due to our inability as humans to visualise them.
Furthermore, unconstrained high-dimensional test would incur very severe
penalty for multiple comparisons and should in most case be avoided.
Thus, our purpose is to reduce our data dimensionality to be 3 or less.
The three spatial dimensions in which the sensors reside can be reduced
to two by projecting their locations onto a plane. Further reduction of
dimensionality will involve averaging over one of the dimensions. The
choices for 'mode' option correspond to all the different possibilities
to average over a subset of data timensions. Some of the options are
only relevant for time-frequency data where the frequency dimension is
present.  
'Conditions' options makes it possible to only convert data for a subset
of conditions in the file. This is especially useful for batch pipeline
building. The conversion module outputs as a dependency a list of all
the generated NIfTI images. These can be used as input to subsequent
steps (e.g. statistical design specification). By including the
'Convert2images' module several times in batch each condition can have a
separate dependency and enter in a different place in the statistical
design (e.g. for two-sample t-test between two groups of trials).  
The 'Channels' option makes it possible to select a subset of channels
for conversions. These can be either selected by modality (e.g. 'EEG')
or chosen by name of by a list in a mat-file (e.g. to average over all
occipital channels).  
'Time window' and 'Frequency window' options limit the data range for
conversion which is especially important if the data are averaged over
this range. Make sure you only include the range of interest.  
Finally the 'Directory prefix' option specifies the prefix for the
directory where images will be written out. This is important if several
different groups of images are generated from the same dataset (e.g.
from different modalities or different channel groups).  

### Output

When running the tool a directory will be created at the dataset
location. Its name will be the name of the dataset with the specified
prefix. In this directory there will be a nii-file for each condition.
In the case of averaged dataset these will be 3D images (where some
dimensions can have size of 1). In the case of an epoched dataset there
will be 4D-NIfTI images where every frame will contain a trial.  

#### Averaging over time or frequency

Although 2D scalp images averaged over time or frequency dimension can
be created directly in conversion to images, they can also be generated
by averaging over part of the Z dimension of previously created 3D
images. This is done via 'Collapse time' tool in the 'Images' menu.  

#### Masking

When you set up your statistical analysis, it might be useful to use an
explicit mask to limit your analysis to a fixed time window of interest.
Such a mask can be created by selecting `Mask images` from "Images"
dropdown menu. You will be asked to provide one unsmoothed image to be
used as a template for the mask. This can be any of the images you
exported. Then you will be asked to specify the time (or frequency)
window of interest and the name for the output mask file. This file can
then enter in your statistical design under the 'Explicit mask' option
or when pressing the 'small volume' button in the 'Results' GUI and
choosing the 'image' option to specify the volume.  

### Smoothing

The images generated from M/EEG data must be smoothed prior to second
level analysis using the <span class="smallcaps">Smooth images</span>
function in the drop down "Images" menu. Smoothing is necessary to
accommodate spatial/temporal variability between subjects and make the
images better conform to the assumptions of random field theory. The
dimensions of the smoothing kernel are specified in the units of the
original data (e.g. \[mm mm ms\] for space-time, \[Hz ms\] for
time-frequency). The general guiding principle for deciding how much to
smooth is the matched filter idea, which says that the smoothing kernel
should match the data feature one wants to enhance. Therefore, the
spatial extent of the smoothing kernel should be more or less similar to
the extent of the dipolar patterns that you are looking for (probably
something of the order of magnitude of several cm). In practice you can
try to smooth the images with different kernels designed according to
the principle above and see what looks best. Smoothing in time dimension
is not always necessary as filtering the data has the same effect. For
scalp images you should set the 'Implicit masking' option to 'yes' in
order to keep excluding the areas outside the scalp from the analysis.  
Once the images have been smoothed one can proceed to the second level
analysis.
