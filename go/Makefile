# Go Makefile - Modern Development Toolkit
# ===========================================================================
# A comprehensive Makefile for Go projects with colors and excellent UX
# ===========================================================================

# Project Configuration
# ---------------------------------------------------------------------------
BINARY_NAME := app
PACKAGE := .
MODULE := $(shell go list -m 2>/dev/null || echo "unknown")
VERSION := $(shell git describe --tags --always --dirty 2>/dev/null || echo "dev")
BUILD_TIME := $(shell date -u '+%Y-%m-%d_%H:%M:%S')
GIT_COMMIT := $(shell git rev-parse --short HEAD 2>/dev/null || echo "unknown")

# Build Configuration
# ---------------------------------------------------------------------------
BUILD_DIR := build
COVERAGE_DIR := coverage
MAIN_PATH := ./cmd/main.go
GO := go
GOFLAGS := -v
LDFLAGS := -ldflags "-X main.Version=$(VERSION) -X main.BuildTime=$(BUILD_TIME) -X main.GitCommit=$(GIT_COMMIT)"

# Color Configuration (ANSI escape codes)
# ---------------------------------------------------------------------------
COLOR_RESET   := \033[0m
COLOR_BOLD    := \033[1m
COLOR_DIM     := \033[2m
COLOR_RED     := \033[31m
COLOR_GREEN   := \033[32m
COLOR_YELLOW  := \033[33m
COLOR_BLUE    := \033[34m
COLOR_MAGENTA := \033[35m
COLOR_CYAN    := \033[36m
COLOR_WHITE   := \033[37m

# Symbols for better UX
# ---------------------------------------------------------------------------
SYMBOL_OK      := ✓
SYMBOL_ERROR   := ✗
SYMBOL_ARROW   := ➜
SYMBOL_INFO    := ℹ
SYMBOL_CLEAN   := 🧹
SYMBOL_BUILD   := 🔨
SYMBOL_TEST    := 🧪
SYMBOL_ROCKET  := 🚀
SYMBOL_PACKAGE := 📦
SYMBOL_SECURITY := 🔒
SYMBOL_DOC     := 📚

# Utility Functions
# ---------------------------------------------------------------------------
define print_header
	@echo "$(COLOR_BOLD)$(COLOR_CYAN)"
	@echo "═══════════════════════════════════════════════════════════════════════════"
	@echo "  $(1)"
	@echo "═══════════════════════════════════════════════════════════════════════════"
	@echo "$(COLOR_RESET)"
endef

define print_success
	@echo "$(COLOR_GREEN)$(SYMBOL_OK) $(1)$(COLOR_RESET)"
endef

define print_error
	@echo "$(COLOR_RED)$(SYMBOL_ERROR) $(1)$(COLOR_RESET)"
endef

define print_info
	@echo "$(COLOR_BLUE)$(SYMBOL_INFO) $(1)$(COLOR_RESET)"
endef

define print_step
	@echo "$(COLOR_YELLOW)$(SYMBOL_ARROW) $(1)$(COLOR_RESET)"
endef

# Default Target
# ---------------------------------------------------------------------------
.DEFAULT_GOAL := help

# Phony Targets
# ---------------------------------------------------------------------------
.PHONY: all help init build build-all install clean test test-verbose test-race \
		test-coverage test-coverage-html benchmark lint fmt vet staticcheck \
		security vuln mod-download mod-tidy mod-verify mod-vendor run docker-build \
		docker-run release version info deps-update deps-graph tools-install \
		pre-commit git-hooks doc godoc

# ===========================================================================
# MAIN TARGETS
# ===========================================================================

## all: Run fmt, vet, test, and build
all: fmt vet test build
	$(call print_success,"All tasks completed successfully!")

## help: Display this help message
help:
	@echo "$(COLOR_BOLD)$(COLOR_CYAN)"
	@echo "╔══════════════════════════════════════════════════════════════════════════╗"
	@echo "║                     Go Project Makefile - Help Menu                     ║"
	@echo "╚══════════════════════════════════════════════════════════════════════════╝"
	@echo "$(COLOR_RESET)"
	@echo ""
	@echo "$(COLOR_BOLD)$(COLOR_MAGENTA)📖 AVAILABLE TARGETS:$(COLOR_RESET)"
	@echo ""
	@sed -n 's/^##//p' ${MAKEFILE_LIST} | column -t -s ':' | sed -e 's/^/ $(COLOR_GREEN)$(SYMBOL_ARROW)$(COLOR_RESET) /'
	@echo ""
	@echo "$(COLOR_BOLD)$(COLOR_CYAN)📋 PROJECT INFO:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)Module:$(COLOR_RESET)      $(MODULE)"
	@echo "  $(COLOR_YELLOW)Version:$(COLOR_RESET)     $(VERSION)"
	@echo "  $(COLOR_YELLOW)Binary:$(COLOR_RESET)      $(BINARY_NAME)"
	@echo "  $(COLOR_YELLOW)Go Version:$(COLOR_RESET)  $$(go version | awk '{print $$3}')"
	@echo ""

# ===========================================================================
# BUILD TARGETS
# ===========================================================================

## init: Initialize a new Go module
init:
	$(call print_header,"$(SYMBOL_PACKAGE) Initializing Go Module")
	@read -p "Enter module name: " module; \
	$(GO) mod init $$module
	$(call print_success,"Module initialized")

## build: Build the application
build:
	$(call print_header,"$(SYMBOL_BUILD) Building Application")
	$(call print_step,"Creating build directory...")
	@mkdir -p $(BUILD_DIR)
	$(call print_step,"Building $(BINARY_NAME)...")
	@$(GO) build $(GOFLAGS) $(LDFLAGS) -o $(BUILD_DIR)/$(BINARY_NAME) $(PACKAGE)
	$(call print_success,"Build completed: $(BUILD_DIR)/$(BINARY_NAME)")
	@echo "$(COLOR_DIM)Version: $(VERSION) | Commit: $(GIT_COMMIT)$(COLOR_RESET)"

## build-all: Build for multiple platforms
build-all:
	$(call print_header,"$(SYMBOL_BUILD) Building for Multiple Platforms")
	@mkdir -p $(BUILD_DIR)
	$(call print_step,"Building for Linux AMD64...")
	@GOOS=linux GOARCH=amd64 $(GO) build $(LDFLAGS) -o $(BUILD_DIR)/$(BINARY_NAME)-linux-amd64 $(PACKAGE)
	$(call print_step,"Building for Linux ARM64...")
	@GOOS=linux GOARCH=arm64 $(GO) build $(LDFLAGS) -o $(BUILD_DIR)/$(BINARY_NAME)-linux-arm64 $(PACKAGE)
	$(call print_step,"Building for macOS AMD64...")
	@GOOS=darwin GOARCH=amd64 $(GO) build $(LDFLAGS) -o $(BUILD_DIR)/$(BINARY_NAME)-darwin-amd64 $(PACKAGE)
	$(call print_step,"Building for macOS ARM64...")
	@GOOS=darwin GOARCH=arm64 $(GO) build $(LDFLAGS) -o $(BUILD_DIR)/$(BINARY_NAME)-darwin-arm64 $(PACKAGE)
	$(call print_step,"Building for Windows AMD64...")
	@GOOS=windows GOARCH=amd64 $(GO) build $(LDFLAGS) -o $(BUILD_DIR)/$(BINARY_NAME)-windows-amd64.exe $(PACKAGE)
	$(call print_success,"Multi-platform build completed!")
	@ls -lh $(BUILD_DIR)/

## install: Install the application to $GOPATH/bin
install:
	$(call print_header,"$(SYMBOL_ROCKET) Installing Application")
	@$(GO) install $(LDFLAGS) $(PACKAGE)
	$(call print_success,"Installed to $$(go env GOPATH)/bin/$(BINARY_NAME)")

## run: Run the application
run:
	$(call print_header,"$(SYMBOL_ROCKET) Running Application")
	@$(GO) run $(PACKAGE)

# ===========================================================================
# TEST TARGETS
# ===========================================================================

## test: Run tests
test:
	$(call print_header,"$(SYMBOL_TEST) Running Tests")
	@$(GO) test ./... -v
	$(call print_success,"Tests completed")

## test-verbose: Run tests with verbose output
test-verbose:
	$(call print_header,"$(SYMBOL_TEST) Running Tests (Verbose)")
	@$(GO) test -v -count=1 ./...
	$(call print_success,"Verbose tests completed")

## test-race: Run tests with race detector
test-race:
	$(call print_header,"$(SYMBOL_TEST) Running Tests (Race Detector)")
	@$(GO) test -race -short ./...
	$(call print_success,"Race detector tests completed")

## test-coverage: Run tests with coverage
test-coverage:
	$(call print_header,"$(SYMBOL_TEST) Running Tests with Coverage")
	@mkdir -p $(COVERAGE_DIR)
	@$(GO) test -coverprofile=$(COVERAGE_DIR)/coverage.out ./...
	@$(GO) tool cover -func=$(COVERAGE_DIR)/coverage.out
	$(call print_success,"Coverage report generated: $(COVERAGE_DIR)/coverage.out")

## test-coverage-html: Generate HTML coverage report
test-coverage-html: test-coverage
	$(call print_header,"$(SYMBOL_TEST) Generating HTML Coverage Report")
	@$(GO) tool cover -html=$(COVERAGE_DIR)/coverage.out -o $(COVERAGE_DIR)/coverage.html
	$(call print_success,"HTML coverage report: $(COVERAGE_DIR)/coverage.html")
	@echo "$(COLOR_CYAN)Open: file://$(PWD)/$(COVERAGE_DIR)/coverage.html$(COLOR_RESET)"

## benchmark: Run benchmarks
benchmark:
	$(call print_header,"$(SYMBOL_TEST) Running Benchmarks")
	@$(GO) test -bench=. -benchmem ./...
	$(call print_success,"Benchmarks completed")

# ===========================================================================
# CODE QUALITY TARGETS
# ===========================================================================

## fmt: Format code using gofmt
fmt:
	$(call print_header,"🎨 Formatting Code")
	@$(GO) fmt ./...
	$(call print_success,"Code formatted")

## vet: Run go vet
vet:
	$(call print_header,"🔍 Running go vet")
	@$(GO) vet ./...
	$(call print_success,"Vet checks passed")

## lint: Run golangci-lint
lint:
	$(call print_header,"🔍 Running golangci-lint")
	@if command -v golangci-lint >/dev/null 2>&1; then \
		golangci-lint run ./...; \
		$(call print_success,"Linting completed"); \
	else \
		$(call print_error,"golangci-lint not installed. Run: make tools-install"); \
		exit 1; \
	fi

## staticcheck: Run staticcheck
staticcheck:
	$(call print_header,"🔍 Running staticcheck")
	@if command -v staticcheck >/dev/null 2>&1; then \
		staticcheck ./...; \
		$(call print_success,"Staticcheck completed"); \
	else \
		$(call print_error,"staticcheck not installed. Run: make tools-install"); \
		exit 1; \
	fi

## security: Run gosec security scanner
security:
	$(call print_header,"$(SYMBOL_SECURITY) Running Security Scan")
	@if command -v gosec >/dev/null 2>&1; then \
		gosec -fmt=colored ./...; \
		$(call print_success,"Security scan completed"); \
	else \
		$(call print_error,"gosec not installed. Run: make tools-install"); \
		exit 1; \
	fi

## vuln: Check for known vulnerabilities
vuln:
	$(call print_header,"$(SYMBOL_SECURITY) Checking for Vulnerabilities")
	@if command -v govulncheck >/dev/null 2>&1; then \
		govulncheck ./...; \
		$(call print_success,"Vulnerability check completed"); \
	else \
		$(call print_error,"govulncheck not installed. Run: go install golang.org/x/vuln/cmd/govulncheck@latest"); \
		exit 1; \
	fi

## pre-commit: Run all checks before committing
pre-commit: fmt vet lint test
	$(call print_success,"Pre-commit checks passed! Ready to commit.")

# ===========================================================================
# DEPENDENCY MANAGEMENT
# ===========================================================================

## mod-download: Download dependencies
mod-download:
	$(call print_header,"$(SYMBOL_PACKAGE) Downloading Dependencies")
	@$(GO) mod download
	$(call print_success,"Dependencies downloaded")

## mod-tidy: Tidy dependencies
mod-tidy:
	$(call print_header,"$(SYMBOL_PACKAGE) Tidying Dependencies")
	@$(GO) mod tidy
	$(call print_success,"Dependencies tidied")

## mod-verify: Verify dependencies
mod-verify:
	$(call print_header,"$(SYMBOL_PACKAGE) Verifying Dependencies")
	@$(GO) mod verify
	$(call print_success,"Dependencies verified")

## mod-vendor: Vendor dependencies
mod-vendor:
	$(call print_header,"$(SYMBOL_PACKAGE) Vendoring Dependencies")
	@$(GO) mod vendor
	$(call print_success,"Dependencies vendored to vendor/")

## deps-update: Update all dependencies
deps-update:
	$(call print_header,"$(SYMBOL_PACKAGE) Updating Dependencies")
	@$(GO) get -u ./...
	@$(GO) mod tidy
	$(call print_success,"Dependencies updated")

## deps-graph: Visualize dependency graph
deps-graph:
	$(call print_header,"$(SYMBOL_PACKAGE) Generating Dependency Graph")
	@$(GO) mod graph | grep -v "$(MODULE)" | head -20
	$(call print_info,"Showing top 20 external dependencies")

# ===========================================================================
# TOOLS & UTILITIES
# ===========================================================================

## tools-install: Install development tools
tools-install:
	$(call print_header,"🔧 Installing Development Tools")
	$(call print_step,"Installing golangci-lint...")
	@curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $$(go env GOPATH)/bin latest
	$(call print_step,"Installing staticcheck...")
	@go install honnef.co/go/tools/cmd/staticcheck@latest
	$(call print_step,"Installing gosec...")
	@go install github.com/securego/gosec/v2/cmd/gosec@latest
	$(call print_step,"Installing govulncheck...")
	@go install golang.org/x/vuln/cmd/govulncheck@latest
	$(call print_step,"Installing goimports...")
	@go install golang.org/x/tools/cmd/goimports@latest
	$(call print_step,"Installing godoc...")
	@go install golang.org/x/tools/cmd/godoc@latest
	$(call print_success,"All development tools installed!")

## git-hooks: Install git hooks
git-hooks:
	$(call print_header,"🔗 Installing Git Hooks")
	@echo '#!/bin/bash\nmake pre-commit' > .git/hooks/pre-commit
	@chmod +x .git/hooks/pre-commit
	$(call print_success,"Git pre-commit hook installed")

## doc: Generate and serve documentation
doc:
	$(call print_header,"$(SYMBOL_DOC) Generating Documentation")
	@$(GO) doc -all
	$(call print_success,"Documentation generated")

## godoc: Start godoc server
godoc:
	$(call print_header,"$(SYMBOL_DOC) Starting godoc Server")
	@if command -v godoc >/dev/null 2>&1; then \
		echo "$(COLOR_CYAN)Opening godoc at http://localhost:6060/pkg/$(MODULE)$(COLOR_RESET)"; \
		godoc -http=:6060; \
	else \
		$(call print_error,"godoc not installed. Run: make tools-install"); \
		exit 1; \
	fi

# ===========================================================================
# DOCKER TARGETS
# ===========================================================================

## docker-build: Build Docker image
docker-build:
	$(call print_header,"🐳 Building Docker Image")
	@docker build -t $(BINARY_NAME):$(VERSION) -t $(BINARY_NAME):latest .
	$(call print_success,"Docker image built: $(BINARY_NAME):$(VERSION)")

## docker-run: Run Docker container
docker-run:
	$(call print_header,"🐳 Running Docker Container")
	@docker run --rm -it $(BINARY_NAME):latest

# ===========================================================================
# CLEANUP TARGETS
# ===========================================================================

## clean: Remove build artifacts
clean:
	$(call print_header,"$(SYMBOL_CLEAN) Cleaning Build Artifacts")
	@rm -rf $(BUILD_DIR)
	@rm -rf $(COVERAGE_DIR)
	@rm -rf vendor/
	@$(GO) clean -cache -testcache -modcache
	$(call print_success,"Cleanup completed")

## clean-light: Remove build artifacts (keep caches)
clean-light:
	$(call print_header,"$(SYMBOL_CLEAN) Light Cleanup")
	@rm -rf $(BUILD_DIR)
	@rm -rf $(COVERAGE_DIR)
	$(call print_success,"Light cleanup completed")

# ===========================================================================
# INFORMATION TARGETS
# ===========================================================================

## version: Display version information
version:
	@echo "$(COLOR_BOLD)$(COLOR_CYAN)Version Information:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)Version:$(COLOR_RESET)    $(VERSION)"
	@echo "  $(COLOR_YELLOW)Commit:$(COLOR_RESET)     $(GIT_COMMIT)"
	@echo "  $(COLOR_YELLOW)Built:$(COLOR_RESET)      $(BUILD_TIME)"
	@echo "  $(COLOR_YELLOW)Module:$(COLOR_RESET)     $(MODULE)"

## info: Display project information
info:
	$(call print_header,"📊 Project Information")
	@echo "$(COLOR_BOLD)Go Environment:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)Go Version:$(COLOR_RESET)   $$(go version | awk '{print $$3}')"
	@echo "  $(COLOR_YELLOW)GOPATH:$(COLOR_RESET)       $$(go env GOPATH)"
	@echo "  $(COLOR_YELLOW)GOROOT:$(COLOR_RESET)       $$(go env GOROOT)"
	@echo "  $(COLOR_YELLOW)GOOS:$(COLOR_RESET)         $$(go env GOOS)"
	@echo "  $(COLOR_YELLOW)GOARCH:$(COLOR_RESET)       $$(go env GOARCH)"
	@echo ""
	@echo "$(COLOR_BOLD)Project Details:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)Module:$(COLOR_RESET)       $(MODULE)"
	@echo "  $(COLOR_YELLOW)Version:$(COLOR_RESET)      $(VERSION)"
	@echo "  $(COLOR_YELLOW)Binary Name:$(COLOR_RESET)  $(BINARY_NAME)"
	@echo "  $(COLOR_YELLOW)Build Dir:$(COLOR_RESET)    $(BUILD_DIR)"
	@echo ""
	@echo "$(COLOR_BOLD)Statistics:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)Go Files:$(COLOR_RESET)     $$(find . -name '*.go' -not -path './vendor/*' | wc -l)"
	@echo "  $(COLOR_YELLOW)Lines of Code:$(COLOR_RESET) $$(find . -name '*.go' -not -path './vendor/*' -exec cat {} \; | wc -l)"
	@echo "  $(COLOR_YELLOW)Dependencies:$(COLOR_RESET)  $$(go list -m all 2>/dev/null | wc -l)"
