# SPM without Graphical Output

- Start MATLAB with options:
  - -nodisplay -nosplash
  - -noFigureWindows on Windows platforms

<!-- -->

- Initialise SPM with the following:

`spm('defaults','fmri');`  
`spm_jobman('initcfg');`

- Enable the Command Line mode in your SPM scripts:

`spm_get_defaults('cmdline',true);`

- Use [Xvfb](http://en.wikipedia.org/wiki/Xvfb) (X virtual framebuffer)
  on Linux platforms.
