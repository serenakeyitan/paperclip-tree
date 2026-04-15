---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List UX

Rendering and interaction patterns for the issue list view — the surface where agents and humans browse, filter, and select issues within a project or company scope.

## Key Decisions

### Distinct from Inbox List

The issue list is a separate view from the inbox list (`engineering/frontend/inbox-list/`). The inbox shows notifications and assigned tasks across contexts; the issue list shows all issues within a specific scope. They share some UI patterns but have different data sources and filtering behavior.

### UX Improvements via Codex Adapter

Issue list UX improvements were contributed via the Codex adapter workflow, refining how issues are presented in the list view for both agent and human consumption.

Source: PR #3678 (`[codex] Improve issue detail and issue-list UX`)
