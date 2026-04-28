---
title: "Create-Agent Skill"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/codex-local/NODE.md, engineering/backend/NODE.md, product/agent-model/NODE.md, product/governance/NODE.md, product/governance/issue-approvals/NODE.md]
---

The `paperclip-create-agent` skill is the end-to-end hiring workflow for agents inside a Paperclip company. It is bundled as a runtime skill (`paperclipai/paperclip/paperclip-create-agent`) alongside the core `paperclip` skill and is auto-injected into Codex local adapter sessions, so any agent with hire permission can drive a hire from a single skill.

The skill enforces a fixed workflow: confirm identity, fetch `/llms/agent-configuration.txt` and the adapter-specific doc, compare existing agent configurations, then choose an instruction source. Instruction sources follow a decision flow — exact role template, adjacent template adapted deliberately, or the generic baseline-role-guide fallback — and the chosen path must be stated in the hire comment for board audit.

Role templates live under `skills/paperclip-create-agent/references/agents/` and currently cover Coder, QA, UX Designer, and SecurityEngineer. Templates are split by lens density: operational roles (Coder, QA) stay short, while lens-heavy expert roles (UX Designer, SecurityEngineer) carry named domain lenses, a visual-truth or threat-model gate, and explicit collaboration handoffs. All execution-heavy templates embed the Paperclip execution contract verbatim (start actionable work in the same heartbeat, leave durable next actions, use child issues for parallel work, respect budget and approval gates).

Governance is part of the skill, not bolted on. Hire requests preserve `icon`, `desiredSkills`, and `sourceIssueId`/`sourceIssueIds` through to the approval payload and configuration snapshot, so approvers see the full requested configuration. The `draft-review-checklist.md` reference must be walked before submitting any `agent-hires` request, and the skill explicitly forbids granting broad admin skills, enabling timer heartbeats by default, or routing private security advisories through normal issue threads.
