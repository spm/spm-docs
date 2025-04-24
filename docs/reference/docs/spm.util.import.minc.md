# MINC Import  
MINC Conversion.
MINC is the image data format used for exchanging data within the ICBM community, and the format used by the MNI software tools. It is based on NetCDF. MINC is no longer supported for reading images into SPM, so MINC files need to be converted to NIFTI format in order to use them. See http://www.bic.mni.mcgill.ca/software/ for more information.

* **MINC files** (select files)  
Select the MINC files to convert.

* **Options**   
Conversion options

    * **Data Type** (choose from the menu)  
    Data-type of output images. Note that the number of bits used determines the accuracy, and the amount of disk space needed.

    * **Output image format** (choose from the menu)  
    Output files can be written as .img + .hdr, or the two can be combined into a .nii file.
