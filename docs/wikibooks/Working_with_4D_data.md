## Working with four-dimensional data

Recent versions of SPM (SPM5 or later) can handle 4D NIfTI data-sets,
which are often used to represent fMRI time-series of 3D volumes, as
well as more general collections of related image volumes (such as the
different tissue classes in DARTEL Templates).

The key to working with 4D data in the graphical user interface (e.g.
after clicking *Display* or *Check Reg*, or selecting images in the
batch interface) is to realise that the **1** shown in the image
selection dialogue (on the right, just above the list of selected files)
refers to the first **volume or frame** of a 4D data-set. Note that 3D
data can be viewed as a special case of 4D data with a single volume, so
this first (only) frame is appropriate for normal 3D images too.

To select volumes other than the first, you simply need to change the
frame specification. To choose a particular frame you can replace the
number 1 with the desired number, e.g. 10 or 100, but more usefully, you
can enter a MATLAB range, and you will then see any/all volumes up to
the upper limit, so entering **1:999** will let you select all volumes
in a time-series of less than 1000 volumes. The volumes are shown with
their frame number after a comma at the end of the filename.

With fMRI data, it is often very helpful to change the **filter**
expression from \'.\*\' to a regular expression that shows only the set
of images you are interested in selecting (e.g. change it to \'^u\' to
see only the realigned and unwarped images starting with a \'u\') and
then to change the frame range from 1 to \'1:999\' (or some number large
enough to show all of your volumes) and then to right click and choose
\'Select All\'. For more help on the filter (regular expressions), click
the \'?\' on the left of the dialogue.

The reason the frame range doesn\'t just default to something like
1:9999999, is that if you entered a directory with a very large number
of NIfTI files, the interface would be very slow, because all of the
NIfTI headers would need to be read to find out how many volumes each
one actually contained (even if most contained just 1). Again, note that
setting the filter before expanding the frame range can help in this
instance.

### An example

In the example illustrated here, I have used *Check Reg*, navigated to
my SPM directory (using the *Prev* pull-down menu), entered the
*toolbox* directory and then the *Seg* directory, which contains the
tissue probability maps (TPMs) for the New Segmentation Toolbox (see
also *SPM -\> Tools -\> New Segment*). There are six TPM volumes in the
4D *TPM.nii* NIfTI file; by changing the frame number from \'1\' to
\'1:10\', I was able to see all six volumes, and I selected the second
and third frames (shown as *TPM.nii,2* and *TPM.nii,3*), which
correspond to the white-matter and CSF tissue classes.

<figure>
<img src="spm_4D_example.png" title="spm_4D_example.png" />
<figcaption>spm_4D_example.png</figcaption>
</figure>

### Further details

For advanced users, the batch system lets you create batches which
select files according to a pattern, using:

`BasicIO -> File Selector (Batch Mode)`

In this case, entire 4D NIfTI files will be selected (with no \',n\'
frame number(s)). You can then use another batch module:

`SPM -> Util -> Expand image frames`

to reference the individual 4D file and associate a specified list or
range of frames with it. The resultant set of volumes is then available
as a **dependency** for use in later modules of the batch.

Finally, even more advanced users can simply add the frame numbers to
filenames in MATLAB scripts, which even lets one work with
five-dimensional data (!) that is sometimes used for multivariate
data-sets (see e.g. pages 3 and 4 of this [NIfTI
diagram](http://nifti.nimh.nih.gov/nifti-1/documentation/nifti1diagrams_v2.pdf)),
by adding two numbers after commas. For example, to view the three (x, y
and z) components that are stored in the fifth dimension of a 5D DARTEL
velocity field (or just to show off!) you can do:

`>> ims = [repmat('u_rc1Blah_Template.nii', 3, 1) [',1,1';',1,2';',1,3']]`  
`>> spm_check_registration(ims)`

To automatically expand the volumes from a 4D nifti file path, you can
use on SPM12:

`>> ims = spm_select('expand', fullfile(SCAN_dir,'SCAN.nii'))`

For SPM8 and below, you have to manually count the number of volumes and
expand yourself, here is a code snippet:

\<syntaxhighlight lang=\"matlab\"\> function n =
spm_select_get_nbframes(file) % spm_select_get_nbframes(file) Get the
number of volumes of a 4D nifti file, excerpt from SPM12 spm_select.m

`   N   = nifti(file);`  
`   dim = [N.dat.dim 1 1 1 1 1];`  
`   n   = dim(4);`

end

function out = expand_4d_vols(nifti) % expand_4d_vols(nifti) Given a
path to a 4D nifti file, count the number of volumes and generate a list
of all volumes

`   nb_vols = spm_select_get_nbframes(nifti);`  
`   out = strcat(repmat(nifti, nb_vols, 1), ',', num2str([1:nb_vols]'));`

end \</syntaxhighlight\>

You can then use:

`>> ims = expand_4d_vols(fullfile(SCAN_dir,'SCAN.nii'))`

Note that any of the techniques above can be used to select only a
subset of the 4D volumes (eg, to discard first x volumes to avoid MRI
saturation effects):

`>> ims = cellstr(ims(4:end,:));`

## Creating 4D NIfTI files in SPM

SPM lets you concatenate multiple 3D volumes into a 4D NIfTI using the
batch system:

`SPM -> Util -> 3D to 4D File Conversion`

The images must have identical dimensions, otherwise it isn\'t
meaningful to consider them as one 4D data-set.

Note that this interface also lets you create a new 4D file using
volumes selected from within an existing 4D file, which means you can do
things like dropping T1-equilibration scans from the start of one 4D
file by creating a new 4D file containing just the volumes you want to
keep.

## Possible issues with 4D NIfTI files

Intrinsically, 4D NIfTI files are bound to produce memory issues, since
they can grow up to any size possible (depending on the number of
volumes a 4D NIfTI file contains). Most SPM modules fully support large
4D NIfTI files, but third-party toolboxes or your own scripts using SPM
functions might not support these large files.

Even through SPM supports large NIfTI files, it can only load files as
large as your available RAM
memory\<ref\><https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=ind1009&L=SPM&D=0&I=-3&d=No+Match%3BMatch%3BMatches&P=42567%3C/ref%3E>;
and as your filesystem
allows\<ref\><https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=ind1411&L=SPM&P=R63209&I=-3&d=No+Match%3BMatch%3BMatches%3C/ref%3E>;
(don\'t forget the output NIfTI file from a processing step might be a
lot larger than the input file!). Techniques like out-of-core computing
could be used to store and split large computations on on-disk files,
but this is very experimental and not available in most neuroscientific
toolboxes.

In practice, if you are using SPM, using too large 4D NIfTI file can
produce the \"Can\'t map view\" error (but this is not the only possible
cause: reading files from a network SAMBA share, permission issues and
antiviruses can also produce it). In this case, you might try to convert
your 4D NIfTI file to a set of 3D NIfTI files, separating each frame
into a separate NIfTI file, to see if this fixes your issue.

## Separating 4D NIfTI files into 3D volumes

This page should have demonstrated that generally you do **not** need to
split 4D data-sets into their constituent 3D volumes for use in SPM,
whether using the interactive interface, batching or scripting, the
exception being if you have too large 4D nifti files as explained above.

In these cases, or for some specific reasons pertaining to your design,
you might want to split data-sets to work on the images in other
software packages that do not handle 4D NIfTIs. To do this, SPM provides
a simple command-line tool (not in the batch interface, sorry), that you
can use by typing:

`>> spm_file_split`

and then selecting (the first frame of) a 4D NIfTI. If you select
*Blah.nii*, this will then create images of the form *Blah_00001.nii,
Blah_00002.nii, etc*. See *help spm_file_split* for more flexible usage.

A [recursive 4D to 3D niftis
converter](https://github.com/lrq3000/csg_mri_pipelines/blob/64622fb4289e485a6b2266dbbed08a3f5f0030b2/analysis/fmri/nifti_4dto3d_convert_recursive/nifti_4dto3d_convert_recursive.m)
using spm_file_split can be found here.
