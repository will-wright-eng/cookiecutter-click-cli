#* Variables
SHELL := /usr/bin/env bash
PYTHON := python
PYTHONPATH := `pwd`

#* Docker variables
IMAGE := {{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }}
VERSION := latest

.PHONY: $(shell sed -n -e '/^$$/ { n ; /^[^ .\#][^ ]*:/ { s/:.*$$// ; p ; } ; }' $(MAKEFILE_LIST))

.DEFAULT_GOAL := help

help: ## command description list make commands
	@echo ${MAKEFILE_LIST}
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

#* Poetry
poetry-download: ## command description
	curl -sSL https://install.python-poetry.org | $(PYTHON) -

poetry-remove: ## command description
	curl -sSL https://install.python-poetry.org | $(PYTHON) - --uninstall

#* Installation
install: ## command description
	poetry lock -n && poetry export --without-hashes > requirements.txt
	poetry install -n
	-poetry run mypy --install-types --non-interactive ./

pre-commit-install: ## command description
	poetry run pre-commit install

#* Formatters
codestyle: ## command description
	poetry run pyupgrade --exit-zero-even-if-changed --py310-plus **/*.py
	poetry run isort --settings-path pyproject.toml ./
	poetry run black --config pyproject.toml ./

#* Linting
test: ## command description
	PYTHONPATH=$(PYTHONPATH) poetry run pytest -c pyproject.toml --cov-report=html --cov={{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }} tests/

check-codestyle: ## command description
	poetry run isort --diff --check-only --settings-path pyproject.toml ./
	poetry run black --diff --check --config pyproject.toml ./
	poetry run darglint --verbosity 2 {{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }} tests

mypy: ## command description
	poetry run mypy --config-file pyproject.toml ./

check-safety: ## command description
	poetry check
	poetry run safety check --full-report
	poetry run bandit -ll --recursive {{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }} tests

update-dev-deps: ## command description
	poetry add -D bandit@latest darglint@latest "isort[colors]@latest" mypy@latest pre-commit@latest pydocstyle@latest pylint@latest pytest@latest pyupgrade@latest safety@latest pytest-html@latest pytest-cov@latest
	poetry add -D --allow-prereleases black@latest

#* Docker
# Example: make docker-build VERSION=latest
# Example: make docker-build IMAGE=some_name VERSION={{ cookiecutter.version }}
docker-build: ## command description
	@echo Building docker $(IMAGE):$(VERSION) ...
	docker build \
		-t $(IMAGE):$(VERSION) . \
		-f ./docker/Dockerfile --no-cache

# Example: make docker-remove VERSION=latest
# Example: make docker-remove IMAGE=some_name VERSION={{ cookiecutter.version }}
docker-remove: ## command description
	@echo Removing docker $(IMAGE):$(VERSION) ...
	docker rmi -f $(IMAGE):$(VERSION)

#* Cleaning
pycache-remove: ## command description
	find . | grep -E "(__pycache__|\.pyc|\.pyo$$)" | xargs rm -rf

dsstore-remove: ## command description
	find . | grep -E ".DS_Store" | xargs rm -rf

mypycache-remove: ## command description
	find . | grep -E ".mypy_cache" | xargs rm -rf

ipynbcheckpoints-remove: ## command description
	find . | grep -E ".ipynb_checkpoints" | xargs rm -rf

pytestcache-remove: ## command description
	find . | grep -E ".pytest_cache" | xargs rm -rf

build-remove: ## command description
	rm -rf build/

.PHONY: cleanup
cleanup: pycache-remove dsstore-remove mypycache-remove ipynbcheckpoints-remove pytestcache-remove

.PHONY: lint
lint: test check-codestyle mypy check-safety
