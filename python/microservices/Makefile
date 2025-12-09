# Microservices Project Makefile
# Comprehensive makefile for microservices development
#
# Copy this file to your microservices project root as 'Makefile'
# Requires: Python 3.10+, Poetry

include ../common/Makefile.common

# ==================================================================================== #
# MICROSERVICES-SPECIFIC VARIABLES
# ==================================================================================== #

SERVICE_NAME?=$(PROJECT_NAME)
SERVICE_PORT?=50051
PROTO_DIR?=proto
GENERATED_DIR?=generated

# Docker Compose
COMPOSE_FILE?=docker-compose.yml

# ==================================================================================== #
# HELP
# ==================================================================================== #

.PHONY: help
help:
	@echo "$(COLOR_CYAN)$(COLOR_BOLD)"
	@echo "╔════════════════════════════════════════════════════════════════╗"
	@echo "║          Microservices Makefile Commands                       ║"
	@echo "╚════════════════════════════════════════════════════════════════╝"
	@echo "$(COLOR_RESET)"
	@echo "$(COLOR_GREEN)$(EMOJI_API) Protocol Buffers:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)proto-compile$(COLOR_RESET)     - Compile .proto files"
	@echo "  $(COLOR_YELLOW)proto-clean$(COLOR_RESET)       - Clean generated proto files"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_ROCKET) Service Management:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)service-start$(COLOR_RESET)     - Start microservice"
	@echo "  $(COLOR_YELLOW)service-stop$(COLOR_RESET)      - Stop microservice"
	@echo "  $(COLOR_YELLOW)service-restart$(COLOR_RESET)   - Restart microservice"
	@echo "  $(COLOR_YELLOW)health-check$(COLOR_RESET)      - Check service health"
	@echo ""
	@echo "$(COLOR_GREEN)🐳 Docker Compose:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)compose-up$(COLOR_RESET)        - Start all services"
	@echo "  $(COLOR_YELLOW)compose-down$(COLOR_RESET)      - Stop all services"
	@echo "  $(COLOR_YELLOW)compose-logs$(COLOR_RESET)      - View service logs"
	@echo "  $(COLOR_YELLOW)compose-ps$(COLOR_RESET)        - List running services"
	@echo ""
	@echo "$(COLOR_GREEN)☸️  Kubernetes:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)k8s-apply$(COLOR_RESET)         - Apply K8s manifests"
	@echo "  $(COLOR_YELLOW)k8s-delete$(COLOR_RESET)        - Delete K8s resources"
	@echo "  $(COLOR_YELLOW)k8s-logs$(COLOR_RESET)          - View pod logs"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_TEST) Testing:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)load-test$(COLOR_RESET)         - Run load tests (locust)"
	@echo "  $(COLOR_YELLOW)integration-test$(COLOR_RESET)  - Run integration tests"
	@echo "  $(COLOR_YELLOW)e2e-test$(COLOR_RESET)          - Run end-to-end tests"
	@echo ""
	@echo "$(COLOR_GREEN)📊 Monitoring:$(COLOR_RESET)"
	@echo "  $(COLOR_YELLOW)logs$(COLOR_RESET)              - Tail service logs"
	@echo "  $(COLOR_YELLOW)metrics$(COLOR_RESET)           - Display metrics"
	@echo ""
	@echo "$(COLOR_GREEN)$(EMOJI_LINT) Quality:$(COLOR_RESET) format, lint, typecheck"
	@echo "$(COLOR_GREEN)$(EMOJI_SECURITY) Security:$(COLOR_RESET) security, safety"
	@echo ""

.DEFAULT_GOAL := help

# ==================================================================================== #
# PROTOCOL BUFFERS
# ==================================================================================== #

## proto-compile: Compile .proto files
.PHONY: proto-compile
proto-compile:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which python | xargs -I {} {} -c "import grpc_tools" 2>/dev/null; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) grpcio-tools not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev grpcio-tools$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)$(EMOJI_BUILD) Compiling protocol buffers...$(COLOR_RESET)"
	@mkdir -p $(GENERATED_DIR)
	@$(POETRY_RUN) python -m grpc_tools.protoc \
		--proto_path=$(PROTO_DIR) \
		--python_out=$(GENERATED_DIR) \
		--grpc_python_out=$(GENERATED_DIR) \
		$(PROTO_DIR)/*.proto
	@touch $(GENERATED_DIR)/__init__.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Protocol buffers compiled!$(COLOR_RESET)"

## proto-clean: Clean generated proto files
.PHONY: proto-clean
proto-clean:
	@echo "$(COLOR_BLUE)$(EMOJI_CLEAN) Cleaning generated proto files...$(COLOR_RESET)"
	@rm -rf $(GENERATED_DIR)/*_pb2.py $(GENERATED_DIR)/*_pb2_grpc.py
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Proto files cleaned!$(COLOR_RESET)"

# ==================================================================================== #
# SERVICE MANAGEMENT
# ==================================================================================== #

## service-start: Start microservice
.PHONY: service-start
service-start:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_ROCKET) Starting $(SERVICE_NAME)...$(COLOR_RESET)"
	@$(POETRY_RUN) python -m $(SERVICE_NAME).server --port $(SERVICE_PORT)

## service-stop: Stop microservice
.PHONY: service-stop
service-stop:
	@echo "$(COLOR_BLUE)$(EMOJI_ROCKET) Stopping $(SERVICE_NAME)...$(COLOR_RESET)"
	@pkill -f "$(SERVICE_NAME).server" || echo "Service not running"
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Service stopped!$(COLOR_RESET)"

## service-restart: Restart microservice
.PHONY: service-restart
service-restart: service-stop service-start
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Service restarted!$(COLOR_RESET)"

## health-check: Check service health
.PHONY: health-check
health-check:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_SECURITY) Checking service health...$(COLOR_RESET)"
	@$(POETRY_RUN) python -c "import grpc; \
		channel = grpc.insecure_channel('localhost:$(SERVICE_PORT)'); \
		grpc.channel_ready_future(channel).result(timeout=10); \
		print('$(COLOR_GREEN)$(EMOJI_CHECK) Service is healthy!$(COLOR_RESET)')" || \
		echo "$(COLOR_RED)$(EMOJI_ERROR) Service is not responding$(COLOR_RESET)"

# ==================================================================================== #
# DOCKER COMPOSE
# ==================================================================================== #

## compose-up: Start all services with Docker Compose
.PHONY: compose-up
compose-up:
	@if [ ! -f "$(COMPOSE_FILE)" ]; then \
		echo "$(COLOR_RED)$(EMOJI_ERROR) $(COMPOSE_FILE) not found$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)🐳 Starting services...$(COLOR_RESET)"
	@docker-compose -f $(COMPOSE_FILE) up -d
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Services started!$(COLOR_RESET)"

## compose-down: Stop all services
.PHONY: compose-down
compose-down:
	@echo "$(COLOR_BLUE)🐳 Stopping services...$(COLOR_RESET)"
	@docker-compose -f $(COMPOSE_FILE) down
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Services stopped!$(COLOR_RESET)"

## compose-logs: View service logs
.PHONY: compose-logs
compose-logs:
	@docker-compose -f $(COMPOSE_FILE) logs -f

## compose-ps: List running services
.PHONY: compose-ps
compose-ps:
	@docker-compose -f $(COMPOSE_FILE) ps

## compose-rebuild: Rebuild and restart services
.PHONY: compose-rebuild
compose-rebuild:
	@echo "$(COLOR_BLUE)🐳 Rebuilding services...$(COLOR_RESET)"
	@docker-compose -f $(COMPOSE_FILE) up -d --build
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Services rebuilt!$(COLOR_RESET)"

# ==================================================================================== #
# KUBERNETES
# ==================================================================================== #

## k8s-apply: Apply Kubernetes manifests
.PHONY: k8s-apply
k8s-apply:
	@if [ ! -d "k8s" ]; then \
		echo "$(COLOR_RED)$(EMOJI_ERROR) k8s/ directory not found$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)☸️  Applying Kubernetes manifests...$(COLOR_RESET)"
	@kubectl apply -f k8s/
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Manifests applied!$(COLOR_RESET)"

## k8s-delete: Delete Kubernetes resources
.PHONY: k8s-delete
k8s-delete:
	@echo "$(COLOR_BLUE)☸️  Deleting Kubernetes resources...$(COLOR_RESET)"
	@kubectl delete -f k8s/
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Resources deleted!$(COLOR_RESET)"

## k8s-logs: View pod logs
.PHONY: k8s-logs
k8s-logs:
	@kubectl logs -f -l app=$(SERVICE_NAME)

## k8s-status: Check deployment status
.PHONY: k8s-status
k8s-status:
	@echo "$(COLOR_BLUE)☸️  Deployment Status:$(COLOR_RESET)"
	@kubectl get pods -l app=$(SERVICE_NAME)
	@kubectl get services -l app=$(SERVICE_NAME)

# ==================================================================================== #
# TESTING
# ==================================================================================== #

## load-test: Run load tests with locust
.PHONY: load-test
load-test:
	$(call check_poetry)
	@if ! $(POETRY_RUN) which locust > /dev/null 2>&1; then \
		echo "$(COLOR_YELLOW)$(EMOJI_WARNING) locust not installed$(COLOR_RESET)"; \
		echo "$(COLOR_YELLOW)Install: poetry add --group dev locust$(COLOR_RESET)"; \
		exit 1; \
	fi
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Starting load test...$(COLOR_RESET)"
	@echo "$(COLOR_CYAN)$(EMOJI_INFO) Locust UI at: http://localhost:8089$(COLOR_RESET)"
	@$(POETRY_RUN) locust -f tests/locustfile.py

## integration-test: Run integration tests
.PHONY: integration-test
integration-test:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running integration tests...$(COLOR_RESET)"
	@$(POETRY_RUN) pytest tests/integration -v
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) Integration tests passed!$(COLOR_RESET)"

## e2e-test: Run end-to-end tests
.PHONY: e2e-test
e2e-test:
	$(call check_poetry)
	@echo "$(COLOR_BLUE)$(EMOJI_TEST) Running e2e tests...$(COLOR_RESET)"
	@$(POETRY_RUN) pytest tests/e2e -v
	@echo "$(COLOR_GREEN)$(EMOJI_CHECK) E2E tests passed!$(COLOR_RESET)"

# ==================================================================================== #
# MONITORING
# ==================================================================================== #

## logs: Tail service logs
.PHONY: logs
logs:
	@echo "$(COLOR_BLUE)📊 Tailing logs...$(COLOR_RESET)"
	@tail -f logs/$(SERVICE_NAME).log 2>/dev/null || echo "$(COLOR_YELLOW)Log file not found$(COLOR_RESET)"

## metrics: Display service metrics
.PHONY: metrics
metrics:
	@echo "$(COLOR_BLUE)📊 Service Metrics:$(COLOR_RESET)"
	@curl -s http://localhost:9090/metrics 2>/dev/null || echo "$(COLOR_YELLOW)Metrics endpoint not available$(COLOR_RESET)"

# ==================================================================================== #
# COMBINED WORKFLOWS
# ==================================================================================== #

## dev-setup: Setup development environment
.PHONY: dev-setup
dev-setup: install-dev proto-compile
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) Development environment ready!$(COLOR_RESET)"

## deploy-local: Deploy locally with Docker Compose
.PHONY: deploy-local
deploy-local: docker-build compose-up
	@echo "$(COLOR_GREEN)$(COLOR_BOLD)$(EMOJI_CHECK) Local deployment complete!$(COLOR_RESET)"
