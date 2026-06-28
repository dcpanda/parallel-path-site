---
title: Releases
description: Version history and changelog for Parallel Path.
---

<div class="content-page" markdown="1">

<h1>Releases</h1>
<p class="page-desc">Current version: <strong>v0.0.1-SNAPSHOT</strong> — active development. Track releases on <a href="https://github.com/{{ site.repository }}/releases" target="_blank" rel="noopener">GitHub Releases</a>.</p>

## v0.0.1-SNAPSHOT (In Development)

The project is in early active development. Recent highlights include:

### Parallel Layers & Architecture
- Multi-layer state flow engine with independent layer execution
- Event-driven inter-layer communication via domain events
- Namespace payload isolation with `layerPayloads` and `mergedPayload`
- Layer dependency management with automatic `LayerHalted` propagation

### Guard System
- `GuardRegistry` with support for AI, Voting, Webhook, and SpEL guards
- Cross-layer contract enforcement between layers
- Guard escalation with infinite loop detection
- Guard failure tracking with detailed audit records

### AI Agent Integration
- MCP Server exposing 12+ engine tools to AI agents
- Agent service accounts with scoped API keys
- Agent contracts (`AgentEventContract`) defining allowed events and readable layers
- Activity tracking via `AgentActivityEntity`

### Task System
- Five task types: `HUMAN_APPROVAL`, `HUMAN_REVIEW`, `AGENT_EXECUTION`, `ESCALATION`, `VOTE`
- Task lifecycle with SLA deadlines and automatic escalation
- `TaskContract` defining allowed events per task type per layer
- Agent and member assignee types

### Compliance & Audit
- Compliance evidence pack generation (SOC 2, HIPAA, GDPR)
- PDF summary download via OpenPDF
- Guard results and actor identity tracking in event store
- Audit export in CSV and JSON formats

### UI & Dashboard
- React 19 + Material Design 3 with Tailwind CSS 4
- Parallel track layout with expand/collapse and progress dots
- State sequence connectors with animated transition lines
- Slide-over inspector drawer for deep-dive state inspection
- SSE live status indicator with connection state badges

### Monitoring
- Custom Actuator health indicators (cache consistency, outbox lag, connection pool)
- `StateEngineMetrics` for transition and guard evaluation metrics
- Configurable health thresholds

### Security
- Three authentication paths: master key, agent key, user-scoped key
- Multi-tenancy with `X-Tenant-Id` propagation
- Rate limiting per IP and per tenant
- Profile-gated security (prod/prod-distributed only)

### Temporal Management
- Scheduled state start times (`scheduledStartTime`)
- Automated retry with configurable intervals and max retries
- Runtime temporal condition updates via MCP tools
- 1-second polling loop for pending temporal transitions

## Upcoming

- Stable API for first v0.1.0 release
- Enhanced webhook delivery statistics
- Additional guard type plugins
- Performance benchmarking suite

---

For a complete list of changes, see the <a href="https://github.com/{{ site.repository }}/releases" target="_blank" rel="noopener">GitHub Releases</a> page. To view recent commits:

```bash
git log --oneline -20
```

</div>
