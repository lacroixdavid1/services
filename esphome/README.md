# esphome

Firmware builder and OTA updater for ESP8266/ESP32 microcontrollers.

- **Website:** https://esphome.io
- **Docs:** https://esphome.io/guides
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `iot_vlan110` | Direct access to IoT VLAN for OTA flashing and device discovery |
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |

## Notes

- Needs to be on the IoT VLAN to reach ESP devices for OTA updates and mDNS discovery
- Dashboard runs on port 6052
