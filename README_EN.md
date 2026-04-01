# VPS Self-Host Services

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

> One-click deploy scripts for self-hosted services

---

## Docker Install

```bash
curl -fsSL https://get.docker.com | bash
systemctl start docker
systemctl enable docker
```

---

## Services

### Nextcloud (Cloud Storage)

```bash
docker run -d --name nextcloud -p 8080:80 -v nextcloud:/var/www/html --restart=always nextcloud
```

### Bitwarden (Password Manager)

```bash
docker run -d --name vaultwarden -p 80:80 -v vaultwarden:/data --restart=always vaultwarden/server:latest
```

### FreshRSS

```bash
docker run -d --name freshrss -p 8080:80 -v freshrss:/var/www/FreshRSS/data --restart=always freshrss/freshrss
```

---

## Resources

| Name | Link |
|------|------|
| **VPSVIP** | [Website](https://vpsvip.net) |
| Docker Hub | [Website](https://hub.docker.com) |

---

MIT License - 2026