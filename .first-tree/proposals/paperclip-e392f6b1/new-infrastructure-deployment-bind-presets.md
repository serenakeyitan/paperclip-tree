---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: PR #3303 introduces bind presets for deployment setup — a new concept for predefined network binding configurations inspired by OpenClaw's networking/discovery/binding patterns — which has no coverage in the existing deployment node or anywhere in the tree.
---
## Overview

Bind presets are predefined network binding configurations for Paperclip deployment. They simplify deployment setup by providing named presets that configure how the server binds to network interfaces, ports, and addresses.

This concept was introduced after reviewing OpenClaw's documentation on networking, discovery, and binding (PAP-438), applying learnings from OpenClaw's approach to Paperclip's deployment model.

## Relationship to Deployment Modes

Bind presets complement the existing `PAPERCLIP_DEPLOYMENT_MODE` (`local_trusted` / `authenticated`) by handling the network layer configuration. While deployment modes control auth and database behavior, bind presets control how the server exposes itself on the network.

## Key Decisions

- **Preset-based configuration** rather than requiring users to manually specify host, port, and exposure settings individually. Presets bundle these into named configurations that match common deployment scenarios.
- **Inspired by OpenClaw's networking model** — the approach of declarative binding configuration with discovery semantics was adapted from OpenClaw's gateway protocol documentation.

## Open Questions

- Exact preset names and their configurations should be documented once the implementation stabilizes.
- Relationship between bind presets and Docker compose configurations needs clarification — do compose files select a preset, or do they override preset values?
