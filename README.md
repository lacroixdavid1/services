# services

Homelab Docker Compose stacks deployed on a self-hosted server via [Komodo](https://komo.do). Each subdirectory is an independent stack. Komodo polls this repo and auto-deploys on push.

For network architecture, VLAN setup, firewall rules, and access patterns see [docs/networking.md](docs/networking.md).
For storage layout and volume mount conventions see [docs/persistence.md](docs/persistence.md).

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
