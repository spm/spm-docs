# Run Shoot (existing Templates)  
Run the Shoot nonlinear image registration procedure  to match individual images to pre-existing template data. Start out with smooth templates, and select crisp templates for the later iterations.

* **Images** (create a list of items)  
Select the images to be warped together. Multiple sets of images can be simultaneously registered. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.

    * **Images** (select files)  
    Select a set of imported images of the same type to be registered by minimising a measure of difference from the template.

* **Templates** (select files)  
Select templates. Smoother templates should be used for the early iterations. Note that the template should be a 4D file, with the 4th dimension equal to the number of sets of images.
