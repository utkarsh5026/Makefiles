# Go Project Makefile
# Comprehensive makefile for modern Go development

# ==================================================================================== #
# VARIABLES
# ==================================================================================== #

# Binary name
BINARY_NAME=app
BINARY_UNIX=$(BINARY_NAME)_unix
BINARY_LINUX=$(BINARY_NAME)_linux

# Go related variables
GOBASE=$(shell pwd)
GOBIN=$(GOBASE)/bin
GOFILES=$(wildcard *.go)

# Version information
VERSION?=0.0.1
BUILD_TIME=$(shell date -u '+%Y-%m-%d_%H:%M:%S')
GIT_COMMIT=$(shell git rev-parse --short HEAD 2>/dev/null || echo "unknown")
GIT_BRANCH=$(shell git rev-parse --abbrev-ref HEAD 2>/dev/null || echo "unknown")

# ANSI Color codes for pretty output
COLOR_RESET=\033[0m
COLOR_BOLD=\033[1m
COLOR_RED=\033[31m
COLOR_GREEN=\033[32m
COLOR_YELLOW=\033[33m
COLOR_BLUE=\033[34m
COLOR_MAGENTA=\033[35m
COLOR_CYAN=\033[36m
COLOR_WHITE=\033[37m

# Emojis for better visual feedback
EMOJI_CHECK=✅
EMOJI_BUILD=🔨
EMOJI_TEST=🧪
EMOJI_CLEAN=🧹
EMOJI_ROCKET=🚀
EMOJI_PACKAGE=📦
EMOJI_DOCS=📚
EMOJI_SECURITY=🔒
EMOJI_LINT=🔍
EMOJI_BENCH=⚡
EMOJI_DOCKER=🐳

# ==================================================================================== #
# HELPERS
# ==================================================================================== #

## help: Display this help message
.PHONY: help
help:
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)"
	@echo "╔════════════════════════════════════════════════════════════════╗"
	@echo "║              Go Project Makefile Commands                      ║"
	@echo "╚════════════════════════════════════════════════════════════════╝"
	@echo "$(COLOR_RESET)"
	@echo "$(COLOR_GREEN)$(EMOJI_BUILD) Build Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)build$(COLOR_RESET)              - Build the application binary"
	@echo "  $(COLOR_YELLOW)build-linux$(COLOR_RESET)        - Build for Linux AMD64"
	@echo "  $(COLOR_YELLOW)build-windows$(COLOR_RESET)      - Build for Windows AMD64"
	@echo "  $(COLOR_YELLOW)build-mac$(COLOR_RESET)          - Build for MacOS (Intel)"
	@echo "  $(COLOR_YELLOW)build-mac-arm$(COLOR_RESET)      - Build for MacOS (ARM/M1)"
	@echo "  $(COLOR_YELLOW)build-all$(COLOR_RESET)          - Build for all platforms"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_TEST) Test Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)test$(COLOR_RESET)               - Run all tests"
	@echo "  $(COLOR_YELLOW)test-verbose$(COLOR_RESET)       - Run tests with verbose output"
	@echo "  $(COLOR_YELLOW)test-race$(COLOR_RESET)          - Run tests with race detector"
	@echo "  $(COLOR_YELLOW)test-cover$(COLOR_RESET)         - Run tests with coverage report"
	@echo "  $(COLOR_YELLOW)test-cover-html$(COLOR_RESET)    - Generate HTML coverage report"
	@echo "  $(COLOR_YELLOW)test-integration$(COLOR_RESET)   - Run integration tests"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_LINT) Code Quality Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)lint$(COLOR_RESET)               - Run golangci-lint"
	@echo "  $(COLOR_YELLOW)lint-fix$(COLOR_RESET)           - Run golangci-lint with auto-fix"
	@echo "  $(COLOR_YELLOW)fmt$(COLOR_RESET)                - Format code with gofmt"
	@echo "  $(COLOR_YELLOW)fmt-check$(COLOR_RESET)          - Check code formatting"
	@echo "  $(COLOR_YELLOW)vet$(COLOR_RESET)                - Run go vet"
	@echo "  $(COLOR_YELLOW)staticcheck$(COLOR_RESET)        - Run staticcheck"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_PACKAGE) Dependency Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)deps$(COLOR_RESET)               - Download dependencies"
	@echo "  $(COLOR_YELLOW)deps-update$(COLOR_RESET)        - Update dependencies"
	@echo "  $(COLOR_YELLOW)deps-tidy$(COLOR_RESET)          - Tidy dependencies"
	@echo "  $(COLOR_YELLOW)deps-verify$(COLOR_RESET)        - Verify dependencies"
	@echo "  $(COLOR_YELLOW)deps-graph$(COLOR_RESET)         - Show dependency graph"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_ROCKET) Run Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)run$(COLOR_RESET)                - Run the application"
	@echo "  $(COLOR_YELLOW)watch$(COLOR_RESET)              - Watch and auto-reload on changes"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_BENCH) Performance Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)bench$(COLOR_RESET)              - Run benchmarks"
	@echo "  $(COLOR_YELLOW)bench-compare$(COLOR_RESET)      - Compare benchmark results"
	@echo "  $(COLOR_YELLOW)profile$(COLOR_RESET)            - Generate CPU profile"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_SECURITY) Security Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)security$(COLOR_RESET)           - Run security checks"
	@echo "  $(COLOR_YELLOW)vuln-check$(COLOR_RESET)         - Check for vulnerabilities"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_DOCKER) Docker Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)docker-build$(COLOR_RESET)       - Build Docker image"
	@echo "  $(COLOR_YELLOW)docker-run$(COLOR_RESET)         - Run Docker container"
	@echo "  $(COLOR_YELLOW)docker-push$(COLOR_RESET)        - Push Docker image"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_DOCS) Documentation Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)docs$(COLOR_RESET)               - Generate documentation"
	@echo "  $(COLOR_YELLOW)docs-serve$(COLOR_RESET)         - Serve documentation locally"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_CLEAN) Cleanup Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)clean$(COLOR_RESET)              - Remove build artifacts"
	@echo "  $(COLOR_YELLOW)clean-all$(COLOR_RESET)          - Remove all generated files"
	@echo ""
	@echo "$(COLOR_GREEN)🛠️  Setup Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)install-tools$(COLOR_RESET)      - Install development tools"
	@echo "  $(COLOR_YELLOW)init$(COLOR_RESET)               - Initialize new Go module"
	@echo ""
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)Version:$(COLOR_RESET) $(VERSION)"
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)Git Commit:$(COLOR_RESET) $(GIT_COMMIT)"
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)Git Branch:$(COLOR_RESET) $(GIT_BRANCH)"
	@echo ""

# Default target
.DEFAULT_GOAL := help

# ==================================================================================== #
# BUILD COMMANDS
# ==================================================================================== #

## build: Build the application binary
.PHONY: build
build:
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building application...$(COLOR_RESET)"
	@go build -ldflags="-X 'main.Version=$(VERSION)' -X 'main.BuildTime=$(BUILD_TIME)' -X 'main.GitCommit=$(GIT_COMMIT)'" -o $(GOBIN)/$(BINARY_NAME) .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Build complete: $(GOBIN)/$(BINARY_NAME)$(COLOR_RESET)"

## build-linux: Build for Linux AMD64
.PHONY: build-linux
build-linux:
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for Linux AMD64...$(COLOR_RESET)"
	@GOOS=linux GOARCH=amd64 go build -ldflags="-X 'main.Version=$(VERSION)' -X 'main.BuildTime=$(BUILD_TIME)' -X 'main.GitCommit=$(GIT_COMMIT)'" -o $(GOBIN)/$(BINARY_NAME)-linux-amd64 .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Linux build complete!$(COLOR_RESET)"

## build-windows: Build for Windows AMD64
.PHONY: build-windows
build-windows:
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for Windows AMD64...$(COLOR_RESET)"
	@GOOS=windows GOARCH=amd64 go build -ldflags="-X 'main.Version=$(VERSION)' -X 'main.BuildTime=$(BUILD_TIME)' -X 'main.GitCommit=$(GIT_COMMIT)'" -o $(GOBIN)/$(BINARY_NAME)-windows-amd64.exe .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Windows build complete!$(COLOR_RESET)"

## build-mac: Build for MacOS Intel
.PHONY: build-mac
build-mac:
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for MacOS Intel...$(COLOR_RESET)"
	@GOOS=darwin GOARCH=amd64 go build -ldflags="-X 'main.Version=$(VERSION)' -X 'main.BuildTime=$(BUILD_TIME)' -X 'main.GitCommit=$(GIT_COMMIT)'" -o $(GOBIN)/$(BINARY_NAME)-darwin-amd64 .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) MacOS Intel build complete!$(COLOR_RESET)"

## build-mac-arm: Build for MacOS ARM (M1/M2)
.PHONY: build-mac-arm
build-mac-arm:
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Building for MacOS ARM...$(COLOR_RESET)"
	@GOOS=darwin GOARCH=arm64 go build -ldflags="-X 'main.Version=$(VERSION)' -X 'main.BuildTime=$(BUILD_TIME)' -X 'main.GitCommit=$(GIT_COMMIT)'" -o $(GOBIN)/$(BINARY_NAME)-darwin-arm64 .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) MacOS ARM build complete!$(COLOR_RESET)"

## build-all: Build for all platforms
.PHONY: build-all
build-all: build-linux build-windows build-mac build-mac-arm
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) All platform builds complete!$(COLOR_RESET)"

# ==================================================================================== #
# TEST COMMANDS
# ==================================================================================== #

## test: Run all tests
.PHONY: test
test:
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running tests...$(COLOR_RESET)"
	@go test ./... -timeout 30s
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Tests completed!$(COLOR_RESET)"

## test-verbose: Run tests with verbose output
.PHONY: test-verbose
test-verbose:
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running tests (verbose)...$(COLOR_RESET)"
	@go test -v ./... -timeout 30s
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Tests completed!$(COLOR_RESET)"

## test-race: Run tests with race detector
.PHONY: test-race
test-race:
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running tests with race detector...$(COLOR_RESET)"
	@go test -race ./... -timeout 60s
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Race tests completed!$(COLOR_RESET)"

## test-cover: Run tests with coverage
.PHONY: test-cover
test-cover:
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running tests with coverage...$(COLOR_RESET)"
	@go test -coverprofile=coverage.out ./... -timeout 30s
	@go tool cover -func=coverage.out
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Coverage report generated!$(COLOR_RESET)"

## test-cover-html: Generate HTML coverage report
.PHONY: test-cover-html
test-cover-html: test-cover
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Generating HTML coverage report...$(COLOR_RESET)"
	@go tool cover -html=coverage.out -o coverage.html
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) HTML coverage report: coverage.html$(COLOR_RESET)"

## test-integration: Run integration tests
.PHONY: test-integration
test-integration:
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running integration tests...$(COLOR_RESET)"
	@go test -tags=integration ./... -timeout 60s
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Integration tests completed!$(COLOR_RESET)"

# ==================================================================================== #
# CODE QUALITY COMMANDS
# ==================================================================================== #

## lint: Run golangci-lint
.PHONY: lint
lint:
	@echo "$(COLOR_BLUE)$(EMOJI_LINT) Running linter...$(COLOR_RESET)"
	@if command -v golangci-lint > /dev/null; then \
		golangci-lint run ./...; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Linting completed!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_RED)❌ golangci-lint not installed. Run 'make install-tools'$(COLOR_RESET)"; \
		exit 1; \
	fi

## lint-fix: Run golangci-lint with auto-fix
.PHONY: lint-fix
lint-fix:
	@echo "$(COLOR_BLUE)$(EMOJI_LINT) Running linter with auto-fix...$(COLOR_RESET)"
	@if command -v golangci-lint > /dev/null; then \
		golangci-lint run --fix ./...; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Linting with fixes completed!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_RED)❌ golangci-lint not installed. Run 'make install-tools'$(COLOR_RESET)"; \
		exit 1; \
	fi

## fmt: Format code with gofmt
.PHONY: fmt
fmt:
	@echo "$(COLOR_BLUE)$(EMOJI_LINT) Formatting code...$(COLOR_RESET)"
	@gofmt -s -w .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Code formatted!$(COLOR_RESET)"

## fmt-check: Check code formatting
.PHONY: fmt-check
fmt-check:
	@echo "$(COLOR_BLUE)$(EMOJI_LINT) Checking code formatting...$(COLOR_RESET)"
	@if [ -n "$$(gofmt -s -l .)" ]; then \
		echo "$(COLOR_RED)❌ Code is not formatted. Run 'make fmt'$(COLOR_RESET)"; \
		gofmt -s -l .; \
		exit 1; \
	else \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Code is properly formatted!$(COLOR_RESET)"; \
	fi

## vet: Run go vet
.PHONY: vet
vet:
	@echo "$(COLOR_BLUE)$(EMOJI_LINT) Running go vet...$(COLOR_RESET)"
	@go vet ./...
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Vet completed!$(COLOR_RESET)"

## staticcheck: Run staticcheck
.PHONY: staticcheck
staticcheck:
	@echo "$(COLOR_BLUE)$(EMOJI_LINT) Running staticcheck...$(COLOR_RESET)"
	@if command -v staticcheck > /dev/null; then \
		staticcheck ./...; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Staticcheck completed!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_RED)❌ staticcheck not installed. Run 'make install-tools'$(COLOR_RESET)"; \
		exit 1; \
	fi

# ==================================================================================== #
# DEPENDENCY COMMANDS
# ==================================================================================== #

## deps: Download dependencies
.PHONY: deps
deps:
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Downloading dependencies...$(COLOR_RESET)"
	@go mod download
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Dependencies downloaded!$(COLOR_RESET)"

## deps-update: Update dependencies
.PHONY: deps-update
deps-update:
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Updating dependencies...$(COLOR_RESET)"
	@go get -u ./...
	@go mod tidy
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Dependencies updated!$(COLOR_RESET)"

## deps-tidy: Tidy dependencies
.PHONY: deps-tidy
deps-tidy:
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Tidying dependencies...$(COLOR_RESET)"
	@go mod tidy
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Dependencies tidied!$(COLOR_RESET)"

## deps-verify: Verify dependencies
.PHONY: deps-verify
deps-verify:
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Verifying dependencies...$(COLOR_RESET)"
	@go mod verify
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Dependencies verified!$(COLOR_RESET)"

## deps-graph: Show dependency graph
.PHONY: deps-graph
deps-graph:
	@echo "$(COLOR_BLUE)$(EMOJI_PACKAGE) Generating dependency graph...$(COLOR_RESET)"
	@go mod graph

# ==================================================================================== #
# RUN COMMANDS
# ==================================================================================== #

## run: Run the application
.PHONY: run
run:
	@echo "$(COLOR_BLUE)$(EMOJI_ROCKET) Running application...$(COLOR_RESET)"
	@go run .

## watch: Watch and auto-reload on changes
.PHONY: watch
watch:
	@echo "$(COLOR_BLUE)$(EMOJI_ROCKET) Starting file watcher...$(COLOR_RESET)"
	@if command -v air > /dev/null; then \
		air; \
	elif command -v reflex > /dev/null; then \
		reflex -s -r '\.go$$' make run; \
	else \
		echo "$(COLOR_RED)❌ No file watcher found. Install 'air' or 'reflex'$(COLOR_RESET)"; \
		exit 1; \
	fi

# ==================================================================================== #
# PERFORMANCE COMMANDS
# ==================================================================================== #

## bench: Run benchmarks
.PHONY: bench
bench:
	@echo "$(COLOR_BLUE)$(EMOJI_BENCH) Running benchmarks...$(COLOR_RESET)"
	@go test -bench=. -benchmem ./...
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Benchmarks completed!$(COLOR_RESET)"

## bench-compare: Compare benchmark results
.PHONY: bench-compare
bench-compare:
	@echo "$(COLOR_BLUE)$(EMOJI_BENCH) Running benchmark comparison...$(COLOR_RESET)"
	@go test -bench=. -benchmem ./... | tee new.bench
	@if [ -f old.bench ]; then \
		if command -v benchcmp > /dev/null; then \
			benchcmp old.bench new.bench; \
		else \
			echo "$(COLOR_YELLOW)⚠️  benchcmp not installed$(COLOR_RESET)"; \
		fi \
	fi
	@mv new.bench old.bench

## profile: Generate CPU profile
.PHONY: profile
profile:
	@echo "$(COLOR_BLUE)$(EMOJI_BENCH) Generating CPU profile...$(COLOR_RESET)"
	@go test -cpuprofile=cpu.prof -bench=.
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Profile generated: cpu.prof$(COLOR_RESET)"
	@echo "$(COLOR_CYAN)View with: go tool pprof cpu.prof$(COLOR_RESET)"

# ==================================================================================== #
# SECURITY COMMANDS
# ==================================================================================== #

## security: Run security checks
.PHONY: security
security:
	@echo "$(COLOR_BLUE)$(EMOJI_SECURITY) Running security checks...$(COLOR_RESET)"
	@if command -v gosec > /dev/null; then \
		gosec ./...; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Security scan completed!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_RED)❌ gosec not installed. Run 'make install-tools'$(COLOR_RESET)"; \
		exit 1; \
	fi

## vuln-check: Check for known vulnerabilities
.PHONY: vuln-check
vuln-check:
	@echo "$(COLOR_BLUE)$(EMOJI_SECURITY) Checking for vulnerabilities...$(COLOR_RESET)"
	@if command -v govulncheck > /dev/null; then \
		govulncheck ./...; \
		echo "$(COLOR_GREEN)$(EMOJI_CHECK) Vulnerability check completed!$(COLOR_RESET)"; \
	else \
		echo "$(COLOR_YELLOW)⚠️  govulncheck not installed. Installing...$(COLOR_RESET)"; \
		go install golang.org/x/vuln/cmd/govulncheck@latest; \
		govulncheck ./...; \
	fi

# ==================================================================================== #
# DOCKER COMMANDS
# ==================================================================================== #

## docker-build: Build Docker image
.PHONY: docker-build
docker-build:
	@echo "$(COLOR_BLUE)$(EMOJI_DOCKER) Building Docker image...$(COLOR_RESET)"
	@docker build -t $(BINARY_NAME):$(VERSION) -t $(BINARY_NAME):latest .
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Docker image built: $(BINARY_NAME):$(VERSION)$(COLOR_RESET)"

## docker-run: Run Docker container
.PHONY: docker-run
docker-run:
	@echo "$(COLOR_BLUE)$(EMOJI_DOCKER) Running Docker container...$(COLOR_RESET)"
	@docker run --rm -it $(BINARY_NAME):latest

## docker-push: Push Docker image
.PHONY: docker-push
docker-push:
	@echo "$(COLOR_BLUE)$(EMOJI_DOCKER) Pushing Docker image...$(COLOR_RESET)"
	@docker push $(BINARY_NAME):$(VERSION)
	@docker push $(BINARY_NAME):latest
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Docker image pushed!$(COLOR_RESET)"

# ==================================================================================== #
# DOCUMENTATION COMMANDS
# ==================================================================================== #

## docs: Generate documentation
.PHONY: docs
docs:
	@echo "$(COLOR_BLUE)$(EMOJI_DOCS) Generating documentation...$(COLOR_RESET)"
	@go doc -all > docs.txt
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Documentation generated: docs.txt$(COLOR_RESET)"

## docs-serve: Serve documentation locally
.PHONY: docs-serve
docs-serve:
	@echo "$(COLOR_BLUE)$(EMOJI_DOCS) Starting documentation server...$(COLOR_RESET)"
	@if command -v godoc > /dev/null; then \
		echo "$(COLOR_CYAN)Documentation available at: http://localhost:6060$(COLOR_RESET)"; \
		godoc -http=:6060; \
	else \
		echo "$(COLOR_RED)❌ godoc not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install with: go install golang.org/x/tools/cmd/godoc@latest$(COLOR_RESET)"; \
		exit 1; \
	fi

# ==================================================================================== #
# CLEANUP COMMANDS
# ==================================================================================== #

## clean: Remove build artifacts
.PHONY: clean
clean:
	@echo "$(COLOR_BLUE)$(EMOJI_CLEAN) Cleaning build artifacts...$(COLOR_RESET)"
	@go clean
	@rm -rf $(GOBIN)
	@rm -f coverage.out coverage.html
	@rm -f *.prof
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Cleanup complete!$(COLOR_RESET)"

## clean-all: Remove all generated files
.PHONY: clean-all
clean-all: clean
	@echo "$(COLOR_BLUE)$(EMOJI_CLEAN) Performing deep clean...$(COLOR_RESET)"
	@rm -rf vendor/
	@rm -f go.sum
	@rm -f docs.txt
	@rm -f *.bench
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Deep cleanup complete!$(COLOR_RESET)"

# ==================================================================================== #
# SETUP COMMANDS
# ==================================================================================== #

## install-tools: Install development tools
.PHONY: install-tools
install-tools:
	@echo "$(COLOR_BLUE)🛠️  Installing development tools...$(COLOR_RESET)"
	@echo "$(COLOR_YELLOW)Installing golangci-lint...$(COLOR_RESET)"
	@go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
	@echo "$(COLOR_YELLOW)Installing staticcheck...$(COLOR_RESET)"
	@go install honnef.co/go/tools/cmd/staticcheck@latest
	@echo "$(COLOR_YELLOW)Installing gosec...$(COLOR_RESET)"
	@go install github.com/securego/gosec/v2/cmd/gosec@latest
	@echo "$(COLOR_YELLOW)Installing govulncheck...$(COLOR_RESET)"
	@go install golang.org/x/vuln/cmd/govulncheck@latest
	@echo "$(COLOR_YELLOW)Installing godoc...$(COLOR_RESET)"
	@go install golang.org/x/tools/cmd/godoc@latest
	@echo "$(COLOR_YELLOW)Installing air (live reload)...$(COLOR_RESET)"
	@go install github.com/cosmtrek/air@latest
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) All tools installed!$(COLOR_RESET)"

## init: Initialize new Go module
.PHONY: init
init:
	@echo "$(COLOR_BLUE)🛠️  Initializing Go module...$(COLOR_RESET)"
	@read -p "Enter module name: " module; \
	go mod init $$module
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Go module initialized!$(COLOR_RESET)"

# ==================================================================================== #
# COMBINED COMMANDS
# ==================================================================================== #

## check: Run all checks (fmt, vet, lint, test)
.PHONY: check
check: fmt-check vet lint test
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) All checks passed!$(COLOR_RESET)"

## ci: Run CI pipeline (check + security + coverage)
.PHONY: ci
ci: check security test-cover
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) CI pipeline completed!$(COLOR_RESET)"

## pre-commit: Run before committing (fmt + check)
.PHONY: pre-commit
pre-commit: fmt check
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) Ready to commit!$(COLOR_RESET)"

# ==================================================================================== #
# INFORMATION COMMANDS
# ==================================================================================== #

## info: Display project information
.PHONY: info
info:
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)Project Information:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)Binary Name:$(COLOR_RESET) $(BINARY_NAME)"
	@echo "  $(COLOR_YELLOW)Version:$(COLOR_RESET) $(VERSION)"
	@echo "  $(COLOR_YELLOW)Build Time:$(COLOR_RESET) $(BUILD_TIME)"
	@echo "  $(COLOR_YELLOW)Git Commit:$(COLOR_RESET) $(GIT_COMMIT)"
	@echo "  $(COLOR_YELLOW)Git Branch:$(COLOR_RESET) $(GIT_BRANCH)"
	@echo "  $(COLOR_YELLOW)Go Version:$(COLOR_RESET) $$(go version)"
	@echo "  $(COLOR_YELLOW)Project Path:$(COLOR_RESET) $(GOBASE)"
