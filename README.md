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
```

---

## 🚀 Basic Setup

### 1. Clone Repository

```bash
git clone https://github.com/StackCraftedYT/vaultwarden-docker.git
cd vaultwarden-docker
```

### 2. Configure Environment

Copy the example environment file and edit it:

```bash
cp .env.example .env
nano .env
```

Set your `DOMAIN` (e.g., `https://vault.example.com`). For the `ADMIN_TOKEN`, see the **Secure Admin Token** section below.

### 3. Start Vaultwarden

```bash
docker compose up -d
```

### 4. Verify Container

```bash
docker ps
```

You should see a running container named `vaultwarden`.

---

## 🔌 Ports & Local Testing

Vaultwarden listens on `127.0.0.1:8081`. This is intentional for reverse proxy usage.

You can test it locally using SSH port forwarding:

```bash
# On your local machine, run:
ssh -L 8081:127.0.0.1:8081 username@your-vps-ip
```

Leave that SSH session open. In a new local terminal, test the connection:

```bash
curl -I http://127.0.0.1:8081
```

Expected output begins with:

```
HTTP/1.1 200 OK
```

---

## 🔐 Secure Admin Token (Argon2)

To securely enable the admin panel (`/admin`), generate a hashed token using Argon2.

1.  Ensure `argon2` is installed (e.g., `sudo apt install argon2`).
2.  Run the following command:

    ```bash
    echo -n "YourStrongPassword" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4 | sed 's#\$#\$\$#g'
    ```
3.  Copy the **entire output** (it will start with `$$argon2id...`).
4.  Paste it as the value for the `ADMIN_TOKEN` variable in your `.env` file.

---

## 🚦 Nginx Proxy Manager (NPM) Setup

For an automated HTTPS setup with Let’s Encrypt, you can deploy Nginx Proxy Manager.

### 1. Create Directory and Configuration

```bash
mkdir -p proxy-examples/nginx-proxy-manager
cd proxy-examples/nginx-proxy-manager
```

Create a `docker-compose.yml` file with the following content:

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

### 2. Create Shared Network and Start NPM

Create the Docker network (only needed once):

```bash
docker network create web-net
```

Start the NPM container:

```bash
docker compose up -d
```

### 3. Access the NPM Admin Panel

For security, the admin panel is bound to localhost. Access it via an SSH tunnel from your local machine:

```bash
ssh -L 8181:localhost:81 user@your-server-ip
```

Then open `http://localhost:8181` in your browser. The default login is `admin@example.com` / `changeme`.

### 4. Configure Proxy Hosts in NPM

In the NPM admin interface, add a new **Proxy Host**:

*   **Domain Names:** `vault.yourdomain.tld`
*   **Scheme:** `http`
*   **Forward Hostname / IP:** `vaultwarden`
*   **Forward Port:** `80`
*   **SSL:** Select your certificate (e.g., Let's Encrypt or Cloudflare) and enable **"Force SSL"**.

*(Optional)* To access the NPM admin panel itself via HTTPS, you can create a second proxy host:
*   **Domain Names:** `npm.yourdomain.tld`
*   **Forward Hostname / IP:** `host.docker.internal`
*   **Forward Port:** `81`

---

## 🌐 Reverse Proxy (Required for Production)

You **must** place Vaultwarden behind a reverse proxy with HTTPS for a production setup. This guide provides instructions for **Nginx Proxy Manager** above. Other supported options include:

*   Nginx
*   Caddy
*   Traefik

---

## 🔐 Access (After Reverse Proxy + SSL)

*   **Vault URL:** `https://vault.yourdomain.tld`
*   **Admin Panel:** `https://vault.yourdomain.tld/admin` (requires the `ADMIN_TOKEN` set earlier)

---

## 🧱 Data Persistence

All Vaultwarden data is stored in the `./data` directory. **Do not delete this folder** unless you intend to wipe your Vaultwarden instance completely.

---

## 🔄 Updating

```bash
docker compose pull
docker compose up -d
```

---

## 🧯 Stopping

```bash
docker compose down
```

---

## 📚 Related Resources

*   **Tutorial Website:** https://stackcraftedyt.github.io/stackcrafted-org/tutorials/
*   **Official Vaultwarden Wiki:** https://github.com/dani-garcia/vaultwarden/wiki# Vaultwarden (Bitwarden) - Docker Deployment

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
```

---

## 🚀 Basic Setup

### 1. Clone Repository

```bash
git clone https://github.com/StackCraftedYT/vaultwarden-docker.git
cd vaultwarden-docker
```

### 2. Configure Environment

Copy the example environment file and edit it:

```bash
cp .env.example .env
nano .env
```

Set your `DOMAIN` (e.g., `https://vault.example.com`). For the `ADMIN_TOKEN`, see the **Secure Admin Token** section below.

### 3. Start Vaultwarden

```bash
docker compose up -d
```

### 4. Verify Container

```bash
docker ps
```

You should see a running container named `vaultwarden`.

---

## 🔌 Ports & Local Testing

Vaultwarden listens on `127.0.0.1:8081`. This is intentional for reverse proxy usage.

You can test it locally using SSH port forwarding:

```bash
# On your local machine, run:
ssh -L 8081:127.0.0.1:8081 username@your-vps-ip
```

Leave that SSH session open. In a new local terminal, test the connection:

```bash
curl -I http://127.0.0.1:8081
```

Expected output begins with:

```
HTTP/1.1 200 OK
```

---

## 🔐 Secure Admin Token (Argon2)

To securely enable the admin panel (`/admin`), generate a hashed token using Argon2.

1.  Ensure `argon2` is installed (e.g., `sudo apt install argon2`).
2.  Run the following command:

    ```bash
    echo -n "YourStrongPassword" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4 | sed 's#\$#\$\$#g'
    ```
3.  Copy the **entire output** (it will start with `$$argon2id...`).
4.  Paste it as the value for the `ADMIN_TOKEN` variable in your `.env` file.

---

## 🚦 Nginx Proxy Manager (NPM) Setup

For an automated HTTPS setup with Let’s Encrypt, you can deploy Nginx Proxy Manager.

### 1. Create Directory and Configuration

```bash
mkdir -p proxy-examples/nginx-proxy-manager
cd proxy-examples/nginx-proxy-manager
```

Create a `docker-compose.yml` file with the following content:

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

### 2. Create Shared Network and Start NPM

Create the Docker network (only needed once):

```bash
docker network create web-net
```

Start the NPM container:

```bash
docker compose up -d
```

### 3. Access the NPM Admin Panel

For security, the admin panel is bound to localhost. Access it via an SSH tunnel from your local machine:

```bash
ssh -L 8181:localhost:81 user@your-server-ip
```

Then open `http://localhost:8181` in your browser. The default login is `admin@example.com` / `changeme`.

### 4. Configure Proxy Hosts in NPM

In the NPM admin interface, add a new **Proxy Host**:

*   **Domain Names:** `vault.yourdomain.tld`
*   **Scheme:** `http`
*   **Forward Hostname / IP:** `vaultwarden`
*   **Forward Port:** `80`
*   **SSL:** Select your certificate (e.g., Let's Encrypt or Cloudflare) and enable **"Force SSL"**.

*(Optional)* To access the NPM admin panel itself via HTTPS, you can create a second proxy host:
*   **Domain Names:** `npm.yourdomain.tld`
*   **Forward Hostname / IP:** `host.docker.internal`
*   **Forward Port:** `81`

---

## 🌐 Reverse Proxy (Required for Production)

You **must** place Vaultwarden behind a reverse proxy with HTTPS for a production setup. This guide provides instructions for **Nginx Proxy Manager** above. Other supported options include:

*   Nginx
*   Caddy
*   Traefik

---

## 🔐 Access (After Reverse Proxy + SSL)

*   **Vault URL:** `https://vault.yourdomain.tld`
*   **Admin Panel:** `https://vault.yourdomain.tld/admin` (requires the `ADMIN_TOKEN` set earlier)

---

## 🧱 Data Persistence

All Vaultwarden data is stored in the `./data` directory. **Do not delete this folder** unless you intend to wipe your Vaultwarden instance completely.

---

## 🔄 Updating

```bash
docker compose pull
docker compose up -d
```

---

## 🧯 Stopping

```bash
docker compose down
```

---

## 📚 Related Resources

*   **Tutorial Website:** https://stackcraftedyt.github.io/stackcrafted-org/tutorials/
*   **Official Vaultwarden Wiki:** https://github.com/dani-garcia/vaultwarden/wiki
