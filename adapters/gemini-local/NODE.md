---
title: "Gemini Local Adapter"
owners: [cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# Gemini Local Adapter

Adapter for **Gemini CLI** (Google's AI coding agent). Spawns `gemini` as a local child process.

**Source:** `packages/adapters/gemini-local/`
**Package:** `@paperclipai/adapter-gemini-local`
**Adapter type key:** `gemini_local`

---

## Capabilities

- **Session resume:** Yes. Session params follow the shared pattern with three `sessionId` variants.
- **Native context management:** Unknown. Default compaction thresholds apply.
- **Skills:** Persistent skill sync via `listGeminiSkills` / `syncGeminiSkills`.
- **Environment test:** Checks `gemini` CLI installation and auth.
- **Output parsing:** Includes auth-required detection (`detectGeminiAuthRequired`), turn-limit detection (`isGeminiTurnLimitResult`), and failure description (`describeGeminiFailure`).

---

## Key Decisions

- **Dedicated `utils.ts`** in the server layer for Gemini-specific helper functions, unlike most other adapters that rely solely on adapter-utils.
- **Auth-required detection** is explicit because Gemini CLI can fail silently or with non-obvious errors when Google Cloud auth expires.
- **Turn-limit result detection** enables the server to distinguish "Gemini hit its turn cap" from other error exits, similar to Claude's max-turns handling.

## Decision Records

- [auth-expiry-and-turn-limit-awareness.md](auth-expiry-and-turn-limit-awareness.md) — Why Gemini auth-expiry and turn-limit cases are detected explicitly at the adapter edge.

---

## Boundaries

- No quota reporting. Google's Gemini API does not expose rate-limit windows in the same way as Anthropic or OpenAI.
- No model listing or detection. Model is configured at the agent level in Paperclip.
