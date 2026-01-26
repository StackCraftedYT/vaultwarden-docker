# Vaultwarden Docker Deployment

Docker Compose deployment for Vaultwarden (Bitwarden-compatible password manager).

This repository is part of the StackCrafted self-hosting tutorials.

---

## Requirements

- Linux server or VPS
- Docker installed
- Docker Compose installed
- Reverse proxy with HTTPS (Nginx Proxy Manager or Caddy)

---

## Setup

Clone repository:

```bash
git clone https://github.com/StackCraftedYT/vaultwarden-docker.git
cd vaultwarden-docker
```

Create environment file:

```bash
cp .env.example .env
nano .env
```

Set a random admin token:

```env
ADMIN_TOKEN=your_random_string_here
```

Start container:

```bash
docker compose up -d
```

---

## Access

Vault URL:

```
https://vault.stackcrafted.org
```

Admin Panel:

```
https://vault.stackcrafted.org/admin
```

---

## Updates

```bash
docker compose pull
docker compose up -d
```

---

## Backup

```bash
tar -czvf vaultwarden-backup.tar.gz data/
```

---

## Reverse Proxy Examples

- Nginx Proxy Manager: proxy-examples/nginx-proxy-manager.md

---

## License

MIT
