# unpoller

Exports UniFi controller metrics to Prometheus.

- **Website:** https://unpoller.com
- **Docs:** https://unpoller.com/docs
- **Access:** None (internal Prometheus exporter)

## Networks

| Network | Purpose |
|---|---|
| `monitoring` | Scraped by Prometheus on port 9130 |

## Variables

| Variable | Description |
|---|---|
| `UNIFI_USER` | UniFi controller username |
| `UNIFI_PASS` | UniFi controller password |
| `UNIFI_URL` | UniFi controller URL |

## Notes

- InfluxDB output is disabled — Prometheus only
- DPI (deep packet inspection) stats are enabled
