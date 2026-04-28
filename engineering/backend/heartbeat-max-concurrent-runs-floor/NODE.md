---
title: "Heartbeat Max Concurrent Runs Floor"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/stranded-issue-recovery/NODE.md]
---

# Heartbeat Max Concurrent Runs Floor

The heartbeat service normalizes an agent's `maxConcurrentRuns` configuration value through `normalizeMaxConcurrentRuns` in `server/src/services/heartbeat.ts`. The normalizer clamps the parsed value into the inclusive range `[HEARTBEAT_MAX_CONCURRENT_RUNS_MIN, HEARTBEAT_MAX_CONCURRENT_RUNS_MAX]`, where the minimum is `1` and the maximum is `10`.

## Key Decisions

### Floor Is 1, Not the Default

Previously the lower bound of the clamp was `HEARTBEAT_MAX_CONCURRENT_RUNS_DEFAULT`, meaning an operator who tried to throttle an agent below the default would silently be raised back to the default. The floor is now an explicit `HEARTBEAT_MAX_CONCURRENT_RUNS_MIN = 1`, so an agent can be configured to run strictly one assignment at a time.

### Honoring The Floor In Scheduling

When `maxConcurrentRuns` resolves to 1, the heartbeat scheduler must leave additional assignment wakes queued rather than starting a second concurrent run. This is exercised by the `honors maxConcurrentRuns 1 by leaving a second assignment wake queued` test in `server/src/__tests__/heartbeat-dependency-scheduling.test.ts`.

## Why This Matters

Single-slot agents are useful for serializing work on shared external resources (one writer at a time) and for cost or rate-limit reasons. Treating `1` as a first-class supported value — rather than rounding it up — is required for those use cases to work as configured.
