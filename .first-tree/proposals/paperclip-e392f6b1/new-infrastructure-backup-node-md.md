---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: Gzip-compressed backups with tiered daily/weekly/monthly retention and Instance Settings UI controls were added — a new infrastructure capability.
---
## Overview

Paperclip supports automated database backups with compression and configurable retention policies, managed through the Instance Settings UI.

## Key Decisions

- **Gzip compression** — Backups are gzip-compressed to reduce storage costs. Orphaned `.sql` files from failed compressions are cleaned up automatically.
- **Tiered retention** — Retention follows a daily/weekly/monthly tier model. Administrators configure how many backups to keep at each tier via the Instance Settings UI.
- **UI-driven configuration** — Retention settings are exposed in Instance Settings rather than requiring env vars or config file edits, making them accessible to non-technical operators.
- **Failure resilience** — Compression failures are handled gracefully with cleanup of partial artifacts and accurate startup logs.
