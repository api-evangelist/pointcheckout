---
name: Manage recurring subscriptions with PointCheckout
description: Create a subscription, list its payments, and pause or resume it.
api: openapi/pointcheckout-merchant-api-openapi.yml
operations: [create-subscription, get-subscription, get-subscription-payments, pause-subscription, resume-subscription]
---

# Manage recurring subscriptions with PointCheckout

## Authentication
Send `X-PointCheckout-Api-Key` and `X-PointCheckout-Api-Secret` headers on every request.
Base URL — test: `https://api.test.paymennt.com/mer/v2.0/`.

## Steps
1. Create a subscription with `create-subscription` (`POST /subscription`); the response wrapper returns the subscription `id`.
2. Retrieve current state with `get-subscription` (`GET /subscription/{subscriptionId}`).
3. List billing history with `get-subscription-payments` (`GET /subscription/{subscriptionId}/payment`) — a paged response (`page`, `size`, `totalPages`, `totalElements`, `content`).
4. Temporarily halt billing with `pause-subscription` (`POST /subscription/{subscriptionId}/pause`) and restart it with `resume-subscription` (`POST /subscription/{subscriptionId}/resume`).

## Rules
- Responses use the `{ success, elapsed, error, result }` envelope; `success: false` at HTTP 400 signals an error.
- Paginate with `page` (zero-based) and `size` query parameters.
