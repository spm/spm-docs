# BMS: Maps (Inference)  
Bayesian Model Selection for Log-Evidence Maps.
.
Input: log-evidence maps for each model, session and subject. Note that there must be identical numbers of models for all sessions, and identical numbers of sessions for all subjects.
.
Output: For the fixed effects analysis, posterior probability maps are created for each model. For the random effects analysis, expected posterior probability and exceedance probability (i.e. the probability that this model is more likely than any other model) maps are created for each model. If there are multiple sessions per subject, the random effects analysis operates on the subject-specific sums of log evidences across sessions. In addition, a BMS.mat file will be save in the specified directory for both methods

* **Directory** (select files)  
Select the directory where the files containing the results from BMS (BMS.mat) will be written.

* **Data** (create a list of items)  
Select the log. evidence maps for each model, session and subject.

    * **Subject** (create a list of items)  
    .

        * **Session**   
        .

            * **Models** (select files)  
            Specify the log. evidence map for each model. Log-evidence maps should be specified in the same order for each subject and session.

* **Name models** (create a list of items)  
Specify name for each model (optional).

    * **Name** (enter text)  
    Specify name for each model (optional).

* **Inference method** (choose from the menu)  
Specify inference method: random effects (2nd-level, RFX) or fixed effects (1st-level, FFX) analysis. RFX uses a Variational Bayes approach.

* **Output files (RFX)** (choose from the menu)  
Specify which output files to save (only valid forRFX analyses). 
.
Default option (and faster option): PPM = xppm.<ext> (Expected Posterior Probability Maps) for each model ie. posterior mean.
.
Second option: PPM + EPM = xppm.<ext> + epm.<ext> (Expected Posterior Probability Maps + Exceedance Probability Maps) for each model.
.
Third option: PPM + EPM + Alpha = xppm.<ext> + epm.<ext> + alpha.<ext> (PPM, EPM and Map of Dirichlet Parameters) for each model.

* **Mask Image** (select files)  
Specify an image for explicitly masking the analysis. (optional). A sensible option here is to use a segmention of structural images to specify a within-brain mask. If you select that image as an explicit mask then only those voxels in the brain will be analysed. This both speeds the inference process and restricts BMS to within-brain voxels. Alternatively, if such structural images are unavailble or no masking is required, then leave this field empty.

* **Number of samples** (enter text)  
Number of samples used to compute exceedance probabilities (default: 1e6). To make computations faster reduce the number of samples when number of models is bigger than 3.
