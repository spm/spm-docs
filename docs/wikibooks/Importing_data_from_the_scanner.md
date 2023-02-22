# Copying your files from the scanner

## FTP

You will probably need an FTP program like SmartFTP (for Windows) or
AxyFTP (for Linux) to connect to your scanner and download all the
images that have been acquired.

## Building your own DICOM archive

Instead of copying your files via FTP or CD/DVD, you may set up a DICOM
server or connect to a PACS/RIS using free DICOM tools.

There are some free DICOM toolkits around from which you can build your
own DICOM archive. An ancient, but very robust DICOM server can be found
at <ftp://ftp.erl.wustl.edu/pub/dicom/software/ctn/>. The software is
known to compile and run under both 32bit and 64bit Linux. Once you have
set it up, you can configure your scanner console to send the images to
this server using the scanner GUI.

# Reconstruction of functional images

See with your physics team.

# DICOM conversion for structurals

## GUI

Go to Toolbox \> DICOM

## SCRIPT

`%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%`  
`% DICOM Conversion of structurals`  
`   `  
`disp('Start of DICOM conversion for structurals');`  
  
`dir_t1=sprintf('%s/mri/t1',dir_subject);`  
`dir_t1_analyze=sprintf('%s/mri/t1_analyze',dir_subject);`  
`disp(sprintf('     Source directory: %s',dir_t1));`  
`disp(sprintf('     Target directory: %s',dir_t1_analyze));`  
  
`% See if the structurals exist`  
` `  
`cd(dir_t1);`  
`dr=dir('*');`  
`if isempty(dr) == false`  
`  cd(dir_t1_analyze);`  
`  P = spm_get('Files',dir_t1,'*');`  
`  hdr = spm_dicom_headers(P);`  
`  spm_dicom_convert(hdr)`  
`else`  
`  disp('No structurals to convert');`  
`end`
