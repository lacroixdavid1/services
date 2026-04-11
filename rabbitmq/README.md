# rabbitmq

AMQP message broker.

- **Website:** https://www.rabbitmq.com
- **Docs:** https://www.rabbitmq.com/docs
- **Access:** Tailscale (direct TCP — no HTTP interface)

## Notes

- Runs in `network_mode: service:tailscale` — the AMQP port is exposed directly over Tailscale
- Management UI is also accessible over Tailscale on port 15672
