# otbr

OpenThread Border Router — bridges Thread mesh networks to IP networks for Matter over Thread.

- **Website:** https://openthread.io/guides/border-router
- **Docs:** https://openthread.io/guides/border-router/build
- **Access:** None (internal service consumed by Home Assistant / Matter server)

## Networks

| Network | Purpose |
|---|---|
| `home_automation` | Exposes Thread border router to Home Assistant and Matter server |
| `iot_vlan110` | Direct access to the Thread coordinator on the IoT VLAN |

## Notes

- Uses the `bnutzer/otbr-tcp` image — required for the SMLIGHT SLZB-MR4U coordinator which uses a TCP socat bridge (`RCP_HOST` / `RCP_PORT=6638`)
- `spinel+hdlc+tcp://` does not work with this device; the socat bridge approach is required
- Requires `NET_ADMIN`, `NET_RAW`, and `SYS_ADMIN` capabilities
- Backbone interface is `eth110` (IoT VLAN)
