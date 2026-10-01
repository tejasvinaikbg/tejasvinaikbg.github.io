# PROGRESS — Resume site

| Stage | Status | Started | Done | Review |
|---|---|---|---|---|
| 1 Semantic HTML | 🟡 | 2026-09-30 | | [round 2: complete pending W3C run](reviews/stage-1.md) |
| 2 Tokens & base | ✅ | 2026-09-30 | 2026-10-01 | [complete](reviews/stage-2.md) |
| 3 Layout & responsive | ✅ | 2026-10-01 | 2026-10-01 | [complete vs BRIEF; design gaps listed](reviews/stage-3.md) |
| 4 Print / PDF | ✅ | 2026-10-01 | 2026-10-01 | exported: 2 pages, no split roles |
| 5 Polish | ✅ | 2026-10-01 | 2026-10-01 | [Lighthouse 98/100/100/100](reviews/stage-5.md) |
| 6 Deploy | 🟡 | 2026-10-01 | | live on GitHub Pages; README to write |

## Current stage notes
Phase 0 decisions pending: GitHub link · public status value · photo · PDF file · skills content (see design/PHASE-PLAN.md)

## Blockers
- Editor overwrote styles.css from a stale tab twice — reload before saving
- Stage 1: W3C validator run outstanding (no Java locally — use validator.w3.org/nu)
- Carried over: photo webp files (Stage 3); status icon must switch via CSS per data-status (Stage 3)
- Real content: experience + education in (2026-09-30); skills updated 2026-09-30; summary still to verify

## Log
- 2026-09-30 — first draft of index.html reviewed informally: no <h1>, headings skip h2, photo/status/head incomplete. 14 html-validate errors.
- 2026-09-30 — Claude restructured index.html markup on request (aside/content, status attr, srcset, contact list, dl skills, ol/article roles, <time>, education stub). html-validate passes. Added .htmlvalidate.json (lowercase doctype, self-closing voids to match Prettier). Stage 1 still open: real content, photo/icon files in assets/.
- 2026-09-30 — Added Naraci, UPapp, Teslon roles + education. Role classes flattened to BEM (role__company, role__meta, role__project as h4, role__tech). Fixed Napses datetime values and "TTech" typo. html-validate passes.
- 2026-09-30 — Skills replaced with real list: 8 groups (Core Stack, Languages, Frontend, Backend, Databases, Architecture, Testing & Analytics, Cloud & Tools) as <dl> + <ul>.
- 2026-09-30 — Stage 1 review written (reviews/stage-1.md). Design icons extracted into assets/ (9 SVGs). Structure passes; blocked on links, photo, content, W3C.
- 2026-09-30 — Stage 1 round 2: LinkedIn fixed, GitHub confirmed (0 public repos), notice icon added inline. User kept content/title/photo as is. Only W3C run outstanding.
- 2026-09-30 — Contact icons (email, LinkedIn, GitHub) switched from <img> to inline SVG (.contact__icon, aria-hidden, currentColor).
- 2026-09-30 — Stage 2 step 1: :root tokens added (fonts, type scale, spacing 1–9, shape, layout). Open: system-dark block has light values; dark --link-hover too dark; font-family Inter on :root.
- 2026-09-30 — Stage 2 step 2 fixes: system-dark block now has dark values; dark --link-hover #8fe0cc (12.4:1 on --bg) in both dark blocks. Still to add: color-scheme: light dark.
- 2026-09-30 — Stage 2 step 2 done: color-scheme light dark on :root, dark/light on [data-theme] blocks.
- 2026-09-30 — Stage 2 step 3 done: Google Fonts preconnects + stylesheet (Geist Mono 400/500, Schibsted Grotesk 400–700, display=swap) before styles.css.
- 2026-09-30 — Stage 2 steps 4–5: :focus-visible, measures on .profile__summary/.role__bullets, .label + .label--caps (h2s, dts). Verified on localhost:8765: fonts load, dark mode swaps, Tab focus ring 2px accent. Reset side-effects fixed same day (see below).
- 2026-09-30 — Reset cleanup: selector trimmed to *, ::before, ::after; svg no longer display:block; lists default list-style none, .role__bullets keep disc + 1.5rem padding; .status and .contact a are inline-flex (icon beside text); 2-space indent. Verified in browser.
- 2026-10-01 — Re-applied reset fixes (lost to stale editor save); added h1/h3/h4/.profile__title sizes from type scale. Stage 2 reviewed: complete. Lighthouse baseline (localhost, mobile): Perf 89 · A11y 100 · BP 96 · SEO 63.
- 2026-10-01 — Stage 3: avatar done (round container + overflow clip, ring via outline, grid-stacked initials/photo, --photo token scales at 900px). Initials --accent on --tint (5.2/7.4:1). <img> commented out until photo files exist.
- 2026-10-01 — Stage 3 layout: body --pad, .page grid (max 1248, centred), 2 cols at 56.25rem, .profile__intro wrapper (gap 12) + .profile gap 40, .content gap 72. Reviewed: mobile/320/wide OK; issues — content column only 352px at 1024, .role__tech 93ch at 1440, sticky sidebar (1838px tall) hides its bottom on tall screens, --tracking-intials undefined.
- 2026-10-01 — Layout fixes: fluid --pad (40/24/56 → 96) and --gap (64 → 96) via clamp; breakpoint moved to 64rem (content col 492px at 1040, 782px at 1440); .role__project/.role__tech capped at --measure (longest line 61ch at every width); sticky commented out by user; --tracking-initials token fixed; layout rules + media queries gathered under a layout section.
- 2026-10-01 — Stage 3: name clamp (44→76px), status 4 states via data-status (dot ::before, icon via CSS mask, hidden leaves no gap), timeline (rail grid, border line, square markers, years) + container query on .content (≥40rem → 112px rail, 32px year). Fixed: marker offset sign + inset shadow, inline svg → mask span, TAO rail year 2026→2024, container rules moved to layout. Verified 320–1920: no overflow; rail narrow at 1040 (492px col) and wide at 1023 (921px col).
- 2026-10-01 — Contact rows (full-width flex, 48px, ink→accent hover, rules), button (ink/bg inverted, 48px, 4px radius, accent hover, download icon), section headers (<header class="section-head">: icon · label · fluid rule via h2::after · range) for Experience + Education. Cleaned: .contact a removed from shared inline-flex rule; duplicate .button rules merged. Verified 320–1440: no overflow, rows 48px, header on one line.
- 2026-10-01 — Skills layout: label above, skills as one comma-separated inline line (ul kept for semantics, li inline + ::after ", "); 4px label→skills, 16px between groups. Skills block 589px (was ~1000); sidebar 1627px.
- 2026-10-01 — Stage 3 reviewed: all 4 BRIEF criteria pass (states, 64rem breakpoint, no overflow 320–1920 / 400% zoom, container query). Design gaps before Stage 4: no spacing inside roles/bullets, secondary text not muted/mono, literal sizes to tokenise.
- 2026-10-01 — Stage 3 design gaps fixed: role spacing (head group, 12px gaps, 8px bullets, hollow markers), muted/mono secondary text, weights 500, education on the rail, literal sizes tokenised. See reviews/stage-3.md follow-up.
- 2026-10-01 — Stage 4: @media print + @page A4 (14/16/16mm, footer name + page x/y). Header grid avatar | name/title/status | contacts; skills 2-col table; roles year | content with rule above, break-inside: avoid; screen-only bits hidden. Exported via Chrome to tejasvi-naik-resume.pdf — exactly 2 pages, no phone number. Contacts now show URLs; GitHub → tejasvinaikbg. Tablet: summary widened to --measure (64ch).
- 2026-10-01 — Repo: tejasvinaikbg.github.io (public). Tejasvi_Naik.pdf (has phone) and working notes git-ignored; repo-local git identity set to personal email.
- 2026-10-01 — Pushed to github.com/tejasvinaikbg/tejasvinaikbg.github.io (public). GitHub Pages built from main: https://tejasvinaikbg.github.io/ — 200 over HTTPS (HSTS), CSS + fonts load, PDF link 200. Stage 6 left: README (what, how to run, Lighthouse scores, what you learned).
- 2026-10-01 — Stage 5: noindex removed (user decision), description/canonical, OG + twitter tags, og-image 1200x630, favicon.ico/svg/apple-touch-icon, theme-color. Unused icon SVGs + duplicate plan removed. Live Lighthouse mobile: 98 / 100 / 100 / 100.
