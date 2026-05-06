# ==========================================
# 🚀 DUCKDNS DEPLOYMENT MANAGER
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = duckdns

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help check-env init up down restart status logs clean

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 DUCKDNS DYNAMIC IP UPDATER"
	@echo "==========================================$(NC)"
	@echo "$(CYAN)Información:$(NC)"
	@echo "  • Mantiene sincronizada la IP pública para tus subdominios"
	@echo ""
	@echo "$(CYAN)Comandos disponibles:$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

check-env:
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)Creando archivo .env a partir de .env.example...$(NC)"; \
		cp .env.example .env; \
		echo "$(GREEN).env creado. Por favor, edita el archivo con tu token y subdominios.$(NC)"; \
	fi

init: check-env ## Inicializa el entorno y despliega el servicio
	@grep -E -q 'your_token_here|subdomain1,subdomain2' .env && echo "$(RED)Error: ¡Debes configurar un TOKEN válido y tus SUBDOMAINS reales en el archivo .env antes de iniciar!$(NC)" && exit 1 || true
	@echo "$(CYAN)Desplegando servicio DuckDNS...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Contenedor desplegado con éxito$(NC)"

up: check-env ## Despliega el servicio de DuckDNS
	@grep -E -q 'your_token_here|subdomain1,subdomain2' .env && echo "$(RED)Error: ¡Debes configurar un TOKEN válido y tus SUBDOMAINS reales en el archivo .env antes de iniciar!$(NC)" && exit 1 || true
	@echo "$(CYAN)Desplegando servicio...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Servicio iniciado$(NC)"

down: ## Detiene el contenedor
	@echo "$(CYAN)Deteniendo servicio...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia el servicio
	@echo "$(CYAN)Reiniciando servicio...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra estado del contenedor
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra logs en tiempo real
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50

clean: ## Limpia recursos del proyecto
	@echo "$(CYAN)Limpiando proyecto...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans
	@echo "$(GREEN)✓ Limpieza completada$(NC)"