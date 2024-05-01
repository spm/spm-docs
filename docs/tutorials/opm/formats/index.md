# Supported File formats

## Introduction
There are a number of different manufacturers and custom OPM systems in operation across the world. As such, there are also a number of different file formats out there. SPM supports the following formats. 

## UCL 
When initially developing our in-house OPM  system there were no file-formats. The data acquisition system was written from scratch and we had to think of how to store the data. We settled on the raw data being stored in a binary file and all the metadata stored in `TSV` and `JSON` files. This was designed to be as aligned as possible to the BIDS file structure and to reinvent as few wheels as possible. All the metadata files are described in the [BIDS standard](https://bids-specification.readthedocs.io/en/stable/introduction.html)

```matlab
S =[];
S.data = 'meg.bin';
S.coordystem='coordsystem.json';
S.channels='channels.tsv';
S.meg='meg.json';
S.positions = 'positions.tsv'
D = spm_opm_create(S);
```

Alternatively, if all the relevant metadata is in the same folder as the binary file and follows the BIDS naming standard you only need to specify the binary file as an argument. 

```matlab
S =[];
S.data = 'meg.bin';
D = spm_opm_create(S);
```

## Cerca Magnetics
Cerca Magnetics offers a very similar format to the UCL format, with a simple binary file to store the data and the metadata stored in text files.

```matlab
S = [];
S.data ='OPM_meg_001.cMEG';
S.positions= 'OPM_HelmConfig.tsv';
D = spm_opm_create(S);
```

## Fieldline 
Fieldline uses the existing  `.fif` format  and can take advantage of existing fileio available in SPM. Additionally, the SPM function `spm_opm_create` can overwrite the sensor positions in the file if you supply it with a `positions.tsv` file. This is optional.  If you do not provide this file the function will use the default sensor positions in the file. 

```matlab
S = [];
S.data = 'raw.fif';
D =spm_opm_create(S);
```

If you wish to overwrite the positions in the file the code below should work.

```matlab
S = [];
S.data = 'raw.fif';
S.positions = 'positions.tsv';
D =spm_opm_create(S);
```

## Neuro-1
The Neuro-1 system developed by QuSpin currently records data in `.lvm` files. just as with the other data files we can provide additional metadata in the form of `.tsv` and .json files 

```matlab
S = [];
S.data ='Array 1.lvm';
S.positions= 'Array 1_positions.tsv';
D = spm_opm_create(S);
```