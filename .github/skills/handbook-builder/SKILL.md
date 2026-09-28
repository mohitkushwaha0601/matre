
---
name: handbook-builder
description: 'Scans the visual design and structure of the existing handbooks in materials/*.html, then writes a new self-contained HTML handbook that matches the same design philosophy (hero/flagstrip, sticky TOC, part dividers, edge callouts, panels, timeline, qbank, cards, tag system) and registers it in index.html and README.md. Use when asked to create a new handbook/knowledge document/briefing for this repo, to turn a topic from further.md or prompt.md into a document, to add a document to materials/, or to keep index.html/README.md in sync with materials/.'
---

# Handbook Builder

Builds new entries for this repo's "Preparation Handbooks" collection: single-file HTML documents on defence/geopolitics/current-affairs topics, styled and structured identically to the existing files in `materials/`.

## When to use

- "Create a new handbook on X"
- "Turn prompt N from further.md / prompt.md into a document"
- "Add a document to materials/ and update index.html"
- Any request to add/regenerate a handbook while "matching the same design/style/philosophy" as the others

## Procedure

### 1. Re-scan the design system before writing anything

Read the two **highest-numbered** files in `materials/` (currently `7.*` and `8.*`) plus one older one (e.g. `1.*`). Do not rely purely on memory or on the notes in this skill — confirm class names, CSS variables and section order still match by grepping for `<style>`, `class="`, and `<section id=` in those files. The [design system reference](./references/design-system.md) documents the shared component catalogue as of the last scan; treat it as a starting hypothesis to verify, not ground truth.

Look specifically for:
- The CSS custom properties block in `:root` (light) and the two duplicated dark-mode blocks (`@media (prefers-color-scheme: dark)` and `:root[data-theme="dark"]`)
- The component class names in use: `.hero`, `.flagstrip`/`.f1`-`.f8`, `.hero-inner`, `.kicker`/`.motto`+`.motto-en`, `.stamp`, `.shell`, `.toc-mobile` + `nav.toc` with `.part` group headers, `.part-head`, `section > h2` + `.sec-rule`, `.lede`, `.edge` (+ `.gold`/`.blue`/`.green`), `.note`, `.panel` (+ `-top` colour variants), `.timeline` + `.yr`, `.qbank`, `.grid.two`/`.grid.three` + `.card`, `.tablewrap` + `table`, `.tag` (+ `.fact`/`.interp`/`.disputed`/`.verify`), `dl.rungs`, `@media print`
- Whether the newest files introduce any pattern not yet in the reference doc — if so, prefer what you find in the files.

### 2. Establish the topic brief

If the user names a topic from `further.md` (the "10 additional deep-research prompts") or `prompt.md` (the revised HTML-only versions of the same list), open that file and use its bullet list of coverage points and closing deliverables as a **menu to select from, not a checklist to exhaust**. Group related bullets into a small number of dense sections rather than one section per bullet — do not re-invent the scope, but do compress it. Otherwise, ask the user for the topic's scope, or infer a reasonable one analogous to a short handbook (history → key actors/turning points → strategic dimensions → competing perspectives → current situation → SSB analysis).

**Target length: 18–22 sections total, sized like `materials/1.indian-navy-knowledge-handbook.html` (19 sections, a 2–3 hour read) — not like the 50–60 section handbooks.** This is a hard budget: if the source coverage list would naturally produce more sections, merge topics until the count fits, rather than writing every bullet as its own section. Keep prose tight — aim for roughly 150–250 words of running text per section (tables, lists and callouts are additional but should stay compact too), so total reading time at a normal reading pace lands at 2–3 hours.

Cross-check against `README.md`'s table and `index.html`'s card grid so you don't duplicate an existing handbook.

### 3. Pick the file name and number

Next sequential number = (count of files in `materials/`) + 1. Filename pattern: `materials/N.<topic-slug>-knowledge-handbook.html` (lowercase, hyphenated), matching files 1–7; file 8 shows a shorter slug is acceptable for a briefing-style doc. Ask the user only if the naming choice is ambiguous.

### 4. Choose a distinct palette, same token names

Every existing file reuses the same variable *names* (`--ink`, `--ink-soft`, `--paper`, `--surface`, `--surface-2`, `--rule`, `--rule-strong`, `--red`, `--blue`, `--hero-bg`, `--hero-ink`, `--hero-dim`, and usually `--gold`/`--green`) but assigns new hex values per document, thematically linked to the topic (e.g. a country's flag colours, an organisation's brand colours). Always provide all three blocks: the light `:root` values, the `@media (prefers-color-scheme: dark)` override, and the identical `:root[data-theme="dark"]` override (for an explicit theme toggle if one exists in the file). Build `.flagstrip`'s `.f1`–`.f8` spans from real flag colours of the countries/entities involved.

### 5. Generate the document

Start from [the template skeleton](./assets/handbook-template.html) — copy it, then fill in every placeholder in `{{DOUBLE_BRACES}}`. Required structure, top to bottom:

1. `<head>`: title, meta description (one sentence summarising scope), Google Fonts preconnect + IBM Plex Sans/Zilla Slab import, full `<style>` block
2. `.hero`: `.flagstrip`, `.kicker` (or `.motto`+`.motto-en` epigraph if the doc wants one, matching files 7–8's pattern), `<h1>`, `.hero-sub`, `.stamp` with 4–6 key stats
3. `.shell` containing:
   - `<details class="toc-mobile" open>` → `<nav class="toc">` with `<li class="part">` group headers and one `<li><a href="#id">` per section — every href must have a matching `<section id>` later in the document
   - `<main>` with all `<section>` elements, each `<h2>` + `<hr class="sec-rule">`, grouped under `.part-head` dividers (`<p>PART LABEL</p><h2>Part title</h2>`) — with the **18–22 section budget** spread over roughly 3–4 parts (e.g. background/history, core subject-matter, strategic analysis, SSB toolkit), not 6–8 parts
4. Required closing sections, in this order, mirroring every existing handbook's "Knowledge toolkit" part, kept intentionally short: one combined section for competing perspectives/controversies/lesser-known facts, one for scenarios and India's strategic options (presented neutrally), a `qbank` of **12–15** practice questions, **6–8** debate motions, a master chronology (`.timeline`), a glossary, and a concise rapid-revision sheet. Skip a separate lecturette-topics section and a multi-brief "framework applied" section — fold one short worked example into the answer-framework section instead of four.
5. `<footer>` matching the existing disclaimer + GitHub link pattern

Use `.edge` for single high-value callouts, `.panel` (optionally with `dl.rungs` inside for a define→locate→establish→contest→analyse→test→position→concede→close style worked example) for structured analysis blocks, `.grid.two`/`.grid.three` + `.card` for comparative or biographical items, `.tablewrap`+`table` for structured data, `.tag` with `fact`/`interp`/`disputed`/`verify` on any contested figures — this is the established way this repo distinguishes fact from interpretation on sensitive geopolitical topics.

Keep everything in one self-contained `.html` file: inline `<style>`, no external JS frameworks, no build step, only the two Google Fonts `<link>`s as external dependencies — exactly like every file currently in `materials/`.

### 6. Validate

- Run [get_errors](#tool:get_errors) style check (or otherwise visually diff) — confirm every TOC `href="#x"` has a matching `id="x"`, tags are balanced, and there's exactly one `<h1>`.
- Skim for neutral framing on contested political/military claims (use the `tag` system rather than asserting disputed numbers as fact).

### 7. Update `index.html`

- Add one new `<a class="handbook">` card inside `.handbooks`, following the existing pattern exactly: `<span class="no">` with the new zero-padded number, `<h3>` title, one descriptive `<p>`, `<span class="cta">Read handbook →</span>`, `href="materials/<new file>"`.
- Bump the handbook count in `<p class="stamp"><span><b>N</b> handbooks</span>...` and update `Last Updated <b>DD Mon YYYY</b>` to today's date.

### 8. Update `README.md`

- Add a row to the contents table: `| [emoji Title](materials/<file>) | HTML | <section count> | <estimated study time> |`, in the same style as existing rows — the study time should read **2–3 hrs** given the 18–22 section budget.
- Update the `**Total:** N handbooks` line and the `**Updated:**` footer line.

### 9. Report

Tell the user the new file path, the section count, and summarise the `index.html`/`README.md` edits made.

## Constraints

- Never change the *meaning* of shared class names across documents (e.g. don't repurpose `.edge` for something other than a callout) even though colours/content differ per file — that consistency of semantics, not identical colours, is what "same design philosophy" means in this repo.
- Don't introduce a CSS framework, JS bundler, or external stylesheet — every handbook is a single portable HTML file.
- Preserve the established neutral, both-sides tone on India-related strategic/political topics, and always close with SSB-oriented material (questions, debate topics, scenarios, revision sheet), per `further.md`/`prompt.md`.
