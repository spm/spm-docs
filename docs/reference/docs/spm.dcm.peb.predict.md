# Predict (cross-validation)  
Builds a PEB model on all but one subjects, and uses it to predict a between-subjects effect, such as group membership, in the remaining subject. This process is repeated, leaving out each subject in turn (leave-one-out cross-validation) to establish the cross- validation accuracy of the model.   
The first covariate (after the automatically inserted mean regressor) is used as the predictor variable.   

* **Name** (enter text)  
Specify a name for the output.   

* **DCMs** (select files)  
Select group DCM file (GCM_*.mat). This is a cell array with one row per subject and one column per DCM.   

* **DCM index** (choose an option)  
If each subject has multiple DCMs, select which DCM to use for this analysis (or select all). For a standard PEB analysis, it is recommended to specify the PEB over the first DCM only, then compare models at the second level.   

    * **Selected DCM index** (enter text)  
    Select index of the DCM (within subject) on which to to build the PEB - e.g. 1 to use each subject's first DCM.   

    * **All**   
    All DCMs.   

* **Covariates** (choose an option)  
Specify between-subjects effects (covariates). The covariates may be entered all at once as a design matrix or individually. Note that if performing bayesian model comparison, only the first covariate will be treated as being of experimental interest.   
.   
Each parameter in the estimated PEB model will represent the influence of a covariate on a connection. If none is set, only the group mean will be estimated.   

    * **Specify design matrix**   
    Specify the second-level design matrix.   

        * **Design matrix** (enter text)  
        Enter or paste the N x C design matrix for N subjects and C covariates. Note that a column of ones will automatically be added to the start of the matrix, to model the group mean, if one is not found.   

        * **Covariate names** (create a list of items)  
        Enter names for each covariate (excluding the mean regressor which is added automatically).   

            * **Name** (enter text)  
            Enter a name for a covariate.   

    * **Specify covariates individually** (create a list of items)  
    Specify the second-level design matrix one    
    covariate (regressor) at a time. Note that a    
    column of ones to model the mean across subjects    
    is added automatically if one is not found.   

        * **Covariate**   
        Regressor.   

            * **Name** (enter text)  
            Enter a name for a covariate.   

            * **Value** (enter text)  
            Enter the vector of regressor values, one element    
            per subject.   

* **Fields** (choose an option)  
Select the fields of the DCM to include in the model.   
.   
A- and B-matrix: Includes all A- and B- connections   
All: Includes all fields   
Enter manually: Enter a cell array e.g. {'A','C'}   

    * **A- and B-matrix**   
    A- and B-matrix.   

    * **All**   
    All fields.   

    * **Enter manually** (enter text)  
    Enter the fields as a cell array e.g. {'A'} or {'A', 'C'} or {'B(:,:,1)', 'B(:,:,3)'}   

* **Between-subjects variability**   
Between-subjects variability over second-level parameters.   
.   
A multi-component model is used. Each component is a [p x p] precision matrix given p DCM parameters, where elements on the diagonal represent the precision (inverse variance) across subjects of each DCM connection. These precisions are set via the Within:between ratio, below. Each precision component is scaled by a hyper-parameter, which is estimated from the data. The prior expectation and uncertainty of these hyper-parameters are also set below.   

    * **Precision components** (choose from the menu)  
    The precision components to include. By default, the between-subjects variability is separately estimated for each DCM connectivity parameter.   
    Single: a single component for all DCM parameters.    
    Fields: one component per field (e.g. A,B,C)   
    All (default): one component per DCM parameter   
    None: all DCM parameters are treated as fixed effects.   

    * **Within:between ratio** (enter text)  
    Within:between variance ratio (M.beta).    
    This ratio controls the expected between-subjects variability for each connection (DCM parameter). The default is 16, meaning we expect the variability in connection strengths across subjects to be 1/16 of our uncertainty about connection strengths at the first level.   
    .   
    Internally, the prior second level covariance of DCM parameters (M.pC) is set to DCM{1}.M.pC / M.beta, where DCM{1} is the first DCM provided.   

    * **Expectation** (enter text)  
    Prior expectation of the log precision parameters (M.hE), which scale each precision component. The default is 0, which is translated internally to exp(0) = 1.   

    * **Uncertainty** (enter text)  
    Uncertainty over the prior expectation of the log precision parameters (M.hC), which scale each precision component. The default is 1/16.   

* **Second level (GLM) priors**   
Priors on the expected values of the second level General Linear Model (GLM) parameters.   

    * **Group ratio** (enter text)  
    The group ratio (M.alpha) expresses our prior uncertainty about second-level (group level GLM) parameters. The default is 1, meaning our uncertainty about the connection strengths at the second level is the same as our uncertainty about the connection strengths at the first level.    
    .   
    Internally, the prior covariance of second level parameters is set to DCM{1}.M.pC / alpha, where DCM{1} is the first DCM provided.   

* **Estimation**   
Settings for estimating the PEB model   

    * **Max iterations** (enter text)  
    Maximum iterations of the VL estimation procedure.   
    Only change this if you find the free energy is still increasing non-trivially up to the default limit of 64.   
