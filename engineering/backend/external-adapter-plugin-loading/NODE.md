---
title: "External Adapter Plugin Loading"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, engineering/backend/NODE.md, plugins/runtime/NODE.md, plugins/sdk/NODE.md]
---

Paperclip's V1 spec recognizes four ways to run an agent heartbeat: local CLI/session adapters, run-a-command process adapters, fire-and-forget HTTP/webhook adapters, and **external adapter plugins** loaded dynamically through the adapter/plugin flow. The last category is what lets self-hosted installs add new runtimes without forking core.

The architectural boundary is explicit: external adapters must be installable through the adapter/plugin flow and must not require hardcoded imports in `server/` or `ui/`. Local adapters (Claude, Codex, Gemini, OpenCode, Pi, Cursor) remain built-in because they require corresponding CLI/session setup on the host machine; everything beyond that set is expected to arrive as a plugin.

This preserves the control-plane/execution-services separation: the core app coordinates heartbeats, budgets, and governance, while the set of supported runtimes is open-ended. Cloud-grade plugin marketplace and packaged public distribution remain out of scope for V1 — only the local/self-hosted plugin runtime is in scope.

See `doc/GOAL.md`, `doc/PRODUCT.md` (Agent Execution), and `doc/SPEC-implementation.md` §5.2 for the canonical scope statements.
