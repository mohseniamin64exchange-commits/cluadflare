# Architecture

The project keeps the existing hosting service at MashhadHost while Cloudflare manages DNS and provides the secure tunnel layer for the accounting web application.

Final traffic flow:

`Internet user -> Cloudflare -> Cloudflare Tunnel -> Windows host -> local accounting web app`

The accounting application and the tunnel connector run on the same Windows machine. For that reason the local service is addressed through the loopback interface rather than the LAN address. This prevents normal DHCP address changes from breaking the route.

The website hosting, cPanel, mail service, and existing website files remain on the original hosting service. Moving DNS authority to Cloudflare did not migrate or delete the hosting account.