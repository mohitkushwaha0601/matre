---
description: "Create a new HTML handbook in materials/ that matches the existing design system, then register it in index.html and README.md."
name: "Createhandbook"
argument-hint: "Topic name, or a prompt number from further.md / prompt.md"
agent: "agent"
tools: ["codebase", "search", "editFiles", "problems"]
---

Create a new handbook for this repository using the [handbook-builder skill](../skills/handbook-builder/SKILL.md).

Topic: ${input:topic:Topic name, or a number/title from further.md's "10 additional deep-research prompts" or prompt.md's HTML-only versions}

Follow the skill's procedure exactly:

1. Re-scan the design system by reading the two highest-numbered files in [materials](../../materials) plus one older file, confirming against [the design system reference](../skills/handbook-builder/references/design-system.md).
2. If the topic matches an entry in [further.md](../../further.md) or [prompt.md](../../prompt.md), use that entry's coverage bullet list and closing deliverables as a menu to select from and compress — not a checklist to exhaust one-bullet-per-section. Otherwise confirm a reasonable scope with me before writing content. **Target 18–22 sections total (a 2–3 hour read, sized like `materials/1.indian-navy-knowledge-handbook.html`), not the 50+ section length of the largest existing handbooks.** Keep prose tight (roughly 150–250 words of running text per section) and trim the closing "knowledge toolkit" material to a combined perspectives/controversies section, a scenarios+options section, a 12–15 question qbank, 6–8 debate motions, a chronology, a glossary, and one rapid-revision sheet — no separate lecturette section, and only one short worked example instead of several.
3. Pick the next sequential file number/slug in `materials/`, and a distinct-but-consistent colour palette per the skill's rules.
4. Generate the full self-contained HTML document from [the template skeleton](../skills/handbook-builder/assets/handbook-template.html), including the fixed `.home-btn` link back to `../index.html` right after `<body>` (copied verbatim from the template) so the reader can always get back to the homepage.
5. Validate the output (balanced tags, every TOC anchor resolves to a section id, no lint/problems, `.home-btn` present and pointing to `../index.html`).
6. Update [index.html](../../index.html) (new card, handbook count, last-updated date) and [README.md](../../README.md) (new table row, total count, updated date).
7. Report the new file path and a summary of what changed in `index.html`/`README.md`.
