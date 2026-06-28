---
title: Guard System
description: Every transition passes through configurable guards that enforce governance before any state change.
---

<div class="content-page" markdown="1">

<h1>Guard System</h1>
<p class="page-desc">Every state transition in Parallel Path is governed by a configurable guard registry. Before any state change occurs, all registered guards must pass — ensuring no ungoverned moves in the system.</p>

## How Guards Work

When an event triggers a transition, the `GuardRegistry` evaluates all guards configured on that transition. The transition only executes if every guard passes.

<div class="architecture-image" style="margin:24px 0">
<img src="{{ site.baseurl }}/assets/images/guard-system.svg" alt="Guard system flow diagram">
</div>

## Guard Types

### AI Guard (`AiGuard`)
Uses configured LLM prompt templates to evaluate whether a transition should proceed. The guard:
- Sends the current state context to the LLM with a prompt template
- The LLM returns a pass/fail decision with reasoning
- Decisions are logged in the audit trail

### Voting Guard (`VotingGuard`)
Multi-agent consensus gate. Configuration includes:
- **Quorum threshold** — Minimum votes required (configurable per guard)
- **Voter pool** — Registered agents or human voters
- **Deadline** — Time window for collecting votes
- **Escalation** — Automatic escalation to designated approvers on timeout

### Webhook Guard (`HttpWebhookGuard`)
Validates transitions by calling an external system:
- **HMAC-SHA256 signed** payload delivery
- **Timeout** — Configurable response window
- **Response validation** — Parse external system's pass/fail decision
- **Retry** — Exponential backoff on delivery failures

### SpEL Expression Guard (`SpelExpressionGuard`)
Uses Spring Expression Language (SpEL) for inline boolean conditions:
- Evaluate payload fields, layer state, or instance metadata
- No external dependencies — evaluated entirely in-engine

### Named Guard (`NamedGuard`)
References a named guard implementation registered in the `GuardRegistry`. Allows reusable guard logic across multiple transitions.

### Conditional Transition
A transition that only fires when certain conditions are met. Often used for automated, data-driven state progression.

## Cross-Layer Contracts

Guards can enforce rules based on other layer states through cross-layer contracts:

```yaml
guard:
  type: spel
  condition: "#layerState('payment') == 'APPROVED'"
```

If a guard fails:
- The transition is blocked.
- An audit log entry records the failure with reasoning.
- The failed guard result is stored for inspection.
- After exceeding `state-model.maxAutomatedIterations` (default: 100), the instance is halted.

</div>
