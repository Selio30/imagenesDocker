# ==========================================
# 🚀 AUTHENTIK (Identity Provider)
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = authentik-sso

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help check-env generate-secret init up down restart status logs clean

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 AUTHENTIK SSO & IdP"
	@echo "==========================================$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

check-env: ## Verifica que .env existe y no tiene secretos por defecto
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)⚠️  Archivo .env no encontrado. Creando desde .env.example...$(NC)"; \
		cp .env.example .env; \
	fi
	@if grep -q "CHANGE_ME" .env; then \
		echo "$(RED)❌ ERROR: El archivo .env contiene contraseñas CHANGE_ME.$(NC)"; \
		echo "$(YELLOW)💡 Ejecuta 'make generate-secret' para crear claves seguras automáticamente.$(NC)"; \
		exit 1; \
	fi
	@echo "$(GREEN)✓ Archivo .env validado.$(NC)"

generate-secret: ## Genera claves criptográficas y las inyecta en el .env
	@if [ ! -f .env ]; then cp .env.example .env; fi
	@echo "$(CYAN)Generando secretos seguros...$(NC)"
	@sed -i 's/CHANGE_ME_SECRET_KEY/'$$(openssl rand -base64 36 | tr -d '\n' | sed 's/[\/&]/\\&/g')'/g' .env
	@sed -i 's/CHANGE_ME_DB_PASSWORD/'$$(openssl rand -base64 24 | tr -d '\n' | sed 's/[\/&]/\\&/g')'/g' .env
	@echo "$(GREEN)✓ Secretos criptográficos inyectados en .env$(NC)"

init: check-env ## Inicializa el despliegue
	@echo "$(CYAN)Desplegando infraestructura de Authentik...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
	@echo "$(GREEN)✓ Stack Authentik desplegado$(NC)"

up: check-env ## Levanta los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d

down: ## Detiene los contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia el servicio
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

status: ## Muestra estado de salud de la BD, Redis, Web y Worker
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

logs: ## Muestra logs del servidor HTTP
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50 server

clean: ## Limpia contenedores huerfanos
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans