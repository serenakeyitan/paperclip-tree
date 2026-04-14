---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The CLI node documents the stack and commands but not dev boot dependencies — this PR reveals that the plugin SDK must be prepared before the CLI dev server can start, a build-ordering constraint not currently captured.
---
### Plugin SDK Dev Dependency

The CLI's dev mode (`tsx`-based execution) depends on the plugin SDK being pre-built. Before the CLI dev server can boot, `@paperclipai/plugin-sdk` must be prepared (compiled/bundled) so that the CLI can resolve plugin-related imports at startup. This is enforced in the dev boot sequence — the CLI's dev script triggers an SDK prepare step before launching.

**Why this matters:** Without the SDK pre-built, the CLI fails on import resolution during dev boot because `tsx` does not resolve un-built workspace dependencies the same way the production `esbuild` bundle does. This is a dev-time constraint only; the production CLI bundle includes the SDK output directly.
