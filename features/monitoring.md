---
title: Monitoring
description: Real-time observability with custom health indicators, metrics, and Server-Sent Events.
---

<div class="content-page">

<h1>Monitoring</h1>
<p class="page-desc">Parallel Path provides comprehensive observability through Spring Actuator custom health indicators, structured metrics collection, and real-time Server-Sent Events for live workflow updates.</p>

## Custom Health Indicators

Three custom Actuator health indicators provide deep insight into engine health:

### Cache Consistency Indicator
Verifies that the in-memory cache is synchronized with the database. Reports:
- **Status** — UP / DEGRADED / DOWN
- **Check count** — Number of cache entries verified
- **Mismatch count** — Desynchronized entries found
- **Last check time** — Timestamp of most recent verification

### Outbox Lag Indicator
Monitors the outbox event queue for delivery backlog:
- **Queue depth** — Number of pending outbox events
- **Oldest event age** — Time since oldest unprocessed event
- **Delivery failures** — Count of failed delivery attempts

### Connection Pool Saturation
Tracks HikariCP connection pool health:
- **Active connections** — Currently in use
- **Idle connections** — Available for use
- **Pending requests** — Threads waiting for a connection
- **Max pool size** — Configured maximum

## Metrics Collection

The `StateEngineMetrics` service exposes key operational metrics:

- **Transition count** — Total state transitions processed
- **Guard evaluation count** — Number of guard checks performed
- **Average transition latency** — Time to process a transition
- **Active instances** — Currently running workflow instances
- **Halted instances** — Instances in halted state
- **Task throughput** — Tasks created/completed per minute

## Real-Time SSE Updates

The Server-Sent Events (SSE) system provides live workflow state changes:

### Subscription Endpoints
- `/sse/instance/{id}` — Subscribe to a specific instance
- `/sse/team/{id}` — Subscribe to all instances for a team
- `/sse/global` — Subscribe to all instances (admin)

### SSE Configuration
- **Timeout** — Configurable connection timeout
- **Keep-alive** — Configurable heartbeat interval
- **Max connections** — Global connection limit
- **Reconnection** — Automatic with exponential backoff

### SSE State Indicator
The dashboard shows a live connection status badge:
- **Connected** — Green dot with "Live" label
- **Reconnecting** — Amber dot with pulsing animation
- **Disconnected** — Red dot

## Configurable Thresholds

All monitoring thresholds are configurable in `application.yml`:

```yaml
parallelpath:
  health:
    enabled: true
    cache-consistency-check-interval: 30000
    outbox-lag-warn-threshold: 100
    connection-pool-warn-threshold: 80
```

</div>
