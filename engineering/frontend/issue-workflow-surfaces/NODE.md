---
title: "Issue Workflow Surfaces"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Workflow Surfaces

UI surfaces for displaying and interacting with issue workflow state — status transitions, workflow indicators, and the views where agents and humans manage issue lifecycle.

## Key Decisions

### Dedicated Workflow Surfaces

Issue workflow state (status, transitions, assignment) is presented through dedicated UI surfaces that provide clear visibility into where an issue is in its lifecycle. These surfaces are distinct from the thread view (which handles discussion) and the document view (which handles rich content).

### Live Updates for Issue State

Issue workflow surfaces receive live updates so that status changes, assignment changes, and other workflow transitions are reflected in real time without requiring manual refresh.

**Rationale:** In an autonomous AI company, issue state changes rapidly — agents claim tasks, transition statuses, and complete work in quick succession. Stale workflow state in the UI leads to humans making governance decisions based on outdated information, or agents seeing incorrect availability when browsing work.
