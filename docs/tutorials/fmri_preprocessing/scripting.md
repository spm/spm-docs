# fMRI data preprocessing

## Bringing it all together - scripting

If you are processing data from multiple subjects or sessions, you might want to automate your preprocessing pipeline by writing a script. This can speed up your data processing and ensure that each datapoint has been processed with the same parameters. 

To start your script we will use the `_job.m` file generated when we saved the batch we put together in the previous tutorial on [the batch interface](./batch.md). 

First, open this your `preprocessing_batch_job.m` file in Matlab/Octave and inspect it's contents. It should look something like this:

```
%-----------------------------------------------------------------------
% Job saved on 22-Feb-2023 18:02:23 by cfg_util (rev $Rev: 7345 $)
% spm SPM - SPM12 (7771)
% cfg_basicio BasicIO - Unknown
%-----------------------------------------------------------------------
matlabbatch{1}.spm.spatial.realign.estwrite.data = {{'/Users/olivia/OneDrive - King''s College London/spm/spm_tutorials/preprocessing/MoAEpilot/sub-01/func/sub-01_task-auditory_bold.nii'}};
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.quality = 0.9;
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.sep = 4;
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.fwhm = 5;
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.rtm = 1;
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.interp = 2;
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.wrap = [0 0 0];
matlabbatch{1}.spm.spatial.realign.estwrite.eoptions.weight = '';
matlabbatch{1}.spm.spatial.realign.estwrite.roptions.which = [0 1];
matlabbatch{1}.spm.spatial.realign.estwrite.roptions.interp = 4;
matlabbatch{1}.spm.spatial.realign.estwrite.roptions.wrap = [0 0 0];
matlabbatch{1}.spm.spatial.realign.estwrite.roptions.mask = 1;
matlabbatch{1}.spm.spatial.realign.estwrite.roptions.prefix = 'r';
matlabbatch{2}.spm.temporal.st.scans{1}(1) = cfg_dep('Realign: Estimate & Reslice: Realigned Images (Sess 1)', substruct('.','val', '{}',{1}, '.','val', '{}',{1}, '.','val', '{}',{1}, '.','val', '{}',{1}), substruct('.','sess', '()',{1}, '.','cfiles'));
matlabbatch{2}.spm.temporal.st.nslices = 64;
matlabbatch{2}.spm.temporal.st.tr = 7;
matlabbatch{2}.spm.temporal.st.ta = 6.890625;
matlabbatch{2}.spm.temporal.st.so = [64 63 62 61 60 59 58 57 56 55 54 53 52 51 50 49 48 47 46 45 44 43 42 41 40 39 38 37 36 35 34 33 32 31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1];
matlabbatch{2}.spm.temporal.st.refslice = 32;
matlabbatch{2}.spm.temporal.st.prefix = 'a';
matlabbatch{3}.spm.spatial.coreg.estimate.ref(1) = cfg_dep('Realign: Estimate & Reslice: Mean Image', substruct('.','val', '{}',{1}, '.','val', '{}',{1}, '.','val', '{}',{1}, '.','val', '{}',{1}), substruct('.','rmean'));
matlabbatch{3}.spm.spatial.coreg.estimate.source = {'/Users/olivia/OneDrive - King''s College London/spm/spm_tutorials/preprocessing/MoAEpilot/sub-01/anat/sub-01_T1w.nii,1'};
matlabbatch{3}.spm.spatial.coreg.estimate.other = {''};
matlabbatch{3}.spm.spatial.coreg.estimate.eoptions.cost_fun = 'nmi';
matlabbatch{3}.spm.spatial.coreg.estimate.eoptions.sep = [4 2];
matlabbatch{3}.spm.spatial.coreg.estimate.eoptions.tol = [0.02 0.02 0.02 0.001 0.001 0.001 0.01 0.01 0.01 0.001 0.001 0.001];
matlabbatch{3}.spm.spatial.coreg.estimate.eoptions.fwhm = [7 7];
matlabbatch{4}.spm.spatial.preproc.channel.vols(1) = cfg_dep('Coregister: Estimate: Coregistered Images', substruct('.','val', '{}',{3}, '.','val', '{}',{1}, '.','val', '{}',{1}, '.','val', '{}',{1}), substruct('.','cfiles'));
matlabbatch{4}.spm.spatial.preproc.channel.biasreg = 0.001;
matlabbatch{4}.spm.spatial.preproc.channel.biasfwhm = 60;
matlabbatch{4}.spm.spatial.preproc.channel.write = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(1).tpm = {'/Applications/spm12/tpm/TPM.nii,1'};
matlabbatch{4}.spm.spatial.preproc.tissue(1).ngaus = 1;
matlabbatch{4}.spm.spatial.preproc.tissue(1).native = [1 0];
matlabbatch{4}.spm.spatial.preproc.tissue(1).warped = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(2).tpm = {'/Applications/spm12/tpm/TPM.nii,2'};
matlabbatch{4}.spm.spatial.preproc.tissue(2).ngaus = 1;
matlabbatch{4}.spm.spatial.preproc.tissue(2).native = [1 0];
matlabbatch{4}.spm.spatial.preproc.tissue(2).warped = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(3).tpm = {'/Applications/spm12/tpm/TPM.nii,3'};
matlabbatch{4}.spm.spatial.preproc.tissue(3).ngaus = 2;
matlabbatch{4}.spm.spatial.preproc.tissue(3).native = [1 0];
matlabbatch{4}.spm.spatial.preproc.tissue(3).warped = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(4).tpm = {'/Applications/spm12/tpm/TPM.nii,4'};
matlabbatch{4}.spm.spatial.preproc.tissue(4).ngaus = 3;
matlabbatch{4}.spm.spatial.preproc.tissue(4).native = [1 0];
matlabbatch{4}.spm.spatial.preproc.tissue(4).warped = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(5).tpm = {'/Applications/spm12/tpm/TPM.nii,5'};
matlabbatch{4}.spm.spatial.preproc.tissue(5).ngaus = 4;
matlabbatch{4}.spm.spatial.preproc.tissue(5).native = [1 0];
matlabbatch{4}.spm.spatial.preproc.tissue(5).warped = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(6).tpm = {'/Applications/spm12/tpm/TPM.nii,6'};
matlabbatch{4}.spm.spatial.preproc.tissue(6).ngaus = 2;
matlabbatch{4}.spm.spatial.preproc.tissue(6).native = [0 0];
matlabbatch{4}.spm.spatial.preproc.tissue(6).warped = [0 0];
matlabbatch{4}.spm.spatial.preproc.warp.mrf = 1;
matlabbatch{4}.spm.spatial.preproc.warp.cleanup = 1;
matlabbatch{4}.spm.spatial.preproc.warp.reg = [0 0.001 0.5 0.05 0.2];
matlabbatch{4}.spm.spatial.preproc.warp.affreg = 'mni';
matlabbatch{4}.spm.spatial.preproc.warp.fwhm = 0;
matlabbatch{4}.spm.spatial.preproc.warp.samp = 3;
matlabbatch{4}.spm.spatial.preproc.warp.write = [0 1];
matlabbatch{4}.spm.spatial.preproc.warp.vox = NaN;
matlabbatch{4}.spm.spatial.preproc.warp.bb = [NaN NaN NaN
                                              NaN NaN NaN];
matlabbatch{5}.spm.spatial.normalise.write.subj.def(1) = cfg_dep('Segment: Forward Deformations', substruct('.','val', '{}',{4}, '.','val', '{}',{1}, '.','val', '{}',{1}), substruct('.','fordef', '()',{':'}));
matlabbatch{5}.spm.spatial.normalise.write.subj.resample(1) = cfg_dep('Slice Timing: Slice Timing Corr. Images (Sess 1)', substruct('.','val', '{}',{2}, '.','val', '{}',{1}, '.','val', '{}',{1}), substruct('()',{1}, '.','files'));
matlabbatch{5}.spm.spatial.normalise.write.woptions.bb = [-78 -112 -70
                                                          78 76 85];
matlabbatch{5}.spm.spatial.normalise.write.woptions.vox = [3 3 3];
matlabbatch{5}.spm.spatial.normalise.write.woptions.interp = 4;
matlabbatch{5}.spm.spatial.normalise.write.woptions.prefix = 'w';
matlabbatch{6}.spm.spatial.smooth.data(1) = cfg_dep('Normalise: Write: Normalised Images (Subj 1)', substruct('.','val', '{}',{5}, '.','val', '{}',{1}, '.','val', '{}',{1}, '.','val', '{}',{1}), substruct('()',{1}, '.','files'));
matlabbatch{6}.spm.spatial.smooth.fwhm = [6 6 6];
matlabbatch{6}.spm.spatial.smooth.dtype = 0;
matlabbatch{6}.spm.spatial.smooth.im = 0;
matlabbatch{6}.spm.spatial.smooth.prefix = 's';
```

