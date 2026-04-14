---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: Multiple CI fixes in the PR address manifest-only changes without lockfile updates and base SHA fetching in PR jobs — edge cases the CI/CD node doesn't document.
---
### Manifest-Only PR Support

CI handles PRs that change only workspace manifests (package.json) without corresponding lockfile changes. The pipeline fetches the base SHA in PR jobs and can restore the lockfile to the PR base when needed, avoiding false failures from lockfile-related policy checks on MCP or manifest-only changes.
