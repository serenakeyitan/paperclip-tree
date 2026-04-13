---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node for company-scoped skills as a product-level entity with UI-driven lifecycle management and agent-usage dependency checks on deletion; a proposal already exists but hasn't been promoted to a tree node.
---
# Company Skills

Company Skills are company-scoped resources that define capabilities available to agents within a company. Unlike adapter-level skill injection (which handles runtime delivery of skills into agent environments), Company Skills are a **product-level entity** managed through the Paperclip UI.

## Key Decisions

### Skills as Company-Scoped Resources

Skills are owned by a Company and follow the same company-scoped isolation as all other Paperclip entities. A skill belongs to exactly one company and is only visible/assignable within that company boundary.

### Agent-Usage Guard on Deletion

Before a company skill can be deleted, the system checks whether any agents are currently using that skill. If agents reference the skill, the UI surfaces this dependency and requires explicit confirmation or reassignment before proceeding. This prevents orphaned agent configurations.

**Rationale:** Skills may be deeply integrated into agent workflows. Silently deleting a skill that agents depend on would cause runtime failures. The usage check follows the governance-first principle — destructive mutations require visibility into downstream impact.

### UI-Driven Skill Management

Skill creation, editing, and deletion are managed through the frontend Settings or company configuration pages. This is distinct from adapter-level skill sync (which is an infrastructure concern handled automatically by adapters).

## Relationship to Adapter Skill Injection

Company Skills define **what skills exist** at the product level. Adapter skill injection (see `adapters/NODE.md`) handles **how skills are delivered** into agent runtime environments. The two layers are complementary: company skills are the source of truth; adapters consume them and inject them into agent workspaces via symlinks or equivalent mechanisms.
