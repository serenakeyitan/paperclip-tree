---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR's main feature — enabling agent re-checkout of in_review tasks when comment feedback arrives — extends the auto-checkout behavior to cover a new trigger (comment feedback on in_review issues), which the existing Auto-Checkout on Wake node likely doesn't cover.
---
## Re-Checkout on Comment Feedback

When a reviewer leaves comment feedback on an issue that is in `in_review` status, the assigned agent can automatically re-checkout the issue and resume work. This extends the auto-checkout mechanism beyond wake-based triggers to include review feedback as a checkout signal.

**Rationale:** Agents frequently receive iterative review feedback. Without automatic re-checkout, an agent whose task is marked `in_review` would remain idle until manually reassigned, creating unnecessary human intervention in the review loop.
