# 🚀 HomeLab & Docker Deployments

Welcome to my central repository for containerized infrastructure and self-hosted applications. This repository contains production-ready Docker Compose stacks, complete with automation scripts (Makefiles) and extensive documentation tailored for home lab environments, VPS deployments, and IT department labs.

## 📁 Project Index

Below is the catalog of currently deployed and documented infrastructure stacks, organized by functional category:

### 🛡️ Redes y Seguridad (Networks & Security)

* **[Blocky GitOps DNS](./Redes_y_Seguridad/Blocky/README.md)**
  Privacy-first, GitOps-powered DNS-level content filtering proxy with DoT upstreams.
  * **Key Features:** Zero-IP ad blocking, declarative YAML configuration, parallel DNS-over-TLS encryption, and hot-reload API.

* **[Pi-Hole HA DNS Sinkhole](./Redes_y_Seguridad/Pi-Hole/README.md)**
  Network-wide ad blocking and DNS filtering engine for implicit protection of every LAN device.
  * **Key Features:** High Availability Macvlan topology, Orbital Sync orchestration, FTLDNS-based resolution, and web admin dashboard.

* **[DuckDNS Updater](./Redes_y_Seguridad/duckdns/README.md)**
  Enterprise-grade Dynamic DNS management for real-time IP synchronization.
  * **Key Features:** Host network detection, automated Makefile validation, and unprivileged execution.

### ⚙️ Gestión e Infraestructura (Management & Infrastructure)

* **[GLPI HelpDesk Stack](./Gestion_e_Infraestructura/GLPI/README.md)**
  IT Asset Management and Service Desk platform optimized for technical department operations.
  * **Core Services:** GLPI (Apache/PHP), MariaDB 10.11.
  * **Key Features:** Health-checked dependency orchestration, loopback binding for security, and externalized secrets.

* **[Portainer CE](./Gestion_e_Infraestructura/Portainer/README.md)**
  Centralized Docker management interface for visual orchestration.
  * **Key Features:** Full orchestration via Docker Socket (RW), loopback-bound HTTPS access, and automated lifecycle management.

### 🗄️ Aplicaciones y Datos (Apps & Data)

* **[NocoDB Enterprise Stack](./Aplicaciones_y_Datos/NocoDB/README.md)**
  Self-hosted Airtable alternative with a Redis caching layer for high-performance data management.
  * **Core Services:** NocoDB, Redis 7 (Alpine), SQLite.
  * **Key Features:** Bind-mounted persistent storage and production-ready restart policies.

### 🍿 Multimedia

* **[MediaStack](./Multimedia/MediaStack/README.md)**
  Complete, automated media server ecosystem for downloading, organizing, and streaming multimedia content.
  * **Core Services:** Jellyfin, Immich (ML & pgvecto.rs), ARR Suite (Radarr, Sonarr, etc.).
  * **Key Features:** Hardlink optimization, Intel QuickSync hardware transcoding, and internal network isolation.

---

## 🛠️ Global Architecture Principles

All projects within this repository adhere to the following deployment standards:
- **Atomic Operations:** Managed via Makefiles for consistent deployment lifecycles. Standard targets typically include `make init`, `make up` (or `make start`, depending on the stack's specific legacy), and `make clean`.
- **Security Awareness:** Where appropriate for administrative and management tools (e.g., Portainer, GLPI), sensitive UI endpoints are loopback-bound (`127.0.0.1`) by default to prevent unauthorized network exposure. Services intended for LAN broadcast (e.g., MediaStack) expose ports directly. For remote deployments, explicit reverse proxying is required. Sensitive variables are strictly managed via `.env` files and excluded from version control.
- **Network Isolation:** Each stack operates within its own dedicated Docker bridge network or uses host mode exclusively for specific network detection needs.
- **Reproducibility:** Images are version-pinned where stability is critical, or tracked to `:latest` with explicit documentation for lab update workflows.

---

## 👨‍💻 Author
**Sergio Barbero** - [LinkedIn Profile](https://www.linkedin.com/in/Selio30)
