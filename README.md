# Vaultwarden (Bitwarden) - Docker Deployment

Self-host Vaultwarden (lightweight Bitwarden server) using Docker and Docker Compose with persistent storage.

This repository is part of the **StackCrafted** project: https://stackcraftedyt.github.io/stackcrafted-org/

---

## 🔁 Important

Anywhere you see:

    vault.YOURDOMAIN.TLD

Replace it with **your own domain or subdomain**.

Example:

    vault.example.com

---

## 📦 What This Deploys

*   Vaultwarden server
*   Persistent data volume
*   Exposed on local port `8081` (for reverse proxy use)

You can place this behind **Nginx**, **Traefik**, **Caddy**, **Nginx Proxy Manager**, or any reverse proxy of your choice.

---

## 📁 Folder Structure

    vaultwarden-docker/
    ├── docker-compose.yml
    ├── .env
    └── data/

---

## ⚙️ Requirements

*   Linux server (VPS or local)
*   Docker installed
*   Docker Compose plugin installed

Check:

```bash
docker --version
docker compose version
