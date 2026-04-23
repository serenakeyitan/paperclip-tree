---
title: "Routine Execution Filtering in Issue Lists"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Routine Execution Filtering in Issue Lists

Issues whose `originKind` is `routine_execution` are first-class issues but are often noise in human-facing lists. The issue list API exposes explicit filter flags to control whether they appear.

## Key Decisions

### Default Is Now Inclusive

The issue list endpoint includes routine-execution issues by default. Callers that want to hide them must pass `excludeRoutineExecutions=true`. Previously the default was the opposite (hidden unless `includeRoutineExecutions=true` was passed). The change reflects that most surfaces now want the full picture and opt out only when presenting a curated human view (e.g., the issues list with the "Hide routine runs" checkbox).

### Unread Inbox No Longer Hardcodes the Filter

The unread-issues query no longer unconditionally excludes routine executions. Routine-execution issues that are genuinely unread will now surface in the inbox, letting the UI decide whether to filter them.

### UI Mirrors the Inversion

The issue filters popover renamed its toggle from "Show routine runs" (`showRoutineExecutions`) to "Hide routine runs" (`hideRoutineExecutions`) so the checkbox state maps directly to the new server flag.
