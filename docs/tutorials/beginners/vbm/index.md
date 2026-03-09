# Voxel-Based Morphometry

??? info "For MSc Advanced Neuroimaging students"
    These notes were originally written for the MSc in Advanced Neuroimaging students at UCL.

    **Learning Objectives**

    * Increase familiarity with using MATLAB and the SPM12 software.

    * Learn about the image features that voxel-based morphometry compares and how they are derived.

    * Understand some of the limitations of the method and how to be more cautious when reading papers.

    **Data and SPM**

    Data is provided for the MSc students, but others may wish to use their own, or download datasets from (e.g.) [http://www.brain-development.org/](http://www.brain-development.org/).

    A script is provided (on Moodle) for students to download and install the SPM software, as well as the data needed for the workshop.

    The data provided are a selection of T1-weighted scans from the freely available IXI dataset, available via [http://www.brain-development.org/](http://www.brain-development.org/).  Note that the scans were acquired in the sagittal orientation, but a matrix in the NIfTI headers encodes this information so SPM can treat them as axial.

The tutorial will use SPM12, which is is available from [http://www.fil.ion.ucl.ac.uk/spm/](http://www.fil.ion.ucl.ac.uk/spm/), and is the latest SPM software release.
SPM runs in the MATLAB package, which is worth learning how to program a little if you ever plan to do any imaging research.

??? note "Older SPM versions"
    Older versions are SPM8, SPM5, SPM2, SPM99, SPM96 and SPM91 (SPMclassic). Anything before SPM5 is considered ancient, and any good manuscript reviewer will look down their noses at studies done using ancient software versions.


The overall plan is:

*    [Display & Check Reg](displaying_images.md) to start SPM and display some images.

    -    Start up SPM.

    -    Check that the images are in a suitable format (**Check Reg** and **Display** buttons).

*    [Image Processing](image_processing_dartel.md) to prepare the data for VBM analysis.

    -    Segment the images, to identify grey and white matter (using the **Segment** button). Grey matter will eventually be warped to MNI space.  This step also generates "imported" images, which will be used in the next step.

    -    Estimate the deformations that best align the images together by iteratively registering the imported images with their average (**SPM:material-arrow-right-bold:Tools:material-arrow-right-bold:Shoot Tools:material-arrow-right-bold:Run Shooting (create Templates)**).

    -    Generate spatially normalised and smoothed Jacobian scaled grey matter images, using the deformations estimated in the previous step (**SPM:material-arrow-right-bold:Tools:material-arrow-right-bold:Shoot Tools:material-arrow-right-bold:Write Normalised**).

*    [Statistical Analysis](statistical_analysis.md) to identify differences.

    -    Do some statistics on the smoothed images (**Basic models**, **Estimate** and **Results** options).

