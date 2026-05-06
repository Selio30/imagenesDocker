# 🚀 HomeLab & Docker Deployments

Welcome to my central repository for containerized infrastructure and self-hosted applications. This repository contains production-ready Docker Compose stacks, complete with automation scripts (Makefiles) and extensive documentation tailored for home lab environments and VPS deployments.

## 📁 Project Index

Below is the catalog of currently deployed and documented infrastructure stacks:

### [1. MediaStack](./MediaStack/README.md)
Complete, automated media server ecosystem for downloading, organizing, and streaming multimedia content.
* **Core Services:** Jellyfin, Immich (ML & pgvecto.rs), ARR Suite (Radarr, Sonarr, etc.).
* **Key Features:** Hardlink optimization, Intel QuickSync hardware transcoding, and internal network isolation.

### [2. NocoDB Enterprise Stack](./NocoDB/README.md)
Self-hosted Airtable alternative with a Redis caching layer for high-performance data management.
* **Core Services:** NocoDB, Redis 7 (Alpine), SQLite.
* **Key Features:** Bind-mounted persistent storage and production-ready restart policies.

### [3. GLPI HelpDesk Stack](./GLPI/README.md)
IT Asset Management and Service Desk platform optimized for technical department operations.
* **Core Services:** GLPI (Apache/PHP), MariaDB 10.11.
* **Key Features:** Dedicated database healthchecks and secure environment variable isolation.

### [4. Portainer CE](./Portainer/README.md)
Centralized Docker management interface for visual orchestration.
* **Key Features:** Full orchestration via Docker Socket (RW), web-based log monitoring, and secure HTTPS access.

### [5. DuckDNS Updater](./duckdns/README.md)
Enterprise-grade Dynamic DNS management for real-time IP synchronization.
* **Key Features:** Host network detection, automated Makefile validation, and unprivileged execution.

---

## 🛠️ Global Architecture Principles

All projects within this repository adhere to the following deployment standards:
- **Atomic Operations:** Managed via Makefiles for consistent deployment lifecycles (specific targets like `make up`, `make start` or `make init` vary by stack, see individual project documentation).
- **Security Awareness:** Services publish ports to the host by default for LAN access. For VPS or public deployments, explicit firewalling or localhost-binding (`127.0.0.1:PORT`) is required. Where applicable, sensitive variables are managed via `.env` files.
- **Network Isolation:** Each stack operates within its own dedicated Docker bridge network or uses host mode for specific network detection needs.
- **Reproducibility:** Images are version-pinned where necessary to prevent breaking upstream changes.

---

## 👨‍💻 Author
**Sergio Barbero** - [LinkedIn Profile](https://www.linkedin.com/in/Selio30)