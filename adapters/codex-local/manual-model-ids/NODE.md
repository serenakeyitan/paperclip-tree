---
title: "Codex Manual Model IDs"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/capability-flags/NODE.md, adapters/codex-local/NODE.md, adapters/codex-local/fast-mode/NODE.md]
---

The Codex local adapter distinguishes **known** model IDs (those listed in the adapter's `models` array, e.g. `gpt-5.3-codex`, `gpt-5.4`) from **manual** model IDs (any non-empty model string the user enters that isn't in the known list). This distinction is exposed via `isCodexLocalKnownModel()` and `isCodexLocalManualModel()` from `@paperclipai/adapter-codex-local`.

## Why

Codex CLI evolves faster than Paperclip's pinned model list. Users need to be able to point an agent at a newly released or experimental Codex model without waiting for Paperclip to add it to the registry. Treating any unrecognized non-empty model string as a manual override gives that escape hatch without losing the curated UX for known models.

## Fast Mode Implications

Fast mode was previously gated strictly on `CODEX_LOCAL_FAST_MODE_SUPPORTED_MODELS` (currently `gpt-5.4` only). With manual model IDs, `isCodexLocalFastModeSupported()` now returns `true` for any manual model as well — Paperclip passes `service_tier="fast"` and `features.fast_mode=true` through to Codex and trusts the user's choice. For known-but-unsupported models (e.g. `gpt-5.3-codex`), fast mode remains ignored and the environment probe surfaces the `codex_fast_mode_unsupported_model` warning with the hint to switch to GPT-5.4 or enter a manual model ID.

## How to apply

- Treat manual model IDs as a deliberate user override, not an error condition. Don't add validation that rejects unknown Codex model strings.
- When adding new Codex capability flags that depend on the model, decide explicitly whether manual IDs should opt in (pass-through, like fast mode) or opt out (capability requires a known entry).
- UI surfaces that show fast-mode availability should reflect that manual model IDs qualify.
