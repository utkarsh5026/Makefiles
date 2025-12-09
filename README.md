<div align="center">
  <img src="logo.png" alt="Makefiles Logo" width="200"/>

  <h1>Comprehensive Makefile Templates</h1>
  <p><strong>Production-ready Makefiles for modern software development</strong></p>

  [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
  [![Go](https://img.shields.io/badge/Go-1.21+-00ADD8?logo=go&logoColor=white)](go/)
  [![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](python/)
  [![Make](https://img.shields.io/badge/Make-GNU-red?logo=gnu&logoColor=white)](#)
  [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/yourusername/makefiles/pulls)
</div>

---

## 📖 Overview

A curated collection of **production-ready Makefile templates** following industry best practices for 2025. Each Makefile is comprehensive, well-documented, and includes smart tool detection, beautiful colored output, and extensive automation for the complete development lifecycle.

### ✨ Features

- **🚀 Production-Ready**: Battle-tested commands for real-world projects
- **🔍 Smart Detection**: Automatic tool discovery with helpful error messages
- **🎨 Beautiful Output**: Color-coded feedback with emoji indicators
- **📚 Comprehensive**: Complete development lifecycle coverage
- **⚡ Extensible**: Easy to customize for your specific needs
- **🛠️ Modern Tools**: Uses latest industry-standard tooling

---

## 📂 Project Structure

```
Makefiles/
├── logo.png                    # Project logo
├── LICENSE                     # Apache 2.0 License
├── README.md                   # This file
│
├── go/                         # Go project templates
│   └── Makefile                # Comprehensive Go Makefile (547 lines)
│
└── python/                     # Python project templates
    ├── common/                 # Shared Python utilities
    │   └── Makefile.common     # Common targets and variables
    │
    ├── web/                    # Web application Makefiles
    │   ├── Makefile.fastapi    # FastAPI projects
    │   ├── Makefile.django     # Django projects
    │   └── Makefile.flask      # Flask projects
    │
    ├── cli/                    # CLI application Makefile
    │   └── Makefile            # Click/Typer CLI tools
    │
    ├── library/                # Python library/package Makefile
    │   └── Makefile            # PyPI publishing workflow
    │
    ├── data-science/           # Data science project Makefile
    │   └── Makefile            # Jupyter, ML, data pipelines
    │
    └── microservices/          # Microservices Makefile
        └── Makefile            # gRPC, Docker Compose, K8s
```

---

## 🚀 Quick Start

### Go Projects

```bash
# Copy to your Go project
cp go/Makefile /path/to/your/go-project/

# Install development tools
make install-tools

# Run all checks
make check

# Build for all platforms
make build-all
```

### Python Projects

#### FastAPI Web Application

```bash
# Copy Makefile to your FastAPI project
cp python/web/Makefile.fastapi /path/to/fastapi-project/Makefile

# Install dependencies
make install-dev

# Apply database migrations
make db-upgrade

# Run development server
make dev-server

# Run all checks before commit
make pre-commit
```

#### Django Web Application

```bash
# Copy Makefile
cp python/web/Makefile.django /path/to/django-project/Makefile

# Install and setup
make install-dev

# Create migrations
make makemigrations

# Apply migrations
make migrate

# Run development server
make runserver
```

#### Flask Web Application

```bash
# Copy Makefile
cp python/web/Makefile.flask /path/to/flask-project/Makefile

make install-dev
make db-upgrade
make run
```

#### CLI Application

```bash
# Copy Makefile
cp python/cli/Makefile /path/to/cli-project/Makefile

# Install and test
make install-dev
make cli-run ARGS="--help"

# Build standalone binary
make build-binary
```

#### Python Library

```bash
# Copy Makefile
cp python/library/Makefile /path/to/library-project/Makefile

# Install dependencies
make install-dev

# Run tests with coverage
make test-cov

# Build package
make build

# Publish to TestPyPI
make publish-test
```

#### Data Science Project

```bash
# Copy Makefile
cp python/data-science/Makefile /path/to/ds-project/Makefile

# Setup project structure
make setup-dirs

# Install dependencies
make install-dev

# Start JupyterLab
make notebook-lab

# Run full pipeline
make pipeline-full
```

#### Microservices

```bash
# Copy Makefile
cp python/microservices/Makefile /path/to/service/Makefile

# Install and compile protos
make install-dev
make proto-compile

# Start services with Docker Compose
make compose-up

# Run load tests
make load-test
```

---

## 📋 Language-Specific Features

### Go Makefile

**570+ lines** | **50+ commands** | **Multi-platform builds**

#### Features
- ✅ Multi-platform builds (Linux, Windows, macOS Intel/ARM)
- ✅ Comprehensive testing (unit, race, coverage, integration)
- ✅ Advanced linting (golangci-lint, staticcheck, go vet)
- ✅ Security scanning (gosec, govulncheck)
- ✅ Performance profiling and benchmarking
- ✅ Docker integration
- ✅ Live reload development (air)
- ✅ Dependency management (go mod)

#### Key Commands
```bash
make build          # Build the application
make build-all      # Build for all platforms
make test-race      # Run tests with race detector
make lint           # Run golangci-lint
make security       # Run security checks
make bench          # Run benchmarks
make docker-build   # Build Docker image
make ci             # Run full CI pipeline
```

---

### Python Makefiles

**Poetry-based** | **Ruff + mypy** | **Python 3.10+** | **~4000 lines total**

All Python Makefiles use **Poetry** for dependency management and include modern tooling:

#### Common Features (All Python Projects)

- ✅ **Poetry** dependency management
- ✅ **Ruff** formatting and linting (fastest Python linter, replaces black/flake8/isort)
- ✅ **mypy** type checking
- ✅ **pytest** with coverage
- ✅ **bandit** security scanning
- ✅ **safety** dependency vulnerability checks
- ✅ Docker containerization
- ✅ Pre-commit hooks
- ✅ CI/CD ready workflows

#### Common Commands (All Python Makefiles)
```bash
make install-dev    # Install all dependencies
make test-cov       # Run tests with coverage
make format         # Format code with ruff
make lint           # Run linter
make typecheck      # Run mypy
make security-all   # Run all security checks
make pre-commit     # Run before committing
make ci             # Run full CI pipeline
```

---

### FastAPI Makefile

**~600 lines** | **Modern async framework**

#### Specialized Commands
```bash
make dev-server         # Run with uvicorn hot-reload
make openapi-schema     # Generate OpenAPI schema
make openapi-validate   # Validate OpenAPI schema
make db-migrate         # Create Alembic migration
make db-upgrade         # Apply migrations
make serve-gunicorn     # Production server
```

#### Features
- Uvicorn development server with hot-reload
- Alembic database migrations
- OpenAPI schema generation and validation
- Async testing patterns
- ASGI deployment with Gunicorn

---

### Django Makefile

**~600 lines** | **Full-featured web framework**

#### Specialized Commands
```bash
make runserver          # Run Django dev server
make makemigrations     # Create migrations
make migrate            # Apply migrations
make collectstatic      # Collect static files
make createsuperuser    # Create admin user
make check-deploy       # Deployment checks
make gunicorn           # Production server
```

#### Features
- Django development server
- Database migrations
- Static file management
- User management
- Django system checks
- WSGI deployment

---

### Flask Makefile

**~550 lines** | **Lightweight and flexible**

#### Specialized Commands
```bash
make run                # Run Flask dev server
make shell              # Open Flask shell
make routes             # Display all routes
make db-migrate         # Flask-Migrate commands
make db-upgrade         # Apply migrations
```

---

### Library/Package Makefile

**~550 lines** | **PyPI publishing workflow**

#### Specialized Commands
```bash
make build              # Build wheel + sdist
make check-package      # Validate with twine
make publish-test       # Publish to TestPyPI
make publish            # Publish to PyPI
make bump-patch         # Bump version (x.x.1)
make bump-minor         # Bump version (x.1.0)
make bump-major         # Bump version (1.0.0)
make api-docs           # Generate API docs
```

#### Features
- Poetry build system
- PyPI/TestPyPI publishing
- Semantic versioning
- Documentation generation (Sphinx/MkDocs)
- Package validation
- Multi-Python version testing (tox)

---

### CLI Application Makefile

**~500 lines** | **Command-line tools**

#### Specialized Commands
```bash
make cli-run ARGS="--help"  # Run CLI
make build-binary           # Build with PyInstaller
make build-all              # Multi-platform builds
make gen-completion         # Generate shell completions
make install-local          # Install for testing
```

#### Features
- Click/Typer framework support
- Binary packaging (PyInstaller)
- Shell completion generation (bash, zsh, fish)
- Multi-platform distribution
- Local installation testing

---

### Data Science Makefile

**~550 lines** | **ML and data pipelines**

#### Specialized Commands
```bash
make notebook-lab       # Start JupyterLab
make notebook-test      # Test notebooks
make data-download      # Download datasets
make data-process       # Process data
make train              # Train model
make evaluate           # Evaluate model
make mlflow-ui          # Start MLflow UI
make check-gpu          # Check GPU availability
```

#### Features
- Jupyter Notebook/Lab integration
- Notebook testing (nbval, papermill)
- Data pipeline management
- Model training and evaluation
- Experiment tracking (MLflow, W&B)
- GPU environment checks

---

### Microservices Makefile

**~600 lines** | **Distributed systems**

#### Specialized Commands
```bash
make proto-compile      # Compile .proto files
make service-start      # Start service
make compose-up         # Docker Compose up
make k8s-apply          # Apply K8s manifests
make load-test          # Run load tests
make health-check       # Check service health
```

#### Features
- Protocol buffer compilation (gRPC)
- Service orchestration (Docker Compose, Kubernetes)
- Health check monitoring
- Load testing (locust)
- Distributed tracing
- Metrics collection

---

## 🛠️ Tool Requirements

### Go Projects

**Required:**
- Go 1.21+
- Make 4.0+

**Optional** (auto-installed via `make install-tools`):
- golangci-lint
- staticcheck
- gosec
- govulncheck
- air (live reload)

---

### Python Projects

**Required:**
- Python 3.10+
- Poetry 1.8+
- Make 4.0+

**Dev Dependencies** (installed via Poetry):
- ruff (formatter + linter)
- mypy (type checker)
- pytest + pytest-cov
- bandit (security)
- safety (dependency security)

**Project-Specific:**
- **Web**: FastAPI/Django/Flask, uvicorn/gunicorn, alembic
- **CLI**: click/typer, PyInstaller, rich
- **Library**: sphinx/mkdocs, twine, build
- **Data Science**: jupyterlab, pandas, numpy, mlflow
- **Microservices**: grpcio, docker-compose, locust

---

## 📚 Command Categories

Each Makefile is organized into these categories:

1. **🔨 Build Commands** - Compilation and packaging
2. **🧪 Test Commands** - Testing and coverage
3. **🔍 Code Quality** - Formatting, linting, type checking
4. **🔒 Security** - Vulnerability scanning
5. **📚 Documentation** - Docs generation and serving
6. **🐳 Docker** - Container operations
7. **📦 Dependency Management** - Package updates
8. **🧹 Cleanup** - Artifact removal
9. **🚀 Development** - Development server, debugging
10. **🎯 Deployment** - Production deployment helpers
11. **⚙️ Combined Workflows** - CI/CD, pre-commit

---

## 🎨 Customization Guide

### Modifying Variables

Each Makefile starts with a variables section. Customize for your project:

```makefile
# Python Makefiles
PROJECT_NAME=myproject
VERSION=0.1.0
PYTHON_VERSION=3.10

# FastAPI specific
APP_MODULE=app.main:app
HOST=127.0.0.1
PORT=8000

# Django specific
DJANGO_SETTINGS_MODULE=config.settings

# Custom variables
DOCKER_IMAGE=$(PROJECT_NAME):$(VERSION)
```

### Adding Custom Commands

Add new commands to the appropriate section:

```makefile
## my-command: Description of what it does
.PHONY: my-command
my-command:
	@echo "$(COLOR_BLUE)Running custom command...$(COLOR_RESET)"
	@poetry run python scripts/custom.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Complete!$(COLOR_RESET)"
```

### Extending Existing Commands

Override or extend commands:

```makefile
.PHONY: test
test: install-dev
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running tests...$(COLOR_RESET)"
	@$(POETRY_RUN) pytest -v --custom-flag
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Tests passed!$(COLOR_RESET)"
```

---

## 🔄 CI/CD Integration

### GitHub Actions

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      # For Python projects
      - uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Install Poetry
        run: curl -sSL https://install.python-poetry.org | python3 -

      - name: Run CI pipeline
        run: make ci
```

### GitLab CI

```yaml
test:
  image: python:3.10
  before_script:
    - curl -sSL https://install.python-poetry.org | python3 -
    - export PATH="/root/.local/bin:$PATH"
  script:
    - make ci
```

### Jenkins

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'make install-dev'
            }
        }
        stage('Test') {
            steps {
                sh 'make ci'
            }
        }
    }
}
```

---

## 💡 Best Practices

### Python Projects

1. **Always use Poetry** for modern dependency management
2. **Run pre-commit before pushing**: `make pre-commit`
3. **Maintain high test coverage**: Aim for 80%+ (`make test-cov`)
4. **Use type hints**: Enable mypy strict mode
5. **Security scan regularly**: `make security-all`
6. **Lock dependencies**: Commit `poetry.lock`
7. **Test in containers**: Ensure reproducibility

### Go Projects

1. **Use modules**: Go modules are required
2. **Follow standard layout**: `cmd/`, `internal/`, `pkg/`
3. **Run all checks**: `make check` before commits
4. **Enable race detector**: `make test-race`
5. **Keep dependencies updated**: `make deps-update`
6. **Use semantic versioning**: Update `VERSION` variable
7. **Profile performance**: `make bench` regularly

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Test your changes thoroughly
4. Commit with clear messages
5. Push to your branch
6. Open a Pull Request

### Adding New Makefiles

When adding a new Makefile:

1. Follow existing structure and conventions
2. Include comprehensive help text
3. Add color-coded output with emojis
4. Implement smart tool detection
5. Update this README
6. Add usage examples
7. Test all commands

---

## 📝 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Inspired by modern DevOps best practices
- Built with feedback from production environments
- Continuously updated with latest tools and patterns
- Community contributions welcome

---

## 📊 Project Stats

- **Languages**: Go, Python
- **Python Project Types**: 6 (Web, CLI, Library, Data Science, Microservices)
- **Total Makefiles**: 10+
- **Lines of Code**: ~5,000+
- **Commands**: 200+
- **Supported Platforms**: Linux, macOS, Windows

---

<div align="center">
  <p><strong>Made with ❤️ for the developer community</strong></p>
  <p>
    <a href="https://github.com/yourusername/makefiles">⭐ Star on GitHub</a> •
    <a href="https://github.com/yourusername/makefiles/issues">🐛 Report Bug</a> •
    <a href="https://github.com/yourusername/makefiles/issues">💡 Request Feature</a>
  </p>

  <p><sub>Last updated: 2025</sub></p>
</div>
