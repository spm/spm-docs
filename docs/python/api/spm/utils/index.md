Module spm.utils
================

Sub-modules
-----------
* spm.utils.spm_mne_converter

Functions
---------

### `mne_raw_2_spm`

```{text}
   Convert MNE raw object to SPM D object.
    Parameters
    ----------
    raw : mne.io.Raw
        MNE raw object.
    data_path : str
        Path to save the SPM D object.
    file_name : str, optional
        Name of the SPM D object file. The default is None, in which case the file name is generated based on the raw object.
    Returns
    -------
    D : spm.meeg
        SPM D object.
```

### `spm_2_mne_raw`

```{text}
   Convert an SPM MEEG object (D) to an MNE Raw object.
    This function translates an SPM D object (typically loaded via SPM's MEEG tools in MATLAB or via spm_matlab)
    into an MNE-Python Raw object, preserving channel information, sensor positions, units, and bad channel markings.
    It is intended for use before epoching and has been primarily tested on OPM (Optically Pumped Magnetometer) data.
    Parameters
    ----------
    D : object
        SPM MEEG object (D), expected to provide methods and attributes such as:
            - nchannels(), nsamples(), fsample(), chanlabels(), chantype(), units()
            - sensors(), badchannels(), path()
    Returns
    -------
    raw : mne.io.RawArray
        The corresponding MNE Raw object containing the data and metadata from the SPM D object.
    Notes
    -----
    - Only works before epoching (continuous/pre-epoched data).
    - Channel types and units are mapped from SPM conventions to MNE conventions.
    - Sensor positions and orientations are embedded for MEG and EEG channels.
    - The function attempts to handle the SPM 'tra' matrix for MEG sensor projections.
    - Bad channels are marked in the resulting MNE Raw object.
    - Additional work may be needed for full compatibility with all SPM data types and sensor configurations.
    Examples
    --------
    >>> raw = spm_2_mne_raw(D)
    >>> raw.plot()
```