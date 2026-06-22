# FieldTrip Synchronisation

??? info "What is FieldTrip?"
    [FieldTrip](https://www.fieldtriptoolbox.org/) is an open-source MATLAB toolbox for MEG, EEG and invasive electrophysiological data analysis, developed at the Donders Institute in Nijmegen. SPM uses it for sensor-level EEG/MEG processing.

A subset of FieldTrip is included directly in the SPM repository under `external/fieldtrip/`. Several third-party packages also shared between SPM and FieldTrip are kept in `external/` alongside it. All of these are updated automatically via a daily GitHub Action that tracks FieldTrip's `release` branch.

The sync process is controlled by [sync-fieldtrip.yml](https://github.com/spm/spm/blob/main/.github/workflows/sync-fieldtrip.yml). It opens a pull request and turns on **auto-merge (squash)**, so an update is automatically merged once the tests pass.

## What is synced

**`external/fieldtrip/`** is updated from the root of FieldTrip's `release` branch.

Specific **third-party packages** in `external/` are also maintained upstream in FieldTrip and are synced from `fieldtrip/external/`:

`bemcp`, `ctf`, `eeprobe`, `mne`, `ricoh_meg_reader`, `yokogawa_meg_reader`

To add another package, insert two lines (`+ external/<name>/` and `+ external/<name>/**`) before the `- external/*` block in [.github/fieldtrip-sync-filters](https://github.com/spm/spm/blob/main/.github/fieldtrip-sync-filters). The file uses standard [rsync filter rule syntax](https://download.samba.org/pub/rsync/rsync.html#FILTER_RULES)

Pre-compiled MEX binaries (`*.mexa64`, `*.mexmaci64`, `*.mexmaca64`, `*.mexw64`) are also copied in the sync, because FieldTrip tests and ships them on the `release` branch. Legacy MEX extensions (`*.mexw32`, `*.mexglx`, etc.) are not synced to SPM, see [.github/fieldtrip-mex-excludes](https://github.com/spm/spm/blob/main/.github/fieldtrip-mex-excludes).

## Configuring the sync

The workflow runs daily at 06:00 UTC. It can also be triggered on demand:

1. Go to **Actions → Sync FieldTrip** on the [SPM GitHub repository](https://github.com/spm/spm/actions/workflows/sync-fieldtrip.yml)
2. Click **Run workflow**

If FieldTrip's `release` branch has not changed since the last sync, the workflow exits early and no pull request is created. The last synced commit hash is tracked in [external/fieldtrip/.fieldtrip-version](https://github.com/spm/spm/blob/main/external/fieldtrip/.fieldtrip-version) and compared against the current `release` HEAD on each run; this file is updated as part of every sync PR.

### Auto-merge

For auto-merge to take effect, two repository-level settings must be in place:

- **Allow auto-merge** must be enabled in the repository settings.
- A **branch protection rule on `main`** must require at least one status check (the `Tests passed` check above).

If either is missing, the `gh pr merge --auto` call fails and the workflow emits a warning; the PR stays open and can be merged manually.

## Authentication token

The sync workflow requires a personal access token stored as the repository secret `PAT_FIELDTRIP_SYNC`.

Without the PAT, the workflow falls back to `GITHUB_TOKEN`: the pull request is still created, but tests are not triggered and auto-merge will not complete, so the PR sits open until handled manually.

The token requires the following permissions on the `spm/spm` repository:

- **Contents:** Read and write
- **Pull requests:** Read and write
