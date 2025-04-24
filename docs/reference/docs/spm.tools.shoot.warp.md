# Run Shooting (create Templates)  
Run the geodesic shooting nonlinear image registration procedure . This involves iteratively matching all the selected images to a template generated from their own mean . A series of Template*.nii files are generated, which become increasingly crisp as the registration proceeds.   

* **Images** (create a list of items)  
Select the images to be warped together. Multiple sets of images can be simultaneously registered. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.   

    * **Images** (select files)  
    Select a set of imported images of the same type to be registered by minimising a measure of difference from the template.   
