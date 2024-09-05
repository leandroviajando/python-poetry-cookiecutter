SHELL          := /usr/bin/env bash
MAKEFLAGS      += --silent

-include .env
export

NOTEBOOKS_DIR  := notebooks
SRC_DIR        := src
TEST_DIR       := tests
CHECK_DIRS     := $(SRC_DIR) $(TEST_DIR)
PYTEST_FLAGS   := --no-header

all: help

.PHONY: help
help: ## Show the available commands
	@echo "Available commands:"
	@grep -h -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: install
install: ## Install dependencies
	poetry config virtualenvs.in-project true
	poetry run pip install --upgrade pip
	poetry install --no-root

.PHONY: update
update: ## Update python dependencies
	poetry update -vvv

.PHONY: install-pre-commit
install-pre-commit: install ## Install pre-commit hooks
	poetry run pre-commit install
	poetry run pre-commit autoupdate

.PHONY: format
format: ## Format repository code
	poetry run black $(CHECK_DIRS)
	poetry run isort $(CHECK_DIRS)

.PHONY: check
check: format-check lint type-check test ## Launch all checks to approve the code

.PHONY: format-check
format-check: ## Check the code format
	poetry run black --check $(CHECK_DIRS)
	poetry run isort --check $(CHECK_DIRS)

.PHONY: lint
lint: ## Launch the linting tool
	poetry run ruff check $(CHECK_DIRS)

.PHONY: lint-fix
lint-fix: ## Launch the linting tool, and auto-fix the issues where possible
	poetry run ruff check --fix $(CHECK_DIRS)

.PHONY: type-check
type-check: ## Launch the type checking tool
	poetry run mypy $(CHECK_DIRS)

.PHONY: test
test: ## Launch basic (fast) tests
	poetry run pytest $(PYTEST_FLAGS) $(TEST_DIR)

.PHONY: test-coverage
test-coverage: ## Launch basic (fast) tests with coverage
	poetry run pytest $(PYTEST_FLAGS) $(TEST_DIR) --cov-fail-under=80 --no-cov-on-fail

.PHONY: clean
clean: ## Clean the repository
	rm -rf dist
	rm -rf .coverage
	rm -rf *.egg-info
	find . -type f -name *.DS_Store -ls -delete
	find . | grep -E '(__pycache__|\.pyc|\.pyo)' | xargs rm -rf
	find . | grep -E .mypy_cache | xargs rm -rf
	find . | grep -E .pytest_cache | xargs rm -rf
	find . | grep -E .ruff_cache | xargs rm -rf
	find . | grep -E .ipynb_checkpoints | xargs rm -rf
	find . | grep -E .trash | xargs rm -rf
