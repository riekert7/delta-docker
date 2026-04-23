# Delta Docker

Self-hosted service stack using Docker Compose, routed through a Cloudflare tunnel — no open inbound ports, no reverse proxy configuration required.

## Services

| Service | Image | Purpose |
|---------|-------|---------|
| **Cloudflared** |  | Cloudflare tunnel — exposes services publicly without opening firewall ports |
| **n8n** |  | Workflow automation, accessible on port 5678 locally |
| **WAHA** |  | WhatsApp HTTP API — scan once, send/receive messages via REST on port 3000 |

## Architecture

All services share a Docker network named . Cloudflared routes external HTTPS traffic into that network, so n8n and WAHA are reachable from the internet without exposing any host ports.



## Setup

Each service lives in its own folder with a  and a  file (not committed). Start Cloudflared first — it creates the shared network.



## Related

- [whatsapp_on_local](https://github.com/riekert7/whatsapp_on_local) — lightweight local WhatsApp gateway used alongside n8n for webhook-triggered messages
