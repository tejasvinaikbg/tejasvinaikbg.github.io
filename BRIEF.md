# Project 1 — Resume Site

**Goal:** your CV as a fast, accessible, responsive web page you can send as a link — and print to a clean PDF from the same page.
**Stack:** plain HTML + CSS only. **No frameworks, no Tailwind, no JavaScript** (except the optional theme toggle in Stage 5).
**Hosting:** GitHub Pages, Netlify or Vercel (static).

## ⚠️ Privacy — before anything is public
A hosted page is readable by anyone and indexed by search engines.
- **Leave out your phone number and home address.** Email + LinkedIn + GitHub are enough for a public page.
- Keep the phone number only in the PDF you send directly to a recruiter, if you want it there.
- Consider `<meta name="robots" content="noindex">` if you don't want it in search results.

## Content
Use your **current, verified resume** — the same facts as the PDF versions. Don't add skills the resume doesn't have (your standing rule).

**Profile photo** — a professional headshot. Serve it responsively (`srcset` with ~160/320/480 px WebP, `width`/`height` set), `alt="Tejasvi Naik"` (a name, not "photo of…"), **not** lazy-loaded (it's at the top). Initials fallback if the image fails.

**Availability status** — one element, four states, switched by editing ONE attribute:
```html
<p class="status" data-status="notice">Notice period: 15 days</p>
<!-- data-status = available | serving | notice | hidden -->
```
- CSS styles each state from the attribute (`.status[data-status="serving"] { … }`); `hidden` removes it with no gap left behind
- The **text always states the status** — the coloured dot is decoration (`aria-hidden`), never the only signal
- In print: plain text, no dot
- ⚠️ Keep the date / notice value **current** — a stale "last working day" on a public page is worse than none

## Stages and acceptance criteria

### Stage 1 — Semantic HTML only (no CSS)
- [ ] One `<h1>` (your name); sections as `<section>` with `<h2>` headings; experience entries as `<article>` with `<h3>`
- [ ] Dates in `<time datetime="2024-03">`; contact links as real `<a href>` (`mailto:`, LinkedIn, GitHub)
- [ ] Photo with correct `alt`, `width`/`height`, responsive `srcset`; availability line present with `data-status`
- [ ] Skills as lists, not comma strings; `<html lang="en">`, `<meta charset>`, viewport tag, `<title>`, `<meta name="description">`
- [ ] Reads correctly top to bottom with CSS off; passes the W3C validator

### Stage 2 — Design tokens and base styles
- [ ] All colours, font sizes and spacing as **custom properties** on `:root`
- [ ] Type scale in `rem`; body line length ≤ ~70ch; fonts loaded with `font-display: swap` and fallbacks
- [ ] Focus styles visible on every link

### Stage 3 — Layout and responsive
- [ ] All four `data-status` states render correctly and `hidden` leaves no gap
- [ ] **Mobile-first**; two-column layout (sidebar + main) from a content-chosen breakpoint
- [ ] Grid/Flexbox only; no fixed widths; no horizontal scroll at 320 px; reflows at 400% zoom
- [ ] At least one **container query** (e.g. experience entries that rearrange by their column's width)

### Stage 4 — Print stylesheet = your PDF
- [ ] `@media print`: A4, sensible margins (`@page`), black text, no backgrounds, link URLs shown where useful
- [ ] Fits **2 pages max**; no entry split across pages (`break-inside: avoid`)
- [ ] "Download PDF" = browser Print → Save as PDF from this page

### Stage 5 — Polish
- [ ] Dark mode via `prefers-color-scheme` + tokens (optional JS toggle with no flash)
- [ ] Lighthouse: Performance, Accessibility, Best Practices, SEO all ≥ 95 on mobile
- [ ] Open Graph tags + a share image, so the link previews nicely in LinkedIn/WhatsApp
- [ ] Favicon

### Stage 6 — Deploy
- [ ] Hosted on a custom or platform URL; HTTPS
- [ ] README in the repo: what it is, how to run, Lighthouse scores, what you learned

## What reviewers will look at
Semantic structure · accessibility (headings, contrast, focus, alt text) · CSS organisation (tokens, low specificity, no `!important`) · responsiveness · print quality · performance.
