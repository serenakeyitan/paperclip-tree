---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The node documents session key strategies but doesn't mention that session keys are prefixed with the configured agent ID to prevent collisions when multiple agents share the same gateway.
---
Under **Key Decisions**, add:

- **Agent-scoped session key prefix.** Session keys are prefixed with the configured agent ID before being sent to the gateway. This prevents session collisions when multiple Paperclip agents connect to the same OpenClaw gateway instance — without the prefix, two agents using the `issue` strategy on the same issue would share a session unintentionally.
