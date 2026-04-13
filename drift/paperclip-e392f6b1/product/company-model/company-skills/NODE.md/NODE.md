---
title: "Company Skills"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Company Skills

Skills are company-scoped capability definitions managed through the UI. They are distinct from adapter-level skill injection (which operates at the runtime layer).

## Key Decisions

### Company Scope

Skills are scoped to a company, not to individual agents or projects. This enables shared capability definitions across all agents within a company.

### Deletion Guards

An agent-usage dependency check prevents deletion of skills that are actively referenced by agent configurations. This prevents orphaned agent configs that reference non-existent skills.

### UI-Managed

Skills are created and managed through the Paperclip UI, giving human operators direct control over what capabilities are available to agents in their company.
