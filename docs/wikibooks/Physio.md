# Physiological noise correction

There are a number of toolboxes available implementing variants of
RETROICOR to correct for physiological noise using physiological
recordings (respiratory, cardiac, \...):

- [TAPAS PhysIO
  Toolbox](https://www.tnu.ethz.ch/en/software/tapas/documentations/physio-toolbox.html) -
  integrated with SPM Batch Editor
- [PhLEM
  Toolbox](https://github.com/timothyv/Physiological-Log-Extraction-for-Modeling--PhLEM--Toolbox),
  see also [1](https://sites.google.com/site/phlemtoolbox/Home)
- [DRIFTER Toolbox](http://becs.aalto.fi/en/research/bayes/drifter/) -
  actually not RETROICOR, but a nonlinear Bayesian model of
  physiological noise
- [3dretroicor](http://afni.nimh.nih.gov/pub/dist/doc/program_help/3dretroicor.html)
- See also some direct implementations of the RETROICOR algorithm in
  MATLAB:
  - <http://cbi.nyu.edu/software/>
  - <https://github.com/neurodebian/freesurfer/blob/master/fsfast/toolbox/fast_retroicor.m>

References:

- <http://www.ncbi.nlm.nih.gov/pubmed/10893535>
- <http://mindhive.mit.edu/book/export/html/88>
- <http://mindhive.mit.edu/book/export/html/89>
- <http://fsl.fmrib.ox.ac.uk/fsl/fslwiki/PNM>
- <http://www.nitrc.org/projects/pestica/>
- <http://www.cabiatl.com/CABI/resources/part/>
- <http://sourceforge.net/projects/spmphysiotools/>
