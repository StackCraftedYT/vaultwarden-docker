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
Use SSH local port forwarding to forward the VPS's localhost:8081 to your machine, then curl the forwarded port locally.

Example (from Windows PowerShell or CMD using OpenSSH ssh): 

``` bash
ssh -L 8081:127.0.0.1:8081 username@vps.example.com
```
Leave that SSH session open on a new terminal tab.
On your PC (new terminal) test with curl:

``` bash
curl -I http://127.0.0.1:8081
```
Expected output begins with the HTTP status line, e.g.:
``` bash
HTTP/1.1 200 OK
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


## 🔐 Secure Admin Token (Argon2)

To enable the admin panel (`/admin`) securely, generate a hashed admin token with Argon2:

```bash
echo -n "YourStrongPassword" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4 | sed 's#\$#\$\$#g'
```

## 🚦 Nginx Proxy Manager (NPM) Setup
For a fully automated HTTPS setup with Let’s Encrypt, you can deploy Nginx Proxy Manager alongside Vaultwarden.

1. Create a directory for NPM:
```bash
mkdir -p proxy-examples/nginx-proxy-manager
cd proxy-examples/nginx-proxy-manager
```

2. Create docker-compose.yml:
 
 ```yaml
version: '3.8'

services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: nginx-proxy-manager
    restart: unless-stopped
    ports:
      - '80:80'
      - '443:443'
      - '127.0.0.1:81:81'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    networks:
      - web-net

networks:
  web-net:
    external: true
 ```
3. Create the shared network (once):
```bash
docker network create web-net
```
4. Start NPM:
```bash
docker compose up -d
```
5. Access the admin panel via SSH tunnel:
```bash
   ssh -L 8181:localhost:81 user@your-server-ip
```
6. Then open http://localhost:8181 in your browser.
   
   vault.yourdomain.tld → forward to vaultwarden:80

npm.yourdomain.tld → forward to nginx-proxy-manager:81 (optional, for secure admin access)

Enable SSL using a Cloudflare wildcard certificate or Let’s Encrypt.


