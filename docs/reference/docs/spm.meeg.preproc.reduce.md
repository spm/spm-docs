# Data reduction  
Perform data reduction.

* **File Name** (select files)  
Select the M/EEG mat file(s).

* **Time window** (enter text)  
Start and stop of the time window [ms].

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

* **Reduction method** (choose an option)  
Reduction method

    * **CVA**   

        * **Channel sets** (create a list of items)  

            * **Set**   

                * **Channels to reduce**   

                    * **Channel selection** (create a list of items)  
                    Channel selection.

                * **Reference channels**   

                    * **Channel selection** (create a list of items)  
                    Channel selection.

                * **Number of components** (enter text)  
                Number of components to retain

                * **Output channel label** (enter text)  
                Label for the output channel(s).
                Numbers are added for multiple channels.

                * **Frequency band of interest** (enter text)  
                Frequency window to optimize for

                * **Time shift window** (enter text)  
                Time shift window (ms). [0 0] - instantaneous

                * **Time shift resolution** (enter text)  
                Time shift resolution (ms)

    * **Imaginary part of CSD**   

        * **Channel sets** (create a list of items)  

            * **Set**   

                * **Channels to reduce**   

                    * **Channel selection** (create a list of items)  
                    Channel selection.

                * **Reference channels**   

                    * **Channel selection** (create a list of items)  
                    Channel selection.

                * **Output channel label** (enter text)  
                Label for the output channel(s).

                * **Frequency band of interest** (enter text)  
                Frequency window to optimize for

    * **PCA data reduction**   

        * **Number of components** (enter text)  
        Number of PCA components

        * **Variance threshold** (enter text)  
        Threshold (1 > u > 0) for normalized eigenvalues

    * **Adaptive PCA**   

        * **Number of components** (enter text)  
        Number of PCA components

    * **Whiten the data**   

        * **Noise dataset (optional)** (select files)  
        Optional noise dataset for covariance covariance computation
        e.g. empty room recording

        * **Covariance inversion method** (choose from the menu)  
        Method for inverting the covariance matrix

        * **Separate channel types** (choose from the menu)  
        If yes, each channel type is whitened separately

        * **Lambda** (enter text)  
        scalar value, or string (expressed as a percentage), specifying
        the regularization parameter for Lavrentiev or Tikhonov
        regularization, or the replacement value for winsorization

        * **Lambda as percentage** (choose from the menu)  
        If yes, lambda is interpreted as a percentage of the average eigenvalue

        * **Kappa** (enter text)  
        Scalar integer - reflects the ordinal singular value at which
        the singular value spectrum will be truncated.

        * **Tolerance** (enter text)  
        scalar, reflects the fraction of the largest singular value
        at which the singular value spectrum will be truncated.
        The default is 10*eps*max(size(X))

* **Keep original channels** (choose from the menu)  
Specify whether you want to keep the original unreduced channels

* **Keep other channels** (choose from the menu)  
Specify whether you want to keep channels that are not contributing to the new channels

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the output dataset. Default prefix is 'R'.
