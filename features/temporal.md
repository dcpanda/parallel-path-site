---
title: Temporal Management
description: Scheduled state starts, automated retries, and runtime temporal condition updates.
---

<div class="content-page">

<h1>Temporal Management</h1>
<p class="page-desc">Parallel Path supports time-based workflow automation through scheduled state starts, automated retry logic, and runtime intervention tools. States can be configured to activate at specific times or retry on failure with configurable intervals.</p>

## Scheduled Start Times

States can define a `scheduledStartTime` — the state remains inactive until the specified time, then automatically transitions:

```yaml
states:
  - name: scheduled_backup
    scheduledStartTime: "2025-01-01T02:00:00Z"
    transitions:
      - event: EXECUTE
        targetState: running
```

## Retry Logic

States can define retry behavior on failure:

```yaml
states:
  - name: processing
    transitions:
      - event: FAIL
        targetState: processing
        retryInterval: 30000
        maxRetries: 5
        failureStateName: failed
```

- **`retryInterval`** — Milliseconds between retries
- **`maxRetries`** — Maximum retry attempts before moving to failure state
- **`failureStateName`** — State to enter when max retries exceeded

## Dynamic Execution State

Each instance tracks runtime temporal data:
- **`retryCount`** — Current retry attempt number
- **`nextRetryTime`** — Calculated time for next retry
- **`scheduledStartTime`** — When a scheduled state should activate

This data is inspectable via the API and MCP tools.

## Automated Polling

The `StateModelService.pollTemporalTransitions()` runs every 1 second:
1. Queries for instances with pending temporal transitions
2. Checks if the scheduled time has arrived
3. Fires the automated event
4. Logs the temporal transition in the audit trail

## Runtime Intervention

The MCP server provides two tools for runtime temporal management:

### `updateStateTemporalConditions()`
Modify a state's scheduled start time or retry parameters while the workflow is running:
- Change the scheduled start time
- Adjust retry interval
- Override max retries

### `resetLayerRetries()`
Clear the retry count for a halted layer, allowing it to retry again:
- Useful when the root cause of failure has been addressed
- Preserves all existing state and guard configurations
- Logged as a manual intervention in the audit trail

## Observability

The `findInstancesWaitingForTemporal()` MCP tool lists all instances with pending temporal conditions:
- Instance ID and name
- Layer and state names
- Scheduled or retry time
- Time remaining until next action

</div>
