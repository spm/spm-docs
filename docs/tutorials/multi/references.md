# Multimodal, Multisubject data fusion 

## References

* Henson, R.N., Mattout, J., Phillips, C. and Friston, K.J. (2009).
Selecting forward models for MEG source-reconstruction using
model-evidence. Neuroimage, 46, 168-176.  
* Henson, R.N., Wakeman, D.G., Litvak, V. and Friston, K.J. (2011). A
Parametric Empirical Bayesian framework for the EEG/MEG inverse problem:
generative models for multisubject and multimodal integration. Frontiers
in Human Neuroscience, 5, 76, 1-16.  
* Wakeman, D.G. and Henson, R.N. A multi-subject, multi-modal human
neuroimaging dataset. Scientific Data, 2:150001.  

## Acknowledgements

This work was supported by MRC (A060_MC_5PR10). The author (RNH) thanks
Rebecca Beresford, Hunar Abdulraham, Daniel Wakeman, Guillaume Flandin
and Vladimir Litvak for their help.

[^1]: <http://www.fil.ion.ucl.ac.uk/spm/doc/papers/HensonEtAl_FiHN_11_PEB_MEEG.pdf>

[^2]: <http://www.nature.com/articles/sdata20151>

[^3]: On Linux, from the command line, you can type the following to
    download all of the relevant data from Subject 15:

    ```
    wget -r -nH –cut-dirs=3 ftp://ftp.mrc-cbu.cam.ac.uk/personal/rik.henson/wakemandg_hensonrn/Sub15/ -X "/*/*/*/*/DWI/,/*/*/*/*/FLASH/" –reject raw.fif
    ```

    On Windows, you can access the FTP server from the File Explorer and
    copy the entire folder on your hard disk. Alternatively, you can use
    dedicated software such as
    [FileZilla](https://filezilla-project.org/) or
    [WinSCP](http://winscp.net/).

[^4]: Note that you can create a MATLAB variable containing the weights
    of the F-contrast with `C = kron(eye(3),[1 0 0])`, and then enter
    `C`, `0.5*C(1,:)+0.5*C(2,:)-C(3,:)`, `C(1,:)`, `C(2,:)` and `C(3,:)`
    respectively for the 5 contrasts specified above.

--8<-- "addons/abbreviations.md"
