## The Problem

With an EPI sequence, a 3D volume is usually not acquired at once but
rather in a sequence of 2D slices, obtained at different times (within
one time of repetition (TR)). In order to fix this issue, the voxels
activations of every slices of a volume can be interpolated to the same
timepoint (the reference slice) if we know when each slice was acquired:
this is the role of slice timing correction\<ref
name=\"henson-1999\"\>\</ref\>. This is why one must ensure to know the
precise TR and slice order acquisition.

## Slice Timing Correction

See *SPM \> Temporal \> Slice Timing*.

### General

- Slice timing correction has been shown to reliably increase
  sensitivity and effect power without any adverse effect\<ref
  name=\"sladky-2011\"\>\</ref\>.
- There is some debate about whether one should use slice timing
  correction first or motion correction (realignment) first. Indeed,
  unbiasing one first will ensure maximum precision for this step but
  will add some additional bias for the subsequent correction step, as
  errors accumulate. Usually, it is advised to use slice timing
  correction first if you use either a complex slice order sequence
  (i.e., anything that is not sequential such as interleaved, central,
  etc.) or with a sequential slice order if significant head movement is
  expected, or motion correction (realignment) first if you use a
  sequential slice order and you expect only slight head movement\<ref
  name=\"rik-email\" /\>. See also
  [Neuroimaging_Data_Processing/Slice_Timing](Neuroimaging_Data_Processing/Slice_Timing "wikilink").
  There are some methods which were devised to perform joint
  optimization of both objectives in 4D simultaneously, such as there is
  no error accumulation, as implemented in
  [nipy.SpaceTimeRealigner()](http://nipype.readthedocs.io/en/latest/interfaces/generated/interfaces.nipy/preprocess.html#spacetimerealigner)
  of the Python library nipy\<ref name=\"roche-2011\"\>\</ref\>.
- For multiband acquisitions, it is necessary to use slice timings
  instead of slice order. Also, with multiband and a small TR, the slice
  timing correction can usually be skipped without much impact, but
  using the correction will always be beneficial. See for more
  information the subsection about multiband EPI acquisitions.
- If you plan to use Dynamic Causal Modeling (DCM), it is mandatory to
  use slice timing correction.
- As an alternative, if the TR \< 2s, it is possible to skip slice
  timing correction and instead use a temporal derivative, which can
  account for +/- 1 second of changes in timing\<ref
  name=\"spm12-manual\"\>\</ref\>, however it was shown that slice
  timing correction is always beneficial, even for TR \< 2s\<ref
  name=\"sladky-2011\"\>\</ref\>, and that slice timing correction
  increases sensitivity and power compared to temporal derivatives\<ref
  name=\"sladky-2011\" /\>. In addition, using both slice timing
  correction and temporal derivatives only decreases power compared to
  using slice timing correction alone\<ref name=\"sladky-2011\" /\>.

### Time of Repetition (TR)

The TR can always be accessed in the required DICOM field
(0018,0080)\<ref
name=\"dicom-field-tr\"\><http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)%3C/ref%3E>;
or usually also in the NIFTI files, with\<ref
name=\"crnl-autodetect-script2\"/\> or without\<ref
name=\"crnl-autodetect-script1\"/\> a BIDS sidecar.

### Slice Order

#### General

- SPM makes two assumptions about the convention to specify slice order:
  1- the temporal order of slices would be coded by the left-to-right
  order of slices in a vector ; 2- The 1st slice is the most bottom
  slice, following the Analyze convention\<ref
  name=rik-email5\>\</ref\>.
- There are currently a few known ways to access the slice order
  information:

<figure markdown>
  <div class="center">
  <img src="../../../assets/figures/wikibooks/EP2D_BOLD_resting-state_Siemens_scanner_sequence_printout.png" style="width:100mm" />
  </div>
  <figcaption>Excerpt page from an EP2D BOLD resting-state Siemens scanner sequence printout. Note the "Series" parameter, which states that the slice order is ascending sequential (and not interleaved).</figcaption>
</figure>

    The most secure way is to access the MRI scanner console, which will
    display the exact parameters used for the EPI BOLD sequence
    acquisition. It is possible to save a **\"printout\"** of the
    sequence\'s parameters, which is a PDF file summarizing the scanner
    parameters. In general, having a copy of the sequence printout is
    always a good idea to check the parameters. These printout are PDF
    files that can be generated from the scanner software interface.
  - From the DICOM files for some recent scanners (see Siemens below).
  - If you created a BIDS sidecar when converting your DICOM images to
    NIfTI (e.g. you used dcm2niix or dicm2nii to convert your images),
    the slice order can be inferred by the \"SliceTiming\" tag in the
    JSON-format BIDS file. The \"SliceTiming\" tag lists the time each
    slice was acquired in seconds\<ref
    name=\"crnl-autodetect-script2\"\>\</ref\>.

<figure markdown>
  <div class="center">
  <img src="../../../assets/figures/wikibooks/Mricron-slice-order-nifti.png" style="width:100mm" />
  </div>
  <figcaption>Some NIFTI viewers (here Mricron) can display the slice order if the information is available.</figcaption>
</figure>

    Even without a BIDS sidecar, some DICOM to NIFTI converters (e.g.
    dcm2niix or MRIconvert) can keep the slice order (and TR)
    information inside the generated nifti files, and some nifti
    viewers (e.g. mricron) can display it or you can then access this
    information programmatically like this\<ref
    name=\"crnl-autodetect-script1\"\>\</ref\>:

`fMRIname = 'path_to_nifti_file.nii';`  
`slice_order = 0; % Set 0 to autodetect`  
  
`%these are the possible slice_orders `[`http://nifti.nimh.nih.gov/pub/dist/src/niftilib/nifti1.h`](http://nifti.nimh.nih.gov/pub/dist/src/niftilib/nifti1.h)  
`kNIFTI_SLICE_UNKNOWN =  0; %AUTO DETECT`  
`kNIFTI_SLICE_SEQ_INC = 1; %1,2,3,4`  
`kNIFTI_SLICE_SEQ_DEC = 2; %4,3,2,1`  
`kNIFTI_SLICE_ALT_INC = 3; %1,3,2,4 Siemens: interleaved with odd number of slices, interleaved for other vendors`  
`kNIFTI_SLICE_ALT_DEC = 4; %4,2,3,1 descending interleaved`  
`kNIFTI_SLICE_ALT_INC2 = 5; %2,4,1,3 Siemens interleaved with even number of slices `  
`kNIFTI_SLICE_ALT_DEC2 = 6; %3,1,4,2 Siemens interleaved descending with even number of slices`  
  
`[pth,nam,ext,vol] = spm_fileparts( deblank(fMRIname(1,:)));`  
`fMRIname1 = fullfile(pth,[ nam, ext]); %'img.nii,1' -> 'img.nii'`  
`if slice_order == 0 %attempt to autodetect slice order`  
`    fid = fopen(fMRIname1);`  
`    fseek(fid,122,'bof');`  
`    slice_order = fread(fid,1,'uint8')`  
`    fclose(fid);`  
`    if (slice_order > kNIFTI_SLICE_UNKNOWN) && (slice_order <= kNIFTI_SLICE_ALT_DEC2)`  
`        fprintf('Auto-detected slice order as %d\n',slice_order);`  
`    else`  
`        fprintf('%s error: unable to auto-detect slice order. Please manually specify slice order or use recent versions of dcm2nii.\n');`  
`        return;`  
`    end;`  
`end`

Here are a few other notes one should pay attention to:

- The scanner head position for the first slice relatively to the
  patient can be found in the DICOM fields Image Position Patient
  (0020, 0032) together with Image Orientation Patient (0020, 0037) and
  Patient Position (0018, 5100). The latter is a required DICOM field
  and is necessary for software to interpret the orientation of the
  images, so you can reliably use this field. See DICOM reference
  C.7.3.1.1.2 for more information on the meaning of the different
  values\<ref\><http://dicom.nema.org/medical/dicom/2016b/output/chtml/part03/sect_C.7.3.html#sect_C.7.3.1.1.2%3C/ref%3E>;.
- SPM expects the first (spatial) slice to be the bottom slice
  (orientation acquisition Transversal and from Inferior to Superior).
  In other words, the slice 1 as in your slice order must represent the
  bottom-most slice of the brain. If not, you need to change the
  reference slice and use as slice order \<tt\>TR - INTRASCAN_TIME -
  SLICE_TIMING_VECTOR\</tt\>\<ref name=\"spm_slice_timing.m\" /\>.
- If possible, prefer to use the slice timing instead of the slice
  order, as some scanners will round off some of the slice timing in
  order to more simplify and more reliably acquire these slices over all
  volumes (because it is nearly impossible for a machine to acquire at a
  time that is defined with a very long floating point number, if it is
  too precise the mechanical parts might not follow, hence the rounding,
  which logic differs depending on the brand and machine). Using the
  slice order will always assume a perfectly spaced acquisition, whereas
  using the slice timing, as found in the DICOMs, BIDS or Scanner
  console log, will account for these specific roundings.

#### Reference slice

When you do slice timing correction, all slices of one volume are
interpolated in time to one **slice of reference**. This reference slice
becomes the most accurate slice since it gets no interpolation, only
other slices are interpolated.

In SPM, you can change this parameter in Reference Slice (refslice). The
slice of reference is 1 by default in SPM, which corresponds to a slice
order sequential ascending or interleaved ascending. If your slice order
is different, then you need to change this value. In other words, if the
first slice in the slice order is not 1, you need to change the
reference slice.

If you want to choose the first acquired slice as a reference slice,
then you need to use the slice **spatial** number as reference
slice\<ref name=\"spm_slice_timing.m\" /\>. For example, with a slice
order sequential descending 4 3 2 1, to set the first temporal slice as
the reference slice, you need to set the reference slice to 4. If
instead of slice order you use slice timing in seconds (e.g., 2.0 1.0
0.0 1.5 0.5), then you need to also specify the reference slice in
seconds (e.g., to set the first slice as the reference, use \'0\').

However, you can also choose to use another reference slice as you wish.
Although setting the reference slice to the first is the easiest and the
most common, another common reference is to use the middle slice for
more precision with sequential slice orders. Indeed, using the middle
slice theoretically guarantees that we minimize the amount of temporal
interpolation error, because then the maximum interpolation will be of
TR/2 (in negative and positive shifts). Also, since we assume we are in
a sequential slice order, the middle slice in time is also the middle
slice in space, thus we also minimize the spatial interpolation error
and push the accumulating errors to the top and bottom slices, where
there is usually less tissues of interest.

There is however some debate about the benefits of using any reference
slice other than the first\<ref name=\"nctu-refslice\"\>\</ref\>:

It is important to note that whatever reference slice you use, you
should check that the Microtime Onset (fMRI_T0) in the statistical
analysis module corresponds to your Reference Slice (refslice) of the
slice timing module to ensure that your onsets during the statistical
test are shifted appropriately (else they might be too early or too
late!)\<ref name=\"rik-email\"\>\</ref\>{{,}}\<ref
name=\"rik-email2\"\>\</ref\>{{,}}\<ref
name=\"rik-email3\"\>\</ref\>{{,}}\<ref name=\"rik-email4\"\>\</ref\>.
Note that by default, the microtime onset is set to 8 over 16 in SPM12,
which corresponds to a middle reference slice, thus if you use the first
temporal slice as the reference slice, you should change the microtime
onset to 1.

In any case, one must ensure that the slice timing correction reference
slice is the same as the statistical test microtime onset. To make it
easier, you can change the microtime resolution to the number of slices
there are in each EPI volume, so that the number of bins in the
statistical test will be the same as the number of slices used for slice
timing correction, hence the microtime onset will correspond to the
slice position in time\<ref name=\"gablab-refslice1\"\>\</ref\>

Note that the microtime onset is to be set in the temporal convention
(the number is the slice position in time) and scaled to the microtime
resolution (you can also set microtime resolution to the number of
slices of your EPI volumes, which will make things easier), whereas the
Reference Slice is in the spatial convention (the number is the slice
position in space).

#### Siemens scanners

The following applies to presumably all Siemens scanners using the Syngo
system, but might or might not apply to other scanners. Please note that
most of the points below are only correct if the \"Orientation\" scanner
parameter is \"Transversal\" \<ref name=\"siemens-sliceorder\"/\>. Also
please note that we use here the 1-based indexing convention (i.e.,
slices start at 1 then 2, 3, \...).

- Slice times (in milliseconds) can sometimes be read directly from the
  header of DICOM files (from any volume of an EPI BOLD sequence after
  the first one) using the private vendor field \<tt\>MosaicRefAcqTimes
  (0019, 1029)\</tt\> below the field \<tt\>(0019, 0010) SIEMENS MR
  HEADER\</tt\>:

`hdr = spm_dicom_headers('dicom.ima');`  
`slice_times = hdr{1}.Private_0019_1029`

If the slice_times returns a vector of integers, then you need to
convert these 8-bit symbols into their double representation:

`slice_times = typecast(uint8(slice_times), 'double')`

This will give you the relative time of acquisition of each slice, which
you can combine with (0008, 0033) Acquisition Time to compute the
absolute time of each slice. Note that \<tt\>(0019, 1029)\</tt\> is a
private field, so it might not always be present on Siemens machines
(also your sequence definition might change this). If you want the slice
order offsets instead of timing:

`[~, slice_order] = sort(slice_times);`

Note that with the slice timing, you don\'t need the slice order, as you
can provide the slice timing in milliseconds directly to SPM instead of
the slice order.

- The slice order is defined by the parameter **\"Series\", and not
  \"Multi-slice mode\"**\<ref name=\"siemens-sliceorder\"\>\</ref\>
  (although both present the option \"interleaved\", only the option in
  \"Series\" is related to the slice order, the \"Multi-slice mode\"
  being interleaved for an EPI sequence just means that the acquisition
  is simultaneous and is equivalent to single-shot\<ref
  name=\"siemens-sliceorder\" /\>). There are usually three options:
  ascending, interleaved (ascending) and descending. Interleaved is
  particular on Siemens machines as this mode always acquire in
  ascending fashion, but the starting slice will change depending on the
  number of slices\<ref name=\"siemens-sliceorder\" /\>: if the total
  number of slices is even, the slice order will be even-first,
  otherwise with an odd number of slices the slice order will be
  odd-first. Note that this odd-first vs even-first interleave behavior
  that is specific to Siemens machines is only true if using a standard
  sequence definition, as a user-defined sequence (such as CMRR\'s\<ref
  name=\"cmrr-epi-sequence\"\>\</ref\>) can change this to always be
  odd-first as with other MRI machines\<ref name=\"crnl1\"\>\</ref\>.
  See the table below for a summary of the possible slice orders for
  Siemens machine and the sample codes to supply to SPM. If you are
  wondering which of interleaved or sequential/contiguous acquisition is
  better, a brief answer is that interleaved is better to reduce slice
  cross-talk but slower at recovering from T1 signal variations due to
  motion noise than sequential acquisition\<ref\>[\"Common intermittent
  EPI artifacts: Subject movement\", practiCal fMRI blog, May 22,
  2012](https://practicalfmri.blogspot.com/2012/05/common-intermittent-epi-artifacts.html)\</ref\>.
- Siemens advises the scanner parameter \<tt\>Transversal\</tt\> to
  always be set to \<tt\>F \>\> H\</tt\>, which means \"from foot to
  head\", in order to simplify viewing images. The
  \<tt\>Transversal\</tt\> parameter should be described in the protocol
  printout that the machine can output. Else if you have \<tt\>H \>\>
  F\</tt\>, the mosaic display (ie, how the slices numbering will be
  stored) will be reversed\<ref name=\"siemens-sliceorder\" /\>.
  Although the official documentation specifies that the slice order is
  reversed\<ref name=\"siemens-sliceorder\" /\>, other studies found in
  practice that only the mosaic display is affected, and not the slice
  order acquisition nor storage, thus this parameter does not affect the
  slice order\<ref name=\"ugent-sliceorder\"\>\</ref\>{{,}}\<ref
  name=\"ugent-sliceorder-supplementary1\"\>\</ref\>{{,}}\<ref
  name=\"ugent-sliceorder-supplementary2\"\>\</ref\>{{,}}\<ref
  name=\"crnl1\" /\>, but you should be aware that the mosaic display
  might be confusing and not reflect the slice order. Note also that
  since \<tr\>H \>\> F\</tr\> always reverses the mosaic display, an
  interleave will then have a mosaic display (but not slice order) that
  will always be odd-first, whether the number of slices is odd or
  even\<ref name=\"siemens-sliceorder\" /\>{{,}}\<ref
  name=\"ugent-sliceorder-supplementary2\" /\>. However, on some newer
  machines, such as the Magnetom, the slice timing is indeed affected,
  thus not only the mosaic display is affected but also it seems the
  slice timing as can be found in the related DICOM field (see above),
  as is stated in the Siemens documentation. In any case, if \<tt\>H
  \>\> F\</tt\>, one should check the slice timing from the DICOM files
  to ensure if the slice timing got reversed or not. See the table below
  for a summary.
- Ascending vs descending meaning: to know what exactly ascending and
  descending refer to, please refer to your printout: it will detail for
  each dimension what is the ascending direction. By default on Siemens
  machines, \<tt\>Transversal\</tt\> should be \<tt\>F \>\> H\</tt\>
  which is foot to head, \<tt\>Sagittal\</tt\> should be \<tt\>R \>\>
  L\</tt\> which is right to left, \<tt\>Coronal\</tt\> should be
  \<tt\>A \>\> P\</tt\> which is anterior to posterior\<ref
  name=\"crnl1\" /\>. Finally, the main slice acquisition dimension, and
  the one that must be accounted for slice timing correction, is given
  by the \<tt\>Orientation\</tt\> parameter, which can be either
  \<tt\>Transversal\</tt\>, \<tt\>Sagittal\</tt\> or
  \<tt\>Coronal\</tt\>\<ref name=\"siemens-sliceorder\" /\>. Thus, if
  your main orientation is \<tt\>Sagittal\</tt\> with direction \<tt\>R
  \>\> L\</tt\>, it means that the slices will be acquired right-first
  and left-last. However, according to practical observations, the
  direction set in the scanner parameters appear to not matter much
  except for mosaic display, as the slices are still acquired in the
  default direction\<ref name=\"ugent-sliceorder\" /\>{{,}}\<ref
  name=\"ugent-sliceorder-supplementary2\" /\>. SPM expects the main
  acquisition dimension to be Transversal, so slices are acquired from
  foot to head\<ref name=\"spm_slice_timing.m\"\>SPM12
  spm_slice_timing.m comments\</ref\>.
- Here is a table summarizing the various slice orders that are possible
  with Siemens machines, with sample code to supply to SPM for correct
  slice timing correction\<ref name=\"ugent-sliceorder\" /\>{{,}}\<ref
  name=\"ugent-sliceorder-supplementary2\" /\>:

| Alias                                                       | Nifti slice order type | Acquisition mode (Series) | Transversal direction | Number of slices | Slice order                        | Mosaic display order | SPM code                                 |
|-------------------------------------------------------------|------------------------|---------------------------|-----------------------|------------------|------------------------------------|----------------------|------------------------------------------|
| Ascending sequential                                        | 1                      | Ascending                 | F \>\> H              | even or odd      | 1 2 3 4                            | 1 2 3 4              | \[1:1:n\]                                |
| Ascending sequential reversed                               | 2                      | Ascending                 | H \>\> F              | even or odd      | 4 3 2 1 on Magnetom or 1 2 3 4     | 4 3 2 1              | \[n:-1:1\] or \[1:1:n\]                  |
| Descending sequential                                       | 2                      | Descending                | F \>\> H              | even or odd      | 4 3 2 1                            | 4 3 2 1              | \[n:-1:1\]                               |
| Descending sequential reversed                              | 1                      | Descending                | H \>\> F              | even or odd      | 1 2 3 4 on Magnetom or 4 3 2 1     | 1 2 3 4              | \[1:1:n\] or \[n:-1:1\]                  |
| Ascending interleaved 1 (odd-first)                         | 3                      | Interleaved               | F \>\> H              | odd              | 1 3 5 2 4                          | 1 3 5 2 4            | \[1:2:n 2:2:n-1\]                        |
| Ascending interleaved 1 reversed (Descending interleaved 1) | 6?                     | Interleaved               | H \>\> F              | odd              | 5 3 1 4 2 on Magnetom or 1 3 5 2 4 | 5 3 1 4 2            | \[n:-2:1 n-1:-2:2\] or \[1:2:n 2:2:n-1\] |
| Ascending interleaved 2 (even-first)                        | 5                      | Interleaved               | F \>\> H              | even             | 2 4 1 3                            | 2 4 1 3              | \[2:2:n 1:2:n-1\]                        |
| Ascending interleaved 2 reversed (Descending interleaved 2) | 6                      | Interleaved               | H \>\> F              | even             | 3 1 4 2 on Magnetom or 2 4 1 3     | 3 1 4 2              | \[n-1:-2:1 n:-2:2\] or \[2:2:n 1:2:n-1\] |

Note that the SPM code is affected by the acquisition mode, the number
of slices and potentially the transversal direction (depending on the
machine model, this can impact only the mosaic display order or also the
slice timing).

- Depending on the TR, Siemens machines will acquire additional \"dummy
  scans\", which are volumes that are discarded prior to beginning the
  real sequence acquisition in order to reduce magnetic saturation. The
  number of dummy scans is chosen to guarantee at least 3 seconds of
  stabilization: Dummy scans = ROUNDUP(3001/TR), and is enabled
  depending on the TR: if there is only one volume in the sequence,
  there is no dummy scan; if 1501 \< TR \<= 1001ms: 3 dummy scans; 3001
  \< TR \<= 1501ms: 2 dummy scans; else if TR \> = 3001ms: 1 dummy
  scan\<ref name=\"bcan-dummyscans\"\>\</ref\>.
- The Siemens machines can be subdivided into two groups depending on
  the underlying system: Numaris 3/3.5 and Syngo. Whereas the slice
  order also changes the visualization of slices for Numaris-based
  machines (hence you can easily deduce the slice order by just looking
  at the slices with any DICOM viewer), the Syngo-based machines
  differentiate the slices storage with the slice acquisition order,
  what is termed \"mosaic\". Indeed, in a mosaic, the slice storage
  order is dependent on the slice spatial number, instead of the slice
  temporal acquisition number. Numaris machines are using SUN OS as the
  operating system, whereas Syngo machines use Windows. Numaris machines
  include Open, Impact, Vision and older Harmony, Symphony scanners ;
  Syngo machines include Concerto, Harmony, Symphony, Trio, Allegra,
  Magnetom\<ref name=\"jisc-scanners-os\"\>\</ref\>.
- Additional technical documents and tutorials from Siemens can be found
  here:
  <https://www.healthcare.siemens.com/magnetic-resonance-imaging/magnetom-world/clinical-corner/application-tips>

#### Philips scanners

Philips scanners also have specific slice order modes, see the table
below\<ref name=\"dbic-philips-sliceorder\"\>\</ref\>:

| Mode            | Number of packages   | Slice order                                                                                                     |
|-----------------|----------------------|-----------------------------------------------------------------------------------------------------------------|
| Default         | Single package       | 1 3 2 4                                                                                                         |
| Default         | Two packages         | 1st package: 1 5 3 7 and 2nd package: 2 6 4 8                                                                   |
| Default         | Multi-packages (\>2) | Slices are first distributed over packages, then the scanner proceeds with first off, then even slices.         |
| Ascending       | Single package       | 1 2 3 4 from anterior to posterior, from left to right, and from foot to head                                   |
| Ascending       | Multi-packages       | 1 3 2 4                                                                                                         |
| Descending       | Single package       | 4 3 2 1 from posterior to anterior, from right to left, and from head to foot                                   |
| Descending       | Multi-packages       | 4 2 3 1                                                                                                         |
| Central         | Single package?      | 3 4 2 5 1 or 3 2 4 1 5, acquire the middle slice first, and then outwards in a ping pong type order             |
| Reverse Central | Single package?      | 1 5 2 4 3, acquire the outer slices first and then ping pongs towards the middle slice last                     |
| Interleaved     | Single package?      | 1 4 7 10 2 5 8 3 6 9, maximizes the time spacing between neighbor slices acquisition, which minimizes spillover |
|                 |                      |                                                                                                                 |

#### Multiband EPI acquisitions

Multi-band acquisition, also known as POMP (GE), Simultaneous excitation
or SMS for simultaneous multi-slice (Siemens), Multi-slice (Philips),
Dual-slice (Hitachi), and QuadScan (Toshiba), is a technique that allows
the acquisition of a few slices simultaneously\<ref
name=\"mriquestions-multiband\"\>\</ref\>. In practice, if you look at
the slice timing, you will see that multiple slices have the same slice
timing, which you will never see with the slice timing of a
non-simultaneous EPI acquisition.

It is very difficult to check if you have multi-band acquisition enabled
on your sequence, as the printout may not reveal this information. One
reliable way is to check the slice timing in the scanner console, or
also in the DICOM if the private vendor field is available (see
above)\<ref name=\"cbs-multiband\"\>\</ref\>{{,}}\<ref
name=\"jisc-multiband1\"\>\</ref\>: if two values are the same in the
slice timing, then your acquisition is multi-band. For newer machines
such as Siemens Magnetom VIDA, the DICOM can include the private fields
(0018,9077) Parallel Acquisition and (0018,9078) Parallel Acquisition
Technique to describe the use of multiband. Note that these two tags
will only report if multiband is enabled (\"(0018,9077) CS \[YES\]\" and
\"(0018,9078) CS \[SMS\]\") not the acceleration factor. Alternatively,
the field (0021,1009) will report the in-plane (e.g. iPAT, SENSE,
GRAPPA) and between slice (e.g. SMS) acceleration (for example
\"(0021,1009) LO \[p2 s4\]\" would suggest multiband 4).

If you use multi-band acquisition, you cannot use the slice order as an
input to slice timing correction, since a slice order cannot represent
multiple slices acquired at the same time (if it was a matrix it would
be possible, but SPM only accepts a vector). However, you can use the
slice timing instead of slice order when using a multi-band EPI
acquisition\<ref name=\"jisc-multiband2\"\>\</ref\>{{,}}\<ref
name=\"spm12-releasenotes\"\>\</ref\>. If you do know your slice order
but not your slice timing, you can artificially create a slice timing
manually, by generating artificial values from the slice order with
equal temporal spacing, and then scale the numbers on the TR, so that
the last temporal slices timings = \<tt\>TR -
TR/(nslices/multiband_channels)\</tt\>.

#### Is slice timing correction necessary? (for small TR, multiband, etc)

With very small TRs such as multiband, it becomes very complicated to
slice time correct because there are usually a high number of slices and
any slight movement might make slice time correction to correct the
wrong slice, and this correction is less necessary as the TR is small
and the temporal difference between slices is thus reduced. In such
cases, you can choose to skip slice timing correction (or use a temporal
derivative, which can account for +/- 1 second of changes in timing\<ref
name=\"spm12-manual\" /\>).

However, it was shown that slice timing correction is always beneficial,
even for fast TR \< 2s such as obtained with multiband\<ref
name=\"sladky-2011\" /\>. Indeed, it was observed that slice timing
correction helps more with the slices that are more delayed (ie, towards
the end of the slice order) and that slice timing correction increases
the statistical effect size the longer the TR, with diminishing returns
as the TR is shorter, down to 0.5s where the common methods of slice
timing correction do not help anymore\<ref
name=\"parker-2017\"\>\</ref\>. However, with new slice timing
correction methods such as upsampling and lowpass filtering, there is
still a significant gain, that is constant whatever the TR\<ref
name=\"parker-2017\" /\>.

## Timing Parameters

### Repetition Time

The time of repetition can be retrieved from DICOM files in the
mandatory field \<tr\>(0018,0080) Repetition Time\</tr\>, which will
give you the value of the TR setting set in the scanner. It can also be
found in NIFTI files headers except if an anomyzation process stripped
it.\<br\> \<br\> However, you might find that the TR value might be a
floating value different from the TR set in the scanner settings. It
seems some scanners such as the Philips Healthcare Ingenia include in
the Repetition Time field the \"real\" time of repetition (the time it
really took to acquire one volume of this sequence). You will hence get
a floating point value, which might be a bit off from the TR you set in
the settings (eg, if TR is 2 then you can have a \"real\" TR of
2.00392).\<br\> \<br\> It would be interesting to be able to use this
precise TR timing per sequence to more precisely correct the TR at the
subject level, unfortunately most (all?) software do not currently
support this feature.

### Microtime

At the statistical analysis step, after the \"fMRI model specification\"
module, thus after preprocessing and slice timing correction, SPM does
not work anymore in the original series domain but in its own: the
(hopefully) slice time corrected volumes are split in \"microtime
bins\". This microtime design is made to super sample the timeseries, in
other word to increase the resolution of the BOLD signal, so that more
precise data points can be sampled at a much greater time resolution
than the original timeseries.

This design is configured by two parameters in the \"fMRI model
specification\" module: **microtime resolution** (fmri_T) and
**microtime onset** (fmri_T0). The first sets the total number of bins
per volume (in other words the \"magnification\" factor, e.g., if you
set 16 you will get 16 points per TR and voxel instead of one), whereas
the second is used to shift the microtime design to correspond to the
slice timing correction parameters (i.e., if you set the reference slice
in the slice timing correction module, you need to change the microtime
onset in the statistical test).

## HRF Time Derivative

The HRF Time Derivative can be enabled during the fmri model
specification to allow for tolerance of small shifts in time (\< 1s),
and can thus be used as an alternative to slice timing correction (even
though slice timing correction increase sensitivisy more than HRF Time
Derivative\<ref name=\"sladky-2011\" /\>, the latter is simpler and more
generic to use).

See *HRF \'Informed\' Basis Set* for more information.

## Resources

- <http://imaging.mrc-cbu.cam.ac.uk/imaging/SliceTiming>
- <http://mindhive.mit.edu/node/109>
- <http://www.brainvoyager.com/bvqx/doc/UsersGuide/Preprocessing/SliceScanTimeCorrection.html>
- Poldrack, Russell A., Jeanette A. Mumford, and Thomas E. Nichols.
  Handbook of functional MRI data analysis. Cambridge University Press,
  2011.
- [Helper scripts for slice timing auto detection and microtime onset
  calculation, in
  MATLAB](https://github.com/lrq3000/neuro_experiments_tools/tree/master/matlab/slice_order).

## References