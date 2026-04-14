---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The CLI node documents commands and stack but not dev boot prerequisites — the plugin SDK must be prepared before CLI dev boot starts, which is a cross-package build dependency worth capturing.
---
### Dev Boot Prerequisites

The CLI dev server (`tsx` via `pnpm dev`) depends on `@paperclipai/plugin-sdk` being built first. The plugin SDK must be prepared before CLI dev boot because the CLI imports plugin types and utilities at startup. This ordering is enforced in the dev script to avoid import failures when starting from a clean state.

See PR #3343 for the fix that added this preparation step.
