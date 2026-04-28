---
title: "Heartbeat Runtime Cleanup"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Heartbeat Runtime Cleanup

Hardening rules for how the heartbeat path tears down adapter child processes and how it derives a run's final outcome from its persisted status. These rules sit on top of the broader heartbeat run orchestration and exist so that misbehaving or hung adapter children do not leave runs in indeterminate states.

## Key Decisions

### Terminal-Result Cleanup Does Not Wait For Child Exit

`runChildProcess` in `packages/adapter-utils/src/server-utils.ts` arms its terminal-result cleanup timer as soon as a terminal result has been observed in the adapter's output, regardless of whether the child process has already exited. Previously cleanup also required `childExited`, which meant a child that emitted a final `{"type":"result"}` and then kept running (e.g. a stray `setInterval`) would never be reaped within the grace window. The `child.on("exit")` handler still calls `maybeArmTerminalResultCleanup` so exit alone can trigger cleanup, but the gate on terminal output no longer blocks on exit. Tests in `server-utils.test.ts` cover both the still-running-after-terminal-output case (expects `SIGTERM`) and the noisy-no-terminal-output case (no cleanup).

### Preserve All Terminal Run Statuses As Outcomes

When the heartbeat loop finishes invoking an adapter, it now consults `isHeartbeatRunTerminalStatus` against the latest persisted run row before classifying the outcome. If the run's status is any of `succeeded`, `failed`, `cancelled`, or `timed_out` (the new `HEARTBEAT_RUN_TERMINAL_STATUSES` constant in `server/src/services/heartbeat.ts`), that status is used directly as the outcome. Only when the persisted status is non-terminal does the heartbeat fall back to inferring the outcome from `adapterResult.timedOut`, exit code, and error message. This generalizes the prior special-case that only respected `cancelled`, so externally-driven terminal transitions (e.g. timeout or failure recorded by another path) are not overwritten by adapter-derived inference.

## How To Apply

When changing adapter child-process lifecycle or heartbeat outcome derivation, preserve both invariants: terminal-result cleanup must be armable from either terminal output or child exit independently, and any new terminal run status must be added to `HEARTBEAT_RUN_TERMINAL_STATUSES` so the heartbeat respects it as the authoritative outcome.
