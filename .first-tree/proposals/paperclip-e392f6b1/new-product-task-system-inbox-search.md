---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR includes a 'Speed up issue search' commit — the existing inbox search node covers matching behavior and fallback but not performance optimization decisions.
---
### Search Performance

Issue search has been optimized for speed, reducing latency on search queries. Performance is a first-class concern because agents execute rapid sequential searches during triage workflows.
