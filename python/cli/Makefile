# CLI Application Makefile
# Comprehensive makefile for CLI tool development
#
# Copy this file to your CLI project root as 'Makefile'
# Requires: Python 3.10+, Poetry

include ../common/Makefile.common

# ==================================================================================== #
# CLI-SPECIFIC VARIABLES
# ==================================================================================== #

CLI_NAME?=$(PROJECT_NAME)
CLI_MODULE?=cli.main
ENTRY_POINT?=$(CLI_MODULE):main

# ==================================================================================== #
# HELP
# ==================================================================================== #

.PHONY: help
help:
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)"
	@echo "╔════════════════════════════════════════════════════════════════╗"
	@echo "║          CLI Application Makefile Commands                     ║"
	@echo "╚════════════════════════════════════════════════════════════════╝"
	@echo "$(COLOR_RESET)"
	@echo "$(COLOR_GREEN)$(EMOJI_ROCKET) CLI Development:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)cli-run$(COLOR_RESET)           - Run CLI (specify ARGS=\"--help\")"
	@echo "  $(COLOR_YELLOW)cli-help$(COLOR_RESET)          - Display CLI help"
	@echo "  $(COLOR_YELLOW)cli-test-args$(COLOR_RESET)     - Test CLI with sample arguments"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_BUILD) Building:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)build-binary$(COLOR_RESET)      - Build standalone executable (PyInstaller)"
	@echo "  $(COLOR_YELLOW)build-linux$(COLOR_RESET)       - Build for Linux"
	@echo "  $(COLOR_YELLOW)build-windows$(COLOR_RESET)     - Build for Windows"
	@echo "  $(COLOR_YELLOW)build-mac$(COLOR_RESET)         - Build for macOS"
	@echo "  $(COLOR_YELLOW)build-all$(COLOR_RESET)         - Build for all platforms"
	@echo ""
	@echo "$(COLOR_GREEN)⚙️  Shell Integration:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)gen-completion$(COLOR_RESET)    - Generate shell completion scripts"
	@echo "  $(COLOR_YELLOW)install-completion$(COLOR_RESET) - Install shell completions"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_PACKAGE) Distribution:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)install-local$(COLOR_RESET)     - Install CLI locally for testing"
	@echo "  $(COLOR_YELLOW)uninstall$(COLOR_RESET)         - Uninstall local CLI"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_TEST) Testing:$(COLOR_RESET) test, test-cov, test-integration"
	@echo "$(COLOR_GREEN)$(EMOJI_LINT) Quality:$(COLOR_RESET) format, lint, typecheck"
	@echo ""

.DEFAULT_GOAL := help

# ==================================================================================== #
# CLI DEVELOPMENT
# ==================================================================================== #

## cli-run: Run CLI application
.PHONY: cli-run
cli-run:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_ROCKET) Running CLI...$(COLOR_RESET)"
	@$(POETRY_RUN) python -m $(CLI_MODULE) $(ARGS)

## cli-help: Display CLI help
.PHONY: cli-help
cli-help:
	$(call check_poetry)
	@$(POETRY_RUN) python -m $(CLI_MODULE) --help

## cli-test-args: Test CLI with sample arguments
.PHONY: cli-test-args
cli-test-args:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Testing CLI...$(COLOR_RESET)"
	@$(POETRY_RUN) python -m $(CLI_MODULE) --version
	@$(POETRY_RUN) python -m $(CLI_MODULE) --help

# ==================================================================================== #
# BUILDING EXECUTABLES
# ==================================================================================== #

## build-binary: Build standalone executable with PyInstaller
.PHONY: build-binary
build-binary:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which pyinstaller > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) PyInstaller not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev pyinstaller$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building executable...$(COLOR_RESET)"
	@$(POETRY_RUN) pyinstaller --onefile --name $(CLI_NAME) $(CLI_MODULE).py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Binary built: dist/$(CLI_NAME)$(COLOR_RESET)"

## build-linux: Build for Linux
.PHONY: build-linux
build-linux:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for Linux...$(COLOR_RESET)"
	@$(POETRY_RUN) pyinstaller --onefile --name $(CLI_NAME)-linux $(CLI_MODULE).py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Linux binary built!$(COLOR_RESET)"

## build-windows: Build for Windows
.PHONY: build-windows
build-windows:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for Windows...$(COLOR_RESET)"
	@$(POETRY_RUN) pyinstaller --onefile --name $(CLI_NAME).exe $(CLI_MODULE).py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Windows binary built!$(COLOR_RESET)"

## build-mac: Build for macOS
.PHONY: build-mac
build-mac:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for macOS...$(COLOR_RESET)"
	@$(POETRY_RUN) pyinstaller --onefile --name $(CLI_NAME)-macos $(CLI_MODULE).py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) macOS binary built!$(COLOR_RESET)"

## build-all: Build for all platforms
.PHONY: build-all
build-all: build-linux build-windows build-mac
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) All binaries built!$(COLOR_RESET)"

# ==================================================================================== #
# SHELL INTEGRATION
# ==================================================================================== #

## gen-completion: Generate shell completion scripts
.PHONY: gen-completion
gen-completion:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)⚙️  Generating shell completions...$(COLOR_RESET)"
	@mkdir -p completions
	@if $(POETRY_RUN) python -c "import click" 2>/dev/null; then \
		echo "Bash completion..."; \
		_$(CLI_NAME)_COMPLETE=bash_source $(POETRY_RUN) python -m $(CLI_MODULE) > completions/$(CLI_NAME).bash || true; \
		echo "Zsh completion..."; \
		_$(CLI_NAME)_COMPLETE=zsh_source $(POETRY_RUN) python -m $(CLI_MODULE) > completions/$(CLI_NAME).zsh || true; \
		echo "Fish completion..."; \
		_$(CLI_NAME)_COMPLETE=fish_source $(POETRY_RUN) python -m $(CLI_MODULE) > completions/$(CLI_NAME).fish || true; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Completions generated in completions/$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) Click not detected. Shell completion generation may not work.$(COLOR_RESET)"; \
	fi

## install-completion: Install shell completions
.PHONY: install-completion
install-completion: gen-completion
	@echo "$(COLOR_BLUE)⚙️  Installing completions...$(COLOR_RESET)"
	@if [ -f completions/$(CLI_NAME).bash ]; then \
		cp completions/$(CLI_NAME).bash ~/.bash_completions/$(CLI_NAME) || \
		echo "Add to ~/.bashrc: source completions/$(CLI_NAME).bash"; \
	fi
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Installation complete!$(COLOR_RESET)"

# ==================================================================================== #
# DISTRIBUTION
# ==================================================================================== #

## install-local: Install CLI locally
.PHONY: install-local
install-local:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Installing CLI locally...$(COLOR_RESET)"
	@poetry install
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) CLI installed! Try: $(CLI_NAME) --help$(COLOR_RESET)"

## uninstall: Uninstall local CLI
.PHONY: uninstall
uninstall:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Uninstalling CLI...$(COLOR_RESET)"
	@pip uninstall -y $(CLI_NAME)
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) CLI uninstalled!$(COLOR_RESET)"

## package-check: Check if CLI runs after packaging
.PHONY: package-check
package-check: build-binary
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Testing packaged CLI...$(COLOR_RESET)"
	@./dist/$(CLI_NAME) --version
	@./dist/$(CLI_NAME) --help
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Package check passed!$(COLOR_RESET)"
