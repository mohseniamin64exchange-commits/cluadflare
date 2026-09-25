# Security Next Steps

The current configuration is a development/test configuration.

Before real financial data, normal employees, or representatives use the system, add an identity layer in front of the application.

## Planned Cloudflare Access model
Initial users:
- owner
- trusted reviewer/consultant

Later:
- employees
- representatives

Preferred identity method:
Google Sign-In.

Fallback:
email one-time-password.

## Keep application authentication
Cloudflare Access is an outer gate. The accounting application's own login, roles and permissions remain required.

## Production checklist
- enable Cloudflare Access
- define explicit allowed identities/groups
- choose and document session lifetime
- verify application lockout and password-reset rules
- enable audit logging
- add rate limiting to sensitive endpoints
- test backup and restore
- test Windows restart recovery
- test full Windows reinstall recovery
- document offboarding/revocation procedure for staff and representatives
- keep secrets outside GitHub

## Additional principle
Knowing the hostname must never be treated as a security control.
