# ==========================================
# 🚀 GLPI DEPLOYMENT MANAGER
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = glpi

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help init up down restart logs status clean

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 GLPI IT ASSET MANAGEMENT"
	@echo "==========================================$(NC)"
	@echo ""
	@echo "$(CYAN)Servicios disponibles:$(NC)"
	@echo "  • GLPI Web   - http://localhost:8080"
	@echo "  • MariaDB    - Base de datos"
	@echo ""
	@echo "$(CYAN)Comandos disponibles:$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

init: ## Inicializa carpetas y despliega servicios
	@echo "$(CYAN)Inicializando estructura de carpetas...$(NC)"
	@mkdir -p db_data glpi_data
	@echo "$(CYAN)Desplegando GLPI...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Stack desplegado con éxito$(NC)"
	@echo "$(GREEN)  Accede a GLPI en: http://localhost:8080$(NC)"

up: ## Despliega el servicio
	@echo "$(CYAN)Desplegando GLPI...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Servicio iniciado$(NC)"

down: ## Detiene el contenedor
	@echo "$(CYAN)Deteniendo GLPI...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia el servicio
	@echo "$(CYAN)Reiniciando GLPI...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra estado del contenedor
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra logs en tiempo real
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50

logs-db: ## Logs de MariaDB
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f mariadb

logs-web: ## Logs de GLPI web
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f glpi

clean: ## Limpia recursos del proyecto
	@echo "$(CYAN)Limpiando proyecto...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans
	@echo "$(GREEN)✓ Limpieza completada$(NC)"
