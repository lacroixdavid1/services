# qbittorrent

BitTorrent client with a web UI.

- **Website:** https://www.qbittorrent.org
- **Docs:** https://github.com/qbittorrent/qBittorrent/wiki
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `media` | Shared access to media storage with arr stack |
| `uptime_kuma` | Health monitoring |

## Notes

- Download categories are mounted separately for organisation (`movies`, `series`, `music`, etc.)
- Integrates with Radarr/Sonarr via the `media` network
