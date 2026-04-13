---
title: "Execution Lock Lifecycle"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Execution Lock Lifecycle

How Paperclip manages exclusive execution locks on workspaces and issues, including detection and reclamation of stale locks left by crashed or unresponsive agents.

**Source:** Server-side lock management in execution workspace and agent orchestration services. Relates to PIP-002.

## Problem

When an agent acquires an exclusive lock on a workspace or issue and then crashes, times out, or loses its heartbeat, the lock becomes stale. Without cleanup, the locked resource is permanently inaccessible to other agents, blocking work indefinitely.

## Key Decisions

### Server-Side Stale Lock Detection

The server is responsible for detecting and reclaiming stale execution locks, not individual agents or adapters. This ensures consistent enforcement regardless of which adapter or invocation path acquired the lock.

### Heartbeat-Linked Lock Validity

Execution locks are tied to agent heartbeat liveness. When an agent's heartbeat lapses beyond a configured threshold, its held locks become candidates for reclamation. This leverages the existing heartbeat infrastructure rather than introducing a separate lock-specific TTL system.

### Graceful vs. Forced Reclamation

Stale lock cleanup distinguishes between graceful release (agent completes or explicitly releases) and forced reclamation (server reclaims after heartbeat timeout). Forced reclamation is logged as a governance event in the activity trail for auditability.

## Boundaries

Execution lock lifecycle manages lock acquisition, renewal, and reclamation only. The execution workspaces domain owns the physical workspace; the task system owns logical assignment. Lock lifecycle sits between them, ensuring exclusive access invariants hold even under failure conditions.
