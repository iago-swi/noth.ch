# noth.ch — Design System

Source of truth for the visual language of noth.ch. All values below are taken from [css/style.css](css/style.css); if the two ever disagree, the CSS wins and this file should be updated.

**Mood:** dark, refined, restrained. Editorial rather than "web app". Few colours, lots of space, serif display type, small monospace labels, slow and soft motion. When in doubt, remove rather than add.

---

## 1. Colour

### Tokens (`:root`, dark theme — default)

| Token     | Value     | Role |
|-----------|-----------|------|
| `--ink`   | `#0E0E10` | Page background (charcoal) |
| `--coal`  | `#16161A` | Surfaces behind images (card frame, gallery image) |
| `--bone`  | `#E8E4DC` | Primary text, active states |
| `--dust`  | `#9A968D` | Muted / secondary text, labels, inactive links |
| `--ember` | `#C8A06B` | Brass accent: brand dot, hover, focus, links |

### Light theme (`[data-theme="light"]`)

| Token     | Value     |
|-----------|-----------|
| `--ink`   | `#F4F1EB` |
| `--coal`  | `#E2DDD5` |
| `--bone`  | `#18181C` |
| `--dust`  | `#6B6760` |
| `--ember` | `#A06830` |

Tokens are semantic, so components should only reference `var(--…)`. Hard-coded rgba values exist only for **borders and translucent overlays**, and each one has a light-theme override at the end of the stylesheet.

### Rules
- Muted text is **always** `var(--dust)`. Never use raw `rgba(232,228,220,0.7)`.
- Ember is an **accent**, not a fill: it marks the dot in `noth.ch`, italic `&` separators, hover states, focus outlines and inline links like `.book-card-link`. Never use it for large areas or body text.
- Borders are hairlines (`1px`) in bone at low alpha:
  - `0.08` image frames · `0.10` footer rule, book card · `0.12` tags, book covers · `0.20` avatar
  - Hover border: `rgba(200,160,107,0.35)` (ember at 35 %). Light mode: `rgba(160,104,48,0.4)`.
- Long-form paragraphs on detail pages use bone at 80 % (`rgba(232,228,220,0.8)`); `<strong>` returns to full `--bone`.
- Tooltip is the one exception: solid `#BB9761` background, black text.

### Atmosphere
- **Top glow:** `.page` background is `radial-gradient(ellipse 80% 50% at 50% -10%, rgba(200,160,107,0.15), transparent 60%)`. In light mode it drops to `0.07` alpha.
- **Film grain:** `body::before` is a fixed SVG `feTurbulence` noise layer with `mix-blend-mode: screen` and `opacity: 0.5`. Light mode uses `multiply` at `0.2`. It sits at `z-index: 1`, under `.page` (`z-index: 10`), and has `pointer-events: none`.

---

## 2. Typography

Loaded from Google Fonts:
`Cormorant Garamond` (300, 400, 500, italic 400) · `Inter` (300, 400, 500, 700) · `JetBrains Mono` (400, 500).

| Family | Used for | Typical settings |
|--------|----------|------------------|
| **Cormorant Garamond** (serif) | Display title, card titles, section `h3`, book titles, brand name | weight **300**, `letter-spacing: -0.01em`, tight line-height (0.95 for display, 1.2 for headings) |
| **Inter** (sans) | Body text, descriptions, tooltips | 13–17px, weight 300 for prose, line-height 1.5–1.8 |
| **JetBrains Mono** | Eyebrows, labels, card numbers, nav links, tags, footer, meta | **10–12px, UPPERCASE**, `letter-spacing` 0.15–0.25em, colour `--dust` |

### Type scale

| Element | Size |
|---------|------|
| `.title` (home display) | `clamp(3rem, 18vw, 10rem)` → 14vw (≥640) → 11vw (≥768) → 9rem (≥1024) → 11rem (≥1280); line-height 0.95 |
| `.detail-title` | 10vw → 8vw → 6vw → 4.5rem (≥1024) |
| `.card-title` | 24px → 26px (≥640) |
| `.detail-section h3` | 26px |
| `.book-card-title` | 20px · `.book-card-sub` 15px italic serif, `--dust` |
| `.intro p` | 16px → 17px, line-height 1.7, weight 300, max-width 36rem |
| `.detail-section p` | 15px, line-height 1.8, weight 300 |
| `.card-desc` / `.book-card p` | 13px / 14px, `--dust` |
| Mono labels | 12px (eyebrow, header, section-label) · 11px (footer, nav back-links) · 10px (card number, tags, book meta/link) |
| Active language (`.lang-link.active`) | 2.4rem, weight 700 — the one bold, oversized mono moment |

### Signature details
- **Italic ampersand:** `&` in titles is wrapped in `.italic` (often also `.ember`). The Cormorant italic `&` is the visual signature of the site.
- **Brand mark:** `noth<span class="ember">.</span>ch`. The ember dot is part of the brand, in the footer, in titles and in the favicon.

---

## 3. Layout & spacing

### Page skeleton
`.page` is a flex column with `min-height: 100vh`, and the footer is pushed down with `margin-top: auto`.
Header → `.intro` (centred) → content → `.site-footer` (hairline top border).

### Horizontal gutters (shared by header, intro, content, footer)

| Viewport | Gutter |
|----------|--------|
| < 640px  | 24px |
| ≥ 640px  | 40px |
| ≥ 1024px | 64px |

### Breakpoints
Mobile-first, `min-width`: **640 / 768 / 1024 / 1280 px**. Two `max-width` helpers (639px, 640–1023px) only add top spacing to wrapped card rows.

### Card grid (home)
- `.cards` is **CSS Grid**: 1 column → 2 columns (≥640) → 4 columns (≥1024). Gap `10px 24px`, then `10px 32px` on desktop.
- Each `.card` spans 3 rows with `grid-template-rows: subgrid`, so the **title / image / footer rows line up across all cards** even when titles differ in length.
- Card anatomy: `.card-body` (title link) → `.card-frame` (4:5 image + `.card-number` 01–04 at top-left 16px) → `.card-footer` (company + `.card-socials`). Card 04's footer is intentionally empty.

### Detail pages
- `.detail-nav` (top back-link) → `.intro` → `.detail-content` → `.detail-nav.detail-nav-bottom` → footer.
- `.detail-content` max-width is 760px, centred (`max-width: none` at ≥1024).
- Vertical rhythm: sections are 52px apart, `h3` → text 16px, paragraphs 14px apart, image rows have a 36px margin.
- `.detail-img-row`: 2 columns, 16px gap, 3:2 images. It drops to 1 column below 640px.
- `.book-cards-row`: 2 columns of `.book-card` (bordered, translucent coal fill, 110px floated cover).
- `.detail-tags`: flex-wrap chips, mono 10px, hairline border, `4px 10px` padding, **square corners**.

### Shape
Corners are **square everywhere**. The only rounded elements are the avatar (circle), the tooltip (2px) and the favicon (3px). No shadows, except the avatar hover lift.

---

## 4. Imagery

- Default treatment: `filter: grayscale(35%) contrast(1.05) brightness(0.9)`.
- Hover: `grayscale(0%) contrast(1.05) brightness(1)` and `scale(1.03)`. The frame border warms to ember at 35 %.
- Images sit on `--coal` inside a hairline-bordered frame with `overflow: hidden`.
- Ratios: **4:5** for card images, **3:2** for gallery images.
- Home card images are CSS backgrounds (`.img-translation`, `.img-travel`, `.img-hiking`, `.img-ai`) pointing to `images/*.jpg`. Swap the file or URL in the CSS, not in the HTML.

---

## 5. Motion

| Pattern | Spec |
|---------|------|
| Colour/hover transitions | `.2s ease` |
| Underline reveal (`.ulink`, nav links, inactive lang links, book link) | `::after` 1px `currentColor` line, `scaleX(0 → 1)` from the left, `.35s ease` |
| Image hover | filter + transform `.7s ease`; border `.4s ease` |
| Springy scale (avatar ×2.6, theme icon ×2.4) | `.35s cubic-bezier(0.34, 1.56, 0.64, 1)`. Only inside `@media (hover: hover)`; `:active` uses a smaller scale (×1.9 / ×1.6) for touch |
| Social icon hover | ember + `translateY(-1px)` |
| Entrance | `fade-up` (10px) for eyebrow 0.05s → title 0.12s → intro p 0.22s; `card-in` (18px) staggered 0.35 / 0.45 / 0.55 / 0.65s; easing `cubic-bezier(0.22, 1, 0.36, 1)` |
| Reduced motion | `prefers-reduced-motion: reduce` disables all entrance animations. Every new animation must be added to this guard. |

Motion should be slow, soft and decorative. Nothing should bounce except the two intentional "springy" hovers.

---

## 6. Interaction & accessibility

- Focus: `:focus-visible { outline: 1px solid var(--ember); outline-offset: 3px; }`.
- Tooltips: pure CSS, `.social-icon::after { content: attr(data-tooltip) }`.
- Hover-only effects that move or enlarge things are wrapped in `@media (hover: hover)`, with an `:active` fallback for touch.
- Theme: a `.theme-toggle` sun/moon button. An inline `<head>` script applies `data-theme="light"` from `localStorage` **before paint** to avoid a flash, and a script at the end of `<body>` handles the toggle.

---

## 7. Brand assets

- **Favicon** `assets/favicon.svg`: serif "n." on an `#0E0E10` rounded square (rx 3), `n` in bone, `.` in ember.
- **Avatar**: `images/portrait-ia-small.jpg`, 36px circle, 1px border.

---

## 8. Checklist for new components

1. Colours only from tokens (or a hairline rgba border **plus** its light-theme override).
2. Headings in Cormorant 300, labels in JetBrains Mono uppercase with letter-spacing, prose in Inter 300.
3. Square corners, 1px hairlines, no drop shadows.
4. Ember only on hover, focus or small accents.
5. Use the 24 / 40 / 64 gutters and the 640 / 768 / 1024 / 1280 breakpoints.
6. Images: 35 % grayscale by default, full colour on hover.
7. Animations: the existing easing curves, plus a `prefers-reduced-motion` guard.
8. Add the component to all four language folders (EN / FR / DE / IT).
