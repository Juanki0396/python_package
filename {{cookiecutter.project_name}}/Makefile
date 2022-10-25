## -----------------
## -----------------
## Config Variables
## -----------------
## -----------------

# Project
PROJECT_NAME = {{  cookiecutter.project_slug }}

# Python
PYTHON_VERSION = python{{ cookiecutter.python_version }}

# Venv
VENV_NAME = .venv
VENV_ACTIVATE = $(VENV_NAME)/bin/activate

# Packaging
{% if cookiecutter.use_flit == 'y' %}
PACKAGE_TOOL = flit
PACKAGE_BUILD = $(PACKAGE_TOOL) build
PACKAGE_INSTALL = $(PACKAGE_TOOL) install
{% else %}
PACKAGE_BUILD = build {{ cookiecutter.project_slug }}
PACKAGE_INSTALL = pip install -e {{ cookiecutter.project_slug }}
{% endif %}
PACKAGE_DEPENDENCIES = dev-requirements.txt

## -----------------
## -----------------
## Recipes
## -----------------
## -----------------

SHELL := /bin/bash
.PHONY: venv clean_venv compile_dev_dependencies install build
.SILENT: clean_venv

venv: # Install the virtual environment
	@echo "Creating the virtual enviroment"
	$(PYTHON_VERSION) -m venv $(VENV_NAME)
	source $(VENV_ACTIVATE) && pip install --upgrade pip && pip install pip-tools
	source $(VENV_ACTIVATE) && pip-sync $(PACKAGE_DEPENDENCIES)
	source $(VENV_ACTIVATE) && pre-commit install

clean_venv: # Clean the venv folder
	@echo "Deleting the environment"
	rm -r $(VENV_NAME)

compile_dev_dependencies: # Compiling dev requeriments file
	@echo "Compiling developing dependencies"
	$(VENV_ACTIVATE) && pip-compile -o $(PACKAGE_DEPENDENCIES) --extra dev --extra test pyproject.toml

install: # Install the package
	@echo "Installing the package: $(PROJECT_NAME)"
	$(PACKAGE_INSTALL)

build: # build the package
	@echo "Building the package: $(PROJECT_NAME)"
	$(PACKAGE_BUILD)

tests:
	@echo "Running tests"
	{% if cookiecutter.use_pytest == 'y' %}
	pytest tests
	{% else %}
	python -m unittest discover -s tests -p "test_*.py"

tox:
	@echo "Running tox"
	tox

clean_cache: # Clean repo from cache files
	@echo "Removing cache files"
	rm -r .mypy_cache
	rm -r .pytest_cache
	rm -r .tox