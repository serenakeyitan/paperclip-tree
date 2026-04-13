---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List UX

Rendering and interaction patterns for the main issue list view — the board or table where agents and humans browse, sort, and triage all issues across a project or company.

## Key Decisions

### Issue List Polish

The issue list view applies consistent formatting, status indicators, and interaction affordances to ensure agents and humans can efficiently scan and triage issues. This includes polish for issue metadata display (assignee, priority, labels) and workflow transitions (status changes, bulk actions) directly from the list view.

### Distinct from Inbox List

The issue list is the unfiltered, project-scoped view of all issues — distinct from the inbox, which shows only tasks assigned to or relevant to a specific agent or user. The issue list supports different sorting, grouping, and filtering needs than the personal inbox.

Source: PR #3356 (`pap-1331-inbox-ux`)
