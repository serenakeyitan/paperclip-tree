---
title: "Inbox"
owners: []
---

## Overview

The inbox is the primary triage surface for human operators. It aggregates issues assigned to the current user with filtering, search, keyboard navigation, and nested parent-child grouping.

## Key Decisions

- **Parent-child nesting** — Issues with sub-issues can be nested inline in the inbox view with a toggle button. Keyboard shortcuts (j/k) traverse nested children. Nesting is disabled on mobile for space reasons.
- **Persistent dismissals** — Non-issue inbox items (e.g., notifications) can be dismissed, and dismissals persist across sessions rather than resetting on reload.
- **Search with comment fallback** — Inbox search checks issue titles first, then falls back to matching against comment content for broader results.
- **Badge accuracy** — The inbox badge counts only unread issues assigned to the current user ('mine'), not all unread items.
- **Shared column controls** — Column show/hide controls (e.g., Parent Issue, time-ago columns) are reused between inbox and issue list views for consistency.
- **Keyboard shortcuts** — Full keyboard shortcut support including g i (go to inbox), archive, and a ? cheatsheet dialog.
