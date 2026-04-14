---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR adds a configurable Parent Issue column with show/hide support and narrows column widths — the existing inbox list node only covers filter-based refetch behavior, not column configuration.
---
### Configurable Columns

The inbox list supports show/hide toggling for optional columns (e.g., Parent Issue). Column visibility is user-controlled. New columns like Parent Issue and time-ago are sized narrowly to preserve horizontal space for the primary issue title and status columns.
