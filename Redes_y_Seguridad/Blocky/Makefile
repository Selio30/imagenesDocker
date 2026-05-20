# ==========================================
# 🚀 BLOCKY DNS (GitOps Sinkhole)
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = blocky-dns

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help check-env init up down restart status logs clean reload

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 BLOCKY GITOPS DNS"
	@echo "==========================================$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

check-env: ## Verifica que .env existe
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)⚠️  Archivo .env no encontrado. Creando desde .env.example...$(NC)"; \
		cp .env.example .env; \
		echo "$(YELLOW)⚙️  Revisa .env y ajusta si es necesario, luego ejecuta 'make init' de nuevo.$(NC)"; \
		exit 1; \
	fi
	@echo "$(GREEN)✓ Archivo .env validado.$(NC)"

init: check-env ## Despliega el stack por primera vez
	@echo "$(CYAN)Desplegando Blocky DNS...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Blocky desplegado$(NC)"

up: check-env ## Levanta los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d

down: ## Detiene los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia el contenedor
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra el estado del servicio
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra los logs en tiempo real
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50

reload: ## Recarga la configuración sin reiniciar el contenedor
	@echo "$(CYAN)Enviando señal para recargar config.yml...$(NC)"
	@curl -s -X POST http://localhost:4000/api/blocking/reload
	@echo "\n$(GREEN)✓ Configuración recargada$(NC)"

clean: ## Detiene y borra contenedores huérfanos
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans