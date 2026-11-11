# Analysis of an auditory fMRI dataset

This tutorial shows how to write equivalent batch scripts in MATLAB and
in Python, for analysing the auditory fMRI dataset available at
[http://www.fil.ion.ucl.ac.uk/spm/data/auditory/](http://www.fil.ion.ucl.ac.uk/spm/data/auditory/).


!!! example "Import the SPM-Python module"

    === ":simple-python: Python"

        ```python
        import spm                # (1)!
        import os
        import os.path as op      # (2)!
        import zipfile            # (3)!

        num = spm.Array.from_any  # (4)!
        ```

        1.  The SPM-Python module
        2.  To manipulate file paths
        3.  To unzip the downloaded data
        4.  A short alias to create numeric arrays

!!! example "Download and unzip the Auditory data"

    === ":custom-matlab: MATLAB"

        ```matlab
        data_path = fileparts(mfilename('fullpath'));
        if isempty(data_path), data_path = pwd; end
        fprintf('%-40s:', 'Downloading Auditory dataset...');
        urlwrite('http://www.fil.ion.ucl.ac.uk/spm/download/data/MoAEpilot/MoAEpilot.zip','MoAEpilot.zip');
        unzip(fullfile(data_path,'MoAEpilot.zip'));
        fprintf(' %30s\n', '...done');
        ```

    === ":simple-python: Python"

        ```python
        url = "https://www.fil.ion.ucl.ac.uk/spm/download/data/MoAEpilot/MoAEpilot.zip"
        try:
            data_path = op.dirname(__file__)
        except NameError:
            data_path = os.getcwd()
        zip_path = op.join(data_path, "MoAEpilot.zip")

        print(f"{'Downloading Auditory dataset...':<40s}")
        spm.Runtime.call("urlwrite", url, zip_path) # (1)!
        with zipfile.ZipFile(zip_path, "r") as f:
            f.extractall(data_path)
        print(f" {'...done':30s}\n")
        ```

        1.  Only SPM functions have a "nice" python wrapper, but you can
            also call any MATLAB function using `spm.Runtime.call`.

!!! example "Initialise SPM"

    === ":custom-matlab: MATLAB"

        ```matlab
        spm('Defaults','fMRI');
        spm_jobman('initcfg');
        ```

    === ":simple-python: Python"

        ```python
        spm.spm("Defaults", "fMRI")
        spm.spm_jobman("initcfg")
        ```

!!! example "Preamble: dummy scans"

    === ":custom-matlab: MATLAB"

        ```matlab
        f = spm_select('FPList', fullfile(data_path,'fM00223'), '^f.*\.img$') ;

        clear matlabbatch

        matlabbatch{1}.cfg_basicio.file_dir.dir_ops.cfg_mkdir.parent = cellstr(data_path);
        matlabbatch{1}.cfg_basicio.file_dir.dir_ops.cfg_mkdir.name = 'dummy';

        matlabbatch{2}.cfg_basicio.file_dir.file_ops.file_move.files = cellstr(f(1:12,:));
        matlabbatch{2}.cfg_basicio.file_dir.file_ops.file_move.action.moveto = cellstr(fullfile(data_path,'dummy'));

        spm_jobman('run',matlabbatch);
        ```

    === ":simple-python: Python"

        ```python
        f = spm.spm_select("FPList", op.join(data_path, "fM00223"), r"^f.*\.img$")

        matlabbatch = spm.Cell() # (1)!

        matlabbatch(0).cfg_basicio.file_dir.dir_ops.cfg_mkdir.parent = [data_path]  # (2)!
        matlabbatch(0).cfg_basicio.file_dir.dir_ops.cfg_mkdir.name = "dummy"        # (3)!

        matlabbatch(0).cfg_basicio.file_dir.file_ops.file_move.files = f[:12]
        matlabbatch(0).cfg_basicio.file_dir.file_ops.file_move.action.moveto = [
            op.join(data_path, "dummy")
        ]  # (4)!

        spm.spm_jobman("run", matlabbatch)
        ```

        1.  In MATLAB, variables do not need to be declared before their
            first use, but they do in Python. SPM batches are cells of
            structures, where each structure in the cell describes a job.
            We therefore declare `matlabbatch` as a `spm.Cell` object,
            which is a Python wrapper for a MATLAB `cell` (or `{}`).
        2.  In Python, indices start at 0 so the first job is `matlabbatch(0)`
            instead of `matlabbatch{1}` in MATLAB.
        3.  SPM-Python objects (`spm.Cell`, `spm.Struct`, etc.) do support
            implicitly defined elements. Here we did not need to allocate
            the first element in the cell, and we did not need to instantiate
            the hierarchy of fields in the nested structure. This allows
            the Python syntax to more closely resemble the MATLAB syntax,
            while still being valid Python code.
        4.  Python lists are interpreted as MATLAB cells.

!!! example "Spatial preprocessing"

    === ":custom-matlab: MATLAB"

        ```matlab
        f = spm_select('FPList', fullfile(data_path,'fM00223'), '^f.*\.img$');
        a = spm_select('FPList', fullfile(data_path,'sM00223'), '^s.*\.img$');

        clear matlabbatch

        % Realign
        %--------------------------------------------------------------------------
        matlabbatch{1}.spm.spatial.realign.estwrite.data = {cellstr(f)};
        matlabbatch{1}.spm.spatial.realign.estwrite.roptions.which = [0 1];

        % Coregister
        %--------------------------------------------------------------------------
        matlabbatch{2}.spm.spatial.coreg.estimate.ref    = cellstr(spm_file(f(1,:),'prefix','mean'));
        matlabbatch{2}.spm.spatial.coreg.estimate.source = cellstr(a);

        % Segment
        %--------------------------------------------------------------------------
        matlabbatch{3}.spm.spatial.preproc.channel.vols  = cellstr(a);
        matlabbatch{3}.spm.spatial.preproc.channel.write = [0 1];
        matlabbatch{3}.spm.spatial.preproc.warp.write    = [0 1];

        % Normalise: Write
        %--------------------------------------------------------------------------
        matlabbatch{4}.spm.spatial.normalise.write.subj.def      = cellstr(spm_file(a,'prefix','y_','ext','nii'));
        matlabbatch{4}.spm.spatial.normalise.write.subj.resample = cellstr(f);
        matlabbatch{4}.spm.spatial.normalise.write.woptions.vox  = [3 3 3];

        matlabbatch{5}.spm.spatial.normalise.write.subj.def      = cellstr(spm_file(a,'prefix','y_','ext','nii'));
        matlabbatch{5}.spm.spatial.normalise.write.subj.resample = cellstr(spm_file(a,'prefix','m','ext','nii'));
        matlabbatch{5}.spm.spatial.normalise.write.woptions.vox  = [1 1 3];

        % Smooth
        %--------------------------------------------------------------------------
        matlabbatch{6}.spm.spatial.smooth.data = cellstr(spm_file(f,'prefix','w'));
        matlabbatch{6}.spm.spatial.smooth.fwhm = [6 6 6];

        spm_jobman('run',matlabbatch);
        ```

    === ":simple-python: Python"

        ```python
        f = spm.spm_select("FPList", op.join(data_path, "fM00223"), r"^f.*\.img$")
        a = spm.spm_select("FPList", op.join(data_path, "sM00223"), r"^s.*\.img$")

        matlabbatch = spm.Cell()

        # Realign
        # ----------------------------------------------------------------------
        matlabbatch(0).spm.spatial.realign.estwrite.data = [f]
        matlabbatch(0).spm.spatial.realign.estwrite.roptions.which = num([0, 1])  # (1)!

        # Coregister
        # ----------------------------------------------------------------------
        matlabbatch(1).spm.spatial.coreg.estimate.ref = [
            spm.spm_file(f[0, 0], "prefix", "mean")
        ]
        matlabbatch(1).spm.spatial.coreg.estimate.source = [a]

        # Segment
        # ----------------------------------------------------------------------
        matlabbatch(2).spm.spatial.preproc.channel.vols = [a]
        matlabbatch(2).spm.spatial.preproc.channel.write = num([0, 1])
        matlabbatch(2).spm.spatial.preproc.warp.write = num([0, 1])

        # Normalise: Write
        # ----------------------------------------------------------------------
        matlabbatch(3).spm.spatial.normalise.write.subj["def"] = [
            spm.spm_file(a, "prefix", "y_", "ext", "nii")
        ]
        matlabbatch(3).spm.spatial.normalise.write.subj.resample = f
        matlabbatch(3).spm.spatial.normalise.write.woptions.vox = num([3, 3, 3])

        matlabbatch(4).spm.spatial.normalise.write.subj["def"] = [
            spm.spm_file(a, "prefix", "y_", "ext", "nii")
        ]
        matlabbatch(4).spm.spatial.normalise.write.subj.resample = [
            spm.spm_file(a, "prefix", "m", "ext", "nii")
        ]
        matlabbatch(4).spm.spatial.normalise.write.woptions.vox = num([1, 1, 3])

        # Smooth
        # ----------------------------------------------------------------------
        matlabbatch(5).spm.spatial.smooth.data = spm.spm_file(f, "prefix", "w")[:, None]  # (2)!
        matlabbatch(5).spm.spatial.smooth.fwhm = num([6, 6, 6])

        spm.spm_jobman("run", matlabbatch)
        ```

        1.  Python lists are interpreted as MATLAB cells, so `[0, 1]` is
            equivalent to `{0 1}` in MATLAB. When numeric arrays are
            expected, they must be explictly converted to a `spm.Array`
            object using the `num` alias defined in the preamble.

            !!! note "Numpy arrays are also accepted"
        2.  In most cases, SPM batches expect a Nx1 cell of filenames,
            that is, a column vector rather than a row vector. However,
            the `spm_file` function returns a 1D list, which is interpreted
            as a row vector. To convert it to a column vector, we use the
            `[:, None]` indexing trick, which adds a new axis to the cell
            array.

!!! example "GLM specification, estimation, inference, results"

    === ":custom-matlab: MATLAB"

        ```matlab
        f = spm_select('FPList', fullfile(data_path,'fM00223'), '^swf.*\.img$');

        clear matlabbatch

        % Output Directory
        %--------------------------------------------------------------------------
        matlabbatch{1}.cfg_basicio.file_dir.dir_ops.cfg_mkdir.parent = cellstr(data_path);
        matlabbatch{1}.cfg_basicio.file_dir.dir_ops.cfg_mkdir.name = 'GLM';

        % Model Specification
        %--------------------------------------------------------------------------
        matlabbatch{2}.spm.stats.fmri_spec.dir = cellstr(fullfile(data_path,'GLM'));
        matlabbatch{2}.spm.stats.fmri_spec.timing.units = 'scans';
        matlabbatch{2}.spm.stats.fmri_spec.timing.RT = 7;
        matlabbatch{2}.spm.stats.fmri_spec.sess.scans = cellstr(f);
        matlabbatch{2}.spm.stats.fmri_spec.sess.cond.name = 'active';
        matlabbatch{2}.spm.stats.fmri_spec.sess.cond.onset = 6:12:84;
        matlabbatch{2}.spm.stats.fmri_spec.sess.cond.duration = 6;

        % Model Estimation
        %--------------------------------------------------------------------------
        matlabbatch{3}.spm.stats.fmri_est.spmmat = cellstr(fullfile(data_path,'GLM','SPM.mat'));

        % Contrasts
        %--------------------------------------------------------------------------
        matlabbatch{4}.spm.stats.con.spmmat = cellstr(fullfile(data_path,'GLM','SPM.mat'));
        matlabbatch{4}.spm.stats.con.consess{1}.tcon.name = 'Listening > Rest';
        matlabbatch{4}.spm.stats.con.consess{1}.tcon.weights = [1 0];
        matlabbatch{4}.spm.stats.con.consess{2}.tcon.name = 'Rest > Listening';
        matlabbatch{4}.spm.stats.con.consess{2}.tcon.weights = [-1 0];

        % Inference Results
        %--------------------------------------------------------------------------
        matlabbatch{5}.spm.stats.results.spmmat = cellstr(fullfile(data_path,'GLM','SPM.mat'));
        matlabbatch{5}.spm.stats.results.conspec.contrasts = 1;
        matlabbatch{5}.spm.stats.results.conspec.threshdesc = 'FWE';
        matlabbatch{5}.spm.stats.results.conspec.thresh = 0.05;
        matlabbatch{5}.spm.stats.results.conspec.extent = 0;
        matlabbatch{5}.spm.stats.results.print = false;

        % Rendering
        %--------------------------------------------------------------------------
        matlabbatch{6}.spm.util.render.display.rendfile = {fullfile(spm('Dir'),'canonical','cortex_20484.surf.gii')};
        matlabbatch{6}.spm.util.render.display.conspec.spmmat = cellstr(fullfile(data_path,'GLM','SPM.mat'));
        matlabbatch{6}.spm.util.render.display.conspec.contrasts = 1;
        matlabbatch{6}.spm.util.render.display.conspec.threshdesc = 'FWE';
        matlabbatch{6}.spm.util.render.display.conspec.thresh = 0.05;
        matlabbatch{6}.spm.util.render.display.conspec.extent = 0;

        spm_jobman('run',matlabbatch);
        ```

    === ":simple-python: Python"

        ```python
        f = spm.spm_select("FPList", op.join(data_path, "fM00223"), r"^swf.*\.img$")

        matlabbatch = spm.Cell()

        # Output Directory
        # ----------------------------------------------------------------------
        matlabbatch(0).cfg_basicio.file_dir.dir_ops.cfg_mkdir.parent = [data_path]
        matlabbatch(0).cfg_basicio.file_dir.dir_ops.cfg_mkdir.name = "GLM"

        # Model Specification
        # ----------------------------------------------------------------------
        matlabbatch(1).spm.stats.fmri_spec.dir = [op.join(data_path, "GLM")]
        matlabbatch(1).spm.stats.fmri_spec.timing.units = "scans"
        matlabbatch(1).spm.stats.fmri_spec.timing.RT = 7
        matlabbatch(1).spm.stats.fmri_spec.sess.scans = f
        matlabbatch(1).spm.stats.fmri_spec.sess.cond.name = "active"
        matlabbatch(1).spm.stats.fmri_spec.sess.cond.onset = num(range(6, 86, 12))
        matlabbatch(1).spm.stats.fmri_spec.sess.cond.duration = 6

        # Model Estimation
        # ----------------------------------------------------------------------
        matlabbatch(2).spm.stats.fmri_est.spmmat = [op.join(data_path, "GLM", "SPM.mat")]

        # Contrasts
        # ----------------------------------------------------------------------
        matlabbatch(3).spm.stats.con.spmmat = [op.join(data_path, "GLM", "SPM.mat")]
        matlabbatch(3).spm.stats.con.consess(0).tcon.name = "Listening > Rest"  # (1)!
        matlabbatch(3).spm.stats.con.consess(0).tcon.weights = num([1, 0])
        matlabbatch(3).spm.stats.con.consess(1).tcon.name = "Rest > Listening"
        matlabbatch(3).spm.stats.con.consess(1).tcon.weights = num([-1, 0])

        # Inference Results
        # ----------------------------------------------------------------------
        matlabbatch(4).spm.stats.results.spmmat = [op.join(data_path, "GLM", "SPM.mat")]
        matlabbatch(4).spm.stats.results.conspec.contrasts = 1
        matlabbatch(4).spm.stats.results.conspec.threshdesc = "FWE"
        matlabbatch(4).spm.stats.results.conspec.thresh = 0.05
        matlabbatch(4).spm.stats.results.conspec.extent = 0
        matlabbatch(4).spm.stats.results.print = False

        # Rendering
        # ----------------------------------------------------------------------
        matlabbatch(5).spm.util.render.display.rendfile = [
            op.join(spm.spm("Dir"), "canonical", "cortex_20484.surf.gii")
        ]
        matlabbatch(5).spm.util.render.display.conspec.spmmat = [
            op.join(data_path, "GLM", "SPM.mat")
        ]
        matlabbatch(5).spm.util.render.display.conspec.contrasts = 1
        matlabbatch(5).spm.util.render.display.conspec.threshdesc = "FWE"
        matlabbatch(5).spm.util.render.display.conspec.thresh = 0.05
        matlabbatch(5).spm.util.render.display.conspec.extent = 0

        spm.spm_jobman("run", matlabbatch)
        ```

        1. `consess` must be a cell of structures. When using implicit
          instantitation, as we do here, it is then important to use
          "round bracket" indexing, as square bracket indexing would
          instead create a structure array.
