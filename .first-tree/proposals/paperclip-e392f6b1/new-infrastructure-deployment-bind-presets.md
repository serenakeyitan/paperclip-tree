---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3303 introduces bind presets — predefined network binding configurations for deployment — which are not covered anywhere in the existing deployment node or tree.
---
# Bind Presets

Predefined network binding configurations that simplify Paperclip deployment setup. Rather than requiring users to manually specify host, port, and network exposure settings individually, bind presets bundle these into named configurations that match common deployment scenarios.

This concept was introduced after reviewing OpenClaw's documentation on networking, discovery, and binding (PAP-438), adapting their declarative binding configuration approach to Paperclip's deployment model.

---

## Relationship to Deployment Modes

Bind presets complement the existing `PAPERCLIP_DEPLOYMENT_MODE` (`local_trusted` / `authenticated`) by handling the **network layer** configuration. Deployment modes control auth and database behavior; bind presets control how the server exposes itself on the network (interface, port, address).

---

## Key Decisions

- **Preset-based configuration** over individual env vars for host/port/exposure. Presets reduce deployment friction and prevent misconfiguration by offering tested bundles.
- **Inspired by OpenClaw's networking model** — the declarative binding-with-discovery approach was adapted from OpenClaw's gateway protocol documentation.

---

## Open Questions

- Exact preset names and their configurations should be documented once the implementation stabilizes.
- Relationship between bind presets and Docker compose files — do compose files select a preset, or override preset values?
