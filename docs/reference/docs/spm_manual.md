

* **SPM**  
.  
Statistical Parametric Mapping refers to the construction and assessment of spatially extended statistical processes used to test hypotheses about functional imaging data. These ideas have been instantiated in software that is called SPM.  
.  
The SPM software package has been designed for the analysis of brain imaging data sequences. The sequences can be a series of images from different cohorts, or time-series from the same subject.  
.  
The current release is designed for the analysis of fMRI, PET, SPECT, EEG and MEG.  
.  


    * **Temporal**  
    Temporal pre-processing functions.  


        * [**Slice Timing**](../spm.temporal.st/)  


    * **Spatial**  
    Spatial pre-processing functions.  


        * **Realign**  
        Within-subject registration of image time series for correcting (some of the) head motion artifacts.  


            * [**Realign: Estimate**](../spm.spatial.realign.estimate/)  


            * [**Realign: Reslice**](../spm.spatial.realign.write/)  


            * [**Realign: Estimate & Reslice**](../spm.spatial.realign.estwrite/)  


        * [**Realign & Unwarp**](../spm.spatial.realignunwarp/)  


        * **Coregister**  
        Within-subject registration using a rigid-body model. There are the options of estimating the transformation, reslicing images according to some rigid-body transformations, or both estimating and applying rigid-body transformations.  


            * [**Coregister: Estimate**](../spm.spatial.coreg.estimate/)  


            * [**Coregister: Reslice**](../spm.spatial.coreg.write/)  


            * [**Coregister: Estimate & Reslice**](../spm.spatial.coreg.estwrite/)  


        * [**Segment**](../spm.spatial.preproc/)  


        * **Normalise**  
        There are two components to spatial normalisation: There is the estimation part, whereby a deformation is estimated by deforming template data to match an individual scan; And there is the actual writing of the spatially normalised images, using the previously estimated deformation.  


            * [**Normalise: Estimate**](../spm.spatial.normalise.est/)  


            * [**Normalise: Write**](../spm.spatial.normalise.write/)  


            * [**Normalise: Estimate & Write**](../spm.spatial.normalise.estwrite/)  


        * [**Smooth**](../spm.spatial.smooth/)  


    * **Stats**  
    Statistical modelling and inference functions.  


        * [**fMRI model specification**](../spm.stats.fmri_spec/)  


        * [**fMRI model specification (design only)**](../spm.stats.fmri_design/)  


        * [**fMRI data specification**](../spm.stats.fmri_data/)  


        * [**Factorial design specification**](../spm.stats.factorial_design/)  


        * [**Model review**](../spm.stats.review/)  


        * [**Model estimation**](../spm.stats.fmri_est/)  


        * [**Contrast Manager**](../spm.stats.con/)  


        * [**Results Report**](../spm.stats.results/)  


        * **Mixed-effects (MFX) analysis**  
        Mixed-effects (MFX) analysis  


            * [**FFX Specification**](../spm.stats.mfx.ffx/)  


            * [**MFX Specification**](../spm.stats.mfx.spec/)  


        * **Bayesian Model Selection**  
        Bayesian Model Selection for group studies (fixed effects and random effects analysis).  


            * [**BMS: Maps (Inference)**](../spm.stats.bms_map.inference/)  


            * [**BMS: Maps (Results)**](../spm.stats.bms_map.results/)  


        * [**Physio/Psycho-Physiologic Interaction**](../spm.stats.ppi/)  


        * [**Set Level test**](../spm.stats.setlevel/)  


    * **DCM**  
    Dynamic Causal Modelling.  


        * **DCM specification**  
        .  


            * **DCM for fMRI**  
            Dynamic Causal Modelling for fMRI  


                * [**Specify group**](../spm.dcm.spec.fmri.group/)  


                * [**Region specification**](../spm.dcm.spec.fmri.regions/)  


                * [**Input specification**](../spm.dcm.spec.fmri.inputs/)  


            * [**DCM for M/EEG**](../spm.dcm.spec.meeg/)  


        * [**DCM estimation**](../spm.dcm.estimate/)  


        * **Bayesian Model Selection**  
        Bayesian Model Selection for group studies (fixed effects and random effects analysis).  


            * [**Model Inference**](../spm.dcm.bms.inference/)  


            * [**Review results**](../spm.dcm.bms.results/)  


        * **Second level**  
        Parametric Empirical Bayes for DCM.  


            * [**Specify / Estimate PEB**](../spm.dcm.peb.specify/)  


            * [**Compare / Average PEB models**](../spm.dcm.peb.compare/)  


            * [**Search nested PEB models**](../spm.dcm.peb.reduce_all/)  


            * [**Review PEB**](../spm.dcm.peb.peb_review/)  


            * [**Predict (cross-validation)**](../spm.dcm.peb.predict/)  


    * **M/EEG**  
    M/EEG functions.  


        * [**Conversion**](../spm.meeg.convert/)  


        * **Preprocessing**  
        M/EEG Preprocessing.  


            * [**Epoching**](../spm.meeg.preproc.epoch/)  


            * [**Prepare**](../spm.meeg.preproc.prepare/)  


            * [**Montage**](../spm.meeg.preproc.montage/)  


            * [**Filter**](../spm.meeg.preproc.filter/)  


            * [**Baseline correction**](../spm.meeg.preproc.bc/)  


            * [**Artefact detection**](../spm.meeg.preproc.artefact/)  


            * [**Downsampling**](../spm.meeg.preproc.downsample/)  


            * [**Merging**](../spm.meeg.preproc.merge/)  


            * [**Fusion**](../spm.meeg.preproc.fuse/)  


            * [**Combine planar**](../spm.meeg.preproc.combineplanar/)  


            * [**Data reduction**](../spm.meeg.preproc.reduce/)  


            * [**Crop**](../spm.meeg.preproc.crop/)  


            * [**Remove bad trials**](../spm.meeg.preproc.remove/)  


            * [**Define spatial confounds**](../spm.meeg.preproc.sconfounds/)  


            * [**Correct sensor data**](../spm.meeg.preproc.correct/)  


        * **Averaging**  
        M/EEG Averaging.  


            * [**Averaging**](../spm.meeg.averaging.average/)  


            * [**Grandmean**](../spm.meeg.averaging.grandmean/)  


            * [**Contrast over epochs**](../spm.meeg.averaging.contrast/)  


        * **Images**  
        M/EEG Images.  


            * [**Convert2Images**](../spm.meeg.images.convert2images/)  


            * [**Collapse time**](../spm.meeg.images.collapse/)  


        * **Time-frequency**  
        M/EEG Time-Frequency.  


            * [**Time-frequency analysis**](../spm.meeg.tf.tf/)  


            * [**Time-frequency rescale**](../spm.meeg.tf.rescale/)  


            * [**Average over frequency**](../spm.meeg.tf.avgfreq/)  


            * [**Average over time**](../spm.meeg.tf.avgtime/)  


            * [**Cross-frequency coupling**](../spm.meeg.tf.cfc/)  


        * **Source reconstruction**  
        M/EEG source reconstruction.  


            * [**Head model specification**](../spm.meeg.source.headmodel/)  


            * [**MEG helmet head model specification**](../spm.meeg.source.headmodelhelmet/)  


            * [**Source inversion**](../spm.meeg.source.invert/)  


            * [**Source inversion, iterative**](../spm.meeg.source.invertiter/)  


            * [**Simulation of sources**](../spm.meeg.source.simulate/)  


            * [**Merge source estimates from multiple inversions**](../spm.meeg.source.inv_mix/)  


            * [**Inversion results**](../spm.meeg.source.results/)  


            * [**Source extraction**](../spm.meeg.source.extract/)  


            * [**Add head model error**](../spm.meeg.source.coregshift/)  


            * [**Add sensor error**](../spm.meeg.source.sensorshift/)  


            * [**Bayesian Dipole fit**](../spm.meeg.source.dipfit/)  


            * [**Bayesian moment fit**](../spm.meeg.source.momentfit/)  


            * [**Distortion Trajectory**](../spm.meeg.source.eeg_shp_distort/)  


            * [**Gain matrices for surfaces**](../spm.meeg.source.eeg_shp_gainmat/)  


        * **Modelling**  
        M/EEG Modelling.  


            * [**Convolution modelling**](../spm.meeg.modelling.convmodel/)  


            * [**GLM regressors**](../spm.meeg.modelling.eegreg/)  


        * **OPM Preprocessing**  
        OPM Preprocessing.  


            * [**Create OPM object**](../spm.meeg.OPM.create/)  


            * [**Synthetic Gradiometery**](../spm.meeg.OPM.denoise/)  


            * [**Epoch M/EEG object on Trigger**](../spm.meeg.OPM.epoch/)  


        * **Other**  
        M/EEG Other.  


            * [**Display**](../spm.meeg.other.review/)  


            * [**Copy**](../spm.meeg.other.copy/)  


            * [**Delete**](../spm.meeg.other.delete/)  


    * **Util**  
    Utility tools.  


        * [**Display Image**](../spm.util.disp/)  


        * [**Check Registration**](../spm.util.checkreg/)  


        * **Rendering**  
        Rendering utilities.  


            * [**Extract Surface**](../spm.util.render.extract/)  


            * [**Display Surface**](../spm.util.render.display/)  


        * **Import**  
        Import.  


            * [**DICOM Import**](../spm.util.import.dicom/)  


            * [**MINC Import**](../spm.util.import.minc/)  


            * [**ECAT Import**](../spm.util.import.ecat/)  


            * [**PAR/REC Import**](../spm.util.import.parrec/)  


        * [**Image Calculator**](../spm.util.imcalc/)  


        * [**Reorient Images**](../spm.util.reorient/)  


        * [**Volume of Interest**](../spm.util.voi/)  


        * [**Change Directory (Deprecated)**](../spm.util.cdir/)  


        * [**Make Directory (Deprecated)**](../spm.util.md/)  


        * [**Get Bounding Box**](../spm.util.bbox/)  


        * [**De-face Images**](../spm.util.deface/)  


        * [**Deformations**](../spm.util.defs/)  


        * [**Tissue Volumes**](../spm.util.tvol/)  


        * [**Print figure**](../spm.util.print/)  


        * [**3D to 4D File Conversion**](../spm.util.cat/)  


        * [**4D to 3D File Conversion**](../spm.util.split/)  


        * [**Expand Image Frames**](../spm.util.exp_frames/)  


        * [**Sendmail**](../spm.util.sendmail/)  


    * **Tools**  
    Other tools.  
    Toolbox configuration files should be placed in the toolbox directory, with their own *_cfg_*.m files. If you write a toolbox, then you can include it in this directory - but remember to try to keep the function names unique (to reduce  clashes with other toolboxes).  
    See spm_cfg.m or MATLABBATCH documentation for information about the form of SPM's configuration files.  


        * **Dartel Tools**  
        This toolbox is based around the "A Fast Diffeomorphic Registration Algorithm" paper. The idea is to register images by computing a "flow field", which can then be exponentiated to generate both forward and backward deformations.  


            * [**Run Dartel (create Templates)**](../spm.tools.dartel.warp/)  


            * [**Run Dartel (existing Templates)**](../spm.tools.dartel.warp1/)  


            * [**Normalise to MNI Space**](../spm.tools.dartel.mni_norm/)  


            * [**Create Warped**](../spm.tools.dartel.crt_warped/)  


            * [**Jacobian determinants**](../spm.tools.dartel.jacdet/)  


            * [**Create Inverse Warped**](../spm.tools.dartel.crt_iwarped/)  


            * [**Population to ICBM Registration**](../spm.tools.dartel.popnorm/)  


            * **Kernel utilities**  
            Dartel can be used for generating matrices of dot-products for various kernel pattern-recognition procedures.  


                * [**Kernel from Images**](../spm.tools.dartel.kernfun.reskern/)  


                * [**Kernel from Flows**](../spm.tools.dartel.kernfun.flokern/)  


        * **DAiSS (beamforming)**  
        Data analysis in source space toolbox  


            * [**Group analysis**](../spm.tools.beamforming.group/)  


            * [**Prepare data**](../spm.tools.beamforming.data/)  


            * [**Copy analysis**](../spm.tools.beamforming.copy/)  


            * [**Define sources**](../spm.tools.beamforming.sources/)  


            * [**Covariance features**](../spm.tools.beamforming.features/)  


            * [**Inverse solution**](../spm.tools.beamforming.inverse/)  


            * [**Output**](../spm.tools.beamforming.output/)  


            * [**Write**](../spm.tools.beamforming.write/)  


            * [**View**](../spm.tools.beamforming.view/)  


        * **FieldMap**  
        The FieldMap toolbox generates unwrapped field maps which are converted to voxel displacement maps (VDM) that can be used to unwarp geometrically distorted EPI images.  


            * [**Calculate VDM**](../spm.tools.fieldmap.calculatevdm/)  


            * [**Apply VDM **](../spm.tools.fieldmap.applyvdm/)  


        * **Longitudinal Registration**  
        Longitudinal registration of (typically) T1-weighted MRI to identify volumetric changes (e.g., atrophy).  


            * [**Pairwise Longitudinal Registration**](../spm.tools.longit.pairwise/)  


            * [**Serial Longitudinal Registration**](../spm.tools.longit.series/)  


        * **Multi-Brain toolbox**  
        The Multi-Brain (MB) toolbox has the general aim of integrating a number of disparate image analysis components within a single unified generative modelling framework (segmentation, nonlinear registration, image translation, etc.).  


            * [**Fit Multi-Brain model**](../spm.tools.mb.run/)  


            * [**Merge tissues**](../spm.tools.mb.merge/)  


            * [**Output**](../spm.tools.mb.out/)  


            * [**Image Labelling**](../spm.tools.mb.fil/)  


            * [**Spatially normalise using Multi-Brain**](../spm.tools.mb.mbnorm/)  


        * **Old Normalise**  
        These ancient tools  are for spatially normalising MRI, PET or SPECT images into a standard space defined by some template image[s]. The transformation can also be applied to any other image that has been coregistered with these scans.  


            * [**Old Normalise: Estimate**](../spm.tools.oldnorm.est/)  


            * [**Old Normalise: Write**](../spm.tools.oldnorm.write/)  


            * [**Old Normalise: Estimate & Write**](../spm.tools.oldnorm.estwrite/)  


        * [**Old Segment**](../spm.tools.oldseg/)  


        * **Rendering**  
        This toolbox provides a limited range of surface rendering options. The idea is to first extract surfaces from image data, which are saved in ``surf_*.gii`` files. These can then be loaded and displayed as surfaces.  


            * [**Surface extraction**](../spm.tools.render.SExtract/)  


            * [**Surface Rendering**](../spm.tools.render.SRender/)  


        * **Shoot Tools**  
        This toolbox is based around the "Diffeomorphic Registration using Geodesic Shooting and Gauss-Newton Optimisation" paper . The idea is to register images by estimating an initial velocity field, which can then be integrated to generate both forward and backward deformations.  


            * [**Run Shooting (create Templates)**](../spm.tools.shoot.warp/)  


            * [**Run Shoot (existing Templates)**](../spm.tools.shoot.warp1/)  


            * [**Write Normalised**](../spm.tools.shoot.norm/)  


            * **Kernel Utilities**  
            The Shoot toolbox can be used for generating matrices of dot products for various kernel pattern recognition procedures.  


                * [**Kernel from velocities**](../spm.tools.shoot.kernfun.velkern/)  


                * [**Generate Scalar Momenta**](../spm.tools.shoot.kernfun.scalmom/)  


                * [**Kernel from Images**](../spm.tools.shoot.kernfun.reskern/)  


        * **Spatial Tools**  
        A selection of work-in-progress tools for various spatial processing tasks.  


            * [**Slice-to-volume alignment**](../spm.tools.spatial.slice2vol/)  


            * [**SCOPE**](../spm.tools.spatial.scope/)  


            * [**Total-variation denoising**](../spm.tools.spatial.denoise/)  


        * **TSSS**  
        Temporal Signal Space Separation (TSSS) toolbox  


            * [**TSSS denoising**](../spm.tools.tsss.tsss/)  


            * [**TSSS space conversion**](../spm.tools.tsss.momentspace/)  
