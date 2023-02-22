# :notebook_with_decorative_cover: SPM Documentation :wave:

[![License: CC-BY-SA-4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/build.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/build.yml)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/spelling.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/spelling.yml)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/linting.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/linting.yml)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/linkcheck.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/linkcheck.yml)

The SPM Documentation is built using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), a theme for the static site generator [MkDocs](https://www.mkdocs.org/).

All the features of Material for MkDocs are described in its [reference documentation](https://squidfunk.github.io/mkdocs-material/reference/). We are [sponsoring](https://github.com/orgs/spm/sponsoring) the project and therefore have access to all of the [Insiders features](https://squidfunk.github.io/mkdocs-material/insiders/).

> :warning: This repository contains all the files used to generate the SPM documentation. This is therefore the place to make some edits and modifications to the documentation. If you only want to read the documentation, please have a look at [TBA](#).

## :skier: Getting started

### :bookmark_tabs: File layout

`MkDocs` is configured using the [mkdocs.yml](mkdocs.yml) file at the root of the git repository.

The [mkdocs.yml](mkdocs.yml) file defines the top level navigation for the site. The `nav` configuration setting in this file defines which pages are included in the global site navigation menu as well as the structure of that menu.

See [MkDocs documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#file-layout) for more details.

### :cool: Markdown syntax

`MkDocs` pages are written using the [Markdown](https://daringfireball.net/projects/markdown/syntax) syntax.

`MkDocs` natively supports [Markdown extensions](https://squidfunk.github.io/mkdocs-material/setup/extensions/) that enhance the Markdown writing experience. [Plugins](https://www.mkdocs.org/dev-guide/plugins/), built-in or third-party, are extensions to the MkDocs framework which allow users to add custom functionality and features to their MkDocs projects. These plugins can be used to add additional features such as search or analytics.

See [MkDocs documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown) for more details, and [best-of-mkdocs](https://github.com/pawamoy/best-of-mkdocs) for a curated list of plugins.

To edit Markdown documents, we recommend [Visual Studio Code](https://code.visualstudio.com/) with its [preview mode](https://code.visualstudio.com/docs/languages/markdown). If you want to make a quick change, navigate on GitHub to the page of the file you wish to modify and press the `.` key: it will open a VS Code environment directly in your browser.

## :computer: Installation

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

Currently not used but something to consider is the [MkDocs plugin for citation management using bibtex](https://github.com/shyamd/mkdocs-bibtex).

## :lion: Style guide

* British English

## :bug: Testing

### :hammer_and_wrench: Build

The documentation is built using [GitHub Action](https://github.com/spm/spm-docs/actions) after each commit in the `main` branch with the non-_Insiders_ version of Material for MkDocs.

### :pencil: Spelling

#### codespell

Detect common misspellings with [`codespell`](https://github.com/codespell-project/codespell).

To run [`codespell`](https://github.com/codespell-project/codespell) interactively on the SPM documentation, use:

```shell
codespell -w -i 1 docs
```

#### PySpelling

To run [`PySpelling`](https://facelessuser.github.io/pyspelling/) on the SPM documentation, use:

```shell
# Run PySpelling using the default spell checker, Aspell
pyspelling
# Run PySpelling using the Hunspell spell checker
pyspelling --spellchecker hunspell
```

### :chains: Links Check

#### markdown-link-check

A [GitHub Action](https://github.com/marketplace/actions/markdown-link-check) checks all Markdown files for broken links (using [markdown-link-check](https://github.com/tcort/markdown-link-check)).

### :ice_cube: Linting

#### markdownlint

To run [`markdownlint-cli`](https://github.com/igorshubovych/markdownlint-cli), a command line interface to [`markdownlint`](https://github.com/DavidAnson/markdownlint), use:

```shell
markdownlint "docs/**/*.md"
```

Note that only MkDocs build and codespell will potentially fail during continuous integration on GitHub Actions. The other reports are only advisory, for your perusal.

## :clinking_glasses: Contributing

Contributions are most welcome and appreciated! See the [First Contributions](https://github.com/firstcontributions/first-contributions) project for instructions.

1. [GitHub repository](https://github.com/spm/spm-docs): Report issues, make feature requests or open pull requests.
2. [SPM@JiscMail](https://www.fil.ion.ucl.ac.uk/spm/support/): Open mailing list for discussion and questions about SPM.

## :ribbon: License

The SPM Documentation is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License. To view a copy of this license, see [LICENSE](LICENSE) or visit http://creativecommons.org/licenses/by-sa/4.0/.
