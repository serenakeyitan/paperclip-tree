---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR enriches the heartbeat-context response with attachment metadata, but the heartbeat orchestration node doesn't mention what data the heartbeat context includes.
---
### Heartbeat Context Enrichment

The heartbeat-context response includes attachment metadata alongside issue and thread data. This allows agents to be aware of files attached to issues during their heartbeat runs without making separate API calls. Context payloads are kept comprehensive so adapters have a complete picture in a single round-trip.
