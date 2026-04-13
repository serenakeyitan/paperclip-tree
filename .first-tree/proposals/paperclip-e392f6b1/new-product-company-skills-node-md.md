---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node covering company-level skill lifecycle management (add/remove/delete skills, agent usage checks); existing nodes only cover skill injection at the adapter layer and the plugin SDK, not the product-level skill CRUD and governance rules.
---
# Company Skills

How skills are managed at the company level — assignment, removal, and lifecycle governance.

## Key Decisions

### Skills as Company-Scoped Resources

Skills are assigned to a company and then made available to agents within that company. The company is the unit of skill ownership; individual agents reference company-level skills rather than managing their own skill inventories independently.

**Rationale:** Company-scoping keeps skill governance centralized. Operators manage a single skill catalog per company rather than configuring skills per-agent.

### Deletion Requires Agent Usage Check

Before a skill can be removed from a company, the system checks whether any agents are currently using it. Skills in active use cannot be deleted without first unassigning them from all agents.

**Rationale:** Deleting a skill that an agent depends on would silently break that agent's capabilities. The usage check makes this a safe, explicit operation rather than a silent failure waiting to happen.

### UI-Driven Skill Management

Skill addition and removal are surfaced in the frontend UI as part of company configuration. This includes visual feedback on which agents use each skill, enabling operators to understand dependencies before making changes.

## Relationships

- **Adapters** handle the mechanics of skill injection (symlinking into agent workspaces). Company Skills handles the higher-level question of *which* skills a company has.
- **Plugins** define how skills are built and packaged. Company Skills defines how they are assigned and governed.
- **Agent Model** defines agent identity and capabilities. Company Skills determines which capabilities are available to agents in a given company.
