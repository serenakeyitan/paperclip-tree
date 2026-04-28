---
title: "Agent Hiring Templates"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, product/agent-model/NODE.md, product/company-model/NODE.md]
---

# Agent Hiring Templates

Paperclip ships a library of reusable agent instruction templates used when hiring new employees into a company. Each template captures the role's recommended fields (`name`, `role`, `title`, `icon`, `capabilities`, `adapterType`) and a starting `AGENTS.md` body that materializes into the agent's instruction bundle (typically via `adapterConfig.promptTemplate` for local managed-bundle adapters).

## Key Decisions

### One File Per Role

Templates live as individual files under `skills/paperclip-create-agent/references/agents/` (e.g. `coder.md`, `qa.md`, `uxdesigner.md`) rather than as one monolithic document. The split keeps each template independently readable and editable, and lets new roles be added without churning the index. The index file `agent-instruction-templates.md` is the human-readable table of contents that links into the per-role files.

### Templates Are Starting Points, Not Contracts

When applying a template, the hiring agent copies it into the new agent's instruction bundle, replaces placeholders (`{{agentName}}`, `{{companyName}}`, `{{managerTitle}}`, `{{issuePrefix}}`), removes tools the target adapter cannot use, and adds role-specific skills only when actually installed. The Paperclip heartbeat requirement and task-comment requirement must be preserved across all templates.

### Current Roster

- **Coder** — software engineers (codex_local / claude_local / cursor) who implement, debug, and test code.
- **QA** — QA engineers (claude_local or browser-capable adapter) who reproduce defects, validate fixes, and capture evidence.
- **UX Designer** — principal product designers (claude_local / codex_local) who own UX strategy, interaction design, and design-system quality, applying named design lenses (Gestalt, Nielsen heuristics, Fitts/Hick, etc.) so reasoning is traceable in comments.

New role templates should follow the same structure: recommended role fields, an `AGENTS.md` block with placeholder variables, and explicit guidance on adapter compatibility.
