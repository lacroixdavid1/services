# cloudflare-ddns

Keeps Cloudflare DNS records up to date with the current public IP address.

- **Website:** https://github.com/favonia/cloudflare-ddns
- **Access:** None (background service)

## Notes

- Updates both the root domain and wildcard (`*.domain`) records
- Runs as a read-only, capability-dropped container for minimal attack surface
- Requires `CF_DDNS_TOKEN` (Cloudflare API token with DNS edit permissions) and `PUBLIC_DOMAIN`
