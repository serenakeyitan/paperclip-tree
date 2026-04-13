---
title: "Security"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

Security model, threat boundaries, and security practices for the Paperclip platform.

## Security Boundaries

### Agent Execution Isolation

AI agents execute in isolated environments with constrained capabilities. The untrusted-review container drops ALL Linux capabilities, prevents privilege escalation, uses tmpfs for `/tmp`, and runs as a dedicated `reviewer` user. This is the hardest security boundary in the system.

### Multi-Company Data Isolation

All data is scoped by `companyId` at the query level. Isolation is enforced in application code — a deliberate trade-off for simpler infrastructure at the cost of discipline in every data access path.

### Log Redaction

Bearer tokens and other sensitive values are redacted from server log output to prevent credential leakage through logging infrastructure.

## Key Decisions

- **Defense in depth for agent execution.** Multiple isolation layers (worktree filesystem isolation, container capability dropping, process sandboxing) rather than relying on any single boundary.
- **Auth handled by agent CLIs.** Paperclip delegates credential management to each agent's native tooling rather than centralizing secrets.
- **Sensitive data never in logs.** Bearer tokens are stripped from log output at the server level.
