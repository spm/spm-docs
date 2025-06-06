# Slice Timing  
Correct differences in image acquisition time between slices.   
Slice-time corrected files are prepended with an ``a``.   
Note: The sliceorder arg that specifies slice acquisition order is a vector of N numbers, where N is the number of slices per volume. Each number refers to the position of a slice within the image file. The order of numbers within the vector is the temporal order in which those slices were acquired. To check the order of slices within an image file, use the SPM Display option and move the cross-hairs to a voxel coordinate of z=1.  This corresponds to a point in the first slice of the volume.   
The function corrects differences in slice acquisition times. This routine is intended to correct for the staggered order of slice acquisition that is used during echo-planar scanning. The correction is necessary to make the data on each slice correspond to the same point in time. Without correction, the data on one slice will represent a point in time as far removed as 1/2 the TR from an adjacent slice (in the case of an interleaved sequence).   
This routine "shifts" a signal in time to provide an output vector that represents the same (continuous) signal sampled starting either later or earlier. This is accomplished by a simple shift of the phase of the sines that make up the signal. Recall that a Fourier transform allows for a representation of any signal as the linear combination of sinusoids of different frequencies and phases. Effectively, we will add a constant to the phase of every frequency, shifting the data in time.   
Shifter - This is the filter by which the signal will be convolved to introduce the phase shift. It is constructed explicitly in the Fourier domain. In the time domain, it may be described as an impulse (delta function) that has been shifted in time the amount described by TimeShift. The correction works by lagging (shifting forward) the time-series data on each slice using sinc-interpolation. This results in each time series having the values that would have been obtained had the slice been acquired at the same time as the reference slice. To make this clear, consider a neural event (and ensuing hemodynamic response) that occurs simultaneously on two adjacent slices. Values from slice "A" are acquired starting at time zero, simultaneous to the neural event, while values from slice "B" are acquired one second later. Without correction, the "B" values will describe a hemodynamic response that will appear to have began one second EARLIER on the "B" slice than on slice "A". To correct for this, the "B" values need to be shifted towards the Right, i.e., towards the last value.   
This correction assumes that the data are band-limited (i.e. there is no meaningful information present in the data at a frequency higher than that of the Nyquist). This assumption is support by the study of Josephs et al (1997, Human Brain Mapping)  that obtained event-related data at an effective TR of 166 msecs. No physio-logical signal change was present at frequencies higher than our typical Nyquist (0.25 HZ).   
When using the slice timing correction it is very important that you input the correct slice order, and if there is any uncertainty then users are encouraged to work with their physicist to determine the actual slice acquisition order.   
One can also consider augmenting the model by including the temporal derivative in the informed basis set instead of slice timing, which can account for +/- 1 second of changes in timing.   
Written by Darren Gitelman at Northwestern U., 1998.  Based (in large part) on ACQCORRECT.PRO from Geoff Aguirre and Eric Zarahn at U. Penn.   

* **Data** (create a list of items)  
Subjects or sessions. The same parameters specified below will be applied to all sessions.   

    * **Session** (select files)  
    Select images to slice-time correct.   

* **Number of Slices** (enter text)  
Enter the number of slices.   

* **TR** (enter text)  
Enter the TR (in seconds).   

* **TA** (enter text)  
Enter the TA (in seconds). It is usually calculated as TR-(TR/nslices).   
You can simply enter this equation with the variables replaced by appropriate numbers.   
If the next two items are entered in milliseconds, this entry will not be used and can be set to 0.   

* **Slice order** (enter text)  
Enter the slice order.   
Bottom slice = 1. Sequence types and examples of code to enter are given below:   
    - ascending (first slice=bottom): ``[1:1:nslices]``   
    - descending (first slice=top): ``[nslices:-1:1]``   
    - interleaved (middle-top):   
    ```   
    for k = 1:nslices   
       round((nslices-k)/2 + (rem((nslices-k),2) * (nslices - 1)/2)) + 1,   
    end   
    ```   
    - interleaved (bottom -> up): ``[1:2:nslices 2:2:nslices]``   
    - interleaved (top -> down): ``[nslices:-2:1, nslices-1:-2:1]``   
Alternatively you can enter the slice timing in ms for each slice individually.If doing so, the next item (Reference Slice) will contain a reference time (in ms) instead of the slice index of the reference slice.For Siemens scanners, this can be acquired in MATLAB from the dicom header as follows (use any volume after the first one):   
   ``hdr = spm_dicom_headers('dicom.ima');``   
   ``slice_times = hdr{1}.Private_0019_1029``   
Note that slice ordering is assumed to be from foot to head. If it is not, enter instead: TR - INTRASCAN_TIME - SLICE_TIMING_VECTOR   

* **Reference Slice** (enter text)  
Enter the reference slice.   
.   
If slice times are provided instead of slice indices in the previous item, this value should represent a reference time (in ms) instead of the slice index of the reference slice.   

* **Filename Prefix** (enter text)  
Specify the string to be prepended to the filenames of the slice-time corrected image file(s).   
Default prefix is 'a'.   
