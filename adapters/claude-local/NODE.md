---
title: "Claude Local Adapter"
owners: [cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# Claude Local Adapter

Adapter for **Claude Code** (Anthropic's CLI agent). Spawns `claude` as a local child process with `--print --output-format stream-json`.

**Source:** `packages/adapters/claude-local/`
**Package:** `@paperclipai/adapter-claude-local`
**Adapter type key:** `claude_local`

---

## Capabilities

- **Session resume:** Yes, via `--session-id` flag. Session params include `sessionId`, `cwd`, `workspaceId`, `repoUrl`, `repoRef`.
- **Native context management:** Confirmed. Claude Code manages its own context window, so Paperclip skips threshold-based session compaction.
- **Skills:** Persistent skill sync. Symlinks Paperclip skills into a temp `.claude/skills/` directory and passes it via `--add-dir`.
- **Models:** Lists available Claude models. Supports Bedrock model IDs.
- **Quota:** Reads Claude auth status and fetches quota windows from Anthropic's API and the Claude CLI usage output.
- **Environment test:** Checks that `claude` CLI is installed, authenticated, and reachable.
- **Billing:** Reports usage (input/output/cached tokens) and cost from Claude's stream-json output.

---

## Key Decisions

- **Stream-json output format** over plain text. This gives structured token usage, cost, and session ID in the output, eliminating fragile regex parsing.
- **`--dangerously-skip-permissions`** is configurable per-agent. Required for headless operation but flagged in environment tests when missing.
- **Login flow (`runClaudeLogin`)** is exposed as a separate export so the server can trigger interactive auth setup outside of task execution.
- **Max-turns detection** (`isClaudeMaxTurnsResult`) distinguishes "agent hit turn limit" from "agent errored," enabling the server to handle continuation differently.

---

## Boundaries

- Does not manage Claude API keys directly. Auth is handled by the Claude CLI's own credential store.
- Does not implement Claude's conversation protocol. Delegates entirely to the CLI binary.
