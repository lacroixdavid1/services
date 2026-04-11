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

## Notes

- Kobo sync is available at `/kobo/*` — configure your Kobo device to use this endpoint
- Book library mounted read-only from the media storage
