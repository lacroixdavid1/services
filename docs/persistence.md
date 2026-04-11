# Persistence

Two volume mount roots are used across all stacks, injected as environment variables via Komodo:

| Variable | Purpose |
|---|---|
| `DATA_NVME_PATH` | Config, state, databases — path to fast storage |
| `DATA_PATH` | Large data — media, downloads, archives |

Each service gets its own subdirectory under `DATA_NVME_PATH`. For example:

```
${DATA_NVME_PATH}/home-assistant/app/
${DATA_NVME_PATH}/home-assistant/postgres/
${DATA_NVME_PATH}/home-assistant/tailscale/
```

Large media volumes (Emby, Frigate recordings, qBittorrent downloads, Ollama models) are mounted from `DATA_PATH` instead.

## Setup

Set both variables as global variables in Komodo (Settings → Variables) to match your storage layout. If you only have one disk, both can point to the same path.
