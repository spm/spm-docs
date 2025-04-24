# Define sources  
Define source space for beamforming  

* **BF.mat file** (select files)  
Select BF.mat file.  

* **Reduce rank** (enter text)  
Enter rank for MEG and EEG lead fields [MEG EEG]  

* **Keep the original orientations** (choose from the menu)  
If not then the extra dimension is physically removed.  

* **Source space type ** (choose an option)  
.  

    * **Grid**   
    .  

        * **Grid resolution** (enter text)  
        Select the resolution of the grid (in mm)  

        * **Coordinate system** (choose from the menu)  
        Select the coordinate system in which the grid should be generated  

        * **Coordinate sources to** (choose from the menu)  
        The boundary to which the grid is confined  

    * **Phantom Grid**   
    .  

        * **Grid resolution** (enter text)  
        Select the resolution of the grid (in mm)  

    * **Cortical mesh**   
    .  

        * **How to orient the sources** (choose from the menu)  
        .  

        * **Downsample factor** (enter text)  
        A number that determines mesh downsampling  
        e.g 5 for taking every 5th vertex  

        * **Symmetric** (choose from the menu)  
        Create a symmetric mesh by reflecting on of the hemispheres.  

        * **Flip** (choose from the menu)  
        Flip the mesh relative to midsagittal plane.  

    * **Mni Coords**   
    .  

        * **Pos coords** (enter text)  
        .  

    * **Scalp mesh**   
    .  

        * **How to orient the sources** (choose from the menu)  
        .  

    * **VOIs in MNI space**   
    .  

        * **VOIs** (create a list of items)  
        .  

            * **VOI**   
            .  

                * **Label** (enter text)  
                Label for the VOI  

                * **MNI coordinates** (enter text)  
                Locations for the VOI in MNI coordinates  

                * **Orientation** (enter text)  
                Source orientatons (only for single points, leave zeros for unoriented)  

            * **Mask VOI**   
            .  

                * **Label** (enter text)  
                Label for the VOI  

                * **MNI mask** (select files)  
                Select a mask image  

        * **Radius** (enter text)  
        Radius (in mm) for the VOIs (leave 0 for single point)  

        * **Resolution** (enter text)  
        Resolution for placing grid points in each VOI (in mm)  

* **Normalise lead fields** (choose from the menu)  
Normalise the lead fields to yield array gain beamformer  

* **Visualise head model and sources** (choose from the menu)  
Visualise head model and sourses to verify that everythin was done correctly  
