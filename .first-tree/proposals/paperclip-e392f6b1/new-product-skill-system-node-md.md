---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree covers plugin SDK/runtime and adapter-level skill sync, but has no node for company-level skill management (CRUD, assignment to agents, usage tracking, deletion with dependency checks) — this PR introduces skill deletion UI with agent usage validation, which is a product-level concern not captured anywhere.
---
# Skill System

How skills are managed, assigned, and governed at the company level in Paperclip.

## Key Decisions

### Company-Scoped Skill Registry

Skills are registered at the company level and can be assigned to individual agents. The registry tracks which agents use each skill, enabling dependency-aware operations.

### Deletion Requires Agent Usage Check

Before a skill can be deleted, the system checks whether any agents are currently using it. This prevents breaking running agents by removing skills they depend on. The UI surfaces agent usage and blocks deletion when dependencies exist.

**Rationale:** Skills are a shared resource across agents. Removing a skill that an active agent depends on would cause silent failures during execution. The usage check makes this dependency visible and prevents accidental breakage.

### Skills vs Plugins

Skills are the unit of capability that agents consume. Plugins are the packaging and delivery mechanism. A plugin may provide one or more skills. Company-level skill management operates on the consumed capability, not the plugin packaging.

## Soft Links

- [plugins/NODE.md](../../plugins/NODE.md) — Plugin system that delivers skills
- [adapters/NODE.md](../../adapters/NODE.md) — Adapter-level skill sync that injects skills into agent runtimes
- [product/agent-model/NODE.md](../agent-model/NODE.md) — Agent model including skill assignment
