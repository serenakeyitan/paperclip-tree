---
title: "Security"
owners: []
---

Security model, threat boundaries, and security practices for the Paperclip platform.

## Security Boundaries

### Agent Execution Isolation

AI agents execute in isolated environments with constrained capabilities. The untrusted-review container drops ALL Linux capabilities, prevents privilege escalation, uses tmpfs for `/tmp`, and runs as a dedicated `reviewer` user. This is the hardest security boundary in the system — changes require explicit security review.

### Multi-Company Data Isolation

All data is scoped by `companyId` at the query level. There is no row-level security or separate databases per company — isolation is enforced in application code. This is a deliberate trade-off: simpler infrastructure at the cost of requiring discipline in every data access path.

### Plugin Sandboxing

Plugins run in separate child processes communicating via JSON-RPC 2.0 over stdio. This prevents plugins from accessing server memory or crashing the host process. The `PluginRuntimeSandbox` enforces resource and capability limits.

### Credential Management

Agent credentials (API keys, auth tokens) are managed by each agent CLI's own credential store, not by Paperclip. Paperclip validates that agents are authenticated via environment tests but never stores or proxies credentials.

### Log Redaction

Bearer tokens and other sensitive values are redacted from server log output to prevent credential leakage through logging infrastructure.

## Key Decisions

- **Defense in depth for agent execution.** Multiple isolation layers (worktree filesystem isolation, container capability dropping, process sandboxing) rather than relying on any single boundary.
- **Auth handled by agent CLIs.** Paperclip delegates credential management to each agent's native tooling rather than centralizing secrets, reducing the attack surface of the Paperclip server itself.
- **Sensitive data never in logs.** Bearer tokens are stripped from log output at the server level, ensuring credentials cannot leak through log aggregation or monitoring systems.
