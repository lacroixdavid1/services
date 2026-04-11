# trilium

Hierarchical personal knowledge base and note-taking application.

- **Website:** https://github.com/TriliumNext/Notes
- **Docs:** https://triliumnext.github.io/Docs
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |

## Notes

- Trusted reverse proxy range set to `100.64.0.0/10` (Tailscale CGNAT range) for correct IP forwarding
