# Visual inspection of the data

## Checking data structure

Let's take a look at the structure of the data you [downloaded](https://www.fil.ion.ucl.ac.uk/spm/download/data/MoAEpilot/MoAEpilot.bids.zip). The data is organised following [BIDS](https://bids.neuroimaging.io/) - a standardised format for organising and naming neuroimaging files, and looks like this:

``` 
MoAEpilot
├── CHANGES
├── dataset_description.json
├── README
├── sub-01
│   ├── anat
│   │   └── sub-01_T1w.nii
│   └── func
│       ├── sub-01_task-auditory_bold.nii
│       └── sub-01_task-auditory_events.tsv
└── task-auditory_bold.json
```

Inside the folder containing the Mother of All Experiments (MoAE) data, you can find `CHANGES`, `dataset_description.json`, `README` and `task-auditory_bold.json` files - these files contain meta-data about the study. The subject directory, `sub-01`, contains `anat` and `func` folders, which store anatomical and functional images, respectively. Additionally, onset timings for task data are stored in the `func` folder alongside task images. 

## Inspecting anatomical data

An important step of any imaging analysis is to visually check your data for any problems that may be related to scanner issues, poor contrast, incidental findings, etc. 

We will first look at the anatomical image `sub-01_T1w.nii` stored in `sub-01/anat`. 

From your terminal or MATLAB window open SPM by typing:

```matlab
spm fmri
```

or:

```matlab
spm
```

and selecting `fMRI` in the SPM window. 

To view an image, click on `Display` from the SPM menu and select the image you want to inspect from the pop-up window. The selected image will now be shown in the SPM graphics window. 

!!! tip "File extensions"
    SPM can read uncompressed NIfTI files in single (`.nii`) and dual (`.hdr`/`.img`) formats. If your files are compressed with `gzip` and end in `.gz`, make sure to uncompress them first, e.g.:
    ```matlab
    gunzip your_file.nii.gz
    ```

Browse through the image using your mouse or arrow keys. The crosshair will help you know what part of the image you are looking at. You can also right-click your mouse on the image to select the `Movie` feature and SPM will scroll through the image for you in `x`, `y` or `z` plane. 

!!! info "What to look for in anatomical data?"
    A few things that are worth paying particular attention to when reviewing anatomical images:

    * overall quality of the image (good contrast, no ring-like artefacts which can indicate excessive motion or image reconstructions issues),

    * correct image orientation,

    * abnormalities in the tissues (often showing as bright or dark spots where there shouldn't be any).

    Although initially, checking your data visually might seem daunting, you will notice that with practice you will quickly pick up this skill. [mrishark.com](http://www.mrishark.com/brain.html) has a rich repository of MRI data with various [scanner-related artefacts](http://www.mrishark.com/artifact.html) and [pathologies](http://www.mrishark.com/brain2.html) which can be a helpful resource when learning to distinguish between good and poor quality data. 

### Video walk-through 
![type:video](../../../assets/videos/inspecting_anatomical_data.mp4)

## Inspecting functional data

Similarly to how we checked the anatomical data, we will now inspect the functional image. 

Select `Check Reg` from the SPM menu and choose the functional data you want to view - in this case `sub-01_task-auditory_bold.nii` stored in `sub-01/func`. Browse through the image with your mouse or arrow keys just like you did with the anatomical data. 

!!! info "Loading 4D datasets into SPM"
    When loading the functional data into SPM, you may have noticed that the file name was followed by `,1`. This indicates volume number in the time series - in this case volume one. You can change what volumes are being displayed in the SPM selection window by using the `Filter` option in the right bottom corner. 
    
    You can filter by:

    * specific volume, e.g. `1`
    
    * several volumes, e.g. `1, 10, 100`
    
    * range of volumes, e.g. `1:1000`

    * all available volumes or a 4D file, e.g. `Inf` or `NaN`
    

Up to this point you have only loaded the first volume from your functional time series. To load the full time series, right-click on the SPM graphics window and navigate to `Browse`. In the pop-up window type `Inf` in the box below the `Filter` button and hit :material-keyboard-return: on your keyboard. This will prompt SPM to show all 84 volumes present in this directory. Right-click on the list of files and choose `Select all`. Press `Done`. 

You will now see :material-play: appear in the bottom-right corner of the SPM graphics window. Press the button to watch your functional time series as a movie. 

!!! info "What to look for in functional data?"
    Checking functional data is similar to checking anatomical data. You want to pay attention to any unwanted distortions, dark/bright spots which can indicate pathologies, incorrect orientation. Keep in mind that the spatial resolution of functional data is lower than that of anatomical data, so some of those issues may be more difficult to spot. Also, it's worth noting that some distortions may be unavoidable in functional data. In the example dataset, you can see that there is some signal dropout towards the frontal pole and orbitofrontal regions. This is a consequence of the sequence used to acquire the data and cannot be fixed with preprocessing but is perfectly normal for this type of acquisition. 

    Another important part of viewing functional data is to visually check for excessive motion. You can do this by viewing the functional data as a movie, where each volume represents a timepoint in the scan. 
    
!!! tip "Top tip"
    It's a good practice to keep a spreadsheet where you note down your qualitative observations of the data at each preprocessing step. This can be useful down the line when trying to troubleshoot potential analysis problems. 

### Video walk-through
![type:video](../../../assets/videos/inspecting_functional_data.mp4)


--8<-- "addons/abbreviations.md"
