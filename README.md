# :notebook_with_decorative_cover: SPM Documentation :wave:

[![License: CC-BY-SA-4.0](https://img.shields.io/badge/license-CC%20BY--SA%204.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/build.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/build.yml)

The SPM Documentation is built using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), a theme for the static site generator [MkDocs](https://www.mkdocs.org/).

All the features of Material for MkDocs are described in its [reference documentation](https://squidfunk.github.io/mkdocs-material/reference/). We are sponsoring the the project and therefore have access to all of the [Insiders features](https://squidfunk.github.io/mkdocs-material/insiders/).

## Getting started :skier:

### File layout :bookmark_tabs:

`MkDocs` is configured using the [mkdocs.yml](mkdocs.yml) file at the root of the git repository.

The [mkdocs.yml](mkdocs.yml) file defines the top level navigation for the site. The `nav` configuration setting in this file defines which pages are included in the global site navigation menu as well as the structure of that menu.

See [MkDocs documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#file-layout) for more details.

### Markdown syntax :cool:

`MkDocs` pages are written using the [Markdown](https://daringfireball.net/projects/markdown/syntax) syntax.

`MkDocs` natively supports [Markdown extensions](https://squidfunk.github.io/mkdocs-material/setup/extensions/) that enhance the Markdown writing experience. [Plugins](https://www.mkdocs.org/dev-guide/plugins/), built-in or third-party, are extensions to the MkDocs framework which allow users to add custom functionality and features to their MkDocs projects. These plugins can be used to add additional features such as search or analytics.

See [MkDocs documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown) for more details.

## Installation :computer:

If you want to edit and build the documentation yourself, you first need to clone or download the repository:

```shell
git clone git@github.com:spm/spm-docs.git
```

Then create a virtual environment for python and install [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and its dependencies:

```shell
python3 -m venv venv
source venv/bin/activate
pip  install --requirement requirements.txt
```

Preview your changes in the documentation with `mkdocs serve` and build a static site (in `site`) with `mkdocs build`.

```shell
mkdocs serve
mkdocs build
```

The _Insiders_-only features will be here ignored but available in the public build on the SPM website.

To deactivate the virtual environment when you finished working, use:

```shell
deactivate
```

## Testing :bug:

### Build :hammer_and_wrench:

The documentation is built using [GitHub Action](https://github.com/spm/spm-docs/actions) after each commit in the `main` branch with the non-_Insiders_ version of Material for MkDocs.

### Spelling :pencil:

Detect common misspellings with [codespell](https://github.com/codespell-project/codespell).

TO run `codespell` interactively on the SPM documentation, use:

```shell
codespell -w -i 1 docs
```

## Contributing :clinking_glasses:

Contributions are most welcome and appreciated! See the [First Contributions](https://github.com/firstcontributions/first-contributions) project for instructions.

1. [GitHub repository](https://github.com/spm/spm-docs): Report issues, make feature requests or open pull requests.
2. [SPM@JiscMail](https://www.fil.ion.ucl.ac.uk/spm/support/): Open mailing list for discussion and questions about SPM.

## License :ribbon:

The SPM Documentation is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License. To view a copy of this license, see [LICENSE](LICENSE) or visit http://creativecommons.org/licenses/by-sa/4.0/.
