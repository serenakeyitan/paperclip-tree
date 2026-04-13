---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: First-class issue blockers with dependency wakeups and blocker/review activity tracking were added — extending the task system with dependency management not currently documented.
---
## Overview

Issues can declare blocking relationships — an issue can block or be blocked by other issues. When a blocking issue is resolved, dependent issues are automatically woken up, enabling hands-off dependency management.

## Key Decisions

- **First-class blocker model** — Blockers are a dedicated concept, not just a label or tag. They appear in issue properties and are tracked via activity events.
- **Dependency wakeups** — When a blocking issue transitions to a resolved state, the system automatically wakes blocked issues. This prevents agents from polling or manually checking dependency status.
- **Activity event tracking** — Blocker creation, removal, and resolution generate activity events, providing an audit trail for dependency changes.
- **UI integration** — Blocking and sub-issue relationships are shown in the issue properties panel. Empty states (no blockers) render cleanly without placeholder text.
