---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The existing authorization decision record describes the pattern but doesn't mention that import, approvals, activity, and heartbeat routes specifically require company-scoped authorization enforcement — this PR closed that gap in the code, and the tree should note which route groups carry scoping requirements.
---
Add to the **Implications** section:

- **Route-level scoping is mandatory for all company-scoped routes.** Import, approvals, activity, and heartbeat routes must enforce company-scoped authorization checks, not rely on implicit access. Any new route group that touches company-scoped data should include explicit actor-based authorization assertions from the start.
