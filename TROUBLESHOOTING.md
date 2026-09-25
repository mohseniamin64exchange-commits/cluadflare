# Troubleshooting

## Cloudflare zone stays Pending
Check that the domain uses only the two assigned Cloudflare nameservers. Wait for propagation and refresh the Cloudflare overview.

## Tunnels not visible inside the domain Network page
The tunnel controls are at the account-level Networking/Tunnels area, not the individual domain's Network settings.

## cloudflared seems to have no desktop application
Expected. cloudflared is a command-line/background connector and Windows service, not a normal GUI application.

## Dashboard says the tunnel is waiting even though the service is running
Check:
`sc query cloudflared`

Then:
`cloudflared tunnel diag`

Then, if available:
`curl http://localhost:20241/diag/tunnel`

During the original setup the local diagnostics showed the connector was connected even while the dashboard briefly lagged behind.

## TLS test showed SEC_E_UNTRUSTED_ROOT
A Windows curl test against a Cloudflare tunnel endpoint returned a Schannel trust error. This alone did not prove the tunnel was down. The cloudflared diagnostics were more useful and confirmed the connector was actually connected.

## Failed to add published application
The working fix was to remove the trailing slash from the local service URL.

Problematic:
`http://127.0.0.1:5000/`

Working:
`http://127.0.0.1:5000`

## Site not found
One test accidentally used the .com version of the domain.

Wrong:
`account.diacoelectronix.com`

Correct:
`account.diacoelectronix.ir`

## LAN IP changed after reboot
No route change is needed as long as the application and cloudflared remain on the same computer and the route uses loopback.

## Quick outage checklist
1. Does the accounting app open locally?
2. Does `http://127.0.0.1:5000` work on the Windows host?
3. Is the cloudflared service RUNNING?
4. Does the connector show connected in local diagnostics or Cloudflare?
5. Does the published route still point to `127.0.0.1:5000`?
6. Does the DNS entry for `account` still exist?
