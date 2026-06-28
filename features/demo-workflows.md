---
title: Demo Workflows
description: Nine ready-to-run workflow examples demonstrating the full capabilities of Parallel Path.
---

<div class="content-page" markdown="1">

<h1>Demo Workflows</h1>
<p class="page-desc">The project includes nine self-contained demo workflows that showcase different aspects of the engine. Each workflow is defined as a JSON file in the <code>demo/</code> module and can be loaded at startup.</p>

## 1. Order Processing (simple-linear.json)
A basic three-state linear workflow demonstrating the core state machine engine.
- **States:** PENDING, PROCESSING, COMPLETED
- **Key feature:** Minimal workflow definition, event-driven transitions
- **Best for:** Understanding the fundamental state/transition model

## 2. Department Approval (parallel-layers.json)
Two independent parallel layers demonstrating concurrent execution.
- **Layer A:** Manager approval track
- **Layer B:** Director approval track
- **Key feature:** Two layers run independently; both must complete before the workflow is done

## 3. Inventory & Shipping (cross-layer.json)
Cross-layer dependency with automated transitions between layers.
- **Layer A:** Inventory check
- **Layer B:** Shipping dispatch (depends on inventory)
- **Key feature:** `LayerDependency` records control inter-layer execution order

## 4. Payment Approval (guarded.json)
Named guard conditions on transitions demonstrating guard enforcement.
- **Guard:** SpEL expression checks payment amount against approval limits
- **Key feature:** Configurable guard logic without code changes

## 5. Scheduled Backup (temporal.json)
Scheduled start and retry logic on failure.
- **Feature:** `scheduledStartTime` for delayed execution
- **Retry:** Automatic retry with configurable interval on failure
- **Key feature:** Temporal management with automated polling

## 6. Insurance Claim (insurance-claim.json)
Full reference architecture with four parallel layers, agent integration, voting guards, temporal conditions, and human escalation.
- **Layer A:** Claim intake (AI-assisted)
- **Layer B:** Fraud detection (voting guard with quorum)
- **Layer C:** Medical review (human task with SLA)
- **Layer D:** Payment processing (automated with temporal scheduling)
- **Scenarios:** Three variations showing different claim paths
- **Key feature:** Most comprehensive demonstration of all engine capabilities

## 7. Hiring Process (hiring.json)
Four-layer hiring workflow with scoring guards and executive override.
- **Layer A:** Resume screening
- **Layer B:** Technical interview (scoring guard)
- **Layer C:** HR review
- **Layer D:** Executive approval
- **Key feature:** Scoring guards evaluate candidate suitability with configurable thresholds

## 8. B2B Onboarding (b2b-onboarding.json)
Four-layer enterprise customer onboarding with KYC verification and system provisioning.
- **Layer A:** KYC verification
- **Layer B:** Contract signing
- **Layer C:** System provisioning
- **Layer D:** Welcome notification
- **Key feature:** Real-world enterprise workflow with external system integration points

## Running Demos

Demos are loaded automatically when running with the `demo` profile:

```bash
docker build -f demo/Dockerfile -t parallelpath-demo demo/
docker run -p 8081:8081 parallelpath-demo
```

Or with Maven:

```bash
mvn spring-boot:run -pl demo
```

Each demo generates sample instances at startup that are visible in the Swagger UI at <code>http://localhost:8081/swagger-ui.html</code>.

</div>
