# Expand Image Frames  
Return a list of image filenames with appended frame numbers.

* **NIfTI file(s)** (select files)  
Files to read. If the same multi-frame image is specified more than once, it will be expanded as often as it is listed.

* **Frames** (enter text)  
Frame number(s) requested.
Only frames that are actually present in the image file(s) will be listed.
Enter 'Inf' to list all frames.
Enter '[N Inf]' to list all frames starting from N.
