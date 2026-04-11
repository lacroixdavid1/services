# Network Architecture

Two isolated VLANs are used, managed by a gateway/router with inter-VLAN routing. Docker containers join these networks directly via ipvlan L2, allowing them to appear as first-class devices on each VLAN.

## VLANs

| VLAN | Purpose |
|---|---|
| IoT | IoT devices, coordinators, sensors |
| Services | Server infrastructure and self-hosted services |

## Docker Networks

| Network | Type | Purpose |
|---|---|---|
| `iot_vlan110` | ipvlan L2 | Direct access to IoT VLAN — used by Home Assistant, OTBR, Frigate |
| `services_vlan130` | ipvlan L2 | Stable IP on services VLAN — used by AdGuard Home for network-wide DNS |
| `caddy_proxy` | bridge | Links Caddy to publicly exposed services |
| `home_automation` | bridge | Internal bus between Home Assistant, Mosquitto, and Zigbee2MQTT |
| `monitoring` | bridge | Links Prometheus to services that expose metrics |
| `uptime_kuma` | bridge | Allows Uptime Kuma to reach service health endpoints |
| `cloudflare_tunnel` | bridge | Links Cloudflared tunnel to exposed services |

> The ipvlan networks require a dedicated interface (or VLAN sub-interface) per VLAN. If your NIC uses a trunk port, create sub-interfaces first:
> ```bash
> ip link add link eth0 name eth0.110 type vlan id 110
> ip link add link eth0 name eth0.130 type vlan id 130
> ```
> Then use those as the `-o parent` values in the `docker network create` commands in the README.

## Access Patterns

**Tailscale (private)** — most services run a `tailscale` sidecar container and an `nginx` reverse proxy. Traffic flows:
```
Tailscale → nginx (8080 HTTP redirect → 8081 HTTPS) → application
```

**Caddy (public)** — select services join `caddy_proxy` and are routed via the Caddy reverse proxy over HTTPS with automatic TLS.

**Some services use both** (e.g. Emby, Calibre-web) — accessible privately over Tailscale and publicly via Caddy.

## DNS

AdGuard Home runs on a static IP on the services VLAN and serves as the network-wide DNS resolver. Firewall rules on the gateway redirect all DNS traffic from every VLAN through it.

## Firewall Rules

By default, VLANs are isolated from each other. The following inter-VLAN rules are required.

**DNS — allow all VLANs to reach AdGuard Home**

```
Source:      any VLAN
Destination: <adguard-static-ip>, port 53 (TCP + UDP)
Action:      Accept
```

Set your DHCP DNS server to the AdGuard static IP for each VLAN. Optionally add a DNS intercept rule to force all port 53 traffic through AdGuard regardless of what clients have configured.

**Media — allow restricted VLANs to reach specific services**

Devices on isolated VLANs (e.g. smart TVs or streaming sticks on the IoT VLAN) that need to reach services like Emby require explicit rules:

```
Source:      IoT VLAN subnet
Destination: server IP, port 8096 + 8920 (Emby HTTP/HTTPS)
Action:      Accept
```

Create one rule per service/port. Avoid blanket inter-VLAN accept rules — they defeat the purpose of VLAN isolation.

## No VLANs? Single Flat Network

If you have a basic ISP router with a single flat network, skip the two ipvlan networks entirely. The rest of the stacks work as-is with these adjustments:

**AdGuard Home** — publish port 53 directly on the host and point your router's DHCP DNS to the server's IP:
```yaml
ports:
  - "53:53/udp"
  - "53:53/tcp"
```

**IoT device access** (Home Assistant, Frigate, OTBR) — remove `iot_vlan110` from their `networks:` block. All devices are on the same network so containers can reach them directly. Use `network_mode: host` for containers that need mDNS/device discovery.

**Skip these two `docker network create` commands:**
```bash
# Not needed on a flat network
# docker network create -d ipvlan ... iot_vlan110
# docker network create -d ipvlan ... services_vlan130
```
