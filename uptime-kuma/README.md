# uptime-kuma

Service uptime monitoring with alerting and status pages.

- **Website:** https://uptime.kuma.pet
- **Docs:** https://github.com/louislam/uptime-kuma/wiki
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Reaches the `nginx` container of every monitored service |

## Notes

- Every user-facing service's `nginx` container joins the `uptime_kuma` external network so this stack can reach their health endpoints
- Monitors are configured via the web UI — not stored in this repo
