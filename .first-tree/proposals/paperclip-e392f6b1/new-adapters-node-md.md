---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The adapters node documents skill sync/injection but doesn't mention that skills can be deleted at the company level with an agent usage dependency check — this cross-cutting concern (adapter skill lifecycle affected by company-level skill deletion) should be noted.
---
Add a note under the skill-related section:

- **Company-level skill deletion with usage check.** When a skill is removed from a company, the system checks which agents (via their adapters) currently have that skill synced. The UI surfaces this dependency before confirming deletion. Adapters are not responsible for cleanup — the platform handles deregistration.
