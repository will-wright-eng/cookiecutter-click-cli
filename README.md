# cookiecutter-click-cli

## TODO

- modify
	- cleanup Makefile
	- tone down dependabot
	- do something to tone down workflows... not sure what
	- fix click/black discrepency
- remove
	- release-drafter
	- docker & dockerignore
	- code coverage
- add
	- pre-commit plugins that I like
	- codeowners file
	- publishing instructions
	- Windows/Linux installs to GHA
	- click docs: https://click.palletsprojects.com/
	- from setup notes
		- python3.10 as default/ remove <3.8
		- instructions for cli dev: `pip install -e .`
		- good variety of bultin functions/methods --> ref docs
			- create explicity code notes
- add cookiecutter options
	- open source boiler docs? ie SECURITY, CODE_OF_CONDUCT, CONTRIBUTING
	- setup.py or pyproject.toml option

---
---

1. make a bunch of changes
2. test cookiecutter setup
3. document setup flow
4. build out GHA

## additonal OS testing

[see waynerv project](https://github.com/waynerv/cookiecutter-pypackage)

```yaml
# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "test"
  test:
    # The type of runner that the job will run on
    strategy:
      matrix:
        python-versions: [3.6, 3.7, 3.8, 3.9]
        os: [ubuntu-18.04, macos-latest, windows-latest]
    runs-on: ${{ matrix.os }}
```

## link dump -- cookiecutter projects

- [click-app/cli.py at main · simonw/click-app](https://github.com/simonw/click-app/blob/main/%7B%7Bcookiecutter.hyphenated%7D%7D/%7B%7Bcookiecutter.underscored%7D%7D/cli.py)
- [fpgmaas/cookiecutter-poetry: A modern cookiecutter template for Python projects that use Poetry for dependency management](https://github.com/fpgmaas/cookiecutter-poetry)
- [s3rius/FastAPI-template: Feature rich robust FastAPI template.](https://github.com/s3rius/FastAPI-template)
- [TezRomacH/python-package-template: 🚀 Your next Python package needs a bleeding-edge project structure.](https://github.com/TezRomacH/python-package-template)
- [waynerv/cookiecutter-pypackage: A tool for creating skeleton python project, built with popular develop tools and conform to best practice.](https://github.com/waynerv/cookiecutter-pypackage)

## setup notes

```bash
conda create -n test-cookie python=3.10
conda activate test-cookie

# create repo manually and replace contents with cookiecutter output
cookiecutter gh:TezRomacH/python-package-template --checkout v1.1.1
git clone git@github.com:will-wright-eng/common-sync.git

conda deactivate
```

```text
Your project common-sync is created.

1) Now you can start working on it:

    $ cd common-sync && git init

2) If you don't have Poetry installed run:

    $ make poetry-download

3) Initialize poetry and install pre-commit hooks:

    $ make install
    $ make pre-commit-install

4) Run codestyle:

    $ make codestyle

5) Upload initial code to GitHub:

    $ git add .
    $ git commit -m ":tada: Initial commit"
    $ git branch -M main
    $ git remote add origin https://github.com/common-sync/common-sync.git
    $ git push -u origin main
```

- GNU License missing `s` in `https`
- poetry error

> The canonical source for Poetry's installation script is now https://install.python-poetry.org. Please update your usage to reflect this.

```bash
#* Poetry
.PHONY: poetry-download
poetry-download:
	curl -sSL https://install.python-poetry.org | $(PYTHON) -
```

- make install
- make pre-commit-install
- make codestyle

> ImportError: cannot import name '_unicodefun' from 'click' (/Users/willcasswrig/Library/Caches/pypoetry/virtualenvs/common-sync-a6r7c1El-py3.10/lib/python3.10/site-packages/click/__init__.py)
> make: *** [codestyle] Error 1

use my answer on SO: https://stackoverflow.com/a/74086834/14343465

```yaml
[tool.poetry.dev-dependencies]
black = {version = "^22.3.0", allow-prereleases = true}

[tool.black]
# https://github.com/psf/black
target-version = ["py39"]
line-length = 120
color = true
```

- install dev cli

```bash
conda create -n test-csync python=3.10
pip install -e .
cysnc --help
csync --name Will

(test-csync) ➜  common-sync git:(first) ✗ csync --name Will
# Hello Will!
(test-csync) ➜  common-sync git:(first) ✗ csync --version
# common-sync version: 0.1.0
```

- references to `master` branch in GHA are out of date

```yaml
on:
  push:
    # branches to consider in the event; optional, defaults to all
    branches:
      - master
```

- example.py sucks, create something more interesting
	- docstring format looks good though
