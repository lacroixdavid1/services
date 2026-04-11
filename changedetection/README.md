# changedetection

Web page change monitoring with browser automation support.

- **Website:** https://changedetection.io
- **Docs:** https://github.com/dgtlmoon/changedetection.io/wiki
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |

## Notes

- Uses a headless Chrome sidecar (`sockpuppetbrowser`) for JavaScript-heavy pages
- Browser is accessible via WebSocket at `ws://browser-sockpuppet-chrome:3000`
