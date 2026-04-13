---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Company Skills are a new first-class product entity (company-scoped, UI-managed, with deletion guards) distinct from adapter-level skill injection.
---
# Company Skills

Skills are company-scoped capability definitions managed through the UI. They are distinct from adapter-level skill injection (which operates at the runtime layer).

## Key Decisions

### Company Scope

Skills are scoped to a company, not to individual agents or projects. This enables shared capability definitions across all agents within a company.

### Deletion Guards

An agent-usage dependency check prevents deletion of skills that are actively referenced by agent configurations. This prevents orphaned agent configs that reference non-existent skills.

### UI-Managed

Skills are created and managed through the Paperclip UI, giving human operators direct control over what capabilities are available to agents in their company.
