---
title: "Product"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/NODE.md", "adapters/NODE.md", "plugins/NODE.md"]
---

# Product

Product strategy, core concepts, and design decisions for Paperclip -- the control plane for autonomous AI companies.

## Identity

Paperclip is infrastructure, not the company itself. It is the operating layer that makes autonomous AI companies possible: org structure, task management, cost control, goal alignment, and human governance. The measure of success is whether Paperclip becomes the default foundation autonomous companies are built on, and whether those companies collectively become a serious economic force.

## Core Mental Model

One Paperclip instance runs multiple **Companies**. Each Company has a **goal**, an **org chart** of AI agents as employees, a **task hierarchy** where all work traces back to the company goal, **budgets** denominating cost in tokens and dollars, and a **Board** of human operators who retain control. Agents run externally and phone home -- Paperclip orchestrates, it does not execute.

## Key Product Decisions

1. **Control plane, not execution plane.** Paperclip never runs agent code directly. Agents run wherever they run (local processes, cloud services, third-party platforms) and report into the control plane via adapters.

2. **Company is the unit of organization.** Everything is company-scoped. Multiple human operators can share a deployment (with real identities, invite flows, and company-scoped membership), but the deployment is still scoped to one organization — not shared across unrelated orgs.

3. **Tasks are the communication channel.** No separate chat or messaging system. All agent communication flows through tasks and comments, keeping context attached to work and creating a natural audit trail.

4. **Board-level abstraction wins.** The default experience answers: what is the company doing, who is doing it, why does it matter, what did it cost, and what needs my approval.

5. **Unopinionated about agent implementation.** Any language, framework, or runtime. The adapter defines the agent's inner workings; Paperclip defines the control plane protocol.

6. **Surface problems, don't hide them.** No silent auto-recovery, no automatic reassignment. Good auditing and visibility let the right entity (human or agent) decide what to do.

7. **Progressive disclosure.** Top layer: human-readable summary. Middle layer: checklist/steps/artifacts. Bottom layer: raw logs/tool calls/transcript.

8. **Local-first, cloud-ready.** The mental model should not change between local solo use and shared/cloud deployment. Two runtime modes: `local_trusted` (no login friction, solo local use) and `authenticated` (session-based, supports multiple human operators with invite/membership flows and private/public hostname exposure).

9. **Thin core, rich edges.** Knowledge bases, external revenue tracking, rich chat, and special surfaces go into plugins -- not the core control plane.

## V1 Scope Summary

V1 demonstrates the full control-plane loop end-to-end: a human board creates a company, defines goals, hires agents into an org tree, agents receive and execute tasks via heartbeat invocations, costs are tracked with budget hard-stops, and the board can intervene anywhere. V1 also ships multi-operator access — multiple humans can share a deployment with real identities, invite flows, and company-scoped membership. Success means a small team of operators can run AI-native companies with clear visibility and control.

**Explicitly deferred from V1:** plugin framework/SDK, knowledge base, revenue/expense accounting beyond tokens, public marketplace (ClipHub), multi-board governance, automatic self-healing orchestration.

## Anti-Requirements

Paperclip is explicitly **not**: an agent runtime, a knowledge base, a SaaS, a general chat app, a complete Jira/GitHub replacement, or a delivery infrastructure manager. These boundaries are load-bearing and protect product focus.

## Sub-domains

- **[company-model/](company-model/NODE.md)** -- Company lifecycle, multi-company data model, portability/export.
- **[agent-model/](agent-model/NODE.md)** -- Agent lifecycle, adapter protocol, org structure, cross-team coordination.
- **[task-system/](task-system/NODE.md)** -- Task hierarchy, workflow states, atomic checkout, inter-agent communication.
- **[governance/](governance/NODE.md)** -- Board powers, approval gates, budget controls, cost tracking.
- **[routines/](routines/NODE.md)** -- Scheduled and event-triggered automation definitions, draft lifecycle, concurrency policies.
