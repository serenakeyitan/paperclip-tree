---
title: "Contributor Documentation"
owners: []
---

---
title: "Contributor Documentation"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/dev-tooling", "engineering/cli", "infrastructure/ci-cd"]
---

# Contributor Documentation

Onboarding guides, contribution workflows, and documentation conventions for the Paperclip project.

**Source:** `CONTRIBUTING.md`, `docs/`, README files across packages.

---

## Scope

This node captures decisions about how contributor documentation is structured, what it covers, and how it stays current. The actual content lives in the source repo — this node tracks the *strategy* and *conventions* around contributor docs.

## Key Decisions

- **CONTRIBUTING.md as the canonical entry point.** New contributors start here. It covers local setup, branch conventions, PR workflow, and testing expectations.
- **Package-level READMEs for domain context.** Each workspace package maintains its own README with setup, architecture notes, and gotchas specific to that package.
- **Docs refresh is a first-class chore.** Documentation updates are tracked as engineering tasks (not afterthoughts) and reviewed with the same rigor as code changes.

## Boundaries

This node covers documentation strategy and conventions. The actual documentation content lives in the source repository. Developer tooling and scripts are covered in `../dev-tooling`.
