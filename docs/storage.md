# Storage

The server uses a two-tier storage layout:

| Path | Type | Purpose |
|---|---|---|
| `/mnt/data-nvme/services/<service>/` | NVMe SSD | Config, state, databases, small/fast data |
| `/mnt/data/` | HDD (3.6 TB) | Large data — media, downloads, archives |

## Convention

Each service gets its own subdirectory under `/mnt/data-nvme/services/`. For example:

```
/mnt/data-nvme/services/home-assistant/app/
/mnt/data-nvme/services/home-assistant/postgres/
/mnt/data-nvme/services/home-assistant/tailscale/
```

Large media volumes (Emby, Frigate recordings, qBittorrent downloads, Ollama models) are mounted from `/mnt/data/` instead.

## Adapting to Your Setup

Replace `/mnt/data-nvme/` and `/mnt/data/` with whatever paths match your own storage. If you only have one disk, you can point both to the same mount point — the split is a performance convention, not a hard requirement.
