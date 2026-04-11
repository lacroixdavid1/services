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

## Notes

- Routes are defined in `config/Caddyfile` using `{env.PUBLIC_DOMAIN}` for the domain
- Static files (sounds, assets) are served from `static/`
- Metrics endpoint available at `:2019/metrics`
