# ==========================================
# 🚀 HOMELAB DEPLOYMENT MANAGER
# ==========================================

COMPOSE_FILE = docker-compose.yml
PROJECT_NAME = mediastack

CYAN := \033[0;36m
GREEN := \033[0;32m
YELLOW := \033[0;33m
RED := \033[0;31m
NC := \033[0m

.PHONY: help up down restart logs status ps clean check-env init logs-media logs-downloads logs-arr

help:
	@echo "$(CYAN)=========================================="
	@echo "🚀 HOMELAB MEDIA SERVER"
	@echo "==========================================$(NC)"
	@echo ""
	@echo "$(CYAN)Servicios disponibles:$(NC)"
	@echo "  • Jellyfin   - http://localhost:18001"
	@echo "  • Radarr    - http://localhost:17878"
	@echo "  • Sonarr    - http://localhost:18089"
	@echo "  • Bazarr    - http://localhost:16767"
	@echo "  • Lidarr    - http://localhost:18686"
	@echo "  • Readarr   - http://localhost:18787"
	@echo "  • Prowlarr  - http://localhost:19696"
	@echo "  • qBittorrent - http://localhost:18091"
	@echo "  • Overseerr - http://localhost:15055"
	@echo "  • Immich    - http://localhost:18000"
	@echo ""
	@echo "$(CYAN)Comandos disponibles:$(NC)"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  $(GREEN)%-18s$(NC) %s\n", $$1, $$2}'

check-env:
	@if [ ! -f .env ]; then \
		echo "$(YELLOW)Creando archivo .env...$(NC)"; \
		echo "# Immich Database Configuration" > .env; \
		echo "DB_PASSWORD=change_me" >> .env; \
		echo "DB_USERNAME=postgres" >> .env; \
		echo "DB_DATABASE_NAME=immich" >> .env; \
		echo "DB_HOSTNAME=immich-postgres" >> .env; \
		echo "$(GREEN).env creado. Por favor, modifica DB_PASSWORD antes de continuar.$(NC)"; \
	fi

init: check-env ## Inicializa carpetas y despliega servicios
    @grep -q '^DB_PASSWORD=change_me$$' .env && echo "$(RED)Error: ¡Debes cambiar DB_PASSWORD en el archivo .env antes de iniciar!$(NC)" && exit 1 || true
    @echo "$(CYAN)Inicializando estructura de carpetas...$(NC)"
    @mkdir -p config data
    @docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) run --rm init-folders
    @echo "$(CYAN)Desplegando servicios...$(NC)"
    @docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
    @echo "$(GREEN)✓ Stack desplegado con éxito$(NC)"

up: check-env ## Despliega todos los servicios
    @grep -q '^DB_PASSWORD=change_me$$' .env && echo "$(RED)Error: ¡Debes cambiar DB_PASSWORD en el archivo .env antes de iniciar!$(NC)" && exit 1 || true
    @echo "$(CYAN)Desplegando servicios...$(NC)"
    @docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d
    @echo "$(GREEN)✓ Servicios iniciados$(NC)"

down: ## Detiene todos los contenedores
	@echo "$(CYAN)Deteniendo servicios...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down

restart: ## Reinicia todos los servicios
	@echo "$(CYAN)Reiniciando servicios...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) restart

ps: ## Muestra estado de contenedores
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) ps

status: ps ## Alias de ps

logs: ## Muestra logs en tiempo real de todos los servicios
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f --tail=50

logs-media: ## Logs de Jellyfin
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f jellyfin

logs-arr: ## Logs de Radarr/Sonarr
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f radarr sonarr

logs-downloads: ## Logs de qBittorrent/Prowlarr
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f qbittorrent prowlarr

logs-immich: ## Logs de Immich
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) logs -f immich-server

update: check-env ## Actualiza imágenes y reinicia
	@echo "$(CYAN)Buscando actualizaciones...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) pull
	@echo "$(CYAN)Re-desplegando...$(NC)"
	@docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) up -d --remove-orphans
	@echo "$(GREEN)✓ Actualización completada$(NC)"

clean: ## Limpia recursos del proyecto (sin afectar al host)
    @echo "$(CYAN)Limpiando proyecto...$(NC)"
    @docker compose -f $(COMPOSE_FILE) -p $(PROJECT_NAME) down --remove-orphans
    @echo "$(GREEN)✓ Limpieza completada$(NC)"

shell-jellyfin: ## Accede a Jellyfin
	@docker exec -it jellyfin sh

shell-radarr: ## Accede a Radarr
	@docker exec -it radarr sh

shell-sonarr: ## Accede a Sonarr
	@docker exec -it sonarr sh

shell-qbittorrent: ## Accede a qBittorrent
	@docker exec -it qbittorrent sh

shell-immich: ## Accede a Immich
	@docker exec -it immich_server sh

shell-lidarr: ## Accede a Lidarr
	@docker exec -it lidarr sh

shell-readarr: ## Accede a Readarr
	@docker exec -it readarr sh

shell-bazarr: ## Accede a Bazarr
	@docker exec -it bazarr sh

shell-prowlarr: ## Accede a Prowlarr
	@docker exec -it prowlarr sh

shell-seerr: ## Accede a Overseerr
	@docker exec -it seerr sh
