---
title: "Codex Local Adapter"
owners: [cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# Codex Local Adapter

Adapter for **Codex CLI** (OpenAI's coding agent). Spawns `codex` as a local child process.

**Source:** `packages/adapters/codex-local/`
**Package:** `@paperclipai/adapter-codex-local`
**Adapter type key:** `codex_local`

---

## Capabilities

- **Session resume:** Yes. Session params follow the shared pattern (`sessionId`, `cwd`, `workspaceId`, `repoUrl`, `repoRef`).
- **Native context management:** Confirmed. Codex manages its own context, so Paperclip skips compaction thresholds.
- **Skills:** Persistent skill sync with `ensureCodexSkillsInjected` for injecting Paperclip skills before execution.
- **Quota:** Reads Codex auth info and fetches quota via the Codex RPC quota API. Includes window label conversion (`secondsToWindowLabel`).
- **Environment test:** Validates `codex` CLI installation and auth state.
- **Billing:** Infers biller from environment (supports OpenRouter detection via `inferOpenAiCompatibleBiller`).

---

## Key Decisions

- **Codex home directory resolution** (`codex-home.ts`) handles platform-specific config paths to locate Codex credentials and configuration.
- **RPC-based quota** (`fetchCodexRpcQuota`) instead of scraping CLI output. This provides structured rate-limit window data with reset times.
- **Skill injection before execution** via `ensureCodexSkillsInjected` rather than at skill-sync time. This ensures skills are always fresh for each run.

---

## Boundaries

- Does not manage OpenAI API keys. Auth is handled by the Codex CLI's credential store.
- Unknown session errors are detected (`isCodexUnknownSessionError`) and surfaced to the server for session reset decisions.
