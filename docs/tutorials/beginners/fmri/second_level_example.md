# Second-level analysis example

??? info "CLNE0068: Research Methods and Data Analysis in Human Neuroscience"
    The material here is for students doing the **CLNE0068: Research Methods and Data Analysis in Human Neuroscience** course at UCL. It may eventually be made more generally available.


First of all, create a folder to save your results into.
Then start MATLAB and type
```matlab
spm fmri
```

## Loading participant information

Age and sex of the participants is stored in a spreadsheet.
You can load this information into MATLAB by typing the following:
```matlab
filename = 'S:\FBS_CLNE0068\Data\Faces\Subject_details.xlsx';
contents = readmatrix(filename,'OutputType','string')
subjects = contents(:,1);              % Participants
age      = str2double(contents(:,2));  % Convert text into numbers
sex      = double(contents(:,3)=="F"); % One denotes female, zero denotes male
```

This gives `age` and `sex` variables in the MATLAB workspace that can be entered into a design matrix.


## Specify 2nd-level
Press the `Specify 2nd-level` button on the "Menu" window to get a `Factorial design specification` job in the batch editor.
Modify the design so it looks like the following
```
Directory                            [specify the directory you created]
Design
 . Multiple regression
 . . Scans                           [select the 1st-leve contrast images]
 . . Covariates
 . . . Covariate
 . . . . Vector                      [enter the age variable]
 . . . . Name                        age
 . . . . Centering                   Overall mean
 . . . Covariate
 . . . . Vector                      [enter the sex variable]
 . . . . Name                        sex
 . . . . Centering                   Overall mean
 . . Intercept                       Include Intercept
Covariates
Multiple covariates
Masking
 . Threshold masking
 . . None
 . Implicit mask                     Yes
 . Explicit mask
Global calculation
 . Omit
Global normalisation
 . Overall grand mean scaling
 . . No
 . Normalisation                     None
```
You can save this as `second_level_spec_job.m` and click the green run button (unless you've forgotten anything) to save the specification.

Once this has finished, you can `Review` the model if you like, and then press the `Estimate` button on the "Menu" window.
Select the `SPM.mat` file that has just been created and press the green run button.
Wait a little while until this finished before going on to looking at the results.


## 2nd-level Results
Press the `Results` button on the "Menu" window and select the `SPM.mat` file in order to open the contrast manager.
there are a few things you could look at from this model.
Remember that the columns of the design matrix correspond with:

1. `mean` - a column of ones to model the mean
2. `age` - the participants' ages (years)
3. `sex` - whether or not the participants are female (1=female, 0=male)


### Main Effect
When the aim is to assess whether the average values in the contrast image are greater than zero, we can simply assess the first beta image, which encodes the mean.
The t contrast vector to do this is
```
1 0 0
```
Note that this is an ANCOVA model because effects of age and sex are covaried out.

### Any age effect
Age is encoded by the second column of the design matrix.
To see both positive and negative age effects, we would need a two-tailed t test.
This can be achieved using an F contrast of
```
0 1 0
```
With data from only 16 participants and three columns in theh design matrix, the statistical analyses have only 13 degrees of freedom.
It may therefore not be very sensitive to age-related effects.

### Any age or sex effect
Now we are intersted in any variance explained by the second and third columns of the design matrix.
This can be done using the following F contrast:
```
0 1 0
0 0 1
```

