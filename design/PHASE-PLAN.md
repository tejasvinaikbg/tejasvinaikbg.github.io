# Phase plan — Resume site (built from the Claude Design spec)

Reference images in this folder: `desktop-light.png` · `desktop-dark.png` · `mobile.png` · `print.png` · `status-states.png` · `tokens.png`. Values: `tokens.md`.
Phases map onto the six stages in `BRIEF.md`. **You write every line; Claude Code tracks and reviews.**

---

## Phase 0 — Before you write code (30 min)

| Decision | Recommendation |
|---|---|
| GitHub link | the design shows `github.com/your-handle` — **replace with your real profile or remove the line** |
| Availability on the public page | decide `notice` / `hidden` (see the visibility note from chat) |
| Photo | the design shows the initials fallback. If you want a photo: square headshot, export WebP at 160, 240, 320, 480 px |
| "Download PDF" | plain HTML can't print without JS — **export the PDF from your own print stylesheet in Phase 4 and link the file** (`<a href="/tejasvi-naik-resume.pdf" download>`) |
| Skills content | check against your PDF before typing — see "Content mismatches" at the bottom |

Repo: `index.html` · `styles.css` · `assets/` (photo, favicon, og image) · `design/` (this folder) · `BRIEF.md` · `CLAUDE.md` · `PROGRESS.md`.

---

## Phase 1 — Semantic HTML, no CSS  → BRIEF Stage 1

**Page outline** (what each part of the design becomes):
```
<body>
  <main class="page">
    <aside class="profile">                       ← left column on desktop, top on mobile
      avatar (img, or initials fallback)
      <p class="profile__location">Bengaluru, India</p>
      <h1 class="profile__name">Tejasvi Naik B G</h1>
      <p class="profile__title">Senior Full Stack Engineer</p>
      <p class="status" data-status="notice">…icon… Notice period: 15 days</p>
      <p class="profile__summary">…</p>
      <ul class="contact">  mailto · LinkedIn · GitHub  </ul>
      <a class="button" href="…pdf" download>Download PDF</a>
      <section aria-labelledby="skills-h"> <h2 id="skills-h">Skills</h2>
        <dl class="skills"> <dt>Languages</dt><dd>…</dd> … </dl>
      </section>
    </aside>

    <div class="content">                          ← right column
      <section class="experience" aria-labelledby="exp-h">
        <header> <h2 id="exp-h">Experience</h2> <p>2018 — 2026</p> </header>
        <ol class="timeline">
          <li class="role role--current">
            <p class="role__years"><time datetime="2024-03">2024</time> <span>Present</span></p>
            <article>
              <h3>Senior Software Engineer</h3>
              <p class="role__meta">TAO Digital India · Client: Condé Nast · <time datetime="2024-03">Mar 2024</time> — Present</p>
              <ul class="role__bullets"> … </ul>
              <p class="role__stack">Next.js / React / TypeScript / Node.js / GraphQL / AWS</p>
            </article>
          </li>
          … four more roles
        </ol>
      </section>
      <section class="education" aria-labelledby="edu-h"> … </section>
    </div>
  </main>
</body>
```
**Why these elements**
- `<ol>` for the timeline — the roles *are* an ordered sequence (newest first)
- `<dl>` for skills — each row is a term (area) with its description (items)
- `<time datetime>` on every date — machine-readable
- icons are inline `<svg aria-hidden="true">` — the text next to them carries the meaning
- one `<h1>`; `<h2>` per section; `<h3>` per role — check with a headings outline tool

**Done when:** it reads correctly top to bottom with CSS off · validator passes · every link works · `<head>` has lang, charset, viewport, title, description.

---

## Phase 2 — Tokens and base styles  → BRIEF Stage 2

1. `:root` custom properties from `tokens.md` — the six colours + `--ok` `--warn` `--tint`, font stacks, type scale, spacing scale (in `rem`: 4 px = 0.25rem … 96 px = 6rem), radii, `--rail`, `--gap`, `--pad`
2. `@media (prefers-color-scheme: dark)` redefines only the colour tokens; add `color-scheme: light dark`
3. Fonts: Google Fonts link with `display=swap`, two `preconnect`s, **only the weights you use** (Grotesk 400/500/600/700, Mono 400/500) — every weight is another download
4. Reset + base: `box-sizing`, body font/colour/line-height 1.55, links in `--accent`, `:focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; }`
5. Label style (mono, 0.8125rem, `--muted`) as one reusable class for SKILLS / EXPERIENCE / skill areas

**Done when:** colours come only from variables (search `#` in the CSS — only `:root` should have hex values) · switching the OS to dark mode swaps everything · focus is visible on every link.

---

## Phase 3 — Layout, timeline and components  → BRIEF Stage 3

**3a. Page grid — mobile first**
- default: one column, `padding: var(--pad)` = 40/24/56 px, sections stacked with `--gap` 64 px
- `@media (width >= 56.25rem)` (900 px): `grid-template-columns: minmax(0, 380px) minmax(0, 1fr)`, gap 96 px, padding 96 px, `max-width: 1248px; margin-inline: auto`
- **Sticky sidebar — careful:** the sidebar (photo + name + contacts + skills) is taller than most laptop screens, so `position: sticky` would hide its bottom. Make it sticky only when it fits: `@media (width >= 56.25rem) and (height >= 62rem) { .profile { position: sticky; top: 6rem; align-self: start; } }` — or drop it

**3b. Name**
- `font-size: clamp(2.75rem, 1.5rem + 3vw, 4.75rem)` so it grows between the mobile and desktop sizes (the design only shows the two ends); `line-height: 1`; tight letter-spacing; let it wrap as in the design

**3c. Avatar**
- 120 px → 160 px at 900 px; `border-radius: 50%`; ring = `outline: 2px solid var(--accent); outline-offset: 3px`
- initials fallback: the avatar box has `background: var(--tint)` and the initials as text; the `<img>` sits on top and covers it — if the image is missing, remove the `<img>` and the initials show

**3d. Availability status — attribute selectors**
- `.status::before` = the 8 px dot (so no extra markup); colour per state: `.status[data-status="available"]::before { background: var(--ok) }`, `serving` → `--warn`, `notice` → `--muted`
- `.status[data-status="hidden"] { display: none; }` — no gap left because the parent uses `gap`, not margins
- icon: put the one matching `<svg>` in the markup when you change state (simplest), keep `aria-hidden`
- text always states the status — the dot is decoration

**3e. Timeline** ⭐ the design's personality
- each `.role` is a grid: `grid-template-columns: var(--rail) minmax(0, 1fr)` — rail 60 px narrow, 112 px wide
- the vertical rule: a `::before` on `.timeline` (1 px `--rule`) positioned at the rail edge, or `border-inline-start` on the article column
- marker: a 10 px square `::before` on each `article` — **filled** `--accent` for `.role--current`, **hollow** (1 px border) for past roles
- year: Geist Mono 2rem `--accent`; end year below in `--muted`, small
- bullets: custom hollow circle markers via `::marker` or `list-style`, `max-width: 64ch`; stack line in mono `--muted`
- **Container query (BRIEF requirement):** `.content { container-type: inline-size; }` then `@container (width >= 40rem) { .role { --rail: 7rem; } }` — the rail widens by the *column's* width, not the screen's

**3f. Contact list and button**
- each contact row is a full-width link with the icon; `min-height: 3rem` (48 px targets); dividers with `border-block` in `--rule`
- button: `--ink` background, `--bg` text, 4 px radius, 48 px tall; hover and focus states

**3g. Section headers**
- icon + mono label + a rule that fills the remaining width + the range at the end: `display: flex; align-items: center; gap`, with the rule as a flex item `flex: 1; height: 1px; background: var(--rule)`

**Done when:** no horizontal scroll at 320 px · reflows at 400% zoom · all four `data-status` values render (switch them in DevTools) · timeline matches `desktop-light.png` · dark matches `desktop-dark.png`.

---

## Phase 4 — Print = your PDF  → BRIEF Stage 4

Compare with `print.png` — the print layout is **different** from the screen layout (a header across the top, contacts on the right, skills as a two-column table, no timeline rail).
- `@page { size: A4; margin: 14mm 16mm 16mm; }`
- `@media print`: black text on white, no backgrounds; hide the button, the status dot and icons (`display: none`); photo 28 mm or hidden
- header: `display: grid; grid-template-columns: auto 1fr auto` — avatar | name/title/status | contacts (mono, right-aligned)
- skills: the `<dl>` as a 2-column grid (`grid-template-columns: 7rem 1fr`)
- roles: `break-inside: avoid` so no role splits across pages; title and company on one line (`Senior Software Engineer — TAO Digital India`)
- "Experience (continued)" on page 2 isn't possible with pure CSS flow — **drop it**, or accept the plain flow
- footer "Tejasvi Naik B G — Resume · 1 / 2": `@page { @bottom-left { content: "Tejasvi Naik B G — Resume"; } @bottom-right { content: counter(page) " / " counter(pages); } }` — **supported in recent Chrome; other browsers ignore it**, which is fine because you'll export the PDF from Chrome
- links: in print the contacts already show the full URLs as text, so no `a::after` needed

**Done when:** Chrome → Print → Save as PDF gives **exactly 2 pages**, no split roles, readable in black and white · save that file as the one the "Download PDF" link points to.

---

## Phase 5 — Polish  → BRIEF Stage 5
- photo (if used): `srcset` 160/240/320/480 WebP, `sizes="(width >= 56.25rem) 160px, 120px"`, `width`/`height`, **no** `loading="lazy"`, `fetchpriority="high"`
- Open Graph: `og:title`, `og:description`, `og:image` (1200×630 — a simple card with your name and title), `og:url`
- favicon (an SVG "TN" in the accent colour works well)
- `noindex` decision
- Lighthouse mobile ≥ 95 in all four categories; check the font weights are the only large requests

## Phase 6 — Deploy  → BRIEF Stage 6
- GitHub Pages (Settings → Pages → deploy from `main`) or Netlify drag-and-drop
- README: what it is, the live link, Lighthouse screenshot, "built by hand from a design spec: no framework, no JS", and what you learned

---

## ⚠️ Content mismatches to resolve before typing (design vs your current PDF)

| Place | Design says | Your PDF says | Decide |
|---|---|---|---|
| Contact | `github.com/your-handle` | no GitHub | real link or remove |
| Skills → Backend | Node.js, Express.js, **API design, system design** | Node.js, Express.js | your rules include System Design in skills — keep that; "API design" is new — keep only if you're happy to claim it |
| Skills → Frontend | …Redux Toolkit… | **Redux**, Redux Toolkit… | the design dropped plain Redux |
| Skills → label | Testing | **Testing & Analytics** | Snowplow is analytics, not testing — the PDF's label is more accurate |
| Napses stack line | …PostgreSQL / AWS S3 | …Express.js, PostgreSQL, AWS (S3), **Tailwind CSS** | fine to shorten, but keep it consistent across versions |

**The public page and the PDF should say the same thing** — recruiters sometimes compare them.

---

## Review checkpoints
After each phase: Claude Code → `review stage N` → push → send the public repo link here for a second review (✅ right · ❌ wrong · 🔧 better · ⚡ optimise).
