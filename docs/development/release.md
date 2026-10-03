# Release Process via GitHub Actions

??? info "What are GitHub Actions?"
    GitHub Actions is a service on GitHub that is free for public repositories. It provides runners with different OS's (Windows, MacOS, Linux) that can perform automated tasks.

Creating a new tag on the [SPM repository](https://github.com/spm/spm) triggers the release process. The release description has to be added manually.

The release process

- bakes the version information into SPM
- releases SPM as a ZIP file
- creates standalone versions for the different OSes
- releases docker and singularity containers

The process is controlled via the GitHub actions files [release.yml](https://github.com/spm/spm/blob/main/.github/workflows/release.yml), [build-docker.yml](https://github.com/spm/spm-docker/blob/main/.github/workflows/build-docker.yml) and [build-singularity.yml](https://github.com/spm/spm-docker/blob/main/.github/workflows/build-singularity.yml).

## Triggering the release

Creating and pushing a tag on a commit triggers the release process. Only tags that conform to the [versioning guidelines](versioning.md) will trigger a new build, for example 25.01.  
This can also be done by creating a [new release on the GitHub webpage](https://github.com/spm/spm/releases/new), selecting the corresponding branch as target and instead of choosing a tag, creating a new one.  

## Setting the version in SPM

The release process automatically sets the version to the one given by the tag and the current date. The version in SPM is controlled by line 2 in the file [Contents.m](https://github.com/spm/spm/blob/main/Contents.m), which is the development version in the GitHub repository.

## SPM release

The main SPM release is available as e.g. spm_25.01.02.zip on the [release page on github](https://github.com/spm/spm/releases). It is basically identical to the files on GitHub with only the version number adjusted. Please note, that downloading the *source code* on the release tab instead would give the same SPM files, but without the version information.

## SPM standalone

The [SPM standalone](../installation/standalone.md) is built on four different OS configurations: Windows, Linux, MacOS (Intel) and MacOS (Apple Silicon). The MATLAB release is fixed in [release.yml](https://github.com/spm/spm/blob/main/.github/workflows/release.yml) rather than following the newest release, so a new MATLAB release cannot change a build unannounced; update it there, and in `install_test_standalone.yml` and `test_container.yml`, before a release. MacOS (Intel) is built with an older release, because MATLAB R2026a and later no longer support Intel Macs. The standalone ZIP contains an installer for the required Matlab Runtime, downloading a smaller Matlab Runtime version tailored to running SPM.

## Containers

The [SPM containers](../installation/containers.md) build-action is automatically triggerend after the release process finishes. It builds one docker container with the SPM standalone and the Matlab Runtime and a second container with SPM using [GNU Octave](https://octave.org/). In addition, both docker containers are transformed into singularity containers and released as well. They can be accessed on the [GitHub Container Registry](https://github.com/spm/spm-docker/pkgs/container/spm-docker). All the containers can be used without a Matlab License.

## Keeping the workflow actions up to date

The GitHub Actions used across the SPM workflows are pinned to commit hashes rather than to version tags, because a tag can be moved to point at different code. [Dependabot](https://github.com/spm/spm/blob/main/.github/dependabot.yml) proposes updates to those pins in a single grouped pull request each month.

Two actions are excluded from Dependabot and have to be checked by hand, roughly once a year: [peter-evans/repository-dispatch](https://github.com/peter-evans/repository-dispatch) and [softprops/action-gh-release](https://github.com/softprops/action-gh-release). They are used only in `release.yml`. No pull request runs that workflow, so the test suite says nothing about them and an outdated or broken version would first show up during a release. It is worth checking both for newer versions before creating a release tag.
