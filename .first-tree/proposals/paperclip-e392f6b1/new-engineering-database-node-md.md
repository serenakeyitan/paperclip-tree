---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The database node mentions 'Backup/restore built in' as a one-liner but now needs to reference the new backups infrastructure node for compression and retention details.
---
Update the Key Decisions bullet on backup/restore to cross-reference the new infrastructure/backups node:

- **Backup/restore built in.** The DB package includes `pg_dump`/`pg_restore` wrappers for data portability. Backup compression and retention policies are managed at the infrastructure layer — see [infrastructure/backups](../../infrastructure/backups/NODE.md).
