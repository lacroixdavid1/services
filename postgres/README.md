# postgres

Standalone PostgreSQL instance for general use.

- **Website:** https://www.postgresql.org
- **Docs:** https://www.postgresql.org/docs
- **Access:** Tailscale (direct TCP — no HTTP interface)

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `POSTGRES_USER` | PostgreSQL superuser username |
| `POSTGRES_PASSWORD` | PostgreSQL superuser password |
| `POSTGRES_DB` | Default database name |
| `DATA_NVME_PATH` | Path to NVMe storage for database files |

## Notes

- Runs in `network_mode: service:tailscale` — the Postgres port is exposed directly over Tailscale, not through an nginx reverse proxy
- Data persisted to NVMe storage
