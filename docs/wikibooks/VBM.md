\<big\>\<big\>**Voxel-Based Morphometry**\</big\>\</big\>

## Criticisms of VBM

There are many criticisms that can be made of VBM. In particular, the
accuracy of the spatial normalisation used by SPM is an issue that
upsets many people. Spatial normalisation in SPM uses only about 1000
parameters, and only fits the overall brain shapes. It is unable to warp
one brain so that it exactly matches another. In order to compensate for
this inaccuracy, the data are smoothed, where the amount of smoothing
should partially depend on the accuracy of the inter-subject
registration. There are many ways of assessing the \"accuracy\" of
spatial normalisation. One approach is to look at the amount of overlap
among the different tissue classes. What this shows is that e.g. grey
matter is matched with grey matter. It does not indicate that e.g. the
various sulci of the different subjects are in alignment. For example,
with a registration model that has too much freedom, a sulcus of one
subject could be warped so that it appears to becomes a different sulcus
of another subject. For the purpose of comparing brain images with
mass-univariate statistics, the objective of spatial normalisation
should not be about making the brains of different subjects appear
identical, but should be about bringing homologous regions as closely
into alignment as possible. There is indication that models with too
much freedom can be worse than models with less freedom. For example,
the evaluations of Hellier et al., whereby registration accuracy of
different approaches are compared indicated that the spatial
normalisation of SPM2 was as accurate as that of any of the other
methods. The average distances between registered sulci were 9.9, 10.3,
11.5, 10.7 and 10.8mm for other methods. The average distance obtained
by the simplistic approach in SPM2 was 8.7mm. Obviously, there is still
plenty of scope for improvement, but for an almost fully automated
approach, it does not do too badly. There are many inter-subject
registration, and tissue segmentation programs out there. These can also
be used for preprocessing if the functions within SPM are found wanting.
In particular, surface based approaches may prove to be superior.

Another criticism is that shape is multivariate, so any kind of
morphometry should be based on multivariate statistics, rather than
mass-univariate tests. I (John Ashburner) largely agree with this.
Unfortunately, the results of multivariate analyses can not be
communicated quite so easily among the brain imaging community -
especially if the results are limited to the 2D pictures required by
papers. The community wants to localise a limited number of discrete
volumetric differences (rather than have a full characterisation of the
differences).

Another one is that different ways of doing an analysis give different
results. The reason for this is that a different analysis is asking a
different question, to which the answer is likely to be different. In
particular, the choice of \"global normalisation\" method will
dramatically change the resulting pattern of blobs. Note also that
different preprocessing will also give different results. The most
\"accurate\" preprocessing will give the most accurate interpretations
of the results and the most sensitivity. Less accurate preprocessing
will give less accurate interpretations. The fact that different
registration or segmentation algorithms produce different results is
exactly analogous to the fact that the use of different landmarks give
different results for other kinds of morphometric analyses. Registration
can never be perfect, so some spurious volumetric differences will be
found. If one averages over larger regions (i.e. smoothes using a larger
FWHM), then the effect of misregistration is reduced. It is therefore
possible to be confident that any differences are really volumetric. If
the arguments against VBM were taken seriously, then all manual
volumetric analyses are wrong because structures can never be outlined
with 100% accuracy.

The non-stationary residual variance from the t-tests is also a problem
for interpreting results. This causes blobs to move towards brain
regions with low residual variance, and away from regions with high
residual variance;
[MASCOI](http://homepages.uni-tuebingen.de/matthias.reimold/mascoi/) may
help with this problem. Many people see significant differences outside
the brain because the residual variance approaches zero as one moves
towards regions without any grey matter; this is one motivation for
using some kind of mask (the other is to reduce the severity of the
multiple-testing problem). Ask your local statistician for further
explanations.

## Interpreting Results

The most general interpretation is that there is a significant
difference in intensities in the preprocessed images in this region. The
hard bit is to determine what may have caused these differences. The two
possible explanations are:

1\) The structure is smaller in one group than the other.

2\) There is some other significant difference between the anatomies of
the groups that shows up in the analysis in this region. There are loads
of possibilities here.

Generally, we hope that the pre-processing has sensitised the tests to
the first explanation, but other causes can never be discounted. e.g.
one group has bigger ventricles, which will systematically influence
spatial normalisation. Another possibility is that there is a contrast
difference, and maybe the structure shows up better (and is therefore
better segmented as grey matter) in one group rather than the other.

The t statistic is proportional to the difference between the groups,
divided by the square root of the residual variability. If there is the
same volumetric difference, but more variability in the grey matter,
then the statstics in the grey matter will be less significant. There
are also likely to be issues relating to estimated residual smoothness.
If the residuals are more smooth, then the correction is less severe so
the corrected stats are more significant.

The statistics are more sensitive further away from regions of greater
variability. In VBM, this means a tendency for the most significant
difference to be shifted away from the centre of a structure.

The contrast images (con\_\*.img) that are estimated with VBM are the
same as if you took the unsmoothed preprocessed data, made a difference
image, and then smoothed this difference image. The initial difference
is mostly at the edges, but is blurred (by the smoothing) so that it
covers a larger region.

## \"Modulation\"

Spatial normalisation expands and contracts some brain regions.
Modulation involves scaling by the amount of contraction, so that the
total amount of grey matter in the modulated GM remains the same as it
would be in the original images.

If you take the limiting case of extremely precise (not necessarily
extremely accurate) registration and segmentation, the pre-processed
concentration images are likely to be all identical. An analysis of
these data would not show you anything. I therefore tend to interpret
unmodulated data as a representation of registration error, rather than
an attempt to preprocess the data in order to sensitise the t tests to
more meaningful volumetric differences.

Shapes are multivariate. The volume of one structure is likely to be
related to that of another. For example, smaller brains may have smaller
putamen. If you include \"globals\" in the model by proportional
scaling, then this could be accounted for. Similarly, the size of a
putamen may, or may not, correlate with the size of surrounding
structures. A modulated analysis attempts to correct the volumes for
regional expansion/shrinkage during spatial normalisation. An
unmodulated analysis would really be comparing volumes after scaling out
the volume changes in neighbouring regions. The definition of a
neighbouring region is vague, and is dependent on the precision of the
spatial normalisation.

For example, consider that spatial normalisation is able to register
only to the resolution of the temporal lobe, and everything in this
brain region expands or contracts by roughly the same amount. If one
group has hippocampi that are large w.r.t. the volume of the temporal
lobe, then this procedure would be more sensitive to such differences.
Unfortunately, it is not straightforward to say exactly what the scale
of the spatial normalisation is, so the results of such an analysis are
a little bit arbitrary.

## VBM in Practice

**Basics**

Start MATLAB, make sure SPM is in your path (e.g. run pathtool and add
the SPM directory), and type *spm pet* to get the GUI up.

### Unified Segmentation in SPM5

**Preliminaries**

(EDITORS: add stuff about DICOM import?)

First ensure that your images are reasonably well aligned with the
template: click *Check Reg*, go to the *SPM5/templates/* directory and
choose *T1.nii* then go to the directory with your dataset and select
the images. If they are not approximately in registration, then you will
need to use the *Display* button and then adjust the values (*up*,
*pitch*, etc.) then click *Reorient* and reselect the image(s).

**Segmentation**

Clicking the *Segment* button will bring up a page of options; you must
select your files (under *Data*), and you will probably want to change
the *Output Files* options. Unless you have reason to do otherwise
(these notes are meant for beginners!) select *modulated normalised* for
the tissues that you are interested in, don\'t save bias corrected, and
don\'t do cleanup. Leave all the *Custom* options as they are. You can
now save the job (as a .mat file) if you want, and/or click *Run* to
segment the images. As a **very** rough estimate, allow 30 minutes per
image.

**Smoothing**

Click *Smooth*, select the images (Modulated normalised/Warped tissue
Class images are prefixed with *mwc*), leave the *data type* option as
*same*, and choose the smoothing width. This is a difficult decision\...
commonly used widths are 8 to 12&nbsp;mm, you may have to experiment to
decide what works best for your scans/subjects, though there is an
argument that the kernel width should match the expected size of the
difference for which you wish to search. Unless you have very good
reasons to do otherwise enter the same number three times, since you
normally wish to smooth equally in all directions (i.e. with an
isotropic kernel).

**Statistics**

This is hard to give a short introduction to, since it very much depends
on your experiment, but to give a very simple example of an unpaired
two-group t-test: click *Basic Models* near the top-left of the GUI,
then choose the two-sample t-test factorial design. Enter the scans, and
enter any covariates that you want (e.g. age, Total Intracranial Volume,
etc. See [\#Obtaining Covariates](#Obtaining_Covariates "wikilink")
below). Now for masking\... it\'s probably best, if this is your first
attempt at VBM, to select *absolute threshold masking* and enter a value
of 0.05. Rather like smoothing though, exactly what masking to use is a
somewhat difficult decision. Set the output *Directory* where you want
the results to be written. It\'s probably best to leave the other
options alone on a first attempt, though maybe check the
[\#Globals](#Globals "wikilink") section below\...

Click *Run*, check the design matrix that appears, and then click
*Estimate* on the left, and choose the SPM.mat file in the directory you
just chose for output. Then click *Results*, and choose this file again.

Now you need to worry about the general linear model contrasts, which
can be quite a complicated area. For a simple unpaired two-group t-test
with no covariates, a t-contrast of \[-1 1\] will give a right-tailed
(group2 \> group1) t-test; \[1 -1\] will give a left-tailed (group1 \>
group2) t-test, and an F-contrast of either \[-1 1\] or \[1 -1\] will
give an F-test (group2 \<math\>\ne\</math\> group1) equivalent to a
two-tailed t-test.

After defining and choosing your contrast, you\'ll then be asked about
masking it with others (say no, unless you know you want this kind of
analysis). Next you\'ll be asked about how to correct/adjust for
multiple comparisons. If you have no idea, I\'d select FDR, and leave
the default of 0.05; better still, read the Genovese and Nichols
articles on FWE and [FDR](http://www.sph.umich.edu/~nichols/FDR/). You
should then be presented with the standard Maximum Intensity Projection
(MIP) plot of the significant regions. Clicking one of the *p-values*
buttons on the left (probably want *volume*) will produce a table of
results, while selecting *overlays\... sections* and choosing e.g. the
T1 template will allow you to move around in the brain viewing the
significant areas.

### Optimised VBM in SPM2 and SPM99

This is not necessary for SPM5 because the Segment button should do
everything in a single unified model. \"Optimised VBM\" was a half-baked
procedure invented in about 10 minutes in order to provide a quick short
term solution for improving the spatial normalisation in SPM. It does
improve the spatial normalisation though, and should also give better
inter-subject registration (and therefore better results) for fMRI data.
If you want to do *optimised VBM* in SPM2 or SPM99, then the procedure
is:

**Segment the original images**

This involves an implicit registration of a template to the image. The
transformation that is estimated here is used to overlay the prior
probability maps, which assist the segmentation. Before doing this step,
you may need to reorient the images via Display. This is so that the
affine registration has better starting estimates. A search for the
keywords *reorient Display* at
[1](http://www.jiscmail.ac.uk/cgi-bin/wa.exe?S1=spm) will give you all
the hints about this that you need.

**Clean up the grey matter.**

This is only for SPM99. In SPM2, this procedure is combined with the
segmentation. The seg1 and seg2 images are entered into the program, and
the result is a brain\_\*.img, which has values of 1 for brain, and 0
for non-brain. The resulting brain\_\*.img is used to remove a few
misclassified voxels from the \*\_seg1.img file. This is done using
ImCalc, selecting the seg1\_, seg2\_, seg3\_, abd brain\_ images,
entering an output filename, and the following expression:

`       `*`i1.*i4./(i1+i2+i3+eps)`*

**Estimate warps from the GM.**

The reason for the first segmentation and cleanup is so that a set of
spatial normalisation parameters can be estimated by matching GM with
GM. In SPM99, you would disable \'Mask brain when registering?\', and
set the resolution for the spatially normalised images to be higher than
the current defaults. The template would be an image of grey matter,
which could be the one in the apriori directory, or it could be a
\"custom\" made template.

Apply these warps to the original T1 image. This gives you a spatially
normalised T1 image, which should have a resolution of about 1mm
isotropic. The original \*\_seg\* and brain\_\* images could be deleted
at this stage.

**Segment the spatially normalised T1.**

Tell the segmentation routine that the image is spatially normalised, so
no affine registration is done.

**Clean up the grey matter.**

The same as above (only for SPM99).

**Modulate**

Spatial normalisation expands and contracts some brain regions.
Modulation involves scaling by the amount of contraction, so that the
total amount of grey matter in the modulated GM remains the same as it
would be in the original images.

**Smooth.**

Best if you do this by about 12mm.

**Stats.**

The whole idea behind the pre-processing is that the t-tests are
sensitised to volumetric differences in the GM. Remember that your
results could reflect other differences among the images.

## Custom Templates

Spatial normalisation in versions of SPM prior to SPM5 required
templates that had similar contrasts to the image to be matched with
them. The reason for this is that the objective function is based on the
mean squared difference between the images. This only makes sense if the
intensities of the different tissue types correspond between the images
(with an additional scaling factor that is estimated, which rescales the
overall intensities). In SPM5, the segment button allows spatial
normalisation to be achieved using a different objective function, which
does not rely on a simple relationship between the intensities of a pair
of images. For full details about the mechanism, I would suggest taking
a look at Ashburner & Friston. *Unified segmentation*. NeuroImage
**26**(3):839-851 (2005).

The idea is that the segmentation of SPM5 warps a set of tissue
probability maps so that they overlay on to the image to segment. After
warping, these will represent (part of) the prior probability of each
voxel belonging to a particular class.

Ideally, these tissue probability maps should represent the prior
probabilities for the population under investigation, and are made by
averaging a large number of spatially normalised tissue class images of
different subjects (wc\*.img). I suspect that if you don\'t have a large
number of subjects, then the tissue probability maps released with SPM5
would be more representative than an average made up of only about 20
subjects.

## Globals

The mass univariate testing approach assumes that all voxels in an image
are independent (apart from the GRF correction part). The use of
\"globals\" is a compromise in order to model dependencies between each
voxel and some global measure.

Most people who study shape use a multi-variate framework. Shape is
based on the relationship between the positions of corresponding
features, after accounting for size, position and orientation.
Unfortunately, we do not have enough subjects to do a full multi-variate
analysis, so we restrict our analyses to being mass-univariate, and
possibly use a \"global\" as a confound, or use some kind of
proportional scaling. We attempt to answer a more clinically interesting
question by limiting our investigation to identifying regions of
increased or decreased GM.

\"Globals\" are a compromise (fudge) that is needed to introduce some
kind of multi-variateness into the analysis. People generally want to
see a map of significant blobs, so a mass-univariate approach (SPM) is
usually adopted. In reality, shape (and hence relative volumes) is
really multivariate. The volume of one structure is related to the
volume of another. Bigger brains may (on average) have bigger
hippocampi. Brains with globally thicker grey matter are more likely to
have grey matter that is thicker at a particular sulcus. Also, brains
where the segmentation (for some reason) overestimates the amount of
grey matter, are also likely to appear to have more grey at a particular
region.

Unfortunately, this multi-variateness is not a simple linear
relationship. For example, bigger brains are likely to have
proportionally more white matter than grey. For brains that are globally
different, it becomes very difficult to interpret exactly what the
differences mean in a mass univariate way. Basically, it is not clear
what to use as globals for any particular experiment. This will depend
on what theory you have about your subjects.

Proportional scaling converts volumes into values that are a proportion
of total brain/GM volume. For example, you may be able to say that a
region around some point contains 3% of the total GM volume. If you are
interested in differences in such measures, then a proportional scaling
model may be preferable. Alternatively, if you want to localise regions
where the trends in GM volume differ from the total GM volume, then an
ANCOVA model may be preferable.

Using no \"globals\" and a \"modulated\" analysis is intended to show
regions of absolute volumetric difference in grey matter. Using total
grey matter as a confound will show regions of absolute difference that
can not be explained by the total grey matter differences.

You can include additional columns in your design matrix that model
various effects of no interest (e.g. an ANCOVA correction for some
global measure). This allows you to localise differences that can not be
explained by these uninteresting effects.

For example, in a group comparison, one group\'s brains may be bigger
than those of the other group. In a situation like this, you may only be
interested in patterns of volumetric differences that differ from those
of the total brain size.

Using proportional scaling to total grey matter will show differences
where a region contains a disproportionately large or small region of
total grey matter. For example, a structure may normally represent 2% of
the total brain grey matter, but in patients it may represent 1.5%.

The males versus females test is a tricky one. Human males are generally
bigger than females, with bigger heads. If you were doing an analysis of
nose sizes, then it is likely that males would have bigger noses. Maybe
you would want to know if this extra size was still significant if total
head size was taken into account, either by modelling head size as a
covariate, or by proportionally scaling the noses such that they were
rendered proportional to the total head size (giving us a measure of
nose as a percentage of head volume).

Normally, a brain with e.g. twice the volume of another brain will not
have twice as much grey matter. The relationship between grey and
white-matter volume approximately obeys a power law, with a 4/3 exponent
(Zhang & Sejnowski, PNAS 97(10):5621-5626, 2000). If we use a
proportional scaling model based on whole brain volume, our null
hypothesis is that grey matter volume varies linearly with brain volume.
This is simply not true, although it may serve as a useful first
approximation.

The choice of model is up to the investigator, and it really does depend
what you want to test. I would really rather not be give any firm
recommendations. When a VBM experiment is written up, the model should
be accurately described. Different models will give different results,
which may appear to conflict with each other.

In the case of dementia, the total intra-cranial volume is often used to
proportionally scale the data. Thus, each structure/region is treated as
a proportion of intra-cranial volume. Head sizes vary, so this
normalisation should reduce the residual variance from the model.

## Obtaining Covariates

**Globals (total segmented tissue volumes)**

*spm_global* is not a good way of computing \"globals\" for VBM, as it
was intended to compute a kind of average brain intensity by excluding
\"non-brain\" voxels (with the assumption that these are below a certain
fraction of the pre-exclusion mean), and computing the mean of what is
left.

The function *spm_get_volumes* can be used to integrate over tissue
segmentations, returning volumes in litres. The differences between
native space (*c*), DARTEL-Imported (*rc*), **modulated** normalised
(*mwc*) and smoothed modulated normalised (*smwc*) segmentation volumes
are usually very small. The main difference is the amount of the spinal
cord included in the white matter segment, which depends on the field of
view of the images. Manual segmentation protocols (e.g. as in [Whitwell
et al., 2001](http://www.ncbi.nlm.nih.gov/pubmed/11559495)) often set an
arbitrary cut-off, such as the most inferior slice (in MNI space) that
includes cerebellum; this is perhaps closest to the results of *mwc* or
*smwc* images.

**Total Intracranial Volume**

To get TIV, one option is to sum the volumes of GM, WM and CSF, for
example using *spm_get_volumes* on each tissue class and then adding
these results. Use of this approach with the *mwc* segments from the New
Segmentation Toolbox in SPM8 performs very well compared to manual or
other automatic measurements ([Ridgway et al.,
2011](http://www.alzheimersanddementia.com/article/S1552-5260%2811%2900238-X/fulltext)).

**Other covariates e.g. from spreadsheets/textfiles**

You can specify any variables in the MATLAB workspace as covariates. If
you have covariates in a text-file you can read this into MATLAB first,
e.g. using *textscan* or *textread*. One approach for importing from
spreadsheets (e.g. Microsoft Excel) is to use the program to save as
*.csv* (comma-separated values) and then to use MATLAB\'s *csvread* to
get variables from the CSV file.

## Data from different scanners

You can generally model out the effect of using different scanners or
scan sequences by modelling confounding effects in the design matrix.
Note that if you are doing a comparison of one group versus another,
where one group is collected on one scanner, and the other group is
collected on another, then modelling out the scanner effect will also
model out the difference between your groups. See Stonnington et al.
2008 [2](http://dx.doi.org/10.1016/j.neuroimage.2007.09.066)

## References and links

### Papers, tutorials, background, etc.

- [Wright et al. 1995](http://dx.doi.org/10.1006/nimg.1995.1032) \-\--
  The original VBM paper
- [Ashburner and Friston 2000](http://dx.doi.org/10.1006/nimg.2000.0582)
  \-\-- Highly cited authoritative paper on the methods
- [Ashburner and Friston
  2005](http://dx.doi.org/10.1016/j.neuroimage.2005.02.018) \-\-- Recent
  paper on SPM5\'s Unified Segmentation model
- [Human Brain Function (2nd
  ed.)](http://www.fil.ion.ucl.ac.uk/spm/doc/books/hbf2/) \-\-- Very
  useful tutorial-style material, especially Ch.6
- [John Ashburner\'s PhD
  Thesis](http://www.fil.ion.ucl.ac.uk/spm/doc/theses/john/) \-\-- More
  thorough than HBF but less recent
- [Brain Warping](http://worldcat.org/wcpa/oclc/40426353) \-\-- edited
  by A. Toga, contains several useful chapters
- [JHU-PNI VBM Methods page](http://pni.med.jhu.edu/methods/vbm.htm)
  \-\-- Tutorial info, references, and links to discussion
- [CBGM at Northwestern University](http://cbmg.tiddlyspot.com/) \-\--
  Many short notes, tutorials, discussion

### Toolboxes, helper scripts, and other software

- [Christian Gaser\'s VBM Toolboxes](http://dbm.neuro.uni-jena.de/vbm)
  including Optimised VBM for SPM2 and SPM5
- [CBGM at Northwestern University](http://cbmg.tiddlyspot.com/)
  contains many useful scripts
- [John\'s Gems](http://www.sph.umich.edu/~nichols/JohnsGems5.html)
  compiled by Tom Nichols
- [Ged Ridgway\'s VBM
  scripts](http://www.cs.ucl.ac.uk/staff/G.Ridgway/vbm/) includes
  resize_img, get_totals, make_diffs
- [Masking toolbox](http://www0.cs.ucl.ac.uk/staff/g.ridgway/masking/)
