# Design tokens — Resume site (from Claude Design, Sep 2026)

Type these into `:root` yourself (Stage 2). Light / dark values.

## Colour
| Token | Light | Dark | Role | Contrast on --bg |
|---|---|---|---|---|
| `--bg` | `#F5F6F7` | `#0E1114` | page background | — |
| `--ink` | `#101418` | `#E7EAED` | headings, body | 17.1 : 1 / 15.4 : 1 |
| `--muted` | `#4E5761` | `#9BA4AE` | meta, labels, stack lines | 6.9 : 1 / 7.6 : 1 |
| `--rule` | `#D3D8DD` | `#2B3137` | dividers, timeline line | — |
| `--accent` | `#0A6B5C` | `#5CCFB4` | years, links, hover, focus | 6.0 : 1 / 10.3 : 1 |
| `--accent-ink` | `#FFFFFF` | `#0E1114` | text on accent | — |
| `--ok` | `#1F8A4C` | `#4CC27A` | status dot — available | — |
| `--warn` | `#B7791F` | `#E0A53A` | status dot — serving notice | — |
| `--tint` | `#DCEBE7` | `#17302B` | photo placeholder background | — |
| link hover | `#08564A` | — | darker accent | — |

## Type
- **Schibsted Grotesk** 400 / 500 / 600 / 700 — all text
- **Geist Mono** 400 / 500 — dates, labels, stack lines, contact text in print
- Base 16 px · body line-height **1.55** · bullets max **64ch** · summary max **42ch**

| Size | Use |
|---|---|
| `4.75rem` (76 px) | name — desktop |
| `2.75rem` | name — small screens |
| `2rem` | timeline year |
| `1.375rem` | job title under the name |
| `1.25rem` | role title |
| `1rem` | body |
| `0.8125rem` | labels (EXPERIENCE, SKILLS, skill areas) |

## Spacing scale (px)
`4 · 8 · 12 · 16 · 24 · 40 · 56 · 72 · 96`
- page padding **96** desktop / **24** mobile (mobile: 40 top, 56 bottom)
- column gap **96** desktop / **64** below
- space between roles **56**
- content max-width **1248 px**; sidebar column `minmax(0, 380px)`
- timeline rail **112 px** desktop / **60 px** narrow

## Shape, focus, breakpoints
- radius **0** by default; **4 px** on the button
- focus: **2 px accent outline, offset 3 px**
- photo: circle, **160 px** desktop / **120 px** mobile / **28 mm** print; **2 px accent ring, offset 3 px**; initials on `--tint` when no photo
- hit targets **48 px** minimum
- **one breakpoint: 900 px** — below it one column and the sidebar is not sticky
- dark theme: `prefers-color-scheme` swaps the custom properties

## Icons (Lucide, stroke 1.75, currentColor)
mail · linkedin · github · download · briefcase (Experience) · graduation-cap (Education) · circle-check · hourglass · calendar-clock (status)
