# cloudflared

Cloudflare Tunnel — exposes internal services to the public internet without opening firewall ports.

- **Website:** https://developers.cloudflare.com/cloudflare-one/connections/connect-networks
- **Docs:** https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started
- **Access:** None (tunnel agent)

## Networks

| Network | Purpose |
|---|---|
| `cloudflare_tunnel` | Reaches services exposed through the tunnel (e.g. Home Assistant) |

## Notes

- Requires `CF_TUNNEL_TOKEN` from the Cloudflare Zero Trust dashboard
- Services are routed via tunnel rules configured in the Cloudflare dashboard, not locally
