# services

Homelab Docker Compose stacks deployed on a self-hosted server via [Komodo](https://komo.do). Each subdirectory is an independent stack. Komodo polls this repo and auto-deploys on push.

## Stacks

| Stack | Description |
|---|---|
| `adguard-home` | Network-wide DNS and ad blocking |
| `arr` | Sonarr / Radarr / Prowlarr media management |
| `caddy` | Reverse proxy for public-facing services |
| `calibre-web` | Ebook library |
| `changedetection` | Web page change monitoring |
| `cloudflare-ddns` | Dynamic DNS updates via Cloudflare |
| `cloudflared` | Cloudflare Tunnel for public access |
| `emby` | Media server |
| `esphome` | ESP microcontroller firmware management |
| `frigate` | NVR with object detection |
| `home-assistant` | Home automation hub (with PostgreSQL) |
| `jdownloader` | Download manager |
| `matter` | Matter controller |
| `monitoring` | Prometheus + Grafana + Loki + Alertmanager |
| `mosquitto` | MQTT broker |
| `n8n` | Workflow automation |
| `namecheap-ddns` | Dynamic DNS updates via Namecheap |
| `ollama` | Local LLM inference |
| `otbr` | OpenThread Border Router |
| `postgres` | Standalone PostgreSQL instance |
| `qbittorrent` | BitTorrent client |
| `rabbitmq` | Message broker |
| `speedtest` | Periodic internet speed tracking |
| `trilium` | Personal knowledge base |
| `unpoller` | UniFi metrics exporter for Prometheus |
| `uptime-kuma` | Service uptime monitoring |
| `zigbee` | Zigbee2MQTT coordinator |

## Prerequisites

Create the external Docker networks before deploying:

```bash
docker network create -d ipvlan \
  --subnet=192.168.110.0/24 --gateway=192.168.110.1 \
  -o parent=enp6s19 -o ipvlan_mode=l2 iot_vlan110

docker network create -d ipvlan \
  --subnet=192.168.130.0/24 --gateway=192.168.130.1 \
  -o parent=enp6s18 -o ipvlan_mode=l2 services_vlan130

docker network create caddy_proxy
docker network create monitoring
docker network create home_automation
docker network create uptime_kuma
docker network create cloudflare_tunnel
```

## Secrets

All secrets are managed via Komodo and injected at deploy time using `[[SECRET_NAME]]` syntax in `komodo.toml`. No credentials are stored in this repository.

## Access Patterns

- **Tailscale** — most services are accessible privately over Tailscale via an nginx sidecar
- **Caddy** — select services are exposed publicly via HTTPS through the Caddy reverse proxy
