# Coregistration of two images using core functions

This tutorial demonstrates how to coregister two images using the core
functions of SPM, instead of relying on the batch system. This leaves
the input images untouched and does not write any output files. Instead,
the calculated co-registration parameters are returned and can be converter
to an affine transformation matrix.

!!! python "Import SPM"
    ```python
    import spm            # (1)!
    import os             # (2)!
    import os.path as op  # (3)!
    import zipfile        # (4)!
    ```

    1. The SPM-Python module
    2. Standard operating system tools, which we'll use to create directories
    3. To manipulate file paths
    4. To extract the example data from a zip file

!!! python "Download example data"
    ```python
    print("Downloading Auditory dataset... ")
    spm.Runtime.call("urlwrite", url, zip_path)  # (1)!
    with zipfile.ZipFile(zip_path, "r") as f:
        f.extractall(data_path)
    ```

    1.  Only SPM functions have a "nice" python wrapper, but you can
        also call any MATLAB function using `spm.Runtime.call`.


!!! python "Paths to the two images to co-register"
    ```python
    fname_func = op.join(data_path, "fM00223", "fM00223_004.img")
    fname_struct = op.join(data_path, "sM00223", "sM00223_002.img")
    ```

!!! python "Coregister the two images"
    ```python
    spm.spm_figure('Create', 'Graphics', 'Graphics', 'on')  # (1)!

    ref = spm.spm_vol(fname_struct)  # (2)!
    mov = spm.spm_vol(fname_func)

    prm = spm.spm_coreg(ref, mov)   # (3)!
    ```

    1. Create a graphics window to display the results
    2. Map the two images into a `spm_vol` object.
    3. Co-register the two images.


!!! python "Convert the results to an affine transformation matrix"
    ```python
    M = spm.spm_matrix(prm)  # (1)!
    ```

    1. The resulting parameters are the translations and rotations estimated
       by the algorithms. They can be converted to a 4x4 affine transformation
       matrix using `spm_matrix`.
