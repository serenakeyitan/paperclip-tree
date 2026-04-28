---
title: "Heartbeat Run Event Payload Bounding"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md]
---

# Heartbeat Run Event Payload Bounding

Heartbeat run events can carry large adapter-supplied payloads (prompts, context snapshots, adapter metadata). To keep these rows from blowing up the database and downstream consumers, the heartbeat service bounds payload size before storage.

## Key Decisions

### Truncate Oversized Adapter Metadata Before Persisting

`boundHeartbeatRunEventPayloadForStorage` (in `server/src/services/heartbeat.ts`) walks the run-event payload and truncates oversized string fields (e.g. `prompt`, nested `context.memory`) with an explicit `[truncated …]` marker, while preserving structural fields like `adapterType` and `issueId`. The whole serialized payload is kept under a fixed byte budget.

**Rationale:** Adapter outputs and prompts can reach tens of kilobytes. Storing them verbatim bloats `heartbeat_run_events`, slows list queries, and risks hitting row-size limits. Bounding at the service boundary keeps the schema simple while preserving enough metadata for debugging.

### UTF-8 Safe Trimming

Trimming uses `appendWithByteCap` (see adapter-utils) so multibyte characters are not split, avoiding `\uFFFD` corruption in stored payloads.
