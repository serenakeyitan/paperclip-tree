---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Routines — draft defaults, run-time overrides, routine variables in titles — are a new product concept with no tree coverage.
---
## Routines

Routines are repeatable issue templates that can be scheduled or triggered on demand. They allow teams to codify recurring agent workflows.

### Key Decisions

- **Draft defaults.** Routines support draft mode, allowing templates to be prepared without immediately activating them.
- **Run-time overrides.** When a routine is triggered, parameters can be overridden at execution time, allowing the same template to serve multiple contexts.
- **Variable interpolation in titles.** Routine titles support variable substitution (e.g. date, workspace name), so each instantiated issue has a unique, descriptive title.

### Soft Links

- [Task System](task-system/NODE.md) — routines create issues and inherit the task system's lifecycle.
- [Frontend UI](../engineering/frontend/NODE.md) — routines are listed as a top-level page in the frontend.
