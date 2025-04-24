# Reorient Images  
Reorient images given a set of parameters.  
The reorientation parameters can be given either as a 4x4 matrix or as parameters as defined for spm_matrix.m. The new image orientation will be computed by PRE-multiplying the original orientation matrix with the supplied matrix.  

* **Images to reorient** (select files)  
Select images to reorient.  

* **Reorient by** (choose an option)  
Specify reorientation parameters - 12 parameters, a 4x4 transformation matrix or a saved mat file (containing the matrix). The resulting transformation will be left-multiplied to the voxel-to-world transformation of each image and the new transformation will be written to the image header.  

    * **Reorientation Matrix** (enter text)  
    Enter a valid 4x4 matrix for reorientation.  
    .  
    Example: This will L-R flip the images.  
    .  
       -1 0 0 0; 0 1 0 0; 0 0 1 0; 0 0 0 1  

    * **Reorientation Parameters** (enter text)  
    Enter 12 reorientation parameters.  
    P(1)  - x translation  
    P(2)  - y translation  
    P(3)  - z translation  
    P(4)  - x rotation about - {pitch} (radians)  
    P(5)  - y rotation about - {roll}  (radians)  
    P(6)  - z rotation about - {yaw}   (radians)  
    P(7)  - x scaling  
    P(8)  - y scaling  
    P(9)  - z scaling  
    P(10) - x affine  
    P(11) - y affine  
    P(12) - z affine  
    Parameters are entered as listed above and then processed by spm_matrix.  
    .  
    Example: This will L-R flip the images (extra spaces are inserted between each group for illustration purposes).  
    .  
       0 0 0   0 0 0   -1 1 1   0 0 0  
    .  

    * **Saved reorientation matrix** (select files)  
    Select mat file containing saved reorientation (e.g. after using reorient from Display or Check Reg and saving when prompted).  

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the reoriented image file(s). If this is left empty, the original files will be overwritten.  
