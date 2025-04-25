# Results Report  
Statistical inference.   

* **Select SPM.mat** (select files)  
Select the SPM.mat file that contains the design specification.   

* **Contrasts** (create a list of items)  

    * **Contrast query**   

        * **Results Title** (enter text)  
        Heading on results page - determined automatically if left empty   

        * **Contrast(s)** (enter text)  
        Index of contrast(s). If more than one number is entered, analyse a conjunction hypothesis.   
        .   
        If only one number is entered, and this number is "Inf", then results are printed for all contrasts found in the SPM.mat file.   

        * **Threshold type** (choose from the menu)  

        * **Threshold** (enter text)  

        * **Extent (voxels)** (enter text)  

        * **Conjunction number** (enter text)  
        Conjunction number. Unused if a simple contrast is entered.   
        For Conjunction Null, enter 1.   
        For Global Null, enter the number of selected contrasts.   
        For Intermediate, enter the number of selected contrasts minus the number of effects under the Null.   

        * **Masking** (choose an option)  

            * **None**   
            No masking.   

            * **Contrast**   
            Masking using contrast.   

                * **Contrast(s)** (enter text)  
                Index of contrast(s) for masking.   

                * **Mask threshold** (enter text)  

                * **Nature of mask** (choose from the menu)  

            * **Image**   
            Masking using image(s).   

                * **Mask image(s)** (select files)  

                * **Nature of mask** (choose from the menu)  

* **Data type** (choose from the menu)  
Data type. This option is only meaningful for M/EEG data. Keep the default 'Volumetric' for any other kind of data.   

* **Export results** (create a list of items)  
Select the export format you want. PostScript (PS) is the only format that allows to append figures to the same file.   

    * **Thresholded SPM**   
    Save filtered SPM{.} as an image.   

        * **Basename** (enter text)  
        Enter basename of output files 'spm?_????_<basename>.ext'.   

    * **All clusters (binary)**   
    Save filtered SPM{.} as a binary image.   

        * **Basename** (enter text)  
        Enter basename of output files 'spm?_????_<basename>.ext'.   

    * **All clusters (n-ary)**   
    Save filtered SPM{.} as an n-ary image.   

        * **Basename** (enter text)  
        Enter basename of output files 'spm?_????_<basename>.ext'.   

    * **PostScript (PS)**   
    PostScript (PS)   

    * **Encapsulated PostScript (EPS)**   
    Encapsulated PostScript (EPS)   

    * **Portable Document Format (PDF)**   
    Portable Document Format (PDF)   

    * **JPEG image**   
    JPEG image   

    * **PNG image**   
    PNG image   

    * **TIFF image**   
    TIFF image   

    * **MATLAB figure**   
    MATLAB figure   

    * **CSV file**   
    CSV file   

    * **Montage**   
    Display montage.   

        * **Background image** (select files)  
        Background image.   

        * **Image orientation** (choose from the menu)  
        Image orientation.   

        * **Slices** (enter text)  
        Slices to display (mm).   

    * **NIDM (Neuroimaging Data Model)**   
    NIDM (Neuroimaging Data Model)   

        * **Modality** (choose from the menu)  
        Modality.   

        * **Reference space** (choose from the menu)  
        Reference space. For an experiment completed only within SPM, choose one of the first four options.   

        * **Groups** (create a list of items)  
        Number of groups. For a single subject analysis, specify one group.   

            * **Group**   
            Number of subjects and labels per group. For a single subject analysis, enter "1" and "single subject".   

                * **Number of subjects** (enter text)  
                Number of subjects.   

                * **Label** (enter text)  
                Group label.   
