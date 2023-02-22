## Preamble

Installing SPM on Windows 64 bit (XP, Vista, 7, 8, 10) should be
straightforward. There are pitfalls common to all versions of SPM:

- It is not recommended to install SPM in MATLAB\'s toolbox folder as
  MATLAB toolbox directories can be cached and changes in SPM files
  (such as updates) might not appear when using SPM. See [Toolbox Path
  Caching](http://www.mathworks.com/help/matlab/matlab_env/toolbox-path-caching-in-the-matlab-program.html)
  and [rehash](http://www.mathworks.com/help/matlab/ref/rehash.html)

<!-- -->

- If you get errors regarding \"gui_mainfcn\", you have to reset your
  MATLAB path (probably following an installation of a new MATLAB
  release), use the button \"Default\" in File \> Set Path. See also
  [restoredefaultpath](http://www.mathworks.com/help/matlab/matlab_env/what-is-the-matlab-search-path.html).

<!-- -->

- If you use [WinZip](http://www.winzip.com/) to unpack SPM\'s \*.tar.gz
  archives, make sure to uncheck \"Tar file smart CR/LF conversion\",
  which is under the \"miscellaneous\" tab in \"configuration\" under
  \"options\", as it could corrupt some of SPM files otherwise.
  [7-Zip](http://www.7-zip.org/) is a good open source software
  alternative.

<!-- -->

- If you see garbled text or incorrect characters in the interface, you
  have to change your *system locale* so that it matches your *user
  locale*. This is documented
  [here](http://www.mathworks.com/help/matlab/matlab_env/characters-incorrectly-displayed-on-windows-systems.html).
  See [How the MATLAB process uses locale
  settings](http://www.mathworks.com/help/matlab/matlab_env/how-the-matlab-process-uses-locale-settings.html)
  and [Setting
  locale](http://www.mathworks.com/help/matlab/matlab_env/setting-locale-on-windows-platforms.html)
  for how to fix it. If problems persist, try the instructions detailed
  at the end of this page.
- If you have an error \"Where is spm_check_version.m?\", make sure not
  to start SPM when your MATLAB current directory is
  *C:\Windows\system32* and not to have this directory in your MATLAB
  path. See [MathWorks
  support](http://www.mathworks.co.uk/support/solutions/en/data/1-5JUPSQ/).

## SPM12

### Installation

- Download
  [spm12.zip](https://www.fil.ion.ucl.ac.uk/spm/software/download/).
- Unzip spm12.zip in a folder of your choice, such as
  C:\Users\\*login*\Documents\MATLAB\spm12).
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\Users\`*`login`*`\Documents\MATLAB\spm12`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

### Update

If you have just downloaded the spm12.zip archive, it already contains
the latest set of updates. To update SPM when a new version is released:

- Download
  [spm12_updates_rxxxx.zip](http://www.fil.ion.ucl.ac.uk/spm/download/spm12_updates/)
- Unzip spm12_updates_rxxxx.zip on top of the folder containing your SPM
  installation so that newer files overwrite existing files.

Alternatively, you can use the spm_update.m function:

`>> spm_update`

If a new version is available, it can be applied to your local
installation by typing:

`>> spm_update update`

### Compilation

Precompiled MEX files (\*.mexw64) are provided with SPM12 and you
shouldn\'t need to recompile them by yourself.

See [Compilation of SPM12 MEX files on
Windows](SPM/SPM12_MEX_Compilation_on_Windows "wikilink") if needed.

## SPM8

### Installation

- Download
  [spm8.zip](https://www.fil.ion.ucl.ac.uk/spm/software/download/).
- Unzip spm8.zip in a folder of your choice, such as
  C:\Users\\*login*\Documents\MATLAB\spm8).
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\Users\`*`login`*`\Documents\MATLAB\spm8`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

### Update

If you have just downloaded the spm8.zip archive, it already contains
the latest set of updates. To update SPM when a new version is released:

- Download
  [spm8_updates_rxxxx.zip](http://www.fil.ion.ucl.ac.uk/spm/download/spm8_updates/)
- Unzip spm8_updates_rxxxx.zip on top of the folder containing your SPM
  installation so that newer files overwrite existing files.

Alternatively, you can use the spm_update.m function:

`>> spm_update`

If a new version is available, it can be applied to your local
installation by typing:

`>> spm_update update`

### Compilation

Precompiled MEX files (\*.mexw64) are provided with SPM8 and you
shouldn\'t need to recompile them by yourself.

See [Compilation of SPM8 MEX files on
Windows](SPM/SPM8_MEX_Compilation_on_Windows "wikilink") if needed.

## SPM5

### Installation

- Download
  [spm5.zip](https://www.fil.ion.ucl.ac.uk/spm/software/download/).
- Unzip spm5.zip in a folder of your choice, such as C:\spm\spm5).
- Start MATLAB and add SPM into your path, either using *File \> Set
  Path \> Add Folder\...* or typing

`>> addpath C:\spm\spm5`

in MATLAB\'s workspace.

- Launch SPM by typing

`>> spm`

### Compilation

Precompiled MEX files (\*.mexw64) are provided with SPM5 and you
shouldn\'t need to recompile them by yourself.

## SPM2

SPM2 is not available on Windows 64 bit.

## Troubleshooting

If text still appears garbled in the interface after changing the system
locale, try the following:

`cd(spm('Dir'))`  
`load spm_Menu.fig -mat`  
`hgS_070000.properties.DefaulttextFontName='Arial Narrow';`  
`hgS_070000.properties.DefaultuicontrolFontName='Arial Narrow';`  
`save spm_Menu.fig hgS_070000 -v6`

`% SPM12 only`  
`% Edit spm/spm_Welcome.m and modify line:`  
`PF.helvetica = 'Arial Narrow';`

`% SPM8 only`  
`cd(spm('Dir'))`  
`load spm_Welcome.fig -mat`  
`hgS_070000=spm_changepath(hgS_070000,'Helvetica','Arial Narrow');`  
`save spm_Welcome.fig hgS_070000 -v6`

`set(0,'defaultTextFontName','Arial Narrow') `  
`set(0,'defaultUicontrolFontName','Arial Narrow') `

Edit spm/matlabbatch/private/cfg_mlbatch_defaults.m and modify lines:

`cfg_defaults.cfg_ui.lfont.FontName   = 'Arial Narrow';`  
`cfg_defaults.cfg_ui.bfont.FontName   = 'Arial Narrow';`
