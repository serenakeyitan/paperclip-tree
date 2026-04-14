---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The Frontend UI node lists page areas but does not mention company-level skill management UI (listing, deletion with agent usage checks); this PR adds a new skill deletion flow that should be noted.
---
Add to the **Page Structure** section:

- **Skills** — company-level skill management: listing assigned skills, deletion with agent usage dependency checks (warns before removing a skill that agents depend on).

Add to **Key Decisions** or as a note under Page Structure:

- **Skill deletion requires agent usage check.** Before deleting a company skill, the UI queries which agents currently use it and presents a confirmation with the list of affected agents. This prevents accidental removal of skills that active agents depend on.
