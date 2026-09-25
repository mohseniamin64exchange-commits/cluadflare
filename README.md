# README

This repository is the durable handoff record for the Diaco Cloudflare/accounting-access project.

## Purpose

The repository is designed so that a new ChatGPT conversation, a technician, or the project owner can reconstruct the project without needing the original chat history.

## Current production-like test state

- Domain: `diacoelectronix.ir`
- Domain and hosting provider: MashhadHost
- Existing web hosting remains on MashhadHost
- Hosting origin IP: `46.245.95.163`
- Authoritative DNS is Cloudflare
- Cloudflare nameservers: `andy.ns.cloudflare.com`, `love.ns.cloudflare.com`
- Cloudflare Tunnel name: `diaco-accounting`
- Public application hostname: `account.diacoelectronix.ir`
- Local application endpoint: `http://127.0.0.1:5000`
- The accounting application and cloudflared connector run on the same Windows computer
- cloudflared is installed as a Windows service
- External access has been tested successfully and the accounting login page opens from outside the LAN
- Cloudflare Access is intentionally deferred until the application is ready for real users and real operational data

## Most important architectural fact

Moving DNS authority to Cloudflare did NOT move or delete the hosting service. Website files, database, cPanel, mail hosting, and the existing hosting account remain with MashhadHost. Cloudflare is the DNS/proxy/tunnel layer.

## Start here after a Windows reinstall

Read `WINDOWS-REINSTALL.md`, then `RUNBOOK.md`.

## Start here if recreating the entire project from zero

Read `ROADMAP.md`, `DNS-MIGRATION.md`, `TUNNEL-SETUP.md`, then `RUNBOOK.md`.

## Security rule

Do not commit passwords, tunnel enrollment strings, API keys, private keys, session cookies, or account recovery material to this repository.