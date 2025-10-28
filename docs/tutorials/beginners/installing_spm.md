# Introducing SPM

SPM (Statistical Parametric Mapping) is a Matlab toolobox designed for the analysis of brain imaging data such as MRI/fMRI, MEG and EEG. It has been developed by the Imaging Neuroscience methods team at UCL since the early 1990s. SPM provides a set of functions that you can access from the Matlab command line or through a graphical user interface (GUI).

# Installing SPM

## Option 1 - If you are using Desktop@UCL Anywhere

SPM is already available in a special network folder set up for our course. We just need to let Matlab know where to find it. To do that, run Matlab, copy the following code into the Matlab command window and press Enter:

```matlab
   addpath('S:\FBS_CLNE0068\SPM');
   fid = fopen('N:\Documents\MATLAB\startup.m','w');
   fprintf(fid, 'addpath(''S:\\FBS_CLNE0068\\SPM'');');
   fclose(fid);
``` 

## Option 2 - If you want to install SPM on your own computer
<br>
<details>
<summary>Click to see the instructions</summary>

1. Download SPM.zip file from the link on the course Moodle page (in `Data analysis resources` section).
2. Double-click the downloaded `SPM.zip` file to navigate into it.
3. Drag the `SPM` folder from inside the zip file to where you want to keep it.
`Documents\MATLAB` would be a good location.

![Copying SPM](./spm_copy.png)

*Figure: Copying the SPM folder to the N: drive*

Once the copying is complete, open Matlab and type the following command in the command window:

```matlab
addpath('C:\Users\yourusername\Documents\MATLAB');
```

This assumes you copied the SPM folder to `C:\Users\yourusername\Documents\MATLAB`. You need to replace `yourusername` with your actual user name. If you put it somewhere else, change the path accordingly.

To make this change permanent, you can save the path by typing:

```matlab
fid = fopen('C:\Users\yourusername\Documents\MATLAB\startup.m','w');
fprintf(fid, 'addpath(''C:\\Users\\yourusername\\Documents\\MATLAB'');');
fclose(fid);
```
</details>

<br>

# Testing the installation

* To check that SPM is installed correctly, restart MATLAB and type the following command in the Matlab command window:

   ```matlab
   spm fmri
   ```
If SPM is installed correctly, this command will open the SPM graphical user interface (GUI) for fMRI analysis.

![SPM GUI](./spm_gui.png)

*Figure: SPM (Statistical Parametric Mapping) software graphical user interface (GUI) for fMRI analysis*

