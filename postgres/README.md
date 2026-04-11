# postgres

Standalone PostgreSQL instance for general use.

- **Website:** https://www.postgresql.org
- **Docs:** https://www.postgresql.org/docs
- **Access:** Tailscale (direct TCP — no HTTP interface)

## Notes

- Runs in `network_mode: service:tailscale` — the Postgres port is exposed directly over Tailscale, not through an nginx reverse proxy
- Credentials injected via Komodo secrets (`POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`)
- Data persisted to NVMe storage
