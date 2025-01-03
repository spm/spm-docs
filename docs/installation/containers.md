# SPM and Containers

??? info "What is Docker?"
    [Docker](https://www.docker.com/) is a container technology, performing operating-system-level virtualisation.

??? info "What is Singularity?"
    Singularity is another container technology that performs operating-system-level virtualization. One of the main uses of Singularity is to  bring containers and reproducibility to scientific computing and HPC.
    <!-- markdown-link-check-disable-next-line -->
    [`https://sylabs.io/singularity/`](https://sylabs.io/singularity/)
    [`https://apptainer.org/`](https://apptainer.org/)

## SPM Containers

Official SPM [`Dockerfile`](https://github.com/spm/spm-docker) (using the [Standalone SPM](standalone.md)) with the docker images hosted on the [GitHub container registry](https://github.com/spm/spm-docker/pkgs/container/spm-docker)

For example, to start SPM with its graphical user interface:

```bash
xhost +local:docker
docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp:/tmp -v /tmp/.X11-unix:/tmp/.X11-unix ghcr.io/spm/spm-docker:docker-matlab-latest fmri
```

If the container\'s root filesystem is mounted as read only (*\--read-only* flag), you need to bind mount an extra volume:

```bash
-v /tmp/.matlab:/root/.matlab
```

### Singularity

<!-- markdown-link-check-disable-next-line -->
[SingularityCE User Guide](https://docs.sylabs.io/guides/latest/user-guide/)

```bash
singularity pull oras://ghcr.io/spm/spm-docker:singularity-matlab-latest
singularity run spm-docker_singularity-matlab-latest.sif --version
```

([how to install singularity on Ubuntu](https://github.com/hpcng/singularity/issues/5390#issuecomment-899111181))

## See also

### Neurodesk

[`https://www.neurodesk.org/`](https://www.neurodesk.org/)

### Neurodocker

[`https://github.com/ReproNim/neurodocker`](https://github.com/ReproNim/neurodocker)

[`https://hub.docker.com/r/repronim/neurodocker`](https://hub.docker.com/r/repronim/neurodocker)

### SPM BIDS-App

[`https://github.com/BIDS-Apps/SPM`](https://github.com/BIDS-Apps/SPM)

[`https://hub.docker.com/r/bids/spm/`](https://hub.docker.com/r/bids/spm/)

### MATLAB Dockerfile

[`https://github.com/mathworks-ref-arch/matlab-dockerfile`](https://github.com/mathworks-ref-arch/matlab-dockerfile)
