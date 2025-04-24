# Inverse solution  
Compute inverse projectors  

* **BF.mat file** (select files)  
Select BF.mat file.  

* **Inverse method** (choose an option)  
  

    * **Champagne**   
      

        * **Number of EM iterations** (enter text)  
          

        * **Voxel covariance structure** (choose from the menu)  
          

        * **Noise covariance** (choose from the menu)  
          

    * **DeFleCT**   
    DeFleCT spatial filter design framework by Matti Stenroos and Olaf Hauk  
    http://imaging.mrc-cbu.cam.ac.uk/meg/AnalyzingData/DeFleCT_SpatialFiltering_Tools  
    Please cite:  
    Hauk O, Stenroos M.  
    A framework for the design of flexible cross-talk functions for spatial filtering of EEG/MEG data: DeFleCT.  
    Human Brain Mapping 2013  

        * **Filters** (create a list of items)  
          

            * **Filter**   
              

                * **Label** (enter text)  
                Label for the output source  

                * **Passband sources** (create a list of items)  
                  

                    * **VOI**   
                      

                    * **List vertex indices** (enter text)  
                    Specify sources of interest by listing vertex indices.  

                * **SVD passband** (enter text)  
                Number of components to summarise the passband in.,  
                Leave at zero for no SVD.  

                * **Force passband** (choose from the menu)  
                Forces the output for all passband components   

                * **Stopband sources** (create a list of items)  
                  

                    * **VOI**   
                      

                    * **List vertex indices** (enter text)  
                    Specify sources of interest by listing vertex indices.  

                * **SVD stopband** (enter text)  
                Number of components to summarise the stopband in.,  
                Leave at zero for no SVD.  

                * **Use covariance matrix** (choose from the menu)  
                Use covariance matrix for pre-whitening  

        * **SNR** (enter text)  
        The assumed ratio of variances of signal and noise,  
        used for setting the regularisation parameter.  

        * **Truncation parameter** (enter text)  
        The number of (smallest) singular values of the covariance matrix that are set to   
        zero before making the whitener. For example, if the data has been SSP-projected, it needs to be at least the number of   
        components projected away.  

    * **DICS**   
      

        * **Optimise for maximal power** (choose from the menu)  
        Optimise dipole orientation for maximal power  

    * **EBB**   
      

        * **Keep oriented leadfields** (choose from the menu)  
          

        * **Identity source covariance** (choose from the menu)  
        Assumes sources are indepedent and identically distributed, equivalent to a Bayesian minimum norm estimation. This option bypasses correlated source mode  

        * **Correlated & homologous sources** (choose from the menu)  
        Prior matrix is modified so to account for power in a location and its correlated partner, allowing for correlated sources normally suppressed by beamformers to be reconstructed. Correlated pairs can be definied in a matrix(see below) or automatically guessed by looking for the mirror along the saggital plane.  

        * **Only correlated sources** (choose from the menu)  
        Specifies whether the correlated prior should be considered on their own (yes) or in combination with the uncorrelated priors (no).  

        * **(On/Off) diagonal correlated priors** (choose from the menu)  
        Sets the correlated pairs to be on the diagonal (default)of the prior matrix or off the diagonal, which is closer to how MSP behaves when specifying correlated priors. or both simultaneously  

        * **Prior combination method** (choose from the menu)  
        How should we combine the correlated and uncorrelated priors (assuming we have both) +SUM: simple addition of the two priors to make one matrix. +REML: Two seperate priors, where the ReMLoptimisation will scale them automatically. (Default: sum)  

        * **Matrix of correlated pairs** (select files)  
        [OPTIONAL] BF.mat file containing a binary adjacency matrix of correlated pairs. If a matrix is not supplied, it will automatically look for a the homologous regions.TIP: If you want a source to not be correlated, pair it with itself (i.e. put a 1 on the diaconal element.  

        * **Hyperprior optimisation** (choose from the menu)  
        Model fitting via ReML. Select loose for (relatively) unrestricted sweep of the hyperpriors whereas strict mode forces the them to follow a rule of the noise being ~1/100th the magnitide of the sources  

        * **BF mat file containing noise matrix** (select files)  
        [OPTIONAL] BF.mat file containing empty-room noise matrix (pre-filtered)  

    * **eLORETA**   
    eLORETA implementation by Guido Nolte  
    Please cite  
    R.D. Pascual-Marqui: Discrete, 3D distributed, linear imaging methods of electric neuronal activity. Part 1: exact, zero  
    error localization. arXiv:0710.3341 [math-ph], 2007-October-17, http://arxiv.org/pdf/0710.3341  

        * **Regularisation parameter** (enter text)  
        Optional regularization parameter (default is .05 corresponding   
        to 5% of the average of the eigenvalues of some matrix to be inverted.)  

    * **LCMV**   
      

        * **Orient to maximum power** (choose from the menu)  
          

        * **Keep oriented leadfields** (choose from the menu)  
          

    * **LCMV use multiple covariance and PCA order**   
      

        * **PCA order** (enter text)  
          

        * **Do bilateral beamformer** (enter text)  
          

        * **Beamformer type** (choose from the menu)  
        Select Scalar or Vector  

    * **Minimum norm**   
    Minimum norm implementation by Matti Stenroos and Olaf Hauk  
    http://imaging.mrc-cbu.cam.ac.uk/meg/AnalyzingData/DeFleCT_SpatialFiltering_Tools  
    Please cite:  
    Hauk O, Stenroos M.  
    A framework for the design of flexible cross-talk functions for spatial filtering of EEG/MEG data: DeFleCT.  
    Human Brain Mapping 2013  

        * **SNR** (enter text)  
        The assumed ratio of variances of signal and noise,  
        used for setting the regularisation parameter.  

        * **Truncation parameter** (enter text)  
        The number of (smallest) singular values of the covariance matrix that are set to   
        zero before making the whitener. For example, if the data has been SSP-projected, it needs to be at least the number of   
        components projected away.  

    * **NUTMEG methods**   
    Methods ported from NUTMEG toolbox  
    See http://www.nitrc.org/plugins/mwiki/index.php/nutmeg:MainPage  

        * **Method** (choose from the menu)  
        Select one of NUTMEG methods  

        * **SNR** (enter text)  
        The assumed ratio of variances of signal and noise,  
        used for setting the regularisation parameter.  

        * **Regularisation parameter** (enter text)  
        Optional regularization parameter.  
