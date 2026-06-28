---
title: Parallel Layers
description: Multi-layer state execution enables concurrent, independent workflow paths within a single instance.
---

<div class="content-page" markdown="1">

<h1>Parallel Layers</h1>
<p class="page-desc">Unlike traditional single-dimension state machines, Parallel Path lets you define multiple independent layers (tracks) that run concurrently within one workflow instance. Each layer progresses autonomously while coordinating through domain events.</p>

## Core Concept

A **StateFlow** contains multiple **Layers**, each representing an independent dimension of the workflow. Think of it as a collection of parallel state machine segments that share a single instance context.

<div class="architecture-image" style="margin:24px 0">
<img src="{{ site.baseurl }}/assets/images/architecture.svg" alt="Multi-layer architecture diagram">
</div>

## Key Characteristics

- **Each layer is autonomous** — Own states, transitions, guards, and payload namespace.
- **Independent progress** — Layers advance at their own pace; one layer can be halted while another completes.
- **Default layer** — The `StateFlow` designates a `defaultLayerId` as the primary entry point.
- **Start and end states** — Every layer must have exactly one start state and one end state.

## Event-Driven Inter-layer Communication

Layers communicate exclusively through **Domain Events** — never via direct method calls. This ensures loose coupling and auditability.

- `StateTransitioned` — Fired when a layer moves between states.
- `LayerHalted` — Propagated to dependent layers when one halts.
- `TaskCreated` — Signals the task system for human intervention.
- `StateModelCompleted` — Emitted when all layers reach their end states.

## Namespace Payload Isolation

Each layer writes to its own `layerPayloads` map, preventing namespace collisions. On completion, payloads are merged into a single `mergedPayload`.

- **Isolation** — Layer A cannot accidentally overwrite Layer B's data.
- **Explicit merge** — The merged payload is assembled at completion time.
- **Visibility** — Contracts define which agents can read which layer payloads.

## Layer Dependencies

Layers can declare dependencies on other layers via `LayerDependency` records. When a dependency halts, dependent layers automatically receive `LayerHalted` propagation.

- **Prerequisite layers** (`dependsOn`) — Must complete before this layer starts.
- **Dependent layers** (`requiredBy`) — Auto-notified when this layer halts.
- **Collapsed summary** — UI shows progress dots, retry counts, and blockers for dependent layers.

## Layered Guard Contracts

Guards can inspect cross-layer state to enforce business rules:

> "Only approve payment (Layer B) if inventory check (Layer A) is complete."

This is enforced via the `GuardRegistry` evaluating conditions across layer boundaries, not through hardcoded logic.

</div>
