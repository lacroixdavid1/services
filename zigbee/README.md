# zigbee

Zigbee coordinator bridge — exposes Zigbee devices to Home Assistant via MQTT.

- **Website:** https://www.zigbee2mqtt.io
- **Docs:** https://www.zigbee2mqtt.io/guide/getting-started
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `home_automation` | Publishes device state to Mosquitto (MQTT) for Home Assistant |
| `iot_vlan110` | Direct access to the Zigbee coordinator on the IoT VLAN |
| `uptime_kuma` | Health monitoring |

## Notes

- Coordinator is on the IoT VLAN — connected via `iot_vlan110` (`eth110` interface)
- Frontend UI runs on port 8080
- Device config and state are persisted to NVMe storage
