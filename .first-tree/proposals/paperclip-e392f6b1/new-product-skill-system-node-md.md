---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree covers skill injection at the adapter layer but has no node for company-level skill management — the product concept of skills as company-scoped entities with CRUD lifecycle, agent usage tracking, and safe deletion with dependency checks.
---
# Skill System

How skills (reusable agent capabilities) are defined, managed, and assigned at the company level in Paperclip.

## Key Decisions

### Skills as Company-Scoped Entities

Skills are first-class resources scoped to a company. They represent reusable capability definitions (behavior prompts, tool configurations, specialized knowledge) that can be assigned to one or more agents. The company-level skill registry is distinct from the adapter-level skill injection mechanism — the registry manages *what* skills exist, while adapters handle *how* skills are delivered to agent runtimes.

### Agent Usage Checks on Deletion

Before a skill can be deleted, the system checks which agents currently reference it. The UI surfaces this dependency information to the operator, preventing accidental removal of skills that are actively in use. This follows Paperclip's governance-first principle: surface problems, don't silently break things.

### Skill Lifecycle: Create → Assign → Delete

Skills follow a simple lifecycle: created at the company level, assigned to agents (one skill can serve many agents), and deleted only after the operator acknowledges agent dependencies. There is no soft-delete or archive — deletion is permanent, matching Paperclip's preference for explicit operator decisions over hidden state.

## Relationship to Adapters

The adapter layer (`../adapters/`) handles the runtime mechanics of skill delivery — symlinks, directory injection, sync protocols. This domain covers the product-level concerns: what skills exist in a company, which agents use them, and the rules for safe modification and removal.

## Sub-domains

*(None yet — skill versioning, marketplace/sharing, and per-agent skill configuration may emerge as the system matures.)*
