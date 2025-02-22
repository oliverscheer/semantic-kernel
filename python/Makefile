SHELL = /bin/bash

.PHONY: help install clean build
.SILENT:
all: install

ifeq ($(PYTHON_VERSION),)
    PYTHON_VERSION="3.10"
endif

.ONESHELL:
help:
	echo -e "\033[1mUSAGE:\033[0m"
	echo "  make [target]"
	echo ""
	echo -e "\033[1mTARGETS:\033[0m"
	echo "  help                 - show this help message"
	echo "  install              - install uv, python, Semantic Kernel and all dependencies"
	echo "      This is the default and will use Python 3.10."
	echo "  install-uv           - install uv"
	echo "  install-python       - install python distributions"
	echo "  install-sk           - install Semantic Kernel and all dependencies"
	echo "  install-pre-commit   - install pre-commit hooks"
	echo "  clean                - remove the virtualenvs"
	echo "  build                - build the project"
	echo ""
	echo -e "\033[1mVARIABLES:\033[0m"
	echo "  PYTHON_VERSION       - Python version to use. Default is 3.10"
	echo "      By default, 3.10, 3.11 and 3.12 are installed as well."

install:
	make install-uv
	make install-python
	make install-sk
	make install-pre-commit

UV_VERSION := $(shell command -v uv 2> /dev/null)
install-uv:
# Check if uv is installed
ifndef UV_VERSION
	echo "uv could not be found"
	echo "Installing uv"
	curl -LsSf https://astral.sh/uv/install.sh | sh
	echo "uv installed"
	echo "Please restart your shell."
	exit 1
else
	echo "uv found $(UV_VERSION)"
	echo "running uv update"
	uv self update 
endif

.ONESHELL:
install-python:
	echo "Installing python 3.10, 3.11, 3.12"
	uv python install 3.10 3.11 3.12

.ONESHELL:
install-pre-commit:
	echo "Installing pre-commit hooks"
	uv run pre-commit install -c python/.pre-commit-config.yaml


.ONESHELL:
install-sk:
	echo "Creating and activating venv for python $(PYTHON_VERSION)"
	uv venv --python $(PYTHON_VERSION)
	echo "Installing Semantic Kernel and all dependencies"
	uv sync --all-extras --dev
	
.ONESHELL:
clean:
	# Remove the virtualenv
	rm -rf .venv

build:
	uvx --from build pyproject-build --installer uv
