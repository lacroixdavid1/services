# namecheap-ddns

Keeps Namecheap DNS records up to date with the current public IP address.

- **Website:** https://github.com/linuxshots/namecheap-ddns
- **Access:** None (background service)

## Notes

- Runs two containers: one for the wildcard subdomain (`*`) and one for the root domain (`@`)
- Requires `NC_PASS` (Namecheap DDNS password) and `PUBLIC_DOMAIN`
