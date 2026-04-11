# ollama

Local LLM inference engine with a web UI.

| Service | Website | Purpose |
|---|---|---|
| Ollama | https://ollama.com | LLM inference backend |
| Open WebUI | https://openwebui.com | Chat interface |

- **Access:** Tailscale
- **Tailscale tag:** `tag:openclaw` — required for inter-VM communication with the `openclaw` VM

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |

## Notes

- AMD GPU passed through for hardware-accelerated inference via ROCm
- `HSA_OVERRIDE_GFX_VERSION=10.3.0` is required for the RX 6700 XT (Navi 22)
- Models are stored on HDD (`/mnt/data/ollama/models`)
- Uses `TS_AUTHKEY_OPENCLAW` so other services (e.g. Home Assistant) can reach it over Tailscale
- WebUI authentication is disabled — access is gated by Tailscale
