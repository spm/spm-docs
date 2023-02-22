## Batch Scripts

See this [Powerpoint
presentation](http://www.icn.ucl.ac.uk/courses/MATLAB-Tutorials/Sessions2008_09/Guillaume_Flandin/Batching%20SPM.ppt)
for an overview.

See the relevant chapter in the [SPM
manual](http://www.fil.ion.ucl.ac.uk/spm/doc/manual.pdf#Chap:batch_interface).

For general comments on scripting with MATLAB and SPM see the
[programming intro](http://en.wikibooks.org/wiki/SPM/Programming_intro).

## Batch Script for SPM12

Any batch script should follow the template:

`spm('defaults','fmri');`  
`spm_jobman('initcfg');`  
  
`matlabbatch{1}.spm... = ...;`  
  
`spm_jobman('run',matlabbatch);`

## Batch Script for SPM8

SPM12\'s advices also apply to SPM8.

The Batch Scripts for SPM5 below can also be used in SPM8.

## Batch Script for SPM5

Examples of script (for SPM5 and compatible with SPM8) are provided with
the [SPM Data Sets](http://www.fil.ion.ucl.ac.uk/spm/data/), for
example:

- [First Level block fMRI data
  analysis](http://www.fil.ion.ucl.ac.uk/spm/data/auditory/)
- [First Level event-related fMRI data
  analysis](http://www.fil.ion.ucl.ac.uk/spm/data/face_rep/)
- [Second Level fMRI data
  analysis](http://www.fil.ion.ucl.ac.uk/spm/data/face_rfx/)
- [DCM/PPI](http://www.fil.ion.ucl.ac.uk/spm/data/attention/)
- [Multi-Subject PET data
  analysis](http://www.fil.ion.ucl.ac.uk/spm/data/fluency/)

See also:

- <http://www.icn.ucl.ac.uk/courses/MATLAB-Tutorials/Sessions2008_09/sessions2008_09.htm#S13>
  for a generic multi-subject preprocessing script.
- <http://www.mrc-cbu.cam.ac.uk/people/rik.henson/personal/analysis.html>
- <http://imaging.mrc-cbu.cam.ac.uk/imaging/SpmBatch5>

## Batch Script for SPM2

There is an [example batch
script](http://www.fil.ion.ucl.ac.uk/spm/data/face_rep/SPM2/SPM2/batch.m)
written by Rik Henson, for a single subject fMRI data set (preprocessing
and statistics).
