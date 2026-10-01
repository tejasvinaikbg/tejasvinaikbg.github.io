# Tejasvi Naik B G — Resume site

My CV as a fast, accessible, responsive web page that also prints to a clean two-page PDF from the same HTML.

**Live:** https://tejasvinaikbg.github.io/ · **PDF:** [tejasvi-naik-resume.pdf](tejasvi-naik-resume.pdf)

Plain HTML and CSS. No framework, no build step, no JavaScript.

## Lighthouse (mobile, live site, 1 Oct 2026)

| Performance | Accessibility | Best Practices | SEO |
|:-:|:-:|:-:|:-:|
| 98 | 100 | 100 | 100 |

First Contentful Paint 1.9 s · Total Blocking Time 0 ms · Cumulative Layout Shift 0

## Features

- **Semantic HTML:** one `<h1>`, sections labelled by their `<h2>`, roles as `<article>`s in an ordered timeline, and every date in `<time datetime>`
- **Design tokens:** every colour, font size, spacing value and layout size is a custom property on `:root`
- **Dark mode:** follows `prefers-color-scheme`, and only the colour tokens change
- **Mobile first:** one column by default, two columns from `64rem`, with fluid padding and gaps using `clamp()`
- **Container query:** the timeline rail widens with the *column's* width, not the window's
- **Availability status:** one `data-status` attribute (`available` · `serving` · `notice` · `hidden`) switches the text colour dot and icon in CSS
- **Print stylesheet:** A4 with a different layout (header row, skills table, no timeline rail), roles never split across pages, and a page-number footer
- **Accessibility:** 48 px tap targets, a visible `:focus-visible` ring, ≥ 4.5:1 contrast in both themes, decorative icons hidden from screen readers
- **Link previews:** Open Graph tags with a 1200 × 630 share image, plus favicons

## Run it locally

No install needed. Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8765
```

Then visit http://localhost:8765.

**Validate the HTML:**

```bash
npx html-validate index.html
```

**Run Lighthouse:**

```bash
npx lighthouse https://tejasvinaikbg.github.io/ --form-factor=mobile --view
```

**Re-export the PDF:** in Chrome, open the page → Print → Destination: Save as PDF → Paper: A4 → Margins: Default → untick *Headers and footers* → save as `tejasvi-naik-resume.pdf`.

## Structure

```
index.html               the page
styles.css               tokens → reset → base → layout → components → utilities → print
tejasvi-naik-resume.pdf  exported from the print stylesheet
favicon.ico
assets/                  share image, favicons, status icons (CSS masks)
.htmlvalidate.json       validator settings (matches Prettier's output)
```

## Deploy

GitHub Pages serves the root of `main`. Push, and the site updates in about a minute.

## What I learned

<!-- Draft from the build log. Rewrite it in your own words. -->

- **Semantics first.** Writing the HTML with no CSS made the structure honest. A `<section>` needs a heading, otherwise it's a `<div>`. Skills are a `<dl>` of lists, not comma strings. Duplicate dates on the rail are `aria-hidden`.
- **Tokens make responsive design cheap.** The breakpoint and the container query only change custom properties (`--photo`, `--rail`, `--year`), and every rule that reads them updates by itself.
- **Container queries vs media queries.** At a 1023 px window the content column is 921 px wide, but at 1040 px it's 492 px. Only a container query gets the timeline right in both cases.
- **`minmax(0, 1fr)`** stops long words from pushing a grid wider than the screen.
- **`gap` instead of margins.** It's why the `hidden` status leaves no empty space behind.
- **Round the container, not the image.** `border-radius: 50%` plus `overflow: hidden` on the avatar gives a circle for both the initials and the photo.
- **Inline SVG with `currentColor`** lets icons follow hover and dark mode. `mask-image` lets CSS choose an icon from an attribute.
- **Print is its own layout.** `display: contents`, `break-inside: avoid` and `@page` margin boxes produced a two-page PDF from the same HTML.
- **Measure, don't guess.** Checking at 320 px through 1920 px caught a cramped content column, and that's what moved the breakpoint from 900 px to 1024 px.
