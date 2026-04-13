---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: A keyboard shortcut system was introduced including a cheatsheet dialog (? keypress), navigation shortcuts (g i for inbox), issue-level shortcuts (comment composer, archive), and j/k traversal of nested issues. This is a new interaction pattern not mentioned in the frontend node.
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
