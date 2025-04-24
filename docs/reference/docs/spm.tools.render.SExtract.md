# Surface extraction  
User-specified algebraic manipulations are performed on a set of images, with the result being used to generate a surface file. The user is prompted to supply images to work on and a number of expressions to evaluate, along with some thresholds. The expression should be a standard matlab expression, within which the images should be referred to as ``i1``, ``i2``, ``i3``,... etc. An isosurface file is created from the results at the user-specified threshold.

* **Input Images** (select files)  
These are the images that are used by the calculator.
They are referred to as ``i1``, ``i2``, ``i3``, etc in the order that they are specified.

* **Surfaces** (create a list of items)  
Multiple surfaces can be created from the same image data.

    * **Surface**   
    An expression and threshold for each of the surfaces to be generated.

        * **Expression** (enter text)  
        Example expressions (f):
            * Mean of six images (select six images)
               f = ``(i1+i2+i3+i4+i5+i6)/6``
            * Make a binary mask image at threshold of 100
               f = ``i1>100``
            * Make a mask from one image and apply to another
               f = ``i2.*(i1>100)``
                     - here the first image is used to make the mask, which is applied to the second image
            * Sum of n images
               f = ``i1 + i2 + i3 + i4 + i5 + ...``

        * **Surface isovalue(s)** (enter text)  
        Enter the value at which isosurfaces through the resulting image is to be computed.
