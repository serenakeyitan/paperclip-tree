---
title: "Pi Local Adapter"
owners: [cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# Pi Local Adapter

Adapter for **Pi** (an AI coding CLI). Spawns `pi` as a local child process.

**Source:** `packages/adapters/pi-local/`
**Package:** `@paperclipai/adapter-pi-local`
**Adapter type key:** `pi_local`

---

## Capabilities

- **Session resume:** Yes. Session params use `sessionId`/`session_id`/`session` variants (note: `session` instead of `sessionID`, unlike other adapters) plus `cwd`/`workdir`/`folder`.
- **Native context management:** Unknown. Default compaction thresholds apply.
- **Skills:** Persistent skill sync via `listPiSkills` / `syncPiSkills`.
- **Models:** Dynamic model discovery (`discoverPiModels`) with a cached variant (`discoverPiModelsCached`). Includes model availability validation (`ensurePiModelConfiguredAndAvailable`).
- **Environment test:** Validates CLI installation and configuration.
- **Output parsing:** JSONL-based (`parsePiJsonl`) with unknown-session error detection.

---

## Key Decisions

- **Session field name divergence** -- Pi uses `session` as a third variant instead of `sessionID`. The codec handles this transparently.
- **Simpler session params** -- Only `sessionId` and `cwd` are serialized (no `workspaceId`, `repoUrl`, `repoRef`), reflecting Pi's simpler session model.
- **Cached model discovery** (`discoverPiModelsCached`) is a separate export from the uncached version, giving callers explicit control over freshness vs. performance.
- **Architecture mirrors opencode-local** -- Both share the dynamic model discovery + validation pattern and have similar test structures, suggesting they may have been developed together or from a common template.

---

## Boundaries

- No quota reporting.
- Model config is read-only. The adapter discovers but does not write Pi configuration.
