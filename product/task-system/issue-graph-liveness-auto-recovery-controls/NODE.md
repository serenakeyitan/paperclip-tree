---
title: "Issue Graph Liveness Auto-Recovery Controls"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/governance/NODE.md, product/task-system/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Issue Graph Liveness Auto-Recovery Controls

Issue-graph liveness auto-recovery is the mechanism that converts advisory liveness findings on blocked dependency chains into recovery issues. It is exposed as an **instance-level experimental setting** and is **disabled by default**. Operators who turn it on also choose a lookback window that bounds how aggressively the system creates recovery issues.

## Key Decisions

### Lookback Window Is a Noise Control, Not a Staleness Delay

The `issueGraphLivenessAutoRecoveryLookbackHours` setting (default 24h, min 1h, max 30 days) means "dependency paths updated within the last N hours." Findings on chains updated outside that window remain advisory and are counted as `skippedOutsideLookback` rather than auto-creating recovery issues. This is explicitly an operator noise control, distinct from the older staleness delay that determined whether a chain was old enough to surface at all.

### Preview Before Enabling

Operators can call a preview endpoint (`POST /instance/settings/experimental/issue-graph-liveness-auto-recovery/preview`) to see exactly which issues would be escalated under a given lookback, including dependency paths, severity, recommended owner agent, and incident keys, without mutating state. The UI surfaces this as a dialog so operators can validate impact before flipping the toggle.

### On-Demand Run Independent of Heartbeat

A companion run endpoint (`.../run`) reconciles liveness immediately and reports `escalationsCreated`, `existingEscalations`, `skippedAutoRecoveryDisabled`, and `skippedOutsideLookback`. This lets operators drain a backlog of findings on demand rather than waiting for the next heartbeat sweep, and it works even when the auto-recovery toggle is off (force path).

### Defaults Bias Toward Safety

Auto-recovery stays off by default on every instance. Enabling it is a deliberate operator action that requires `manage instance settings` authority, and the lookback bounds (1h–720h) prevent both runaway noise and accidentally-empty windows.

## How This Connects

- Liveness *findings* and the `liveness` metadata field are described in [run-liveness-continuations](../run-liveness-continuations/NODE.md). This node covers the *control plane* on top of those findings.
- Recovery issues created here flow through the standard issue/escalation system; severity and recommended owners follow the recovery service's existing rules.
- The setting lives alongside other experimental toggles (`enableEnvironments`, `enableIsolatedWorkspaces`, `autoRestartDevServerWhenIdle`).
