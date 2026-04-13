---
title: "Heartbeat Run Orchestration"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Heartbeat Run Orchestration

How the backend server schedules, executes, and manages the lifecycle of agent heartbeat runs — the bridge between the heartbeat protocol (product layer) and adapter execution (adapter layer).

**Source:** `server/src/services/heartbeat*`, `server/src/routes/heartbeat*`, related run-management services.

---

## Architecture

### Run Lifecycle

A heartbeat run progresses through defined states: scheduled → executing → completed/failed/timed-out. The orchestration service manages these transitions, emitting `heartbeat_run_events` at each step for observability and audit. State transitions are atomic — a run cannot regress to a prior state.

### Adapter Coordination

The orchestration layer calls the adapter's `execute(ctx)` method and monitors the result. It handles adapter-level failures (process crash, timeout, unexpected exit codes) separately from agent-level failures (agent reports failure but adapter succeeded). This separation enables retry logic that distinguishes transient infrastructure failures from persistent agent errors.

### Failure Handling and Hardening

Run orchestration includes guards against edge cases: duplicate concurrent runs for the same agent, adapter processes that outlive their timeout window, partial results from crashed executions, and race conditions between scheduled heartbeats and manual triggers. These hardening measures ensure the system degrades gracefully rather than producing inconsistent state.

---

## Key Decisions

- **Orchestration lives in backend services, not adapters.** Adapters own execution mechanics; the server owns scheduling, lifecycle, and failure policy. This keeps adapters simple and ensures consistent behavior regardless of which adapter is in use.
- **Event-sourced run history.** Each state transition produces an immutable event in `heartbeat_run_events`, enabling full reconstruction of any run's lifecycle for debugging and audit.

---

## Boundaries

- The **heartbeat protocol** (when to fire, what context to include) is defined in `product/agent-model`. This node covers the **execution** of that protocol.
- **Adapter execution mechanics** (process spawning, output parsing) are owned by `adapters/`. This node covers the **coordination** around adapter calls.
