# 🚀 HomeLab & Docker Deployments

Welcome to my central repository for containerized infrastructure and self-hosted applications. This repository contains production-ready Docker Compose stacks, complete with automation scripts (Makefiles) and extensive documentation tailored for home lab environments and VPS deployments.

## 📁 Project Index

Below is the catalog of currently deployed and documented infrastructure stacks:

### [1. MediaStack](./MediaStack/README.md)
Complete, automated media server ecosystem for downloading, organizing, and streaming multimedia content.
* **Media Servers:** Jellyfin, Immich (with Machine Learning & pgvecto.rs PostgreSQL)
* **Automation Suite:** Radarr, Sonarr, Lidarr, Readarr, Bazarr
* **Acquisition & Requests:** qBittorrent, Prowlarr, Jellyseerr
* **Key Features:** Hardlink optimization (TRaSH Guides), Intel QuickSync hardware transcoding, dedicated Docker network isolation, non-standard secure ports

### [2. NocoDB Enterprise Stack](./NocoDB/README.md)
Self-hosted Airtable alternative with Redis caching layer for high-performance.
* **Core Services:** NocoDB, Redis 7 (Alpine), SQLite
* **Key Features:** Bind-mounted persistent storage, production-ready restart policies, LAN-accessible via port 8080

---

## 🛠️ Global Architecture Principles

All projects within this repository adhere to the following deployment standards:
- **Atomic Operations:** Managed via Makefiles for consistent deployment lifecycles (specific targets like `make up` or `make start` vary by stack, see individual project documentation).
- **Security Awareness:** Services publish ports to the host by default for LAN access. For VPS or public deployments, explicit firewalling or localhost-binding (`127.0.0.1:PORT`) is required. Where applicable, sensitive variables are managed via `.env` files.
- **Network Isolation:** Each stack operates within its own dedicated Docker bridge network.
- **Reproducibility:** Images are version-pinned where necessary to prevent breaking upstream changes.

---

## 👨‍💻 Author
**Sergio Barbero** - [LinkedIn Profile](https://www.linkedin.com/in/Selio30)