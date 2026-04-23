---
title: "Dependency-Blocked Interaction"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/task-system/issue-blockers/NODE.md", "product/task-system/comment-wake/NODE.md"]
---

# Dependency-Blocked Interaction

When a heartbeat fires on an issue that still has unresolved blockers — for example, a human leaves a comment on a blocked parent — the agent must respond to the interaction without treating the underlying deliverable as unblocked. The wake prompt carries this as a first-class concept so adapters do not have to infer it.

## Key Decisions

### Blocked issues stay idle until the final blocker resolves

Per `doc/execution-semantics.md`, Paperclip does not queue a regular heartbeat run for a blocked issue. The canonical wake for deliverable work on a blocked issue is `issue_blockers_resolved`, fired when the last blocker closes. Any wake that fires earlier (e.g. `issue_commented`) is an **interaction** wake, not a work wake.

### Wake payload carries blocker context

The Paperclip wake prompt payload now includes `dependencyBlockedInteraction: boolean`, `unresolvedBlockerIssueIds: string[]`, and `unresolvedBlockerSummaries` (id, identifier, title, status, priority per blocker). This lets the agent acknowledge the comment, triage it, or escalate without needing to re-query the blocker graph.

### Default agent prompt codifies the behavior

The default `DEFAULT_PAPERCLIP_AGENT_PROMPT_TEMPLATE` now includes: "If woken by a human comment on a dependency-blocked issue, respond or triage the comment without treating the blocked deliverable work as unblocked." The rule is baked into the prompt so every adapter that uses the shared template inherits it.

## Source

- `packages/adapter-utils/src/server-utils.ts` — `dependencyBlockedInteraction`, `PaperclipWakeBlockerSummary`, default prompt.
- `packages/adapter-utils/src/server-utils.test.ts` — "renders dependency-blocked interaction guidance".
- `server/src/__tests__/heartbeat-dependency-scheduling.test.ts`.
- `doc/execution-semantics.md` (blocker idle rule).
