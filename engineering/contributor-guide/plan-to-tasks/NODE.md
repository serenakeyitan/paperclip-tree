---
title: "Converting Plans to Executable Tasks"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [product/agent-model/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/NODE.md, product/task-system/issue-blockers/NODE.md]
---

How a plan becomes executable work in a Paperclip company. This is method, not format — the plan structure is flexible, but the translation into issues follows fixed conventions so Paperclip's executor can pick the work up automatically.

## Key Decisions

### Plans live as issue documents, not repo files

Plans are stored as the issue's `plan` document (issue-document key `plan`), not appended to the issue description and not committed as files in the repo unless explicitly requested. Plan revisions update the same document. Comments referencing the plan link directly to the document by key.

### Assign for specialty, including to yourself

Every deliverable becomes an issue assigned to the agent best suited to it, based on the company's actual org chart and role descriptions — not defaulted to the planner. When the planner is the best fit, they take it; when no agent fits, the gap (hire, tool, decision, external dependency) is surfaced rather than papered over.

### Express dependencies through `blockedByIssueIds`, not prose

Real blockers are wired via the `blockedByIssueIds` field on issues. Paperclip's executor automatically starts any assigned task with no open blockers and auto-wakes dependents when blockers reach `done`. Independent branches of the dependency graph should start in parallel — most agents allow concurrent runs, so parallel work can land on the same agent.

### Approval gating uses `request_confirmation`

When a plan needs explicit approval before implementation, update the `plan` document and create a `request_confirmation` issue-thread interaction bound to the latest plan revision. Wait for acceptance before creating implementation subtasks.
