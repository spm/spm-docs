\_\_NOTOC\_\_

# Brain Imaging Data Structure

The [Brain Imaging Data Structure (BIDS)](http://bids.neuroimaging.io/)
is a simple and intuitive way to organise and describe neuroimaging and
behavioural data.

### Standard specification

- [Current release](http://bids-specification.readthedocs.io/)
- [Current draft](https://github.com/bids-standard/bids-specification)

### Validator

- [BIDS Validator](https://github.com/bids-standard/bids-validator/):
  [online](http://incf.github.io/bids-validator/) or from the command
  line \<code\>bids-validator\</code\>.

### Discussion Forums

- [Google
  Groups](https://groups.google.com/forum/#!forum/bids-discussion)
- [NeuroStars](https://neurostars.org/tags/bids)

### Tutorials

- [BIDS Examples](https://github.com/bids-standard/bids-examples/)
- [BIDS Starter Kit](https://github.com/bids-standard/bids-starter-kit/)

### Publications

[**The brain imaging data structure, a format for organizing and
describing outputs of neuroimaging
experiments**.](https://www.nature.com/articles/sdata201644) Gorgolewski
K.J., Auer T., Calhoun V.D., Craddock R.C., Das S., Duff E.P., Flandin
G., Ghosh S.S., Glatard T., Halchenko Y.O., Handwerker D.A., Hanke M.,
Keator D., Li X., Michael Z., Maumet C., Nichols B.N., Nichols T.E.,
Pellman J., Poline J.-B., Rokem A., Schaefer G., Sochat V., Triplett W.,
Turner J.A., Varoquaux G. & Poldrack R.A. *Scientific Data* 3, 160044
(2016).

[**BIDS apps: Improving ease of use, accessibility, and reproducibility
of neuroimaging data analysis
methods**.](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005209)
Gorgolewski K.J., Alfaro-Almagro F., Auer T., Bellec P., Capota M.,
Chakravarty M.M., Churchill N.W., Cohen A.L., Craddock R.C., Devenyi
G.A., Eklund A., Esteban O., Flandin G., Ghosh S.S., Guntupalli J.S.,
Jenkinson M., Keshavan A., Kiar G., Liem F., Raamana P.R., Raffelt D.,
Steele C.J., Quirion P.-O., Smith R.E., Strother S.C., Varoquaux G.,
Wang Y., Yarkoni T. & Poldrack R.A. *PLoS Computational Biology*
13(3):e1005209 (2017).

[**MEG-BIDS, the brain imaging data structure extended to
magnetoencephalography**.](https://www.nature.com/articles/sdata2018110)
Niso G., Gorgolewski K.J., Bock E., Brooks T.L., Flandin G., Gramfort
A., Henson R.N., Jas M., Litvak V., Moreau J.T., Oostenveld R.,
Schoffelen J.-M., Tadel F., Wexler J. & Baillet S. *Scientific Data* 5,
180110 (2018).

[**EEG-BIDS, an extension to the brain imaging data structure for
electroencephalography**.](https://www.nature.com/articles/s41597-019-0104-8)
Pernet C.R., Appelhoff S., Gorgolewski K.J., Flandin G., Phillips C.,
Delorme A. & Oostenveld R. *Scientific Data* 6, 103 (2019) .

## BIDS Tools in SPM/MATLAB

SPM provides a number of functionalities
([MATLAB](https://www.mathworks.com/)/[Octave](https://www.octave.org/)
functions) to facilitate the creation or use of datasets formatted
according to BIDS.

### JSON files

A [JSON file](https://www.json.org/) can be read/written using
\<code\>spm_jsonread\</code\> and \<code\>spm_jsonwrite\</code\>.

\<syntaxhighlight lang=\"matlab\"\> \>\> type sub-01_task-rest_bold.json

{

` "RepetitionTime": 3.0,`  
` "Instruction": "Lie still and keep your eyes open"`

} \>\> bold = spm_jsonread(\'sub-01_task-rest_bold.json\')

bold =

`   RepetitionTime: 3`  
`      Instruction: 'Lie still and keep your eyes open'`

\</syntaxhighlight\>

\<syntaxhighlight lang=\"matlab\"\> \>\> descr = struct(\'Name\',\'My
Dataset\',\'BIDSVersion\',\'1.0.2\')

descr =

`          Name: 'My Dataset'`  
`   BIDSVersion: '1.0.2'`

\>\> spm_jsonwrite(\'dataset_description.json\',descr,
struct(\'indent\',\' \')); \>\> type dataset_description.json

{

` "Name": "My Dataset",`  
` "BIDSVersion": "1.0.2"`

} \</syntaxhighlight\>

These functions are also independently available in [JSONio, a JSON
library for MATLAB and Octave](https://github.com/gllmflndn/JSONio).
They are compatible with MATLAB\'s
[jsonencode](http://www.mathworks.com/help/matlab/ref/jsonencode.html)
and
[jsondecode](http://www.mathworks.com/help/matlab/ref/jsondecode.html).

### TSV files

A [tab-separated values (TSV)
file](https://www.wikipedia.org/wiki/Tab-separated_values) can be
read/written using \<code\>spm_load\</code\> and
\<code\>spm_save\</code\>.

\<syntaxhighlight lang=\"matlab\"\> \>\> type
task-Checkerboard_acq-TR645_events.tsv

onset duration trial_type 0 20 Fixation 20 20 Checkerboard 40 20
Fixation 60 20 Checkerboard 80 20 Fixation 100 20 Checkerboard \>\>
events = spm_load(\'task-Checkerboard_acq-TR645_events.tsv\')

events =

`        onset: [6x1 double]`  
`     duration: [6x1 double]`  
`   trial_type: {6x1 cell}`

\</syntaxhighlight\>

\<syntaxhighlight lang=\"matlab\"\> \>\> p =
struct(\'participant_id\',{{\'sub-01\',\'sub-02\'}},
\'sex\',{{\'M\',\'F\'}}, \'age\',\[28 21\])

p =

`   participant_id: {'sub-01'  'sub-02'}`  
`              sex: {'M'  'F'}`  
`              age: [28 21]`

\>\> spm_save(\'participants.tsv\',p) \>\> type participants.tsv

participant_id sex age sub-01 M 28 sub-02 F 21 \</syntaxhighlight\>

These functions are compatible with MATLAB [table
array](https://www.mathworks.com/help/matlab/ref/table.html) and handle
gzip compression transparently.

\<syntaxhighlight lang=\"matlab\"\> \>\> participant_id = {\'sub-01\';
\'sub-02\'}; \>\> sex = {\'M\'; \'F\'}; \>\> age = \[28 21\]\'; \>\> p =
table(participant_id,sex,age); \>\> spm_save(\'participants.tsv.gz\',p)
\>\> spm_load(\'participants.tsv.gz\')

ans =

`   participant_id: {2x1 cell}`  
`              sex: {2x1 cell}`  
`              age: [2x1 double]`

\</syntaxhighlight\>

### NIfTI files

[NIfTI files](https://nifti.nimh.nih.gov/) can be read/written using
\<code\>spm_vol\</code\> or \<code\>@nifti\</code\>.

\<syntaxhighlight lang=\"matlab\"\> \>\> S =
nifti(\'sub-2475376\_\_T1w.nii\')

S =

NIFTI object: 1-by-1

`           dat: [256x256x192 file_array]`  
`           mat: [4x4 double]`  
`    mat_intent: 'Scanner'`  
`          mat0: [4x4 double]`  
`   mat0_intent: 'Scanner'`  
`       descrip: 'MR'`

\>\> F = nifti(\'sub-2475376_task-Checkerboard_bold.nii\')

F =

NIFTI object: 1-by-1

`           dat: [4-D file_array]`  
`           mat: [4x4 double]`  
`    mat_intent: 'Scanner'`  
`          mat0: [4x4 double]`  
`   mat0_intent: 'Scanner'`  
`        timing: [1x1 struct]`  
`       descrip: '4D image'`

\>\> F.timing.tspace

ans =

`   1.4000`

\</syntaxhighlight\>

By default, SPM does not support compressed NIfTI files
(\<code\>.nii.gz\</code\>) but MATLAB/Octave provide
[gzip](https://www.mathworks.com/help/matlab/ref/gzip.html)/[gunzip](https://www.mathworks.com/help/matlab/ref/gunzip.html)
functions if needed and they are also available through the batch
interface from \<code\>BasicIO \> File Operations \> Gunzip
Files\</code\>.

### BIDS parser and queries

A data directory organised according to BIDS can be parsed with
\<code\>spm_BIDS\</code\>.

Here is an example using the
[ds007](https://github.com/INCF/BIDS-examples/tree/master/ds007)
dataset:

\<syntaxhighlight lang=\"matlab\"\> \>\> % Parse BIDS directory \>\>
BIDS = spm_BIDS(\'/data/BIDS-examples/ds007\');

\>\> % Make general queries about the dataset \>\>
spm_BIDS(BIDS,\'subjects\') ans =

`   '01'  '02'  '03'  '04'  '05'  '06'  '07'  '08'  '09'  '10'  '11'  '12'  '13'  '14'  '15'  '16'  '17'  '18'  '19'  '20'`

\>\> spm_BIDS(BIDS,\'sessions\') ans =

`   Empty cell array: 1-by-0`

\>\> spm_BIDS(BIDS,\'runs\') ans =

`   '01'    '02'`

\>\> spm_BIDS(BIDS,\'tasks\') ans =

`   'stopsignalwithletternaming'    'stopsignalwithmanualresponse'    'stopsignalwithpseudowordnaming'`

\>\> spm_BIDS(BIDS,\'types\') ans =

`   'T1w'    'bold'    'events'    'inplaneT2'`

\>\> spm_BIDS(BIDS,\'modalities\') ans =

`   'anat'    'func'`

\>\> % Make more specific queries \>\>
spm_BIDS(BIDS,\'runs\',\'type\',\'T1w\') ans =

`  Empty cell array: 1-by-0`

\>\> spm_BIDS(BIDS,\'runs\',\'type\',\'bold\') ans =

`   '01'    '02'`

\>\> % Get the NIfTI file for subject \'05\', run \'02\' and task
\'stopsignalwithmanualresponse\': \>\>
spm_BIDS(BIDS,\'data\',\'sub\',\'05\',\'run\',\'02\',\'task\',\'stopsignalwithmanualresponse\',\'type\',\'bold\')

ans =

`   '/data/ds007/sub-05/func/sub-05_task-stopsignalwithmanualresponse_run-02_bold.nii.gz'`

\>\> % and corresponding metadata, including TR: \>\>
spm_BIDS(BIDS,\'metadata\',\'sub\',\'05\',\'run\',\'02\',\'task\',\'stopsignalwithmanualresponse\',\'type\',\'bold\')

ans =

`   RepetitionTime: 2`  
`         TaskName: 'stop signal with manual response'`

\>\> % Get the T1-weighted images from all subjects: \>\>
spm_BIDS(BIDS,\'data\',\'type\',\'T1w\')

ans =

`   '/data/ds007/sub-01/anat/sub-01_T1w.nii.gz'`  
`   '/data/ds007/sub-02/anat/sub-02_T1w.nii.gz'`  
`   '/data/ds007/sub-03/anat/sub-03_T1w.nii.gz'`  
`   '/data/ds007/sub-04/anat/sub-04_T1w.nii.gz'`  
`   '/data/ds007/sub-05/anat/sub-05_T1w.nii.gz'`  
`   '/data/ds007/sub-06/anat/sub-06_T1w.nii.gz'`  
`   '/data/ds007/sub-07/anat/sub-07_T1w.nii.gz'`  
`   '/data/ds007/sub-08/anat/sub-08_T1w.nii.gz'`  
`   '/data/ds007/sub-09/anat/sub-09_T1w.nii.gz'`  
`   '/data/ds007/sub-10/anat/sub-10_T1w.nii.gz'`  
`   '/data/ds007/sub-11/anat/sub-11_T1w.nii.gz'`  
`   '/data/ds007/sub-12/anat/sub-12_T1w.nii.gz'`  
`   '/data/ds007/sub-13/anat/sub-13_T1w.nii.gz'`  
`   '/data/ds007/sub-14/anat/sub-14_T1w.nii.gz'`  
`   '/data/ds007/sub-15/anat/sub-15_T1w.nii.gz'`  
`   '/data/ds007/sub-16/anat/sub-16_T1w.nii.gz'`  
`   '/data/ds007/sub-17/anat/sub-17_T1w.nii.gz'`  
`   '/data/ds007/sub-18/anat/sub-18_T1w.nii.gz'`  
`   '/data/ds007/sub-19/anat/sub-19_T1w.nii.gz'`  
`   '/data/ds007/sub-20/anat/sub-20_T1w.nii.gz'`

\</syntaxhighlight\>

### Formatting datasets into BIDS

Helper functions \<code\>spm_mkdir\</code\> and
\<code\>spm_copy\</code\> might come handy, as well as previously
mentioned \<code\>spm_save\</code\> and \<code\>spm_jsonwrite\</code\>.

For example, the following piece of code using
\<code\>spm_mkdir\</code\>:

\<syntaxhighlight lang=\"matlab\"\> \>\>
spm_mkdir(\'/data/bids\',{\'sub-2475376\',\'sub-5489652\'},{\'ses-1\',\'ses-2\'},\'func\');
\>\>
spm_mkdir(\'/data/bids\',{\'sub-2475376\',\'sub-5489652\'},\'ses-1\',\'anat\');
\</syntaxhighlight\>

creates this directory hierarchy:

\<syntaxhighlight lang=\"bash\"\> /data └── bids

`   ├── sub-2475376`  
`   │   ├── ses-1`  
`   │   │   ├── anat`  
`   │   │   └── func`  
`   │   └── ses-2`  
`   │       └── func`  
`   └── sub-5489652`  
`       ├── ses-1`  
`       │   ├── anat`  
`       │   └── func`  
`       └── ses-2`  
`           └── func`

\</syntaxhighlight\>

while \<code\>spm_copy\</code\> makes it easier to copy files and their
attached metadata (e.g. sidecar JSON files) with compression on the fly.

\<syntaxhighlight lang=\"matlab\"\> \>\> ls
sub-2475376_task-rest_bold.json sub-2475376_task-rest_bold.nii.gz \>\>
spm_copy(\'sub-2475376_task-rest_bold.nii.gz\',\'/derivatives\',
\'nifti\',true, \'gunzip\',true) \>\> ls /derivatives
sub-2475376_task-rest_bold.json sub-2475376_task-rest_bold.nii
\</syntaxhighlight\>

See also these options in the batch interface:

- The \<code\>DICOM Import\</code\> batch module has an option to create
  metadata sidecar JSON files:

`matlabbatch{1}.spm.util.import.dicom.convopts.meta = true;`

- The \<code\>3D to 4D File Conversion\</code\> batch module has an
  option to store the TR in the NIfTI header:

`matlabbatch{1}.spm.util.cat.RT = TR;`

## See also

[SPM BIDS-App](https://github.com/BIDS-Apps/SPM)
