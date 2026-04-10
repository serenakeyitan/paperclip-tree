---
title: "Cursor Local Adapter"
owners: [cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# Cursor Local Adapter

Adapter for **Cursor** (the AI-powered code editor's agent mode). Spawns the Cursor CLI as a local child process.

**Source:** `packages/adapters/cursor-local/`
**Package:** `@paperclipai/adapter-cursor-local`
**Adapter type key:** `cursor`

---

## Capabilities

- **Session resume:** Yes. Session params support `sessionId`/`session_id`/`sessionID` variants plus `cwd`, `workspaceId`, `repoUrl`, `repoRef`.
- **Native context management:** Unknown. Paperclip applies default compaction thresholds (200 runs, 2M tokens, 72 hours).
- **Skills:** Persistent skill sync with `ensureCursorSkillsInjected` for injecting Paperclip skills before execution.
- **Environment test:** Validates Cursor CLI availability and configuration.
- **Shared utilities:** Has a `shared/` directory for code shared between server and UI layers.

---

## Key Decisions

- **Context management marked "unknown"** because Cursor's internal context window behavior is not publicly documented. Conservative compaction thresholds are applied as a safety net.
- **Three session ID field variants** (`sessionId`, `session_id`, `sessionID`) supported in the codec to handle Cursor's inconsistent field naming across versions.

## Decision Records

- [conservative-context-and-session-normalization.md](conservative-context-and-session-normalization.md) — Why Cursor uses conservative compaction plus adapter-level session field normalization.

---

## Boundaries

- No quota/rate-limit reporting. Cursor does not expose usage metrics via its CLI.
- No model listing. Model selection is managed inside the Cursor editor itself.
