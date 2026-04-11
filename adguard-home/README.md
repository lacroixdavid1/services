# adguard-home

Network-wide DNS server and ad blocker. Runs on a static IP on the services VLAN so it is reachable from all VLANs via UniFi firewall rules.

- **Website:** https://adguard.com/adguard-home.html
- **Docs:** https://github.com/AdguardTeam/AdGuardHome/wiki
- **Access:** Tailscale

## Networks

| Network | Purpose |
|---|---|
| `services_vlan130` | Static IP (`192.168.130.53`) — reachable as DNS from all VLANs |
| `uptime_kuma` | Health monitoring |

## Variables

| Variable | Description |
|---|---|
| `TS_AUTHKEY` | Tailscale auth key |
| `DATA_NVME_PATH` | Path to NVMe storage for config and state |

## Notes

- DNS traffic from all VLANs is redirected to this instance via UniFi firewall rules
- Configure each VLAN's DHCP DNS server to point to the static IP
