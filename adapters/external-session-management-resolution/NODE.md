---
title: "External Adapter Session Management Resolution"
owners: [bingran-you, cryppadotta, serenakeyitan, superbiche]
soft_links: [adapters/NODE.md, adapters/capability-flags/NODE.md, engineering/backend/NODE.md]
---

External adapter packages (loaded via `createServerAdapter()`) can declare their own `sessionManagement` policy on the returned `ServerAdapterModule`. The server adapter registry resolves session management for these externals using a fixed precedence:

1. **Module-provided `sessionManagement`** wins if present. This lets external adapters ship their own resume/native-context/compaction policy without host changes.
2. **Host fallback by adapter type.** If the module does not provide `sessionManagement`, the registry looks up the hardcoded built-in policy keyed by the adapter's `type`. This means an external adapter that overrides a built-in type (same `type` key) inherits the built-in's policy by default.
3. **Undefined** if neither is available.

This precedence is implemented by `resolveExternalAdapterRegistration` in `server/src/adapters/registry.ts` and applied during the init-time external-adapter load pass. The hot-install path (`server/src/routes/adapters.ts` → `registerServerAdapter`) already preserved module-provided `sessionManagement`; this change brings init-time registration to parity, and additionally applies the built-in registry fallback for externals overriding a built-in type.

**Why this matters:** session resume, native context management, and default compaction thresholds are governance-relevant runtime behaviors. External adapter authors get an explicit, testable contract for declaring these, while still being able to opt into host defaults by omitting the field.

**Source:** `server/src/adapters/registry.ts` (`resolveExternalAdapterRegistration`), tests in `server/src/__tests__/adapter-registry.test.ts`.
