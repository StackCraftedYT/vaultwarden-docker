# Nginx Proxy Manager – Vaultwarden Proxy Setup

This example shows how to expose Vaultwarden using Nginx Proxy Manager with HTTPS.

---

## Proxy Host Settings

Domain Names:
vault.yourdomain.tld

Scheme:
http

Forward Hostname / IP:
vaultwarden

Forward Port:
80

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

https://vault.yourdomain.tld
