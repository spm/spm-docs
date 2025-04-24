# Get Bounding Box  
Determine the bounding box of an image.  
This is the [2 x 3] array of the minimum and maximum X, Y, and Z coordinates (in mm),   
BB = [min_X min_Y min_Z  
      max_X max_Y max_Z]  

* **Image** (select files)  
Image for which to determine bounding box.  

* **Bounding box definition** (choose an option)  
Bounding box for field of view (using only header information) or for the set of voxels over a specified threshold.  

    * **Field of view**   
    Bounding box is for entire field of view of image.  

    * **Supra-threshold voxels**   
    Bounding box is for voxels over specified threshold.  

        * **Threshold** (enter text)  
        Bounding box is for set of voxels above this value.  
