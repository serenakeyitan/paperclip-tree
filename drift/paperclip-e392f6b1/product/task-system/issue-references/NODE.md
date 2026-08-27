---
title: "Issue References"
owners: [bingran-you, serenakeyitan, cryppadotta]
---

Issue references are the canonical way the UI names and links to issues from free-form text and structured fields. The parsing and formatting rules live in `ui/src/lib/issue-reference.ts` (covered by `issue-reference.test.ts`) and are shared by the Inbox list, the issues list, and the issue detail view so that a reference means the same thing everywhere it appears.

Treat `issue-reference.ts` as the single entry point: components should not hand-roll regexes or string templates for issue IDs. This keeps display, linking, and comparison consistent as the reference format evolves (e.g. adding company or project prefixes) and makes it safe to change the surface format in one place.

When an issue reference resolves to an in-app route, it should go through `ui/src/lib/router.tsx` rather than a bare `<a>` so navigation stays within the SPA and preserves filter/search state. Pair any new reference shape with a test in `issue-reference.test.ts` before wiring it into components.
