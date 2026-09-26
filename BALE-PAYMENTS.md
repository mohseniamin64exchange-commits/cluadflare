# Bale Payment Integration

## Purpose

This document records the Bale (بله) payment option for the Diaco accounting application so the integration can be implemented later without relying on chat history.

## Confirmed technical capability

Bale provides an official bot/payment API that can be used by an application to issue and track payments through Bale.

Relevant API concepts/endpoints identified in the official Bale developer documentation include:

- Bot Token for the Bale bot
- `provider_token` for payment capability
- `sendInvoice` for sending a payment invoice
- `createInvoiceLink` for creating an invoice/payment link
- `pre_checkout_query` before final payment confirmation
- `successful_payment` after a successful payment
- `inquireTransaction` for transaction-status inquiry

The payment flow supports carrying an internal invoice/order identifier in the payment payload so the accounting application can associate the returned payment with the correct customer and invoice.

## Proposed accounting flow

`Accounting invoice -> Pay with Bale -> Bale invoice/payment link -> Customer payment -> Bale API verification -> Accounting event -> Authorized confirmation/registration`

Recommended internal fields:

- internal invoice ID
- customer ID
- expected amount
- Bale payment payload
- transaction/reference ID
- payment status
- creation time
- verification time
- registration/approval status

## Important architecture rule

Receiving a successful payment response must not bypass the accounting application's existing authorization rules.

For the current project architecture, external integrations such as Bale/Telegram should create a verified payment event or proposed receipt. They should not receive unrestricted direct database access.

Where manual approval is still required by the accounting workflow, the verified Bale payment should appear in the accounting application's Events section and be registered only after the authorized user confirms it.

If the project later explicitly changes to automatic posting, that must be a separate documented security/accounting decision.

## Transaction verification

Do not treat a client-side redirect, screenshot, customer message, or payment receipt image as proof of settlement.

The server should verify payment through the Bale API and store the provider's transaction/reference identifier. Transaction inquiry should be used when the payment state is uncertain or when reconciliation is required.

Possible statuses previously identified from the payment API include:

- `paid`
- `pending`
- `failed`
- `rejected`

These exact values and API response fields must be rechecked against the current Bale documentation at implementation time.

## Security requirements

- Never commit Bot Tokens, `provider_token`, API credentials, access tokens, cookies, or private keys to GitHub.
- Secrets must be stored outside the repository using the application's secure configuration/secrets mechanism.
- Verify amount, invoice ID, customer association, payment state, and transaction/reference ID on the server.
- Make payment callbacks idempotent so one transaction cannot be posted twice.
- Keep an audit log for payment creation, verification, rejection, refund/reversal, manual approval, and accounting registration.
- Do not trust user-supplied payment status.
- Use HTTPS for all external API communication.

## Wallet / operational notes

Bale documentation describes wallet/payment capabilities and different wallet contexts such as personal, business, or organizational usage, together with operations related to transaction inquiry and payment handling.

Before production use, the following must be confirmed from current official Bale rules/support:

1. Requirements for obtaining a real `provider_token`
2. Business-wallet onboarding and identity verification requirements
3. Transaction limits
4. Withdrawal/settlement limits and timing
5. Fees/commissions
6. Refund/reversal rules
7. Any merchant, tax, licensing, or regulatory requirements

## Legal/compliance warning

Do **not** document or advertise Bale payment as "no Enamad", "no tax file", or "no merchant/legal requirements" unless that statement is confirmed by current official Bale rules and applicable Iranian regulations.

Third-party plugin or seller claims are not sufficient evidence for this project.

## Testing

Bale provides a testing/development payment path described in its developer documentation. Use the test flow before any real-money integration.

Testing must cover:

- successful payment
- failed payment
- pending/unknown payment
- repeated callback
- wrong amount
- wrong invoice/customer
- expired invoice
- transaction re-inquiry
- duplicate-posting prevention
- application restart during payment

## Primary documentation

Official Bale developer documentation:

https://docs.bale.ai/

Because payment rules and API fields can change, revalidate the implementation against the current official documentation before production deployment.

## Project status

Status: documented integration candidate; not yet implemented in the accounting application.
