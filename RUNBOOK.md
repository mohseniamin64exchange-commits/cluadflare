# Operational Runbook

## Normal state
Public URL:
`https://account.diacoelectronix.ir`

Tunnel:
`diaco-accounting`

Local service:
`http://127.0.0.1:5000`

## Basic health check
1. On the Windows host, open `http://127.0.0.1:5000`.
2. Run `sc query cloudflared`.
3. Confirm the tunnel is healthy in Cloudflare.
4. Test the public URL from a network outside the office LAN.

## If local works but public does not
Check cloudflared service state, tunnel connector health, published route, and Cloudflare DNS.

## If local does not work
Do not troubleshoot Cloudflare first. Fix the accounting application or its runtime/service so the local endpoint is available again.

## If Windows has just been reinstalled
Follow `WINDOWS-REINSTALL.md`.

## If recreating from zero
Follow:
1. `DNS-MIGRATION.md`
2. `TUNNEL-SETUP.md`
3. `RUNBOOK.md`

## Before production
Read `SECURITY-NEXT-STEPS.md`.
