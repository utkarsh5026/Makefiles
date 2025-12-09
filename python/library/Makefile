# Python Library/Package Makefile
# Comprehensive makefile for Python library development and PyPI publishing
#
# Copy this file to your library project root as 'Makefile'
# Requires: Python 3.10+, Poetry

include ../common/Makefile.common

# ==================================================================================== #
# LIBRARY-SPECIFIC VARIABLES
# ==================================================================================== #

# Package name from pyproject.toml
PACKAGE_NAME := $(shell $(POETRY) version 2>/dev/null | cut -d' ' -f1 || echo "unknown")
PACKAGE_VERSION := $(shell $(POETRY) version -s 2>/dev/null || echo "0.0.1")

# ==================================================================================== #
# HELP
# ==================================================================================== #

.PHONY: help
help:
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)"
	@echo "╔════════════════════════════════════════════════════════════════╗"
	@echo "║          Python Library Makefile Commands                      ║"
	@echo "╚════════════════════════════════════════════════════════════════╝"
	@echo "$(COLOR_RESET)"
	@echo "$(COLOR_GREEN)$(EMOJI_BUILD) Build Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)build$(COLOR_RESET)             - Build wheel and sdist"
	@echo "  $(COLOR_YELLOW)build-wheel$(COLOR_RESET)       - Build wheel only"
	@echo "  $(COLOR_YELLOW)build-sdist$(COLOR_RESET)       - Build source distribution only"
	@echo "  $(COLOR_YELLOW)check-package$(COLOR_RESET)     - Validate package"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_PACKAGE) Publishing:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)publish$(COLOR_RESET)           - Publish to PyPI"
	@echo "  $(COLOR_YELLOW)publish-test$(COLOR_RESET)      - Publish to TestPyPI"
	@echo "  $(COLOR_YELLOW)install-local$(COLOR_RESET)     - Install package locally"
	@echo ""
	@echo "$(COLOR_GREEN)📌 Versioning:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)bump-patch$(COLOR_RESET)        - Bump patch version (x.x.1)"
	@echo "  $(COLOR_YELLOW)bump-minor$(COLOR_RESET)        - Bump minor version (x.1.0)"
	@echo "  $(COLOR_YELLOW)bump-major$(COLOR_RESET)        - Bump major version (1.0.0)"
	@echo "  $(COLOR_YELLOW)version$(COLOR_RESET)           - Show current version"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_DOCS) Documentation:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)docs-build$(COLOR_RESET)        - Build documentation"
	@echo "  $(COLOR_YELLOW)docs-serve$(COLOR_RESET)        - Serve documentation"
	@echo "  $(COLOR_YELLOW)api-docs$(COLOR_RESET)          - Generate API documentation"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_TEST) Testing:$(COLOR_RESET) test, test-cov, test-all-python"
	@echo "$(COLOR_GREEN)$(EMOJI_LINT) Quality:$(COLOR_RESET) format, lint, typecheck"
	@echo ""
	@echo "$(COLOR_CYAN)Package:$(COLOR_RESET) $(PACKAGE_NAME) v$(PACKAGE_VERSION)"
	@echo ""

.DEFAULT_GOAL := help

# ==================================================================================== #
# BUILD COMMANDS
# ==================================================================================== #

## build: Build wheel and sdist
.PHONY: build
build:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building package...$(COLOR_RESET)"
	@poetry build
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Package built: dist/$(COLOR_RESET)"
	@ls -lh dist/

## build-wheel: Build wheel only
.PHONY: build-wheel
build-wheel:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building wheel...$(COLOR_RESET)"
	@poetry build -f wheel
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Wheel built!$(COLOR_RESET)"

## build-sdist: Build source distribution only
.PHONY: build-sdist
build-sdist:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building sdist...$(COLOR_RESET)"
	@poetry build -f sdist
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Source distribution built!$(COLOR_RESET)"

## check-package: Validate package with twine
.PHONY: check-package
check-package: build
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Checking package...$(COLOR_RESET)"
	@if $(POETRY_RUN) which twine > /dev/null 2>&1; then \
		$(POETRY_RUN) twine check dist/*; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Package is valid!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) twine not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev twine$(COLOR_RESET)"; \
	fi

# ==================================================================================== #
# PUBLISHING
# ==================================================================================== #

## publish: Publish to PyPI
.PHONY: publish
publish: clean-build build check-package
	$(call check_poetry)
	@echo "$(COLOR_RED)$(EMOJI_WARNING) Publishing to PyPI...$(COLOR_RESET)"
	@read -p "Are you sure? [y/N] " -n 1 -r; \
	echo; \
	if [[ $$REPLY =~ ^[Yy]$$ ]]; then \
		poetry publish; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Published to PyPI!$(COLOR_RESET)"; \
	fi

## publish-test: Publish to TestPyPI
.PHONY: publish-test
publish-test: clean-build build check-package
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Publishing to TestPyPI...$(COLOR_RESET)"
	@poetry publish -r testpypi
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Published to TestPyPI!$(COLOR_RESET)"
	@echo "$(COLOR_CYAN)Test install: pip install --index-url https://test.pypi.org/simple/ $(PACKAGE_NAME)$(COLOR_RESET)"

## install-local: Install package locally in editable mode
.PHONY: install-local
install-local:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Installing package locally...$(COLOR_RESET)"
	@poetry install
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Package installed!$(COLOR_RESET)"

# ==================================================================================== #
# VERSIONING
# ==================================================================================== #

## bump-patch: Bump patch version
.PHONY: bump-patch
bump-patch:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)📌 Bumping patch version...$(COLOR_RESET)"
	@poetry version patch
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Version: $(shell poetry version -s)$(COLOR_RESET)"

## bump-minor: Bump minor version
.PHONY: bump-minor
bump-minor:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)📌 Bumping minor version...$(COLOR_RESET)"
	@poetry version minor
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Version: $(shell poetry version -s)$(COLOR_RESET)"

## bump-major: Bump major version
.PHONY: bump-major
bump-major:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)📌 Bumping major version...$(COLOR_RESET)"
	@poetry version major
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Version: $(shell poetry version -s)$(COLOR_RESET)"

## version: Show current version
.PHONY: version
version:
	$(call check_poetry)
	@poetry version

# ==================================================================================== #
# DOCUMENTATION
# ==================================================================================== #

## api-docs: Generate API documentation
.PHONY: api-docs
api-docs:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_DOCS) Generating API documentation...$(COLOR_RESET)"
	@if $(POETRY_RUN) which sphinx-apidoc > /dev/null 2>&1; then \
		$(POETRY_RUN) sphinx-apidoc -o docs/api $(PACKAGE_NAME); \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) API docs generated!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) sphinx not installed$(COLOR_RESET)"; \
	fi

## test-all-python: Test against multiple Python versions with tox
.PHONY: test-all-python
test-all-python:
	$(call check_poetry)
	@if $(POETRY_RUN) which tox > /dev/null 2>&1; then \
		echo "$(COLOR_BLUE)$(EMOJI_TEST) Testing with tox...$(COLOR_RESET)"; \
		$(POETRY_RUN) tox; \
	else \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) tox not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev tox$(COLOR_RESET)"; \
	fi

## release: Full release workflow
.PHONY: release
release: ci bump-patch build publish
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) Release complete!$(COLOR_RESET)"
