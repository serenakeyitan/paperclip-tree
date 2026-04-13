---
title: "Keyboard Shortcuts"
owners: []
---

## Keyboard Shortcuts

The frontend now supports a comprehensive keyboard shortcut system for power-user navigation and actions.

### Features

- **Cheatsheet dialog** — Pressing `?` opens a keyboard shortcut reference dialog.
- **Navigation shortcuts** — `g i` navigates to inbox from issue detail. Scope-prefixed shortcuts (e.g., `g c` for comment composer) are guarded from leaking into global shortcut handlers.
- **Issue traversal** — `j`/`k` keys traverse issues in inbox, including nested child issues when parent-child nesting is enabled.
- **Issue actions** — Shortcuts for archive, comment composer focus, and review actions are available on issue detail pages.
- **Focus management** — Issue detail main pane receives focus on navigation; scroll-to-bottom is offset when the properties panel is open.

### Decisions

- Vim-style `j`/`k` navigation follows established conventions (Gmail, Linear, GitHub).
- Global shortcuts use a `g` prefix for navigation to avoid conflicts with text input.
- Main content focus outlines are removed for visual cleanliness.
