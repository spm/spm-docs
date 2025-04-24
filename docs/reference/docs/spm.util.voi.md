# Volume of Interest  
 VOI time-series extraction of adjusted data (& local eigenimage analysis).  

* **Select SPM.mat** (select files)  
Select SPM.mat file  

* **Adjust data** (enter text)  
Index of F-contrast used to adjust data. All effects covered by this contrast are retained, while all other effects in the design matrix are regressed out.  
For example, typing: eye(3) will retain the effects encoded in any of the first 3 columns of the design matrix and will regress out effects in columns 4 onwards.   
If all effects in the design matrix should be regressed out (e.g. for resting state analysis) then enter 'NaN' to adjust for everything.  
Entering '0' will select no adjustment (not typically recommended).  

* **Which session** (enter text)  
Enter the session number from which you want to extract data.  

* **Name of VOI** (enter text)  
Name of the VOI mat file that will be saved in the same directory than the SPM.mat file. A 'VOI_' prefix will be added. A binary NIfTI image of the VOI will also be saved.  

* **Region(s) of Interest** (create a list of items)  
Region(s) of Interest  

    * **Thresholded SPM**   
    Thresholded SPM  

        * **Select SPM.mat** (select files)  
        Select SPM.mat file. If empty, use the SPM.mat selected above.  

        * **Contrast** (enter text)  
        Index of contrast. If more than one index is entered, a conjunction analysis is performed.  

        * **Conjunction number** (enter text)  
        Conjunction number. Unused if a simple contrast is entered.  
        For Conjunction Null, enter 1.  
        For Global Null, enter the number of selected contrasts.  
        For Intermediate, enter the number of selected contrasts minus the number of effects under the Null.  

        * **Threshold type** (choose from the menu)  
          

        * **Threshold** (enter text)  
          

        * **Extent (voxels)** (enter text)  
          

        * **Masking** (create a list of items)  
          

            * **Mask definition**   
              

                * **Contrast** (enter text)  
                Indices of contrast(s).  

                * **Uncorrected mask p-value** (enter text)  
                  

                * **Nature of mask** (choose from the menu)  
                  

    * **Sphere**   
    Sphere.  

        * **Centre** (enter text)  
        Centre [x y z] {mm}.  

        * **Radius** (enter text)  
        Sphere radius (mm).  

        * **Movement of centre** (choose an option)  
        Movement of centre.  

            * **Fixed**   
            Fixed centre  

            * **Global maximum**   
            Global maximum  

                * **SPM index** (enter text)  
                SPM index  

                * **Mask expression** (enter text)  
                Example expressions (f):  

            * **Nearest local maximum**   
            Nearest local maximum  

                * **SPM index** (enter text)  
                SPM index  

                * **Mask expression** (enter text)  
                Example expressions (f):  

            * **Nearest suprathreshold voxel**   
            Nearest suprathreshold voxel.  

                * **SPM index** (enter text)  
                SPM index  

                * **Mask expression** (enter text)  
                Example expressions (f):  

    * **Box**   
    Box.  

        * **Centre** (enter text)  
        Centre [x y z] {mm}.  

        * **Dimensions** (enter text)  
        Box dimensions [x y z] {mm}.  

        * **Movement of centre** (choose an option)  
        Movement of centre.  

            * **Fixed**   
            Fixed centre  

            * **Global maximum**   
            Global maximum  

                * **SPM index** (enter text)  
                SPM index  

                * **Mask expression** (enter text)  
                Example expressions (f):  

            * **Nearest local maximum**   
            Nearest local maximum  

                * **SPM index** (enter text)  
                SPM index  

                * **Mask expression** (enter text)  
                Example expressions (f):  

            * **Nearest suprathreshold voxel**   
            Nearest suprathreshold voxel.  

                * **SPM index** (enter text)  
                SPM index  

                * **Mask expression** (enter text)  
                Example expressions (f):  

    * **Mask Image**   
    Mask Image.  

        * **Image file** (select files)  
        Select image.  

        * **Threshold** (enter text)  
        Threshold.  

    * **Label Image**   
    Label Image.  

        * **Image file** (select files)  
        Select image.  

        * **List of labels** (enter text)  
        List of labels.  

* **Expression** (enter text)  
Expression to be evaluated, using i1, i2, ...and logical operators (&, $|$, ~)  
