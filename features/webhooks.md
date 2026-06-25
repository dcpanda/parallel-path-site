---
title: Webhooks
description: Resilient external notifications with HMAC-SHA256 signing and outbox pattern delivery.
---

<div class="content-page">

<h1>Webhooks</h1>
<p class="page-desc">Parallel Path provides a robust webhook system for notifying external systems of workflow events. Webhooks use HMAC-SHA256 signed payloads, outbox pattern delivery, and exponential backoff retry.</p>

## Architecture

Webhooks follow the reliable **outbox pattern**:
1. A state transition occurs — the webhook event is written to the **outbox table** in the same database transaction
2. A background processor picks up pending outbox events
3. The webhook payload is delivered to the configured URL
4. Delivery status is tracked in `WebhookDeliveryEntity`

This ensures that webhook delivery is **atomic with the state change** — if the transition succeeds, the webhook is guaranteed to be queued.

## Delivery States

| State | Description |
|-------|-------------|
| `PENDING` | Queued for delivery |
| `DELIVERING` | Currently being sent |
| `DELIVERED` | Successfully delivered (HTTP 2xx) |
| `FAILED` | Delivery failed (will be retried) |

## HMAC-SHA256 Signing

Every webhook payload is signed with HMAC-SHA256:
- The signing key is configured per webhook endpoint
- The signature is sent in the `X-Signature-256` header
- Receivers verify the signature to confirm payload authenticity
- Prevents tampering and replay attacks

## Retry Logic

Failed deliveries use exponential backoff:
- **Initial retry** — After 1 second
- **Backoff multiplier** — Each retry doubles the interval
- **Max retries** — Configurable via `parallelpath.webhooks.max-retries`
- **Dead letter** — After max retries, the event is moved to the dead letter queue

## Webhook Guard Integration

The `HttpWebhookGuard` uses the webhook system for guard validation:
- Before a transition proceeds, the guard calls an external system
- The external system returns a pass/fail decision
- The guard evaluation result is recorded and audited
- Supports custom response parsing for integration flexibility

## Configuration

```yaml
parallelpath:
  webhooks:
    enabled: true
    max-retries: 5
    retry-base-delay-ms: 1000
    signing-key: ${WEBHOOK_SIGNING_KEY}
```

</div>
