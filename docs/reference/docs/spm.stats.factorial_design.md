# Factorial design specification  
Configuration of the design matrix, describing the general linear model, data specification, and other parameters necessary for the statistical analysis.   
These parameters are saved in a configuration file (SPM.mat), which can then be passed on to spm_spm.m which estimates the design. This is achieved by pressing the 'Estimate' button. Inference on these estimated parameters is then handled by the SPM results section.    

This interface is used for setting up analyses of PET data, morphometric data, or 'second level' ('random effects') fMRI data, where first level models can be used to produce appropriate summary data that are then used as raw data for the second-level analysis. For example, a simple t-test on contrast images from the first-level turns out to be a random-effects analysis with random subject effects, inferring for the population based on a particular sample of subjects.   

A separate interface handles design configuration for first level fMRI time series.   

Various data and parameters need to be supplied to specify the design (1) the image files, (2) indicators of the corresponding condition/subject/group (2) any covariates, nuisance variables, or design matrix partitions (3) the type of global normalisation (if any) (4) grand mean scaling options (5) thresholds and masks defining the image volume to analyse. The interface supports a comprehensive range of options for all these parameters.   

* **Directory** (select files)  
Select a directory where the SPM.mat file containing the specified design matrix will be written.   

* **Design** (choose an option)  


    * **One-sample t-test**   
    One-sample t-test.   

        * **Scans** (select files)  
        Select the images.  They must all have the same image dimensions, orientation, voxel size etc.   

    * **Two-sample t-test**   
    Two-sample t-test.   

        * **Group 1 scans** (select files)  
        Select the images from sample 1.  They must all have the same image dimensions, orientation, voxel size etc.   

        * **Group 2 scans** (select files)  
        Select the images from sample 2.  They must all have the same image dimensions, orientation, voxel size etc.   

        * **Independence** (choose from the menu)  
        By default, the measurements are assumed to be independent between levels.    

        If you change this option to allow for dependencies, this will violate the assumption of sphericity. It would therefore be an example of non-sphericity. One such example would be where you had repeated measurements from the same subjects - it may then be the case that, over subjects, measure 1 is correlated to measure 2.    

        Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

        * **Variance** (choose from the menu)  
        By default, the measurements in each level are assumed to have unequal variance.    

        This violates the assumption of 'sphericity' and is therefore an example of 'non-sphericity'.   

        This can occur, for example, in a 2nd-level analysis of variance, one contrast may be scaled differently from another.  Another example would be the comparison of qualitatively different dependent variables (e.g. normals vs. patients).  Different variances (heteroscedasticy) induce different error covariance components that are estimated using restricted maximum likelihood (see below).   

        Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

        * **Grand mean scaling** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'grand mean scaling by factor' which could be eg. 'grand mean scaling by subject' if the factor is 'subject'.    

        Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" to obtain a combination of between subject proportional scaling and within subject AnCova.    

        * **ANCOVA** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'ANCOVA-by-factor' regressors. This includes eg. 'Ancova by subject' or 'Ancova by effect'. These options allow eg. different subjects to have different relationships between local and global measurements.    

    * **Paired t-test**   
    Paired t-test.   

        * **Pairs** (create a list of items)  


            * **Pair**   
            Add a new pair of scans to your experimental design.   

                * **Scans [1,2]** (select files)  
                Select the pair of images.   

        * **Grand mean scaling** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'grand mean scaling by factor' which could be eg. 'grand mean scaling by subject' if the factor is 'subject'.    

        Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" to obtain a combination of between subject proportional scaling and within subject AnCova.    

        * **ANCOVA** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'ANCOVA-by-factor' regressors. This includes eg. 'Ancova by subject' or 'Ancova by effect'. These options allow eg. different subjects to have different relationships between local and global measurements.    

    * **Multiple regression**   
    Multiple regression.   

        * **Scans** (select files)  
        Select the images.  They must all have the same image dimensions, orientation, voxel size etc.   

        * **Covariates** (create a list of items)  
        Covariates   

            * **Covariate**   
            Add a new covariate to your experimental design.   

                * **Vector** (enter text)  
                Vector of covariate values.   

                * **Name** (enter text)  
                Name of covariate.   

                * **Centering** (choose from the menu)  
                Centering refers to subtracting the mean (central) value from the covariate values, which is equivalent to orthogonalising the covariate with respect to the constant column.   

                Subtracting a constant from a covariate changes the beta for the constant term, but not that for the covariate. In the simplest case, centering a covariate in a simple regression leaves the slope unchanged, but converts the intercept from being the modelled value when the covariate was zero, to being the modelled value at the mean of the covariate, which is often more easily interpretable. For example, the modelled value at the subjects' mean age is usually more meaningful than the (extrapolated) value at an age of zero.   

                If a covariate value of zero is interpretable and/or you wish to preserve the values of the covariate then choose 'No centering'. You should also choose not to center if you have already subtracted some suitable value from your covariate, such as a commonly used reference level or the mean from another (e.g. larger) sample.   


        * **Intercept** (choose from the menu)  
        By default, an intercept is always added to the model. If the covariates supplied by the user include a constant effect, the intercept may be omitted.   

    * **One-way ANOVA**   
    One-way Analysis of Variance (ANOVA).   

        * **Cells** (create a list of items)  
        Enter the scans a cell at a time.   

            * **Cell**   
            Enter data for a cell in your design.   

                * **Scans** (select files)  
                Select the images for this cell.  They must all have the same image dimensions, orientation, voxel size etc.   

        * **Independence** (choose from the menu)  
        By default, the measurements are assumed to be independent between levels.    

        If you change this option to allow for dependencies, this will violate the assumption of sphericity. It would therefore be an example of non-sphericity. One such example would be where you had repeated measurements from the same subjects - it may then be the case that, over subjects, measure 1 is correlated to measure 2.    

        Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

        * **Variance** (choose from the menu)  
        By default, the measurements in each level are assumed to have unequal variance.    

        This violates the assumption of 'sphericity' and is therefore an example of 'non-sphericity'.   

        This can occur, for example, in a 2nd-level analysis of variance, one contrast may be scaled differently from another.  Another example would be the comparison of qualitatively different dependent variables (e.g. normals vs. patients).  Different variances (heteroscedasticy) induce different error covariance components that are estimated using restricted maximum likelihood (see below).   

        Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

        * **Grand mean scaling** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'grand mean scaling by factor' which could be eg. 'grand mean scaling by subject' if the factor is 'subject'.    

        Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" to obtain a combination of between subject proportional scaling and within subject AnCova.    

        * **ANCOVA** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'ANCOVA-by-factor' regressors. This includes eg. 'Ancova by subject' or 'Ancova by effect'. These options allow eg. different subjects to have different relationships between local and global measurements.    

    * **One-way ANOVA - within subject**   
    One-way Analysis of Variance (ANOVA) - within subject.   

        * **Subjects** (create a list of items)  


            * **Subject**   
            Enter data and conditions for a new subject.   

                * **Scans** (select files)  
                Select the images to be analysed.  They must all have the same image dimensions, orientation, voxel size etc.   

                * **Conditions** (enter text)  


        * **Independence** (choose from the menu)  
        By default, the measurements are assumed to be dependent between levels.    

        If you change this option to allow for dependencies, this will violate the assumption of sphericity. It would therefore be an example of non-sphericity. One such example would be where you had repeated measurements from the same subjects - it may then be the case that, over subjects, measure 1 is correlated to measure 2.    

        Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

        * **Variance** (choose from the menu)  
        By default, the measurements in each level are assumed to have unequal variance.    

        This violates the assumption of 'sphericity' and is therefore an example of 'non-sphericity'.   

        This can occur, for example, in a 2nd-level analysis of variance, one contrast may be scaled differently from another.  Another example would be the comparison of qualitatively different dependent variables (e.g. normals vs. patients).  Different variances (heteroscedasticy) induce different error covariance components that are estimated using restricted maximum likelihood (see below).   

        Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

        * **Grand mean scaling** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'grand mean scaling by factor' which could be eg. 'grand mean scaling by subject' if the factor is 'subject'.    

        Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" to obtain a combination of between subject proportional scaling and within subject AnCova.    

        * **ANCOVA** (choose from the menu)  
        This option is for PET or VBM data (not second level fMRI).   

        Selecting YES will specify 'ANCOVA-by-factor' regressors. This includes eg. 'Ancova by subject' or 'Ancova by effect'. These options allow eg. different subjects to have different relationships between local and global measurements.    

    * **Full factorial**   
    This option is best used when you wish to test for all main effects and interactions in one-way, two-way or three-way ANOVAs. Design specification proceeds in 2 stages. Firstly, by creating new factors and specifying the number of levels and name for each. Nonsphericity, ANOVA-by-factor and scaling options can also be specified at this stage. Secondly, scans are assigned separately to each cell. This accommodates unbalanced designs.   

    For example, if you wish to test for a main effect in the population from which your subjects are drawn and have modelled that effect at the first level using K basis functions (eg. K=3 informed basis functions) you can use a one-way ANOVA with K-levels. Create a single factor with K levels and then assign the data to each cell eg. canonical, temporal derivative and dispersion derivative cells, where each cell is assigned scans from multiple subjects.   

    SPM will also automatically generate the contrasts necessary to test for all main effects and interactions.   

        * **Factors** (create a list of items)  
        Specify your design a factor at a time.   

            * **Factor**   
            Add a new factor to your experimental design.   

                * **Name** (enter text)  
                Name of factor, eg. 'Repetition'.   

                * **Levels** (enter text)  
                Enter number of levels for this factor, eg. 2.   

                * **Independence** (choose from the menu)  
                By default, the measurements are assumed to be independent between levels.    

                If you change this option to allow for dependencies, this will violate the assumption of sphericity. It would therefore be an example of non-sphericity. One such example would be where you had repeated measurements from the same subjects - it may then be the case that, over subjects, measure 1 is correlated to measure 2.    

                Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

                * **Variance** (choose from the menu)  
                By default, the measurements in each level are assumed to have unequal variance.    

                This violates the assumption of 'sphericity' and is therefore an example of 'non-sphericity'.   

                This can occur, for example, in a 2nd-level analysis of variance, one contrast may be scaled differently from another.  Another example would be the comparison of qualitatively different dependent variables (e.g. normals vs. patients).  Different variances (heteroscedasticy) induce different error covariance components that are estimated using restricted maximum likelihood (see below).   

                Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

                * **Grand mean scaling** (choose from the menu)  
                This option is for PET or VBM data (not second level fMRI).   

                Selecting YES will specify 'grand mean scaling by factor' which could be eg. 'grand mean scaling by subject' if the factor is 'subject'.    

                Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" to obtain a combination of between subject proportional scaling and within subject AnCova.    

                * **ANCOVA** (choose from the menu)  
                This option is for PET or VBM data (not second level fMRI).   

                Selecting YES will specify 'ANCOVA-by-factor' regressors. This includes eg. 'Ancova by subject' or 'Ancova by effect'. These options allow eg. different subjects to have different relationships between local and global measurements.    

        * **Cells** (create a list of items)  
        Enter the scans a cell at a time.   

            * **Cell**   
            Enter data for a cell in your design.   

                * **Levels** (enter text)  
                Enter a vector or scalar that specifies which cell in the factorial design these images belong to. The length of this vector should correspond to the number of factors in the design   

                For example, length 2 vectors should be used for two-factor designs eg. the vector [2 3] specifies the cell corresponding to the 2nd-level of the first factor and the 3rd level of the 2nd factor.   

                * **Scans** (select files)  
                Select the images for this cell.  They must all have the same image dimensions, orientation, voxel size etc.   

        * **Generate contrasts** (choose from the menu)  
        Automatically generate the contrasts necessary to test for all main effects and interactions.   

    * **Flexible factorial**   
    Create a design matrix a block at a time by specifying which main effects and interactions you wish to be included.   

    This option is best used for one-way, two-way or three-way ANOVAs but where you do not wish to test for all possible main effects and interactions. This is perhaps most useful for PET where there is usually not enough data to test for all possible effects. Or for 3-way ANOVAs where you do not wish to test for all of the two-way interactions. A typical example here would be a group-by-drug-by-task analysis where, perhaps, only (i) group-by-drug or (ii) group-by-task interactions are of interest. In this case it is only necessary to have two-blocks in the design matrix - one for each interaction. The three-way interaction can then be tested for using a contrast that computes the difference between (i) and (ii).   

    Design specification then proceeds in 3 stages. Firstly, factors are created and names specified for each. Nonsphericity, ANOVA-by-factor and scaling options can also be specified at this stage.   

    Secondly, a list of scans is produced along with a factor matrix, I. This is an nscan x 4 matrix of factor level indicators (see xX.I below). The first factor must be 'replication' but the other factors can be anything. Specification of I and the scan list can be achieved in one of two ways (a) the 'Specify All' option allows I to be typed in at the user interface or (more likely) loaded in from the matlab workspace. All of the scans are then selected in one go. (b) the 'Subjects' option allows you to enter scans a subject at a time. The corresponding experimental conditions (ie. levels of factors) are entered at the same time. SPM will then create the factor matrix I. This style of interface is similar to that available in SPM2.   

    Thirdly, the design matrix is built up a block at a time. Each block can be a main effect or a (two-way) interaction.    

        * **Factors** (create a list of items)  
        Specify your design a factor at a time.   

            * **Factor**   
            Add a new factor to your design.   

            If you are using the 'Subjects' option to specify your scans and conditions, you may wish to make use of the following facility. There are two reserved words for the names of factors. These are 'subject' and 'repl' (standing for replication). If you use these factor names then SPM will automatically create replication and/or subject factors without you having to type in an extra entry in the condition vector.   

            For example, if you wish to model Subject and Task effects (two factors), under Subjects->Subject->Conditions you should simply type in eg. [1 2 1 2] to specify just the 'Task' factor level, instead of, eg. for the 4th subject the matrix [4 1;4 2;4 1;4 2].   

                * **Name** (enter text)  
                Name of factor, eg. 'Repetition'.   

                * **Independence** (choose from the menu)  
                By default, the measurements are assumed to be independent between levels.    

                If you change this option to allow for dependencies, this will violate the assumption of sphericity. It would therefore be an example of non-sphericity. One such example would be where you had repeated measurements from the same subjects - it may then be the case that, over subjects, measure 1 is correlated to measure 2.    

                Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

                * **Variance** (choose from the menu)  
                By default, the measurements in each level are assumed to have unequal variance.    

                This violates the assumption of 'sphericity' and is therefore an example of 'non-sphericity'.   

                This can occur, for example, in a 2nd-level analysis of variance, one contrast may be scaled differently from another.  Another example would be the comparison of qualitatively different dependent variables (e.g. normals vs. patients).  Different variances (heteroscedasticy) induce different error covariance components that are estimated using restricted maximum likelihood (see below).   

                Restricted Maximum Likelihood (REML): The ensuing covariance components will be estimated using ReML in spm_spm (assuming the same for all responsive voxels) and used to adjust the statistics and degrees of freedom during inference. By default spm_spm will use weighted least squares to produce Gauss-Markov or Maximum likelihood estimators using the non-sphericity structure specified at this stage. The components will be found in SPM.xVi and enter the estimation procedure exactly as the serial correlations in fMRI models.   

                * **Grand mean scaling** (choose from the menu)  
                This option is for PET or VBM data (not second level fMRI).   

                Selecting YES will specify 'grand mean scaling by factor' which could be eg. 'grand mean scaling by subject' if the factor is 'subject'.    

                Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" to obtain a combination of between subject proportional scaling and within subject AnCova.    

                * **ANCOVA** (choose from the menu)  
                This option is for PET or VBM data (not second level fMRI).   

                Selecting YES will specify 'ANCOVA-by-factor' regressors. This includes eg. 'Ancova by subject' or 'Ancova by effect'. These options allow eg. different subjects to have different relationships between local and global measurements.    

        * **Specify Subjects or all Scans & Factors** (choose an option)  


            * **Subjects** (create a list of items)  


                * **Subject**   
                Enter data and conditions for a new subject.   

                    * **Scans** (select files)  
                    Select the images to be analysed.  They must all have the same image dimensions, orientation, voxel size etc.   

                    * **Conditions** (enter text)  


            * **Specify all**   
            Specify (i) all scans in one go and (ii) all conditions using a factor matrix, I. This option is for 'power users'. The matrix I must have four columns and as as many rows as scans. It has the same format as SPM's internal variable SPM.xX.I.    

            The first column of I denotes the replication number and entries in the other columns denote the levels of each experimental factor.   

            So, for eg. a two-factor design the first column denotes the replication number and columns two and three have entries like 2 3 denoting the 2nd level of the first factor and 3rd level of the second factor. The 4th column in I would contain all 1s.   

                * **Scans** (select files)  
                Select the images to be analysed.  They must all have the same image dimensions, orientation, voxel size etc.   

                * **Factor matrix** (enter text)  
                Specify factor/level matrix as a nscan-by-4 matrix. Note that the first column of I is reserved for the internal replication factor and must not be used for experimental factors.   

        * **Main effects & Interactions** (create a list of items)  


            * **Main effect**   
            Add a main effect to your design matrix.   

                * **Factor number** (enter text)  
                Enter the number of the factor.   

            * **Interaction**   
            Add an interaction to your design matrix.   

                * **Factor numbers** (enter text)  
                Enter the numbers of the factors of this (two-way) interaction.   

* **Covariates** (create a list of items)  
This option allows for the specification of covariates and nuisance variables (note that SPM does not make any distinction between effects of interest (including covariates) and nuisance effects).   

    * **Covariate**   
    Add a new covariate to your experimental design.   

        * **Vector** (enter text)  
        Vector of covariate values.   
        Enter the covariate values ''per subject'' (i.e. all for subject 1, then all for subject 2, etc). Importantly, the ordering of the cells of a factorial design has to be the same for all subjects in order to be consistent with the ordering of the covariate values.   

        * **Name** (enter text)  
        Name of covariate.   

        * **Interactions** (choose from the menu)  
        For each covariate you have defined, there is an opportunity to create an additional regressor that is the interaction between the covariate and a chosen experimental factor.    

        * **Centering** (choose from the menu)  
        Centering, in the simplest case, refers to subtracting the mean (central) value from the covariate values, which is equivalent to orthogonalising the covariate with respect to the constant column.   

        Subtracting a constant from a covariate changes the beta for the constant term, but not that for the covariate. In the simplest case, centering a covariate in a simple regression leaves the slope unchanged, but converts the intercept from being the modelled value when the covariate was zero, to being the modelled value at the mean of the covariate, which is often more easily interpretable. For example, the modelled value at the subjects' mean age is usually more meaningful than the (extrapolated) value at an age of zero.   

        If a covariate value of zero is interpretable and/or you wish to preserve the values of the covariate then choose 'No centering'. You should also choose not to center if you have already subtracted some suitable value from your covariate, such as a commonly used reference level or the mean from another (e.g. larger) sample. Note that 'User specified value' has no effect, but is present for compatibility with earlier SPM versions.   

        Other centering options should only be used in special cases. More complicated centering options can orthogonalise a covariate or a covariate-factor interaction with respect to a factor, in which case covariate values within a particular level of a factor have their mean over that level subtracted. As in the simple case, such orthogonalisation changes the betas for the factor used to orthogonalise, not those for the covariate/interaction being orthogonalised. This therefore allows an added covariate/interaction to explain some otherwise unexplained variance, but without altering the group difference from that without the covariate/interaction. This is usually *inappropriate* except in special cases. One such case is with two groups and covariate that only has meaningful values for one group (such as a disease severity score that has no meaning for a control group); centering the covariate by the group factor centers the values for the meaningful group and (appropriately) zeroes the values for the other group.   


* **Multiple covariates** (create a list of items)  
This option allows for the specification of multiple covariates from TXT/MAT files.   

    * **Covariates**   
    Add a new set of covariates to your experimental design.   

        * **File(s)** (select files)  
        Select the *.mat/*.txt file(s) containing details of your multiple covariates.    

        You will first need to create a *.mat file containing a matrix R or a *.txt file containing the covariates. Each column of R will contain a different covariate. Unless the covariates names are given in a cell array called 'names' in the MAT-file containing variable R, the covariates will be named R1, R2, R3, ..etc.   

        * **Interactions** (choose from the menu)  
        For each covariate you have defined, there is an opportunity to create an additional regressor that is the interaction between the covariate and a chosen experimental factor.    

        * **Centering** (choose from the menu)  
        Centering, in the simplest case, refers to subtracting the mean (central) value from the covariate values, which is equivalent to orthogonalising the covariate with respect to the constant column.   

        Subtracting a constant from a covariate changes the beta for the constant term, but not that for the covariate. In the simplest case, centering a covariate in a simple regression leaves the slope unchanged, but converts the intercept from being the modelled value when the covariate was zero, to being the modelled value at the mean of the covariate, which is often more easily interpretable. For example, the modelled value at the subjects' mean age is usually more meaningful than the (extrapolated) value at an age of zero.   

        If a covariate value of zero is interpretable and/or you wish to preserve the values of the covariate then choose 'No centering'. You should also choose not to center if you have already subtracted some suitable value from your covariate, such as a commonly used reference level or the mean from another (e.g. larger) sample. Note that 'User specified value' has no effect, but is present for compatibility with earlier SPM versions.   

        Other centering options should only be used in special cases. More complicated centering options can orthogonalise a covariate or a covariate-factor interaction with respect to a factor, in which case covariate values within a particular level of a factor have their mean over that level subtracted. As in the simple case, such orthogonalisation changes the betas for the factor used to orthogonalise, not those for the covariate/interaction being orthogonalised. This therefore allows an added covariate/interaction to explain some otherwise unexplained variance, but without altering the group difference from that without the covariate/interaction. This is usually *inappropriate* except in special cases. One such case is with two groups and covariate that only has meaningful values for one group (such as a disease severity score that has no meaning for a control group); centering the covariate by the group factor centers the values for the meaningful group and (appropriately) zeroes the values for the other group.   


* **Masking**   
The mask specifies the voxels within the image volume which are to be assessed. SPM supports three methods of masking (1) Threshold, (2) Implicit and (3) Explicit. The volume analysed is the intersection of all masks.   

    * **Threshold masking** (choose an option)  
    Images are thresholded at a given value and only voxels at which all images exceed the threshold are included.   

        * **None**   
        No threshold masking   

        * **Absolute**   
        Images are thresholded at a given value and only voxels at which all images exceed the threshold are included.    

        This option allows you to specify the absolute value of the threshold.   

            * **Threshold** (enter text)  
            Enter the absolute value of the threshold.   

        * **Relative**   
        Images are thresholded at a given value and only voxels at which all images exceed the threshold are included.   

        This option allows you to specify the value of the threshold as a proportion of the global value.   

            * **Threshold** (enter text)  
            Enter the threshold as a proportion of the global value.   

    * **Implicit Mask** (choose from the menu)  
    An "implicit mask" is a mask implied by a particular voxel value. Voxels with this mask value are excluded from the analysis.   

    For image data-types with a representation of NaN (see spm_type.m), NaN's is the implicit mask value, (and NaN's are always masked out).   

    For image data-types without a representation of NaN, zero is the mask value, and the user can choose whether zero voxels should be masked out or not.   

    By default, an implicit mask is used.   

    * **Explicit Mask** (select files)  
    Explicit masks are other images containing (implicit) masks that are to be applied to the current analysis.   

    All voxels with value NaN (for image data-types with a representation of NaN), or zero (for other data types) are excluded from the analysis.   

    Explicit mask images can have any orientation and voxel/image size. Nearest neighbour interpolation of a mask image is used if the voxel centers of the input images do not coincide with that of the mask image.   

* **Global calculation** (choose an option)  
This option is for PET or VBM data (not second level fMRI).   

There are three methods for estimating global effects (1) Omit (assuming no other options requiring the global value chosen) (2) User defined (enter your own vector of global values) (3) Mean: SPM standard mean voxel value (within per image fullmean/8 mask)    

    * **Omit**   
    Omit   

    * **User**   
    User defined  global effects (enter your own vector of global values).   

        * **Global values** (enter text)  
        Enter the vector of global values.   

    * **Mean**   
    SPM standard mean voxel value.   

    This defines the global mean via a two-step process. Firstly, the overall mean is computed. Voxels with values less than 1/8 of this value are then deemed extra-cranial and get masked out. The mean is then recomputed on the remaining voxels.   

* **Global normalisation**   
These options are for PET or VBM data (not second level fMRI).   

'Overall grand mean scaling' simply scales all the data by a common factor such that the mean of all the global values is the value specified.   

'Normalisation' refers to either proportionally scaling each image or adding a covariate to adjust for the global values.   

    * **Overall grand mean scaling** (choose an option)  
    Scaling of the overall grand mean simply scales all the data by a common factor such that the mean of all the global values is the value specified. For qualitative data, this puts the data into an intuitively accessible scale without altering the statistics.    

    When proportional scaling global normalisation is used each image is separately scaled such that it's global value is that specified (in which case the grand mean is also implicitly scaled to that value). So, to proportionally scale each image so that its global value is eg. 20, select <Yes> then type in 20 for the grand mean scaled value.   

    When using AnCova or no global normalisation, with data from different subjects or sessions, an intermediate situation may be appropriate, and you may be given the option to scale group, session or subject grand means separately.    

        * **No**   
        No overall grand mean scaling.   

        * **Yes**   
        Scaling of the overall grand mean simply scales all the data by a common factor such that the mean of all the global values is the value specified. For qualitative data, this puts the data into an intuitively accessible scale without altering the statistics.   

            * **Grand mean scaled value** (enter text)  
            The default value of 50, scales the global flow to a physiologically realistic value of 50ml/dl/min.   

    * **Normalisation** (choose from the menu)  
    This option is for PET or VBM data (not second level fMRI).   

    Global nuisance effects (such as average values for PET images, or total tissue volumes for VBM) can be accounted for either by dividing the intensities in each image by the image's global value (proportional scaling), or by including the global covariate as a nuisance effect in the general linear model (AnCova).   

    Much has been written on which to use, and when. Basically, since proportional scaling also scales the variance term, it is appropriate for situations where the global measurement predominantly reflects gain or sensitivity. Where variance is constant across the range of global values, linear modelling in an AnCova approach has more flexibility, since the model is not restricted to a simple proportional regression.    

    'Ancova by subject' or 'Ancova by effect' options are implemented using the ANCOVA options provided where each experimental factor (eg. subject or effect), is defined. These allow eg. different subjects to have different relationships between local and global measurements.    

    Since differences between subjects may be due to gain and sensitivity effects, AnCova by subject could be combined with "grand mean scaling by subject" (an option also provided where each experimental factor is originally defined) to obtain a combination of between subject proportional scaling and within subject AnCova.    
