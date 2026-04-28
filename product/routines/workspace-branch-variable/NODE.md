---
title: "Routine workspaceBranch Automatic Variable"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/execution-workspaces/NODE.md, product/routines/NODE.md, product/task-system/NODE.md]
---

# Routine `workspaceBranch` Automatic Variable

Routines expose a built-in variable named `workspaceBranch` (`WORKSPACE_BRANCH_ROUTINE_VARIABLE` in shared constants) that Paperclip auto-populates from execution context when a routine run reuses an isolated execution workspace.

## Key Decisions

### Auto-Populated From Reused Isolated Workspaces

When a routine run is dispatched against a project whose execution workspace policy resolves to an isolated workspace, the routine service looks up the active execution workspace's `branchName` and supplies it as the value of `workspaceBranch`. The variable only needs to be referenced in the routine template for Paperclip to compute it; templates that don't reference it pay no lookup cost.

### Automatic Values Override Caller Input

`resolveRoutineVariableValues` takes an `automaticVariables` map that is consulted before payload-, manual-, and API-supplied values, and before the routine variable's `defaultValue`. Workspace-derived values like `workspaceBranch` are therefore authoritative — schedule, manual, API, and webhook callers cannot override them. This keeps routine runs honest about which branch they are actually executing against.

### Variable Discovery Drives Lookup

`extractRoutineVariableNames` is used to detect whether the routine template references `workspaceBranch` so the routine service knows when to fetch the execution workspace row. This avoids extra DB reads for routines that don't need the value.
