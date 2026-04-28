---
title: "Hire Approval Default"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/database/NODE.md, product/company-model/NODE.md, product/governance/NODE.md, product/governance/issue-approvals/NODE.md]
---

# Hire Approval Default

New companies default to **hire approvals off**. The CEO can hire agents directly without a board-approval gate unless the operator explicitly enables `requireBoardApprovalForNewAgents` in company settings.

## Key Decisions

### Default Off, Opt-In Gate

Migration `0071_default_hire_approval_off` flips the column default on `companies.require_board_approval_for_new_agents` from `true` to `false`. Schema, docs (`docs/start/core-concepts.md`, `docs/guides/board-operator/delegation.md`), and the company-portability service all align on the new default. Operators who want a hiring gate enable it deliberately; the previous always-gated behavior produced friction for routine team scaling.

### Portability Export Inverts Its Default Marker

`company-portability.ts` previously emitted `requireBoardApprovalForNewAgents: false` only when approvals were disabled (since `true` was the implicit default). With the flipped default, export now emits `true` only when approvals are explicitly required, and import treats a missing field as `false`. The `readCompanyApprovalDefault` helper returns `false` to match.

### Issue-Level Approvals Are Unaffected

This change only governs the *hiring* approval gate. Issue-level approvals (see `product/governance/issue-approvals`) continue to use the same approval infrastructure independent of the hire-default flag.

## Why It Matters

The hire approval gate is a high-friction governance control. Defaulting it off matches how most operators actually run their companies and reserves the gate for cases where the board explicitly wants to slow down agent expansion.
