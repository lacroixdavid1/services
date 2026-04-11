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

## Notes

- Metrics retention: 3 years with WAL compression enabled
- Prometheus scrapes Home Assistant metrics via a long-lived API token stored in `/mnt/data-nvme/services/monitoring/prometheus/ha_token` (not in the repo)
- Alert rules are defined in `prometheus/rules/`
- Grafana provisioning (datasources) is in `grafana/provisioning/`
- Uses `TS_AUTHKEY_OPENCLAW` to communicate with the `openclaw` VM
