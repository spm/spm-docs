# fMRI data preprocessing

## Slice timing correction

Slice timing correction aims to account for the differences in timing of data acquisition from different slices. 

??? info "Why use slice timing correction?"
    fMRI data is often collected in slices, with one or several slices acquired at a time. Slices can be acquired top-to-bottom (descending) or bottom-to-top (ascending) and be either contiguous or interleaved:

    ![](../../assets/figures/slice_timing_figure.png){ width="500" }
    
    This means that depending on the TR, there is a delay between the acquisition of the first and the last slice/group of slices. To fix this issue, information about the time of acquitision of each slice can be used to interpolate the signal of each slice in a volume to the same timepoint (referred to as the *reference slice*). It is thus important to know your data's TR and slice order acquisition or slice timings. 
    
    Slice timing has been shown to reliably increase sensitivity and statistical power without adverse effects, particularly for studies with longer TRs (>2s; [Sladky et al., 2011](https://doi.org/10.1016/j.neuroimage.2011.06.078)). However, it is important to remember that slice timing is an interpolation method, which means it modifies the original data. As a general rule in imaging analysis, interpolation should be avoided unless necessary. In the case of slice timing, there is a risk of artifacts from one volume being propagated to other volumes in the timeseries,  which can be particularly problematic for data with large amounts of motion. 
    
    An alternative to slice timing for task fMRI is the use of a *temporal derivative*, i.e. additional regressors in the first-level model which can shift the model in time to get the best fit. This avoids modyfying the original data and has an additional benefit of accounting for variations in the HRF. Temporal derivative can be particularly useful for studies with shorter TRs (≤2s), where slice timining issues are not as problematic. 

    For a thorough overview of issues related to slice timing in fMRI, see the SPM book:

    [Penny, W., Friston, K., Ashburner, J., Kiebel, S., & Nichols, T. (2006). *Statistical parametric mapping: The analysis of functional brain images* (1st ed.).](http://www.elsevierdirect.com/product.jsp?isbn=9780123725608&srccode=89660)

    And other readings:

    [Jenkinson, M. & Chappell, M. (2018). *Introduction to neuroimaging analysis*. Oxford University Press.](http://www.neuroimagingprimers.org/examples/introduction-primer-example-boxes/)

    [Poldrack, R. A., Mumford, J. A., & Nichols, T. E. (2011). *Handbook of functional MRI data analysis*. Cambridge University Press.](https://www.cambridge.org/core/books/handbook-of-functional-mri-data-analysis/8EDF966C65811FCCC306F7C916228529)

??? info "When to use slice timing correction?"
    * Slice timing can be done on any fMRI data but it is particularly beneficial for studies with **longer TRs** (i.e. >2s). For studies with shorter TRs (≤2s), temporal derivative may be a better option.

    * Slice timing can be performed on **multiband acquisition data**, however, with multiband data and a short TR, slice timing correction can usually be skipped without much impact. If you decide to run slice timing correction on multiband data, it is necessary to use slice timings instead of slice order, since a slice order cannot represent multiple slices acquired at the same time in a vector. If you don't know your slice timings, you can artificially create a slice timing manually, by generating artificial values from the slice order with equal temporal spacing and then scale the numbers on the TR, so that the last temporal slices timings = `TR - TR/(nslices/multiband_channels)`.

    * If you plan to use **Dynamic Causal Modeling (DCM)**, slice timing correction is necessary.

1. From the SPM menu panel, select `Slice timing`. A pop-up window will open:

![](../../assets/figures/slice_timing_batch.png)

2. Select `Data` :material-arrow-right-bold: `Session`.
3. In the pop-up window, use the left-hand panel to navigate to `sub-01/func/`. 
3. Identify the realigned timeseries - this will be the file with an `r` prefix, i.e. `rsub-01_task-auditory_bold.nii`. Use the box underneath the `Filter` button to show a 4D file by typing in `NaN` and pressing ++return++. You can do this in combination with filtering for files starting with `r` by typing in `^r.*` in the `Filter` box and pressing ++return++. 

!!! tip "Top tip"
    The `Filter` box allows you to filter files based on a specific combination of characters. The syntax used in filtering file is based on regular expressions. Below are some useful expressions for selecting files:

    * `.` - a period indicates any character. Filtering for `...` would show all files containing at least 3 characters. To filter for files containing a period in their name, use `\.`.

    * `*` - an asterisk indicates 0 or more instances of the preceding character. SPM's default filter is `.*`, meaning filter for any number of any characters, i.e. show all files present in a directory.

    * `^` - a caret indicates a string that begins with the indicated characters. Filtering for `^r` would show all files starting with `r`. 

    * `$` - a dollar sign indicates that the preceding characters end the file name. Filering for `\.nii$` will show all files with the extension `.nii`.

    These expressions can be combined to create more powerful filters. For example, using `^r.*\.nii$` will filter for all files starting with `r`, having any number of characters and ending in `.nii`.   

4. Select `rsub-01_task-auditory_bold.nii` from the right-hand panel and press `Done`. 
5. In the batch window, select `Number of slices` :material-arrow-right-bold: [COMPLETE LATER]
6. Select `TR` :material-arrow-right-bold: [COMPLETE LATER]
7. Select `TA` :material-arrow-right-bold: [COMPLETE LATER]
8. Select `Slice order` :material-arrow-right-bold: [COMPLETE LATER]
9. Select `Reference slice` :material-arrow-right-bold: [COMPLETE LATER]
10. Save this batch for future reference - `File` :material-arrow-right-bold: `Save batch`. Give the file a meaningful name, such as `slice_timing_batch.mat`. 
8. Now run your batch by pressing :material-play: in the top left corner. The button will be green if all required fields have specified inputs. 

??? info "Slice timing correction - before or after realignment?"
    There is some debate about whether slice timing correction should be used before or after realignment (i.e. motion correction). Performing motion correction first, might mean that data acquired at one time point may be moved to another slice. On the other hand, performing slice timing correction first may result in propagation of motion-induced intensity changes from one volume to the rest of the timeseries. 
    
    It is advised to use slice timing correction first if:

    * you use a complex slice order sequence (i.e. anything that is not contiguous),

    * you use a contiguous slice order but expect significant head movement.
    
    On the other hand, realignment can be done first if you use a contiguous slice order and expect only slight head movement. 
    
    To avoid the interactions between realignment and slice timing correction and propagation of errors from one step to the next step, methods performing simultaneous realignment and slice timing correction have been designed. To learn more about them, visit the [SpaceTimeRealigner section of the nipype documentation](https://nipype.readthedocs.io/en/0.13.1/interfaces/generated/interfaces.nipy/preprocess.html) and the companion paper [Roche (2011)](https://doi.org/10.1109/TMI.2011.2131152). 

### Video walkthrough

--8<-- "addons/abbreviations.md"