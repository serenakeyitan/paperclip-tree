---
title: "Agent Max Concurrent Runs Default"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/agent-model/NODE.md, product/governance/NODE.md]
---

# Agent Max Concurrent Runs Default

Each agent's heartbeat schedule includes a `maxConcurrentRuns` cap — the maximum number of concurrent heartbeat runs the scheduler will allow for that agent. This was previously fixed at `1` for V1 and is now a configurable integer with a default of `5`.

## Key Decisions

### Default to 5 Concurrent Runs

`AGENT_DEFAULT_MAX_CONCURRENT_RUNS = 5` (in `packages/shared/src/constants.ts`) is the default applied when creating new agents and when normalizing imported agent runtime configs (company portability). The SPEC describes `maxConcurrentRuns` as an integer rather than a fixed value, with new agents defaulting to `5`.

**Rationale:** V1's hard cap of `1` was overly conservative once the heartbeat scheduler, run liveness tracking, and approval gates matured. A default of `5` lets agents make progress on multiple issues in parallel without unbounded fan-out, while remaining tunable per agent.

### Threaded Through Creation, Import, and UI

The default is applied in agent creation routes/services, in company portability normalization (so imported agents land on the new default), and in the UI's new-agent runtime config helper. Existing agents keep whatever value they have stored.

## Related

- Issue approval and pending-approval activation paths (see `product/governance/issue-approvals`) interact with newly created agents but do not override the concurrency default.
