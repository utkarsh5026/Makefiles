# Data Science/ML Project Makefile
# Comprehensive makefile for data science and machine learning projects
#
# Copy this file to your data science project root as 'Makefile'
# Requires: Python 3.10+, Poetry

include ../common/Makefile.common

# ==================================================================================== #
# DATA SCIENCE-SPECIFIC VARIABLES
# ==================================================================================== #

NOTEBOOK_DIR?=notebooks
DATA_DIR?=data
MODELS_DIR?=models
REPORTS_DIR?=reports

# Jupyter settings
JUPYTER_PORT?=8888
JUPYTER_HOST?=localhost

# ==================================================================================== #
# HELP
# ==================================================================================== #

.PHONY: help
help:
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)"
	@echo "╔════════════════════════════════════════════════════════════════╗"
	@echo "║        Data Science/ML Makefile Commands                       ║"
	@echo "╚════════════════════════════════════════════════════════════════╝"
	@echo "$(COLOR_RESET)"
	@echo "$(COLOR_GREEN)📓 Jupyter Commands:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)notebook$(COLOR_RESET)          - Start Jupyter Notebook"
	@echo "  $(COLOR_YELLOW)notebook-lab$(COLOR_RESET)      - Start JupyterLab"
	@echo "  $(COLOR_YELLOW)notebook-convert$(COLOR_RESET)  - Convert notebooks to Python/HTML"
	@echo "  $(COLOR_YELLOW)notebook-test$(COLOR_RESET)     - Test notebooks with nbval"
	@echo "  $(COLOR_YELLOW)notebook-clean$(COLOR_RESET)    - Clear notebook outputs"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_DATA) Data Pipeline:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)data-download$(COLOR_RESET)     - Download datasets"
	@echo "  $(COLOR_YELLOW)data-process$(COLOR_RESET)      - Process raw data"
	@echo "  $(COLOR_YELLOW)data-validate$(COLOR_RESET)     - Validate data quality"
	@echo "  $(COLOR_YELLOW)data-clean$(COLOR_RESET)        - Clean data directory"
	@echo ""
	@echo "$(COLOR_GREEN)🤖 ML/Training:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)train$(COLOR_RESET)             - Train model"
	@echo "  $(COLOR_YELLOW)evaluate$(COLOR_RESET)          - Evaluate model performance"
	@echo "  $(COLOR_YELLOW)predict$(COLOR_RESET)           - Run inference"
	@echo "  $(COLOR_YELLOW)tune$(COLOR_RESET)              - Hyperparameter tuning"
	@echo ""
	@echo "$(COLOR_GREEN)📊 Experiment Tracking:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)mlflow-ui$(COLOR_RESET)         - Start MLflow UI"
	@echo "  $(COLOR_YELLOW)wandb-sync$(COLOR_RESET)        - Sync with Weights & Biases"
	@echo ""
	@echo "$(COLOR_GREEN)⚙️  Environment:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)check-gpu$(COLOR_RESET)         - Check GPU availability"
	@echo "  $(COLOR_YELLOW)setup-dirs$(COLOR_RESET)        - Create project directory structure"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_TEST) Testing:$(COLOR_RESET) test, test-cov"
	@echo "$(COLOR_GREEN)$(EMOJI_LINT) Quality:$(COLOR_RESET) format, lint, typecheck"
	@echo ""

.DEFAULT_GOAL := help

# ==================================================================================== #
# JUPYTER COMMANDS
# ==================================================================================== #

## notebook: Start Jupyter Notebook
.PHONY: notebook
notebook:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which jupyter > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) Jupyter not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev jupyter$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)📓 Starting Jupyter Notebook...$(COLOR_RESET)"
	@echo "$(COLOR_CYAN)$(EMOJI_INFO) Notebook at: http://$(JUPYTER_HOST):$(JUPYTER_PORT)$(COLOR_RESET)"
	@$(POETRY_RUN) jupyter notebook --notebook-dir=$(NOTEBOOK_DIR) --port=$(JUPYTER_PORT)

## notebook-lab: Start JupyterLab
.PHONY: notebook-lab
notebook-lab:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which jupyter-lab > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) JupyterLab not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev jupyterlab$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)📓 Starting JupyterLab...$(COLOR_RESET)"
	@echo "$(COLOR_CYAN)$(EMOJI_INFO) JupyterLab at: http://$(JUPYTER_HOST):$(JUPYTER_PORT)$(COLOR_RESET)"
	@$(POETRY_RUN) jupyter lab --notebook-dir=$(NOTEBOOK_DIR) --port=$(JUPYTER_PORT)

## notebook-convert: Convert notebooks to Python/HTML
.PHONY: notebook-convert
notebook-convert:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)📓 Converting notebooks...$(COLOR_RESET)"
	@mkdir -p $(REPORTS_DIR)/html $(REPORTS_DIR)/python
	@for notebook in $(NOTEBOOK_DIR)/*.ipynb; do \
		echo "Converting $$notebook..."; \
		$(POETRY_RUN) jupyter nbconvert --to html --output-dir=$(REPORTS_DIR)/html $$notebook; \
		$(POETRY_RUN) jupyter nbconvert --to python --output-dir=$(REPORTS_DIR)/python $$notebook; \
	done
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Notebooks converted!$(COLOR_RESET)"

## notebook-test: Test notebooks with nbval
.PHONY: notebook-test
notebook-test:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which pytest > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) pytest/nbval not installed$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Testing notebooks...$(COLOR_RESET)"
	@$(POETRY_RUN) pytest --nbval $(NOTEBOOK_DIR)
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Notebook tests passed!$(COLOR_RESET)"

## notebook-clean: Clear notebook outputs
.PHONY: notebook-clean
notebook-clean:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_CLEAN) Clearing notebook outputs...$(COLOR_RESET)"
	@$(POETRY_RUN) jupyter nbconvert --clear-output --inplace $(NOTEBOOK_DIR)/*.ipynb
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Notebook outputs cleared!$(COLOR_RESET)"

# ==================================================================================== #
# DATA PIPELINE
# ==================================================================================== #

## data-download: Download datasets
.PHONY: data-download
data-download:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_DATA) Downloading datasets...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/download_data.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Datasets downloaded!$(COLOR_RESET)"

## data-process: Process raw data
.PHONY: data-process
data-process:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_DATA) Processing data...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/process_data.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Data processing complete!$(COLOR_RESET)"

## data-validate: Validate data quality
.PHONY: data-validate
data-validate:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_DATA) Validating data...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/validate_data.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Data validation complete!$(COLOR_RESET)"

## data-clean: Clean data directory
.PHONY: data-clean
data-clean:
	@echo "$(COLOR_BLUE)$(EMOJI_CLEAN) Cleaning data directory...$(COLOR_RESET)"
	@rm -rf $(DATA_DIR)/processed/* 2>/dev/null || true
	@rm -rf $(DATA_DIR)/interim/* 2>/dev/null || true
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Data directory cleaned!$(COLOR_RESET)"

# ==================================================================================== #
# ML/TRAINING
# ==================================================================================== #

## train: Train model
.PHONY: train
train:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)🤖 Training model...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/train.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Training complete!$(COLOR_RESET)"

## evaluate: Evaluate model performance
.PHONY: evaluate
evaluate:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)📊 Evaluating model...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/evaluate.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Evaluation complete!$(COLOR_RESET)"

## predict: Run inference
.PHONY: predict
predict:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)🤖 Running predictions...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/predict.py $(ARGS)
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Predictions complete!$(COLOR_RESET)"

## tune: Hyperparameter tuning
.PHONY: tune
tune:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)⚙️  Running hyperparameter tuning...$(COLOR_RESET)"
	@$(POETRY_RUN) python scripts/tune.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Tuning complete!$(COLOR_RESET)"

# ==================================================================================== #
# EXPERIMENT TRACKING
# ==================================================================================== #

## mlflow-ui: Start MLflow UI
.PHONY: mlflow-ui
mlflow-ui:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which mlflow > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) MLflow not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add mlflow$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)📊 Starting MLflow UI...$(COLOR_RESET)"
	@echo "$(COLOR_CYAN)$(EMOJI_INFO) MLflow UI at: http://localhost:5000$(COLOR_RESET)"
	@$(POETRY_RUN) mlflow ui

## wandb-sync: Sync with Weights & Biases
.PHONY: wandb-sync
wandb-sync:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which wandb > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) wandb not installed$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)📊 Syncing with W&B...$(COLOR_RESET)"
	@$(POETRY_RUN) wandb sync
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Sync complete!$(COLOR_RESET)"

# ==================================================================================== #
# ENVIRONMENT
# ==================================================================================== #

## check-gpu: Check GPU availability
.PHONY: check-gpu
check-gpu:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)⚙️  Checking GPU availability...$(COLOR_RESET)"
	@$(POETRY_RUN) python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'CUDA devices: {torch.cuda.device_count()}') if torch.cuda.is_available() else None" 2>/dev/null || \
	$(POETRY_RUN) python -c "import tensorflow as tf; print(f'GPU devices: {len(tf.config.list_physical_devices(\"GPU\"))}')" 2>/dev/null || \
	echo "$(COLOR_YELLOW)$(EMOJI_WARNING) PyTorch or TensorFlow not installed$(COLOR_RESET)"

## setup-dirs: Create project directory structure
.PHONY: setup-dirs
setup-dirs:
	@echo "$(COLOR_BLUE)⚙️  Creating directory structure...$(COLOR_RESET)"
	@mkdir -p $(DATA_DIR)/raw $(DATA_DIR)/processed $(DATA_DIR)/interim $(DATA_DIR)/external
	@mkdir -p $(NOTEBOOK_DIR)
	@mkdir -p $(MODELS_DIR)
	@mkdir -p $(REPORTS_DIR)/figures $(REPORTS_DIR)/html $(REPORTS_DIR)/python
	@mkdir -p scripts
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Directory structure created!$(COLOR_RESET)"

## pipeline-full: Run full data pipeline
.PHONY: pipeline-full
pipeline-full: data-download data-process data-validate train evaluate
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) Full pipeline complete!$(COLOR_RESET)"
