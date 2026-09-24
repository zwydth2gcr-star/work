# Лендинг «Мозг бухгалтерии» Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single self-contained `index.html` landing page for the «Мозг бухгалтерии» idea that collects early contact leads before the actual `SKILL.md` procedures exist.

**Architecture:** One dependency-free HTML file, following the same convention as `Persona/index.html` and `Game/index.html` (CSS custom properties in `:root`, `prefers-color-scheme: dark` override, no build tooling). Task 1 lays down the full page skeleton and CSS system; Tasks 2–4 fill in one content section each, reusing exactly the classes and empty containers Task 1 defines; Task 5 verifies the finished page end-to-end.

**Tech Stack:** HTML5, CSS3 (custom properties, `prefers-color-scheme`, CSS Grid), vanilla JS (~10 lines, only for the CTA mailto handler). No frameworks, no bundler. Verified with the `playwright` MCP tools (`mcp__playwright__browser_navigate`, `browser_resize`, `browser_emulate_media`, `browser_take_screenshot`, `browser_snapshot`).

**Spec:** `docs/superpowers/specs/2026-09-24-mozg-buhgalterii-landing-design.md`

## Global Constraints

- Single file: `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html`. No external libraries, no build tooling — must open directly via double-click (spec: "Технический подход").
- All visible copy is in Russian (spec: throughout; project-wide convention from `StartUp/CLAUDE.md`).
- No real company data, contractor names, or figures anywhere in the copy — only invented examples (spec: "Риски").
- Every procedure card is labeled "В разработке" — no `SKILL.md` files exist yet, the page must not claim otherwise (spec: "Риски", "Структура страницы" §3).
- No backend and no storage of collected contacts — the CTA is a `mailto:` link only, destination `rpw4dzvs8g@privaterelay.appleid.com` (spec: "Структура страницы" §6, confirmed by user).
- Light/dark theme via `prefers-color-scheme` CSS only — no theme-toggle JS (spec: "Технический подход").

## Review Focus

- Submitting the CTA form with an empty or invalid contact field must not open a `mailto:` link with a blank body — a user who taps the button by mistake shouldn't fire off an empty email draft. (owned by Task 4)
- On a very wide desktop viewport (≥1600px), body text must stay capped by `.wrap`'s max-width instead of stretching edge-to-edge and becoming unreadable. (owned by Task 1)
- Switching the OS color scheme while the page is already open must repaint colors immediately via the `prefers-color-scheme` media query, without a manual reload. (owned by Task 1)
- FAQ `<details>` items must be collapsed by default on page load — a page that opens with everything already expanded looks cluttered and defeats the point of using `<details>`. (owned by Task 3)
- At mobile width (375px), the procedure-card grid must collapse to one column and the CTA form must stack vertically, with no horizontal page scroll. (owned by Task 5, since it needs the complete page content from Tasks 1–4 to check meaningfully)

---

### Task 1: Каркас и стили

**Files:**
- Create: `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html`

**Interfaces:**
- Consumes: nothing (first task).
- Produces (for Tasks 2–5 to reuse verbatim):
  - CSS custom properties on `:root` (light) and inside `@media (prefers-color-scheme: dark)`: `--ink`, `--paper`, `--surface`, `--card-bg`, `--muted`, `--border`, `--accent`, `--accent-dark`, `--accent-contrast`, `--shadow`, `--radius`, `--badge-bg`, `--badge-ink`.
  - CSS classes: `.wrap`, `.section`, `.section--alt`, `.muted`, `.btn`, `.badge`, `.hero`, `.hero .lead`, `.procedures-grid`, `.procedure-card`, `.steps`, `.faq` (with `details`/`summary` rules), `.cta-form` (with `input` rules), `footer.site-footer`.
  - Empty section shells with ids, each containing one empty `<div class="wrap"></div>`: `#hero`, `#problem-solution`, `#procedures`, `#how-it-works`, `#faq`, `#cta`, plus `<footer class="site-footer">` with its own empty `.wrap`.

- [ ] **Step 1: Create the directory and the file with full `<head>`, CSS, and empty section skeleton**

Create `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html`:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Мозг бухгалтерии — процедуры для Claude Code</title>
<meta name="description" content="Набор процедур для делегирования рутины бухгалтерии агенту в Claude Code.">
<style>
  :root {
    color-scheme: light dark;
    --ink: #21262b;
    --paper: #f6f7f8;
    --surface: #ffffff;
    --card-bg: #ffffff;
    --muted: #5b6570;
    --border: rgba(33, 38, 43, 0.12);
    --accent: #2f5d6b;
    --accent-dark: #1f414c;
    --accent-contrast: #ffffff;
    --shadow: rgba(20, 24, 28, 0.08);
    --radius: 12px;
    --badge-bg: rgba(47, 93, 107, 0.12);
    --badge-ink: #2f5d6b;
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --ink: #e6e9eb;
      --paper: #181b1e;
      --surface: #202428;
      --card-bg: #23282c;
      --muted: #9aa4ad;
      --border: rgba(230, 233, 235, 0.14);
      --accent: #74a9b8;
      --accent-dark: #93c3d0;
      --accent-contrast: #12181a;
      --shadow: rgba(0, 0, 0, 0.4);
      --badge-bg: rgba(116, 169, 184, 0.16);
      --badge-ink: #93c3d0;
    }
  }

  * { box-sizing: border-box; }
  html { scroll-behavior: smooth; }
  html, body { margin: 0; padding: 0; }

  body {
    background: var(--paper);
    color: var(--ink);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    line-height: 1.65;
    -webkit-font-smoothing: antialiased;
  }

  h1, h2, h3 { line-height: 1.25; margin: 0 0 0.5em; font-weight: 700; }
  p { margin: 0 0 1em; }
  code {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 1px 6px;
    font-size: 0.9em;
  }

  .wrap { max-width: 760px; margin: 0 auto; padding: 0 24px; }
  .section { padding: 64px 0; }
  .section--alt { background: var(--surface); }
  .muted { color: var(--muted); }

  .btn {
    display: inline-block;
    background: var(--accent);
    color: var(--accent-contrast);
    padding: 14px 28px;
    border-radius: var(--radius);
    text-decoration: none;
    font-weight: 600;
    border: none;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.15s ease;
  }
  .btn:hover, .btn:focus-visible { background: var(--accent-dark); }

  .badge {
    display: inline-block;
    background: var(--badge-bg);
    color: var(--badge-ink);
    font-size: 13px;
    font-weight: 600;
    padding: 4px 10px;
    border-radius: 999px;
    margin-top: 8px;
  }

  .hero { padding-top: 88px; }
  .hero h1 { font-size: 40px; }
  .hero .lead { font-size: 19px; color: var(--muted); max-width: 560px; }

  .procedures-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
    margin-top: 24px;
  }
  .procedure-card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
    box-shadow: 0 1px 2px var(--shadow);
  }
  .procedure-card h3 { font-size: 17px; margin-bottom: 4px; }

  .steps { padding-left: 20px; }
  .steps li { margin-bottom: 12px; }

  .faq details {
    border-bottom: 1px solid var(--border);
    padding: 16px 0;
  }
  .faq summary {
    cursor: pointer;
    font-weight: 600;
    list-style: none;
  }
  .faq summary::-webkit-details-marker { display: none; }
  .faq summary::after { content: "+"; float: right; color: var(--muted); }
  .faq details[open] summary::after { content: "\2013"; }
  .faq p { margin-top: 12px; color: var(--muted); }

  .cta-form { display: flex; gap: 12px; margin-top: 24px; flex-wrap: wrap; }
  .cta-form input {
    flex: 1 1 260px;
    padding: 14px 16px;
    border-radius: var(--radius);
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--ink);
    font-size: 16px;
  }
  .cta-form input:focus-visible { outline: 2px solid var(--accent); }

  footer.site-footer {
    padding: 32px 0 48px;
    text-align: center;
    color: var(--muted);
    font-size: 13px;
  }

  @media (max-width: 640px) {
    .hero h1 { font-size: 30px; }
    .procedures-grid { grid-template-columns: 1fr; }
    .cta-form { flex-direction: column; }
    .section { padding: 48px 0; }
  }
</style>
</head>
<body>

<section id="hero" class="section hero">
  <div class="wrap"></div>
</section>

<section id="problem-solution" class="section section--alt">
  <div class="wrap"></div>
</section>

<section id="procedures" class="section">
  <div class="wrap"></div>
</section>

<section id="how-it-works" class="section section--alt">
  <div class="wrap"></div>
</section>

<section id="faq" class="section faq">
  <div class="wrap"></div>
</section>

<section id="cta" class="section section--alt">
  <div class="wrap"></div>
</section>

<footer class="site-footer">
  <div class="wrap"></div>
</footer>

</body>
</html>
```

- [ ] **Step 2: Verify the skeleton opens and the zebra/typography/dark-mode system works**

Run: `open /Users/alesasycevnik/Work/mozg-buhgalterii/index.html`
Expected: page opens in the default browser with six empty bands of alternating background color (`--paper` / `--surface`) stacked top to bottom, plus a slim footer band. No content text yet — that's expected, sections are still empty.

- [ ] **Step 3: Verify wide-viewport line length is capped (Review Focus item 2)**

Using the `playwright` MCP tools:
1. `mcp__playwright__browser_navigate` to `file:///Users/alesasycevnik/Work/mozg-buhgalterii/index.html`
2. `mcp__playwright__browser_resize` to width `1800`, height `900`
3. `mcp__playwright__browser_take_screenshot`

Expected: the (currently empty) section bands do not span the full 1800px browser width — each keeps the `.wrap` container's centered, padded column (max 760px + 24px side padding). Confirm visually from the screenshot.

- [ ] **Step 4: Verify dark mode repaints live (Review Focus item 3)**

Using the `playwright` MCP tools, on the same open page:
1. `mcp__playwright__browser_emulate_media` with `colorScheme: "dark"`
2. `mcp__playwright__browser_take_screenshot`
3. `mcp__playwright__browser_emulate_media` with `colorScheme: "light"`
4. `mcp__playwright__browser_take_screenshot`

Expected: background/text colors swap between the two screenshots without navigating or reloading — confirms the `prefers-color-scheme` media query is wired correctly.

- [ ] **Step 5: Commit**

```bash
cd /Users/alesasycevnik/Work
git add mozg-buhgalterii/index.html
git commit -m "$(cat <<'EOF'
Add skeleton and style system for «Мозг бухгалтерии» landing

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: Первый экран (Hero)

**Files:**
- Modify: `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html` (the empty `<div class="wrap">` inside `#hero`)

**Interfaces:**
- Consumes: `.hero`, `.hero .lead`, `.btn` classes and the empty `#hero > .wrap` div from Task 1.
- Produces: nothing new for later tasks — later tasks only rely on `#cta` existing as a scroll target, which Task 1 already created.

- [ ] **Step 1: Fill in the hero copy and CTA button**

In `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html`, replace:

```html
<section id="hero" class="section hero">
  <div class="wrap"></div>
</section>
```

with:

```html
<section id="hero" class="section hero">
  <div class="wrap">
    <h1>Мозг бухгалтерии</h1>
    <p class="lead">Набор из 5–7 процедур для Claude Code — закрытие месяца, сверка с контрагентами, проверка первички и другая рутина, которую можно поручить агенту, а не сотруднику.</p>
    <a class="btn" href="#cta">Хочу процедуру</a>
  </div>
</section>
```

- [ ] **Step 2: Verify the hero renders and the CTA button scrolls**

Run: `open /Users/alesasycevnik/Work/mozg-buhgalterii/index.html`
Expected: heading "Мозг бухгалтерии", the lead paragraph, and a styled "Хочу процедуру" button are visible at the top. Click the button.
Expected: the page smooth-scrolls down to the (still empty) `#cta` band — confirms the `scroll-behavior: smooth` + anchor link from Task 1 works correctly.

- [ ] **Step 3: Commit**

```bash
cd /Users/alesasycevnik/Work
git add mozg-buhgalterii/index.html
git commit -m "$(cat <<'EOF'
Fill hero section on «Мозг бухгалтерии» landing

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: Блок проблемы и решения

**Files:**
- Modify: `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html` (the empty `<div class="wrap">` inside `#problem-solution`, `#procedures`, `#how-it-works`, and `#faq`)

**Interfaces:**
- Consumes: `.procedures-grid`, `.procedure-card`, `.badge`, `.steps`, `.faq` (+ `details`/`summary` rules), `.muted` classes, and the four empty `.wrap` divs from Task 1.
- Produces: nothing new for later tasks — Task 4 only needs `#cta`'s empty wrap, already present from Task 1.

- [ ] **Step 1: Fill in the problem/solution section**

Replace:

```html
<section id="problem-solution" class="section section--alt">
  <div class="wrap"></div>
</section>
```

with:

```html
<section id="problem-solution" class="section section--alt">
  <div class="wrap">
    <h2>Слышали про агентов. Не знаете, с чего начать поручать им рутину</h2>
    <p>У сотрудников каждый месяц одни и те же задачи: закрыть период, свести контрагентов, проверить первичку перед отчётом. Поручить это агенту в теории можно — но непонятно, с чего начать: как описать процедуру, что агент должен делать, где граница между «сделал сам» и «доверил агенту».</p>
    <p>«Мозг бухгалтерии» — обезличенный набор уже описанных процедур для Claude Code. Каждая — это готовый файл <code>SKILL.md</code>: пошаговая инструкция, которую сотрудник подключает и запускает, не придумывая формулировки с нуля.</p>
  </div>
</section>
```

- [ ] **Step 2: Fill in the procedures list**

Replace:

```html
<section id="procedures" class="section">
  <div class="wrap"></div>
</section>
```

with:

```html
<section id="procedures" class="section">
  <div class="wrap">
    <h2>Что входит в пак</h2>
    <div class="procedures-grid">
      <article class="procedure-card">
        <h3>Закрытие месяца</h3>
        <span class="badge">В разработке</span>
      </article>
      <article class="procedure-card">
        <h3>Сверка с контрагентами</h3>
        <span class="badge">В разработке</span>
      </article>
      <article class="procedure-card">
        <h3>Проверка первички</h3>
        <span class="badge">В разработке</span>
      </article>
      <article class="procedure-card">
        <h3>Подготовка регулярного отчёта</h3>
        <span class="badge">В разработке</span>
      </article>
    </div>
    <p class="muted">Список пополняется — дальше ещё 1–3 процедуры, отталкиваясь от того, что чаще всего просят.</p>
  </div>
</section>
```

- [ ] **Step 3: Fill in the "how it works" section**

Replace:

```html
<section id="how-it-works" class="section section--alt">
  <div class="wrap"></div>
</section>
```

with:

```html
<section id="how-it-works" class="section section--alt">
  <div class="wrap">
    <h2>Как это будет работать</h2>
    <ol class="steps">
      <li>Оставляете контакт в форме внизу страницы.</li>
      <li>Получаете готовый <code>SKILL.md</code> для первой процедуры, как только она готова.</li>
      <li>Подключаете файл к Claude Code и запускаете на своей задаче — без ручного описания процесса с нуля.</li>
    </ol>
  </div>
</section>
```

- [ ] **Step 4: Fill in the FAQ section**

Replace:

```html
<section id="faq" class="section faq">
  <div class="wrap"></div>
</section>
```

with:

```html
<section id="faq" class="section faq">
  <div class="wrap">
    <h2>Частые вопросы</h2>
    <details>
      <summary>Это безопасно для данных компании?</summary>
      <p>Сами процедуры не содержат ни одного реального документа, контрагента или цифры — только выдуманные примеры. Вы подключаете свои данные сами, файл их никуда не передаёт.</p>
    </details>
    <details>
      <summary>Это готовая консультация или расчёт?</summary>
      <p>Нет. Это описание процесса для агента, а не готовое решение под вашу конкретную ситуацию — трактовку и ответственность за итоговый результат вы оставляете за собой.</p>
    </details>
    <details>
      <summary>Сколько это будет стоить?</summary>
      <p>Пока бесплатно — это этап проверки, насколько формат вообще нужен.</p>
    </details>
  </div>
</section>
```

- [ ] **Step 5: Verify content renders and FAQ starts collapsed (Review Focus item 4)**

Run: `open /Users/alesasycevnik/Work/mozg-buhgalterii/index.html`
Expected: problem/solution text, the four procedure cards (two per row on desktop width) each with a "В разработке" badge, the three numbered steps, and the three FAQ questions are all visible. None of the `<details>` elements has an `open` attribute in the HTML, so on page load every FAQ answer must be hidden — click one question and confirm it expands, then collapses again on a second click.

- [ ] **Step 6: Commit**

```bash
cd /Users/alesasycevnik/Work
git add mozg-buhgalterii/index.html
git commit -m "$(cat <<'EOF'
Fill problem/solution, procedures, how-it-works and FAQ sections

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: Блок призыва к действию (CTA)

**Files:**
- Modify: `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html` (the empty `<div class="wrap">` inside `#cta` and inside `<footer class="site-footer">`, plus a `<script>` before `</body>`)

**Interfaces:**
- Consumes: `.cta-form`, `.btn`, `footer.site-footer` classes and the `#cta` / footer empty `.wrap` divs from Task 1. Destination address `rpw4dzvs8g@privaterelay.appleid.com` (from spec, confirmed by user).
- Produces: `#cta-form` and `#cta-contact` element ids, referenced by this task's own script and by Task 5's verification steps.

- [ ] **Step 1: Fill in the CTA form**

Replace:

```html
<section id="cta" class="section section--alt">
  <div class="wrap"></div>
</section>
```

with:

```html
<section id="cta" class="section section--alt">
  <div class="wrap">
    <h2>Пришлём, когда будет готово</h2>
    <p>Оставьте контакт — напишем, как только первая процедура будет готова к использованию.</p>
    <form class="cta-form" id="cta-form">
      <input type="email" id="cta-contact" name="contact" placeholder="Ваш email" required>
      <button type="submit" class="btn">Пришлите, когда будет готово</button>
    </form>
  </div>
</section>
```

- [ ] **Step 2: Fill in the footer disclaimer**

Replace:

```html
<footer class="site-footer">
  <div class="wrap"></div>
</footer>
```

with:

```html
<footer class="site-footer">
  <div class="wrap">
    <p>Обезличенный проект без готовых консультаций. Реальные данные компаний и контрагентов в материалах не используются.</p>
  </div>
</footer>
```

- [ ] **Step 3: Add the mailto submit handler**

Immediately before `</body>`, add:

```html
<script>
  document.getElementById('cta-form').addEventListener('submit', function (event) {
    event.preventDefault();
    var contact = document.getElementById('cta-contact').value.trim();
    if (!contact) return;
    var subject = encodeURIComponent('Заявка с лендинга «Мозг бухгалтерии»');
    var body = encodeURIComponent('Мой контакт для связи: ' + contact);
    window.location.href = 'mailto:rpw4dzvs8g@privaterelay.appleid.com?subject=' + subject + '&body=' + body;
  });
</script>
```

- [ ] **Step 4: Verify empty/invalid submission is blocked (Review Focus item 1)**

Run: `open /Users/alesasycevnik/Work/mozg-buhgalterii/index.html`, scroll to the CTA form, click "Пришлите, когда будет готово" without typing anything.
Expected: the browser's native validation stops the submit (the `required` + `type="email"` attributes on the input trigger a validation bubble) — no mail client opens and no `mailto:` navigation happens.

- [ ] **Step 5: Verify a valid submission opens the mail client correctly**

Type a test address (e.g. `test@example.com`) into the field and click submit.
Expected: the default mail client opens a new draft addressed to `rpw4dzvs8g@privaterelay.appleid.com`, subject "Заявка с лендинга «Мозг бухгалтерии»", body containing "Мой контакт для связи: test@example.com".

- [ ] **Step 6: Commit**

```bash
cd /Users/alesasycevnik/Work
git add mozg-buhgalterii/index.html
git commit -m "$(cat <<'EOF'
Add CTA form, footer disclaimer and mailto submit handler

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: Финальная проверка

**Files:**
- Modify (only if issues are found): `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html`

**Interfaces:**
- Consumes: the complete page produced by Tasks 1–4.
- Produces: a verified, finished page — no further tasks depend on this one.

- [ ] **Step 1: Verify mobile layout collapses correctly (Review Focus item 5)**

Using the `playwright` MCP tools:
1. `mcp__playwright__browser_navigate` to `file:///Users/alesasycevnik/Work/mozg-buhgalterii/index.html`
2. `mcp__playwright__browser_resize` to width `375`, height `800`
3. `mcp__playwright__browser_take_screenshot`

Expected: the four procedure cards stack into a single column, the CTA form's input and button stack vertically instead of sitting side by side, and there is no horizontal scrollbar. If any of this fails, fix the `@media (max-width: 640px)` block in the `<style>` section and re-screenshot.

- [ ] **Step 2: Verify full-page dark mode with real content**

Using the `playwright` MCP tools on the same page:
1. `mcp__playwright__browser_emulate_media` with `colorScheme: "dark"`
2. `mcp__playwright__browser_take_screenshot`

Expected: all six sections (now with real text, cards, and the form) are readable with sufficient contrast against the dark background — no light-mode-only colors left hardcoded.

- [ ] **Step 3: Verify desktop layout and internal navigation end-to-end**

Using the `playwright` MCP tools:
1. `mcp__playwright__browser_resize` to width `1280`, height `900`
2. `mcp__playwright__browser_snapshot`
3. Click the hero's "Хочу процедуру" button (via the snapshot's element ref)
4. `mcp__playwright__browser_snapshot` again

Expected: the accessibility snapshot confirms the hero button, the four procedure cards, the three FAQ questions, and the CTA form/button are all present with their real Russian copy; after the click, the CTA section is scrolled into view.

- [ ] **Step 4: Proofread copy against the spec's risk checklist**

Read through `/Users/alesasycevnik/Work/mozg-buhgalterii/index.html` and confirm, per `docs/superpowers/specs/2026-09-24-mozg-buhgalterii-landing-design.md` → "Проверка перед сдачей":
- No real company names, contractor names, or figures appear anywhere.
- Every procedure card still says "В разработке".
- The FAQ explicitly states the page is not a consultation.
Fix inline if any of these is missing, then re-run the affected step above.

- [ ] **Step 5: Commit (only if Steps 1–4 required fixes)**

```bash
cd /Users/alesasycevnik/Work
git add mozg-buhgalterii/index.html
git commit -m "$(cat <<'EOF'
Fix responsive/dark-mode/copy issues found in final review

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```
