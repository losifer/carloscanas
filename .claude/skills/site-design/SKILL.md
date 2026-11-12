---
name: site-design
description: Design system for the Carlos Cañas site — typography (STIX Two Text for titles, Roboto Serif for content), plum satin palette, and the top navigation pattern. Use whenever adding or changing pages, sections, components, text styles, or colors on this site.
---

# Carlos Cañas site design

Follow these rules for every page and component on this site. The reference implementation is `index.html`.

## Typography

| Role | Font | Notes |
|---|---|---|
| Titles (h1–h3, hero name, section headings) | **STIX Two Text**, weight 400 | Reserved for titles only. Never use it for body copy, nav or UI labels. |
| Content (body text, nav, buttons, captions, forms) | **Roboto Serif**, weights 300–500 | Default font for everything that is not a title. |

- Load both from Google Fonts in one request:
  ```html
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Roboto+Serif:opsz,wght@8..144,300..500&family=STIX+Two+Text:wght@400&display=swap" rel="stylesheet" />
  ```
- Reference fonts through CSS variables, never hard-coded names:
  ```css
  --font-title: "STIX Two Text", Georgia, serif;
  --font-body: "Roboto Serif", Georgia, serif;
  ```
- `body` uses `var(--font-body)`; title elements use `var(--font-title)`.
- Fonts already tried and rejected for titles: DM Serif Display, Comfortaa. Don't reintroduce them.

## Color

Plum-dominant, with violet only as a subtle accent — avoid drifting toward blue.

```css
--plum-deep: #220a26;  /* darkest background */
--plum:      #66206b;  /* main plum */
--indigo:    #6a3a9e;  /* soft violet accent (name kept for history) */
--ink:       #f4ecff;  /* text on plum */
--rule:      rgba(244, 236, 255, 0.28);  /* 1px dividers */
```

The WebGL satin background in `index.html` uses matching shader constants (`PLUM_DEEP`, `PLUM`, `BLUEVIOLET`, `SHEEN`). If you change the palette, update both the CSS variables and the shader constants.

## Navigation

- Fixed at the top, full width, links aligned **right**.
- Items: `About me | Work | Contact`, separated by a dimmed `|` (CSS `li + li::before`, not literal text in the HTML).
- Small type: Roboto Serif, `0.8125rem`, weight 300, `letter-spacing: 0.04em`, color `var(--ink)`.
- Flush to the top (`top: 0`), inset left and right by a 1% margin (`left/right: var(--gutter)`), so neither the links nor the line touch the side edges.
- `border-bottom: 1px solid var(--rule)` spanning the nav's width (edge to edge minus the margin).
- Hover/focus: full opacity and a 1px underline offset 4px.
- Padding `var(--gutter)`.

## Hero

- Name "Carlos Cañas" in the title font, centered.
- Subtitle "Product Designer" directly below in Roboto Serif, weight 300, `clamp(1rem, 2.2vw, 1.6rem)`, `letter-spacing: 0.06em`, 85% opacity.

## Layout and behavior

- Content padding is 1% all around: `--gutter: 1vw`, overridden to `16px` at ≤480px. Use `var(--gutter)` for page-edge padding everywhere.
- The satin canvas sits behind everything; overlay layers that don't need clicks use `pointer-events: none` so the mouse still reaches the satin.
- No horizontal scroll on phones.
- Respect `prefers-reduced-motion` for any new animation.
