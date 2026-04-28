---
title: "Agent Instructions Bundle"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/claude-local/NODE.md, adapters/codex-local/NODE.md, product/agent-model/NODE.md]
---

How custom per-agent instructions are delivered to local managed-bundle adapters when hiring or updating an agent.

## Key Decisions

### Top-level `instructionsBundle` is the canonical surface

For local managed-bundle adapters (e.g. `claude_local`, `codex_local`), custom instructions must be sent as a top-level `instructionsBundle` field on the agent create/update payload, with the adapted `AGENTS.md` content under `files['AGENTS.md']` and `entryFile` pointing at it. Paperclip materializes the bundle into the agent's workspace.

### Do not use `adapterConfig.promptTemplate` for new agents

The legacy `adapterConfig.promptTemplate` and `bootstrapPromptTemplate` fields are no longer the right place for new-agent instructions. Skills, hire flows, and draft-review checklists explicitly forbid setting these for new hires. Treat them as deprecated for instruction delivery.

### Secrets stay out of the bundle

Long-lived tokens, API keys, and private URLs must not be embedded in `instructionsBundle`, `adapterConfig`, or any legacy prompt field. Use environment-injected credentials or scoped skills instead. This applies to both the bundle files and any adapter config that ships alongside them.
