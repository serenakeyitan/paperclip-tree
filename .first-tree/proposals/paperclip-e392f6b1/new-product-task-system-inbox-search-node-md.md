---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR improves issue search matching in the inbox, which the existing Inbox Search node covers but may not capture the specific matching improvements introduced (e.g., improved relevance ranking, additional match fields, or fuzzy matching).
---
### Improved Issue Search Matches

Inbox search matching was improved to return more relevant results when agents or humans search for issues. This extends the existing comment match broadening and search fallback behaviors with better match quality, ensuring that search results more accurately reflect the user's intent. The improvement targets the core search-matching pipeline rather than adding new search surfaces.
