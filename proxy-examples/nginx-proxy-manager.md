# Nginx Proxy Manager – Vaultwarden Proxy Setup

This example shows how to expose Vaultwarden using Nginx Proxy Manager with HTTPS.

---

## Proxy Host Settings

Domain Names:
vault.stackcrafted.org

Scheme:
http

Forward Hostname / IP:
127.0.0.1

Forward Port:
8080

Enable:
- Block Common Exploits
- Websockets Support

---

## SSL Settings

- Request a new SSL Certificate
- Force SSL
- HTTP/2 Support
- Agree to Let's Encrypt Terms

---

## Result

Vaultwarden will be reachable at:

https://vault.stackcrafted.org
