# calibre-web

Web-based ebook library manager with Kobo sync support.

- **Website:** https://github.com/janeczku/calibre-web
- **Docs:** https://github.com/janeczku/calibre-web/wiki
- **Access:** Tailscale + Caddy (public)

## Networks

| Network | Purpose |
|---|---|
| `caddy_proxy` | Public access via Caddy |
| `media` | Shared access to book library |
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |
| `DATA_PATH` | Path to HDD storage for the book library |

## Notes

- Kobo sync is available at `/kobo/*` — configure your Kobo device to use this endpoint
- Book library mounted read-only from the media storage
