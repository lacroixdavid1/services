# emby

Media server for movies, TV shows, and music.

- **Website:** https://emby.media
- **Docs:** https://emby.media/support/articles/Home.html
- **Access:** Tailscale + Caddy (public)

## Networks

| Network | Purpose |
|---|---|
| `caddy_proxy` | Public access via Caddy |
| `media` | Shared access to media storage |
| `uptime_kuma` | Health monitoring |

## Notes

- AMD GPU is passed through (`/dev/dri`, `/dev/kfd`) for hardware transcoding via ROCm
- Media library is mounted from HDD storage (`/mnt/data/`)
