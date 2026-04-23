---
title: "Hermes Local Adapter"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend", "engineering/frontend"]
---

# Hermes Local Adapter

Adapter for **Hermes** (a local coding agent). Registered under adapter type key `hermes_local` and exposed in the UI as "Hermes Agent".

**Source:** `server/src/adapters/registry.ts`, `ui/src/adapters/hermes-local/index.ts`, and `ui/src/components/AgentConfigForm.tsx`; Hermes-specific runtime helpers come from the external `hermes-paperclip-adapter` dependency referenced by `server/package.json` and `ui/package.json`.
**UI module:** `ui/src/adapters/hermes-local/index.ts`
**Server registry:** `server/src/adapters/registry.ts`

---

## Config shape

Unlike most adapters, Hermes stores its local command override under the field name `hermesCommand` rather than the shared `command` field. The UI builds its adapter config via `buildHermesConfig` from the Hermes package, and the generic `AgentConfigForm` special-cases `adapterType === "hermes_local"` to read/write `hermesCommand` instead of `command`.

## Command override normalization

Because older agent records (and the schema-backed config form) may still carry the command under the generic `command` key, the server registry runs a `normalizeHermesConfig` step before dispatching execution. It inspects both `ctx.config` and `ctx.agent.adapterConfig`, and when `hermesCommand` is missing but a non-empty string `command` is present, it copies `command` into `hermesCommand` so downstream execution honors the user's override. This preserves backward compatibility with agents created before Hermes moved to its dedicated config field.

---

## Related

See [adapters/NODE.md](../NODE.md) for the adapter interface contract and other local adapters (Claude, Codex, Cursor, Gemini, OpenCode, Pi) which use the shared `command` field convention.
