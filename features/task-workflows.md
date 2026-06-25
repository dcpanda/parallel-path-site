---
title: Task Workflows
description: Human-in-the-loop workflows with task lifecycle management, SLA deadlines, and escalation paths.
---

<div class="content-page">

<h1>Task Workflows</h1>
<p class="page-desc">Parallel Path provides a full task engine for human-in-the-loop workflows. Tasks represent points where a human decision, review, or action is required before the workflow can proceed.</p>

## Task Types

| Type | Purpose |
|------|---------|
| `HUMAN_APPROVAL` | A human must approve or reject a pending action |
| `HUMAN_REVIEW` | A human must review contextual information and provide feedback |
| `AGENT_EXECUTION` | An AI agent performs an action within scope |
| `ESCALATION` | Automatic escalation when SLA is breached |
| `VOTE` | Multi-party vote with configurable quorum |

## Task Lifecycle

Every task follows a well-defined lifecycle:

```
PENDING → ASSIGNED → IN_PROGRESS → COMPLETED
                                  → REJECTED
                                  → TIMED_OUT
```

1. **PENDING** — Task created, not yet assigned
2. **ASSIGNED** — Assigned to a human member or agent service account
3. **IN_PROGRESS** — Assignee is actively working on the task
4. **COMPLETED** — Task completed successfully (workflow continues)
5. **REJECTED** — Task rejected (workflow may branch)
6. **TIMED_OUT** — SLA deadline exceeded (escalation triggered)

## SLA Deadlines

Each task can have an SLA deadline:
- Configured at task creation time
- If breached, the task automatically escalates
- Escalation creates a new task assigned to a higher authority
- SLA deadlines can be overridden for specific assignees

## Task Contracts (`TaskContract`)

Task contracts define which events are allowed for which task types per layer:

```yaml
taskContract:
  type: HUMAN_APPROVAL
  allowedTaskEvents: ["approve", "reject", "requestInfo", "delegate"]
  layerId: "order-processing"
```

This ensures that a human approval task cannot be accidentally completed with a reject-only event, and vice versa.

## Assignee Types

- **`TaskAssigneeMember`** — Human team member with expected response time
- **`TaskAssigneeAgent`** — Agent service account with auto-accept config

## Task Creation Before Transitions

When a transition requires a task, the task must be created before the dependent transition can fire. The workflow progresses like this:

1. Transition reaches a state that requires human approval
2. Task is created (`TaskCreated` domain event)
3. Task is assigned to a human or agent
4. When the task completes, a follow-up event fires
5. The follow-up event triggers the next transition (through guard evaluation)

</div>
