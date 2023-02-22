Various functional MRI analyses, such as PPI or DCM, begin by extracting
representative timeseries from selected brain regions. These are
sometimes called Volumes of Interest (VOIs) or Regions of Interest
(ROIs). There are manual and automated ways of doing this with SPM,
detailed below. Before doing this, we recommend defining an \'Effects of
Interest\' F-contrast, which tells SPM which regressors (columns) in the
design matrix are interesting. Any others regressors, such as head
motion or the mean of the signal, will be regressed out of the
timeseries during VOI extraction.

### Preliminary step - defining effects of interest

The effects of interest F-contrast is defined like any other contrast -
either manually (by pressing Define New Contrast when viewing SPM
results) or using the Contrast Manager batch. Make sure to create an
F-contrast rather than a T-contrast, and for clarity name it Effects of
Interest.

The F-contrast matrix typically has one row per effect of interest. For
example, if the first three regressors in the design matrix are
interesting, then the contrast matrix will be:

\<math\>\begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1
\end{bmatrix}\</math\>

Alternatively, if only the first and third regressors are interesting,
then the matrix is:

\<math\>\begin{bmatrix} 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}\</math\>

In both cases, any effects encoded in the fourth column onwards of the
design matrix will be regressed out during timeseries extraction.

### Manual timeseries extraction

1.  View your SPM results in the usual way by clicking Results,
    selecting your SPM.mat file, and following the questions that appear
    until the results are displayed.
2.  Place the crosshairs / cursor where you want the VOI to be located.
    This is done most easily by clicking on one of the rows of the SPM
    results table (on the mm coordinates at the right hand side).
3.  Click eigenvariate in the Results window. **Note:** If you cursor is
    not positioned at a peak, it will jump to the nearest peak.
4.  Give a name for the region
5.  Click the \"adjust data\" dropdown menu and select the Effects of
    Interest F-contrast you defined earlier.
6.  Select the criteria for which voxels to include for generating the
    representative timeseries for this region. You can choose between a
    sphere centred on the crosshairs, or a box, or the thresholded
    activation cluster located at the crosshairs, or a predefined mask.
    Note that voxels will only be included which exceed the threshold
    for significance chosen in Step 1.
7.  Answer any additional questions about the shape of the VOI, and
    press Done. A file will be saved in the same directory as the
    SPM.mat, called VOI_XX_sess.mat, where sess is the session or run
    number. It contains a variable called Y, which is a representative
    timeseries for the region. More specifically, it is the first
    principal component or eigenvariate of the pre-whitened, high-pass
    filtered and confounded-corrected timeseries in the selected region.

### Batch timeseries extraction (GUI)

The batch editor provides additional flexibility for defining VOIs, and
also enables scripting VOI extraction over subjects. Here we\'ll walk
through creating a VOI based on a sphere which jumps to the peak for
each subject, within a certain maximum radius.

1.  From the main SPM window, click Batch, then using the menu at the
    top, click SPM -\> Util -\> Volume of Interest.
2.  Choose the estimated SPM.mat
3.  For **Adjust data**, select the index of the Effects of Interest
    F-contrast defined above. For example, if the F-contrast was the
    first contrast added to the SPM, and so is first contrast when
    viewing the list of contrasts, type: 1 and press OK.
4.  For **Which session**, select the session or run number for which
    you want to extract a VOI.
5.  Type a short **Name of VOI**.
6.  Define the shape of the VOI by mixing and matching different
    components under Regions of Interest. For this example we\'ll need
    three components: 1) a map of which voxels exceed a statistical
    significance of p \< 0.001 uncorrected 2) a large sphere which is in
    a fixed position across subjects 3) a smaller, mobile sphere which
    can jump automatically to the peak of the larger sphere. To create
    these components, Click: \"**New Thresholded SPM**\", \"**New:
    Sphere**\" and then \"**New: Sphere**\" again. (We\'ll refer to
    these three components as i1, i2 and i3 in the steps which follow.)
7.  For \"**Select SPM.mat**\" under \"Thresholded SPM\", you can leave
    this blank - it will pick up the SPM defined in step 2
    automatically.
8.  For **Contrast**, type in the index of the contrast you want to use
    for finding the peak. E.g. type the number 2 and press OK to use the
    second contrast.
9.  For **Centre** under the first sphere, type the coordinates in mm.
    This will be the outer sphere which constrains the movement of the
    smaller inner sphere. For **Radius**, set the desired radius to
    cover the anatomy of interest, e.g. 15 (in units of mm).
10. For **Centre** under the second sphere, set the Centre to any value,
    e.g. \[0 0 0\] (as it will move automatically). Set the **Radius**
    to the size of the desired ROI, e.g. 8mm. Set **Movement of centre**
    to Global maximum. Set **SPM index** to 1 (if it was the first
    element you selected in step 6) and for **Mask expression**, type:
    \"i2\" (without the quotes) . That tells it to use the outer sphere
    to limit the movement of the inner sphere.
11. Finally, for **Expression** at the bottom, type: \"i1 & i3\"
    (without quotes). That will include all voxels in the first and
    third components added in Step 6 - i.e. the thresholded SPM and the
    mobile sphere.

You can now run the batch, and the timeseries will be saved in a file
named VOI_XX_sess.mat, where XX is the VOI name and sess is the session
number. As for the manual example above, the file will contain a
variable called Y, which is a representative timeseries for the region.
More specifically, it is the first principal component or eigenvariate
of the pre-whitened, high-pass filtered and confounded-corrected
timeseries in the selected region.

## Batch timeseries extraction (script)

A Matlab script corresponding to the steps above is as follows. This can
easily be placed in a loop over subjects. Make sure to check each of the
settings carefully and adjust as needed for each experiment.

\<syntaxhighlight lang=\"matlab\"\> % Insert the subject\'s SPM .mat
filename here spm_mat_file = \'\';

% Start batch clear matlabbatch; matlabbatch{1}.spm.util.voi.spmmat =
cellstr(spm_mat_file); matlabbatch{1}.spm.util.voi.adjust = 1; % Effects
of interest contrast number matlabbatch{1}.spm.util.voi.session = 1; %
Session index matlabbatch{1}.spm.util.voi.name = \'name\'; % VOI name

% Define thresholded SPM for finding the subject\'s local peak response
matlabbatch{1}.spm.util.voi.roi{1}.spm.spmmat = {\'\'};
matlabbatch{1}.spm.util.voi.roi{1}.spm.contrast = 2; % Index of contrast
for choosing voxels matlabbatch{1}.spm.util.voi.roi{1}.spm.conjunction =
1; matlabbatch{1}.spm.util.voi.roi{1}.spm.threshdesc = \'none\';
matlabbatch{1}.spm.util.voi.roi{1}.spm.thresh = 0.001;
matlabbatch{1}.spm.util.voi.roi{1}.spm.extent = 0;
matlabbatch{1}.spm.util.voi.roi{1}.spm.mask \...

`   = struct('contrast', {}, 'thresh', {}, 'mtype', {});`

% Define large fixed outer sphere
matlabbatch{1}.spm.util.voi.roi{2}.sphere.centre = \[-15 -37 59\]; % Set
coordinates here matlabbatch{1}.spm.util.voi.roi{2}.sphere.radius = 15;
% Radius (mm) matlabbatch{1}.spm.util.voi.roi{2}.sphere.move.fixed = 1;

% Define smaller inner sphere which jumps to the peak of the outer
sphere matlabbatch{1}.spm.util.voi.roi{3}.sphere.centre = \[0 0 0\]; %
Leave this at zero matlabbatch{1}.spm.util.voi.roi{3}.sphere.radius = 8;
% Set radius here (mm)
matlabbatch{1}.spm.util.voi.roi{3}.sphere.move.global.spm = 1; % Index
of SPM within the batch
matlabbatch{1}.spm.util.voi.roi{3}.sphere.move.global.mask = \'i2\'; %
Index of the outer sphere within the batch

% Include voxels in the thresholded SPM (i1) and the mobile inner sphere
(i3) matlabbatch{1}.spm.util.voi.expression = \'i1 & i3\';

% Run the batch spm_jobman(\'run\',matlabbatch); \</syntaxhighlight\>
