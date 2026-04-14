---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The 'Human-Readable Identifiers' section documents the format ({TEAM_KEY}-{NUMBER}) but says nothing about collision prevention under concurrent issue creation — this PR adds that server-side guarantee.
---
### Suggested addition to 'Human-Readable Identifiers' section:

Identifier assignment is collision-safe under concurrent creation. The server prevents duplicate `{TEAM_KEY}-{NUMBER}` identifiers even when multiple agents or users create issues in the same project simultaneously. This was added after a production bug where concurrent issue creation could produce duplicate identifiers (see #2705).

**Rationale:** Agents frequently create issues in bursts (e.g., decomposing a milestone into sub-issues). Without collision prevention, parallel creation could silently produce duplicate identifiers, breaking the assumption that identifiers are unique and stable references.
