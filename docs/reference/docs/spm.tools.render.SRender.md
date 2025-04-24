# Surface Rendering  
This utility is for visualising surfaces.  Surfaces first need to be extracted and saved in ``surf_*.gii`` files using the surface extraction routine.   

* **Objects** (create a list of items)  
Several surface objects can be displayed together in different colours and with different reflective properties.   

    * **Object**   
    Each object is a surface (from a ``surf_*.gii`` file), which may have a number of light-reflecting qualities, such as colour and shinyness.   

        * **Surface File** (select files)  
        Filename of the ``surf_*.gii`` file containing the rendering information. This can be generated via the surface extraction routine in SPM. Normally, a surface is extracted from grey and white matter tissue class images, but it is also possible to threshold e.g. an spmT image so that activations can be displayed.   

        * **Color**   
        Specify the colour using a mixture of red, green and blue. For example, white is specified by ``1,1,1``, black is by ``0,0,0`` and purple by ``1,0,1``.   

            * **Red** (choose from the menu)  
            The intensity of the red colouring (0 to 1).   

            * **Green** (choose from the menu)  
            The intensity of the green colouring (0 to 1).   

            * **Blue** (choose from the menu)  
            The intensity of the blue colouring (0 to 1).   

        * **Diffuse Strength** (choose from the menu)  
        The strength with which the object diffusely reflects light. Mat surfaces reflect light diffusely, whereas shiny surfaces reflect speculatively.   

        * **Ambient Strength** (choose from the menu)  
        The strength with which the object reflects ambient (non-directional) lighting.   

        * **Specular Strength** (choose from the menu)  
        The strength with which the object specularly reflects light (i.e. how shiny it is). Mat surfaces reflect light diffusely, whereas shiny surfaces reflect speculatively.   

        * **Specular Exponent** (choose from the menu)  
        A parameter describing the specular reflectance behaviour. It relates to the size of the high-lights.   

        * **Specular Color Reflectance** (choose from the menu)  
        Another parameter describing the specular reflectance behaviour.   

        * **Face Alpha** (choose from the menu)  
        The opaqueness of the surface.   
        A value of 1 means it is opaque, whereas a value of 0 means it is transparent.   

* **Lights** (create a list of items)  
There should be at least one light specified so that the objects can be clearly seen.   

    * **Light**   
    Specification of a light source in terms of position and colour.   

        * **Position** (enter text)  
        The position of the light in 3D.   

        * **Color**   
        Specify the colour using a mixture of red, green and blue. For example, white is specified by ``1,1,1``, black is by ``0,0,0`` and purple by ``1,0,1``.   

            * **Red** (choose from the menu)  
            The intensity of the red colouring (0 to 1).   

            * **Green** (choose from the menu)  
            The intensity of the green colouring (0 to 1).   

            * **Blue** (choose from the menu)  
            The intensity of the blue colouring (0 to 1).   
