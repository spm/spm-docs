# DICOM Import  
DICOM Conversion.   
Most scanners produce data in DICOM format. This routine attempts to convert DICOM files into SPM compatible image volumes, which are written into the current directory by default. Note that not all flavours of DICOM can be handled, as DICOM is a very complicated format, and some scanner manufacturers use their own fields, which are not in the official documentation at http://medical.nema.org/   

* **DICOM files** (select files)  
Select the DICOM files to convert.   

* **Directory structure** (choose from the menu)  
Choose root directory of converted file tree. The options are:   
    * **StudyDate-StudyTime**: Automatically determine the project name and try to convert into the output directory, starting with a StudyDate-StudyTime subdirectory. This option is useful if automatic project recognition fails and one wants to convert data into a project directory.   
    * **PatientID**: Convert into the output directory, starting with a PatientID subdirectory.   
    * **ProtocolName**: Convert into the output directory, starting with a ProtocolName subdirectory.   
    * **No directory hierarchy**: Convert all files into the output directory, without sequence/series subdirectories   

* **Output directory** (select files)  
Files produced by this function will be written into this output directory. If no directory is given, images will be written to current working directory.   

* **Protocol name filter** (enter text)  
A regular expression to filter protocol names. DICOM images whose protocol names do not match this filter will not be converted.   

* **Conversion options**   

    * **Output image format** (choose from the menu)  
    Output files can be written as .img + .hdr, or the two can be combined into a single .nii file.   
    In any case, only 3D image files will be produced.   

    * **Export metadata** (choose from the menu)  
    Save DICOM fields in a sidecar JSON file.   

    * **Use ICEDims in filename** (choose from the menu)  
    If image sorting fails, one can try using the additional SIEMENS ICEDims information to create unique filenames. Use this only if there would be multiple volumes with exactly the same file names.   
