# ==========================================
# 🚀 PI-HOLE DEPLOYMENT MANAGER
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = pihole

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help check-env init up down restart status logs clean

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 PI-HOLE DNS AD BLOCKER"
	@echo "==========================================$(NC)"
	@echo "$(CYAN)Información:$(NC)"
	@echo "  • DNS sinkhole para bloquear publicidad y rastreadores"
	@echo "  • Interfaz web disponible en: http://localhost:80/admin"
	@echo ""
	@echo "$(CYAN)Comandos disponibles:$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

check-env:
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)Creando archivo .env a partir de .env.example...$(NC)"; \
		cp .env.example .env; \
		echo "$(GREEN).env creado. Por favor, edita el archivo con tu contraseña.$(NC)"; \
	fi

init: check-env ## Inicializa el entorno y despliega el servicio
	@mkdir -p etc-pihole etc-dnsmasq.d
	@grep -E -q 'tu_contraseña_admin' .env && echo "$(RED)Error: ¡Debes configurar una WEBPASSWORD válida en el archivo .env antes de iniciar!$(NC)" && exit 1 || true
	@echo "$(CYAN)Desplegando Pi-Hole...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Contenedor desplegado con éxito$(NC)"
	@echo "$(GREEN)  Interfaz web: http://localhost:80/admin$(NC)"

up: check-env ## Despliega el servicio
	@mkdir -p etc-pihole etc-dnsmasq.d
	@grep -E -q 'tu_contraseña_admin' .env && echo "$(RED)Error: ¡Debes configurar una WEBPASSWORD válida en el archivo .env antes de iniciar!$(NC)" && exit 1 || true
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
