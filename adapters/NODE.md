---
title: "Adapters"
owners: [cryppadotta, serenakeyitan]
soft_links: ["engineering/backend", "engineering/cli", "engineering/frontend"]
---

# Adapters

Adapters are the bridge between Paperclip's orchestration layer and the actual AI coding agents that do work. Each adapter knows how to invoke a specific agent CLI or gateway, parse its output into Paperclip's unified transcript format, manage sessions, and report usage/billing.

**Source:** `packages/adapters/` (per-adapter packages) and `packages/adapter-utils/` (shared library).

---

## Architecture

### Shared Interface (`ServerAdapterModule`)

Every adapter implements the `ServerAdapterModule` interface from `@paperclipai/adapter-utils`. This provides a uniform contract the server uses regardless of which agent is behind it:

- **`execute(ctx)`** — Spawn or connect to the agent, stream logs, return an `AdapterExecutionResult` with exit code, token usage, billing info, session state, and optional structured results.
- **`testEnvironment(ctx)`** — Pre-flight check that the agent CLI is installed, authenticated, and reachable. Returns pass/warn/fail with actionable hints.
- **`listSkills` / `syncSkills`** — Skill lifecycle management (install/uninstall Paperclip skills into the agent's native skill directory).
- **`sessionCodec`** — Serialize/deserialize session parameters so Paperclip can resume sessions across runs.
- **`getQuotaWindows`** — Fetch live provider rate-limit and quota status.
- **`detectModel`** — Read the agent's local config to discover the active model.
- **`getConfigSchema`** — Declarative UI config schema so the frontend can render adapter-specific settings without custom React code.
- **`onHireApproved`** — Lifecycle hook when an agent is approved/hired.

### Three-Layer Package Structure

Each adapter package has three layers:

1. **`server/`** — The `ServerAdapterModule` implementation. Spawns CLI processes (local adapters) or opens WebSocket connections (gateway adapters). Parses stdout/stderr into structured transcript entries.
2. **`cli/`** — The `CLIAdapterModule` for formatting agent output in the Paperclip CLI.
3. **`ui/`** — Stdout line parsers and transcript rendering for the web frontend.

### Local vs Gateway Pattern

- **Local adapters** (`*-local`) spawn the agent CLI as a child process on the same machine. They use `runChildProcess()` from adapter-utils, stream stdout/stderr, and parse output line-by-line.
- **Gateway adapters** (`*-gateway`) connect to a remote agent over WebSocket. They implement a request/response protocol with challenge-based auth, device pairing, and event streaming.

### Session Management

Adapters declare their session management capabilities via `ADAPTER_SESSION_MANAGEMENT` in adapter-utils:

- **`nativeContextManagement: "confirmed"`** — The agent handles its own context window (Claude, Codex). Paperclip does not apply threshold-based session compaction.
- **`nativeContextManagement: "unknown"`** — Paperclip applies default compaction thresholds (max 200 runs, 2M input tokens, 72 hours).

All sessioned adapters share a common `sessionCodec` pattern that normalizes field names (`sessionId`/`session_id`/`sessionID`, `cwd`/`workdir`/`folder`, etc.) for cross-adapter compatibility.

---

## Key Decisions

- **One package per adapter** rather than a monolithic adapter module. This keeps dependencies isolated (e.g., only openclaw-gateway depends on `ws`) and allows independent versioning via changesets.
- **adapter-utils as shared library** rather than base class inheritance. Adapters compose utilities (`runChildProcess`, `buildPaperclipEnv`, `renderPaperclipWakePrompt`) rather than extending a base. This avoids tight coupling while still sharing ~90% of process management, prompt rendering, and skill injection logic.
- **Declarative config schema** (`getConfigSchema`) over custom UI components. The frontend renders adapter settings from a JSON schema, so adding a new adapter never requires frontend changes.
- **Skill injection via symlinks** into a temporary `.claude/skills/` directory. This avoids modifying the user's actual project while still making Paperclip skills discoverable by the agent's native skill system.
- **Billing inference** is adapter-side. Each adapter reports `billingType`, `costUsd`, `provider`, and `biller` from its own output parsing, since different agents surface cost information differently.

---

## Sub-domains

- **[claude-local/](claude-local/NODE.md)** — Claude Code (Anthropic CLI) adapter
- **[codex-local/](codex-local/NODE.md)** — Codex CLI (OpenAI) adapter
- **[cursor-local/](cursor-local/NODE.md)** — Cursor editor agent adapter
- **[gemini-local/](gemini-local/NODE.md)** — Gemini CLI (Google) adapter
- **[openclaw-gateway/](openclaw-gateway/NODE.md)** — OpenClaw WebSocket gateway adapter
- **[opencode-local/](opencode-local/NODE.md)** — OpenCode CLI adapter
- **[pi-local/](pi-local/NODE.md)** — Pi CLI adapter

---

## Boundaries

- Adapters do NOT own task scheduling, queue management, or run orchestration. That belongs to the server/backend.
- Adapters do NOT persist state. Session params flow through the server; adapters only serialize/deserialize them.
- Adapters do NOT define the transcript schema. They parse agent output INTO the shared `TranscriptEntry` types defined in adapter-utils.
