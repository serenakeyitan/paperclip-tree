---
title: "No LLM Calls In Default CI"
owners: [cryppadotta, serenakeyitan]
---

# No LLM Calls In Default CI

Paperclip integrates with agent runtimes and model providers, but its default
CI path should remain reliable even when providers are unavailable or costly.

## Decision

Keep standard CI and release verification free of required live LLM calls.
Model-dependent checks belong in separate manual or specialized evaluation
lanes, while default e2e and verify jobs use deterministic shortcuts and skip
flags when needed.

## Why

This keeps CI fast, reproducible, and affordable while still preserving room
for deeper runtime evaluation in dedicated workflows.

## Implications

- A normal pull request should not require provider credentials or incur model
  spend just to prove the codebase is healthy.
- E2E flows can run in trusted local mode with model-dependent assertions
  disabled when necessary.
- Prompt and runtime quality evaluation still matters, but it should be staged
  separately from core correctness and release safety checks.
- Failures in model-provider integrations should be investigated through the
  eval lane or targeted smoke runs, not by making mainline CI flaky.

## Related Domains

- [infrastructure ci/cd](../ci-cd/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
- [adapters](../../adapters/NODE.md)
