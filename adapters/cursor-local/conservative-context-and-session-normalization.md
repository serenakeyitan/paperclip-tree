---
title: "Conservative Context And Session Normalization"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Conservative Context And Session Normalization

Cursor exposes less stable operational detail to Paperclip than some other
local runtimes, so the adapter has to absorb that ambiguity rather than push it
into the control plane.

## Decision

Treat Cursor as a runtime with unknown native context management and normalize
multiple session field variants at the adapter boundary so the rest of
Paperclip can still reason about continuity safely.

## Why

Cursor's CLI behavior around context windows and field naming is not stable
enough to become a core assumption. A conservative fallback keeps long-running
session behavior safe without requiring backend special cases for Cursor-only
quirks.

## Implications

- Paperclip applies generic compaction thresholds to Cursor agents instead of
  assuming Cursor will always manage context optimally on its own.
- Session continuity remains portable because the adapter translates
  `sessionId`, `session_id`, and `sessionID` variants into a shared contract.
- Cursor-specific parsing and UI support can evolve independently while the
  server continues to consume the standard adapter interface.
- If Cursor later exposes stronger guarantees, the adapter can tighten its
  contract without changing the overall control-plane model.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [engineering frontend](../../engineering/frontend/NODE.md)
