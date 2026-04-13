---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Routines are a new product concept — draft routine defaults, run-time overrides, routine variables in titles, and CLI routines management — that represent a significant new capability not covered by any existing tree node.
---
## Routines

Routines are recurring task templates that agents can execute on a schedule or on-demand. They represent repeatable workflows with configurable parameters.

### Key Concepts

- **Draft routines** — Routines can be created in draft state with default parameters before being activated.
- **Run-time overrides** — When executing a routine, parameters can be overridden from their defaults for the specific run.
- **Variable substitution** — Routine titles support variable interpolation, allowing dynamic naming based on run context.
- **CLI management** — The CLI includes a `routines` subcommand for listing and managing routine definitions programmatically.

### Decisions

- Routines are a first-class entity in the data model, not just scheduled issues. This allows richer configuration and history tracking.
- Draft state allows iterating on routine definitions before they start executing.
