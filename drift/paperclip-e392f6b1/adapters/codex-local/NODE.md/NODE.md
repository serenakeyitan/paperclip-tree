---
title: "Codex Local Adapter"
owners: []
---

Update the existing Capabilities section to add:

- **Fast mode:** Supported. Allows Paperclip to invoke Codex in fast mode for lower-latency responses at the cost of reduced reasoning depth. Controlled via adapter configuration.

Update the Key Decisions section to add:

- **Fast mode as an adapter-level concern** — Fast mode is toggled at the adapter layer rather than the task layer, keeping the task system runtime-agnostic while letting operators choose speed vs. depth per adapter instance.
