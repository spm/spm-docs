# SPM and Python

## Troubleshooting

### MacOS

#### `mpython` / `mwpython`

The MATLAB SDK cannot be used with the normal python interpreter on MacOS.
Instead, the MATLAB runtime ships with its own interpreter called `mwpython`.

However, `mwpython` does not interface correctly with conda environments
(it overrides environement variables that cause compiled libraries
to not be correctly loaded). For example, `mwpython` crashes when
importing `scipy.sparse`.

Instead, we provide our own wrapper, `mpython`, which is automatically
installed with this package. It does solve the conda environement issue.

That said, the `matplotlib` package still cannot be used with this wrapper
(nor can it be used with MATLAB's `mwpython`).

#### Jupyter + libcrypto

When running jupyter, you may face an error related to `libcrypto.3.dylib`.
If this happens, you may want to try running the runtime installation
with the `--patch` option (or `patch=True`). **Warning:** this option
modifies files whithin the MATLAB runtime installation folder, which
may have unexpected effects. This is not a robust/thoroughly tested fix.

#### Jupyter + `mpython`

By default, jupyter runs its kernel through the "normal" python interpreter.
In order to use the MATLAB packages in the kernel, it is necessary to
inform jupyter that it should run its kernels through `mpython`.

To do so, locate the kernel file(s), usually at
`/Users/{username}/Library/Jupyter/kernels/{kernel_name}/kernel.json`, and
replace the path to the default interpreter (_e.g._,
`"/Users/{username}/miniforge3/envs/{env_name}/bin/python"`) with the path
to `mpython` (_e.g._,
`"/Users/{username}/miniforge3/envs/{env_name}/bin/mpython"`).

If you cannot locate the kerenel file, install the kernel
by running:

```shell
mpython -m ipykernel install --user --name {kernel_name}
```

You may need to install `ipykernel` beforehand.

### Windows

#### `DeclarativeService.dll not found`

Python installation made from Microsoft Store on Windows will not work
(raises DeclarativeService.dll not found), install it from Python website.
