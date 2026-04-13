---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3303 introduces bind presets — predefined network binding configurations for deployment — which are not covered anywhere in the existing deployment node or tree.
---
# Bind Presets

Predefined network binding configurations that simplify Paperclip deployment setup. Instead of manually specifying host, port, and exposure settings individually, operators select a named preset that bundles these into a single configuration matching common deployment scenarios.

**Source:** server configuration and Docker compose files

---

## Relationship to Deployment Modes

Bind presets complement the existing `PAPERCLIP_DEPLOYMENT_MODE` (`local_trusted` / `authenticated`). Deployment modes control auth and database behavior; bind presets control how the server exposes itself on the network (interface, port, address). The two are orthogonal — any bind preset can be combined with any deployment mode.

## Key Decisions

### Preset-based configuration over manual knobs

Inspired by OpenClaw's networking and discovery model (PAP-438), bind presets use declarative named configurations rather than requiring users to understand and set multiple low-level network variables. This reduces misconfiguration risk, especially for first-time deployers.

### OpenClaw influence

The concept of declarative binding with discovery semantics was adapted from reviewing OpenClaw's gateway protocol documentation on networking, discovery, and binding patterns.

## Open Questions

- Exact preset names and their network configurations should be documented as the implementation stabilizes.
- Whether Docker compose files select a preset or override preset values needs clarification.
