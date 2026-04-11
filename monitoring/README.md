# monitoring

Full observability stack — metrics, logs, dashboards, and alerts.

| Service | Website | Purpose |
|---|---|---|
| Prometheus | https://prometheus.io | Metrics collection and storage |
| Grafana | https://grafana.com | Dashboards and visualization |
| Loki | https://grafana.com/oss/loki | Log aggregation |
| Promtail | https://grafana.com/docs/loki/latest/send-data/promtail | Log shipping agent |
| Alertmanager | https://prometheus.io/docs/alerting/latest/alertmanager | Alert routing |
| Node Exporter | https://github.com/prometheus/node_exporter | Host system metrics |
| cAdvisor | https://github.com/google/cadvisor | Container metrics |

- **Access:** Tailscale (Grafana)

## Networks

| Network | Purpose |
|---|---|
| `monitoring` | Scrapes metrics from all stacks that join this network |
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY_OPENCLAW` | Tailscale auth key with `tag:openclaw` for inter-VM communication |
| `TS_TAILNET` | Tailscale tailnet name (used for `GF_SERVER_ROOT_URL`) |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |

## Notes

- Metrics retention: 3 years with WAL compression enabled
- Prometheus scrapes Home Assistant metrics via a long-lived API token stored at `${DATA_NVME_PATH}/monitoring/prometheus/ha_token` (not in the repo)
- Alert rules are defined in `prometheus/rules/`
- Grafana provisioning (datasources) is in `grafana/provisioning/`
- Uses `TS_AUTHKEY_OPENCLAW` to communicate with the `openclaw` VM
