# Delta Docker

Self-hosted service stack using Docker Compose, routed through a Cloudflare tunnel — no open inbound ports, no reverse proxy configuration required.

## Services

| Service | Image | Purpose |
|---------|-------|---------|
| **Cloudflared** | `cloudflare/cloudflared` | Cloudflare tunnel — exposes services publicly without opening firewall ports |
| **n8n** | `n8nio/n8n` | Workflow automation, accessible on port 5678 locally |
| **WAHA** | `devlikeapro/waha` | WhatsApp HTTP API — scan once, send/receive messages via REST on port 3000 |

## Architecture

All services share a Docker network named `cf-tunnel`. Cloudflared routes external HTTPS traffic into that network, so n8n and WAHA are reachable from the internet without exposing any host ports.

```
Internet → Cloudflare Edge → cloudflared container → cf-tunnel network → n8n / WAHA
```

## Setup

Each service lives in its own folder with a `docker-compose.yml` and a `.env` file (not committed). Start Cloudflared first — it creates the shared network.

```bash
cd cloudflared && docker compose up -d
cd ../n8n      && docker compose up -d
cd ../waha     && docker compose up -d
```

## Related

- [whatsapp_on_local](https://github.com/riekert7/whatsapp_on_local) — lightweight local WhatsApp gateway used alongside n8n for webhook-triggered messages
