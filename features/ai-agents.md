---
title: AI Agent Integration
description: Native Model Context Protocol (MCP) server for AI agent orchestration with scoped contracts and LLM guard prompts.
---

<div class="content-page">

<h1>AI Agent Integration</h1>
<p class="page-desc">Parallel Path provides a built-in MCP Server that exposes the full engine API to AI agents. Combine agent autonomy with governance through scoped contracts, LLM guard prompts, and activity tracking.</p>

## MCP Server

The Model Context Protocol (MCP) server exposes 12+ engine tools to AI agents via Server-Sent Events at `/mcp`. This follows the Spring AI MCP Server specification, allowing any MCP-compatible client (including Claude, Cursor, and custom agents) to interact with the state engine.

### Available Tools

| Tool | Purpose |
|------|---------|
| `fireEvent` | Send a state transition event with idempotency key |
| `createStateModel` | Define a new workflow from a JSON definition |
| `getInstance` | Retrieve current instance state and history |
| `listInstances` | Query instances by status, layer, or tenant |
| `simulateEvent` | Dry-run a transition to preview consequences |
| `getGuardStatus` | Check guard evaluation results for a transition |
| `findInstancesWaitingForTemporal` | Discover instances pending scheduled or retry transitions |
| `updateStateTemporalConditions` | Modify scheduled start or retry parameters |
| `resetLayerRetries` | Clear retry count for a halted layer |
| `assignTask` | Assign a task to a human or agent |
| `registerAgent` | Register a new agent service account |
| `getMetrics` | Query engine health and performance metrics |

## Agent Service Accounts

Agents authenticate using scoped API keys. Each agent service account has:

- **Scoped event types** — Only allowed to fire specified event types.
- **Readable layers** — Can only access payloads from permitted layers.
- **Activity tracking** — All agent actions logged to `AgentActivityEntity`.
- **JWT tokens** — For session-based authentication.

## Agent Contracts (`AgentEventContract`)

Define exactly which events an agent can fire and which layer payloads it can read:

```yaml
agent:
  allowedEventTypes: ["APPROVE", "REJECT", "ESCALATE"]
  readableLayers: ["order", "payment"]
  maxAutomatedIterations: 25
```

Contracts prevent agents from exceeding their authority — if an agent tries to fire an unauthorized event, the guard registry blocks the transition.

## LLM Guard Prompts

When using `AiGuard`, you configure prompt templates that define the AI's decision boundaries:

```
You are a guard evaluating a state transition.
Current state: {{currentState}}
Target state: {{targetState}}
Layer context: {{layerPayload}}
Rules: {{guardRules}}
Respond with PASS or FAIL and a brief reason.
```

Prompt templates are stored in configuration, not hardcoded, so they can be tuned without code changes.

## Activity Tracking

All agent actions are recorded in `AgentActivityEntity`:
- Agent ID and name
- Event type fired
- Instance and layer affected
- Timestamp and outcome
- Evidence payload (guard decisions, LLM responses)

</div>
