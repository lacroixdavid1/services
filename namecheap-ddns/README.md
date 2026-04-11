# namecheap-ddns

Keeps Namecheap DNS records up to date with the current public IP address.

- **Website:** https://github.com/linuxshots/namecheap-ddns
- **Access:** None (background service)

## Variables

| Variable | Description |
|---|---|
| `NC_PASS` | Namecheap DDNS password |
| `PUBLIC_DOMAIN` | Public domain name to keep updated |

## Notes

- Runs two containers: one for the wildcard subdomain (`*`) and one for the root domain (`@`)
