
### Group Statistics on Source Reconstructions

Now we have a new set of 16$\times$<!-- -->3 GIfTI images for the power
between 10-20Hz and 100-250ms for each subject for each condition after
group-inversion, we can put them into the same repeated-measures ANOVA
that we used above, i.e., the `batch_stats_rmANOVA_job.m` file. This can
be scripted as (i.e, simply changing the output directories at the start
from, e.g, `IndMSPStats` to `GrpMSPStats`).

```matlab
srcstatsdir{1} = fullfile(outpth,'MEEG','GrpMSPStats');
srcstatsdir{2} = fullfile(outpth,'MEEG','GrpMNMStats');

jobfile = {fullfile(scrpth,'batch_stats_rmANOVA_job.m')};

for val = 1:length(srcstatsdir)
    if ~exist(srcstatsdir{val})
        eval(sprintf('!mkdir %s',srcstatsdir{val}));
    end
    
    inputs  = cell(nsub+1, 1);    
    inputs{1} = {srcstatsdir{val}};    
    for s = 1:nsub
        inputs{s+1,1} = cellstr(strvcat(spm_select('FPList',fullfile(outpth,subdir{s},'MEEG'),sprintf('^apMcbdspmeeg_run_01_sss_%d.*\.gii',val))));   
    end
    
    spm_jobman('serial', jobfile, '', inputs{:});
end
```

When it has run, press "Results" from the SPM Menu window and select the
`SPM.mat` file in the relevant output directories. You will notice that
the results for minimum norm have not changed much -- a lot of voxels
remain significant after correction, but in a broadly distributed swathe
of ventral temporal lobe. For the results in the `MEEG/GrpMSPStats`
directory, there is a small anterior right temporal cluster that
survives correction. But if you lower the threshold to $p<.001$
uncorrected, you should see results like in
Figure <a href="#multi:fig:15" data-reference-type="ref"
data-reference="multi:fig:15">1.15</a>, which includes more focal
regions in the ventral temporal lobe, and importantly, more such regions
that for the individual MSP inversions the `MEEG/IndMSPStats` directory
(demonstrating the advantage of group-based inversion).

<figure id="multi:fig:15">
<div class="center">
<img src="../../../../assets/figures/manual/multi/figure15.png" style="width:150mm" />
</div>
<figcaption><em>Group SPM for Faces vs Scrambled power on cortical mesh
between 10-20Hz and 100-250ms across all 16 subjects at <span
class="math inline"><em>p</em> &lt; .001</span>uncorrected, using
Group-optimised MSP. <span id="multi:fig:15"
label="multi:fig:15"></span></em></figcaption>
</figure>

## Group MEEG Source Reconstruction with fMRI priors

Finally, in an example of full multi-modal integration, we will use the
significant clusters in the group fMRI analysis as separate spatial
priors for the group-optimised source reconstruction of the fused MEG
and EEG data (see \[Henson et al, 2011\]). Each cluster becomes a
separate prior, allowing for fact that activity in those clusters may
occur at different post-stimulus times.

This group-based inversion can be implemented in SPM simply by selecting
the binary (thresholded) image we created from the group fMRI statistics
(`fac-scr_fmri_05_cor.nii` in the `BOLD` directory), which contains
non-zero values for voxels to be included in the clustered priors. This
is simply an option in the inversion module, so can scripted like this
(using the same batch file as before, noting that this includes two
inversions -- MNM and MSP -- hence the two inputs of the same data files
below):

```matlab
jobfile = {fullfile(scrpth,'batch_localise_evoked_job.m')};
tmp = cell(nsub,1);
for s = 1:nsub
    tmp{s} = spm_select('FPList',fullfile(outpth,subdir{s},'MEEG'),'^apMcbdspmeeg.*\.mat');
end
inputs = cell(4,1);
inputs{1} = cellstr(strvcat(tmp{:}));
inputs{2} = {fullfile(outpth,'BOLD','fac-scr_fmri_05cor.nii')};  % Group fMRI priors
inputs{3} = cellstr(strvcat(tmp{:}));
inputs{4} = {fullfile(outpth,'BOLD','fac-scr_fmri_05cor.nii')};  % Group fMRI priors
spm_jobman('serial', jobfile, '', inputs{:});
```

Note again that once you have run this, the previous "group" inversions
in the data files will have been overwritten (you could modify the batch
to add new inversion indices `5` and `6`, so as to compare with previous
inversions above, but the file will get very big). Note also that we
have used group-defined fMRI priors, but the scripts can easily be
modified to define fMRI clusters on each individual subject's 1st-level
fMRI models, and use subject-specific source priors here.

### Group Statistics on Source Reconstructions

After running the attached script, we will have a new set of
16$\times$<!-- -->3 GIfTI images for the power between 10-20Hz and
100-250ms for each subject for each condition after group-inversion
using fMRI priors, and can put them into the same repeated-measures
ANOVA that we used above, i.e. the `batch_stats_rmANOVA_job.m` file.
This can be scripted as (i.e. simply changing the output directories at
the start from, e.g. `GrpMNMStats` to `fMRIGrpMNMStats`).

```matlab
srcstatsdir{1} = fullfile(outpth,'MEEG','fMRIGrpMSPStats');
srcstatsdir{2} = fullfile(outpth,'MEEG','fMRIGrpMNMStats');

jobfile = {fullfile(scrpth,'batch_stats_rmANOVA_job.m')};

for val = 1:length(srcstatsdir)
    if ~exist(srcstatsdir{val})
        eval(sprintf('!mkdir %s',srcstatsdir{val}));
    end
    
    inputs  = cell(nsub+1, 1);    
    inputs{1} = {srcstatsdir{val}};    
    for s = 1:nsub
        inputs{s+1,1} = cellstr(strvcat(spm_select('FPList',fullfile(outpth,subdir{s},'MEEG'),...
            sprintf('^apMcbdspmeeg_run_01_sss_%d.*\.gii$',val))));   
    end
    
    spm_jobman('serial', jobfile, '', inputs{:});
end
```

When it has run, press "Results" from the SPM Menu window an select the
`SPM.mat` file from the `fMRIGrpMSPStats` directory, and choose an
uncorrected threshold of $p<.001$. You should see results like in
Figure <a href="#multi:fig:16" data-reference-type="ref"
data-reference="multi:fig:16">1.16</a>, which you can compare to
Figure <a href="#multi:fig:15" data-reference-type="ref"
data-reference="multi:fig:15">1.15</a>. The fMRI priors have improved
consistency across subjects, even in medial temporal lobe regions, as
well as increasing significance of more posterior and lateral temporal
regions (cf., Figure <a href="#multi:fig:11" data-reference-type="ref"
data-reference="multi:fig:11">1.11</a>, at $p<.001$ uncorrected).

<figure id="multi:fig:16">
<div class="center">
<img src="../../../../assets/figures/manual/multi/figure16.png" style="width:150mm" />
</div>
<figcaption><em>Group SPM for Faces vs Scrambled power on cortical mesh
between 10-20Hz and 100-250ms across all 16 subjects at <span
class="math inline"><em>p</em> &lt; .001</span> uncorrected, using
Group-optimised MSP and fMRI priors. <span id="multi:fig:16"
label="multi:fig:16"></span></em></figcaption>
</figure>

--8<-- "addons/abbreviations.md"
