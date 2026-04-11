# services

Homelab Docker Compose stacks deployed on a self-hosted server via [Komodo](https://komo.do). Each subdirectory is an independent stack. Komodo polls this repo and auto-deploys on push.

## Stacks

| Stack | Description |
|---|---|
| [adguard-home](adguard-home/) | Network-wide DNS and ad blocking |
| [arr](arr/) | Sonarr / Radarr / Prowlarr media management |
| [caddy](caddy/) | Reverse proxy for public-facing services |
| [calibre-web](calibre-web/) | Ebook library |
| [changedetection](changedetection/) | Web page change monitoring |
| [cloudflare-ddns](cloudflare-ddns/) | Dynamic DNS updates via Cloudflare |
| [cloudflared](cloudflared/) | Cloudflare Tunnel for public access |
| [emby](emby/) | Media server |
| [esphome](esphome/) | ESP microcontroller firmware management |
| [frigate](frigate/) | NVR with object detection |
| [home-assistant](home-assistant/) | Home automation hub (with PostgreSQL) |
| [jdownloader](jdownloader/) | Download manager |
| [matter](matter/) | Matter controller |
| [monitoring](monitoring/) | Prometheus + Grafana + Loki + Alertmanager |
| [mosquitto](mosquitto/) | MQTT broker |
| [n8n](n8n/) | Workflow automation |
| [namecheap-ddns](namecheap-ddns/) | Dynamic DNS updates via Namecheap |
| [ollama](ollama/) | Local LLM inference |
| [otbr](otbr/) | OpenThread Border Router |
| [postgres](postgres/) | Standalone PostgreSQL instance |
| [qbittorrent](qbittorrent/) | BitTorrent client |
| [rabbitmq](rabbitmq/) | Message broker |
| [speedtest](speedtest/) | Periodic internet speed tracking |
| [trilium](trilium/) | Personal knowledge base |
| [unpoller](unpoller/) | UniFi metrics exporter for Prometheus |
| [uptime-kuma](uptime-kuma/) | Service uptime monitoring |
| [zigbee](zigbee/) | Zigbee2MQTT coordinator |

## Network Architecture

The server runs multiple isolated VLANs managed by a UniFi gateway. Docker containers join these networks directly via ipvlan L2, allowing them to appear as first-class devices on each VLAN.

### VLANs

| VLAN | Subnet | Purpose |
|---|---|---|
| IoT (110) | `192.168.110.0/24` | IoT devices, coordinators, sensors |
| Services (130) | `192.168.130.0/24` | Server infrastructure and self-hosted services |

### Docker Networks

| Network | Type | Purpose |
|---|---|---|
| `iot_vlan110` | ipvlan L2 (`enp6s19`) | Direct access to IoT VLAN — used by Home Assistant, OTBR, Frigate |
| `services_vlan130` | ipvlan L2 (`enp6s18`) | Stable IP on services VLAN — used by AdGuard Home for network-wide DNS |

> Each VLAN uses a dedicated vNIC on this setup (`enp6s18`, `enp6s19`), which is typical for VMs where the hypervisor assigns one vNIC per VLAN/port group. If you have a single NIC with a trunk port instead, create VLAN sub-interfaces and use those as the `parent`:
> ```bash
> ip link add link enp6s18 name enp6s18.110 type vlan id 110
> ip link add link enp6s18 name enp6s18.130 type vlan id 130
> ```
> Then use `enp6s18.110` and `enp6s18.130` as the `-o parent` values in the `docker network create` commands below.
| `caddy_proxy` | bridge | Links Caddy to publicly exposed services |
| `home_automation` | bridge | Internal bus between Home Assistant, Mosquitto, and Zigbee2MQTT |
| `monitoring` | bridge | Links Prometheus to services that expose metrics |
| `uptime_kuma` | bridge | Allows Uptime Kuma to reach service health endpoints |
| `cloudflare_tunnel` | bridge | Links Cloudflared tunnel to exposed services |

### Access Patterns

**Tailscale (private)** — most services run a `tailscale` sidecar container and an `nginx` reverse proxy. Traffic flows:
```
Tailscale → nginx (8080 HTTP redirect → 8081 HTTPS) → application
```

**Caddy (public)** — select services join `caddy_proxy` and are routed via the Caddy reverse proxy over HTTPS with automatic TLS.

**Some services use both** (e.g. Home Assistant, Grafana) — accessible privately over Tailscale and publicly via Caddy.

### DNS

AdGuard Home runs on a static IP on the services VLAN (`192.168.130.53`) and serves as the network-wide DNS resolver. UniFi firewall rules redirect all DNS traffic from every VLAN through it.

### UniFi Firewall Rules

By default, VLANs are isolated from each other. The following inter-VLAN rules are required for things to work correctly.

**DNS — allow all VLANs to reach AdGuard Home**

AdGuard Home runs at a static IP on the services VLAN. For every other VLAN (IoT, devices, etc.) to use it as their DNS resolver, create an accept rule on the gateway:

```
Source:      any VLAN (or specific VLAN subnets)
Destination: <adguard-static-ip>, port 53 (TCP + UDP)
Action:      Accept
```

Then set your UniFi DHCP network DNS server to the AdGuard static IP for each VLAN. Optionally, add a DNS intercept rule to force all port 53 traffic through AdGuard regardless of what DNS clients have configured.

**Media and services — allow restricted VLANs to reach specific services**

Devices on isolated VLANs (e.g., smart TVs or streaming sticks on the IoT VLAN) that need to reach services like Emby require explicit rules:

```
Source:      IoT VLAN (192.168.110.0/24)
Destination: server IP or services VLAN, port 8096 + 8920 (Emby HTTP/HTTPS)
Action:      Accept
```

Create one rule per service/port you want to expose. Keep rules as specific as possible — avoid blanket inter-VLAN accept rules as they defeat the purpose of VLAN isolation.

**General principle:** the default posture should be deny inter-VLAN, with explicit accept rules only for traffic that has a clear reason to cross VLAN boundaries (DNS, specific media ports, home automation callbacks, etc.).

---

### No VLANs? Single flat network

If you have a basic ISP router with a single flat network, skip the two ipvlan networks entirely. The rest of the stacks still work as-is with these adjustments:

**AdGuard Home** — instead of a static VLAN IP, publish port 53 directly on the host and point your router's DNS setting to the server's IP:
```yaml
ports:
  - "53:53/udp"
  - "53:53/tcp"
```
Then set your router's DHCP DNS server to your host's IP address.

**IoT device access** (Home Assistant, Frigate, OTBR) — without VLAN isolation, all devices are on the same network as the server. These containers can reach IoT devices using their IP addresses directly. Remove `iot_vlan110` from their `networks:` block and ensure the container can route to your LAN (the default bridge network is sufficient, or use `network_mode: host` for containers that need broad device discovery like mDNS).

**Skip these two `docker network create` commands:**
```bash
# Not needed on a flat network
# docker network create -d ipvlan ... iot_vlan110
# docker network create -d ipvlan ... services_vlan130
```

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

> Adjust `--subnet`, `--gateway`, and `-o parent` to match your own network interfaces and VLAN layout.

## Secrets

All secrets are managed via Komodo and injected at deploy time using `[[SECRET_NAME]]` syntax in `komodo.toml`. No credentials are stored in this repository.
