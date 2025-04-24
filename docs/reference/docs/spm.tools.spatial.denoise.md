# Total-variation denoising  
Total-variation (multi-channel) denoising of brain MRI.

* **Data** (create a list of items)  
Specify the number of different image channels. If you have scans of different contrasts for each of the subjects, then it is possible to combine the information from them in a way that might improve the denoising.

    * **Volumes** (select files)  
    Select scans from this channel for processing. If multiple channels are used (eg PDw & T2w), then the same order of subjects must be specified for each channel and they must be in register (same position, size, voxel dims etc..).

* **Denoising strength** (enter text)  
Vary the strength of denoising. This setting may need tweaking to obtain the best results. Note that the standard deviation over the field of view is used to adjust this setting.

* **Iterations** (choose from the menu)  
Number of denoising relaxation iterations.
