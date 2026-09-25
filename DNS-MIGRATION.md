# DNS Migration Notes

## Original state
The domain and hosting service were purchased from MashhadHost.

Original nameservers:
- `irdns1.mrservers.net`
- `irdns2.mrservers.net`

The web hosting server IP was `46.245.95.163`.

## Preparation before nameserver change
Important DNS records from the hosting/cPanel zone were reviewed and recreated in Cloudflare before changing nameservers.

Key record categories included:
- apex web record
- www
- mail and MX
- SPF
- DKIM
- DMARC
- cPanel/WHM/webmail/webdisk
- FTP
- autodiscover
- CalDAV/CardDAV records

Mail-related and other direct hosting service records were kept as DNS Only where appropriate.

## Nameserver change
Cloudflare-assigned nameservers:
- `andy.ns.cloudflare.com`
- `love.ns.cloudflare.com`

After changing the nameservers at the registrar/provider side, Cloudflare initially showed the zone as pending. After propagation completed, the zone became Active.

## Important interpretation
Changing nameservers changes which DNS provider is authoritative. It does not move the website, database, cPanel, email hosting, or hosting files.

Current conceptual path for the website:
`domain -> Cloudflare DNS/proxy -> existing MashhadHost web server`
