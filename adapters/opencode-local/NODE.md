---
title: "OpenCode Local Adapter"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# OpenCode Local Adapter

Adapter for **OpenCode** (an open-source AI coding CLI). Spawns `opencode` as a local child process.

**Source:** `packages/adapters/opencode-local/`
**Package:** `@paperclipai/adapter-opencode-local`
**Adapter type key:** `opencode_local`

---

## Capabilities

- **Session resume:** Yes. Session params follow the shared three-variant pattern (`sessionId`/`session_id`/`sessionID`).
- **Native context management:** Unknown. Default compaction thresholds apply.
- **Skills:** Persistent skill sync via `listOpenCodeSkills` / `syncOpenCodeSkills`.
- **Models:** Dynamic model discovery (`discoverOpenCodeModels`) and cached listing (`listOpenCodeModels`). Includes validation that the configured model is available (`ensureOpenCodeModelConfiguredAndAvailable`).
- **Environment test:** Validates CLI installation, auth, and model availability.
- **Runtime config:** Dedicated `runtime-config.ts` for reading and validating OpenCode's configuration files.

---

## Key Decisions

- **Dynamic model discovery** rather than a hardcoded list. OpenCode supports pluggable providers, so available models depend on which providers are configured. The adapter scans the runtime config to build the model list.
- **Model availability validation** (`ensureOpenCodeModelConfiguredAndAvailable`) at execution time prevents confusing failures when a model is configured in Paperclip but not available in the local OpenCode installation.
- **Runtime config module** (`runtime-config.ts`) isolates OpenCode config file parsing. This is tested independently (`runtime-config.test.ts`) since config format changes across OpenCode versions.
- **Test cache reset** (`resetOpenCodeModelsCacheForTests`) exposes cache invalidation for test isolation, indicating the model list is cached in-process for performance.

## Decision Records

- [dynamic-model-discovery-from-runtime-config.md](dynamic-model-discovery-from-runtime-config.md) — Why OpenCode model choices are discovered from local runtime config instead of being hardcoded in Paperclip.

---

## Boundaries

- No quota reporting. OpenCode proxies to various providers; quota is provider-dependent and not uniformly exposed.
- Model config is read-only. The adapter discovers models but does not write to OpenCode's config files.
