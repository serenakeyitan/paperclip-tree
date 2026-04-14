---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR is specifically a security release (v2026.410.0) with release notes — there is no node covering release management, versioning strategy, or security release processes.
---
# Release Management

Versioning, release cadence, and security release conventions for Paperclip.

## Key Decisions

### Calendar-Based Versioning

Paperclip uses calendar-based version numbers (e.g., `v2026.410.0`) rather than semver. The format encodes the year and approximate date, making it easy to identify when a release was cut.

### Security Releases

Security fixes are shipped as dedicated releases with explicit release notes. Security release notes are committed to the repository to maintain an audit trail of what was patched and why.

### Release Notes in Repo

Release notes are checked into the repository rather than maintained only in GitHub Releases. This ensures they are versioned alongside the code and accessible to agents and CI without GitHub API calls.
