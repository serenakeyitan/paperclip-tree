---
title: "RPC Quota And Fresh Skill Injection"
owners: [cryppadotta, serenakeyitan]
---

# RPC Quota And Fresh Skill Injection

The Codex adapter needs live operational information and reliable skill state,
but both concerns become brittle if Paperclip depends on scraped terminal text
or stale synced files.

## Decision

Use Codex RPC endpoints for quota discovery and inject Paperclip skills
immediately before execution. Treat Codex as a runtime with confirmed native
context management rather than applying Paperclip's generic compaction logic.

## Why

Structured RPC quota responses are more reliable than parsing human-readable CLI
output, and pre-run skill injection keeps the runtime's task-facing behavior
fresh even when local skill state has drifted.

## Implications

- The control plane can surface quota windows and reset timing without relying
  on fragile text parsing.
- Skill freshness becomes part of the execution path, reducing cases where the
  adapter runs with outdated Paperclip instructions.
- Session resume semantics still flow through Paperclip's shared adapter
  contract, but context-window policy stays native to Codex.
- Operational errors such as unknown-session handling remain adapter concerns,
  not backend special cases.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
