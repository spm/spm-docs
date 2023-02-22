## SPM Optimisation

### Adjusting SPM settings

A defaults variable called *maxmem* indicates how much memory can be
used at the same time during GLM estimation. If your computer has a
large amount of RAM, you can increase that memory setting in
spm_defaults.m:

`defaults.stats.maxmem   = 2^30;`

- 2^32 = 4GB
- 2^31 = 2GB
- 2^30 = 1GB
- 2^29 = 512MB

In SPM12, there is another defaults variable called *resmem* governing
whether temporary files during GLM estimation are stored on disk
(*false*) or kept in memory (*true*). If you have enough available RAM,
not writing the files to disk will speed the estimation.

`defaults.stats.resmem   = true;`

### Compiling the MEX files

The compiled MEX files provided with SPM are built in such a way to be
compatible with most platforms and MATLAB versions but you might benefit
from compiling them for your exact platform/MATLAB version - some C
compilers might also produce better optimised binaries (such as [Intel
Compilers](http://software.intel.com/en-us/intel-compilers/)). See the
installation pages for more details on how to recompile SPM MEX files.

## MATLAB Optimisation

### General statements

[Maximizing MATLAB
Performance](https://www.mathworks.com/help/matlab/performance-and-memory.html).

Note that it is not recommended to disable the JAVA Virtual Machine when
launching MATLAB (matlab -nojvm). If you don\'t want to use the MATLAB
desktop, you can preferably launch MATLAB with:

`matlab -nodesktop`

When using Matlab in \'nodesktop\' mode, initialise SPM in the following
manner to prevent graphics windows from opening:

\<syntaxhighlight lang=\"matlab\"\> spm(\'defaults\', \'fmri\')
spm_jobman(\'initcfg\') spm_get_defaults(\'cmdline\',true)
\</syntaxhighlight\>

(Substituting \'fmri\' for \'pet\' or \'eeg\' as appropriate.)

### Multithreading

Recent MATLABs support implicit multiprocessing allowing to run multiple
threads on a single machine without any change to the MATLAB code
itself: this requires a multiple CPU (multiprocessor or multicore)
system. The gain in compute time with SPM is not dramatic though.

See:

- <https://www.mathworks.com/help/matlab/ref/maxnumcompthreads.html>

If you run many MATLAB sessions in parallel to manually distribute your
SPM processings, it is recommended to set the number of computational
threads to one.

### Updating BLAS/LAPACK

Install the latest Basic Linear Algebra Subroutines (BLAS)/Linear
Algebra PACKage (LAPACK) for your system, as MATLAB doesn\'t ship the
latest version in its releases.

- [BLAS](http://www.netlib.org/blas/)
- [LAPACK](http://www.netlib.org/lapack/)

The main choices are between:

- [ATLAS](http://math-atlas.sourceforge.net/) - Automatically Tuned
  Linear Algebra Software (open source)
- [MKL](http://software.intel.com/en-us/intel-mkl/) - Intel Math Kernel
  Library
- [ACML](http://developer.amd.com/cpu/libraries/acml/Pages/default.aspx) -
  AMD Core Math Library
- [GotoBLAS](http://www.tacc.utexas.edu/tacc-projects/#blas) - GotoBLAS
  (Texas Advanced Computing Center)

For more information on how to upgrade your MATLAB libraries with
ATLAS/MKL, see:

- <http://software.intel.com/en-us/articles/using-intel-mkl-with-matlab/>
- <http://software.intel.com/en-us/articles/intel-math-kernel-library-intel-mkl-for-windows-using-intel-mkl-in-matlab-executable-mex-files/>
- <http://imaging.mrc-cbu.cam.ac.uk/imaging/SpmWithPentium4>

## Parallel Computing

### SPM toolboxes

See [pSPM](http://prdownloads.sourceforge.net/parallelspm/) for SPM2.

### General tools

There is currently work in progress to provide an official
parallel/distributed version of SPM.

- [MATLAB Parallel
  Survey](http://www.tech.plym.ac.uk/spmc/links/matlab/matlab_parallel.html)
- [Parallel MATLAB: Doing it
  right](http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=1386655&tag=1)
- [Parallel MATLAB: The next
  generation](http://www.ll.mit.edu/HPEC/agendas/proc03/abstracts/kepner-pMatlab.pdf)

See also the Sun Grid Engine Project (SGE):

- [Sun Grid Engine](http://en.wikipedia.org/wiki/Oracle_Grid_Engine)
- [qsubfunc](http://www.mathworks.com/matlabcentral/fileexchange/loadFile.do?objectId=2600&objectType=file)

## Using the Graphics Processing Unit (GPU)

It is possible for MATLAB to take advantage of the GPU (the processor of
the graphic card, by opposition to the CPU), to perform some operations,
resulting in significant speed improvements. A number of toolboxes are
available:

- [Parallel Computing
  Toolbox](https://uk.mathworks.com/help/parallel-computing/gpu-computing.html)
  GPU Computing with MATLAB
- [MATLAB GPU
  Computing](https://uk.mathworks.com/solutions/gpu-computing.html)
- [MATLAB Acceleration with CUDA-enabled
  GPUs](http://www.nvidia.com/object/matlab_acceleration.html)
- [NVIDIA GPU and
  MATLAB](https://www.nvidia.com/object/tesla-matlab-accelerations.html)
- [NVIDIA
  NGC](https://ngc.nvidia.com/catalog/containers/partners:matlab)
- [CUDA SPM](http://mri.ee.ntust.edu.tw/cuda) GPU-Accelerated SPM image
  registration
- [Deep Learning
  Frameworks](https://developer.nvidia.com/deep-learning-frameworks#matlab)

See also:

- <https://www.frontiersin.org/articles/10.3389/fninf.2014.00024/full>
- <https://www.sciencedirect.com/science/article/pii/S0169260711001957>
- <https://www.sciencedirect.com/science/article/pii/S1361841513000820>
- <https://link.springer.com/article/10.3758%2Fs13415-013-0165-7>
- <https://doi.org/10.1109/MSP.2009.935387>
