# Persistence

Two volume mount roots are used across all stacks:

| Path | Purpose |
|---|---|
| `/mnt/data-nvme/services/<service>/` | Config, state, databases |
| `/mnt/data/` | Large data — media, downloads, archives |

Each service gets its own subdirectory under `/mnt/data-nvme/services/`. For example:

```
/mnt/data-nvme/services/home-assistant/app/
/mnt/data-nvme/services/home-assistant/postgres/
/mnt/data-nvme/services/home-assistant/tailscale/
```

Replace these paths with whatever suits your own setup. If you only have one disk, both can point to the same mount — the split is a convention, not a requirement.
