# :notebook_with_decorative_cover: SPM Documentation :wave:

[![License: CC-BY-SA-4.0](https://img.shields.io/badge/license-CC%20BY--SA%204.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/build.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/build.yml)

The SPM Documentation is built using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), a theme for the static site generator [MkDocs](https://www.mkdocs.org/).

All the features of Material for MkDocs are described in its [reference documentation](https://squidfunk.github.io/mkdocs-material/reference/). We are sponsors of the project so we have access to all of the [Insiders features](https://squidfunk.github.io/mkdocs-material/insiders/).

## Installation :computer:

First, clone or download the repository:

```shell
git clone git@github.com:spm/spm-docs.git
```

Then create a virtual environment for python and install [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and its dependencies:

```shell
python -m venv venv
source venv/bin/activate
pip  install --requirement requirements.txt
```

Preview your changes in the documentation with `mkdocs serve`
and build a static site (in `site`) with `mkdocs build`.

```shell
mkdocs serve
mkdocs build
```

The _Insiders_-only features will be available in the public site build on the SPM website.

To deactivate the virtual environment when you finished working, use:

```shell
deactivate
```

## Test :bug:

The documentation is built using [GitHub Action](https://github.com/spm/spm-docs/actions) after each commit with the non-Insiders version of Material for MkDocs.

## License

The SPM Documentation is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License. To view a copy of this license, see [LICENSE](LICENSE) or visit http://creativecommons.org/licenses/by-sa/4.0/.
