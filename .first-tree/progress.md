# Context Tree Init

**Agent instructions:** Before starting work, analyze the full task list below and identify all information you need from the user. Ask the user for their code repositories or project directories so you can analyze the source yourself — derive project descriptions, domains, and members from the code instead of asking the user to describe them. Collect everything upfront using the **AskUserQuestion** tool with structured options — present selectable choices (with label and description) so the user can pick instead of typing free-form answers. You may batch up to 4 questions per AskUserQuestion call.

## Root Node
- [x] NODE.md has a placeholder title — replace with your organization name
- [x] NODE.md has placeholder owners — set owners to your GitHub username(s)
- [x] NODE.md has placeholder content — ask the user for their code repositories or project directories, then analyze the source to fill in the project description and domain structure

## Agent Instructions
- [x] Add your project-specific instructions below the framework markers in AGENTS.md
- [x] Add your project-specific instructions below the framework markers in CLAUDE.md

## Members
- [x] Add at least one member node under `members/`. Analyze the user's code repositories (git history, CODEOWNERS, README contributors) to suggest members, then confirm with the user

## Agent Integration
- [x] No agent configuration detected. Configure your agent to load tree context at session start. See `.agents/skills/first-tree/assets/framework/examples/` for supported agents. You can skip this and set it up later.

## CI / Validation
- [x] No validation workflow found — copy `validate.yml` from the bundled first-tree workflow templates to `.github/workflows/validate.yml`
- [x] No CODEOWNERS workflow found — copy `codeowners.yml` from the bundled first-tree workflow templates to `.github/workflows/codeowners.yml` to auto-generate CODEOWNERS from tree ownership on every PR.

## Populate Tree
- [x] Read `progress.md` (normally `.first-tree/progress.md` in a dedicated tree repo) as the source of truth for the onboarding checkpoint before you ask about deeper tree population. Tell the user what is already done and what remains, and split the report into at least two lanes: (1) setup / source-workspace integration progress and (2) tree-content baseline coverage progress. Highlight the remaining work categories, and prefer phrases like `baseline coverage` or `first pass` instead of claiming the tree is fully complete.
- [x] Ask the user whether they want to continue building the first-pass full tree now using the **AskUserQuestion** tool. Present two options: (1) **Yes — continue baseline tree expansion**: explain the expected scope first, including how many top-level domains you plan to cover, how many waves of parallel work you expect, and that you will finish with root-node reconciliation plus `first-tree verify`; (2) **No — keep the initial scaffold only**: skip deep population and finish init with the current top-level structure. If the user selects No, check off all remaining items in this section and move on.
- [x] If the user selected Yes: analyze the codebase (and any additional repositories the user provides) to identify logical sub-domains within each top-level domain. For each sub-domain, create a directory with a NODE.md containing proper frontmatter (title, owners) and a description of the sub-domain's purpose, boundaries, and key decisions. Create deeper sub-domains when a domain is large enough to warrant further decomposition.
- [x] Use parallel **sub-tasks / subagents** (for example, TaskCreate where available) in waves, not as unbounded fan-out. The default split is one sub-task per top-level domain so each domain can be populated concurrently. Keep the main agent responsible for the root `NODE.md`, cross-domain `soft_links`, overlap cleanup between domains, and the final `first-tree verify` pass. Each domain sub-task should: read the relevant source code, identify sub-domains, create NODE.md files, and establish soft_links between related domains.
- [x] Continue launching additional waves until every top-level domain has first-pass baseline coverage. After the domain waves finish, update the root NODE.md to list every top-level domain with a one-line description, reconcile any cross-domain soft_links, and ensure all NODE.md files pass `first-tree verify` — valid frontmatter, no placeholders, and soft_links that resolve correctly.

## Verification
After completing the tasks above, run `first-tree verify` to confirm:
- [x] `.first-tree/VERSION` exists
- [x] Root NODE.md has valid frontmatter (title, owners)
- [x] `AGENTS.md` has framework markers and `CLAUDE.md` mirrors the same workflow guidance
- [x] `first-tree verify` passes with no errors
- [x] At least one member node exists

---

**Important:** As you complete each task, check it off in `.first-tree/progress.md` by changing `- [ ]` to `- [x]`. Run `first-tree verify` when done — it will fail if any items remain unchecked.
