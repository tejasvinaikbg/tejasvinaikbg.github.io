# Stage 1 review — Semantic HTML (round 2)

**Date:** 2026-09-30 · **File:** `index.html` · **Checked against:** BRIEF.md Stage 1, design/PHASE-PLAN.md Phase 1, resume PDF
Round 1 findings: LinkedIn fixed; GitHub confirmed real (account `tejasvinaik`, 0 public repos); content, title and photo left as they are, by your decision. Notice icon added inline (on request).

**Checks run**
| Check | Result |
|---|---|
| `npx html-validate index.html` | ✅ 0 errors |
| W3C Nu validator | ⚠️ still **not run**. There's no Java runtime on this Mac. Upload the file at https://validator.w3.org/nu/ |
| Heading outline | ✅ 1 × h1 → h2 × 3 → h3 per role → h4 per project |
| Overflow at 320 px | ✅ none |
| Links | ✅ GitHub 200 · LinkedIn 999 (LinkedIn's normal answer to scripts like mine, so the check is inconclusive; the URL format is correct) · mailto ok |

## 1. ✅ What's right
- Everything listed in round 1 still holds: one `<h1>` for your name, headings in order, sections labelled with `aria-labelledby`, `<ol>` → `<article>` roles, all 10 `<time datetime>` values correct, a `<dl>` + `<ul>` skills list, a complete `<head>`, flat BEM class names, and no phone number or address.
- **LinkedIn** now points to `https://www.linkedin.com/in/tejasvinaikbg/`, which matches the PDF.
- **Status line:** it has `data-status="notice"`, the icon is inline with `aria-hidden="true"`, and the text states the status on its own. This meets the brief's rule that the icon is never the only signal. The icon uses `stroke="currentColor"`, so it will follow `--muted` and dark mode.

## 2. ❌ What's wrong
Nothing blocks the structure. There are two open items:
1. **The W3C validator hasn't been run** (Stage 1 criterion 5). html-validate passes, but the brief names the W3C validator specifically.
2. **The photo files are missing** (`assets/photo-*.webp`). The markup is correct and the initials fallback works in the meantime. You chose to leave this for now, so it's carried over to Stage 3, where the avatar gets styled.

## 3. 🔧 How to make it better
1. **Run the W3C validator** and write the result in PROGRESS.md.
2. **The status icon breaks the "switch by one attribute" rule.**
   - The notice icon is in the HTML, so changing `data-status` to `serving` would change the colour but still show the notice icon.
   - Fix it in Stage 3 by having CSS pick the icon from the attribute, e.g. `.status[data-status="serving"] .status__icon { mask-image: url(assets/status-serving.svg) }`. Alternatively, put all three icons in the HTML and let CSS show only the one that matches.
3. **`target="_blank"` has been added back** on LinkedIn and GitHub. It's allowed, but it's an accessibility smell. If you keep it, consider adding "(opens in new tab)" as visually hidden text.
4. **Your GitHub has 0 public repos.** A recruiter clicking it sees an empty profile. Consider hiding it until you have a repo to show. This site's own repo, once it's public in Stage 6, is a good first one.
5. **Contact icons are still `<img>`,** so they'll always be black: no hover colour, and wrong in dark mode. Change them to inline SVG like the status icon before Stage 5.

## 4. ⚡ Optimisation
- **`noindex`** conflicts with Stage 5's target of SEO ≥ 95. Decide whether you want the page in search results before you get there.
- **Inline SVGs** cost about 400 bytes each and save a network request each. Keep using them for all icons.

## 5. Verdict
**Stage 1: complete on every check I could run.** One criterion is still unverified: the **W3C validator** run. Upload the file at https://validator.w3.org/nu/. If it passes, tick Stage 1 ✅ in PROGRESS.md.
Carried over: photo files (to Stage 3) · contact icons as inline SVG (before Stage 5) · Download PDF target (Stage 4).
