# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**noth.ch** is a static multilingual personal portfolio website for Philippe Noth. Pure HTML + CSS — no build step, no JavaScript framework, no package manager. JavaScript is limited to the inline light/dark theme toggle in each page and the Google Analytics `gtag` snippet (all pages except `404.html`). `js/script.js` is an empty placeholder.

## Development

No build pipeline. Open files directly in a browser or serve with any static file server:

```powershell
# Serve locally with Python
python -m http.server 8080

# Or with Node (if installed)
npx serve .
```

No linting, no test suite. Validate HTML at validator.w3.org if needed.

## Architecture

### Multilingual Structure

All content is duplicated across four language folders:

```
/en/   /fr/   /de/   /it/
  index.html
  translation_languages.html
  travel_photography.html
  hiking_nature.html
  ai_it_mathematics.html
```

Language routing is handled by `.htaccess` via Apache `Accept-Language` detection (302 redirects). The root `index.html` is a standalone copy of the English homepage (root-relative links), served when the redirect doesn't apply (e.g. local servers, which ignore `.htaccess`). The real entry points are in `en/`, `fr/`, `de/`, `it/`. Keep the root `index.html` in sync with `en/index.html`.

When editing content, changes must be propagated to all four language folders manually.

### Favicon

The favicon lives at `assets/favicon.svg` — an "n." monogram on a `#0E0E10` rounded rectangle, with the period in `--ember` (`#C8A06B`) echoing the `noth.ch` title motif.

### Single Stylesheet

All styling lives in [css/style.css](css/style.css). No CSS framework. Structure:
1. Reset + base
2. CSS custom properties (design tokens)
3. SVG film grain overlay (mix-blend-mode: screen)
4. Layout components: `.site-header`, `.intro`, `.interests`, `.card`, `.site-footer`
5. Detail page components: `.detail-nav`, `.detail-nav-bottom`, `.detail-content`, `.detail-img-row`, `.book-card`
6. Entrance animations (`card-in`, `fade-up` keyframes) with `prefers-reduced-motion` guard
7. Four media query breakpoints: 640px, 768px, 1024px, 1280px

### Design Tokens

> **Full design system:** [design-system.md](design-system.md) covers colour (dark + light themes), typography, spacing, imagery, motion and a checklist for new components. Read it before any visual or CSS change, or before adding a new component.

```css
--ink:   #0E0E10  /* charcoal background */
--bone:  #E8E4DC  /* primary text */
--dust:  #9A968D  /* muted/secondary text */
--ember: #C8A06B  /* brass accent (links, hover states) */
```

Typography: Cormorant Garamond (display), Inter (body), JetBrains Mono (labels) — all from Google Fonts CDN.

Muted text uses `var(--dust)` throughout — do not use raw `rgba(232,228,220,0.7)`.

### Card & Detail Page Patterns

Index pages use a 4-card CSS Grid with subgrid for aligned footers. Each `.card` contains `.card-body` (title link), `.card-frame` (image + number overlay 01–04), `.card-footer` (company + social icons). Card 04 (AI/IT) has an empty `.card-footer` — this is intentional.

Detail pages follow a consistent layout: `.detail-nav` (top back link) → `.intro` → `.detail-content` sections → `.detail-nav.detail-nav-bottom` (bottom back link) → footer. The bottom nav uses the same `.detail-nav` styles plus `.detail-nav-bottom` for top padding.

Gallery rows use `.detail-img-row` (2-column, grayscale by default). Tags use `.detail-tags` / `.detail-tag` chips.

Social icon tooltips are pure CSS via `::after` pseudo-elements using `data-tooltip` attributes.

### HTML Conventions

- `<body id="top">` on every language page (`en/`, `fr/`, `de/`, `it/`) — required for the footer `↑` scroll anchor (`href="#top"`). The root `index.html` and `404.html` have no `↑` link and no `id="top"`.
- Footer copyright: `noth<span class="ember">.</span>ch` — the ember dot is part of the brand.
- All language pages include Open Graph meta tags (`og:title`, `og:description`, `og:type`, `og:url`). The root `index.html` and `404.html` don't.
- Language-specific back-link text: EN "← All interests" · FR "← Tous les intérêts" · DE "← Alle Interessen" · IT "← Tutti gli interessi".

### Images

Content images are referenced via CSS background-image classes (`.img-translation`, `.img-travel`, `.img-hiking`, `.img-ai`) and `<img>` tags. Image files live in `images/`. Card images can be swapped in those CSS classes (local file or URL) without touching HTML.
