---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: Draft routines with run-time overrides and variable support in titles were introduced — a new product concept with no tree coverage.
---
## Overview

Routines are recurring task templates that can be scheduled and customized at run time. They allow companies to define repeatable workflows that agents execute on a cadence.

## Key Decisions

- **Draft defaults and run-time overrides** — Routines support draft state with default configurations that can be overridden when a routine run is triggered. This allows templates to be flexible without requiring per-run manual setup.
- **Variable substitution in titles** — Routine titles support variables that are resolved at run time, enabling dynamic naming (e.g., including dates or parameters in the generated issue titles).
- **CLI surface** — Routines are accessible via CLI subcommands, consistent with other entity types (issues, agents, etc.).

## Relationship to Task System

Routines generate issues within the existing task hierarchy. Each routine run creates issues that follow the standard lifecycle defined in `product/task-system/`. The routine layer adds scheduling and templating on top of the task system, not a parallel execution path.
