---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node covering company-level skill management (CRUD, deletion with agent usage checks, UI) — adapters/NODE.md only covers skill injection into agent runtimes, not the product-level skill lifecycle.
---
# Company Skills

Company-scoped skill definitions that can be assigned to agents and managed through the board UI.

## Key Decisions

### Skills are Company-Scoped Resources

Skills are defined at the company level and assigned to individual agents. This mirrors the real-world pattern where a company defines capabilities and assigns them to employees, rather than each employee defining their own.

### Deletion Requires Agent Usage Check

Before a skill can be deleted, the system checks whether any agents are currently using it. The UI surfaces which agents reference the skill and requires confirmation before proceeding. This prevents accidentally breaking agent configurations by removing skills they depend on.

**Rationale:** Skills are wired into agent behavior via adapter-level injection. Silently deleting a skill that agents reference would cause runtime failures without clear attribution. The usage check makes the blast radius visible before the action is taken.

### Skill Lifecycle: Create → Assign → Delete

Skills follow a simple CRUD lifecycle at the company level. The adapter layer handles the mechanics of injecting assigned skills into agent runtimes (see `adapters/NODE.md` for injection details).

## Boundaries

This domain covers the product-level skill model: what skills are, how they're scoped, and how they're managed. Adapter-level skill injection (symlinks, sync, runtime discovery) lives in `../../adapters/`. The backend routes and services live in `../../engineering/backend/`. The deletion UI lives in `../../engineering/frontend/`.
