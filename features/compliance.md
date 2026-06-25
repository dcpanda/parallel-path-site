---
title: Compliance
description: SOC 2, HIPAA, and GDPR-ready audit trails with evidence pack generation.
---

<div class="content-page">

<h1>Compliance</h1>
<p class="page-desc">Parallel Path is designed from the ground up for regulated environments. Every state transition is a versioned, inspectable audit record. Generate structured compliance evidence packs for SOC 2, HIPAA, and GDPR frameworks.</p>

## Versioned Audit Trail

Every action becomes a durable, inspectable state change:

- **StateTransitioned** events record before/after snapshots
- **Guard results** include LLM reasoning, vote tallies, and webhook responses
- **Actor identity** captured for every transition (human, agent, system)
- **Idempotency keys** prevent duplicate event processing
- **Dead letter queue** captures failed outbox events for inspection

## Evidence Pack Generation

Generate structured compliance evidence packs via API endpoints. Each pack includes:

- **Workflow definition** — The state flow JSON with all layers
- **Transition history** — Every state change with timestamps and actors
- **Guard evaluation results** — All guard checks with reasoning
- **Task completion records** — Human and agent task artifacts
- **Payload snapshots** — Layer payloads at each transition point
- **PDF summary** — Downloadable compliance report via OpenPDF

## Compliance Framework Mapping

| Framework | Requirement | How Parallel Path Addresses |
|-----------|-------------|---------------------------|
| SOC 2 | Access controls | API key system, rate limiting, audit logging |
| SOC 2 | Change management | Versioned state transitions, guard enforcement |
| HIPAA | Access logging | Per-transaction actor tracking with timestamps |
| HIPAA | Data integrity | Persistence-first architecture, idempotency keys |
| GDPR | Data portability | Evidence packs with complete workflow history |
| GDPR | Right to erasure | Tenant-scoped data with isolation guarantees |

## Automatic Recovery

On startup, the `StartupRecoveryManager` rehydrates state from the database, ensuring no transitions are lost:
- Reads all persisted events
- Rebuilds in-memory state for active instances
- Continues automated polling for temporal transitions

## Dead Letter Queue

When outbox event delivery fails repeatedly:
- The event is moved to a dead letter queue
- Failed events are inspectable with full context
- Manual or automated retry is supported
- Queue depth is monitored via custom Actuator health indicator

</div>
