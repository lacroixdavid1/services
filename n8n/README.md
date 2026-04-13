# n8n

Workflow automation platform with 400+ integrations.

- **Website:** https://n8n.io
- **Docs:** https://docs.n8n.io
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `uptime_kuma` | Health monitoring |
| `caddy_proxy` | Public webhook access via Caddy |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `TS_TAILNET` | Tailscale tailnet name (used for the n8n UI hostname) |
| `PUBLIC_DOMAIN` | Public domain for webhook URLs (e.g. `example.com`) |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |

## Notes

- The n8n UI is accessible only via Tailscale (`https://n8n.<tailnet>`)
- Webhooks are exposed publicly at `https://n8n.<PUBLIC_DOMAIN>/webhook/` and `/webhook-test/` via Caddy — all other paths return 404
- `WEBHOOK_URL` is set to the public domain so n8n generates correct webhook URLs for external integrations
- Workflow data is persisted to NVMe storage
