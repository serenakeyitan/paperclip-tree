---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR treats Pi quota exhaustion as a failed run — the Pi adapter node says 'No quota reporting' in boundaries, but now quota exhaustion is detected and handled.
---
### Quota Exhaustion Handling

When Pi reports quota exhaustion, the adapter treats it as a failed run rather than silently retrying or hanging. This surfaces the failure to the heartbeat orchestration layer so the run is marked as failed and the board is notified.

**Update to Boundaries:** The adapter now detects quota exhaustion, though it still does not report quota levels proactively. The 'No quota reporting' boundary should be narrowed to 'No proactive quota reporting — quota exhaustion is detected reactively and surfaced as a run failure.'
