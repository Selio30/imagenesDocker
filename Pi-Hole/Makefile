# ==========================================
# 🚀 PI-HOLE HIGH AVAILABILITY (HA)
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = pihole-ha

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help check-env init up down restart status logs logs-sync clean network-check

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 PI-HOLE HA + ORBITAL SYNC"
	@echo "==========================================$(NC)"
	@echo "$(CYAN)Comandos disponibles:$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

network-check: ## Muestra las interfaces de red del host para configurar el .env
	@echo "$(CYAN)Busca tu interfaz principal (ej: eth0, ens18) para la variable MACVLAN_PARENT_INTERFACE:$(NC)"
	@ip -br a

check-env: ## Verifica que .env existe y las credenciales han sido configuradas
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)⚠️  Archivo .env no encontrado. Creando desde .env.example...$(NC)"; \
		cp .env.example .env; \
		echo "$(RED)❌ Por favor, edita el archivo .env con tus credenciales e IPs antes de continuar.$(NC)"; \
		exit 1; \
	fi
	@if grep -q "CHANGE_ME" .env; then \
		echo "$(RED)❌ ERROR: El archivo .env todavía contiene contraseñas por defecto (CHANGE_ME).$(NC)"; \
		echo "$(RED)⚠️  Debes cambiarlas antes de desplegar por seguridad.$(NC)"; \
		exit 1; \
	fi
	@echo "$(GREEN)✓ Archivo .env validado correctamente.$(NC)"

init: check-env ## Crea carpetas y despliega
	@echo "$(CYAN)Inicializando directorios...$(NC)"
	@mkdir -p primary/etc-pihole primary/etc-dnsmasq.d
	@mkdir -p secondary/etc-pihole secondary/etc-dnsmasq.d
	@echo "$(CYAN)Desplegando Pi-hole HA...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Stack HA desplegado$(NC)"

up: check-env ## Despliega los servicios
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d

down: ## Detiene los servicios
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia todos los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra estado
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra logs generales
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50

logs-sync: ## Muestra logs solo de Orbital Sync
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f orbital-sync

clean: ## Detiene y borra contenedores huerfanos
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans