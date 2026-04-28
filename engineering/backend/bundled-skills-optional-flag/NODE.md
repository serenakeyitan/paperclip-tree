---
title: "Bundled Skill Optional Flag"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [adapters/NODE.md, engineering/backend/NODE.md, engineering/cli/NODE.md]
---

Bundled Paperclip skills shipped under `skills/` are loaded by `listPaperclipSkillEntries` in `packages/adapter-utils/src/server-utils.ts` and made available to local adapters. By default every bundled skill is treated as **required** — it is always installed for local adapters with a `requiredReason` of "Bundled Paperclip skills are always available for local adapters."

## Opting Out via SKILL.md Frontmatter

A bundled skill can mark itself optional by setting `required: false` in its SKILL.md YAML frontmatter. The loader parses the frontmatter block (delimited by `---` lines) and looks for a top-level `required: false` key. When found, the entry is emitted with `required: false` and `requiredReason: null`, leaving installation to user choice rather than forcing it.

If the SKILL.md is missing, has no frontmatter, has malformed frontmatter, or omits the `required` key, the skill defaults to required. This keeps the existing behavior of all previously-shipped skills unchanged and makes opt-in to optional-mode explicit per skill.

## First Consumer: `paperclip-dev`

The `paperclip-dev` skill is the first bundled skill to use this flag. It documents day-to-day workflows for developing and operating a local Paperclip instance (starting/stopping servers, pulling from master, builds, tests, worktree management, database backups, diagnostics) and points to `doc/DEVELOPING.md` as the canonical CLI reference. Because it is only useful to people hacking on the Paperclip codebase itself, it ships as `required: false` so end-user agents do not get it installed automatically.
