---
name: Accept a payment with PointCheckout
description: Create a hosted web checkout, then track it to completion via status polling or webhooks.
api: openapi/pointcheckout-merchant-api-openapi.yml
operations: [create-web-checkout, get-checkout, refund-checkout, create-webhook]
---

# Accept a payment with PointCheckout

Use the PointCheckout Merchant API (powered by paymennt.com) to collect a payment.

## Authentication
Send both API credential headers on every request:
- `X-PointCheckout-Api-Key`
- `X-PointCheckout-Api-Secret`

Base URL — test: `https://api.test.paymennt.com/mer/v2.0/`, live: `https://api.paymennt.com/mer/v2.0/`.

## Steps
1. Create a hosted checkout with `create-web-checkout` (`POST /checkout/web`). Supply the order totals, currency, customer, and items. The response wrapper (`success`, `elapsed`, `result`) returns the checkout `id` and a redirect/checkout URL.
2. Redirect the shopper to the returned checkout URL to complete payment.
3. Confirm the outcome either by:
   - polling `get-checkout` (`GET /checkout/{checkoutId}`) and reading the `status` (`PAID` / `CANCELLED`), or
   - registering a webhook with `create-webhook` (`POST /webhooks`) and verifying the HMAC signature on delivered messages.
4. To reverse a captured payment, call `refund-checkout` (`POST /checkout/{checkoutId}/refund`).

## Rules
- Treat any response with `success: false` as an error; read the `error` string (HTTP 400).
- There is no idempotency-key contract — avoid blind retries of create operations; reconcile with `search-checkouts` if a create times out.
