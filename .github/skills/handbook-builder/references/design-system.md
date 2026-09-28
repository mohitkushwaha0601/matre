# Handbook design system (as last scanned from `materials/*.html`)

Snapshot of the shared conventions across `materials/1.*` … `8.*`. Re-verify against
the actual files before generating a new document (see SKILL.md step 1) — each new
handbook has occasionally introduced a small variant (e.g. file 8 adds `.officelist`,
file 7 adds `dl.rungs`/`.ladder`/`.dbank`).

## Page skeleton (top to bottom)

```
<!DOCTYPE html>
<html lang="en">
<head>
  meta charset, viewport
  <title>Topic — A Deep Knowledge Handbook</title>
  <meta name="description" content="one-sentence scope summary">
  Google Fonts preconnect x2 + stylesheet link (IBM Plex Sans + Zilla Slab)
  <style> ... all CSS inline ... </style>
</head>
<body>
  <a class="home-btn" href="../index.html" aria-label="Back to Handbooks homepage">Home</a>

  <header class="hero">
    <div class="flagstrip" aria-hidden="true"> <span class="f1">...<span class="f8"> </div>
    <div class="hero-inner">
      [optional <p class="motto">quote</p><p class="motto-en">translation/context</p>]
      <p class="kicker">...</p>  <!-- simpler docs use kicker instead of motto -->
      <h1>Title</h1>
      <p class="hero-sub">one paragraph describing the handbook</p>
      <p class="stamp"><span>Stat <b>value</b></span> ... </p>
    </div>
  </header>

  <div class="shell">
    <details class="toc-mobile" open>
      <summary>Contents</summary>
      <nav class="toc" aria-label="Contents">
        <h2>Contents</h2>
        <ol>
          <li class="part">Part label</li>
          <li><a href="#id">Section title</a></li>
          ...
        </ol>
      </nav>
    </details>

    <main>
      <section id="...">
        <h2>...</h2><hr class="sec-rule">
        ... content ...
      </section>

      <div class="part-head"><p>PART II · LABEL</p><h2>Part title</h2></div>
      <section id="...">...</section>
      ...
    </main>
  </div>

  <footer>
    <p>Compiled from open sources. Study aid only, not an official publication.</p>
    <p>Source &amp; contributions: <a href="https://github.com/...">github.com/...</a></p>
  </footer>
</body>
</html>
```

`index.html` (the collection homepage) is a *lighter* variant of the same system —
no `.shell`/TOC/`main`, just `.hero` + a flat list of `section`s — see that file directly.

## CSS custom properties (`:root`)

Every file defines the same variable *names* with document-specific hex values, then
repeats the same values in two dark-mode blocks:

```css
:root{
  --ink:#101E26; --ink-soft:#4C6472;
  --paper:#F0F2F1; --surface:#FFFFFF; --surface-2:#E3E8E7;
  --rule:#C6CFCC; --rule-strong:#98A6A3;
  --red:#A62B22; --blue:#15517E; --gold:#9C7412; --green:#1E6B4E;
  --hero-bg:#101E26; --hero-ink:#F0F2F1; --hero-dim:#9DB0B8;
  color-scheme:light dark;
}
:root:not([data-theme="light"]){
  @media (prefers-color-scheme: dark){ /* same variable names, lighter-on-dark values */ }
}
:root[data-theme="dark"]{ /* identical block, for an explicit toggle */ }
```

`--red`/`--blue`/`--gold`/`--green` are semantic accents used by `.edge`, `.panel`, `.tag`,
`.timeline .yr` etc. — not decorative choices; keep all four defined even if a topic only
uses two or three of them, since shared components reference all four.

## Component catalogue

| Class | Purpose | Notes |
|---|---|---|
| `.hero` / `.flagstrip` / `.f1`..`.f8` | Top banner + coloured strip | Strip spans = real flag colours of the countries/entities covered |
| `.kicker` | Small eyebrow label above `<h1>` | Alternative to `.motto`/`.motto-en` epigraph |
| `.motto` / `.motto-en` | Large vernacular quote + English gloss/context | Used in files 7–8 for an evocative opening; optional |
| `.stamp` | Row of key stats in the hero | 4–6 `<span>Stat <b>value</b></span>` |
| `.shell` | Two-column grid: sticky TOC + `main` | Collapses to one column under 1000px |
| `.toc-mobile` + `nav.toc` | Contents list | `<li class="part">` = unlinked group header; every other `<li>` wraps an `<a href="#id">` |
| `.part-head` | Divider between major parts | `<p>` = uppercase part label in `--red`, `<h2>` = part title |
| `section > h2` + `.sec-rule` | Section heading + double rule | One `<section id="...">` per TOC entry |
| `.lede` | Larger intro paragraph directly under a section heading | |
| `.edge` (+ `.gold`/`.blue`/`.green`) | Left-border callout box, one key insight | `<span class="edge-h">` = coloured label line |
| `.note` | Neutral grey box, usually a bulleted list of caveats/traps | |
| `.panel` (+ `.blue-top`/`.red-top`/`.gold-top`/`.green-top`) | Bordered analysis block | Often wraps `dl.rungs` for a structured worked example |
| `dl.rungs` | Define/Locate/Establish/Contest/Analyse/Test/Position/Concede/Close style structured answer | `<dt>` label + `<dd>` content pairs, 2-col grid |
| `.timeline` + `li .yr` | Chronology list | `.yr` = bold year in `--red` |
| `.qbank` | Numbered practice-question list | Auto-numbered via CSS counter, no manual numbering in markup |
| `.dbank` | Alternative numbered list (debate motions) | Same counter pattern as `.qbank` |
| `.ladder` | Circular-numbered step list | Used for escalation ladders / response doctrines |
| `.grid.two` / `.grid.three` + `.card` | Comparison cards | `.card .name`/`.role`/`.since` sub-classes for people/office cards |
| `.officelist` | Compact list of post → holder → since-date rows | Used in file 8 for leadership rosters |
| `.tablewrap` + `table` | Scrollable data table | `caption` for table title, `td.num`/`th.num` for tabular-numeral columns |
| `.tag` (+ `.fact`/`.interp`/`.disputed`/`.verify`) | Inline pill labelling a claim's evidentiary status | Core to neutral treatment of contested figures |
| `.term` | Definition list glossary | `<dt>` term + `<dd>` definition |
| `figure`/`svg.svg-*` | Inline hand-drawn SVG maps/diagrams | Optional; only where a real geographic/organisational diagram adds value |
| `.home-btn` | Fixed pill button, top-left, on every page | `position:fixed`, hardcoded dark colours (not theme variables) so it reads consistently regardless of the page's palette or light/dark mode; `href="../index.html"`; hidden in `@media print` |
| `@media print` | Print stylesheet | Hides `.flagstrip`/`.toc-mobile`/`.home-btn`, forces page breaks before `.part-head`, avoids breaking `.panel`/`.edge`/`.note`/`.tablewrap`/`figure` |

## Typical closing "Knowledge toolkit" part

Every handbook ends with some ordering of: competing perspectives/narratives →
controversies → lesser-known facts → future scenarios → India's strategic options
(presented neutrally, without recommending a political position) → practice question
bank (`.qbank`, 20–25 items) → master chronology (`.timeline`) → glossary (`.term`) →
a concise one-to-two-page rapid revision sheet.
