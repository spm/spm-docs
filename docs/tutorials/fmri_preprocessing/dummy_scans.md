# fMRI data preprocessing

## Removal of dummy scans

The first few volumes of a functional acqusition contain large signal changes which stabilise as the tissues reach steady state. For this reason, it is recommended to discard the initial few volumes from datasets that do not include lead-in dummy scans. 

In this dataset, the first 12 volumes have been discarded. 

!!! info "Note about more recent datasets"
    Please note that for more recent datatsets which include lead-in dummy scans, this step may not be necessary. Always make sure that you know the protocol used for acquisition of the data you are working with. 

--8<-- "addons/abbreviations.md"