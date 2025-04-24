# Covariance features  
Define features for covariance computation  

* **BF.mat file** (select files)  
Select BF.mat file.  

* **What conditions to include?** (choose an option)  
.  

    * **All**   
    .  

    * **Conditions** (create a list of items)  
    Specify the labels of the conditions to be included in the inversion  

        * **Condition label** (enter text)  
        .  

* **Time windows of interest** (enter text)  
Time windows to average over (ms)  

* **Select modalities** (choose from the menu)  
Select modalities for the inversion (only relevant for multimodal datasets).  

* **Fuse modalities** (choose from the menu)  
Fuse sensors for different modalities together (requires prior rescaling).  

* **Zero cross terms** (choose from the menu)  
When fusing, set cross-terms between modalities to zero  

* **Covariance computation method** (choose an option)  
.  

    * **Robust covariance**   
    Robust covariance for continuous data  

    * **Band limited covariance**   
    .  

        * **Frequency bands of interest** (enter text)  
        Frequency windows within which to compute covariance over (sec)  

        * **Windowing** (choose from the menu)  
        Select a window for pre-multiplying the data  

    * **Cov estimation with variable width WOIs**   
    .  

    * **Cross-spectral density**   
    .  

        * **Frequency band of interest** (enter text)  
        Frequency window within which to compute CSD over (Hz)  

        * **Taper** (choose from the menu)  
        Save taper as well as power  

        * **Keep real** (choose from the menu)  
        Keep only the real part of the CSD  

        * **Pre-multiply with Hanning** (choose from the menu)  
        .  

    * **Identity matrix**   
    Returns identity matrix for cases when covariance is not necessary  

    * **Regularized multiple covariance**   
    .  

    * **Covariance with temporal decomposition**   
    .  

        * **Frequency bands of interest** (enter text)  
        Frequency windows within which to compute covariance over (sec)  

        * **Number of temporal modes** (enter text)  
        Number of temporal modes, set to 0 for automatic determination  

        * **Windowing** (choose from the menu)  
        Select a window for pre-multiplying the data  

    * **VB Factor Analysis**   
    This method uses code contributed by Sri Nagarajan  
    It should be used for computing noise covariance for Champagne  

        * **Factor dimensionality** (enter text)  
        .  

        * **Number of EM iterations** (enter text)  
        .  

* **Regularisation method** (choose an option)  
.  

    * **Eigenspectum cliff-face truncation**   
    .  

        * **Z-score threshold** (enter text)  
        Set the Z-score at which to detect cliff-face in eigenspectrum, or set to below 0 to find largest jump (default: -1)  

        * **Number of PCs to omit** (enter text)  
        Omit a number of components after the order has been determined (e.g. if matrix order is 200, selecting 10 would reduce it to 190).  

    * **Manual truncation**   
    .  

        * **Dimensionality** (enter text)  
        User-specified number of dimensions  

    * **User-specified regularisation**   
    .  

        * **Regularisation** (enter text)  
        Select the regularisation (in %)  

    * **Minka truncation**   
    .  

        * **Reduce data dimension** (choose from the menu)  
        Reduce the data to spatial modes based on Bayesian PCA  

    * **ROI regularisation**   
    .  

        * **ROI(s)** (create a list of items)  
        This makes it possible to define new VOIs when the original source space was mesh or grid.  
        Only the sources present in the original source space can be used at this stage  

            * **VOI**   
            .  

                * **MNI coordinates** (enter text)  
                Locations for the VOI in MNI coordinates  

                * **Radius** (enter text)  
                Radius (in mm) for the VOIs (leave 0 for single point)  

        * **Dimension choice** (choose an option)  
        .  

            * **Keep original**   
            Keep the original dimensionality of the ROI  

            * **User-specified** (enter text)  
            User-specified number of dimensions  

            * **Minka truncation**   
            Use Bayesian algorithm to determine dimensionality  

    * **Tichonov regularisation**   
    .  

        * **Dimensionality** (enter text)  
        True known data rank  

        * **Regularisation** (enter text)  
        Select the regularisation (in %)  

* **Bootstrap** (choose from the menu)  
.  

* **Visualise eigen-spectrum** (choose from the menu)  
Visualise covariance log eigen-spectrum to check for effective data dimensionality.  
