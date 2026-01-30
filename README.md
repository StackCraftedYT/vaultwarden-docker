# Vaultwarden (Bitwarden) - Docker Deployment

Self-host Vaultwarden (lightweight Bitwarden server) using Docker and
Docker Compose with persistent storage.

This repository is part of the **StackCrafted** project:
https://stackcraftedyt.github.io/stackcrafted-org/

------------------------------------------------------------------------

## 🔁 Important

Anywhere you see:

    vault.YOURDOMAIN.TLD

Replace it with **your own domain or subdomain**.

Example:

    vault.example.com

------------------------------------------------------------------------

## 📦 What this deploys

-   Vaultwarden server
-   Persistent data volume
-   Exposed on local port `8081` (for reverse proxy use)

You can place this behind **Nginx**, **Traefik**, **Caddy**, or any
reverse proxy of your choice.

------------------------------------------------------------------------

## 📁 Folder Structure

    vaultwarden-docker/
    ├── docker-compose.yml
    └── data/

------------------------------------------------------------------------

## ⚙️ Requirements

-   Linux server (VPS or local)
-   Docker installed
-   Docker Compose plugin installed

Check:

``` bash
docker --version
docker compose version
```

------------------------------------------------------------------------

## 🚀 Setup

### 1. Clone repository

``` bash
git clone https://github.com/StackCraftedYT/vaultwarden-docker.git
cd vaultwarden-docker
```

------------------------------------------------------------------------

### 2. Start Vaultwarden

``` bash
docker compose up -d
```

------------------------------------------------------------------------

### 3. Verify container

``` bash
docker ps
```

You should see a running container named `vaultwarden`.

------------------------------------------------------------------------

## 🔌 Ports

Vaultwarden listens on:

    127.0.0.1:8081

This is intentional for reverse proxy usage.

Test locally:

``` bash
curl http://127.0.0.1:8081
```

------------------------------------------------------------------------

## 🌐 Reverse Proxy (Required)

You must place Vaultwarden behind a reverse proxy with HTTPS.

Supported examples:

-   Nginx
-   Caddy
-   Traefik

The StackCrafted tutorial site provides a full Nginx + SSL walkthrough.

------------------------------------------------------------------------

## 🔐 Access (after reverse proxy + SSL)

Vault URL:

    https://vault.YOURDOMAIN.TLD

Admin Panel:

    https://vault.YOURDOMAIN.TLD/admin

------------------------------------------------------------------------

## 🧱 Data Persistence

All Vaultwarden data is stored in:

    ./data

Do not delete this folder unless you want to wipe Vaultwarden.

------------------------------------------------------------------------

## 🔄 Updating

``` bash
docker compose pull
docker compose up -d
```

------------------------------------------------------------------------

## 🧯 Stopping

``` bash
docker compose down
```

------------------------------------------------------------------------

## 📚 Related

Tutorial Website:

https://stackcraftedyt.github.io/stackcrafted-org/tutorials/

------------------------------------------------------------------------

## ✅ Status

This repository contains only container deployment.

SSL and domain configuration are handled in the tutorial documentation.
