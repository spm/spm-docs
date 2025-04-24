# 3D to 4D File Conversion  
Concatenate a number of 3D volumes into a single 4D file.  

* **3D Volumes** (select files)  
Select the volumes to concatenate  

* **Output Filename** (enter text)  
Specify the name of the output 4D volume file.  
Unless explicit, the output folder is the one containing the first image.  
A '.nii' extension will be added if not specified.  

* **Data Type** (choose from the menu)  
Data-type of output image. SAME indicates the same datatype as the original images.  

* **Interscan interval** (enter text)  
Interscan interval, TR, (specified in seconds).  
This is the time between acquiring a plane of one volume and the same plane in the next volume. It is assumed to be constant throughout.  
