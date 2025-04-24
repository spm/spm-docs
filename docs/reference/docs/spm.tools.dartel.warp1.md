# Run Dartel (existing Templates)  
Run the Dartel nonlinear image registration procedure to match individual images to pre-existing template data. Start out with smooth templates, and select crisp templates for the later iterations. Note that tissue maps should first be "imported", which is typically done at the **Segment** stage.

* **Images** (create a list of items)  
Select the images to be warped together. Multiple sets of images can be simultaneously registered. For example, the first set may be a bunch of grey matter images, and the second set may be the white matter images of the same subjects.

    * **Images** (select files)  
    Select a set of imported images of the same type to be registered by minimising a measure of difference from the template.

* **Settings**   
Various settings for the optimisation. The default values should work reasonably well for aligning tissue class images together.

    * **Regularisation Form** (choose from the menu)  
    The registration is penalised by some "energy" term.  Here, the form of this energy term is specified. Three different forms of regularisation can currently be used.

    * **Outer Iterations** (create a list of items)  
    The images are warped to match a sequence of templates. Early iterations should ideally use smoother templates and more regularisation than later iterations.

        * **Outer Iteration**   
        Different parameters and templates can be specified for each outer iteration.

            * **Inner Iterations** (choose from the menu)  
            The number of Gauss-Newton iterations to be done within this outer iteration.

            * **Reg params** (enter text)  
            For *linear elasticity*, the parameters are $\mu$, $\lambda$ and id. For *membrane energy*, the parameters are $\lambda$, unused and id, where id is a term for penalising absolute displacements, and should therefore be small.  For *bending energy*, the parameters are $\lambda$, id$_1$ and id$_2$, and the regularisation is by $(-\lambda \nabla + \mathsf{id}_1)^2 + \mathsf{id}_2$
            Use more regularisation for the early iterations so that the deformations are smooth, and then use less for the later ones so that the details can be better matched.

            * **Time Steps** (choose from the menu)  
            The number of time points used for solving the partial differential equations.  A single time point would be equivalent to a small deformation model. Smaller values allow faster computations, but are less accurate in terms of inverse consistency and may result in the one-to-one mapping breaking down.  Earlier iteration could use fewer time points, but later ones should use about 64 (or fewer if the deformations are very smooth).

            * **Template** (select files)  
            Select template. Smoother templates should be used for the early iterations. Note that the template should be a 4D file, with the 4th dimension equal to the number of sets of images.

    * **Optimisation Settings**   
    Settings for the optimisation.  If you are unsure about them, then leave them at the default values.  Optimisation is by repeating a number of Levenberg-Marquardt iterations, in which the equations are solved using a full multi-grid (FMG) scheme. FMG and Levenberg-Marquardt are both described in Numerical Recipes (2nd edition).

        * **LM Regularisation** (enter text)  
        Levenberg-Marquardt regularisation.  Larger values increase the the stability of the optimisation, but slow it down.  A value of zero results in a Gauss-Newton strategy, but this is not recommended as it may result in instabilities in the FMG.

        * **Cycles** (choose from the menu)  
        Number of cycles used by the full multi-grid matrix solver. More cycles result in higher accuracy, but slow down the algorithm. See Numerical Recipes for more information on multi-grid methods.

        * **Iterations** (choose from the menu)  
        Number of relaxation iterations performed in each multi-grid cycle. More iterations are needed if using *bending energy* regularisation, because the relaxation scheme only runs very slowly. See the chapter on solving partial differential equations in Numerical Recipes for more information about relaxation methods.
