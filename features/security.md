---
title: Security
description: Three-tier authentication, multi-tenancy, rate limiting, and comprehensive audit logging.
---

<div class="content-page" markdown="1">

<h1>Security</h1>
<p class="page-desc">Parallel Path implements defense-in-depth security with multiple authentication paths, tenant isolation, rate limiting, and full audit logging. Security is profile-gated and only active in production profiles.</p>

## Three Authentication Paths

### 1. Master API Key
Configured via `PARALLELPATH_SECURITY_API_KEY` environment variable. Provides full admin access to all engine operations. Intended for infrastructure automation and initial setup.

### 2. Agent Service Account API Key
Scoped API keys for AI agent service accounts. Configuration includes:
- Allowed event types the agent can fire
- Readable layer payloads
- Maximum automated iterations before forced halt

### 3. User-Generated Scoped API Keys
Created at runtime via the `/api/keys` endpoint. Each key has:
- **Custom expiration** — Configurable TTL
- **Scopes** — Specific permissions (read, write, admin)
- **Owner tracking** — Bound to the creating user

## Multi-Tenancy

- `X-Tenant-Id` header propagates through the entire call chain via `TenantContext`
- Workflow instances, tasks, agents, and audit logs are tenant-scoped
- Team-based permissions control access within a tenant
- Namespace payload isolation prevents cross-tenant data leakage

## Rate Limiting

The `RateLimitFilter` provides:
- **Per-IP limits** — Protect against request flooding
- **Per-tenant limits** — Prevent one tenant from starving others
- Configurable thresholds in `application.yml`

## API Key Management

API keys are stored as hashed values (never plaintext). The API key management system:
- Supports key rotation with overlap windows
- Logs all key creation, revocation, and usage
- Provides `GET /api/keys` for auditing active keys
- Enforces minimum key length and character requirements

## Security Audit Logging

The `SecurityAuditService` records all security-relevant events:
- Authentication successes and failures
- API key usage
- Permission denied attempts
- Rate limit breaches
- Tenant isolation violations

## Profile Gating

Security features are only active in `prod` and `prod-distributed` Spring profiles. The `default` and `dev` profiles disable security for local development convenience.

| Profile | Security | Flyway | Redis |
|---------|----------|--------|-------|
| default | off | off | off |
| dev | off | on | off |
| distributed | off | off | on |
| prod | on | on | off |
| prod-distributed | on | on | on |

</div>
