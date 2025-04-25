# Convolution modelling  
M/EEG convolution modelling   

* **Channel selection** (create a list of items)  
Channel selection.   

    * **All**   

    * **Select channels by type** (choose from the menu)  
    Select channels by type.   

    * **Custom channel** (enter text)  
    Enter a single channel name.   

    * **Regular expression** (enter text)  
    Enter a regular expression for matching multiple channel labels.   

    * **Channel file** (select files)  

* **Timing parameters**   
Timing parameters   

    * **Time window** (enter text)  
    Start and end of epoch [ms].   

    * **Units for design** (choose from the menu)  
    The onsets of events can be specified in either samples or seconds.   

    * **Microtime resolution** (enter text)  
    The microtime resolution, t, is the number of time-bins per input sample used when building regressors.    
    Can be modified to make the output up- or downsampled with respect to the input.   

* **Subject/Session**   
The design matrix consists of one or more separable, session-specific partitions.  These partitions are usually either one per subject, or one per scanning session for that subject.   

    * **M/EEG dataset** (select files)  
    Select the M/EEG mat file.   

    * **Conditions** (create a list of items)  
    You are allowed to combine both event- and epoch-related responses in the same model and/or regressor. Any number of condition (event or epoch) types can be specified.  Epoch and event-related responses are modeled in exactly the same way by specifying their onsets [in terms of onset times] and their durations.  Events are specified with a duration of 0.  If you enter a single number for the durations it will be assumed that all trials conform to this duration.For factorial designs, one can later associate these experimental conditions with the appropriate levels of experimental factors.    

        * **Condition**   
        An array of input functions is constructed, specifying occurrence events or epochs (or both). These are convolved with a basis set at a later stage to give regressors that enter into the design matrix. Interactions of evoked responses with some parameter (time or a specified variate) enter at this stage as additional columns in the design matrix with each trial multiplied by the [expansion of the] trial-specific parameter. The 0th order expansion is simply the main effect in the first column.   

            * **Name** (enter text)  
            Condition Name.   

            * **How to define events** (choose an option)  
            Choose the way to specify events for building regressors.   

                * **Specify manually**   

                    * **Onsets** (enter text)  
                    Specify a vector of onset times for this condition type.    

                    * **Durations** (enter text)  
                    Specify the event durations. Epoch and event-related responses are modeled in exactly the same way but by specifying their different durations.  Events are specified with a duration of 0.  If you enter a single number for the durations it will be assumed that all trials conform to this duration. If you have multiple different durations, then the number must match the number of onset times.   

                * **Take from dataset** (create a list of items)  

                    * **Event**   

            * **Time Modulation** (choose from the menu)  
            This option allows for the characterisation of linear or nonlinear time effects. For example, 1st order modulation would model the stick functions and a linear change of the stick function heights over time. Higher order modulation will introduce further columns that contain the stick functions scaled by time squared, time cubed etc.   
            .   
            Interactions or response modulations can enter at two levels.  Firstly the stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate or, secondly interactions among the trials themselves can be modeled using a Volterra series formulation that accommodates interactions over time (and therefore within and between trial types).   

            * **Parametric Modulations** (create a list of items)  
            The stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate. The events can be modulated by zero or more parameters.   

                * **Parameter**   
                Model interactions with user specified parameters. This allows nonlinear effects relating to some other measure to be modelled in the design matrix.   
                .   
                Interactions or response modulations can enter at two levels.  Firstly the stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate or, secondly interactions among the trials themselves can be modeled using a Volterra series formulation that accommodates interactions over time (and therefore within and between trial types).   

                    * **Name** (enter text)  
                    Enter a name for this parameter.   

                    * **Values** (enter text)  
                    Enter a vector of values, one for each occurrence of the event.   

                    * **Polynomial Expansion** (choose from the menu)  
                    For example, 1st order modulation would model the stick functions and a linear change of the stick function heights over different values of the parameter. Higher order modulation will introduce further columns that contain the stick functions scaled by parameter squared, cubed etc.   

            * **Orthogonalise modulations** (choose from the menu)  
            Orthogonalise regressors within trial types.   

    * **Multiple conditions** (select files)  
    Select the *.mat file containing details of your multiple experimental conditions.    
    .   
    If you have multiple conditions then entering the details a condition at a time is very inefficient. This option can be used to load all the required information in one go. You will first need to create a *.mat file containing the relevant information.    
    .   
    This *.mat file must include the following cell arrays (each 1 x n): names, onsets and durations. eg. names=cell(1,5), onsets=cell(1,5), durations=cell(1,5), then names{2}='SSent-DSpeak', onsets{2}=[3 5 19 222], durations{2}=[0 0 0 0], contain the required details of the second condition. These cell arrays may be made available by your stimulus delivery program, eg. COGENT. The duration vectors can contain a single entry if the durations are identical for all events. Optionally, a (1 x n) cell array named orth can also be included, with a 1 or 0 for each condition to indicate whether parameteric modulators should be orthogonalised.   
    .   
    Time and Parametric effects can also be included. For time modulation include a cell array (1 x n) called tmod. It should have a have a single number in each cell. Unused cells may contain either a 0 or be left empty. The number specifies the order of time modulation from 0 = No Time Modulation to 6 = 6th Order Time Modulation. eg. tmod{3} = 1, modulates the 3rd condition by a linear time effect.   
    .   
    For parametric modulation include a structure array, which is up to 1 x n in size, called pmod. n must be less than or equal to the number of cells in the names/onsets/durations cell arrays. The structure array pmod must have the fields: name, param and poly.  Each of these fields is in turn a cell array to allow the inclusion of one or more parametric effects per column of the design. The field name must be a cell array containing strings. The field param is a cell array containing a vector of parameters. Remember each parameter must be the same length as its corresponding onsets vector. The field poly is a cell array (for consistency) with each cell containing a single number specifying the order of the polynomial expansion from 1 to 6.   
    .   
    Note that each condition is assigned its corresponding entry in the structure array (condition 1 parametric modulators are in pmod(1), condition 2 parametric modulators are in pmod(2), etc. Within a condition multiple parametric modulators are accessed via each fields cell arrays. So for condition 1, parametric modulator 1 would be defined in  pmod(1).name{1}, pmod(1).param{1}, and pmod(1).poly{1}. A second parametric modulator for condition 1 would be defined as pmod(1).name{2}, pmod(1).param{2} and pmod(1).poly{2}. If there was also a parametric modulator for condition 2, then remember the first modulator for that condition is in cell array 1: pmod(2).name{1}, pmod(2).param{1}, and pmod(2).poly{1}. If some, but not all conditions are parametrically modulated, then the non-modulated indices in the pmod structure can be left blank. For example, if conditions 1 and 3 but not condition 2 are modulated, then specify pmod(1) and pmod(3). Similarly, if conditions 1 and 2 are modulated but there are 3 conditions overall, it is only necessary for pmod to be a 1 x 2 structure array.   
    .   
    EXAMPLE:   
    Make an empty pmod structure:    
      pmod = struct('name',{''},'param',{},'poly',{});   
    Specify one parametric regressor for the first condition:    
      pmod(1).name{1}  = 'regressor1';   
      pmod(1).param{1} = [1 2 4 5 6];   
      pmod(1).poly{1}  = 1;   
    Specify 2 parametric regressors for the second condition:    
      pmod(2).name{1}  = 'regressor2-1';   
      pmod(2).param{1} = [1 3 5 7];    
      pmod(2).poly{1}  = 1;   
      pmod(2).name{2}  = 'regressor2-2';   
      pmod(2).param{2} = [2 4 6 8 10];   
      pmod(2).poly{2}  = 1;   
    .   
    The parametric modulator should be mean corrected if appropriate. Unused structure entries should have all fields left empty.   

    * **Convolution regressors** (create a list of items)  
    Convolution regressors are continuous variables that are convolved with a basis set to estimate an impulse-response pattern.   

        * **Regressor**   
        Specification for convolution regressor   

            * **Name** (enter text)  
            Enter name of regressor eg. First movement parameter   

            * **Value** (enter text)  
            Enter the vector of regressor values   

    * **Multiple convolution regressors** (select files)  
    Select the *.mat/*.txt file containing details of your multiple regressors.    
    .   
    If you have multiple regressors eg. realignment parameters, then entering the details a regressor at a time is very inefficient. This option can be used to load all the required information in one go.    
    .   
    You will first need to create a *.mat file containing a matrix R or a *.txt file containing the regressors. Each column of R will contain a different regressor. When SPM creates the design matrix the regressors will be named R1, R2, R3, ..etc.   

    * **Regressors** (create a list of items)  
    Regressors are additional columns included in the design matrix, which may model effects that would not be convolved with the haemodynamic response.  One such example would be the estimated movement parameters, which may confound the data.   

        * **Regressor**   
        regressor   

            * **Name** (enter text)  
            Enter name of regressor eg. First movement parameter   

            * **Value** (enter text)  
            Enter the vector of regressor values   

    * **Multiple regressors** (select files)  
    Select the *.mat/*.txt file containing details of your multiple regressors.    
    .   
    If you have multiple regressors eg. realignment parameters, then entering the details a regressor at a time is very inefficient. This option can be used to load all the required information in one go.    
    .   
    You will first need to create a *.mat file containing a matrix R or a *.txt file containing the regressors. Each column of R will contain a different regressor. When SPM creates the design matrix the regressors will be named R1, R2, R3, ..etc.   

    * **Save regressor coefficients** (choose from the menu)  
    Choose 'yes' to save the coefficients for regressors as a separate dataset (of spectra for TF data). If you are only using regressors to model out nuisance variables   
    (e.g. motion) saving might not be necessary   

    * **High-pass filter** (enter text)  
    The default high-pass filter cutoff is 10 seconds.Slow signal drifts with a period longer than this will be removed. Use 'explore design' to ensure this cut-off is not removing too much experimental variance. High-pass filtering is implemented using a residual forming matrix (i.e. it is not a convolution) and is simply to a way to remove confounds without estimating their parameters explicitly.  The constant term is also incorporated into this filter matrix.   

* **Basis Functions** (choose an option)  
Choose the basis set   

    * **Fourier Set**   
    Fourier basis functions.   

        * **Order** (enter text)  
        Number of basis functions   

    * **Fourier Set (Hanning)**   
    Fourier basis functions with Hanning Window   

        * **Order** (enter text)  
        Number of basis functions   

    * **Finite Impulse Response**   
    Finite impulse response.   

        * **Order** (enter text)  
        Number of basis functions   

    * **Cosine Set**   
    Discrete cosine set.   

        * **Order** (enter text)  
        Number of basis functions   

* **Model Interactions (Volterra)** (choose from the menu)  
Generalized convolution of inputs (U) with basis set (bf).   
.   
For first order expansions the causes are simply convolved (e.g. stick functions) in U.u by the basis functions in bf to create a design matrix X.  For second order expansions new entries appear in ind, bf and name that correspond to the interaction among the original causes. The basis functions for these efects are two dimensional and are used to assemble the second order kernel. Second order effects are computed for only the first column of U.u.   
Interactions or response modulations can enter at two levels.  Firstly the stick function itself can be modulated by some parametric variate (this can be time or some trial-specific variate like reaction time) modeling the interaction between the trial and the variate or, secondly interactions among the trials themselves can be modeled using a Volterra series formulation that accommodates interactions over time (and therefore within and between trial types).   

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the output dataset. Default prefix is 'C'.   
