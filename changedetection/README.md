# changedetection

Web page change monitoring with browser automation support.

- **Website:** https://changedetection.io
- **Docs:** https://github.com/dgtlmoon/changedetection.io/wiki
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

- Uses a headless Chrome sidecar (`sockpuppetbrowser`) for JavaScript-heavy pages
- Browser is accessible via WebSocket at `ws://browser-sockpuppet-chrome:3000`
