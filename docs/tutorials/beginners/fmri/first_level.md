# Multiple Regression

This section describes fitting a mutiple regression model to preprocessed
data from a single participant.
Normally, statistical findings from a single individual are not very useful for
informing us about brain function in the general population, so a group analysis
of data from several participants wil be done later.
The main outcome required at this stage is a "contrast image"
from the subject, which can be entered into the group analysis.


## Loading onset times

Before setting up the analysis we must first load the stimulus
onsets times into MATLAB. A separate file has been created that
contains the stimulus onset times for each of the two runs.
To load these, you would type the following (obviously after
changing `XX` to the appropriate participant number, and possibly
changing the name of the folder depending which directory you are working in):
```matlab
r1 = load('func/sub-XX_ses-mri_task-facerecognition_run-01_events.mat');
r2 = load('func/sub-XX_ses-mri_task-facerecognition_run-02_events.mat');
```

Loading the data will give two data structures (called `r1` and `r2`)
in the MATLAB workspace. These each contain three fields called `famous`,
`scrambled` and `unfamiliar`. Each field contains a vector of onset times
(in seconds).

## Create a results folder

The results from fitting the multiple regression are saved into a folder,
which needs to be created beforehand. How you create this empty folder
(and what you call it) is up to you.

## Model specification

Now press the `Specify 1st-level` button.
This will call up the specification of a fMRI specification job in the
batch editor window. Then

- For `Directory`, select the folder (directory) you created for saving
the result into.
- In the `Timing parameters` option,
    - Highlight `Units for design` and select `Seconds`. This should be self-explanatory.
    - Highlight `Interscan interval` and enter `2`. Again, this should be self explanatory because the TR is two seconds.
    - Leave `Microtime resolution` at the default value of `16`. This option is there to simplify convolving a vector of ones and zeros with the haemodynamic response function.
    - leave `Microtime onset` at the default value of `8`. This sets time zero to the middle slice in time.
- Highlight `Data and Design` and select `New Subject/Session` twice to set up the analysis of the two fMRI runs.
    - For the first run (`Subject/Session`):
        - Highlight `Scans` and use SPM's file selector to choose all the 
          smoothed, spatially normalised and realigned volumes in the first run
          (i.e., all volumes in `func/swrsub-XX_ses-mri_task-facerecognition_run-01_bold.nii`).
        - Highlight `Conditions` and select `New condition`[^2] three times.
            - Open the first newly created `Condition` option and
                - Highlight `Name` and enter `Famous`. This is simply giving the condition an interpretable name.
                - Highlight `Onsets` and enter `r1.famous` (to specify the run 1 onset times for famous faces that you loaded into MATLAB earlier).
                - Highlight `Durations` and enter `1`, because the duration of the pictures on screen was about a second.
                - Leave `Time Modulation` alone. This would be for including additional regressors that model possible habituation/familiarisation effects.
                - Leave `Parametric Modulations` alone. This would be for including additional regressors that modulate the amlitudes of the events.
                - Leave `Orthogonalise modulations` alone. It is not relevant because no moduations are included.
            - Open the second newly created condition and
                - Highlight `Name` and enter `Unfamiliar`.
                - Highlight `Onsets` and enter `r1.unfamiliar`.
                - Highlight `Durations` and enter `1`.
                - As previously, no modulations are included.
            - Open the third newly created condition and
                - Highlight `Name` and enter `Scrambled`.
                - Highlight `Onsets` and enter `r1.scrambled`.
                - Highlight `Durations` and enter `1`.
                - As previously, no modulations are included.
        - Leave `Multiple conditions` alone. This is a short-cut for those who can set up MATAB data structures.
        - Leave `Regressors` alone. This allows additional confounding effects to be included in the model. A potentia use case might be to ignore a corrupted time point by including a vector of all zeros - apart from a `1` corresponding to the time point to ignore.
        - Double-click `Multiple regressors` and specify the text file containing the estimated motion parameters for the first fMRI run (`func/rp_sub-XX_ses-mri_task-facerecognition_run-01_bold.txt`). This is to remove variance that has a simple linear relationship with head motion.
        - `High-pass filter` can be left at the default setting of `128` seconds. This specifies how many cosine basis functions should be included to model out slow drifts over time. Although included in the multiple regression, these basis functions are not dispayed by SPM.
    - Now you need to specify a similar set of information for the second fMRI run. This should be straightforward, but if there were nine runs, you would begin to see why scripting could be useful.
- Ignore `Factorial design`.
- Click on `Basis Functions` and choose
    - `Canonical HRF`.
        - Click on `Model derivatives` and select `Time derivatives`. This allows more leeway in the timings of the events.
- Ignore `Model Interactions (Volterra)`. This is a bit too complicated for beginners.
- Leave `Global normalisation` at its default value of `None`. This would allow all the fMRI volumes to be rescaled so they have the same average intensity, but we do not want this.
- Leave `Masking threshold` at its default value of `0.8`. This is a crude heuristic for deciding which oparts of the data to include in the analysis.
- Leave `Serial correlations` at the default setting of `AR(1)`. This option is for dealing with the fact that noise in fMRI is correlated over time. Data with shorter TRs might need the `FAST` option, but `AR(1)` is reasonable for two second TRs.

Save the job as `model_spec_job.m` and press the `Run` button.

SPM will then write an `SPM.mat` file to the folder you specified.
It will also show the design matrix on the A4-shaped window.

At this stage it is advisable to check your model specification using
SPM's review facility which is accessed via the `Review` button. This
brings up a `Design` tab on the interactive window clicking on which
produces a pulldown menu. You can play around with the various options
without doing any harm.

<figure>
<div class="center">
<img src="../design.png" style="width:100mm" />
</div>
<figcaption>Design matrix</figcaption>
</figure>


## Model estimation

Press the `Estimate` button. This will call
up the specification of an fMRI estimation job in the batch editor
window. Then

- Highlight the `Select SPM.mat` option and then choose the `SPM.mat`
  file saved in the folder you specified.
- Leave `Write residuals` as `No` so as not to clutter the results directory with lots of unnecessary files.
- Leave the `Method` as `Classical`.

Save the job as `model_est_job.m` and press `Run` button.

SPM will make a number of changes to the `SPM.mat` file, and write a number of additiona files into the selected folder.
These include
- `mask.nii` is a binary image of ones and zeros that indicate which parts of the image were analysed.
- `beta_0001.nii` through to `beta_0026.nii`. Each of these contains the parameter estimates for the corresponding column of the design matrix.
- `ResMS.nii`. This contains a map of the residual sum of squares at each voxel.
- `RPV.nii`. This contains a map of the "resels" (resoution elements) per voxel, which indicates the smoothness of the residuals at each location. This is derived from the correlations among neighbouring voxels.


## Inference 

### Faces - Scrambled
Press the `Results` button and select the `SPM.mat` file. This
will invoke the contrast manager. 
This is where you define what you'd like to ask of the data by specifying contrast vectors (for t statistics) or matrices (for F statistics).

We'll start by asking where the BOLD signal is higher for faces versus scrambled faces in both sessions.
To formulate the contrast vector, we'll recap what all the columns of the design matrix encode.
1. Famous faces in run 1
2. Temporal derivatives for famous faces in run 1
3. Unfamiliar faces in run 1
4. Temporal derivatives for unfamiliar faces in run 1
5. Scrambled faces in run 1
6. Temporal derivatives for scrambled faces in run 1
7. Confounding motion effects in run 1 (x translation)
8. Confounding motion effects in run 1 (y translation)
9. Confounding motion effects in run 1 (z translation)
10. Confounding motion effects in run 1 (pitch)
11. Confounding motion effects in run 1 (roll)
12. Confounding motion effects in run 1 (yaw)
13. Famous faces in run 2
14. Temporal derivatives for famous faces in run 2
15. Unfamiliar faces in run 2
16. Temporal derivatives for unfamiliar faces in run 2
17. Scrambled faces in run 2
18. Temporal derivatives for scrambled faces in run 2
19. Confounding motion effects in run 2 (x translation)
20. Confounding motion effects in run 2 (y translation)
21. Confounding motion effects in run 2 (z translation)
22. Confounding motion effects in run 2 (pitch)
23. Confounding motion effects in run 2 (roll)
24. Confounding motion effects in run 2 (yaw)
25. Constant term for run 1
26. Constant term for run 2

Identifying voxels where BOLD is higher for faces versus scrambled involves taking the average of the beta values from the famous and unfamiliar faces and subtracting the beta values for the scrambled faces, and doing this for both runs.
Note that the beta values corresponding with the temporal derivatives are ignored.
This can be considered as taking a weighted sum of the beta values, where the weights are as follows:
```matlab
0.5 0 0.5 0 -1 0  0 0 0 0 0 0  0.5 0 0.5 0 -1 0  0 0 0 0 0 0  0 0
```
This is the contrast vector that would be used for a t test to identify these differences.

When the contrast manager is opened, you can define a new t contrast in the contrast manager by selecting `t contrast` and hitting the `Define new contrast` button (see the figure below for an example).
<figure>
<div class="center">
<img src="../conman0.png" style="width:80mm" />
</div>
<figcaption><strong>The SPM contrast manager.</strong></figcaption>
</figure>

You can then enter a suitable name for the contrast, along with the contrast vector itself, before clicking `submit` (to see a visualisation of the contrast vector) and then `OK`.
<figure>
<div class="center">
<img src="../conman1.png" style="width:80mm" />
</div>
<figcaption><strong>Defining a t contrast vector.</strong></figcaption>
</figure>

This will then lead to a few questions in the "interactive window".
- The first is about whether to `apply masking`, which will allow you to reduce the search volume and therefore reduce the severity of the corrections for multiple comparisons.
This is most useful if you have a prior hypothesis about the involvement of certain brain regions.
To begin with, select `none`.
- The next is whether youd like `p value adjustment to control` for family-wise error or not.
By selecting FWE, you see only those regions that have survived after correction for multiple comparisons.
- The next is the `p value (FWE)` that you consider to be statistically significant (the alpha value). The convention is usually 0.05, but this is purely a convention and has no empirical or theoretical justification.
- The final question is `& extent threshold (voxels)`. This is an option to filter out any tiny blobs, but a value of `0` is suggested.

This then leads on to the results being shown in the "graphics" (A4 shaped) window.
The results you obtain may look like those shown in the figure below.
<figure>
<div class="center">
<img src="../results1.png" style="width:80mm" />
</div>
<figcaption><strong>Defining a t contrast vector.</strong></figcaption>
</figure>

The top left of the window shows the *maximum intensity projection* (MIP), with is like three X-ray views through the results.
Next to this is the design matrix and contrast, and below them is the table of results that we next focus on.

The rows written in bold in the table relate to the highest t statistic within each distinct blob.
Where the text is not written in bold, the values relate to locally maximum voxels within the blobs (i.e., where a t statistic is higher than all the neighbouring t statistics) that are more than 8 mm apart.  

The columns in volume table show, from right to left:

- **x, y, z (mm)**: coordinates in [ICBM](https://nist.mni.mcgill.ca/icbm-152lin/) space for each maximum.
- **peak-level**: the chance (p) of finding (under the null hypothesis)
  a peak with this or a greater height (T- or Z-statistic), corrected
  (FWE or FDR) / uncorrected for search volume.
- **cluster-level**: the chance (p) of finding a cluster with this
  many(ke) or a greater number of voxels, corrected (FWE or FDR) /
  uncorrected for search volume.
- **set-level**: the chance (p) of finding this (c) or a greater number
  of clusters in the search volume.

Ignore the `set-level` and `cluster-level` columns for now.
If we focus on the `peak-level` columns we see:
- `p_FWE-cor` shows family-wise corrected p vaues based on the magnitudes of the t statistics. These should all be below the `0.05` threshold entered earlier. This is the main column of interest.
- `q_FDR-cor` shows the results of a false discovery rate correction, based on a proportion of blobs that will be false discoveries. This might be a bit advanced, so ignore it.
- `T` shows the actual t statistics computed from the data.
- `(Z_E)` shows the Z statistic that would have the equivalent statistical significance as the t statistic in the previous column.
- `p_uncorr` shows uncorrected p values. These are usually very close to zero, but if you click on them their values are shown in the MATLAB window.

The final three columns of the table show the coordinates at which the statistics are computed.
If you click on these coordinates, you should see that the red `<` in the MIP moves to that location.
The units of the coordinates are mm, and relate to the space to which the images were normalised (ICBM space).
These coordinates may not mean much to you, but if you'd like to see what brain structure they approximately correspond with, you could click on `Atlas` at the top of the interactive window (the lower left of the three that appear when you start SPM), where you get the option to `Label using` `Neuromorphometrics`.
If you then right-click on the coordinates, you can see which brain structure they probably correspond with.

The interactive window gives you many other options, which should be safe to play with without breaking anything.
One of the options is to superimpose the blobs onto some background image, which you can do from the `Overlays` pulldown menu by selecting `sections`.
For this, you could specify the spatially normalised and intensity nonuniformity corrected anatomical scan you created previously (`../anat/wmsub-XX-T1w.nii`).
Returning to the table can be done by clicking the `whole brain` button on the interactive figure.

Notice that some new images have been created in the results folder.
These are `con_0001.nii` and `spmT_0001.nii`.
The former is the linear combination of beta images that you specified using a contrast vector, while the latter is the corresponding map of t statistics.
When you later do a second-level anaysis across subjects, this will be based on a `con_00XX.nii` from each subject.


### Famous - Unfamiliar 
Here, the aim is to identify where the BOLD signal is higher for the famous faces versus the unfamiliar faces.
As previously, hit the `Results` button, select the `SPM.mat`, define a new t contrast and give it a name (whatever you like as long as you know what it represents).
The contrast vector you enter would be:
```matlab
1 0 -1 0 0 0  0 0 0 0 0 0  1 0 -1 0 0 0  0 0 0 0 0 0  0 0
```

One might expect more subtle effects from this contrast, so it may be worth using a prior hypotheses about where to expect differences in order to reduce the severity of the corrections for multiple comparisons.
As previously, click the `Results` button and define a t contrast using the contrast manager.
This time though, when asked about whether to `apply masking`, click the `atlas` button.
This will ask you to select an atlas file, in which case choose 'labels_Neuromorphometrics.nii,1'.
Once this is specified, SPM will open a table listing the various brain structures from the
Neuromorphometrics atlas. You can select one or more of these (which will require you to hold
the `Ctrl` button on your keyboard when selecting the 2nd, 3rd, etc structure).
Brain regions to consider (from skimming a review by Natu \& O'Tool, 2011) might include the following, which are listed in order:
- `Right Amygdala`
- `Left Amygdala`
- `Right FuG fusiform gyrus`
- `Left FuG fusiform gyrus`
- `Right LiG lingual gyrus`
- `Right MTG middle temporal gyrus`
- `Left MTG middle temporal gyrus`
- `Right PCgG posterior cingulate gyrus`
- `Left PCgG posterior cingulate gyrus`
- `Right PHG parahippocampal gyrus`
- `Right SFG superior frontal gyrus`
- `Left SFG superior frontal gyrus`
- `Right STG superior temporal gyrus`
- `Left STG superior temporal gyrus`
- `Right TMP temporal pole`
- `Left TMP temporal pole`
Another way to suggest brain regions that may be involved in various tasks is to search for relevant terms at (*Neurosynth*)[https://neurosynth.org/analyses/terms/].
Note that the mask is `inclusive` because it includes only these regions. You can see which voxells are included by looking at the `Neuromorphometrics_mask_0XX.nii` image.

There may or may not be any statistically significant findings from this comparison.
Only two of the nine runs per subject are used, which will reduce statistical power.


## Any effect
Now we will do an F test to identify regions where there is any variance in the data explained by the events, in a way that is shared between the runs.
As for the t contrasts, this involves hitting the `Results` button and selecting the `SPM.mat` to open the contrast manager.
This time you would definine a new F contrast.
This requires an F contrast matrix that would take a lot of effort to type out as ones and zeros.
Instead an easier way is used to generate it by defining it as follows:
```matlab
eye(6) zeros(6) eye(6)
```

[^1]: Neuromorphometrics is a company that manually labels brain MRI.
    This atlas is based on their manual annotations.

--8<-- "addons/abbreviations.md"

