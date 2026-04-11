# jdownloader

Automated download manager supporting a wide range of file hosting sites.

- **Website:** https://jdownloader.org
- **Docs:** https://support.jdownloader.org
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
| `DATA_PATH` | Path to HDD storage for downloads |

## Notes

- Downloads are saved to HDD storage (`${DATA_PATH}/medias`)
- Can be controlled remotely via the MyJDownloader cloud service
