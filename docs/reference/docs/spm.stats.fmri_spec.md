# fMRI model specification  
Statistical analysis of fMRI data using a mass-univariate approach based on General Linear Models (GLMs).   
It comprises the following steps (1) specification of the GLM design matrix, fMRI data files and filtering (2) estimation of GLM parameters using classical or Bayesian approaches and (3) interrogation of results using contrast vectors to produce Statistical Parametric Maps (SPMs) or Posterior Probability Maps (PPMs).   
The design matrix defines the experimental design and the nature of hypothesis testing to be implemented.  The design matrix has one row for each scan and one column for each effect or explanatory variable. (eg. regressor or stimulus function). You can build design matrices with separable session-specific partitions.  Each partition may be the same (in which case it is only necessary to specify it once) or different.    
Responses can be either event- or epoch related, the only distinction is the duration of the underlying input or stimulus function. Mathematically they are both modeled by convolving a series of delta (stick) or box functions (u), indicating the onset of an event or epoch with a set of basis functions.  These basis functions model the hemodynamic convolution, applied by the brain, to the inputs.  This convolution can be first-order or a generalized convolution modeled to second order (if you specify the Volterra option). The same inputs are used by the Hemodynamic model or Dynamic Causal Models which model the convolution explicitly in terms of hidden state variables.    
Basis functions can be used to plot estimated responses to single events once the parameters (i.e. basis function coefficients) have been estimated.  The importance of basis functions is that they provide a graceful transition between simple fixed response models (like the box-car) and finite impulse response (FIR) models, where there is one basis function for each scan following an event or epoch onset.  The nice thing about basis functions, compared to FIR models, is that data sampling and stimulus presentation does not have to be synchronized thereby allowing a uniform and unbiased sampling of peri-stimulus time.   
Event-related designs may be stochastic or deterministic.  Stochastic designs involve one of a number of trial-types occurring with a specified probability at successive intervals in time.  These probabilities can be fixed (stationary designs) or time-dependent (modulated or non-stationary designs).  The most efficient designs obtain when the probabilities of every trial type are equal. A critical issue in stochastic designs is whether to include null events If you wish to estimate the evoked response to a specific event type (as opposed to differential responses) then a null event must be included (even if it is not modeled explicitly).   
In SPM, analysis of data from multiple subjects typically proceeds in two stages using models at two 'levels'. The 'first level' models are used to implement a within-subject analysis. Typically there will be as many first level models as there are subjects. Analysis proceeds as described using the 'Specify first level' and 'Estimate' options. The results of these analyses can then be presented as 'case studies'. More often, however, one wishes to make inferences about the population from which the subjects were drawn. This is an example of a 'Random-Effects (RFX) analysis' (or, more properly, a mixed-effects analysis). In SPM, RFX analysis is implemented using the 'summary-statistic' approach where contrast images from each subject are used as summary measures of subject responses. These are then entered as data into a 'second level' model.    

* **Directory** (select files)  
Select a directory where the SPM.mat file containing the specified design matrix will be written.   

* **Timing parameters**   
Specify various timing parameters needed to construct the design matrix.   
This includes the units of the design specification and the interscan interval.   
Also, with longs TRs you may want to shift the regressors so that they are aligned to a particular slice. This is effected by changing the microtime resolution and onset.    

    * **Units for design** (choose from the menu)  
    The onsets of events or blocks can be specified in either scans or seconds.   

    * **Interscan interval** (enter text)  
    Interscan interval, TR, (specified in seconds).   
    This is the time between acquiring a plane of one volume and the same plane in the next volume.  It is assumed to be constant throughout.   

    * **Microtime resolution** (enter text)  
    The microtime resolution, t, is the number of time-bins per scan used when building regressors.   
    If you have performed slice-timing correction, change this parameter to match the number of slices specified there; otherwise, you would typically not need to change this.   

    * **Microtime onset** (enter text)  
    The microtime onset, t0, is the reference time-bin at which the regressors are resampled to coincide with data acquisition.   
    If you have performed slice-timing correction, you must change this parameter to match the reference slice specified there.   
    Otherwise, you might still want to change this if you have non-interleaved acquisition and you wish to sample the regressors so that they are appropriate for a slice in a particular part of the brain.   
    For example, if t0 = 1, then the regressors will be appropriate for the first slice; if t0=t, then the regressors will be appropriate for the last slice.   
    Setting t0 = t/2 is a good compromise if you are interested in slices at the beginning and end of the acquisition, or if you have interleaved data, or if you have 3D EPI data.   

* **Data & Design** (create a list of items)  
The design matrix defines the experimental design and the nature of hypothesis testing to be implemented.  The design matrix has one row for each scan and one column for each effect or explanatory variable. (e.g. regressor or stimulus function).     
This allows you to build design matrices with separable session-specific partitions.  Each partition may be the same (in which case it is only necessary to specify it once) or different.  Responses can be either event- or epoch related, where the latter model involves prolonged and possibly time-varying responses to state-related changes in experimental conditions.  Event-related response are modelled in terms of responses to instantaneous events.  Mathematically they are both modelled by convolving a series of delta (stick) or box-car functions, encoding the input or stimulus function. with a set of hemodynamic basis functions.   

    * **Subject/Session**   
    The design matrix for fMRI data consists of one or more separable, session-specific partitions.   
    These partitions are usually either one per subject, or one per fMRI scanning session for that subject.   

        * **Scans** (select files)  
        Select the fMRI scans for this session. They must all have the same image dimensions, orientation, voxel size etc.   

        * **Conditions** (create a list of items)  
        You are allowed to combine both event- and epoch-related responses in the same model and/or regressor.   
        Any number of condition (event or epoch) types can be specified. Epoch and event-related responses are modeled in exactly the same way by specifying their onsets [in terms of onset times] and their durations.  Events are specified with a duration of 0.  If you enter a single number for the durations it will be assumed that all trials conform to this duration. For factorial designs, one can later associate these experimental conditions with the appropriate levels of experimental factors.   

            * **Condition**   
            An array of input functions is constructed, specifying occurrence events or epochs (or both).   
            These are convolved with a basis set at a later stage to give regressors that enter into the design matrix. Interactions of evoked responses with some parameter (time or a specified variate) enter at this stage as additional columns in the design matrix with each trial multiplied by the [expansion of the] trial-specific parameter. The 0th order expansion is simply the main effect in the first column.   

                * **Name** (enter text)  
                Condition Name.   

                * **Onsets** (enter text)  
                Specify a vector of onset times for this condition type.   

                * **Durations** (enter text)  
                Specify the event durations.   
                Epoch and event-related responses are modeled in exactly the same way but by specifying their different durations.  Events are specified with a duration of 0.  If you enter a single number for the durations it will be assumed that all trials conform to this duration. If you have multiple different durations, then the number must match the number of onset times.   

                * **Time Modulation** (choose from the menu)  
                This option allows for the characterisation of linear or nonlinear time effects. For example, 1st order modulation would model the stick functions and a linear change of the stick function heights over time. Higher order modulation will introduce further columns that contain the stick functions scaled by time squared, time cubed etc.   
                Interactions or response modulations can enter at two levels.  Firstly the stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate or, secondly interactions among the trials themselves can be modeled using a Volterra series formulation that accommodates interactions over time (and therefore within and between trial types).   

                * **Parametric Modulations** (create a list of items)  
                The stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate. The events can be modulated by zero or more parameters.   

                    * **Parameter**   
                    Model interactions with user specified parameters. This allows nonlinear effects relating to some other measure to be modelled in the design matrix.   
                    Interactions or response modulations can enter at two levels.  Firstly the stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate or, secondly interactions among the trials themselves can be modeled using a Volterra series formulation that accommodates interactions over time (and therefore within and between trial types).   

                * **Orthogonalise modulations** (choose from the menu)  
                Orthogonalise regressors within trial types.   

        * **Multiple conditions** (select files)  
        Select the *.mat file containing details of your multiple experimental conditions.   
        If you have multiple conditions then entering the details a condition at a time is very inefficient. This option can be used to load all the required information in one go. You will first need to create a *.mat file containing the relevant information.    
        This *.mat file must include the following cell arrays (each 1 x n): names, onsets and durations. eg. names=cell(1,5), onsets=cell(1,5), durations=cell(1,5), then names{2}='SSent-DSpeak', onsets{2}=[3 5 19 222], durations{2}=[0 0 0 0], contain the required details of the second condition. These cell arrays may be made available by your stimulus delivery program, eg. COGENT. The duration vectors can contain a single entry if the durations are identical for all events. Optionally, a (1 x n) cell array named orth can also be included, with a 1 or 0 for each condition to indicate whether parameteric modulators should be orthogonalised.   
        Time and Parametric effects can also be included. For time modulation include a cell array (1 x n) called tmod. It should have a have a single number in each cell. Unused cells may contain either a 0 or be left empty. The number specifies the order of time modulation from 0 = No Time Modulation to 6 = 6th Order Time Modulation. eg. tmod{3} = 1, modulates the 3rd condition by a linear time effect.   
        For parametric modulation include a structure array, which is up to 1 x n in size, called pmod. n must be less than or equal to the number of cells in the names/onsets/durations cell arrays. The structure array pmod must have the fields: name, param and poly.  Each of these fields is in turn a cell array to allow the inclusion of one or more parametric effects per column of the design. The field name must be a cell array containing strings. The field param is a cell array containing a vector of parameters. Remember each parameter must be the same length as its corresponding onsets vector. The field poly is a cell array (for consistency) with each cell containing a single number specifying the order of the polynomial expansion from 1 to 6.   
        Note that each condition is assigned its corresponding entry in the structure array (condition 1 parametric modulators are in pmod(1), condition 2 parametric modulators are in pmod(2), etc. Within a condition multiple parametric modulators are accessed via each fields cell arrays. So for condition 1, parametric modulator 1 would be defined in  pmod(1).name{1}, pmod(1).param{1}, and pmod(1).poly{1}. A second parametric modulator for condition 1 would be defined as pmod(1).name{2}, pmod(1).param{2} and pmod(1).poly{2}. If there was also a parametric modulator for condition 2, then remember the first modulator for that condition is in cell array 1: pmod(2).name{1}, pmod(2).param{1}, and pmod(2).poly{1}. If some, but not all conditions are parametrically modulated, then the non-modulated indices in the pmod structure can be left blank. For example, if conditions 1 and 3 but not condition 2 are modulated, then specify pmod(1) and pmod(3). Similarly, if conditions 1 and 2 are modulated but there are 3 conditions overall, it is only necessary for pmod to be a 1 x 2 structure array.   
        EXAMPLE:   
        Make an empty pmod structure:    
        ``  pmod = struct('name',{''},'param',{},'poly',{});   
        Specify one parametric regressor for the first condition:    
          pmod(1).name{1}  = 'regressor1';   
          pmod(1).param{1} = [1 2 4 5 6];   
          pmod(1).poly{1}  = 1;``   
        Specify 2 parametric regressors for the second condition:    
        ``  pmod(2).name{1}  = 'regressor2-1';   
          pmod(2).param{1} = [1 3 5 7];    
          pmod(2).poly{1}  = 1;   
          pmod(2).name{2}  = 'regressor2-2';   
          pmod(2).param{2} = [2 4 6 8 10];   
          pmod(2).poly{2}  = 1;``   
        The parametric modulator should be mean corrected if appropriate. Unused structure entries should have all fields left empty.   

        * **Regressors** (create a list of items)  
        Regressors are additional columns included in the design matrix, which may model effects that would not be convolved with the haemodynamic response.   
        One such example would be the estimated movement parameters, which may confound the data.   

            * **Regressor**   
            regressor   

                * **Name** (enter text)  
                Enter name of regressor eg. First movement parameter.   

                * **Value** (enter text)  
                Enter the vector of regressor values.   

        * **Multiple regressors** (select files)  
        Select the *.mat/*.txt file(s) containing details of your multiple regressors.   
        If you have multiple regressors eg. realignment parameters, then entering the details a regressor at a time is very inefficient. This option can be used to load all the required information in one go.    
        You will first need to create a *.mat file containing a matrix R or a *.txt file containing the regressors. Each column of R will contain a different regressor. Unless the regressor names are given in a cell array called 'names' in the MAT-file containing variable R, the regressors will be named R1, R2, R3, ..etc.   
        You can also select a PPI.mat file and SPM will automatically create regressors from fields PPI.ppi, PPI.Y and PPI.P.   

        * **High-pass filter** (enter text)  
        The default high-pass filter cutoff is 128 seconds. Slow signal drifts with a period longer than this will be removed.   
        Use 'explore design' to ensure this cut-off is not removing too much experimental variance. High-pass filtering is implemented using a residual forming matrix (i.e. it is not a convolution) and is simply to a way to remove confounds without estimating their parameters explicitly.   

* **Factorial design** (create a list of items)  
If you have a factorial design then SPM can automatically generate the contrasts necessary to test for the main effects and interactions.    
This includes the F-contrasts necessary to test for these effects at the within-subject level (first level) and the simple contrasts necessary to generate the contrast images for a between-subject (second-level) analysis.   
To use this option, create as many factors as you need and provide a name and number of levels for each.  SPM assumes that the condition numbers of the first factor change slowest, the second factor next slowest etc. It is best to write down the contingency table for your design to ensure this condition is met. This table relates the levels of each factor to the conditions.    
For example, if you have 2-by-3 design  your contingency table has two rows and three columns where the the first factor spans the rows, and the second factor the columns. The numbers of the conditions are 1,2,3 for the first row and 4,5,6 for the second.    

    * **Factor**   
    Add a new factor to your experimental design.   

        * **Name** (enter text)  
        Name of this factor.   

        * **Levels** (enter text)  
        Number of levels for this factor.   

* **Basis Functions** (choose an option)  
The most common choice of basis function is the Canonical HRF with or without time and dispersion derivatives.   

    * **Canonical HRF**   
    Canonical Hemodynamic Response Function.   
    This is the default option. Contrasts of these effects have a physical interpretation and represent a parsimonious way of characterising event-related responses. This option is also useful if you wish to look separately at activations and deactivations (this is implemented using a t-contrast with a +1 or -1 entry over the canonical regressor).   

        * **Model derivatives** (choose from the menu)  
        Model HRF Derivatives. The canonical HRF combined with time and dispersion derivatives comprise an 'informed' basis set, as the shape of the canonical response conforms to the hemodynamic response that is commonly observed.   
        The incorporation of the derivate terms allow for variations in subject-to-subject and voxel-to-voxel responses. The time derivative allows the peak response to vary by plus or minus a second and the dispersion derivative allows the width of the response to vary. The informed basis set requires an SPM{F} for inference. T-contrasts over just the canonical are perfectly valid but assume constant delay/dispersion. The informed basis set compares favourably with eg. FIR bases on many data sets.    

    * **Fourier Set**   
    Fourier basis functions. This option requires an SPM{F} for inference.   

        * **Window length** (enter text)  
        Post-stimulus window length (in seconds).   

        * **Order** (enter text)  
        Number of basis functions.   

    * **Fourier Set (Hanning)**   
    Fourier basis functions with Hanning Window - requires SPM{F} for inference.   

        * **Window length** (enter text)  
        Post-stimulus window length (in seconds).   

        * **Order** (enter text)  
        Number of basis functions.   

    * **Gamma Functions**   
    Gamma basis functions - requires SPM{F} for inference.   

        * **Window length** (enter text)  
        Post-stimulus window length (in seconds).   

        * **Order** (enter text)  
        Number of basis functions   

    * **Finite Impulse Response**   
    Finite impulse response - requires SPM{F} for inference.   

        * **Window length** (enter text)  
        Post-stimulus window length (in seconds)   

        * **Order** (enter text)  
        Number of basis functions   

    * **None**   
    No convolution.   

* **Model Interactions (Volterra)** (choose from the menu)  
Generalized convolution of inputs (U) with basis set (bf).   
For first order expansions the causes are simply convolved (e.g. stick functions) in U.u by the basis functions in bf to create a design matrix X.  For second order expansions new entries appear in ind, bf and name that correspond to the interaction among the original causes. The basis functions for these efects are two dimensional and are used to assemble the second order kernel. Second order effects are computed for only the first column of U.u.   
Interactions or response modulations can enter at two levels.  Firstly the stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate or, secondly interactions among the trials themselves can be modeled using a Volterra series formulation that accommodates interactions over time (and therefore within and between trial types).   

* **Global normalisation** (choose from the menu)  
Global intensity normalisation   

* **Masking threshold** (enter text)  
Masking threshold, defined as proportion of globals.   

* **Explicit mask** (select files)  
Specify an image for explicitly masking the analysis.    
If masking is not required, you can leave this field empty.   
A sensible option here is to use a segmention of structural images to specify a within-brain mask. If you select that image as an explicit mask then only those voxels in the brain will be analysed. This both speeds the estimation and restricts SPMs/PPMs to within-brain voxels.   

* **Serial correlations** (choose from the menu)  
Serial correlations in fMRI time series due to aliased biorhythms and unmodelled neuronal activity can be accounted for using an autoregressive AR(1) model during Classical (ReML) parameter estimation.     
This estimate assumes the same correlation structure for each voxel, within each session.  ReML estimates are then used to correct for non-sphericity during inference by adjusting the statistics and degrees of freedom appropriately.  The discrepancy between estimated and actual intrinsic (i.e. prior to filtering) correlations are greatest at low frequencies.  Therefore specification of the high-pass filter is particularly important.    
Serial correlation can be ignored if you choose the 'none' option. Note that the above options only apply if you later specify that your model will be estimated using the Classical (ReML) approach. If you choose Bayesian estimation these options will be ignored. For Bayesian estimation, the choice of noisemodel (AR model order) is made under the estimation options.    
