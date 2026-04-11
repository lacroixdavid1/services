# arr

Media management stack — indexing, movies, series, and subtitles.

| Service | Website | Purpose |
|---|---|---|
| Prowlarr | https://prowlarr.com | Indexer manager |
| Radarr | https://radarr.video | Movie collection manager |
| Sonarr | https://sonarr.tv | TV series collection manager |
| Bazarr | https://www.bazarr.media | Subtitle manager |

- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `media` | Shared access to media storage with other media stacks |
| `uptime_kuma` | Health monitoring |

## Notes

- Authentication is set to External — handled upstream by the Tailscale/nginx layer
- Media files are stored on the HDD (`/mnt/data/`)
