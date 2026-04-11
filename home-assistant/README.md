# home-assistant

Home automation hub integrating smart devices, automations, and dashboards.

- **Website:** https://www.home-assistant.io
- **Docs:** https://www.home-assistant.io/docs
- **Access:** Tailscale + Cloudflare Tunnel (public)

## Networks

| Network | Purpose |
|---|---|
| `home_automation` | Communication with Mosquitto (MQTT) and Zigbee2MQTT |
| `iot_vlan110` | Direct access to IoT devices on the IoT VLAN |
| `cloudflare_tunnel` | Public remote access via Cloudflare Tunnel |
| `monitoring` | Exposes Prometheus metrics |
| `uptime_kuma` | Health monitoring |

## Notes

- Uses PostgreSQL 16 as the recorder database (instead of SQLite) for better performance
- Connects to the IoT VLAN directly for device polling and mDNS discovery
- Uses `TS_AUTHKEY_OPENCLAW` to enable communication with the `openclaw` VM (e.g. Ollama)
