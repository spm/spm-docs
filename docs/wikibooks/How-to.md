# Introduction

This page lists a series of How-tos using SPM. They should work with all
recent versions of the software.

## How to manually change the orientation of an image?

<figure markdown>
  <div class="center">
  <img src="../../../assets/figures/wikibooks/Spm_reorient.png" style="width:100mm" />
  </div>
  <figcaption>SPM Display</figcaption>
</figure>

To change the origin of an image:

- open the image with SPM Display
- move the crosshair position so that it roughly points to the [anterior
  commissure](w:Anterior_commissure "wikilink") (AC).
- click on the Set Origin button
- click on the Reorient button and press done (your image is already
  selected). If you want to apply the same transformation to other
  images (e.g. if you have a series of functional images), select them
  all at this stage.
- say No to Do you want to save the reorientation matrix?

This will set the origin of the image (0 0 0 mm coordinates) to AC. You
might also want to rotate the image such that it is better aligned with
MNI space: to do so, you also need to edit the entries for the rotations
(in [radian](w:Radian "wikilink")) along the [pitch, roll and yaw
axes](w:Aircraft_principal_axes "wikilink").

You can also try to automate the process using
[coregistration](SPM/How-to#How_to_automatically_reorient_images.3F "wikilink").

See also: [Finding
Commissures](http://imaging.mrc-cbu.cam.ac.uk/imaging/FindingCommissures).

For earlier versions of SPM, see [Repositioning
MRIs](http://imaging.mrc-cbu.cam.ac.uk/meg/RepositioningMRIs).

## How to compute the mean of a set of images?

Use the **ImCalc** facility with *expression* \"mean(X)\" and *Data
Matrix* option set to \"yes - read images into data matrix\". You can
also choose the output filename (say *mean.img*) for the mean image and
its directory (default is current folder).

<figure>
<img src="../../assets/figures/wikibooks/SPM_Imcalc_mean_image.png" title="SPM Imcalc mean image" />
<figcaption>SPM Imcalc mean image</figcaption>
</figure>

Alternatively, you can use the **spm_mean_ui** function but the number
of options is limited - **ImCalc** should be preferred.

If you want to compute the sum instead, just use *expression*
\"sum(X)\".

## How to change file paths in a batch file or SPM.mat?

If you change the location of your data and/or the SPM installation, the
full paths contained in batch files and SPM.mat will be broken. An
utility function **spm_changepath** is available in SPM to replace all
occurrences of one string with another within a MAT-file. For example:

`spm_changepath('SPM.mat','C:\mydata','D:\Experiments\data');`

will update the *SPM.mat* file to replace all occurrence of *C:\mydata*
with *D:\Experiments\data*. A backup of the initial file can be found in
*SPM.mat.old*. A list of all the altered paths will be displayed at
runtime.

## How to choose the colour limits in a rendering?

By default, the colour limits are such that they span min/max of the
rendered data. If you want to specify them yourself, you can use the
following piece of code.

For surface rendering, use:

`H = getappdata(get(findobj('Tag','SPMMeshRender'),'Parent'),'handles');`  
`spm_mesh_render('CLim',H) % return current limits`  
`spm_mesh_render('CLim',H,[0 16]); % set limits to [0 16]`

For rendering on the three orthogonal views, use:

`global st`  
`st.vols{1}.blobs{1}.min = 0;`  
`st.vols{1}.blobs{1}.max = 16;`  
`spm_orthviews('redraw');`

## How to remove clusters under a certain size in a binary mask?

The following piece of code will read a binary mask (*ROI*), perform
connected component labelling, filter out all clusters under a certain
size in voxel (variable *k*) and save it as another mask (*ROIf*).

`ROI  = 'myvoi.nii';  % input image (binary, ie a mask)`  
`k    = 100;          % minimal cluster size`  
`ROIf = 'newvoi.nii'; % output image (filtered on cluster size)`  
  
`%-Connected Component labelling`  
`V = spm_vol(ROI);`  
`dat = spm_read_vols(V);`  
`[l2, num] = spm_bwlabel(double(dat>0),26);`  
`if ~num, warning('No clusters found.'); end`  
  
`%-Extent threshold, and sort clusters according to their extent`  
`[n, ni] = sort(histc(l2(:),0:num), 1, 'descend');`  
`l  = zeros(size(l2));`  
`n  = n(2:end); ni = ni(2:end)-1;`  
`ni = ni(n>=k); n  = n(n>=k);`  
`for i=1:length(n), l(l2==ni(i)) = i; end`  
`clear l2 ni`  
`fprintf('Selected %d clusters (out of %d) in image.\n',length(n),num);`  
  
`%-Write new image`  
`V.fname = ROIf;`  
`spm_write_vol(V,l~=0); % save as binary image. Remove '~=0' so as to`  
`                       % have cluster labels as their size. `  
`                       % or use (l~=0).*dat if input image was not binary`

## How to display NIfTI images in SPM when double-clicking on them in MATLAB Current Folder browser?

Save the following functions in your MATLAB path:

`function openimg(img)`  
`spm_image('Display',img);`

`function opennii(nii)`  
`N=nifti(nii);`  
`if size(N.dat,4) == 1`  
`   spm_image('Display',nii);`  
`else`  
`   if ~isdeployed, addpath(fullfile(spm('Dir'),'spm_orthviews')); end`  
`   spm_ov_browser('ui',spm_select('expand',nii));`  
`end`

## How to overlay a mask image on another one with transparency?

After using Display \> Add Overlay or CheckReg \> Overlay \> Add
coloured image, type the following:

`global st`  
`col = st.vols{1}.blobs{1}.colour;`  
`st.vols{1}.blobs{1}.colour = struct('cmap',[0 0 0;col],'prop',0.5);`

## How to change the voxel size of an image?

Run the following (with the appropriate voxel size) and select the
images you want to resample. The resliced images will be saved with an
\'r\' prefix:

`voxsiz = [2 2 2]; % new voxel size {mm}`  
`V = spm_select([1 Inf],'image');`  
`V = spm_vol(V);`  
`for i=1:numel(V)`  
`   bb        = spm_get_bbox(V(i));`  
`   VV(1:2)   = V(i);`  
`   VV(1).mat = spm_matrix([bb(1,:) 0 0 0 voxsiz])*spm_matrix([-1 -1 -1]);`  
`   VV(1).dim = ceil(VV(1).mat \ [bb(2,:) 1]' - 0.1)';`  
`   VV(1).dim = VV(1).dim(1:3);`  
`   spm_reslice(VV,struct('mean',false,'which',1,'interp',0)); % 1 for linear`  
`end`

## How to automatically reorient images?

The following function (from
[here](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=SPM;d1f675f1.0810))
will rigidly align images to a T1 template:

`function auto_reorient(p)`  
`if ~nargin`  
`    [p,sts] = spm_select(Inf,'image');`  
`    if ~sts, return; end`  
`end`  
`p = cellstr(p);`  
`vg = spm_vol(fullfile(spm('Dir'),'canonical','avg152T1.nii'));`  
`tmp = [tempname '.nii'];`  
`for i=1:numel(p)`  
`    spm_smooth(p{i},tmp,[12 12 12]);`  
`    vf = spm_vol(tmp);`  
`    M  = spm_affreg(vg,vf,struct('regtype','rigid'));`  
`    [u,s,v] = svd(M(1:3,1:3));`  
`    M(1:3,1:3) = u*v';`  
`    N  = nifti(p{i});`  
`    N.mat = M*N.mat;`  
`    create(N);`  
`end`  
`spm_unlink(tmp);`
