# Run Dartel (create Templates)  
Run the Dartel nonlinear image registration procedure. This involves iteratively matching all the selected images to a template generated from their own mean. A series of ``Template*.nii`` files are generated, which become increasingly crisp as the registration proceeds. Note that tissue maps should first be "imported", which is typically done at the **Segment** stage.   

* **Images** (create a list of items)  
Select the images to be warped together. Multiple sets of images can be simultaneously registered. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.   

    * **Images** (select files)  
    Select a set of imported images of the same type to be registered by minimising a measure of difference from the template.   

* **Settings**   
Various settings for the optimisation. The default values should work reasonably well for aligning tissue class images together.   

    * **Template basename** (enter text)  
    Enter the base for the template name.  Templates generated at each outer iteration of the procedure will be basename_1.nii, basename_2.nii etc.  If empty, then no template will be saved. Similarly, the estimated flow-fields will have the basename appended to them.   

    * **Regularisation Form** (choose from the menu)  
    The registration is penalised by some "energy" term.  Here, the form of this energy term is specified. Three different forms of regularisation can currently be used.   

    * **Outer Iterations** (create a list of items)  
    The images are averaged, and each individual image is warped to match this average.  This is repeated a number of times.   

        * **Outer Iteration**   
        Different parameters can be specified for each outer iteration. Each of them warps the images to the template, and then regenerates the template from the average of the warped images. Multiple outer iterations should be used for more accurate results, beginning with a more coarse registration (more regularisation) then ending with the more detailed registration (less regularisation).   

            * **Inner Iterations** (choose from the menu)  
            The number of Gauss-Newton iterations to be done within this outer iteration. After this, new average(s) are created, which the individual images are warped to match.   

            * **Reg params** (enter text)  
            For *linear elasticity*, the parameters are $\mu$, $\lambda$ and id. For *membrane energy*, the parameters are $\lambda$, unused and id, where id is a term for penalising absolute displacements, and should therefore be small.  For *bending energy*, the parameters are $\lambda$, id$_1$ and id$_2$, and the regularisation is by $(-\lambda \nabla + \mathsf{id}_1)^2 + \mathsf{id}_2$   
            Use more regularisation for the early iterations so that the deformations are smooth, and then use less for the later ones so that the details can be better matched.   

            * **Time Steps** (choose from the menu)  
            The number of time points used for solving the partial differential equations.  A single time point would be equivalent to a small deformation model. Smaller values allow faster computations, but are less accurate in terms of inverse consistency and may result in the one-to-one mapping breaking down.  Earlier iteration could use fewer time points, but later ones should use about 64 (or fewer if the deformations are very smooth).   

            * **Smoothing Parameter** (choose from the menu)  
            A LogOdds parameterisation of the template is smoothed using a multi-grid scheme.  The amount of smoothing is determined by this parameter.   

    * **Optimisation Settings**   
    Settings for the optimisation.  If you are unsure about them, then leave them at the default values.  Optimisation is by repeating a number of Levenberg-Marquardt iterations, in which the equations are solved using a full multi-grid (FMG) scheme. FMG and Levenberg-Marquardt are both described in Numerical Recipes (2nd edition).   

        * **LM Regularisation** (enter text)  
        Levenberg-Marquardt regularisation.  Larger values increase the the stability of the optimisation, but slow it down.  A value of zero results in a Gauss-Newton strategy, but this is not recommended as it may result in instabilities in the FMG.   

        * **Cycles** (choose from the menu)  
        Number of cycles used by the full multi-grid matrix solver. More cycles result in higher accuracy, but slow down the algorithm. See Numerical Recipes for more information on multi-grid methods.   

        * **Iterations** (choose from the menu)  
        Number of relaxation iterations performed in each multi-grid cycle. More iterations are needed if using *bending energy* regularisation, because the relaxation scheme only runs very slowly. See the chapter on solving partial differential equations in Numerical Recipes for more information about relaxation methods.   
