# Portfolio Site + Profile README — Design

**Date:** 2026-06-18
**Author:** denfry (with Claude)

## Goal

Two deliverables:

1. Rewrite the GitHub **profile README** (`denfry/denfry`) to be shorter, more
   useful, and visually cleaner.
2. Build a **portfolio website** hosted at `https://denfry.github.io` (new repo
   `denfry/denfry.github.io`).

Both share one curated set of projects and an **Editorial / Swiss** visual
language. The hard requirement: no "AI slop" — no purple/blue gradients, no
glassmorphism, no emoji spam, no shields.io badge walls, no everything-centered
layout.

## Decisions (locked)

| Topic | Decision |
|-------|----------|
| Design direction | Editorial / Swiss typographic |
| Hosting | New repo `denfry/denfry.github.io` (clean root URL) |
| Language | Bilingual EN/RU with a client-side switcher |
| Projects | Curated (~7 featured) + link to all repos |
| Theme | Light/dark toggle |
| Accent color | Terracotta `#C2492D` |
| Tech | Plain HTML + CSS + minimal vanilla JS; no framework, no build step |

## Featured projects (curated)

Original, non-fork projects with real substance:

| # | Name | Lang | One-liner |
|---|------|------|-----------|
| 01 | codebase-index | Python | Local-first codebase indexing for AI coding agents — hybrid FTS5 + Tree-sitter + graph search, fully offline. ⭐4 |
| 02 | agent-sync | Python | Coordinate multiple AI coding-agent sessions in one repo: shared tasks, file locks, live messaging over local SQLite. |
| 03 | OverWatch-ML | Java | Next-gen cheat-detection system for modern Minecraft servers. |
| 04 | AquaGuard | Java | Block logging, inspection & rollback mod for Minecraft Forge 1.18.2–1.21.1. |
| 05 | ContinentRegions | Java | Paper 1.21.x plugin: draw continents on a BlueMap web map → WorldGuard polygon regions. SQLite, flags, rollback, REST API. |
| 06 | WorldAccessBlocker | Java | Blocks access to End / Nether / elytra with configurable rules. |
| 07 | VeritasAd | Python | Neural-network analysis system for advertising integrations. |

University labs (`Lab*`, `Algos*`, etc.) and forks are excluded from the
curated set. A "All repositories →" link points to
`https://github.com/denfry?tab=repositories`.

## Visual language

- **Typography is the hero.** Display: bold grotesque — system stack
  `"Helvetica Neue", Arial, sans-serif` (authentic Swiss, instant load).
  Labels / numbers / tags: monospace system stack
  (`ui-monospace, "SF Mono", "Cascadia Mono", Menlo, monospace`).
- **Palette (light):** background `#F4F1EA` (warm off-white), text `#111111`,
  accent `#C2492D` (terracotta), hairline `#D8D3C8`.
- **Palette (dark):** background `#121110`, text `#EDE9E0`, accent `#E0623E`
  (slightly lifted terracotta for contrast), hairline `#2A2723`.
- **Layout:** strict left-aligned column grid, generous whitespace, thin
  hairline dividers between sections. Max content width ~960px.
- **No:** gradients, glow shadows, pill cards, emoji, shields.io badges,
  centered hero blocks.
- **Motion:** minimal — at most a subtle underline/offset on link hover and a
  smooth theme transition. No scroll-jacking, no parallax.

## Site structure (single page, scroll)

1. **Header** — `DENFRY` wordmark; short title (e.g. "Java developer");
   right side: `EN / RU` toggle, theme toggle, TG + GitHub links.
2. **Intro** — one or two large lines stating who he is and what he builds.
3. **Selected Work** — numbered list `01 — 07`: name, one-line description,
   tags (language/type), link to repo. Hairline between rows.
4. **Focus / Stack** — compact prose (not badges): Minecraft (Paper/Spigot/
   Forge), Java backend (Spring, SQL/PostgreSQL/MySQL), AI dev-tools,
   anti-cheat / ML.
5. **Background** — 2–3 lines (networking, databases, backend architecture).
6. **Footer** — contacts (Telegram, GitHub) + "All repositories →" + year.

## File structure (`denfry/denfry.github.io`)

```
index.html      # semantic markup, both EN and RU text present (data-lang)
style.css       # design tokens via CSS custom properties; light/dark via [data-theme]
main.js         # language toggle, theme toggle (persist to localStorage), footer year
.nojekyll       # skip Jekyll processing
README.md       # short note: "Source for denfry.github.io"
```

### Bilingual mechanism

Both languages live in the HTML. Each translatable element carries
`data-en="..."` and `data-ru="..."`; `main.js` swaps `textContent` based on the
chosen language and sets `document.documentElement.lang`. Default language: EN.
Choice persisted in `localStorage`. No flash: inline a tiny script in `<head>`
that applies the saved theme + language before paint.

### Theme mechanism

`[data-theme="light|dark"]` on `<html>`; all colors are CSS custom properties
that switch under the attribute. Toggle persists to `localStorage`. Initial
value: saved choice, else `prefers-color-scheme`.

## README plan (`denfry/denfry`)

Cut from ~105 lines to a tight, scannable profile:

- Short H1 + one-line subtitle. Keep exactly two badges (Telegram, GitHub);
  drop all the tech-stack shields.io badges.
- 2–3 lines about what he does (run through anti-AI-slop editing — no
  "scalable, production-ready, real production use" cliché stacking).
- **Selected Work** — the same 7 projects as a compact list with links.
- **Stack** — one line, comma-separated, not a badge wall.
- **Contact** — Telegram, GitHub, link to the site.
- Remove duplicated sections ("What I Build" vs "Main Focus" vs "Development
  Principles" overlap), the long Keywords block (collapse to a single hidden-
  feeling line or drop), and redundant horizontal rules.

## Out of scope

- Custom domain / DNS.
- A build pipeline, bundler, or CSS framework.
- Dynamic GitHub API fetching (curated content is static and deliberate).
- Blog / multi-page site.
- Analytics.

## Success criteria

- Site loads at `https://denfry.github.io` with zero console errors.
- EN/RU toggle and light/dark toggle both work and persist across reloads.
- No external runtime dependencies (no CDN frameworks); fonts are system stacks.
- README renders cleanly on the GitHub profile, noticeably shorter than before.
- Visual result reads as deliberate editorial design, not generic AI template.
