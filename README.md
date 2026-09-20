# noth.ch

Personal portfolio site for [Philippe Noth](https://noth.ch) — freelance translator, hiker, photographer, and math enthusiast from Valais, Switzerland.

Static, multilingual (EN / FR / DE / IT), no framework, no build step. Pure HTML, CSS, and a touch of vanilla JavaScript.

## Screenshots

<p align="center">
  <img src="docs/screenshots/desktop-home.png" alt="Desktop view of the homepage" width="90%">
</p>

<p align="center">
  <img src="docs/screenshots/mobile-home.png" alt="Mobile view of the homepage" width="32%">
  &nbsp;&nbsp;
  <img src="docs/screenshots/inner-translation.png" alt="Translation & Languages inner page" width="60%">
</p>

## Design

Dark, refined, restrained. Charcoal `#0E0E10` background with a subtle warm glow at the top of the page. Cormorant Garamond serif for the display name and card titles (its italic ampersands are the visual signature), Inter for body, JetBrains Mono for small uppercase labels. A muted brass accent (`#C8A06B`) for the period in `noth.ch`, italic `&` separators, and the available indicator.

## Layout — flexbox throughout

- Page is a vertical flex column with the footer pushed down via `margin-top: auto`.
- Header is a single flex row with the locale/availability indicator floating right.
- The card grid uses `flex-wrap` with `flex-basis: calc(50% - 12px)` on tablet and `calc(25% - 24px)` on desktop — 1 card per row on mobile, 2 on tablet, 4 on desktop.
- Each card is itself a flex column with the social icon row pinned to the bottom, so icons stay aligned across all four cards even when titles have different lengths.
- Footer row collapses from horizontal to vertical below `sm`.

## Structure

```
/
├── index.html              # Root redirect (browser language → subfolder)
├── .htaccess               # Apache language redirect + 404
├── 404.html
├── assets/
│   └── favicon.svg
├── css/
│   └── style.css
├── js/
│   └── script.js           # Theme toggle
├── images/                 # Portrait, cards, book covers
├── en/  fr/  de/  it/      # One folder per language, mirrored content
│   ├── index.html
│   ├── translation_languages.html
│   ├── travel_photography.html
│   ├── hiking_nature.html
│   └── ai_it_mathematics.html
└── docs/
    └── screenshots/        # Screenshots for this README
```

## Running locally

No build pipeline. Any static file server works:

```powershell
# Python
python -m http.server 8080

# Node
npx serve .
```

Then open <http://localhost:8080>.

## Design touches

- Images sit at 35 % grayscale and are slightly dimmed by default; on hover the photo restores to full color, scales up gently, and the card frame border warms to brass.
- Subtle film grain across the whole page (SVG noise filter, `mix-blend-mode: screen`).
- Numbered cards (01–04) with monospace labels, top-aligned in each image.
- Pulsing "Available" dot in the header.
- Hover underline animation on footer links.
- Portrait avatar in the header scales to 2.6× on hover, with a soft drop shadow.

## Responsive breakpoints

Four `@media` blocks at 640 / 768 / 1024 / 1280 px. Each block adjusts padding, the display type size, the card `flex-basis`, and toggles the locale and availability indicators.

## Deployment

Any static host works. The included `.htaccess` handles browser-language redirection on Apache. For Netlify / Cloudflare Pages / Vercel, translate the rules to a `_redirects` file.

## License

MIT for the source code. Personal content (photos, portrait, prose, the *noth.ch* name) © 2026 Philippe Noth, all rights reserved. See [LICENSE](LICENSE).
