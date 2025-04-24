# Model Inference  
Bayesian Model Selection for Dynamic Causal Modelling (DCM) for fMRI or MEEG.  
.  
Input: DCM files (.mat) for each model, session and subject. Note that there must be identical numbers of models for all each sessions, and identical numbers of sessions for all subjects.   
.  
Output: For the fixed effects analysis, the log-evidence for each model (relative to the worst model) is plotted in the graphics window, as well as the posterior probability for each model. In addition, the corresponding values are saved in the directory specified (BMS.mat). For the random effects analysis, the expected posterior probability and exceedance probability of each model (i.e. the probability that this model is more likely than any other model) are plotted in the graphics window, and the corresponding values are saved in the directory specified. If there are multiple sessions per subject, the random effects analysis operates on the subject-specific sums of log evidences across sessions.  

* **Directory** (select files)  
Select the directory where the files containing the results from BMS (BMS.mat) will be written.  

* **Data** (create a list of items)  
Select the DCM_*.mat file for each model, session and subject.  

    * **Subject** (create a list of items)  
    .  

        * **Session**   
        .  

            * **Models** (select files)  
            Select the DCM_*.mat file for each model. DCM_*.mat files (models) should be specified in the same order for each subject and session.  

* **Load model space** (select files)  
Optional: load .mat file with all subjects, sessions and models. This option is a faster alternative to selecting the DCM.mat files for each subject/model (above in 'Data').  
This file is created if the 'Data' option has been used. It is saved in the same directory as BMS.mat and can then be loaded for future BMS/BMA analyses with the same data.  
The model space file should contain the structure 'subj'. This structure should have the field 'sess' for sessions, then the subfield 'model' and in 'model' there should be five subfields: 'fname' contains the path to the DCM.mat file, '.F' the Free Energy of that model, '.Ep' and 'Cp' the mean and covariance of the parameters estimates. Finally the subfield '.nonLin' should be 1 if the model is non-linear and 0 otherwise.  
Example: subj(3).sess(1).model(4).fname contains the path to the DCM.mat file for subject 3, session 1 and model 4. subj(3).sess(1).model(4).F contains the value of the Free Energy for the same model/session/subject.  

* **Log-evidence matrix** (select files)  
Optional: load .mat file with log-evidence values for comparison. This option is a faster alternative to selecting the DCM.mat files for each subject/model (above in 'Data') but it does not allow for Bayesian Model Averaging. To compute BMA the user needs to specify the DCM.mat files or the model space.   
This file should contain an F matrix consisting of [s x m] log-evidence values, where s is the number of subjects and m the number of models.  

* **Inference method** (choose from the menu)  
Specify inference method: random effects (2nd-level, RFX) or fixed effects (1st-level, FFX) analysis. RFX uses Gibbs sampling.  

* **Family inference** (choose an option)  
Optional field to perform family level inference.Options: load family.mat or specify family names and models using the interface.  

    * **Load family** (select files)  
    Load family.mat file. This file should contain the structure 'family' with fields 'names' and 'partition'. Example: family.names = {'F1', 'F2'} and family.partition = [1 2 2 1 1].  This structure specifies two families with names 'F1' and 'F2' and assigns model 1, 4 and 5 to the first family and models 2 and 3 to the second family.  

    * **Construct family** (create a list of items)  
    Create family. Specify family name and models.  

        * **Family**   
        Specify family name and models.  

            * **Name** (enter text)  
            Specify name for family.  

            * **Models** (enter text)  
            Specify models belonging to this family. Example: write '2 6' if the second and sixth model belong to this family.  

* **BMA** (choose an option)  
Optional field to compute Bayesian Model Averaging (BMA).  

    * **Do not compute**   
    Do not compute Bayesian Model Averaging (BMA).  

    * **Choose family** (choose an option)  
    Specify family for Bayesian Model Averaging (BMA). Options: 'winning family', 'enter family' or 'all families'.  

        * **Winning family**   
        Use winning family for Bayesian Model Averaging (BMA).  

        * **All families**   
        Use all families for Bayesian Model Averaging (BMA).  

        * **Enter family** (enter text)  
        Specify family (integer). E.g. '2' for the second family to use in BMA.   

* **Verify data identity** (choose from the menu)  
Verify whether the model comparison is valid i.e. whether the models have been fitted to the same data.  
