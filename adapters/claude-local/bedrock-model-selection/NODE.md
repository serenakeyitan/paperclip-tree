---
title: "Bedrock Model Selection"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Bedrock Model Selection

When a Paperclip agent is configured to use Claude via **AWS Bedrock**, the adapter must forward the user's selected model to the Claude CLI. Prior to this change, the adapter did not pass the model selection through, causing Bedrock users to always get the default model regardless of their configuration.

## Key Decisions

### Pass Model Selection to CLI for Bedrock

When the agent's configuration specifies a model (e.g., a Bedrock model ARN or model ID), the adapter now includes the appropriate `--model` flag in the CLI invocation. This ensures Bedrock users can select specific Claude model versions through Paperclip's agent configuration, just as direct Anthropic API users can.

**Rationale:** Bedrock deployments often have specific model versions provisioned. Ignoring the user's model selection silently overrides their infrastructure choices and can cause unexpected behavior or cost differences.

### Model ID Format

Bedrock model IDs follow AWS ARN patterns (e.g., `anthropic.claude-3-5-sonnet-20241022-v2:0`) rather than Anthropic's native model names. The adapter passes these through without transformation — the Claude CLI handles the Bedrock-specific format internally.

## Boundaries

- Model validation is not performed by the adapter. Invalid model IDs will surface as CLI errors at runtime.
- This does not change behavior for non-Bedrock (direct Anthropic API) users.
