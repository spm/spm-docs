

* **SPM**  
Statistical Parametric Mapping refers to the construction and assessment of spatially extended statistical processes used to test hypotheses about functional imaging data. These ideas have been instantiated in software that is called SPM.   
The SPM software package has been designed for the analysis of brain imaging data sequences. The sequences can be a series of images from different cohorts, or time-series from the same subject.   
The current release is designed for the analysis of fMRI, PET, SPECT, EEG and MEG.   


    * **Temporal**  
    Temporal pre-processing functions.   


        * [**Slice Timing**](./spm.temporal.st.md)  


    * **Spatial**  
    Spatial pre-processing functions.   


        * **Realign**  
        Within-subject registration of image time series for correcting (some of the) head motion artifacts.   


            * [**Realign: Estimate**](./spm.spatial.realign.estimate.md)  


            * [**Realign: Reslice**](./spm.spatial.realign.write.md)  


            * [**Realign: Estimate & Reslice**](./spm.spatial.realign.estwrite.md)  


        * [**Realign & Unwarp**](./spm.spatial.realignunwarp.md)  


        * **Coregister**  
        Within-subject registration using a rigid-body model. There are the options of estimating the transformation, reslicing images according to some rigid-body transformations, or both estimating and applying rigid-body transformations.   


            * [**Coregister: Estimate**](./spm.spatial.coreg.estimate.md)  


            * [**Coregister: Reslice**](./spm.spatial.coreg.write.md)  


            * [**Coregister: Estimate & Reslice**](./spm.spatial.coreg.estwrite.md)  


        * [**Segment**](./spm.spatial.preproc.md)  


        * **Normalise**  
        There are two components to spatial normalisation: There is the estimation part, whereby a deformation is estimated by deforming template data to match an individual scan; And there is the actual writing of the spatially normalised images, using the previously estimated deformation.   


            * [**Normalise: Estimate**](./spm.spatial.normalise.est.md)  


            * [**Normalise: Write**](./spm.spatial.normalise.write.md)  


            * [**Normalise: Estimate & Write**](./spm.spatial.normalise.estwrite.md)  


        * [**Smooth**](./spm.spatial.smooth.md)  


    * **Stats**  
    Statistical modelling and inference functions.   


        * [**fMRI model specification**](./spm.stats.fmri_spec.md)  


        * [**fMRI model specification (design only)**](./spm.stats.fmri_design.md)  


        * [**fMRI data specification**](./spm.stats.fmri_data.md)  


        * [**Factorial design specification**](./spm.stats.factorial_design.md)  


        * [**Model review**](./spm.stats.review.md)  


        * [**Model estimation**](./spm.stats.fmri_est.md)  


        * [**Contrast Manager**](./spm.stats.con.md)  


        * [**Results Report**](./spm.stats.results.md)  


        * **Mixed-effects (MFX) analysis**  
        Mixed-effects (MFX) analysis   


            * [**FFX Specification**](./spm.stats.mfx.ffx.md)  


            * [**MFX Specification**](./spm.stats.mfx.spec.md)  


        * **Bayesian Model Selection**  
        Bayesian Model Selection for group studies (fixed effects and random effects analysis).   


            * [**BMS: Maps (Inference)**](./spm.stats.bms_map.inference.md)  


            * [**BMS: Maps (Results)**](./spm.stats.bms_map.results.md)  


        * [**Physio/Psycho-Physiologic Interaction**](./spm.stats.ppi.md)  


        * [**Set Level test**](./spm.stats.setlevel.md)  


    * **DCM**  
    Dynamic Causal Modelling.   


        * **DCM specification**  
        .   


            * **DCM for fMRI**  
            Dynamic Causal Modelling for fMRI   


                * [**Specify group**](./spm.dcm.spec.fmri.group.md)  


                * [**Region specification**](./spm.dcm.spec.fmri.regions.md)  


                * [**Input specification**](./spm.dcm.spec.fmri.inputs.md)  


            * [**DCM for M/EEG**](./spm.dcm.spec.meeg.md)  


        * [**DCM estimation**](./spm.dcm.estimate.md)  


        * **Bayesian Model Selection**  
        Bayesian Model Selection for group studies (fixed effects and random effects analysis).   


            * [**Model Inference**](./spm.dcm.bms.inference.md)  


            * [**Review results**](./spm.dcm.bms.results.md)  


        * **Second level**  
        Parametric Empirical Bayes for DCM.   


            * [**Specify / Estimate PEB**](./spm.dcm.peb.specify.md)  


            * [**Compare / Average PEB models**](./spm.dcm.peb.compare.md)  


            * [**Search nested PEB models**](./spm.dcm.peb.reduce_all.md)  


            * [**Review PEB**](./spm.dcm.peb.peb_review.md)  


            * [**Predict (cross-validation)**](./spm.dcm.peb.predict.md)  


    * **M/EEG**  
    M/EEG functions.   


        * [**Conversion**](./spm.meeg.convert.md)  


        * **Preprocessing**  
        M/EEG Preprocessing.   


            * [**Epoching**](./spm.meeg.preproc.epoch.md)  


            * [**Prepare**](./spm.meeg.preproc.prepare.md)  


            * [**Montage**](./spm.meeg.preproc.montage.md)  


            * [**Filter**](./spm.meeg.preproc.filter.md)  


            * [**Baseline correction**](./spm.meeg.preproc.bc.md)  


            * [**Artefact detection**](./spm.meeg.preproc.artefact.md)  


            * [**Downsampling**](./spm.meeg.preproc.downsample.md)  


            * [**Merging**](./spm.meeg.preproc.merge.md)  


            * [**Fusion**](./spm.meeg.preproc.fuse.md)  


            * [**Combine planar**](./spm.meeg.preproc.combineplanar.md)  


            * [**Data reduction**](./spm.meeg.preproc.reduce.md)  


            * [**Crop**](./spm.meeg.preproc.crop.md)  


            * [**Remove bad trials**](./spm.meeg.preproc.remove.md)  


            * [**Define spatial confounds**](./spm.meeg.preproc.sconfounds.md)  


            * [**Correct sensor data**](./spm.meeg.preproc.correct.md)  


        * **Averaging**  
        M/EEG Averaging.   


            * [**Averaging**](./spm.meeg.averaging.average.md)  


            * [**Grandmean**](./spm.meeg.averaging.grandmean.md)  


            * [**Contrast over epochs**](./spm.meeg.averaging.contrast.md)  


        * **Images**  
        M/EEG Images.   


            * [**Convert2Images**](./spm.meeg.images.convert2images.md)  


            * [**Collapse time**](./spm.meeg.images.collapse.md)  


        * **Time-frequency**  
        M/EEG Time-Frequency.   


            * [**Time-frequency analysis**](./spm.meeg.tf.tf.md)  


            * [**Time-frequency rescale**](./spm.meeg.tf.rescale.md)  


            * [**Average over frequency**](./spm.meeg.tf.avgfreq.md)  


            * [**Average over time**](./spm.meeg.tf.avgtime.md)  


            * [**Cross-frequency coupling**](./spm.meeg.tf.cfc.md)  


        * **Source reconstruction**  
        M/EEG source reconstruction.   


            * [**Head model specification**](./spm.meeg.source.headmodel.md)  


            * [**MEG helmet head model specification**](./spm.meeg.source.headmodelhelmet.md)  


            * [**Source inversion**](./spm.meeg.source.invert.md)  


            * [**Source inversion, iterative**](./spm.meeg.source.invertiter.md)  


            * [**Simulation of sources**](./spm.meeg.source.simulate.md)  


            * [**Merge source estimates from multiple inversions**](./spm.meeg.source.inv_mix.md)  


            * [**Inversion results**](./spm.meeg.source.results.md)  


            * [**Source extraction**](./spm.meeg.source.extract.md)  


            * [**Add head model error**](./spm.meeg.source.coregshift.md)  


            * [**Add sensor error**](./spm.meeg.source.sensorshift.md)  


            * [**Bayesian Dipole fit**](./spm.meeg.source.dipfit.md)  


            * [**Bayesian moment fit**](./spm.meeg.source.momentfit.md)  


            * [**Distortion Trajectory**](./spm.meeg.source.eeg_shp_distort.md)  


            * [**Gain matrices for surfaces**](./spm.meeg.source.eeg_shp_gainmat.md)  


        * **Modelling**  
        M/EEG Modelling.   


            * [**Convolution modelling**](./spm.meeg.modelling.convmodel.md)  


            * [**GLM regressors**](./spm.meeg.modelling.eegreg.md)  


        * **OPM Preprocessing**  
        OPM Preprocessing.   


            * [**Create OPM object**](./spm.meeg.OPM.create.md)  


            * [**Synthetic Gradiometery**](./spm.meeg.OPM.denoise.md)  


            * [**Epoch M/EEG object on Trigger**](./spm.meeg.OPM.epoch.md)  


        * **Other**  
        M/EEG Other.   


            * [**Display**](./spm.meeg.other.review.md)  


            * [**Copy**](./spm.meeg.other.copy.md)  


            * [**Delete**](./spm.meeg.other.delete.md)  


    * **Util**  
    Utility tools.   


        * [**Display Image**](./spm.util.disp.md)  


        * [**Check Registration**](./spm.util.checkreg.md)  


        * **Rendering**  
        Rendering utilities.   


            * [**Extract Surface**](./spm.util.render.extract.md)  


            * [**Display Surface**](./spm.util.render.display.md)  


        * **Import**  
        Import.   


            * [**DICOM Import**](./spm.util.import.dicom.md)  


            * [**MINC Import**](./spm.util.import.minc.md)  


            * [**ECAT Import**](./spm.util.import.ecat.md)  


            * [**PAR/REC Import**](./spm.util.import.parrec.md)  


        * [**Image Calculator**](./spm.util.imcalc.md)  


        * [**Reorient Images**](./spm.util.reorient.md)  


        * [**Volume of Interest**](./spm.util.voi.md)  


        * [**Change Directory (Deprecated)**](./spm.util.cdir.md)  


        * [**Make Directory (Deprecated)**](./spm.util.md.md)  


        * [**Get Bounding Box**](./spm.util.bbox.md)  


        * [**De-face Images**](./spm.util.deface.md)  


        * [**Deformations**](./spm.util.defs.md)  


        * [**Tissue Volumes**](./spm.util.tvol.md)  


        * [**Print figure**](./spm.util.print.md)  


        * [**3D to 4D File Conversion**](./spm.util.cat.md)  


        * [**4D to 3D File Conversion**](./spm.util.split.md)  


        * [**Expand Image Frames**](./spm.util.exp_frames.md)  


        * [**Sendmail**](./spm.util.sendmail.md)  


    * **Tools**  
    Other tools.   
    Toolbox configuration files should be placed in the toolbox directory, with their own *_cfg_*.m files. If you write a toolbox, then you can include it in this directory - but remember to try to keep the function names unique (to reduce  clashes with other toolboxes).   
    See spm_cfg.m or MATLABBATCH documentation for information about the form of SPM's configuration files.   


        * **Dartel Tools**  
        This toolbox is based around the "A Fast Diffeomorphic Registration Algorithm" paper. The idea is to register images by computing a "flow field", which can then be exponentiated to generate both forward and backward deformations.   


            * [**Run Dartel (create Templates)**](./spm.tools.dartel.warp.md)  


            * [**Run Dartel (existing Templates)**](./spm.tools.dartel.warp1.md)  


            * [**Normalise to MNI Space**](./spm.tools.dartel.mni_norm.md)  


            * [**Create Warped**](./spm.tools.dartel.crt_warped.md)  


            * [**Jacobian determinants**](./spm.tools.dartel.jacdet.md)  


            * [**Create Inverse Warped**](./spm.tools.dartel.crt_iwarped.md)  


            * [**Population to ICBM Registration**](./spm.tools.dartel.popnorm.md)  


            * **Kernel utilities**  
            Dartel can be used for generating matrices of dot-products for various kernel pattern-recognition procedures.   


                * [**Kernel from Images**](./spm.tools.dartel.kernfun.reskern.md)  


                * [**Kernel from Flows**](./spm.tools.dartel.kernfun.flokern.md)  


        * **DAiSS (beamforming)**  
        Data analysis in source space toolbox   


            * [**Group analysis**](./spm.tools.beamforming.group.md)  


            * [**Prepare data**](./spm.tools.beamforming.data.md)  


            * [**Copy analysis**](./spm.tools.beamforming.copy.md)  


            * [**Define sources**](./spm.tools.beamforming.sources.md)  


            * [**Covariance features**](./spm.tools.beamforming.features.md)  


            * [**Inverse solution**](./spm.tools.beamforming.inverse.md)  


            * [**Output**](./spm.tools.beamforming.output.md)  


            * [**Write**](./spm.tools.beamforming.write.md)  


            * [**View**](./spm.tools.beamforming.view.md)  


        * **FieldMap**  
        The FieldMap toolbox generates unwrapped field maps which are converted to voxel displacement maps (VDM) that can be used to unwarp geometrically distorted EPI images.   


            * [**Calculate VDM**](./spm.tools.fieldmap.calculatevdm.md)  


            * [**Apply VDM **](./spm.tools.fieldmap.applyvdm.md)  


        * **Longitudinal Registration**  
        Longitudinal registration of (typically) T1-weighted MRI to identify volumetric changes (e.g., atrophy).   


            * [**Pairwise Longitudinal Registration**](./spm.tools.longit.pairwise.md)  


            * [**Serial Longitudinal Registration**](./spm.tools.longit.series.md)  


        * **Multi-Brain toolbox**  
        The Multi-Brain (MB) toolbox has the general aim of integrating a number of disparate image analysis components within a single unified generative modelling framework (segmentation, nonlinear registration, image translation, etc.).   


            * [**Fit Multi-Brain model**](./spm.tools.mb.run.md)  


            * [**Merge tissues**](./spm.tools.mb.merge.md)  


            * [**Output**](./spm.tools.mb.out.md)  


            * [**Image Labelling**](./spm.tools.mb.fil.md)  


            * [**Spatially normalise using Multi-Brain**](./spm.tools.mb.mbnorm.md)  


        * **Old Normalise**  
        These ancient tools  are for spatially normalising MRI, PET or SPECT images into a standard space defined by some template image[s]. The transformation can also be applied to any other image that has been coregistered with these scans.   


            * [**Old Normalise: Estimate**](./spm.tools.oldnorm.est.md)  


            * [**Old Normalise: Write**](./spm.tools.oldnorm.write.md)  


            * [**Old Normalise: Estimate & Write**](./spm.tools.oldnorm.estwrite.md)  


        * [**Old Segment**](./spm.tools.oldseg.md)  


        * **Rendering**  
        This toolbox provides a limited range of surface rendering options. The idea is to first extract surfaces from image data, which are saved in ``surf_*.gii`` files. These can then be loaded and displayed as surfaces.   


            * [**Surface extraction**](./spm.tools.render.SExtract.md)  


            * [**Surface Rendering**](./spm.tools.render.SRender.md)  


        * **Shoot Tools**  
        This toolbox is based around the "Diffeomorphic Registration using Geodesic Shooting and Gauss-Newton Optimisation" paper . The idea is to register images by estimating an initial velocity field, which can then be integrated to generate both forward and backward deformations.   


            * [**Run Shooting (create Templates)**](./spm.tools.shoot.warp.md)  


            * [**Run Shoot (existing Templates)**](./spm.tools.shoot.warp1.md)  


            * [**Write Normalised**](./spm.tools.shoot.norm.md)  


            * **Kernel Utilities**  
            The Shoot toolbox can be used for generating matrices of dot products for various kernel pattern recognition procedures.   


                * [**Kernel from velocities**](./spm.tools.shoot.kernfun.velkern.md)  


                * [**Generate Scalar Momenta**](./spm.tools.shoot.kernfun.scalmom.md)  


                * [**Kernel from Images**](./spm.tools.shoot.kernfun.reskern.md)  


        * **Spatial Tools**  
        A selection of work-in-progress tools for various spatial processing tasks.   


            * [**Slice-to-volume alignment**](./spm.tools.spatial.slice2vol.md)  


            * [**SCOPE**](./spm.tools.spatial.scope.md)  


            * [**Total-variation denoising**](./spm.tools.spatial.denoise.md)  


        * **TSSS**  
        Temporal Signal Space Separation (TSSS) toolbox   


            * [**TSSS denoising**](./spm.tools.tsss.tsss.md)  


            * [**TSSS space conversion**](./spm.tools.tsss.momentspace.md)  
