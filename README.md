# cookiecutter-click-cli

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
