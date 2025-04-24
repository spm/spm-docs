# Time-frequency rescale  
Rescale (avg) spectrogram with nonlinear and/or difference operator.   
For 'Log' and 'Sqrt', these functions are applied to spectrogram.   
For 'LogR', 'Rel' and 'Diff' this function computes power in the baseline.   
p_b and outputs:   
(i) p-p_b for 'Diff'   
(ii) 100*(p-p_b)/p_b for 'Rel'   
(iii) log (p/p_b) for 'LogR'   

* **File Name** (select files)  
Select the M/EEG mat file.   

* **Rescale method** (choose an option)  
Select the rescale method.   

    * **Log Ratio**   
    Log Ratio.   

        * **Baseline**   
        Baseline parameters.   

            * **Baseline time window** (enter text)  
            Start and stop of the baseline time window [ms].   

            * **Pool baseline across trials** (choose from the menu)  
            Combine baseline across trials to avoid bias   
            See Ciuparu and Muresan Eur J Neurosci. 43(7):861-9, 2016   

            * **External baseline dataset** (select files)  
            Select the baseline M/EEG mat file. Leave empty to use the input dataset   

    * **Difference**   
    Difference.   

        * **Baseline**   
        Baseline parameters.   

            * **Baseline time window** (enter text)  
            Start and stop of the baseline time window [ms].   

            * **Pool baseline across trials** (choose from the menu)  
            Combine baseline across trials to avoid bias   
            See Ciuparu and Muresan Eur J Neurosci. 43(7):861-9, 2016   

            * **External baseline dataset** (select files)  
            Select the baseline M/EEG mat file. Leave empty to use the input dataset   

    * **Relative**   
    Relative.   

        * **Baseline**   
        Baseline parameters.   

            * **Baseline time window** (enter text)  
            Start and stop of the baseline time window [ms].   

            * **Pool baseline across trials** (choose from the menu)  
            Combine baseline across trials to avoid bias   
            See Ciuparu and Muresan Eur J Neurosci. 43(7):861-9, 2016   

            * **External baseline dataset** (select files)  
            Select the baseline M/EEG mat file. Leave empty to use the input dataset   

    * **Zscore**   
    Z score   

        * **Baseline**   
        Baseline parameters.   

            * **Baseline time window** (enter text)  
            Start and stop of the baseline time window [ms].   

            * **Pool baseline across trials** (choose from the menu)  
            Combine baseline across trials to avoid bias   
            See Ciuparu and Muresan Eur J Neurosci. 43(7):861-9, 2016   

            * **External baseline dataset** (select files)  
            Select the baseline M/EEG mat file. Leave empty to use the input dataset   

    * **Log**   
    Log.   

    * **Log+eps**   
    Log + epsilon (to avoid boosting very low values)   

    * **Sqrt**   
    Square Root.   

    * **None**   
    No rescaling - just copy. Useful for optimising batch pipelines.   

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the output dataset. Default prefix is 'r'.   
