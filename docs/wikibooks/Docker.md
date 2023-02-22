\_\_NOTOC\_\_

# Docker

Docker is a container technology, performing operating-system-level
virtualisation.

[`https://www.docker.com/`](https://www.docker.com/)

# Singularity

Singularity is another container technology that performs
operating-system-level virtualization. One of the main uses of
Singularity is to bring containers and reproducibility to scientific
computing and HPC.

[`https://sylabs.io/singularity/`](https://sylabs.io/singularity/)  
[`https://apptainer.org/`](https://apptainer.org/)

## SPM Containers

Official SPM12 Dockerfile and singularity.def (using the [Standalone
SPM](SPM/Standalone "wikilink")):

[`https://github.com/spm/spm-docker`](https://github.com/spm/spm-docker)  
  
[`https://github.com/spm/spm-docker/pkgs/container/spm-docker`](https://github.com/spm/spm-docker/pkgs/container/spm-docker)  
[`https://hub.docker.com/r/spmcentral/spm/`](https://hub.docker.com/r/spmcentral/spm/)

For example, to start SPM with its graphical user interface:

`xhost +local:docker`  
`docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp:/tmp -v /tmp/.X11-unix:/tmp/.X11-unix spmcentral/spm fmri`

If the container\'s root filesystem is mounted as read only
(*\--read-only* flag), you need to bind mount an extra volume:

`-v /tmp/.matlab:/root/.matlab`

## See also

### Neurodesk

[`https://www.neurodesk.org/`](https://www.neurodesk.org/)

### Neurodocker

[`https://github.com/ReproNim/neurodocker`](https://github.com/ReproNim/neurodocker)  
[`https://hub.docker.com/r/kaczmarj/neurodocker/`](https://hub.docker.com/r/kaczmarj/neurodocker/)

### SPM BIDS-App

[`https://github.com/BIDS-Apps/SPM`](https://github.com/BIDS-Apps/SPM)  
[`https://hub.docker.com/r/bids/spm/`](https://hub.docker.com/r/bids/spm/)

### MATLAB Dockerfile

[`https://github.com/mathworks-ref-arch/matlab-dockerfile`](https://github.com/mathworks-ref-arch/matlab-dockerfile)

### Singularity

[SingularityCE User Guide](https://sylabs.io/guides/3.8/user-guide/)

`sudo singularity build spm12.sif spm12-octave.def`  
`singularity exec spm12.sif`  
`./spm12.sif --help`

([how to install singularity on
Ubuntu](https://github.com/hpcng/singularity/issues/5390#issuecomment-899111181))
