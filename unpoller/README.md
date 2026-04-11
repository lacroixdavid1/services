# unpoller

Exports UniFi controller metrics to Prometheus.

- **Website:** https://unpoller.com
- **Docs:** https://unpoller.com/docs
- **Access:** None (internal Prometheus exporter)

## Networks

| Network | Purpose |
|---|---|
| `monitoring` | Scraped by Prometheus on port 9130 |

## Notes

- Connects to the UniFi controller using `UNIFI_USER`, `UNIFI_PASS`, and `UNIFI_URL` (all injected via Komodo)
- InfluxDB output is disabled — Prometheus only
- DPI (deep packet inspection) stats are enabled
