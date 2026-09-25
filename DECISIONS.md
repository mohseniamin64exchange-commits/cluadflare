# Decision Log

## D-001 - DNS authority moved to Cloudflare
Decision: Cloudflare becomes authoritative DNS.

Reason: centralized DNS management and support for the tunnel/access architecture.

This is not a hosting migration.

## D-002 - Hosting remains at MashhadHost
Decision: website files, database, cPanel and mail hosting remain on the original hosting service.

Reason: the project goal is remote access to the accounting app, not replacement of the website hosting provider.

## D-003 - Use Cloudflare Tunnel instead of router port forwarding
Decision: do not expose port 5000 directly on the router.

Reason:
- no inbound port forwarding
- no dependency on a fixed public IP
- easier future integration with Cloudflare Access
- the local application does not need to be directly reachable from the public internet

## D-004 - Run cloudflared as a Windows service
Reason: automatic startup after reboot and no need for a user to manually start a terminal session.

## D-005 - Use loopback instead of a LAN IP
Decision: use `127.0.0.1` for the local service.

Reason: DHCP may change the computer's LAN IP. Loopback always points to the same machine.

Constraint: this remains valid only while cloudflared and the accounting web application run on the same Windows computer.

## D-006 - Final public subdomain is account
Final hostname:
`account.diacoelectronix.ir`

The earlier draft name was `accounting`.

## D-007 - Defer Cloudflare Access during the test phase
Reason: the application is still experimental and not yet carrying normal production users or real operational data.

Trigger for change: before employees, representatives, or real financial data are introduced.

## D-008 - Future identity plan
Preferred: Google Sign-In.
Fallback: email one-time-password.
Application login remains separate and should not be removed.

## D-009 - Do not store secrets in GitHub
Do not store passwords, tunnel enrollment strings, API keys, private keys, cookies, or recovery material in this repository.
