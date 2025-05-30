# Contrast Manager  
Specify T and F contrasts.   

* **Select SPM.mat** (select files)  
Select SPM.mat file for contrasts.   

* **Contrast Sessions** (create a list of items)  
For general linear model Y = XB + E with data Y, design matrix X, parameter vector B, and (independent) errors E, a contrast is a linear combination of the parameters c'B. Usually c is a column vector, defining a simple contrast of the parameters, assessed via an SPM{T}. More generally, c can be a matrix (a linear constraining matrix), defining an "F-contrast" assessed via an SPM{F}.   
.   
The vector/matrix c contains the contrast weights. It is this contrast weights vector/matrix that must be specified to define the contrast. The null hypothesis is that the linear combination c'B is zero. The order of the parameters in the parameter (column) vector B, and hence the order to which parameters are referenced in the contrast weights vector c, is determined by the construction of the design matrix.   
.   
There are two types of contrast in SPM: simple contrasts for SPM{T}, and "F-contrasts" for SPM{F}.   
.   
For a thorough theoretical treatment, see the Human Brain Function book and the statistical literature referenced therein.   
.   
.   
* Non-orthogonal designs   
.   
Note that parameters zero-weighted in the contrast are still included in the model. This is particularly important if the design is not orthogonal (i.e. the columns of the design matrix are not orthogonal). In effect, the significance of the contrast is assessed *after* accounting for the other effects in the design matrix. Thus, if two covariates are correlated, testing the significance of the parameter associated with one will only test for the part that is not present in the second covariate. This is a general point that is also true for F-contrasts. See Andrade et al, Ambiguous results in functional neuroimaging, NeuroImage, 1999, for a full description of the effect of non orthogonal design testing.   
.   
.   
* Estimability   
.   
The contrast c'B is estimated by c'b, where b are the parameter estimates given by b=pinv(X)*Y.   
.   
However, if a design is rank-deficient (i.e. the columns of the design matrix are not linearly independent), then the parameters are not unique, and not all linear combinations of the parameter are valid contrasts, since contrasts must be uniquely estimable.   
.   
A weights vector defines a valid contrast if and only if it can be constructed as a linear combination of the rows of the design matrix. That is c' (the transposed contrast vector - a row vector) is in the row-space of the design matrix.   
.   
Usually, a valid contrast will have weights that sum to zero over the levels of a factor (such as condition).   
.   
A simple example is a simple two condition design including a constant, with design matrix   
.   
          [ 1 0 1 ]   
          [ : : : ]   
     X =  [ 1 0 1 ]   
          [ 0 1 1 ]   
          [ : : : ]   
          [ 0 1 1 ]   
.   
The first column corresponds to condition 1, the second to condition 2, and the third to a constant (mean) term. Although there are three columns to the design matrix, the design only has two degrees of freedom, since any one column can be derived from the other two (for instance, the third column is the sum of the first two). There is no unique set of parameters for this model, since for any set of parameters adding a constant to the two condition effects and subtracting it from the constant effect yields another set of viable parameters. However, the difference between the two condition effects is uniquely estimated, so c'=[-1,+1,0] does define a contrast.   
.   
If a parameter is estimable, then the weights vector with a single "1" corresponding to that parameter (and zero elsewhere) defines a valid contrast.   
.   
.   
* Multiple comparisons   
.   
Note that SPM implements no corrections to account for you looking at multiple contrasts.   
.   
If you are interested in a set of hypotheses that together define a consistent question, then you should account for this when assessing the individual contrasts. A simple Bonferroni approach would assess N simultaneous contrasts at significance level alpha/N, where alpha is the chosen significance level (usually 0.05).   
.   
For two sided t-tests using SPM{T}s, the significance level should be halved. When considering both SPM{T}s produced by a contrast and it's inverse (the contrast with negative weights), to effect a two-sided test to look for both "increases" and "decreases", you should review each SPM{T} at at level 0.05/2 rather than 0.05. (Or consider an F-contrast!)   
.   
.   
* Contrast images and ESS images   
.   
For a simple contrast, SPM (spm_getSPM.m) writes a contrast image: con_????.{img,nii}, with voxel values c'b. (The ???? in the image names are replaced with the contrast number.) These contrast images (for appropriate contrasts) are suitable summary images of an effect at this level, and can be used as input at a higher level when effecting a random effects analysis.   
.   
For an F-contrast, SPM (spm_getSPM.m) writes the Extra Sum-of-Squares (the difference in the residual sums of squares for the full and reduced model) as ess_????.{img,nii}. (Note that the ess_????.{img,nii} and SPM{T,F}_????.{img,nii} images are not suitable input for a higher level analysis.)   

    * **T-contrast**   
    * Simple one-dimensional contrasts for an SPM{T}   
    .   
    A simple contrast for an SPM{T} tests the null hypothesis c'B=0 against the one-sided alternative c'B>0, where c is a column vector.    
    .   
        Note that throughout SPM, the transpose of the contrast weights is used for display and input. That is, you'll enter and visualise c'. For an SPM{T} this will be a row vector.   
    .   
    For example, if you have a design in which the first two columns of the design matrix correspond to the effects for "baseline" and "active" conditions respectively, then a contrast with weights c'=[-1,+1,0,...] (with zero weights for any other parameters) tests the hypothesis that there is no "activation" (the parameters for both conditions are the same), against the alternative that there is some activation (i.e. the parameter for the "active" condition is greater than that for the "baseline" condition). The resulting SPM{T} (created by spm_getSPM.m) is a statistic image, with voxel values the value of the t-statistic for the specified contrast at that location. Areas of the SPM{T} with high voxel values indicate evidence for "activation". To look for areas of relative "de-activation", the inverse contrast could be used c'=[+1,-1,0,...].   
    .   
    Similarly, if you have a design where the third column in the design matrix is a covariate, then the corresponding parameter is essentially a regression slope, and a contrast with weights c'=[0,0,1,0,...] (with zero weights for all parameters but the third) tests the hypothesis of zero regression slope, against the alternative of a positive slope. This is equivalent to a test no correlation, against the alternative of positive correlation. If there are other terms in the model beyond a constant term and the covariate, then this correlation is a partial correlation, the correlation between the data Y and the covariate, after accounting for the other effects.   

        * **Name** (enter text)  
        Name of contrast.   

        * **Weights vector** (enter text)  
        Enter T-contrast weights vector.   
        This is done similarly to the contrast manager. A 1 x n vector should be entered for T-contrasts.   
        Contrast weight vectors will be padded with zeros to the correct length.   

        * **Replicate over sessions** (choose from the menu)  
        If there are multiple sessions with identical conditions, one might want to specify contrasts which are identical over sessions. This can be done automatically based on the contrast spec for one session.   
        Contrasts can be either replicated (thus testing average effects over sessions) or created per session. In both cases, zero padding up to the length of each session and the block effects is done automatically. In addition, weights of replicated contrasts can be scaled by the number of sessions. This allows to use the same contrast manager batch for fMRI analyses with a variable number of sessions. The scaled contrasts can then be compared in a 2nd level model without a need for further adjustment of effect sizes.   

    * **F-contrast**   
    * Linear constraining matrices for an SPM{F}   
    .   
    The null hypothesis c'B=0 can be thought of as a (linear) constraint on the full model under consideration, yielding a reduced model. Taken from the viewpoint of two designs, with the full model an extension of the reduced model, the null hypothesis is that the additional terms in the full model are redundant.   
    .   
    Statistical inference proceeds by comparing the additional variance explained by full design over and above the reduced design to the error variance (of the full design), an "Extra Sum-of-Squares" approach yielding an F-statistic for each voxel, whence an SPM{F}.   
    .   
    This is useful in a number of situations:   
    .   
    * Two sided tests   
    .   
    The simplest use of F-contrasts is to effect a two-sided test of a simple linear contrast c'B, where c is a column vector. The SPM{F} is the square of the corresponding SPM{T}. High values of the SPM{F} therefore indicate evidence against the null hypothesis c'B=0 in favour of the two-sided alternative c'B~=0.   
    .   
    * General linear hypotheses   
    .   
    Where the contrast weights is a matrix, the rows of the (transposed) contrast weights matrix c' must define contrasts in their own right, and the test is effectively simultaneously testing the null hypotheses associated with the individual component contrasts with weights defined in the rows. The null hypothesis is still c'B=0, but since c is a matrix, 0 here is a zero vector rather than a scalar zero, asserting that under the null hypothesis all the component hypotheses are true.   
    .   
    For example: Suppose you have a language study with 3 word categories (A,B & C), and would like to test whether there is any difference at all between the three levels of the "word category" factor.   
    .   
    The design matrix might look something like:   
    .   
             [ 1 0 0 ..]   
             [ : : : ..]   
             [ 1 0 0 ..]   
             [ 0 1 0 ..]   
        X =  [ : : : ..]   
             [ 0 1 0 ..]   
             [ 0 0 1 ..]   
             [ : : : ..]   
             [ 0 0 1 ..]   
             [ 0 0 0 ..]   
             [ : : : ..]   
    .   
     ...with the three levels of the "word category" factor modelled in the  first three columns of the design matrix.   
    .   
    The matrix of contrast weights will look like:   
    .   
     c' = [1 -1  0 ...;   
           0  1 -1 ...]   
    .   
    Reading the contrasts weights in each row of c', we see that row 1 states that category A elicits the same response as category B, row 2 that category B elicits the same response as category C, and hence together than categories A, B & C all elicit the same response.   
    .   
    The alternative hypothesis is simply that the three levels are not all the same, i.e. that there is some difference in the parameters for the three levels of the factor: The first and the second categories produce different brain responses, OR the second and third categories, or both.   
    .   
    In other words, under the null hypothesis (the categories produce the same brain responses), the model reduces to one in which the three level "word category" factor can be replaced by a single "word" effect, since there is no difference in the parameters for each category. The corresponding design matrix would have the first three columns replaced by a single column that is the sum (across rows) of the first three columns in the design matrix above, modelling the brain response to a word, whatever is the category. The F-contrast above is in fact testing the hypothesis that this reduced design doesn't account for significantly less variance than the full design with an effect for each word category.   
    .   
    Another way of seeing that, is to consider a reparameterisation of the model, where the first column models effects common to all three categories, with the second and third columns modelling the differences between the three conditions, for example:   
    .   
             [ 1  1  0 ..]   
             [ :  :  : ..]   
             [ 1  1  0 ..]   
             [ 1  0  1 ..]   
        X =  [ :  :  : ..]   
             [ 1  0  1 ..]   
             [ 1 -1 -1 ..]   
             [ :  :  : ..]   
             [ 1 -1 -1 ..]   
             [ 0  0  0 ..]   
             [ :  :  : ..]   
    .   
    In this case, an equivalent F contrast is of the form   
     c' = [ 0 1 0 ...;   
            0 0 1 ...]   
    and would be exactly equivalent to the previous contrast applied to the previous design. In this latter formulation, you are asking whewher the two columns modelling the "interaction space" account for a significant amount of variation (variance) of the data. Here the component contrasts in the rows of c' are simply specifying that the parameters for the corresponding rows are are zero, and it is clear that the F-test is comparing this full model with a reduced model in which the second and third columns of X are omitted.   
    .   
        Note the difference between the following two F-contrasts:   
             c' = [ 0 1 0 ...;     (1)   
                    0 0 1 ...]   
         and   
             c' = [ 0 1 1 ...]     (2)   
    .   
        The first is an F-contrast, testing whether either of the parameters for the effects modelled in the 2nd & 3rd columns of the design matrix are significantly different from zero. Under the null hypothesis c'B=0, the first contrast imposes a two-dimensional constraint on the design. The second contrast tests whether the SUM of the parameters for the 2nd & 3rd columns is significantly different from zero. Under the null hypothesis c'B=0, this second contrast only imposes a one dimensional constraint on the design.   
    .   
        An example of the difference between the two is that the first contrast would be sensitive to the situation where the 2nd & 3rd parameters were +a and -a, for some constant a, whereas the second contrast would not detect this, since the parameters sum to zero.   
    .   
    The test for an effect of the factor "word category" is an F-test with 3-1=2 "dimensions", or degrees of freedom.   
    .   
    * Testing the significance of effects modelled by multiple columns   
    .   
    A conceptually similar situation arises when one wonders whether a set of confound effects are explaining any variance in the data. One important advantage of testing the with F contrasts rather than one by one using SPM{T}'s is the following. Say you have two covariates that you would like to know whether they can "predict" the brain responses, and these two are correlated (even a small correlation would be important in this instance). Testing one and then the other may lead you to conclude that there is no effect. However, testing with an F test the two covariates may very well show a not suspected effect. This is because by testing one covariate after the other, one never tests for what is COMMON to these covariates (see Andrade et al, Ambiguous results in functional neuroimaging, NeuroImage, 1999).   
    .   
    .   
    More generally, F-tests reflect the usual analysis of variance, while t-tests are traditionally post hoc tests, useful to see in which direction is an effect going (positive or negative). The introduction of F-tests can also be viewed as a first means to do model selection.   
    .   
    .   
    Technically speaking, an F-contrast defines a number of directions (as many as the rank of the contrast) in the space spanned by the column vectors of the design matrix. These directions are simply given by X*c if the vectors of X are orthogonal, if not, the space define by c is a bit more complex and takes care of the correlation within the design matrix. In essence, an F-contrast is defining a reduced model by imposing some linear constraints (that have to be estimable, see below) on the parameters estimates. Sometimes, this reduced model is simply made of a subset of the column of the original design matrix but generally, it is defined by a combination of those columns. (see spm_FcUtil for what (I hope) is an efficient handling of F-contrasts computation).   

        * **Name** (enter text)  
        Name of contrast.   

        * **Weights matrix** (enter text)  
        Enter F-contrast weights matrix.   
        This is done similarly to the contrast manager.   
        Contrast weight matrices will be padded with zeros to the correct length.   

        * **Replicate over sessions** (choose from the menu)  
        If there are multiple sessions with identical conditions, one might want to specify contrasts which are identical over sessions. This can be done automatically based on the contrast spec for one session.   
        Contrasts can be either replicated (thus testing average effects over sessions) or created per session. In both cases, zero padding up to the length of each session and the block effects is done automatically. In addition, weights of replicated contrasts can be scaled by the number of sessions. This allows to use the same contrast manager batch for fMRI analyses with a variable number of sessions. The scaled contrasts can then be compared in a 2nd level model without a need for further adjustment of effect sizes.   

    * **F-contrast (reduced model)**   
    An alternative way of specifying an F-contrast by listing the column indices for a reduced model.   
    .   
    The contrast test for the significant contribution of the columns that are listed if the Include option is chosen or that are not listed if the Exclude option is chosen   

        * **Name** (enter text)  
        Name of contrast.   

        * **Columns list** (enter text)  

        * **Specification mode** (choose from the menu)  
        How to specify the reduced model.   
        .   
        Test for the contribution of the listed columns (Include)   
        Test for the contribution of unlisted columns (Exclude, default)   

        * **Replicate over sessions** (choose from the menu)  
        If there are multiple sessions with identical conditions, one might want to specify contrasts which are identical over sessions. This can be done automatically based on the contrast spec for one session.   
        Contrasts can be either replicated (thus testing average effects over sessions) or created per session. In both cases, zero padding up to the length of each session and the block effects is done automatically. In addition, weights of replicated contrasts can be scaled by the number of sessions. This allows to use the same contrast manager batch for fMRI analyses with a variable number of sessions. The scaled contrasts can then be compared in a 2nd level model without a need for further adjustment of effect sizes.   

    * **T-contrast (cond/sess based)**   
    Define a contrast in terms of conditions or regressors instead of columns of the design matrix. This allows to create contrasts automatically even if some columns are not always present (e.g. parametric modulations).   
    .   
    Each contrast column can be addressed by specifying   
    * session number   
    * condition number   
    * basis function number   
    * parametric modulation number and   
    * parametric modulation order.   
    .   
    If the design is specified without time or parametric modulation, SPM creates a "pseudo-modulation" with order zero. To put a contrast weight on a basis function one therefore has to enter "1" for parametric modulation number and "0" for parametric modulation order.   
    .   
    Time and parametric modulations are not distinguished internally. If time modulation is present, it will be parametric modulation "1", and additional parametric modulations will be numbered starting with "2".   
    .   
    * Simple one-dimensional contrasts for an SPM{T}   
    .   
    A simple contrast for an SPM{T} tests the null hypothesis c'B=0 against the one-sided alternative c'B>0, where c is a column vector.    
    .   
        Note that throughout SPM, the transpose of the contrast weights is used for display and input. That is, you'll enter and visualise c'. For an SPM{T} this will be a row vector.   
    .   
    For example, if you have a design in which the first two columns of the design matrix correspond to the effects for "baseline" and "active" conditions respectively, then a contrast with weights c'=[-1,+1,0,...] (with zero weights for any other parameters) tests the hypothesis that there is no "activation" (the parameters for both conditions are the same), against the alternative that there is some activation (i.e. the parameter for the "active" condition is greater than that for the "baseline" condition). The resulting SPM{T} (created by spm_getSPM.m) is a statistic image, with voxel values the value of the t-statistic for the specified contrast at that location. Areas of the SPM{T} with high voxel values indicate evidence for "activation". To look for areas of relative "de-activation", the inverse contrast could be used c'=[+1,-1,0,...].   
    .   
    Similarly, if you have a design where the third column in the design matrix is a covariate, then the corresponding parameter is essentially a regression slope, and a contrast with weights c'=[0,0,1,0,...] (with zero weights for all parameters but the third) tests the hypothesis of zero regression slope, against the alternative of a positive slope. This is equivalent to a test no correlation, against the alternative of positive correlation. If there are other terms in the model beyond a constant term and the covariate, then this correlation is a partial correlation, the correlation between the data Y and the covariate, after accounting for the other effects.   

        * **Name** (enter text)  
        Name of contrast.   

        * **Contrast columns** (choose an option)  
        Contrasts can be specified either over conditions or over extra regressors.   

            * **T contrast for conditions** (create a list of items)  
            Assemble your contrast column by column.   

                * **Contrast entry**   

                    * **Contrast weight** (enter text)  
                    The contrast weight for the selected column.   

                    * **Condition \#** (enter text)  
                    Select which condition function set is to be contrasted.   

                    * **Basis function \#** (enter text)  
                    Select which basis function from the basis function set is to be contrasted.   

                    * **Parametric modulation \#** (enter text)  
                    Select which parametric modulation is to be contrasted. If there is no time/parametric modulation, enter "1". If there are both time and parametric modulations, then time modulation comes before parametric modulation.   

                    * **Parametric modulation order** (enter text)  
                    Order of parametric modulation to be contrasted.    
                    .   
                    0 - the basis function itself, 1 - 1st order mod etc   

            * **T contrast for extra regressors** (enter text)  
            Enter T contrast vector for extra regressors.   

        * **Session(s)** (enter text)  
        Enter session number(s) for which this contrast should be created. If more than one session number is specified, the contrast will be an average contrast over the specified conditions or regressors from these sessions.   

* **Delete existing contrasts** (choose from the menu)  
Delete existing contrasts.   
