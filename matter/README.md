# matter

Matter protocol controller for smart home devices over Thread and Wi-Fi.

- **Website:** https://www.home-assistant.io/integrations/matter
- **Docs:** https://github.com/home-assistant-libs/python-matter-server
- **Access:** None (internal service consumed by Home Assistant)

## Networks

| Network | Purpose |
|---|---|
| `home_automation` | Exposes Matter server to Home Assistant |
| `iot_vlan110` | Direct access to Matter/Thread devices on the IoT VLAN |

## Notes

- Connects to the SMLIGHT SLZB-MR4U Thread coordinator via `RCP_HOST` / `RCP_PORT=6638`
- Backbone interface is `eth110` (IoT VLAN)
- Consumed by Home Assistant via the Matter integration
