---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: adapters/claude-local/NODE.md
supersedes: adapters/claude-local/NODE.md
rationale: The Claude local adapter node lists model IDs but doesn't mention Bedrock authentication support or Bedrock model selection, which were added as significant new capabilities including region prefix matching and DIRECT_MODELS configuration.
---
## Bedrock Model Selection

The Claude local adapter now supports AWS Bedrock as an alternative model provider with configurable model selection.

### Features

- **Bedrock auth support** — Agents can authenticate via AWS Bedrock credentials instead of direct Anthropic API keys.
- **Model selection UI** — Users can select from available Bedrock model IDs in the adapter configuration.
- **Region prefix matching** — Global region prefix matching determines the correct Bedrock endpoint.
- **DIRECT_MODELS reuse** — The implementation shares the `DIRECT_MODELS` import for consistency between direct and Bedrock model lists.

### Decisions

- Bedrock support is additive — the adapter auto-detects whether to use direct Anthropic or Bedrock based on configuration, with no breaking changes to existing setups.
- `configurable-claimed-api-key-path` allows per-agent API key path configuration for multi-account setups.
