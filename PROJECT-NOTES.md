# Project Notes

## Goal
Provide remote access to the locally hosted accounting web application while keeping the existing website hosting service unchanged.

## Hosting and DNS
The domain and hosting service were purchased from MashhadHost. The original nameservers belonged to the hosting provider. DNS management was later moved to Cloudflare. This did not move the website files, database, cPanel, or hosting account.

## Local application
The accounting software is a web application running on a Windows computer. The project uses the same computer for the application and the Cloudflare connector.

## Connection design
The public subdomain is `account.diacoelectronix.ir`. Traffic reaches Cloudflare, then the Cloudflare Tunnel, then the Windows connector, and finally the local web application.

## Local endpoint decision
A changing LAN address was avoided. The local loopback address is used because both the connector and the application run on the same computer. This keeps the route stable even if the machine receives a different private LAN address after a restart.

## Tunnel
The tunnel is named `diaco-accounting`. The connector is installed as a Windows service so it can start automatically after Windows restarts.

## Troubleshooting history
During setup, the dashboard briefly showed the tunnel as waiting even though the local diagnostics reported an active connection. Refreshing the dashboard resolved the display delay. The published application form also failed when the local service URL contained a trailing slash; removing that final slash allowed the route to be created successfully.

## Current result
The public subdomain successfully opens the accounting application's login page from outside the local network.

## Future work
The application is still in a testing phase. Identity-based access control will be added later, before the system is used by staff or representatives and before real operational data is introduced.

## Windows reinstall note
The domain, DNS zone, tunnel object, and published route remain in Cloudflare. After reinstalling Windows, the local application and connector must be installed again and the new connector must be attached to the existing tunnel. The local application should be tested first before checking the external route.