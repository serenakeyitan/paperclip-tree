---
title: "Issue Blockers and Dependencies"
owners: []
---

## Overview

Issues can declare blocking relationships — an issue can block or be blocked by other issues. When a blocking issue is resolved, dependent issues are automatically woken up, enabling hands-off dependency management.

## Key Decisions

- **First-class blocker model** — Blockers are a dedicated concept, not just a label or tag. They appear in issue properties and are tracked via activity events.
- **Dependency wakeups** — When a blocking issue transitions to a resolved state, the system automatically wakes blocked issues. This prevents agents from polling or manually checking dependency status.
- **Activity event tracking** — Blocker creation, removal, and resolution generate activity events, providing an audit trail for dependency changes.
- **UI integration** — Blocking and sub-issue relationships are shown in the issue properties panel. Empty states (no blockers) render cleanly without placeholder text.
