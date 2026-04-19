# caddy

Reverse proxy handling public HTTPS access for select services. Automatic TLS via Let's Encrypt.

- **Website:** https://caddyserver.com
- **Docs:** https://caddyserver.com/docs
- **Access:** Public internet (ports 80/443)

## Networks

| Network | Purpose |
|---|---|
| `caddy_proxy` | Reaches all services exposed publicly |
| `monitoring` | Exposes Caddy metrics to Prometheus (port 2019) |
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `PUBLIC_DOMAIN` | Public domain name (e.g. `example.com`) |
| `DATA_NVME_PATH` | Path to NVMe storage for Caddy state |
| `DATA_PATH` | Path to HDD storage for large shared files |

## Notes

- Routes are defined in `config/Caddyfile` using `{$PUBLIC_DOMAIN}` for the domain
- Static files (sounds, assets) are served from `static/`
- Large shared files are served from `$DATA_PATH/files/` at `files.<PUBLIC_DOMAIN>` — directory listing is disabled; use GUID filenames for security by obscurity
- Metrics endpoint available at `:2019/metrics`
