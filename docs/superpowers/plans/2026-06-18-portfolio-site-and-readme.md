# Portfolio Site + Profile README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ship a static Editorial/Swiss portfolio site at `https://denfry.github.io` and rewrite the `denfry/denfry` profile README to be short, clean, and consistent with the site.

**Architecture:** A single static page (`index.html` + `style.css` + `main.js`) in a new repo `denfry/denfry.github.io`, served by GitHub Pages with no build step and no runtime dependencies. Bilingual content lives inline via `data-en` / `data-ru` attributes swapped by vanilla JS; light/dark theme via a `[data-theme]` attribute over CSS custom properties; both choices persisted in `localStorage`. The README is edited in place in the current repo (`C:\Projects\denfry`).

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla ES module JS, GitHub Pages, `gh` CLI, git.

## Global Constraints

- No CSS framework, no JS framework, no bundler, no build step.
- No external runtime dependencies: no CDN scripts, no web fonts — system font stacks only.
- No "AI slop": no gradients, no glow/box-shadow glow, no glassmorphism, no pill/rounded cards, no emoji, no shields.io tech-stack badges, no centered hero blocks.
- Palette light: bg `#F4F1EA`, text `#111111`, accent `#C2492D`, hairline `#D8D3C8`.
- Palette dark: bg `#121110`, text `#EDE9E0`, accent `#E0623E`, hairline `#2A2723`.
- Display font stack: `"Helvetica Neue", Arial, sans-serif`. Mono stack: `ui-monospace, "SF Mono", "Cascadia Mono", Menlo, monospace`.
- Default language EN; default theme = saved choice, else `prefers-color-scheme`.
- Max content width ~960px, left-aligned column grid.
- Featured projects (exact, in order): 01 codebase-index (Python, ⭐4), 02 agent-sync (Python), 03 OverWatch-ML (Java), 04 AquaGuard (Java), 05 ContinentRegions (Java), 06 WorldAccessBlocker (Java), 07 VeritasAd (Python).
- Site repo working dir: `C:\Projects\denfry.github.io` (sibling of the profile repo). README work happens in `C:\Projects\denfry`.

---

### Task 1: Create site repo and scaffold

**Files:**
- Create (remote+local): repo `denfry/denfry.github.io`
- Create: `C:\Projects\denfry.github.io\.nojekyll`
- Create: `C:\Projects\denfry.github.io\README.md`
- Create: `C:\Projects\denfry.github.io\index.html` (placeholder)
- Create: `C:\Projects\denfry.github.io\style.css` (empty)
- Create: `C:\Projects\denfry.github.io\main.js` (empty)

**Interfaces:**
- Produces: a git repo at `C:\Projects\denfry.github.io` with remote `origin` → `denfry/denfry.github.io`, containing the file skeleton the later tasks fill in.

- [ ] **Step 1: Create the remote repo and clone it**

```bash
gh repo create denfry/denfry.github.io --public \
  --description "Source for denfry.github.io — portfolio site" \
  --clone
# If the parent dir matters, run from C:\Projects so it clones to C:\Projects\denfry.github.io
```

- [ ] **Step 2: Add the scaffold files**

`.nojekyll` — empty file (disables Jekyll so files are served verbatim).

`README.md`:
```markdown
# denfry.github.io

Source for [denfry.github.io](https://denfry.github.io) — a static portfolio
page. Plain HTML/CSS/JS, no build step.
```

`index.html`:
```html
<!doctype html>
<html lang="en"><head><meta charset="utf-8"><title>denfry</title></head>
<body><!-- filled in Task 2 --></body></html>
```

`style.css`: empty. `main.js`: empty.

- [ ] **Step 3: Verify the repo state**

Run: `cd C:\Projects\denfry.github.io && git remote -v && ls -a`
Expected: remote `origin` points to `denfry/denfry.github.io`; files `.nojekyll README.md index.html style.css main.js` present.

- [ ] **Step 4: Commit**

```bash
cd C:\Projects\denfry.github.io
git add .
git commit -m "chore: scaffold portfolio site repo"
```

---

### Task 2: Build the HTML structure and bilingual content

**Files:**
- Modify: `C:\Projects\denfry.github.io\index.html` (full replace)

**Interfaces:**
- Consumes: nothing.
- Produces: semantic markup where every translatable element has both `data-en` and `data-ru`; `<html data-theme>` and `lang` are set by an inline head script; toggle controls have ids `#lang-toggle` and `#theme-toggle`; footer year span has id `#year`. `main.js` (Task 4) and `style.css` (Task 3) depend on these hooks.

- [ ] **Step 1: Write the full `index.html`**

```html
<!doctype html>
<html lang="en" data-theme="light">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Denfry — Java developer</title>
  <meta name="description" content="Denfry — Java developer building Minecraft server-side systems and open-source tooling for AI coding agents.">
  <link rel="stylesheet" href="style.css">
  <script>
    /* no-flash: apply saved theme + language before paint */
    (function () {
      try {
        var t = localStorage.getItem('theme');
        if (!t) t = matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
        document.documentElement.setAttribute('data-theme', t);
        var l = localStorage.getItem('lang') || 'en';
        document.documentElement.setAttribute('lang', l);
        document.documentElement.setAttribute('data-lang', l);
      } catch (e) {}
    })();
  </script>
</head>
<body>
  <div class="wrap">

    <header class="site-head">
      <a class="wordmark" href="/">DENFRY</a>
      <p class="role" data-en="Java developer" data-ru="Java-разработчик">Java developer</p>
      <nav class="head-nav">
        <button id="lang-toggle" class="ctl" type="button" aria-label="Toggle language">EN / RU</button>
        <button id="theme-toggle" class="ctl" type="button" aria-label="Toggle theme">Theme</button>
        <a class="ctl" href="https://t.me/kfcbossalbino">Telegram</a>
        <a class="ctl" href="https://github.com/denfry">GitHub</a>
      </nav>
    </header>

    <section class="intro">
      <p class="intro-lead"
         data-en="I build server-side Minecraft systems — plugins, mods, anti-cheat — and open-source tooling for AI coding agents."
         data-ru="Делаю серверные системы для Minecraft — плагины, моды, античит — и open-source инструменты для AI-агентов.">
        I build server-side Minecraft systems — plugins, mods, anti-cheat — and open-source tooling for AI coding agents.
      </p>
    </section>

    <section class="work">
      <h2 class="sec-label" data-en="Selected work" data-ru="Избранные проекты">Selected work</h2>
      <ol class="work-list">

        <li class="work-item">
          <span class="num">01</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/codebase-index">codebase-index</a>
            <p class="work-desc"
               data-en="Local-first codebase indexing for AI coding agents — hybrid FTS5 + Tree-sitter + graph search, fully offline."
               data-ru="Локальная индексация кода для AI-агентов — гибрид FTS5 + Tree-sitter + графовый поиск, полностью офлайн.">
              Local-first codebase indexing for AI coding agents — hybrid FTS5 + Tree-sitter + graph search, fully offline.</p>
            <p class="tags"><span>Python</span><span>★ 4</span></p>
          </div>
        </li>

        <li class="work-item">
          <span class="num">02</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/agent-sync">agent-sync</a>
            <p class="work-desc"
               data-en="Coordinate multiple AI coding-agent sessions in one repo: shared tasks, file locks, and live messaging over a local SQLite layer."
               data-ru="Координация нескольких сессий AI-агентов в одном репозитории: общие задачи, блокировки файлов и обмен сообщениями поверх локального SQLite.">
              Coordinate multiple AI coding-agent sessions in one repo: shared tasks, file locks, and live messaging over a local SQLite layer.</p>
            <p class="tags"><span>Python</span><span>CLI</span></p>
          </div>
        </li>

        <li class="work-item">
          <span class="num">03</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/OverWatch-ML">OverWatch-ML</a>
            <p class="work-desc"
               data-en="Cheat-detection system for modern Minecraft servers, using behavior analysis and machine learning."
               data-ru="Система детекта читов для современных Minecraft-серверов на основе анализа поведения и машинного обучения.">
              Cheat-detection system for modern Minecraft servers, using behavior analysis and machine learning.</p>
            <p class="tags"><span>Java</span><span>ML</span></p>
          </div>
        </li>

        <li class="work-item">
          <span class="num">04</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/AquaGuard">AquaGuard</a>
            <p class="work-desc"
               data-en="Block logging, inspection and rollback mod for Minecraft Forge 1.18.2–1.21.1."
               data-ru="Мод логирования блоков, инспекции и отката для Minecraft Forge 1.18.2–1.21.1.">
              Block logging, inspection and rollback mod for Minecraft Forge 1.18.2–1.21.1.</p>
            <p class="tags"><span>Java</span><span>Forge mod</span></p>
          </div>
        </li>

        <li class="work-item">
          <span class="num">05</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/ContinentRegions">ContinentRegions</a>
            <p class="work-desc"
               data-en="Paper 1.21.x plugin: draw continents on a BlueMap web map and turn them into WorldGuard regions. SQLite, flag presets, rollback, REST API."
               data-ru="Плагин Paper 1.21.x: рисует континенты на веб-карте BlueMap и превращает их в регионы WorldGuard. SQLite, пресеты флагов, откат, REST API.">
              Paper 1.21.x plugin: draw continents on a BlueMap web map and turn them into WorldGuard regions. SQLite, flag presets, rollback, REST API.</p>
            <p class="tags"><span>Java</span><span>Paper plugin</span></p>
          </div>
        </li>

        <li class="work-item">
          <span class="num">06</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/WorldAccessBlocker">WorldAccessBlocker</a>
            <p class="work-desc"
               data-en="Blocks access to the End, Nether and elytra with configurable, fine-grained rules."
               data-ru="Блокирует доступ в Энд, Незер и к элитрам с гибкими настраиваемыми правилами.">
              Blocks access to the End, Nether and elytra with configurable, fine-grained rules.</p>
            <p class="tags"><span>Java</span><span>Paper plugin</span></p>
          </div>
        </li>

        <li class="work-item">
          <span class="num">07</span>
          <div class="work-body">
            <a class="work-name" href="https://github.com/denfry/VeritasAd">VeritasAd</a>
            <p class="work-desc"
               data-en="Neural-network analysis system for detecting advertising integrations."
               data-ru="Нейросетевая система анализа для детекта рекламных интеграций.">
              Neural-network analysis system for detecting advertising integrations.</p>
            <p class="tags"><span>Python</span><span>ML</span></p>
          </div>
        </li>

      </ol>
    </section>

    <section class="focus">
      <h2 class="sec-label" data-en="Focus" data-ru="Направления">Focus</h2>
      <p class="focus-text"
         data-en="Minecraft (Paper, Spigot, Forge) · Java backend (Spring, PostgreSQL, MySQL) · Python tooling and ML · anti-cheat and behavior analysis · server performance and infrastructure."
         data-ru="Minecraft (Paper, Spigot, Forge) · Java-бэкенд (Spring, PostgreSQL, MySQL) · инструменты на Python и ML · античит и анализ поведения · производительность серверов и инфраструктура.">
        Minecraft (Paper, Spigot, Forge) · Java backend (Spring, PostgreSQL, MySQL) · Python tooling and ML · anti-cheat and behavior analysis · server performance and infrastructure.</p>
    </section>

    <section class="background">
      <h2 class="sec-label" data-en="Background" data-ru="О себе">Background</h2>
      <p class="bg-text"
         data-en="Networking and information systems, database design, and backend architecture. I run my own Minecraft server, so most projects are tested in real production, not just prototypes."
         data-ru="Сетевые технологии и информационные системы, проектирование баз данных и серверная архитектура. Держу собственный Minecraft-сервер, поэтому большинство проектов проверены в реальной эксплуатации, а не только в прототипах.">
        Networking and information systems, database design, and backend architecture. I run my own Minecraft server, so most projects are tested in real production, not just prototypes.</p>
    </section>

    <footer class="site-foot">
      <div class="foot-links">
        <a href="https://t.me/kfcbossalbino">Telegram</a>
        <a href="https://github.com/denfry">GitHub</a>
        <a href="https://github.com/denfry?tab=repositories"
           data-en="All repositories →" data-ru="Все репозитории →">All repositories →</a>
      </div>
      <p class="colophon">© <span id="year">2026</span> denfry</p>
    </footer>

  </div>
  <script type="module" src="main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify `data-en` / `data-ru` pairing (real check)**

Run (PowerShell, counts must be equal):
```powershell
$en = (Select-String -Path index.html -Pattern 'data-en=' -AllMatches).Matches.Count
$ru = (Select-String -Path index.html -Pattern 'data-ru=' -AllMatches).Matches.Count
"en=$en ru=$ru"; if ($en -ne $ru) { throw "data-en/data-ru mismatch" }
```
Expected: `en` equals `ru` (no exception thrown).

- [ ] **Step 3: Verify no forbidden patterns**

Run:
```powershell
Select-String -Path index.html -Pattern 'shields\.io|linear-gradient|radial-gradient|backdrop-filter|😀|🚀'
```
Expected: no matches (empty output).

- [ ] **Step 4: Commit**

```bash
cd C:\Projects\denfry.github.io
git add index.html
git commit -m "feat: add page structure and bilingual content"
```

---

### Task 3: Style the page (Editorial/Swiss, light + dark)

**Files:**
- Modify: `C:\Projects\denfry.github.io\style.css` (full content)

**Interfaces:**
- Consumes: the class/id hooks from Task 2 (`.wrap`, `.site-head`, `.wordmark`, `.role`, `.head-nav`, `.ctl`, `.intro-lead`, `.sec-label`, `.work-list`, `.work-item`, `.num`, `.work-name`, `.work-desc`, `.tags`, `.focus-text`, `.bg-text`, `.site-foot`, `[data-theme]`).
- Produces: a complete stylesheet; no later task depends on internal selectors.

- [ ] **Step 1: Write the full `style.css`**

```css
:root {
  --bg: #F4F1EA;
  --text: #111111;
  --muted: #5b554a;
  --accent: #C2492D;
  --hair: #D8D3C8;
  --maxw: 960px;
  --sans: "Helvetica Neue", Arial, sans-serif;
  --mono: ui-monospace, "SF Mono", "Cascadia Mono", Menlo, monospace;
}
[data-theme="dark"] {
  --bg: #121110;
  --text: #EDE9E0;
  --muted: #9a9384;
  --accent: #E0623E;
  --hair: #2A2723;
}

* { box-sizing: border-box; }
html { -webkit-text-size-adjust: 100%; }
body {
  margin: 0;
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  line-height: 1.5;
  transition: background-color .25s ease, color .25s ease;
}

.wrap {
  max-width: var(--maxw);
  margin: 0 auto;
  padding: clamp(2rem, 5vw, 5rem) clamp(1.25rem, 5vw, 3rem) 4rem;
}

/* header */
.site-head {
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: baseline;
  gap: .5rem 1.5rem;
  padding-bottom: 1.25rem;
  border-bottom: 1px solid var(--hair);
}
.wordmark {
  font-weight: 700;
  font-size: clamp(1.4rem, 3vw, 1.9rem);
  letter-spacing: .12em;
  text-decoration: none;
  color: var(--text);
}
.role {
  grid-column: 1 / 2;
  margin: .15rem 0 0;
  font-family: var(--mono);
  font-size: .8rem;
  color: var(--muted);
}
.head-nav {
  grid-column: 2 / 3;
  grid-row: 1 / 3;
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-self: start;
}
.ctl {
  font-family: var(--mono);
  font-size: .78rem;
  color: var(--text);
  background: none;
  border: 0;
  padding: 0;
  cursor: pointer;
  text-decoration: none;
  border-bottom: 1px solid transparent;
}
.ctl:hover { border-bottom-color: var(--accent); color: var(--accent); }

/* intro */
.intro { padding: clamp(2.5rem, 7vw, 5rem) 0; }
.intro-lead {
  margin: 0;
  max-width: 22ch;
  font-weight: 700;
  font-size: clamp(2rem, 6vw, 3.6rem);
  line-height: 1.05;
  letter-spacing: -.01em;
}

/* section label */
.sec-label {
  font-family: var(--mono);
  font-weight: 400;
  font-size: .78rem;
  text-transform: uppercase;
  letter-spacing: .18em;
  color: var(--muted);
  margin: 0 0 1.5rem;
}

/* selected work */
.work { padding-top: 2rem; border-top: 1px solid var(--hair); }
.work-list { list-style: none; margin: 0; padding: 0; }
.work-item {
  display: grid;
  grid-template-columns: 3rem 1fr;
  gap: 0 1.25rem;
  padding: 1.5rem 0;
  border-top: 1px solid var(--hair);
}
.work-item:first-child { border-top: 0; }
.num { font-family: var(--mono); font-size: .85rem; color: var(--muted); padding-top: .35rem; }
.work-name {
  display: inline-block;
  font-size: clamp(1.25rem, 3vw, 1.6rem);
  font-weight: 700;
  color: var(--text);
  text-decoration: none;
  border-bottom: 2px solid transparent;
}
.work-name:hover { border-bottom-color: var(--accent); color: var(--accent); }
.work-desc { margin: .4rem 0 .6rem; max-width: 60ch; color: var(--text); }
.tags { margin: 0; display: flex; gap: .5rem; flex-wrap: wrap; }
.tags span {
  font-family: var(--mono);
  font-size: .72rem;
  letter-spacing: .04em;
  color: var(--muted);
  border: 1px solid var(--hair);
  padding: .15rem .5rem;
}

/* focus + background */
.focus, .background { padding: 2.5rem 0; border-top: 1px solid var(--hair); }
.focus-text, .bg-text { margin: 0; max-width: 65ch; font-size: 1.05rem; }
.focus-text { font-size: 1.15rem; }

/* footer */
.site-foot {
  padding-top: 2rem;
  border-top: 1px solid var(--hair);
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 1rem;
}
.foot-links { display: flex; gap: 1.5rem; flex-wrap: wrap; }
.foot-links a {
  font-family: var(--mono);
  font-size: .82rem;
  color: var(--text);
  text-decoration: none;
  border-bottom: 1px solid transparent;
}
.foot-links a:hover { border-bottom-color: var(--accent); color: var(--accent); }
.colophon { margin: 0; font-family: var(--mono); font-size: .78rem; color: var(--muted); }

@media (max-width: 520px) {
  .work-item { grid-template-columns: 2rem 1fr; gap: 0 .75rem; }
}
```

- [ ] **Step 2: Verify no forbidden style patterns**

Run:
```powershell
Select-String -Path style.css -Pattern 'gradient|backdrop-filter|border-radius:\s*[1-9]|box-shadow'
```
Expected: no matches. (Confirms no rounded pill cards, glassmorphism, glow, or gradients.)

- [ ] **Step 3: Visual check in the browser**

Open `index.html` in a browser. Confirm: left-aligned grid, large bold intro line, numbered work list with hairline dividers, terracotta on link hover, warm off-white background. No console errors (toggles are wired in Task 4 — buttons may be inert here, that is expected).

- [ ] **Step 4: Commit**

```bash
cd C:\Projects\denfry.github.io
git add style.css
git commit -m "feat: add Editorial/Swiss styling with light and dark themes"
```

---

### Task 4: Wire language toggle, theme toggle, and footer year

**Files:**
- Modify: `C:\Projects\denfry.github.io\main.js` (full content)

**Interfaces:**
- Consumes: `#lang-toggle`, `#theme-toggle`, `#year`, all `[data-en]`/`[data-ru]` elements, `<html data-theme>` and `lang`/`data-lang` set inline in `<head>`.
- Produces: runtime behavior only.

- [ ] **Step 1: Write the full `main.js`**

```js
const root = document.documentElement;

function applyLang(lang) {
  document.querySelectorAll('[data-en]').forEach((el) => {
    const text = el.getAttribute('data-' + lang);
    if (text != null) el.textContent = text;
  });
  root.setAttribute('lang', lang);
  root.setAttribute('data-lang', lang);
  try { localStorage.setItem('lang', lang); } catch (e) {}
}

function applyTheme(theme) {
  root.setAttribute('data-theme', theme);
  try { localStorage.setItem('theme', theme); } catch (e) {}
}

// initialize from what the no-flash head script already set
let lang = root.getAttribute('data-lang') || 'en';
let theme = root.getAttribute('data-theme') || 'light';
applyLang(lang);

const langBtn = document.getElementById('lang-toggle');
const themeBtn = document.getElementById('theme-toggle');

langBtn.addEventListener('click', () => {
  lang = lang === 'en' ? 'ru' : 'en';
  applyLang(lang);
});

themeBtn.addEventListener('click', () => {
  theme = theme === 'light' ? 'dark' : 'light';
  applyTheme(theme);
});

const yearEl = document.getElementById('year');
if (yearEl) yearEl.textContent = String(new Date().getFullYear());
```

- [ ] **Step 2: Verify behavior in the browser**

Open `index.html`. Confirm, with the console open (zero errors):
1. Click `EN / RU` → all text switches to Russian; reload → stays Russian.
2. Click `Theme` → switches to dark palette; reload → stays dark.
3. Footer year reads the current year.
4. With `localStorage` cleared and OS in dark mode, first load is dark (prefers-color-scheme).

- [ ] **Step 3: Commit**

```bash
cd C:\Projects\denfry.github.io
git add main.js
git commit -m "feat: wire language toggle, theme toggle, and footer year"
```

---

### Task 5: Publish via GitHub Pages and verify live

**Files:**
- None (repo settings + push only)

**Interfaces:**
- Consumes: the committed site from Tasks 1–4.
- Produces: a live site at `https://denfry.github.io`.

- [ ] **Step 1: Push all commits**

```bash
cd C:\Projects\denfry.github.io
git push -u origin main   # or master, match the default branch gh created
```

- [ ] **Step 2: Enable Pages from the default branch root**

```bash
gh api -X POST repos/denfry/denfry.github.io/pages \
  -f "source[branch]=main" -f "source[path]=/" || \
gh api -X PUT repos/denfry/denfry.github.io/pages \
  -f "source[branch]=main" -f "source[path]=/"
```
(For a `denfry.github.io` user-site repo Pages often auto-enables on first push; this makes it explicit. Adjust branch name if the repo default is `master`.)

- [ ] **Step 3: Verify the live site**

Run (after ~1 min for the first build):
```bash
curl -sS -o /dev/null -w "%{http_code}\n" https://denfry.github.io
```
Expected: `200`. Then open `https://denfry.github.io` in a browser and re-run the Task 4 Step 2 checks against the live page; confirm zero console errors.

- [ ] **Step 4: No commit needed** (settings + push only). Record the live URL in the site `README.md` if not already present.

---

### Task 6: Rewrite the profile README

**Files:**
- Modify: `C:\Projects\denfry\README.md` (full replace)

**Interfaces:**
- Consumes: the curated project list and the live site URL from Task 5.
- Produces: the GitHub profile page content.

- [ ] **Step 1: Replace `README.md` with the short version**

```markdown
# Denfry

[![Telegram](https://img.shields.io/badge/Telegram-@kfcbossalbino-0a0a0a?style=flat-square&logo=telegram)](https://t.me/kfcbossalbino)
[![GitHub](https://img.shields.io/badge/GitHub-denfry-0a0a0a?style=flat-square&logo=github)](https://github.com/denfry)

Java developer. I build server-side Minecraft systems — plugins, mods, and
anti-cheat — and open-source tooling for AI coding agents. I run my own
Minecraft server, so most projects are tested in real production.

**Portfolio:** [denfry.github.io](https://denfry.github.io)

### Selected work

- **[codebase-index](https://github.com/denfry/codebase-index)** — local-first codebase indexing for AI coding agents (FTS5 + Tree-sitter + graph search, offline). `Python`
- **[agent-sync](https://github.com/denfry/agent-sync)** — coordinate multiple AI coding-agent sessions: shared tasks, file locks, live messaging over SQLite. `Python`
- **[OverWatch-ML](https://github.com/denfry/OverWatch-ML)** — cheat-detection system for Minecraft servers using behavior analysis and ML. `Java`
- **[AquaGuard](https://github.com/denfry/AquaGuard)** — block logging, inspection and rollback mod for Forge 1.18.2–1.21.1. `Java`
- **[ContinentRegions](https://github.com/denfry/ContinentRegions)** — Paper plugin: draw continents on BlueMap → WorldGuard regions, with REST API. `Java`
- **[WorldAccessBlocker](https://github.com/denfry/WorldAccessBlocker)** — configurable access control for End, Nether and elytra. `Java`
- **[VeritasAd](https://github.com/denfry/VeritasAd)** — neural-network analysis system for advertising integrations. `Python`

### Stack

Java · Paper / Spigot / Forge · Spring · PostgreSQL · MySQL · Python · SQL · Git

### Contact

[Telegram @kfcbossalbino](https://t.me/kfcbossalbino) · [GitHub](https://github.com/denfry) · [all repositories](https://github.com/denfry?tab=repositories)
```

- [ ] **Step 2: Verify length and content**

Run:
```powershell
(Get-Content C:\Projects\denfry\README.md | Measure-Object -Line).Lines
Select-String -Path C:\Projects\denfry\README.md -Pattern 'img.shields.io' -AllMatches | ForEach-Object { $_.Matches.Count }
```
Expected: line count well under the original 105 (target ~30); exactly 2 shields.io badge references (Telegram + GitHub) — no tech-stack badge wall.

- [ ] **Step 3: Verify no AI-slop cliché stacking**

Run:
```powershell
Select-String -Path C:\Projects\denfry\README.md -Pattern 'production-ready|scalable|cutting-edge|seamless|robust|leverage'
```
Expected: no matches (or only the intentional, factual "real production").

- [ ] **Step 4: Commit**

```bash
cd C:\Projects\denfry
git add README.md
git commit -m "docs: rewrite profile README — shorter, curated, no badge wall"
```

---

## Self-Review

**Spec coverage:**
- Site at `denfry.github.io`, new repo → Task 1, 5. ✓
- Editorial/Swiss visual language, palette, fonts → Task 3 + Global Constraints. ✓
- Bilingual EN/RU switcher → Task 2 (content) + Task 4 (toggle). ✓
- Light/dark toggle, persistence, no-flash → Task 2 (head script) + Task 4. ✓
- Curated 7 projects + "all repositories" link → Task 2, Task 6. ✓
- README rewrite, shorter, 2 badges only → Task 6. ✓
- No framework / no build / no runtime deps → Global Constraints, enforced by Task 2/3 grep checks. ✓
- Success criteria (200 OK, toggles persist, no console errors) → Task 4, Task 5. ✓

**Placeholder scan:** No TBD/TODO; all code blocks are complete; the only deliberate placeholder is the Task 1 stub `index.html`, fully replaced in Task 2.

**Type/hook consistency:** ids `#lang-toggle`, `#theme-toggle`, `#year` and attributes `data-en`/`data-ru`/`data-theme`/`data-lang` are defined in Task 2 and consumed identically in Tasks 3 and 4. Branch name caveat (`main` vs `master`) is flagged in Task 5.
