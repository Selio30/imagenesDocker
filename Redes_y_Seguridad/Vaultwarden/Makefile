# ==========================================
# 🚀 VAULTWARDEN (Gestor de Contraseñas)
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = vaultwarden-vault

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help check-env generate-secret init up down restart status logs clean

help:
	@echo "$(CYAN)=========================================="
	@echo "🛡️  VAULTWARDEN SECURE VAULT"
	@echo "==========================================$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

check-env: ## Verifica que .env existe y no tiene secretos por defecto
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)⚠️  Archivo .env no encontrado. Creando desde .env.example...$(NC)"; \
		cp .env.example .env; \
	fi
	@if grep -q "CHANGE_ME" .env || grep -qE "^ADMIN_TOKEN=[[:space:]]*$$" .env || grep -qE "^MYSQL_ROOT_PASSWORD=[[:space:]]*$$" .env || grep -qE "^DB_PASSWORD=[[:space:]]*$$" .env; then \
		echo "$(RED)❌ ERROR: El archivo .env contiene contraseñas por defecto o vacías.$(NC)"; \
		echo "$(YELLOW)💡 Ejecuta 'make generate-secret' para crear/rotar claves seguras.$(NC)"; \
		exit 1; \
	fi
	@echo "$(GREEN)✓ Archivo .env validado.$(NC)"

generate-secret: ## Genera claves criptográficas y las inyecta en el .env
	@if [ ! -f .env ]; then cp .env.example .env; fi
	@echo "$(CYAN)Generando (o rotando) secretos seguros...$(NC)"
	@TOKEN=$$(openssl rand -base64 48 | tr -d '\n' | sed 's/[\/&]/\\&/g'); \
	ROOT_PASS=$$(openssl rand -base64 32 | tr -d '\n' | sed 's/[\/&]/\\&/g'); \
	PASS=$$(openssl rand -base64 24 | tr -d '\n' | sed 's/[\/&]/\\&/g'); \
	sed -i.bak "s/^ADMIN_TOKEN=.*/ADMIN_TOKEN=$$TOKEN/" .env; \
	sed -i.bak "s/^MYSQL_ROOT_PASSWORD=.*/MYSQL_ROOT_PASSWORD=$$ROOT_PASS/" .env; \
	sed -i.bak "s/^DB_PASSWORD=.*/DB_PASSWORD=$$PASS/" .env; \
	rm -f .env.bak
	@echo "$(GREEN)✓ Secretos criptográficos inyectados en .env$(NC)"

init: check-env ## Inicializa el despliegue de la bóveda
	@echo "$(CYAN)Desplegando la bóveda de Vaultwarden...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Stack Vaultwarden desplegado$(NC)"

up: check-env ## Levanta los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d

down: ## Detiene los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia el servicio
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra estado de salud de la BD y la bóveda
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra logs del servidor Vaultwarden
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50 vaultwarden

clean: ## Limpia contenedores huérfanos
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans