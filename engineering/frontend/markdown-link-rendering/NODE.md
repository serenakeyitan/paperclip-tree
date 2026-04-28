---
title: "Markdown Link Icon Wrapping"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/NODE.md]
---

`MarkdownBody` decorates rendered markdown links with contextual icons: a GitHub icon prefixed to GitHub links (markdown links and autolinks) and an external-link icon suffixed to other external HTTP links. Internal links get no icon.

To prevent the icon from being orphaned on a separate line from the link text when the URL wraps, the icon and its adjacent character (first character for the leading GitHub icon, last character for the trailing external-link icon) are wrapped in a `<span style="white-space:nowrap">`. Single-character link text wraps the icon and the character together; multi-character text splits into a nowrap-prefix span, the middle text, and a nowrap-suffix span so the middle is free to wrap normally.

This nowrap-splitting behavior is intentionally limited to plain string children. React-markdown can pass arrays or styled elements as link children (bold, code, etc.), and in that case the icons are rendered inline without the nowrap wrapper — splitting structured children is out of scope.

External-link detection reuses `isExternalHttpUrl`; internal app links (e.g., `/company/settings`) skip the indicator entirely. Tests in `MarkdownBody.test.tsx` lock the nowrap-span shape for both GitHub and external link cases.
