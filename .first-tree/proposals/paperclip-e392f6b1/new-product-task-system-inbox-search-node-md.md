---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR includes a 'Speed up issue search' commit — the inbox search node covers broadening and fallback behavior but not performance optimization decisions.
---
### Search Performance Optimization

Issue search has been optimized for speed. The search path was tightened to reduce latency, with async result handling on the frontend to keep the UI responsive during search operations.
