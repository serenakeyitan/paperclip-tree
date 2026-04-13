---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The Codex Local Adapter node documents capabilities but has no mention of fast mode support, which is a new execution mode that affects how tasks are dispatched to Codex.
---
Update the existing Capabilities section to add:

- **Fast mode:** Supported. Allows Paperclip to invoke Codex in fast mode for lower-latency responses at the cost of reduced reasoning depth. Controlled via adapter configuration.

Update the Key Decisions section to add:

- **Fast mode as an adapter-level concern** — Fast mode is toggled at the adapter layer rather than the task layer, keeping the task system runtime-agnostic while letting operators choose speed vs. depth per adapter instance.
