.PHONY: help install start stop restart logs status ps clean rebuild health init

help:
	@echo "NocoDB Docker Deployment - Makefile Commands"
	@echo ""
	@echo "  make install    - Initialize environment and start services"
	@echo "  make start      - Start all containers (detached)"
	@echo "  make stop       - Stop all containers"
	@echo "  make restart   - Restart all containers"
	@echo "  make logs      - View logs (follow mode)"
	@echo "  make status    - Show container status"
	@echo "  make ps         - List running containers"
	@echo "  make clean     - Remove containers and volumes"
	@echo "  make rebuild  - Rebuild and restart containers"
	@echo "  make health   - Check service health"

init:
	@echo "Creating directory structure..."
	@mkdir -p data redis
	@echo "Directory structure created"

install: init
	@echo "Installing NocoDB stack..."
	@docker compose up -d
	@echo "Installation complete. Access NocoDB at http://localhost:8080"

start:
	@docker compose up -d
	@echo "NocoDB started"

stop:
	@docker compose down
	@echo "NocoDB stopped"

restart: stop start

logs:
	@docker compose logs -f

status:
	@docker compose ps

ps:
	@docker compose ps

clean:
	@docker compose down -v
	@echo "Containers and volumes removed"

rebuild:
	@docker compose pull
	@docker compose up -d
	@echo "Rebuild complete"

health:
	@curl -sf http://localhost:8080/api/v1/version > /dev/null && echo "Service is healthy" || echo "Service is unreachable"