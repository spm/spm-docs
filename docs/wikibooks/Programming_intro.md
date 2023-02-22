## Aim

This page is intended to provide a quick-start guide to writing your own
MATLAB scripts and functions using SPM as a library. SPM programming can
mean simply [writing batch scripts](SPM/Batch "wikilink") to automate
common pipelines, writing short helper scripts or functions to
accomplish useful tasks, writing your own [SPM
extensions](http://www.fil.ion.ucl.ac.uk/spm/ext/), or even modifying
your local installation of SPM.

## MATLAB

[MATLAB](http://www.mathworks.com/products/matlab/) is a programming
language developed by [MathWorks](http://www.mathworks.com/).

- [MATLAB](http://en.wikipedia.org/wiki/MATLAB) and
  [MathWorks](http://en.wikipedia.org/wiki/MathWorks) on Wikipedia
- [MATLAB on
  WikiBooks](https://en.wikibooks.org/wiki/MATLAB_Programming)

Many MATLAB tutorials are available online:

- [MATLAB
  Tutorial](http://www.mathworks.com/academia/student_center/tutorials/launchpad.html)

There are also useful books:

- [MATLAB for
  Neuroscientists](http://www.amazon.com/Matlab-Neuroscientists-Introduction-Scientific-Computing/dp/0123745519/)
- [MATLAB for Psychologists](http://www.antoniahamilton.com/matlab.html)

MATLAB courses:

- [MATLAB for Psychology and Neuroscience short course in
  Nottingham](http://www.psychology.nottingham.ac.uk/matlab)
- [MATLAB for Psychologists short course in
  London](http://www.antoniahamilton.com/matlab/index.html)

## SPM functions

### Introduction

SPM has extensive developer documentation in the headers of the source
files. To view the documentation of a function either open the
corresponding source file or call \<code\>help function\</code\> from
the MATLAB command line. Make sure that the SPM folder is in MATLABs
search path.

### Reading image headers and data

- \<code\>spm_vol\</code\> -- header information
- \<code\>spm_read_vols\</code\> -- for reading entire volumes (see
  also: \<code\>spm_vol\</code\>)
- \<code\>spm_slice_vol\</code\> -- for arbitrary planes
- \<code\>spm_sample_vol\</code\> -- any voxels
- \<code\>spm_get_data\</code\> (\<code\>spm_sample_vol\</code\>) -- any
  voxels from multiple volumes
- \<code\>spm_bsplins\</code\> (\<code\>spm_bsplinc\</code\>) -- NB
  \<code\>spm_slice_vol\</code\> and \<code\>spm_sample_vol\</code\>
  offer polynomial or sinc interpolation; these functions provide
  b-spline interp as used in \<code\>spm_reslice\</code\>

### Writing data

- \<code\>spm_write_vol\</code\> (\<code\>spm_vol\</code\>)
- \<code\>spm_create_vol\</code\>
- \<code\>spm_write_plane\</code\>

### Reading and writing data with the alternative NIfTI class library

- \<code\>nifti\</code\> (\<code\>@nifti/Contents\</code\>,
  \<code\>@nifti/create\</code\>)
- \<code\>file_array\</code\> (\<code\>@file_array/Contents\</code\>)

### Geometry, voxel-world mappings

- \<code\>spm_get_space\</code\> -- get the voxel-world mapping matrix
  (a rigid or affine transform, in homogeneous coordinates)
- \<code\>spm_imatrix\</code\> -- convert above matrix to parametrised
  form
- \<code\>spm_matrix\</code\> -- convert parameter vector to affine
  matrix
- \<code\>spm_check_orientations\</code\>

### Linear (rigid/affine) registration and reslicing

- \<code\>spm_realign\</code\>
- \<code\>spm_coreg\</code\> (\<code\>spm_reslice\</code\>)
- \<code\>spm_affreg\</code\>
- \<code\>spm_reslice\</code\> -- needs reference image; see following
  for reslicing to specified geometry
- \<code\>reorient\</code\> (\<code\>resize_img\</code\>) -- available
  from [John\'s
  Gems](http://www.sph.umich.edu/~nichols/JohnsGems5.html#Gem2) 2 and 3

### Preprocessing, including segmentation and non-linear normalisation

- \<code\>spm_preproc\</code\> (\<code\>spm_config_preproc\</code\>,
  \<code\>spm_prep2sn\</code\>, \<code\>spm_preproc_write\</code\>) --
  SPM5\'s unified segmentation and normalisation
- \<code\>spm_normalise\</code\> -- the old pre-SPM5 non-unified spatial
  normalisation
- \<code\>spm_segment\</code\> -- the old pre-SPM5 non-unified tissue
  segmentation
- \<code\>spm_smooth\</code\>

### Processing

- \<code\>spm_imcalc\</code\> -- perform arbitrary calculations on
  volumes (low level function)
- \<code\>spm_imcalc_ui\</code\> -- high level convenience wrapper for
  \<code\>spm_imcalc\</code\>

### Statistics

- \<code\>spm_ancova\</code\> (\<code\>spm_reml_ancova\</code\>) --
  unused by SPM itself, but often useful for scripting or educational
  purposes

### Viewing data

- \<code\>spm_check_registration\</code\> (\<code\>spm_image\</code\>,
  \<code\>spm_orthviews\</code\>) -- the ubiquitous three orthogonal
  views
- \<code\>slover\</code\> -- slices through images, overlays of
  thresholded or raw statistics; see also
  [slice_overlay](http://imaging.mrc-cbu.cam.ac.uk/imaging/DisplaySlices)
- \<code\>spm_mip_ui\</code\> (\<code\>spm_mip\</code\>) -- maximum
  intensity projections or *glass brain* images

### Configuration

- \<code\>spm_defaults\</code\>
- \<code\>spm_get_defaults\</code\>

### Batch System

- \<code\>spm_select\</code\>
- \<code\>spm_jobman\</code\>

## Illustrative examples

### Reading and writing a volume (to replace NaNs with zeros)

A simpler (but more memory-hungry) version of an old
[Gem](http://www.sph.umich.edu/~nichols/JohnsGems.html#Gem2). See the
[SPM8 version of
gem](http://blogs.warwick.ac.uk/nichols/entry/zero_nans_in/) for an
example of plane-wise reading and writing.

`fnm = spm_select(1, 'image');`  
`[pth, bnm, ext] = spm_fileparts(fnm);`  
`VI = spm_vol(fnm);`  
`VO = VI; % copy input info for output image`  
`VO.fname = fullfile(pth, [bnm '_zn' ext]);`  
`img = spm_read_vols(VI);`  
`img(isnan(img)) = 0; % use ~isfinite instead of isnan to replace +/-inf with zero`  
`spm_write_vol(VO,img);`

## External links

See also:

- [John Ashburner\'s SPM
  Gems](http://www-personal.umich.edu/~nichols/JohnsGems.html)
  ([SPM2](http://www-personal.umich.edu/~nichols/JohnsGems2.html),
  [SPM5](http://www-personal.umich.edu/~nichols/JohnsGems5.html))
- [Ged Ridgway\'s SPM8
  scripts](http://www.cs.ucl.ac.uk/staff/g.ridgway/#sw)
- [Chris Rorden\'s SPM8
  scripts](http://www.mccauslandcenter.sc.edu/CRNL/tools/spm8-scripts)
- [SPM Batch scripting](SPM/Batch "wikilink")
- [SPM Extensions](http://www.fil.ion.ucl.ac.uk/spm/ext/)
