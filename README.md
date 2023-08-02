# :notebook_with_decorative_cover: SPM Documentation :wave:

[![License: CC-BY-SA-4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/build.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/build.yml)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/spelling.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/spelling.yml)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/linting.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/linting.yml)
[![Tests](https://github.com/spm/spm-docs/actions/workflows/linkcheck.yml/badge.svg)](https://github.com/spm/spm-docs/actions/workflows/linkcheck.yml)

This repository contains the [documentation](https://www.fil.ion.ucl.ac.uk/spm/docs/) of the [SPM software](https://github.com/spm/spm). It is built using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), a theme for the static site generator [MkDocs](https://www.mkdocs.org/).

All the features of Material for MkDocs are described in its [reference documentation](https://squidfunk.github.io/mkdocs-material/reference/). We are [sponsoring](https://github.com/orgs/spm/sponsoring) the project and therefore have access to all of the [Insiders features](https://squidfunk.github.io/mkdocs-material/insiders/).

> :warning: This repository contains all the files used to generate the SPM documentation. This is therefore the place to make some edits and modifications to the documentation. If you only want to read the documentation, please have a look [here](https://www.fil.ion.ucl.ac.uk/spm/docs/).

## :skier: Getting started

### :bookmark_tabs: File layout

`MkDocs` is configured using the [`mkdocs.yml`](mkdocs.yml) file at the root of the Git repository.

The [`mkdocs.yml`](mkdocs.yml) file defines the top level navigation for the site. The `nav` configuration setting in this file defines which pages are included in the global site navigation menu as well as the structure of that menu.

See [MkDocs documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#file-layout) for more details.

### :cool: Markdown syntax

`MkDocs` pages are written using the [Markdown](https://daringfireball.net/projects/markdown/syntax) syntax.

`MkDocs` natively supports [Markdown extensions](https://squidfunk.github.io/mkdocs-material/setup/extensions/) that enhance the Markdown writing experience. [Plugins](https://www.mkdocs.org/dev-guide/plugins/), built-in or third-party, are extensions to the MkDocs framework which allow users to add custom functionality and features to their MkDocs projects. These plugins can be used to add additional features such as search or analytics.

See [MkDocs documentation](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown) for more details, and [best-of-mkdocs](https://github.com/pawamoy/best-of-mkdocs) for a curated list of plugins.

To edit Markdown documents, we recommend [Visual Studio Code](https://code.visualstudio.com/) with its [preview mode](https://code.visualstudio.com/docs/languages/markdown). If you want to make a quick change, navigate on GitHub to the page of the file you wish to modify (or click on the `Edit this page` icon) and press the `.` key: it will open a VS Code environment directly in your browser.

## :computer: Installation

If you want to edit and build the documentation yourself, you first need to clone or download the repository:

```shell
git clone git@github.com:spm/spm-docs.git
```

Then create a virtual environment for Python and install [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and its dependencies:

```shell
python3 -m venv venv
source venv/bin/activate
pip install --requirement requirements.txt
```

On Windows, install [Python from the Microsoft Store](https://apps.microsoft.com/store/detail/python-311/9NRWMJP3717K) then type the following in a Command Prompt or PowerShell window:

```shell
python3 -m venv venv
.\venv\Scripts\activate
pip install --requirement requirements.txt
```

Still on Windows, if you have issues with the above commands due to restrictions on your system, please run the following after creating the virtual environment:

```shell
Powershell.exe -NoProfile -ExecutionPolicy Bypass -File <the full path of the activate.ps1 file>
```

You can preview the documentation as you work on it thanks to a built-in web server. When you are in the same directory as the `mkdocs.yml` configuration file, start the server with the command:

```shell
mkdocs serve
```

<!-- markdown-link-check-disable-next-line -->
You can then browse the documentation in your web browser at [http://127.0.0.1:8000/](http://127.0.0.1:8000/). Each time a change is made, the documentation is rebuilt and the page auto-reloaded.

To deploy the documentation, use the following command:

```shell
mkdocs build
```

The documentation is then built as a static site in a directory called `site`.

The _Insiders_-only features will be here ignored but available in the public build on the SPM website.

To deactivate the virtual environment when you finished working, use:

```shell
deactivate
```

[MkDocs plugins](https://github.com/pawamoy/best-of-mkdocs) currently used:
* [`mkdocs-bibtex`](https://github.com/shyamd/mkdocs-bibtex): a plugin for citation management using bibtex
* [`MkDocs Video`](https://github.com/soulless-viewer/mkdocs-video): a plugin to embed videos in the documentation pages

## :lion: Style guide

### 1. Language
Use British English.

### 2. Code
Format any code included in the documentation as [code blocks](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/).

Raw text:

\```matlab

code

\```

Display text:
```
code
```

Format any file names, paths, functions, input variables, etc. as in-line code. 

Raw text: \`code`

Display text:
`code`

### 3. Abbreviations
All [abbreviations](https://squidfunk.github.io/mkdocs-material/reference/tooltips/#adding-abbreviations) should be defined alphabetically in [`addons/abbreviations.md`](addons/abbreviations.md) in the following format:
	
	*[BOLD]: Blood Oxygen Level Dependent 

To use the abbreviations file on the page you’re creating, add the below at the end of your markdown file:

	--8<-- "addons/abbreviations.md"

### 4. Images    
Illustrations should be saved as PNG or SVG files in the [`docs/assets/figures/`
](docs/assets/figures/) directory. [OptiPNG](https://optipng.sourceforge.net/) should be applied on PNG files before commit.

To display [images with captions](https://squidfunk.github.io/mkdocs-material/reference/images/#image-captions), use:

```html
<figure markdown>
  ![Image title](../../../assets/figures/image.png){ width="300" }
  <figcaption>Image caption</figcaption>
</figure>
```

Videos should be saved as MP4 in the [`docs/assets/videos/`](docs/assets/videos/) directory. Videos encoded in other formats can be converted to MP4 with [FFmpeg](https://www.ffmpeg.org/).

### 5. Information boxes
Additional information can be highlighted using [information boxes](https://squidfunk.github.io/mkdocs-material/reference/admonitions/). 

Boxes can be expanded:
```
!!! tip “Title of your box”
    Text of your box
```

Or collapsible:
```
??? tip "Title of your box"
    Text of your box
```
Use expanded boxes for crucial succinct information. For anything additional or lengthy, use collapsible boxes. 

Various icons and colour-coding are available, please use:
- `tip` 	for top tips on SPM things
- `info` 	for core information not covered in the main text
- `note`	for additional non-essential information
- `failure` 	for help on troubleshooting

Full list of icons is available [here](https://squidfunk.github.io/mkdocs-material/reference/admonitions/).

### 6. Symbols

A selection of symbols can be used. 
Currently used:
- `++return++` enter/return key
- `:material-arrow-right-bold:` right facing arrow
- `:material-play:` play icon

Full list of symbols is available [here](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/). 

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
