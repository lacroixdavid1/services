# n8n

Workflow automation platform with 400+ integrations.

- **Website:** https://n8n.io
- **Docs:** https://docs.n8n.io
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `TS_TAILNET` | Tailscale tailnet name (used for the webhook URL) |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |

## Notes

- Webhook URL is set to the Tailscale hostname via `${TS_TAILNET}` so external webhook triggers work correctly
- Workflow data is persisted to NVMe storage
