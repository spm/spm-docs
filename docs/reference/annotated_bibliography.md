# Statistical Parametric Mapping: An Annotated Bibliography

W.D. Penny, J. Ashburner, S. Kiebel, R. Henson, D.E. Glaser, C. Phillips and K. Friston.
Wellcome Department of Imaging Neuroscience, University College London.

*21 May 2001*

!!! info ""
    Please note that this is a historic document published in 2001 and does not contain most up-to-date sources about SPM. 

## Abstract

Statistical Paramemetric Mapping (SPM) is a method for detecting regional, task-related changes in brain activity from neuroimaging data. In this paper we provide a guide to the methodological literature on SPM, highlighting key papers and those of historical interest. Complementary approaches, which look at distributed task-related changes, such as multivariate analysis and effective connectivity, are also covered.

## Introduction

The fundamental text on Statistical Parametric Mapping (SPM) is the Human Brain Function (HBF) book [1]. This comprises a basic overview [2], and then chapters on the spatial transformation of images [3], characterizing brain images with the general linear model [7], making statistical inferences [5], characterizing distributed functional systems [3], characterizing functional integration [2] and a chapter looking at the basic types of study design [2].

In addition there are a number of general introductory articles and commentaries. In [9], the distinction is made between (i) making maps of functional responses in the brain and (ii) discerning the principles underlying their organization, where both approaches are considered fruitful. A more detailed and up-to-date review of functional brain imaging appears in [10]. A comprehensive tutorial on SPM, experimental design and functional and effective connectivity appears in [11]. Researchers new to the field are encouraged to read [12].

For each section, we provide a list of methods papers and describe the contribution of each. The lists are not exhaustive and some papers appear in more than one section. We emphasise that this bibliography is focussed on the methodological literature on SPM from the Wellcome Department of Imaging Neuroscience and is by no means meant to be an exhaustive bibliography of the more general area of neuroimaging analysis.

### References

1. R.S.J. Frackowiak, K.J. Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors. Human Brain Function. Academic Press USA, 1997.
2. K.J. Friston. Analyzing brain images: Principles and overview. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 25-41. Academic Press USA, 1997.
3. J. Ashburner and K.J. Friston. Spatial transformation of images. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 43-58. Academic Press USA, 1997.
4. A. Holmes, J-B Poline, and K.J. Friston. Characterizing brain images with the general linear model. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 59-84. Academic Press USA, 1997.
5. J-B Poline, A. Holmes, K. Worsley, and K.J. Friston. Making statistical inferences. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 85-106. Academic Press USA, 1997.
6. K.J. Friston. Characterizing distributed functional systems. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 107-126. Academic Press USA, 1997.
7. C. Buchel and K.J. Friston. Characterizing functional integration. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 127-140. Academic Press USA, 1997.
8. K.J. Friston, C.J. Price, C. Buchel, and R.S.J. Frackowiak. A taxonomy of study design. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 141-159. Academic Press USA, 1997.
9. K.J. Friston. Imaging neuroscience: principles or maps? Proc. Natl. Acad. Sci. USA, 95:796-802, 1998.
10. B. Horwitz, K.J. Friston, and J.G. Taylor. Neural modeling and functional brain imaging: An overview. Neural Networks, 13:829-846, 2000.
11. K.J. Friston. Statistical parametric mapping and other analysis of functional imaging data. In Brain Mapping: The Methods, pages 363-385. Academic Press, 1996.
12. K.J. Friston. Imaging cognitive anatomy. Trends in Cognitive Science, 1:21-27, 1997.

## Spatial processing

Before subsequent processing, images must be realigned and spatially normalised to remove movement artifacts or to put data from different subjects into a common anatomical frame. These methods are reviewed in [1].

The idea of using structural information in functional data to register images was first presented in [2]. This approach provided a linear affine normalisation of PET images (an affine transformation preserves parallel lines).

In [3], the authors consider the registration of PET images using linear translations to correct for positional shifts and nonlinear resampling to account for morphological differences among subjects.

Friston et al. [4] propose an algorithm for removing movement-related artifacts in fMRI. The method is composed of two stages. In the first stage, each image is transformed via a 6-parameter rigid-body transformation so as to be as similar as possible to the first image in the time series. This corrects for changes in instantaneous position of the body in the scanner. A second stage, based on an autoregressive-moving average (ARMA) model, corrects for changes in the recorded signal due to the sequence of body positions. This second component can be attributed to the spin-excitation history of the object being scanned.

In [5], the authors describe a general framework for registering images that involves both spatial and intensity transformations using a Gauss-Newton optimisation method. This paper introduced spatial basis functions for defining warps and is the foundation for the nonlinear normalisation used in SPM.

In [6], the authors present a method for co-registration of brain images from different modalities. Instead of matching the images directly, one performs intermediate within modality registration to two template images that are already in register. This paper also describes a segmentation method that is refined in [7]. The main contribution of this work is that it uses spatial priors from previously segmented images. In [8], the authors consider the co-registration of functional PET images with structural fMRI images (using the above method). The algorithm is compared to the Automatic Image Realignment (AIR) method. Ashburner [9] et al. extend the standard affine registration method by incorporating information about the variability in the shape and size of heads in the form of Bayesian priors. The paper is is important because it brought the spatial basis function approach into a Bayesian framework. In [10] and [11], the authors present a nonlinear spatial registration algorithm which uses a set of low spatial frequency basis functions (discrete cosines), and a smoothness constraint which is implemented using a Bayesian Maximum a Posteriori (MAP) estimator. In this short review article [12], the authors explain the motivation behind the development of the above registration methods - highlighting the need to remove motion artifacts and to bring data from many subjects into the same space for anatomical localization and intersubject averaging.

In [13], the authors present a finite element (hence high dimensional) approach to nonlinear image registration. Symmetric priors are imposed to ensure smooth transforms and a MAP solution is found. The work in [14] extends the algorithm from 2 to 3-dimensional images.

Registration methods are also central to the field of computational neuroanatomy which looks at differences in the structure (rather than function) of different human brains.

In [15], the authors illustrate a method for identifying macroscopic anatomical differences among the brains of different populations of subjects. The method involves spatial normalization of structural MR images (affine transformation and a nonlinear deformation, based on a discrete cosine basis set) and a canonical variates analysis (see section 8) to assess significant differences in deformations between groups. This is known as Deformation-Based Morphometry (DBM).

A related procedure is known as Voxel-Based Morphometry (VBM) [7] and was first described in [16]. VBM throws away global differences and focuses on local differences. The normalised images are first classified as being either grey matter, white matter or Cerebro-Spinal Fluid (CSF), using a refined version of the algorithm described in [6]. Following this, the data are smoothed before performing voxel-wise statistical tests.

### References

1. J. Ashburner. Computational Neuroanatomy. PhD thesis, University College London, 2000.
2. K.J. Friston, R.E. Passingham, J.G. Nutt, J.D. Heather, G.V. Sawle, and R.S.J. Frackowiak. Localisation in PET images: direct fitting of the intercommissural (AC-PC) line. J. Cereb Blood Flow Metab, 9:690-695, 1989.
3. K. J. Friston, C. D. Frith, P. F. Liddle, and R. S. J. Frackowiak. Plastic transformation of PET images. J. Comput. Assist. Tomogr., 15:634-639, 1991.
4. K. J. Friston, S. Williams, R. Howard, R. S. J. Frackowiak, and R. Turner. Movement-related effects in fMRI time-series. Magnetic Resonance in Medicine, 35:346-355, 1996.
5. K. J. Friston, J. Ashburner, C. D. Frith, J.-B. Poline, J. D. Heather, and R. S. J. Frackowiak. Spatial registration and normalization of images. Human Brain Mapping, 2:165-189, 1995.
6. J. Ashburner and K. J. Friston. Multimodal image coregistration and partitioning - a unified framework. NeuroImage, 6(3):209-217, 1997. 7 J. Ashburner and K. J. Friston. Voxel-based morphometry - the methods. NeuroImage, 11:805-821, 2000.
7. S. J. Kiebel, J. Ashburner, J.-B. Poline, and K. J. Friston. MRI and PET coregistration - a cross validation of statistical parametric mapping and automated image registration. NeuroImage, 5:271-279, 1997.
8. J. Ashburner, P. Neelin, D. L. Collins, A. C. Evans, and K. J. Friston. Incorporating prior knowledge into image registration. NeuroImage, 6:344-352, 1997.
9. J. Ashburner and K. J. Friston. Nonlinear spatial normalization using basis functions. Human Brain Mapping, 7(4):254-266, 1999.
10. J. Ashburner and K. J. Friston. Spatial normalization. In A.W. Toga, editor, Brain Warping, pages 27-44. Academic Press, 1999.
11. J. Ashburner and K. J. Friston. The role of registration and spatial normalization in detecting activations in functional imaging. Clinical MRI/Developments in MR, 7(1):26-28, 1997.
12. J. Ashburner, J. Andersson, and K. J. Friston. High-dimensional nonlinear image registration using symmetric priors. NeuroImage, 9:619-628, 1999.
13. J. Ashburner, J. Andersson, and K. J. Friston. Image registration using a symmetric prior - in three-dimensions. Human Brain Mapping, 9(4):212-225, 2000.
14. J. Ashburner, C. Hutton, R.S.J. Frackowiak, I. Johnsrude, C. Price, and K. J. Friston. Identifying global anatomical differences: Deformation-based morphometry. Human Brain Mapping, 6(5):348-357, 1998.
15. I.C. Wright, P.K. McGuire, J.-B Poline, J.M. Travere, R.M. Murray, C.D. Frith, R.S.J. Frackowiak, and K.J. Friston. A voxel-based method for the statistical analysis of gray and white matter density applied to schizophrenia. NeuroImage, 2:244-252, 1995.

## General linear model

The General Linear Model (GLM) and the theory of Gaussian Random Fields (GRFs) (or, simply, `Random Field Theory') lie at the heart of statistical parametric mapping. SPM was first formally introduced in [1] which described a GLM that used global activity as a confounding covariate. The defining features of SPM were, at that time, the construction of maps of a statistical parameter obtained by treating each voxel separately in a mass univariate approach. In [2], the problem of multiple comparisons was first addressed using the theory of stochastic processes and estimates of the smoothness of the images (this work was later generalised - as described in section 4). The two elements of SPM, namely GLM and GRF, are described together in [3].

The validity of many of the assumptions underlying SPM (eg. that error variance changes over voxels, that voxel error terms are Gaussian-distributed), is addressed in [4].

In [5], Holmes considers a number of issues underlying the statistical analysis of PET data. This covers GLMs and GRFs and again addresses the issue of homoskedacity (equal error variance) across voxels. A Markov Random Field (MRF) model is also proposed, for segmenting active areas from non-active areas. Finally, a non-parametric approach is proposed for the analysis of simple activation studies. This removes the need to assume that voxels are Gaussian-distributed, and develops a `distribution-free' procedure which is always valid, albeit at the cost of increased computation. The non-parametric analysis is also published in [6]. An introduction to the GLM and its application to neuroimaging (including to fMRI) is given in [7].

### References

1. K.J. Friston, C.D. Frith, P.F. Liddle, R.J. Dolan, A.A. Lammertsma, and R.S.J. Frackowiak. The relationship between global and local changes in PET scans. J. Cereb Blood Flow Metab, 10:458-466, 1990.
2. K.J. Friston, C.D. Frith, P.F. Liddle, and R.S.J. Frackowiak. Comparing functional (PET) images: The assessment of significant change. Journal of Cerebral Blood Flow and Metabolism, 11:690-699, 1991.
3. K. J. Friston, A. P. Holmes, K. J. Worsley, J.-B. Poline, C. D. Frith, and R. S. J. Frackowiak. Statistical parametric maps in functional imaging: A general linear approach. Human Brain Mapping, 2:189-210, 1995.
4. K.J. Friston. Statistical parametric mapping: Ontology and current issues. Journal of Cerebral Blood Flow and Metabolism, 15:361-370, 1995.
5. A.P. Holmes. Statistical Issues in Functional Brain Mapping. PhD thesis, University of Glasgow, December 1994.
6. A. P. Holmes, R. C. Blair, D. G. Watson J, and I. Ford. Non-parametric analysis of statistic images from functional mapping experiments. Journal of Cerebral Blood Flow and Metabolism, 16:7-22, 1996.
7. A. Holmes, J-B Poline, and K.J. Friston. Characterizing brain images with the general linear model. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 59-84. Academic Press USA, 1997.

## Random field theory

In SPM, the GLM is used to relate the activity at each voxel to the experimental task. Significant task-related changes in voxel activity are assessed using F, t or z statistics. To guard against false positives, a correction for multiple comparisons (ie. comparisons over the voxels) must be made. This is achieved using Gaussian Random Field (GRF) theory.

The core paper in this area is [1]. This provides an estimate of the p-value for local maxima of Gaussian, t, and F fields over search regions of any shape or size in any number of dimensions. This unifies earlier results on 2D [2] and 3D [3] images.

The above analysis requires an estimate of the smoothness of the images (images have some inherent smoothness but are also smoothed using Gaussian kernels (i) to allow for variability in subjects neuroanatomy and (ii) to ensure the validity of GRF theory). In [4], Poline et al. estimate the dependence of the resulting SPMs on the estimate of this parameter. Whilst the applied smoothness is usually fixed, [5] propose a scale-space procedure for assessing significance of activations over a range of proposed smoothings. In [6], the authors implement an improved estimation procedure for estimating smoothness in Gaussianised t-fields.

Another approach to assessing significance is based, not on the height of a cluster of activity, but on its spatial extent [7].

In [8], the authors consider a hierarchy of tests that are regarded as voxel-level, cluster-level and set-level inferences. These inferences have increasing power but decreasing spatial localization. The notion of set-level inferences allows one to assess the significance of distributed (rather than local) activations.

If the approximate location of an activation can be specified in advance then the significance of the activation can be assessed using the spatial extent or volume of the nearest activated region [9]. This test is particularly elegant as it does not require a correction for multiple comparisons.

In more recent work [10], Kiebel et al. propose a model for the analysis of PET and fMRI data that allows incorporation of prior neuroanatomical knowledge. Specifically, a set of basis functions placed on the grey matter surface obtained from a T1-weighted MRI image, allows one to specify a spatial smoothing on the data that both varies over the image and is anatomically informed. This improves the spatial resolution and the sensitivity of the resulting SPM analysis.

The mathematical basis of GRF theory is described in a series of peer-reviewed articles in statisical journals [11, 12, 13, 14, 15].

### References

1. K. J. Worsley, S. Marrett, P. Neelin, A. C. Vandal, K. J. Friston, and A. C. Evans. A unified statistical approach for determining significant voxels in images of cerebral activation. Human Brain Mapping, 4:58-73, 1996.
2. K.J. Friston, C.D. Frith, P.F. Liddle, and R.S.J. Frackowiak. Comparing functional (PET) images: The assessment of significant change. Journal of Cerebral Blood Flow and Metabolism, 11:690-699, 1991.
3. K.J. Worsley, A.C. Evans, S. Marrett, and P. Neelin. A three dimensional statistical analysis for CBF activation studies in the human brain. Journal of Cerebral Blood Flow and Metabolism, 12:900-918, 1992.
4. J.-B. Poline, K. J. Friston, K. J. Worsley, and R. S. J. Frackowiak. Estimating smoothness in statistical parametric maps: Confidence intervals on p-values. J. Comput. Assist. Tomogr., 19(5):788-796, 1995.
5. K.J. Worsley, S. Marrett, P. Neelin, A.C. Vandal, K.J. Friston, and A.C. Evans. Searching scale space for activation in PET images. Human Brain Mapping, 4:74-90, 1995.
6. Stefan J. Kiebel, Jean-Baptiste Poline, Karl J. Friston, Andrew P. Holmes, and Keith J. Worsley. Robust smoothness estimation in statistical parametric maps using standardized residuals from the general linear model. NeuroImage, 10:756-766, 1999.
7. K.J. Friston, K.J. Worsley R.S.J. Frackowiak, J.C. Mazziotta, and A.C. Evans. Assessing the significance of focal activations using their spatial extent. Human Brain Mapping, 1:214-220, 1994.
8. K.J. Friston, A. Holmes, J.-B. Poline, C.J. Price, and C.D. Frith. Detecting activations in PET and fMRI: Levels of inference and power. NeuroImage, 40:223-235, 1995.
9. Karl J. Friston. Testing for anatomically specified regional effects. Human Brain Mapping, 5:133-136, 1997.
10. Stefan J. Kiebel, Rainer Goebel, and Karl J. Friston. Anatomically informed basis functions. NeuroImage, 11(6):656-667, 2000.
11. K.J. Worsley. Local maxima and the expected Euler characteristic of excursion sets of , F and t fields. Advances in Applied Probability, 26:13-42, 1994.
12. D.O. Sigmund and K.J. Worsley. Testing for a signal with unknown location and scale in a stationary Gaussian random field. Annals of Statistics, 23:608-639, 1995.
13. K.J. Worsley. The geometry of random images. Chance, 9(1):27-40, 1996.
14. J. Cao and K.J. Worsley. The detection of local shape changes via the geometry of Hotelling's fields. Annals of Statistics, 27(3):925-942, 1999.
15. K.J. Worsley. Estimating the number of peaks in a random field using the Hadwiger characteristic of excursion sets, with applications to medical images. Annals of Statistics, 23:640-669, 1995.

## Experimental design

There are a number of useful reviews of different approaches to experimental design in neuroimaging, the most introductory being [1], with [2] being the most tutorial, and [3] being the most up-to-date (including a discussion of event-related fMRI designs).

The first analyses of PET images used a `cognitive subtraction' paradigm which relies upon the assumption of pure insertion - this assumes that a new cognitive component can be purely inserted ie. without affecting the expression of previous ones. This assumption is often invalid, however, as shown in [4] where it was demonstrated that interaction terms (between the new and previous cognitive components) are often significant.

More powerful experimental designs, such as parametric designs (involving continuous dependent variables, such as time) and factorial designs (which explicitly look at interaction terms) can be facilitated with the GLM. The first published study using a parametric analysis was by Price et al. [5] where activity in primary auditory cortex was found to be linearly related to the word presentation rate (this was in contrast to, for example, Wernicke's area which responded solely to the occurence of a word rather than its presentation rate).

In [6] Buchel et al. extend the above analysis by allowing stimulus or task parameters to be nonlinear functions of the hemodynamic response. This was implemented using second-order polynomial expansions and applied to PET data. In [7] the method was implemented in the context of the General Linear Model and SPM's, based on omnibus F-statistics (eg. to test for any significant linear and/or nonlinear effect), were used to test for local linear or nonlinear dependencies in fMRI data.

The first factorial analysis of PET data, for example, found a significant interaction between motor activation and time during a paced finger opposition task [8]. The first psycho-pharmaceutical study using a factorial design was [9].

Despite these advances, the experimental paradigms so far described are restricted in that the inferences made relate to the particular subjects scanned. This is overcome in the Random Effects (RFX) Analysis procedure where inferences can be made about the populations from which the subjects are sampled (eg. males/females). This can be implemented in SPM using a two-level analysis procedure described in [10].

Conjunction analysis [11] looks for areas of activation that are common to a number of tasks. For example, in a phonological retrieval task whether subjects named words, objects, letters or colors there is always activation of the left thalamus (and a number of other areas). In a further paper [12], the authors show that conjunction analysis can be applied to data from multiple subjects to make inferences about populations. This work relied on a technical development by Worsley [13].

The relative merits of RFX versus conjunction analysis and the larger issues are discussed in [14].

### References

1. K.J. Friston, C.J. Price, and C. Buchel. Designing activation experiments. In B. Gulyas and H.W. Miller-Gartner, editors, Positron Emission Tomography: A Critical Assessment of Recent Trends. Kluwer Academic, 1998.
2. K.J. Friston, C.J. Price, C. Buchel, and R.S.J. Frackowiak. A taxonomy of study design. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 141-159. Academic Press USA, 1997.
3. K.J. Friston. Experimental design and statistical issues. In Brain Mapping: The Disorders, pages 33-58. Academic Press, 2000.
4. K.J. Friston, C.J. Price, P. Fletcher, C. Moore, R.S.J. Frackowiak, and R.J. Dolan. The trouble with cognitive subtraction. NeuroImage, 4:97-104, 1996.
5. C. Price, R. Wise, S. Ramsay, K. Friston, D. Howard, K. Patterson, and R. Frackowiak. Regional response differences withing the human auditory cortex when listening to words. Neuroscience Letters, 11:179-182, 1992.
6. C. Buchel, R.J.S. Wise, C.J. Mummery, J.B. Poline, and K.J. Friston. Nonlinear regression in parametric activation studies. NeuroImage, 4:60-66, 1996.
7. C. Buchel, A.P. Holmes, G. Rees, and K.J. Friston. Characterizing stimulus-response functions using nonlinear regressors in parametric fMRI experiments. NeuroImage, 8:140-148, 1998.
8. K.J. Friston, C. Frith, R.E. Passingham, P. Liddle, and R.S.J. Frackowiak. Motor practice and neurophysiological adaptation in the cerebellum: a positron tomography study. Proc. Royal Soc. Lond. Series B, 248:223-228, 1992.
9. K.J. Friston, P.M. Grasby, C.J. Bench, C.D. Frith, P.J. Cowen, P.F. Liddle, R.S.J. Frackowiak, and R. Dolan. Measuring the neuromodulatory effects of drugs in man with positron emission tomography. Neuroscience Letters, 141:106-110, 1992.
10. A.P. Holmes and K.J. Friston. Generalisability, random effects and population inference. In NeuroImage, volume 7, page S754, 1998.
11. Cathy J. Price and Karl J. Friston. Cognitive conjunction: A new approach to brain activation experiments. NeuroImage, 5:261-270, 1997.
12. K.J. Friston, A.P. Holmes, C.J. Price, C. Buchel, and K.J. Worsley. Multisubject fMRI studies and conjunction analyses. NeuroImage, 10:385-396, 1999.
13. K.J. Worsley and K.J. Friston. A test for a conjunction. Statistics and Probability Letters, 47:135-140, 2000.
14. K.J. Friston, A.P. Holmes, and K.J. Worsley. How many subjects constitute a study ? NeuroImage, 10:1-5, 1999.

## fMRI

To apply SPM to fMRI time-series it was necessary to account for two fundamental characteristics of fMRI data: (i) that the Hemodynamic Reponse Function (HRF) is transient, delayed and dispersed in time and (ii) that the sampling interval is shorter than the time constant of the HRF, making the observed time-series highly correlated. These issues were addressed in [1] where the HRF was modelled using a Poisson function and the intrinsic correlation was measured after first removing the signal components that were phase-locked to the stimulus (ie. the extrinsic components).

In a later paper [2], the authors consider a temporal smoothing of the fMRI time-series (using a Gaussian filter) so as to induce a known autocorrelation structure. This is then used to adjust the degrees of freedom used when making inferences from the GLM. In the further development of this model [3], Worsley and Friston introduced a general procedure for unbiased estimation of the error variance term. This series of papers culminates in [4], where it was shown that whitening (estimating the autocorrelations and then removing them - a procedure favoured by a number of other researchers) can result in a large bias in the resulting statistical inferences (ie. many false positives).

At this point in time, the favoured approach for handling correlations in fMRI time seris was therefore `smoothing' rather than `whitening'. More recent developments, however, have changed this view somewhat. With the advent of Hierarchical Bayesian Models [5],[6] a new approach, which might be termed `Bayesian whitening' is now the preferred method. This allows for both the error variance term and the intrinsic correlation term to be estimated in an unbiased manner.

A second key feature of fMRI data, as opposed to PET data, is that data can be collected more than once from any given subject. This allows for a quantification of the `within-subject' variability, which is to be contrasted with the `between-subject' variability. Both sources of variability can be accounted for the random effects analysis procedure discussed in the previous section. In earlier work Buchel et al. [7] showed that fMRI data from multiple subjects could be pooled in a `fixed-effects' analysis.

### References

1. K.J. Friston, P. Jezzard, and R. Turner. Analysis of functional MRI time-series. Human Brain Mapping, 1:153-171, 1994.
2. K. J. Friston, A. P. Holmes, J.-B. Poline, P. J. Grasby, S. C. R. Williams, R. S. J. Frackowiak, and R. Turner. Analysis of fMRI time series revisited. NeuroImage, 2:45-53, 1995.
3. K. J. Worsley and K. J. Friston. Analysis of fMRI time-series revisited - again. NeuroImage, 2:173-181, 1995.
4. K.J. Friston, O. Josephs, E. Zarahn, A.P. Holmes, S. Rouqette, and J.-B. Poline. To smooth or not to smooth? Bias and efficiency in fMRI time-series analysis. NeuroImage, 12:196-208, 2000.
5. K.J. Friston, W. Penny, C. Phillips, S. Kiebel, G. Hinton, and J. Ashburner. Classical and Bayesian inference in neuroimaging I: Overview and theory. Submitted, 2001.
6. K.J. Friston, D. Glaser, R. Henson, and J. Ashburner. Classical and Bayesian inference in neuroimaging II: Within and between subject variability in fmri. Submitted, 2001.
7. C. Buchel, R. Turner, and K.J. Friston. Lateral geniculate activations can be detected using intersubject averaging and fMRI. Magnetic Resonance in Medicine, 38(691-694), 1997.

## Event-related fMRI

Event-related fMRI (efMRI) is the use of fMRI to characterize and detect transient hemodynamic responses to brief stimuli or tasks, and is analagous to the the study of event-related potentials in electrophysiology. A review of the statistical issues underlying efMRI is presented in [1] with special emphasis on determining the optimal experimental design for testing a given hypothesis.

In [2], temporal basis functions were used to model the `early' and `late' components of the evoked response (in the context of a block-design). These took the form of exponentially modulated sine functions. The model can detect `conventional' activations, where both components are expressed to the same degree, and differential activations such as are involved in tasks that do not require sustained attention.

Josephs et al. [3] proposed staggering stimulus times relative to scan acquisition times so as to realize a higher effective sampling rate. This effectively allowed, for the first time, whole-brain EPI scans to be used in an efMRI context. In [4], the GLM is employed to detect responses that are different in both magnitude and latency.

Friston et al. [5] characterise the evoked hemodynamic response using Volterra kernels. This allows for nonlinear components of the response to be modelled, such as the saturation of responses at high presentation rates. The linear models described above may be viewed as a first order approximation to this nonlinear model.

In [6], the authors consider stochastic experimental designs where an event (eg. a stimulus) has a certain probability of occuring at any given time point. This is to be contrasted with deterministic designs in which the timing of events is fixed. They then make the distinction between stationary stochastic designs where the probability is fixed and nonstationary designs where this probability may vary over time. They conclude that block designs are generally the most efficient for detecting differential responses, whereas designs including null events are the most efficient for detecting transient responses.

In [7], Hopfinger et al. looked at the sensitivity of event-related fMRI analyses to voxel size, spatial and temporal smoothing parameters and the basis set used to characterise the HRF.

### References

1. O. Josephs and R.N.A. Henson. Event-related functional magnetic resonance imaging: modelling, inference and optimization. Phil. Trans. R. Soc. Lond. B, 354:1215-1228, 1999.
2. K.J. Friston, C.D. Frith, R. Turner, and R.S.J. Frackowiak. Characterizing evoked hemodynamics with fMRI. NeuroImage, 2:157-165, 1995.
3. O. Josephs, R. Turner, and K.J. Friston. Event-related fMRI. Human Brain Mapping, 5:243-248, 1997.
4. K.J. Friston, P. Fletcher, O. Josephs, A. Holmes, M.D. Rugg, and R. Turner. Event-related fMRI: characterizing differential responses. NeuroImage, 7:30-40, 1998.
5. K.J. Friston, O. Josephs, G. Rees, and R. Turner. Non-linear event-related responses in fMRI. Magnetic Resonance in Medicine, 39:41-52, 1998.
6. K.J. Friston KJ, E. Zarahn E, O. Josephs, R.N.A. Henson, and A.M. Dale. Stochastic designs in event-related fMRI. NeuroImage, 10:607-619, 1999.
7. J.B. Hopfinger, C. Buchel, A.P. Holmes, and K.J. Friston. A study of analysis parameters that influence the sensitivity of event-related fMRI analyses. NeuroImage, 11:326-333, 2000.

## Multivariate analysis

In neuroimaging, multivariate analysis is concerned with characterising the relations between different areas during brain function; ie. characterising distributed rather than local functional systems. The general approach and a number of instances of it are described in a tutorial in [2].

The simplest and most frequently applied multivariate procedure is Principal Component Analyis (PCA) which can be implemented using Singular Value Decomposition (SVD). As applied to neuroimaging data PCA finds a set of spatial modes that are mutually uncorrelated both spatially and temporally. The modes are also ordered according to the amount of variance they explain. In [2], the authors applied PCA to the analysis of PET data. By comparing the temporal expression of the first few spatial eigenmodes with the variation in experimental condition a distributed functional system associated with the activation could be identified.

A more sophisticated use of PCA occurs in the context of Generalised Eigenimage Analysis (GEA) [3] where the principal component is found which is maximally expressed in one experimental condition/group and minimally expressed in another (eg. control and patient groups).

Friston et al. [4] apply Multi-Dimensional Scaling (MDS) to data from schizophrenic and control subjects. The MDS procedure maps functionally connected areas into similar positions in a 2D or 3D map. This results in a `functional map' rather than an `anatomical map'. For the data studied, abnormal connectivity patterns were observed in a schizophrenic group. The MDS algorithm works by preserving pairwise distances between data in the original high dimensional space and data in the low dimensional (2D or 3D) visualisation space. If the distance metric is chosen to be the Euclidian distance then MDS turns out to be equivalent to projecting the data onto the first 2 or 3 principal components.

A second useful multivariate procedure is the Multivariate Linear Model (MLM) (see eg. [3]) which maps one set of variables to another, ie. there are multiple independent variables (inputs) and multiple dependent variables (outputs). This is to be contrasted with the Linear Model (LM) or General Linear Model (GLM) which has multiple independent variables but only a single dependent variable (eg. the model for a voxel in a standard SPM analysis). The multivariate analysis of covariance (ManCova), for example, may be implemented using an MLM - the MLM is to ManCova what the LM is to the analysis of variance (Anova).

In [5], the authors use a MLM in an analysis of PET data. Wilk's Lambda is used to assess the significance of the amount of signal variance explained (in proportion to the amount of variance unexplained - the `noise'). Canonical Variates Analysis (CVA) is then used to find the associated `canonical' images. These are modes that are expressed most in the signal and least in the noise - CVA is therefore a generalisation of GEA which allows for multiple groups and parametric designs. In [6], the approach is applied to event-related fMRI where task-dependent HRFs were identified. In [5], the approach is applied to the analysis of evoked responses in EEG and MEG. Worsley et al. [7] apply a MLM and CVA to PET and fMRI data. The paper extends previous work by allowing for temporal correlations - hence the application to fMRI.

In [8], the authors derive a nonlinear PCA algorithm, implemented in a neural network, which finds a number of sources which are orthogonal in time. These sources generate the data via first and second order spatial modes - these may be said to constitute the generative network. The sources themselves are derived from the data using a separate `recognition' network. Standard PCA is recovered in the absence of second order spatial modes - or when the generative network implements the inverse function of the recognition network. In [9], Ashburner et al. apply a cluster analysis procedure to PET radiotracer time series. This identified a number of archetypal time series (tracer kinetics) and associated areas.

In [10], Friston offers a critique of Independent Component Analysis (ICA). As applied to neuroimaging data, ICA attempts to find a set of spatial modes that are spatially independent. In contrast to PCA, which finds spatially uncorrelated modes, the ICA modes are sparser and more spatially localised. As with many other multivariate procedures (eg. PCA, MDS, cluster analysis), ICA is data-driven rather than hypothesis-driven and is therefore best suited to the generation of new hypotheses rather than the testing of existing ones.

### References

1. C. Buchel and K.J. Friston. Characterizing functional integration. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 127-140. Academic Press USA, 1997.
2. Karl J. Friston, Chris D. Frith, F.P. Liddle, and Richard S.J. Frackowiak. Functional connectivity: The principal-component analysis of large (PET) data sets. Journal of Cerebral Blood Flow and Metabolism, 13:5-14, 1993.
3. K.J. Friston. Characterizing distributed functional systems. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 107-126. Academic Press USA, 1997.
4. K.J. Friston, C.D. Frith, P. Fletcher, P.F. Liddle, and R.S.J. Frackowiak. Functional topography: multidimensional scaling and functional connectivity in the brain. Cerebral Cortex, 6:156-164, 1996.
5. Karl J. Friston, Jean-Baptiste Poline, Andrew P. Holmes, Chris D. Frith, and Richard S.J. Frackowiak. A multivariate analysis of PET activation studies. Human Brain Mapping, 4:140-151, 1996.
6. K. J. Friston, C. D. Frith, R. S. J. Frackowiak, and R. Turner. Characterizing dynamic brain responses with fMRI: A multivariate approach. NeuroImage, 2:166-172, 1995.
7. Keith J. Worsley, Jean-Baptiste Poline, Karl J. Friston, and Alan C. Evans. Characterizing the response of PET and fMRI data using multivariate linear models. NeuroImage, 6:305-319, 1998.
8. K.J. Friston KJ, J. Phillips, D. Chawla, and C. Buchel. Nonlinear PCA: Characterizing interactions between modes of brain activity. Phil Trans R Soc Lond B, 355:135-146, 2000.
9. J. Ashburner, J. Haslam, C. Taylor, V. J. Cunningham, and T. Jones. A cluster analysis approach for the characterization of dynamic PET data. In R. Myers, V. Cunningham, B. Dale, and T. Jones, editors, Quantification of Brain Function Using PET.
10. K.J. Friston. Modes or models: a critique on independent component analysis for fMRI. Trends in Cognitive Sciences, 2:373-374, 1998.

## Effective connectivity

Effective connectivity is defined in the neuroimaging community [1] as the influence one neural system exerts over another, either at a synaptic or cortical level. It is defined both by a neuroanatomical model, defining which areas are connected, and a mathematical model detailing how they are connected (linearly, nonlinearly, with or without temporal evolution etc.). A tutorial on effective connectivity appears in [2]. Friston et al. [1] initially focussed on linear time-varying connections.

Effective connectivity is to be contrasted with functional connectivity. In [3], the authors present a synthesis of functional and effective connectivity, defining functional connectivity as the temporal correlation between spatially remote neurophysiological processes. Operationally, this is implemented using PCA (see previous section) where the different spatial modes together account for the observed correlations in the data. In contrast, effective connectivity posits directed (and therefore causal - rather than associative) relations between variables. The differences are illustrated on PET and fMRI data and nonlinear time-varying effective connectivity models are illustrated.

In [4], the interactions between V1 and V2 were characterized using a nonlinear model of effective connectivity. This extended the linear model by allowing for modulatary connections, ie. the activity in V1 is linearly dependent on activity in other areas and dependent on the product of activity from other areas and the intrinsic activity in V1 (this is the modulatory term).

To characterize the interactions between multiple areas a 'Structural Equation Model (SEM)' or a 'Path Model' is required. This is specified by a directed graph. In its simplest incarnation a number of path coefficients define the linear relation between nodes on the graph (ie. neuroanatomical areas). These coefficients are then set so as to explain the observed covariance among the variables, as described in [5]. SEM was introduced to neuroimaging by McIntosh and Gonzalez-Lima [6].

Buchel and Friston [7] propose an SEM with time-dependent connections which are estimated using a Kalman filter algorithm. The approach is reviewed in [8] which also has a general discussion on effective connectiviy.

In [9], Friston et al. introduced the concept of Psychophysiological Interactions (PPIs) to neuroimaging. This model explains the activation at any given voxel as an interaction between a psychological variable and a physiological variable (plus the main effects of each). This is a powerful cross-fertilisation of two related concepts (i) factorial designs - which look at the interactions between two psychological variables and (ii) effective connecticity - which looks at the interactions between two physiological variables (ie. the activations in different anatomical areas).

In a study of associative learning of objects and their positions [10], increases in effective connectivity between distinct cortical systems specialised for spatial and object processing were observed (in addition to the expected repetition supression effect).

In a study of visual attentional mechanisms [11] measures of effective connectivity based on a nonlinear system identification showed that the effective connectivity between V2 and V5/MT is dependent on activity in parietal cortex. This provides evidence for the role of top-down 'backwards' connections in visual processing.

### References

1. K.J. Friston, C.D. Frith, and R.S.J. Frackowiak. Time-dependent changes in effective connectivity measured with PET. Human Brain Mapping, 1:69-79, 1993.
2. C. Buchel and K.J. Friston. Characterizing functional integration. In R.S.J. Frackowiak, K.J Friston, C.D. Frith, R.J. Dolan, and J.C. Mazziotta, editors, Human Brain Function, pages 127-140. Academic Press USA, 1997.
3. K.J. Friston. Functional and effective connectivity in neuroimaging: A synthesis. Human Brain Mapping, 2:56-78, 1995.
4. K.J. Friston, L.G. Ungerleider, P. Jezzard, and R. Turner. Charcterizing modulatory interactions between v1 and v2 in human cortex with fMRI. Human Brain Mapping, 2:211-224, 1995.
5. C. Buchel and K.J. Friston. Modulation of connectivity in visual pathways by attention: Cortical interactions evaluated with structural equation modelling and fMRI. Cerebral Cortex, 7:768-778, 1997.
6. A.R. McIntosh and F. Gonzalez-Lima. Structural modeling of functional neural pathways mapped with 2-deoxyglucose: Effects of acoustic startle habituation on the auditory system. Brain Research, 547:295-302, 1991.
7. C. Buchel and K.J. Friston. Dynamic changes in effective connectivity characterized by variable parameter regression and Kalman filtering. Human Brain Mapping, 6(5):403-408, 1998.
8. C. Buchel and K.J. Friston. Assessing interactions among neuronal systems using functional neuroimaging. Neural Networks, 13:871-882, 2000.
9. K.J. Friston, C. Beuchel, G.R. Fink, J. Morris, E. Rolls, and R.J. Dolan. Psychophysiological and modulatory interactions in neuroimaging. NeuroImage, 6:218-229, 1997.
10. C. Buchel, J.T. Coull, and K.J. Friston. The predictive value of changes in effective connectivity for human learning. Science, pages 1538-1541, 1999.
11. K.J. Friston and C. Buchel. Attentional modulation of effective connectivity from V2 to V5/MT in humans. Proceedings of the National Academy of Sciences of the United States of America, 97(13):7591-7596, 2000.

## Most quoted papers

The table below shows a list of those papers that have been cited at least 50 times in scientific journals - naturally, older papers appear more often than newer ones. The information was collated from the Institute for Scientific Information (ISI) citation database. Note that this database does not include volumes 1 and 2 of the Human Brain Mapping journal. A list of SPM methods papers with at least 50 citations. The number of citations are in the left column.

|**Citations**	| **Papers**                                                            | 
|722	        | Comparing functional (PET) images - the assessment .. (1991)          | 
|456	        | The relationship between global and local changes .. (1990)           | 
|436	        | A 3-dimensional statistical analysis .. (1992)                        | 
|278	        | Localization in PET images: direct fitting .. (1989)                  | 
|205	        | Plastic transformation of PET images (1991)                           | 
|169	        | Analysis of fMRI time-series revisited (1995)                         | 
|165	        | Functional connectivity - the PCA of large .. (1993)                  | 
|158	        | Analysis of fMRI time-series revisited - again (1995)                 | 
|152	        | Spatial registration and normalisation of images (1995)               | 
|143	        | Movement-related effects in fMRI time-series (1996)                   | 
|102	        | A unified statistical approach for determining .. (1996)              | 
|98	            | Cognitive conjunction: a new approach to brain .. (1997)              | 
|97	            | Functional neuroanatomy of the human brain .. (1994)                  | 
|76	            | Event-related fMRI (1997)                                             | 
|68	            | Local maxima and the expected Euler .. (1994)                         | 
|63	            | The trouble with cognitive subtraction (1996)                         | 
|57	            | Nonlinear event-related responses in fMRI (1998)                      | 
|56	            | Event-related fMRI: characterising differential responses (1998)      | 
|52	            | Statistical parametric mapping: ontology and current issues (1995)    | 
|51	            | Characterizing evoked hemodynamics with fMRI (1995)                   | 
|51	            | Modulation of connectivity in visual pathways .. (1997)               | 
|50	            | A voxel-based method for the statistical ... (1995)                   | 