# n8n

Workflow automation platform with 400+ integrations.

- **Website:** https://n8n.io
- **Docs:** https://docs.n8n.io
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |

## Notes

- Webhook URL is set to the Tailscale hostname via `${TS_TAILNET}` so external webhook triggers work correctly
- Workflow data is persisted to NVMe storage
