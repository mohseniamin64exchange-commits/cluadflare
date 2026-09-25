# Roadmap

## Phase 1 - Initial objective
Provide internet access to a locally hosted accounting web application without exposing the local application port directly on the router.

## Phase 2 - Preserve existing hosting
Keep the domain's existing website hosting, database, cPanel and mail service at MashhadHost.

## Phase 3 - Move authoritative DNS to Cloudflare
Before changing nameservers, review and reproduce the important DNS records in Cloudflare.

Old nameservers:
- `irdns1.mrservers.net`
- `irdns2.mrservers.net`

Cloudflare nameservers:
- `andy.ns.cloudflare.com`
- `love.ns.cloudflare.com`

After propagation, verify the Cloudflare zone is Active.

## Phase 4 - Create the accounting tunnel
Create a Cloudflare Tunnel named `diaco-accounting`.

Install cloudflared on the Windows computer that runs the accounting web application and register it as a Windows service.

## Phase 5 - Avoid dependence on DHCP
The Windows computer may receive a different LAN address after reboot. Because cloudflared and the accounting application run on the same computer, use the loopback endpoint rather than a private LAN address.

Final local endpoint:
`http://127.0.0.1:5000`

## Phase 6 - Publish the application
Public hostname:
`account.diacoelectronix.ir`

Local service:
`http://127.0.0.1:5000`

Important: when the route was created, a trailing slash after port 5000 caused the dashboard to reject the route. The working value does not include the trailing slash.

## Phase 7 - Validate external access
Test the public hostname from outside the local network. The accounting application's login page was successfully reached.

## Phase 8 - Future production hardening
Before real financial data, employees or representatives use the system:
- enable Cloudflare Access
- define allowed identities
- keep application-level authentication
- review session lifetime
- enable and review audit logs
- add rate limiting for sensitive endpoints
- verify backup and restore
- test recovery after Windows restart and Windows reinstall
