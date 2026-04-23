---
title: "Agent Max Concurrent Runs"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/agent-model/NODE.md", "product/governance/NODE.md"]
---

# Agent Max Concurrent Runs

Each agent has a `maxConcurrentRuns` setting on its heartbeat `runtimeConfig` that caps how many heartbeat runs the scheduler may have in flight simultaneously for that agent.

## Key Decisions

### New Agents Default to 5

V1 originally fixed `maxConcurrentRuns` at `1` per agent. This was too restrictive in practice — agents frequently had useful parallelizable work (e.g., responding to a comment wake while a separate assignment run is still finishing). New agents now default to `5`, exported as `AGENT_DEFAULT_MAX_CONCURRENT_RUNS` from `@paperclipai/shared`.

**Rationale:** Let agents overlap cheap work (comment wakes, reviews) with longer-running assignments without requiring an operator to hand-tune the field. 5 is conservative enough to keep adapter and cost blast radius bounded while unblocking common cases.

### Applied to Imports and New Creations

Company portability imports and new agent creation both populate `heartbeat.maxConcurrentRuns = 5` when the field is absent, keeping imported agents behaviorally consistent with freshly created ones.

## Source

- `packages/shared/src/constants.ts` — `AGENT_DEFAULT_MAX_CONCURRENT_RUNS`
- `server/src/services/agents.ts`, `server/src/services/company-portability.ts`
- `doc/SPEC-implementation.md` — scheduler spec
