# Stage 3 review — Layout and responsive

**Date:** 2026-10-01 · **Files:** `styles.css` (504 lines), `index.html` · **Checked against:** BRIEF.md Stage 3, design/PHASE-PLAN.md Phase 3, `design/desktop-light.png`, `mobile.png`, `status-states.png`

**Checks run** (on `http://localhost:8765`)
| Check | Result |
|---|---|
| `npx html-validate index.html` | ✅ 0 errors |
| CSS parses (css-tree) | ✅ no errors |
| `float`, fixed `width`/`height`, `!important`, ID selectors | ✅ none. `position` is used only for the timeline marker (`relative`/`absolute`) and the commented-out sticky rule |
| Horizontal overflow at 320 · 375 · 768 · 1023 · 1040 · 1280 · 1440 · 1920 px | ✅ none. 0 elements past the right edge at any width |
| 400% zoom (1280 px ÷ 4 = 320 CSS px) | ✅ same as the 320 px row: one column, no overflow |
| All four `data-status` values (switched live) | ✅ see table below |
| Container query (rail width) | ✅ follows the content column, not the window (see table below) |

**Status states**
| `data-status` | display | dot | icon | title → summary gap |
|---|---|---|---|---|
| available | flex | `--ok` `#1f8a4c` | status-available.svg | 49 px |
| serving | flex | `--warn` `#b7791f` | status-serving.svg | 49 px |
| notice | flex | `--muted` `#4e5761` | status-notice.svg | 49 px |
| hidden | **none** | — | — | **12 px** (exactly one `gap`, so nothing is left behind) |

**Layout by width**
| Window | Columns | Content column | Timeline rail |
|---|---|---|---|
| 320 | 1 | 272 | 60 |
| 768 | 1 | 691 | 112 |
| 1023 | 1 | 921 | 112 |
| 1040 | **2** | **492** | **60** ← a wider window but a narrower rail: the container query at work |
| 1440 | 2 | 782 | 112 |
| 1920 | 2 | 772 (page capped at 1248, centred) | 112 |

---

## 1. ✅ What's right

- **Mobile first.**
  - Without a media query, `.page` (`:178`) is a one-column grid.
  - `--pad` and `--gap` are fluid `clamp()` tokens: 40/24/56 px padding on phones, growing to 96 px.
  - Two columns start only at `@media (width >= 64rem)` (`:206`).
- **The breakpoint was chosen from the content.** It moved from 900 to 1024 px after measuring that the content column was only 352 px wide at 1024. It now gets ≥ 492 px when two columns start.
- **Grid and Flexbox only.** The page uses grid with `minmax(0, …)` on both tracks (`:212`), so long words can't widen the page. The sidebar, content, sections, contact rows, button and section headers use flex. The timeline rows use grid.
- **No fixed widths.** Sizes come from `max-inline-size`, `minmax()`, `aspect-ratio` and tokens. The sidebar shrinks freely and only stops growing at 380 px.
- **Container query** (brief requirement):
  - `.content { container-type: inline-size }` (`:198`)
  - `@container (width >= 40rem)` (`:224`) changes only `--rail` and `--year`
  - Measured: 60 px rail at a 1040 px window, 112 px at 1023 px. A media query couldn't produce that.
- **Status.**
  - One attribute switches the dot (`::before` + attribute selectors) and the icon (CSS `mask-image`).
  - `hidden` uses `display: none` (`:284`) and leaves no gap, because the parent uses `gap` rather than margins.
  - The text always states the status, and the icon is `aria-hidden`.
- **Name:** `clamp(44px, 1.5rem + 3vw, 76px)` (`:236`) grows smoothly. The `rem` term keeps it responding to zoom, and `text-wrap: balance` avoids a lone word on the last line.
- **Avatar:**
  - The container is rounded and clips its children (`overflow: hidden`).
  - The ring is drawn with `outline` and the focus tokens.
  - The initials and photo are stacked in one grid cell.
  - The `--photo` token resizes everything at the breakpoint.
- **Timeline:**
  - Each role is a rail + article grid (`:333`).
  - The line is the article's `border-inline-start`, and `padding-block-end` keeps it unbroken between roles.
  - The 11 px marker sits exactly on the line (`:345`): filled for the current role, hollow with an inset shadow for past roles.
  - The rail years are `aria-hidden`, so screen readers don't hear the dates twice.
- **Contact rows** (`:383`): full-width flex links, each ≥ 48 px (`--hit`), with `--rule` dividers on the list and items. Ink, turning accent on hover.
- **Button** (`:407`): 48 px tall, inverted colours with `--ink`/`--bg` (so it flips in dark mode), `--accent` + `--accent-ink` on hover, and `align-self: flex-start` so it doesn't stretch.
- **Section headers:** icon · label · a line that fills the space, drawn with `h2::after { flex: 1 }` (`:483`) and no extra markup · range. They stay on one line from 320 px up.
- **Skills:** the `<ul>` is kept for meaning and displayed as a comma-separated line (`:447–453`). Groups are 16 px apart.
- **CSS order:** tokens → reset → base → **layout** → components → utilities. All the breakpoint and container rules are in the layout section.

## 2. ❌ What's wrong

The four BRIEF Stage 3 criteria pass. These are gaps against the **plan's** done-when ("timeline matches `desktop-light.png`") and against Stage 2's "all sizes as tokens":

1. **No spacing inside a role.**
   - *Problem:* the title, company, meta, project heading, bullets and tech line touch, with **0 px** between them. The bullets are also 0 px apart.
   - *Why it matters:* the design has 14 px between groups inside a role and 8 px between bullets. Without that spacing, the timeline reads as one dense block (compare the 1440 px screenshot with `desktop-light.png`).
   - *Where:* `.role article` (`:337`) and `.role__bullets` (`:252`).
2. **Secondary text is in `--ink` instead of `--muted`.**
   - *Problem:* the summary, the company/meta lines and the tech lines all use the full text colour. The design uses `--muted` for them, and also mono for the tech line and the location.
   - *Why it matters:* the design's hierarchy depends on these lines being quieter than the titles and bullets. At the moment everything has the same weight.
   - *Where:* `.profile__summary`, `.profile__location`, `.role__company`, `.role__meta`, `.role__tech`.
3. **Sizes written straight into rules** (Stage 2 says *all* font sizes and spacing should be tokens):
   - `0.75rem` font-size on `.role__end` (`:375`)
   - `0.95` and `-0.035em` on the name (`:238–239`)
   - the marker's `0.375rem` and `0.6875rem` (`:350`)
   - icon sizes `1rem` and `1.25rem` (`:289`, `:403`)

## 3. 🔧 How to make it better (most important first)

1. **Spacing inside a role:**
   - Make the article a flex column with `gap: var(--space-3)`.
   - Give `.role__bullets` `display: flex; flex-direction: column; gap: var(--space-2)`.

   These two rules make the biggest visual difference.
2. **Quieter secondary text:** set `color: var(--muted)` on the summary, company, meta and tech lines. Add `font-family: var(--font-mono); font-size: var(--text-label)` to the tech line and the location, to match the design.
3. **Bullet markers:** the design uses hollow circles. Change to `list-style: circle`, or style `::marker` with `color: var(--muted)`.
4. **Status weight:** the design shows the status text at 500 (`--weight-medium`). It's currently 400.
5. **Year weight:** the design shows 500. It's currently 400.
6. **Education** is plain text, but the design puts it on the same rail grid with "2017" as a large year. Reuse `.role` there, or leave it as a simpler section by choice.
7. **Tokens:** move the values listed under ❌ 3 into `:root` (`--marker`, `--icon`, `--icon-sm`, `--text-meta`, `--leading-name`, `--tracking-name`).
8. **Safari list semantics:** add `role="list"` to the skills `<ul>`s, which are styled inline.

## 4. ⚡ Optimisation

- **The sidebar is 1627 px tall,** so the sticky rule should stay off. If you want it later, use `@media (width >= 64rem) and (height >= 110rem)`, which in practice means don't make it sticky.
- **Mask icons** are fetched as 3 separate SVG files, about 400 bytes each, and only 1 shows at a time. That's fine. If you ever want zero requests, inline them as `data:` URLs in the CSS.
- **`.skills__list dd + dt`** is 3 levels deep. CLAUDE.md allows a maximum nesting depth of 2. It's a sibling combinator, not nesting, so it's acceptable. Moving the margin to a class on each `<dt>` would avoid the question.
- **The photo is still commented out.** When you add it, use `sizes="(width >= 64rem) 160px, 120px"`, which matches the new breakpoint.

## 5. Verdict

**Stage 3: complete against BRIEF.md.** All four criteria pass:
- [x] All four `data-status` states render correctly, and `hidden` leaves no gap
- [x] Mobile first, with two columns from a breakpoint chosen by the content (64rem)
- [x] Grid and Flexbox only, no fixed widths, no horizontal scroll at 320 px, reflows at 400% zoom
- [x] At least one container query (the timeline rail and year size)

**Not yet matching the design (the plan's done-when):** spacing inside roles and muted secondary text (❌ 1–2). Fix these before Stage 4, because the print stylesheet starts from the screen styles.
Carried over: the photo files, tokens for the literal sizes (❌ 3), and the `noindex`/SEO decision (Stage 5).

---
## Follow-up — 2026-10-01: design gaps fixed
- ❌ 1 → `.role article` flex column, gap 12px; new `<header class="role__head">` keeps title/company/meta 4px apart; bullets 8px apart, hollow `circle` markers in `--muted`
- ❌ 2 → summary, company, meta, tech, location, education meta in `--muted`; location and tech reuse the `.label` utility (mono, 13px)
- ❌ 3 → new tokens `--text-meta`, `--leading-name`, `--tracking-name`, `--icon`, `--icon-sm`, `--marker`, `--hairline`; only token overrides inside `@media`/`@container` remain as literals
- Status and timeline years at weight 500; Education on the same rail (year moved out of the meta line into `.education__year`, still a `<time>`), widened by the same container query
- `sizes` on the commented photo updated to `(width >= 64rem) 160px, 120px`
- Not done: `role="list"` on skills lists — html-validate flags it as redundant; Safari/VoiceOver list announcement left as a known limitation
- Re-checked: html-validate 0 errors · CSS parses · no overflow at 320/1040/1440 · education year fits its rail at every width
