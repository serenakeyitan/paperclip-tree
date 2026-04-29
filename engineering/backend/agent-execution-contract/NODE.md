---
title: "Agent Execution Contract"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, adapters/claude-local/NODE.md, adapters/codex-local/NODE.md, adapters/cursor-local/NODE.md, adapters/gemini-local/NODE.md, adapters/openclaw-gateway/NODE.md, adapters/opencode-local/NODE.md, adapters/pi-local/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Agent Execution Contract

Paperclip ships a shared execution contract that the in-repo local agent adapters inject into their default prompt template. The contract is exported as `DEFAULT_PAPERCLIP_AGENT_PROMPT_TEMPLATE` from `@paperclipai/adapter-utils/server-utils` and is consumed by the claude-local, codex-local, cursor-local, gemini-local, opencode-local, and pi-local adapters as the fallback when an agent config does not override `promptTemplate`. The openclaw-gateway adapter inlines the same rules into its wake-text builder. The hermes-local adapter is documented in `docs/agents-runtime.md` but does not import the shared template — it predates this consolidation and is not currently covered by this contract.

## Key Decisions

### Single Source of Truth for the Contract

The contract lives in one constant in `packages/adapter-utils/src/server-utils.ts` rather than being copy-pasted into each adapter. Adapters import the constant and pass it as the default to `asString(config.promptTemplate, DEFAULT_PAPERCLIP_AGENT_PROMPT_TEMPLATE)`. This keeps every local runtime aligned on the same execution rules without per-adapter drift.

### Contract Rules

The contract tells the agent to: start actionable work in the same heartbeat (no planning-only exits unless planning was requested), leave durable progress in comments, documents, or work products with a clear next action, use child issues for parallel or long delegated work instead of polling agents/sessions/processes, mark blocked issues with the unblock owner and action, and respect budget, pause/cancel, approval gates, and company boundaries.

### Documentation and Onboarding Alignment

The same rules are mirrored in the heartbeat protocol guide, the agents-runtime spec, the promptfoo heartbeat-system eval prompt, the company-creator skill (so generated agents inherit the contract), the create-agent-adapter skill (so new adapters use the shared default), and the openclaw-gateway wake text. New adapters MUST use `DEFAULT_PAPERCLIP_AGENT_PROMPT_TEMPLATE` as their default prompt rather than reintroducing the legacy one-line default.

## Relationship to Run Liveness

The contract pairs with run liveness continuations: liveness states like `plan_only` and `empty_response` exist because agents sometimes violate the "act in this heartbeat" rule, and continuation wakes re-prompt them with this contract. Workspace provisioning alone does not count as durable progress.
