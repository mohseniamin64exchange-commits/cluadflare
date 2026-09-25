# Windows Reinstall Recovery

## What survives a Windows reinstall
These remain outside the local Windows installation:
- domain registration
- Cloudflare nameservers
- Cloudflare DNS zone and records
- tunnel object `diaco-accounting`
- published hostname `account.diacoelectronix.ir`
- existing website hosting at MashhadHost

## What must be rebuilt on the Windows computer
- accounting web application and its runtime/dependencies
- cloudflared installation
- cloudflared Windows service
- a new connector instance attached to the existing tunnel
- any required local firewall exceptions

## Recovery order

### 1. Restore the accounting application
Before touching Cloudflare, make sure the application opens locally on the new Windows installation.

Expected local endpoint:
`http://127.0.0.1:5000`

If this local URL does not work, fix the application first.

### 2. Install cloudflared
Install the current Windows 64-bit cloudflared package.

Verify:
`cloudflared.exe --version`

### 3. Reconnect the new Windows installation to the existing tunnel
Open the existing tunnel:
`diaco-accounting`

Generate a fresh Windows connector/service-install command from the Cloudflare dashboard and run it in an Administrator Command Prompt.

Do not create a second tunnel unless there is a deliberate architecture change.

### 4. Verify service
`sc query cloudflared`

Expected:
`STATE : 4 RUNNING`

### 5. Verify connector
Use Cloudflare dashboard status and, if needed, local cloudflared diagnostics.

### 6. Do not recreate the public route if it still exists
The existing route should still be:
`account.diacoelectronix.ir -> http://127.0.0.1:5000`

### 7. External test
From outside the LAN, open:
`https://account.diacoelectronix.ir`

### 8. Restart test
Restart Windows once and confirm the connector and application return without manual intervention.

## Important benefit of loopback
The Windows computer may receive a different private LAN address after reinstall or reboot. The published route does not need to change while both the connector and application remain on the same computer.
