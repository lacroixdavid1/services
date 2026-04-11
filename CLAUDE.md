# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a homelab Docker Compose services repository. Each subdirectory is an independent service stack deployed on a server named "tower" via **Komodo** (a Docker Compose orchestration tool). Komodo polls this git repo and auto-deploys changes.

## Security

**Never hardcode credentials or sensitive information in any file committed to this repository.** This includes passwords, tokens, API keys, and RTSP URLs containing credentials. Always use Komodo secrets (`[[SECRET_NAME]]`) injected as environment variables via the `environment` block in `komodo.toml`, then referenced as `${VAR_NAME}` in `compose.yaml` and in any config files that support env var substitution (e.g. Frigate's `config.yml`).

## Deployment

Deployment is fully managed by Komodo using `komodo.toml`. Pushing to the repo triggers Komodo to redeploy the affected stack. Each stack entry in `komodo.toml` maps to a subdirectory (`run_directory`) containing a `compose.yaml`. Secrets like `[[TS_AUTHKEY]]` are injected by Komodo at deploy time. Only Komodo should up and down the stacks (compose).

## Docker Network Architecture

See [README.md](README.md) for the full network layout and setup commands. Key rules when adding or modifying services:

- Join `uptime_kuma` on the **nginx** container, not the app container — when nginx is on multiple networks, Docker DNS can resolve service names to the wrong container on another network; container names are globally unique and always resolve correctly
- Join `monitoring` on any container that exposes Prometheus metrics
- Join `iot_vlan110` (with `driver_opts` endpoint name `eth110`) for direct IoT VLAN access
- Join `home_automation` for services that need to communicate with Home Assistant, Mosquitto, or Zigbee2MQTT

## Service Patterns

### Tailscale sidecar (private access via VPN)
Most services use this pattern for access over Tailscale:
- A `tailscale` sidecar container handles VPN connectivity
- An `nginx` container acts as a local reverse proxy (listening on 8080/8081)
- `ts-serve.json` configures Tailscale to forward traffic to nginx using `${TS_CERT_DOMAIN}` as the hostname variable (resolves to the service's fully qualified Tailscale hostname)
- The chain: `Tailscale → nginx (8080 HTTP redirect → 8081 HTTPS) → application container`
- All three containers share an internal `proxy` network (not external) defined per-stack for inter-container communication
- **Every service that exposes an HTTP interface must use an nginx sidecar** — do not use `network_mode: service:tailscale` to bypass this (that pattern is disallowed)
- **In `nginx.conf`, always use the container name** (e.g. `http://zigbee-app:8080`) as the `proxy_pass` upstream, never the service name (`application`). When nginx is on multiple Docker networks (e.g. `proxy` + `uptime_kuma`), Docker DNS can resolve the service name `application` to the wrong container (e.g. `uptime-kuma-app`) on another network. Container names are globally unique and always resolve correctly.

### Caddy reverse proxy (public internet access)
Services join the `caddy_proxy` network and are referenced directly by container name in `caddy/config/Caddyfile`.

### Both patterns
Some services (e.g., `home-assistant`, `monitoring/grafana`) join both `caddy_proxy` and use Tailscale.

## Host Hardware

"tower" is a **remote** Debian 12 VM — do not attempt to run `docker` or `docker compose` commands locally; all stack management goes through Komodo. It has the following resources:
- **CPU:** Intel i9-12900KF (16 cores)
- **RAM:** 32 GB
- **GPU:** AMD Radeon RX 6700 XT (Navi 22) — passed through to the VM; services that need GPU access (e.g., `ollama`) use it via device passthrough in their `compose.yaml`
- **Storage:** 512 GB (NVMe → `/mnt/data-nvme/`) and 3.6 TB (HDD → `/mnt/data/`)

### IoT Devices

- **SMLIGHT SLZB-MR4U** – multi-protocol coordinator at `192.168.110.7` (IoT VLAN 110)
  - Radio: EFR32MG26, currently in **Matter-over-Thread** (RCP) mode
  - Thread radio URL: use `bnutzer/otbr-tcp` image with `RCP_HOST=<coordinator-ip>` and `RCP_PORT=6638` — `spinel+hdlc+tcp://` does not work with this device; the socat bridge approach is required
  - Accessible via `iot_vlan110` Docker network (`eth110` interface)

## Data Persistence

Two storage tiers are available on the host:
- `/mnt/data-nvme/services/<service-name>/` – NVMe SSD; for config, state, and small/fast data
- `/mnt/data/` – HDD; for large data (media, archives, downloads, etc.)

Configuration files committed to this repo (like `nginx.conf`, `ts-serve.json`, `prometheus/`, `alertmanager/`) are bind-mounted read-only.

## Tailscale ACL Tags

Containers use `tag:container`, provisioned via `[[TS_AUTHKEY]]`. Known tags and rules:
- `tag:david-device` – personal devices; can access all users/devices on all ports
- `tag:openclaw` – inter-VM communication for the "openclaw" VM; can only access `tag:openclaw`
- `tag:container` – Docker Tailscale sidecars (standard `[[TS_AUTHKEY]]`)

When a sidecar needs to reach services tagged `tag:openclaw` (e.g. ollama), use `[[TS_AUTHKEY_OPENCLAW]]` instead of `[[TS_AUTHKEY]]`.

## Komodo Variables (Secrets)

Komodo supports two injection syntaxes:

**In `komodo.toml` `environment` blocks** — maps env var names to Komodo secrets:
```
TS_AUTHKEY = [[TS_AUTHKEY]]
TS_AUTHKEY_OPENCLAW = [[TS_AUTHKEY_OPENCLAW]]
```

**In `compose.yaml`** — two styles:
- `[[SECRET_NAME]]` – Komodo injects the secret value directly
- `${VAR_NAME}` – Docker Compose substitutes the env var set by Komodo's `environment` block

Most services use `TS_AUTHKEY=[[TS_AUTHKEY]]` directly. Services needing a specific Tailscale auth key (e.g. for inter-VM tags) declare a named var in `komodo.toml` and use `${VAR}` in `compose.yaml` — e.g. ollama uses `TS_AUTHKEY=${TS_AUTHKEY_OPENCLAW}` to get tags required for communicating with the "openclaw" VM.

Known secrets:
- `[[TS_AUTHKEY]]` – Tailscale auth key (most services)
- `[[TS_AUTHKEY_OPENCLAW]]` – Tailscale auth key with tags for inter-VM communication with "openclaw"
- `[[NC_PASS]]` – Namecheap DDNS password
- `[[UNPOLLER_UNIFI_PASS]]` – UniFi password for unpoller

## Adding a New Service

1. Create a subdirectory with a `compose.yaml`
2. Add a `[[stack]]` entry in `komodo.toml` pointing to that directory
3. If using Tailscale: add `ts-serve.json` and `nginx.conf` following the existing pattern (see `home-assistant/` as a reference)
4. Join the appropriate external networks (`caddy_proxy`, `monitoring`, `home_automation`, `iot_vlan110`)
5. Add the `nginx` container to the `uptime_kuma` external network so Uptime Kuma can monitor it (for caddy-only services without nginx, add the application container instead); use the container name in `nginx.conf` `proxy_pass`, not the service name
5. If exposing publicly via Caddy: add a route in `caddy/config/Caddyfile`
6. For private-registry images (e.g. ghcr.io): set `registry_provider` and `registry_account` in the `[[stack]]` config
