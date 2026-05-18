# ==========================================
# 🚀 PORTAINER DEPLOYMENT MANAGER
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = portainer

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help init up down restart logs status clean

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 PORTAINER MANAGEMENT STACK"
	@echo "==========================================$(NC)"
	@echo ""
	@echo "$(CYAN)Servicio disponible en:$(NC)"
	@echo "  • Portainer UI (HTTPS) - https://localhost:19443"
	@echo ""
	@echo "$(CYAN)Comandos disponibles:$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

init: ## Inicializa carpetas y despliega servicios
	@echo "$(CYAN)Inicializando estructura de carpetas...$(NC)"
	@mkdir -p data
	@echo "$(CYAN)Desplegando Portainer...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Stack desplegado con éxito$(NC)"

up: ## Despliega el servicio
	@echo "$(CYAN)Desplegando Portainer...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Servicio iniciado$(NC)"

down: ## Detiene el contenedor
	@echo "$(CYAN)Deteniendo Portainer...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia el servicio
	@echo "$(CYAN)Reiniciando Portainer...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra estado del contenedor
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra logs en tiempo real
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50

clean: ## Limpia recursos del proyecto
	@echo "$(CYAN)Limpiando proyecto...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans
	@echo "$(GREEN)✓ Limpieza completada$(NC)"