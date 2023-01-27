# fMRI data preprocessing

## Normalisation

Normalisation refers to bringing your data into a standard template space to allow group-level statistics. 

??? info "Why normalise my data?" 
    Normalisation is an essential step in neuroimaging analysis that allows group-level analyses. When neuroimaging data is normalised it means it has been transformed to fit into a standard template space. While many different templates exist, the most commonly used one is [MNI152](https://mcin.ca/research/neuroimaging-methods/atlases/). 

    For a thorough overview of issues related to normalisation, see the SPM book:

    [Penny, W., Friston, K., Ashburner, J., Kiebel, S., & Nichols, T. (2006). *Statistical parametric mapping: The analysis of functional brain images* (1st ed.).](http://www.elsevierdirect.com/product.jsp?isbn=9780123725608&srccode=89660)

    And other readings:

    [Brett, M., Johnsrude, I. S., & Owen, A. M. (2002). The problem of functional localization in the human brain. *Nature Reviews Neuroscience, 3*(3), 243-249.](https://doi.org/10.1038/nrn756)

    [Jenkinson, M. & Chappell, M. (2018). *Introduction to neuroimaging analysis*. Oxford University Press.](http://www.neuroimagingprimers.org/examples/introduction-primer-example-boxes/)

    [Poldrack, R. A., Mumford, J. A., & Nichols, T. E. (2011). *Handbook of functional MRI data analysis*. Cambridge University Press.](https://www.cambridge.org/core/books/handbook-of-functional-mri-data-analysis/8EDF966C65811FCCC306F7C916228529)

1. From the SPM menu panel, select `Normalise (Write)`. You will see a pop-up window appear looking like this:

    ![](../../assets/figures/normalisation_batch.png)

2. Select `Data` :material-arrow-right-bold: `New: Subject`.
3. Select `Deformation field`.
4. In the pop-up window, use the left-hand panel to navigate to `sub-01/anat/`. 
4. From the right-hand panel, select the deformation field generated during [coregistration](./coregistration.md) - `y_sub-01_T1w.nii` and press `Done`.
5. Select `Images to write`.
6. In the pop-up window, navigate to `sub-01/func/`. 
7. From the right-hand panel, select the religned and slice time corrected data - `arsub-01_task-auditory.nii`. Use the box underneath the `Filter` button to show a 4D file by typing in `NaN` and pressing ++return++. You can do this in combination with filtering for files starting with `ar` by typing in `^ar.*` in the `Filter` box and pressing ++return++. 
8. *Optional*: Under `Writing options`, change `Voxel sizes` form `[2 2 2]` to `[3 3 3]`. This will write images at a resolution closer to that at which they were acquired.
9. Save this batch for future reference - `File` :material-arrow-right-bold: `Save batch` and name it, e.g. `normalisation_batch.mat.`
10. Run your batch by pressing :material-play:.

SPM will now write spatially normalised files to the functional data directory, i.e. `sub-01/func/`. These files have the prefix `w`.

### Video walkthrough

--8<-- "addons/abbreviations.md"