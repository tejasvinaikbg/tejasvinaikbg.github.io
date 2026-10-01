# Stage 2 review — Design tokens and base styles

**Date:** 2026-10-01 · **Files:** `styles.css` (205 lines), `index.html` `<head>` · **Checked against:** BRIEF.md Stage 2, design/PHASE-PLAN.md Phase 2, design/tokens.md

**Before the review:** your editor overwrote the reset fixes from 2026-09-30 (`styles.css` was saved from a stale tab). I re-applied them, and added heading sizes from the type scale, because `<h1>` was still the browser default of 32 px. **Reload `styles.css` in your editor before your next save.**

**Checks run** (on `http://localhost:8765` via `.claude/launch.json`)
| Check | Result |
|---|---|
| CSS parses (css-tree) | ✅ no errors |
| Hex colours outside `:root` blocks | ✅ none. Every hex is on lines 2–100 |
| Hard-coded px/rem/ch outside `:root` | ✅ none (one `0.08em` letter-spacing, see ⚡) |
| `!important` / ID selectors | ✅ none |
| Fonts load | ✅ Schibsted Grotesk 400/600/700 and Geist Mono 400/500 loaded, with `display=swap` |
| Dark mode | ✅ emulating light gives `--bg` `#f5f6f7`; emulating dark gives `#0e1114`. `color-scheme` follows |
| Focus on every link (Tab ×4) | ✅ email, LinkedIn, GitHub and Download PDF all show a 2 px solid accent outline |
| Contrast, light / dark | ✅ ink 17.1 / 15.7 · muted 6.8 / 7.5 · accent 5.9 / 10.0 · hover 8.0 / 12.4 (all ≥ 4.5 : 1) |
| Overflow at 320 px (dark) | ✅ none |
| `npx html-validate index.html` | ✅ 0 errors |
| Lighthouse (mobile, localhost baseline) | Perf **89** · A11y **100** · Best Practices **96** · SEO **63**. This is Stage 5's target, recorded here as a baseline |

---

## 1. ✅ What's right

- **All the tokens live on `:root`** (`styles.css:2–65`):
  - the 10 colour tokens
  - two font stacks with system fallbacks (`:16`, `:18`)
  - 4 weights
  - a 7-step type scale in `rem` (`:25–31`)
  - the 9-step spacing scale matching the design (4 → 96 px)
  - shape, focus and layout tokens

  Each one has a comment giving its px value. That makes them easy to check against `tokens.md`.
- **The dark-theme setup is correct** (`:68–100`). `:root:not([data-theme="light"])` inside the media query means the OS preference applies unless the user has chosen light. `color-scheme` is set on each theme block, so scrollbars and form controls follow the choice.
- **Hover changes in the right direction for each theme:** darker on light (`#08564a`), lighter on dark (`#8fe0cc`). Both have more contrast than the normal link colour.
- **Fonts** (`index.html:12–17`):
  - two `preconnect`s, and the gstatic one has `crossorigin`
  - only the weights you use
  - `display=swap`
  - the fonts load before `styles.css`
- **Reset** (`:102–124`): `box-sizing` on everything and `::before`/`::after`. `img` is block-level but `svg` stays inline. List markers are removed by default and added back only on `.role__bullets` (`:174`).
- **Base styles** only read tokens: the body font, size, leading and colours (`:127`), headings from the type scale (`:135–149`), links (`:151`), and a `:focus-visible` ring built from `--focus-width` and `--focus-offset` (`:160`).
- **Line length:** the summary is limited to 42ch (`:170`) and the bullets to 64ch (`:174`), both within the brief's 70ch.
- **The label utility** (`:195–205`) is one `.label` class plus a `.label--caps` modifier, used on 3 `<h2>`s and 8 `<dt>`s. This matches the design: headings are uppercase, skill group names aren't.
- **The CSS follows the CLAUDE.md order:** tokens → reset → base → components → utilities, with a comment header for each section.
- **Accessibility scores 100** in Lighthouse.

## 2. ❌ What's wrong

1. **The `tejasvi-naik-resume.pdf` and photo files still return 404** (Lighthouse `errors-in-console`).
   - *Why it matters:* it costs Best Practices points. The broken image icon also shows at the top of the page.
   - *Where:* `index.html:20–32`, `:75`.
   - These were carried over from Stage 1 (photo → Stage 3, PDF → Stage 4), so they don't block Stage 2.
2. **SEO scores 63 because of `noindex`** (Lighthouse `is-crawlable`).
   - *Why it matters:* Stage 5 needs ≥ 95.
   - *Where:* `index.html:11`.
   - You made this choice on purpose, so it doesn't block Stage 2. But it can't stay as it is if the Stage 5 criterion is going to pass. Remove `noindex`, or record SEO as an accepted exception in PROGRESS.md.

Nothing else is wrong against the Stage 2 criteria.

## 3. 🔧 How to make it better (most important first)

1. **Stop your editor overwriting fixes:** reload `styles.css` from disk before saving, or turn on your editor's auto-reload for files changed outside it. It has happened twice now.
2. **Choose the job title shown under your name** (`index.html:36`). It says "Senior Software Engineer", but the PDF headline, `<title>` and summary say "Senior Full Stack Engineer". You chose to keep it in Stage 1, but it's now shown at 22 px, so the mismatch stands out.
3. **Watch line length in Stage 3.** Without a layout yet, the `.role__tech` lines and `<h4>`s span the full window. Once the two-column grid is in, check that no body text runs past about 70ch.
4. **Token for the letter-spacing:** move `0.08em` (`:203`) to `--tracking-label` so that `:root` really holds every value.

## 4. ⚡ Optimisation

- **Performance 89 on localhost** is lower than the real site will score:
  - Python's `http.server` doesn't compress files or set cache headers (`uses-text-compression`, `uses-long-cache-ttl`). GitHub Pages and Netlify do both.
  - The Google Fonts CSS blocks rendering. If Stage 5 is still below 95, preload the two most-used font files, or host the fonts yourself.
- **Unused tokens:** `--radius`, `--text-name`, `--text-year`, `--rail`, `--photo`, `--sidebar-max`, `--content-max` and `--gap` are for Stage 3. Check at the end that each one is used, and delete any that aren't.
- **`--weight-medium`** (500 in Schibsted Grotesk) is loaded but only used by `.label--caps`, which is set in Geist Mono. If nothing in Schibsted Grotesk ends up using 500, drop it from the font URL to save one download.

## 5. Verdict

**Stage 2: complete.** All three acceptance criteria pass:
- [x] All colours, font sizes and spacing are custom properties on `:root`
- [x] Type scale in `rem`; body line length ≤ 70ch; fonts use `swap` with fallbacks
- [x] Focus is visible on every link

Carried forward, not blocking: photo (Stage 3), PDF (Stage 4), the `noindex` vs SEO decision (Stage 5).
