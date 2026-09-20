Dark, refined, restrained. Charcoal `#0E0E10` background with a subtle warm glow at the top of the page, Cormorant Garamond serif for the display name and card titles (its italic ampersands are the visual signature), Inter for body, JetBrains Mono for small uppercase labels. A muted brass accent (`#C8A06B`) for the period in `noth.ch`, italic `&` separators, and the available indicator.

**Flexbox throughout:**
- Page is a vertical flex column with the footer pushed down via `mt-auto`.
- Header is a single flex row with the locale/availability indicator floating right.
- The card grid uses `flex flex-wrap` with `basis-full sm:basis-[calc(50%-12px)] lg:basis-[calc(25%-24px)]` — that's 1 card per row on mobile, 2 on tablet, 4 on desktop.
- Each card is itself `flex flex-col` with the social icon row pinned to the bottom via `mt-auto`. This keeps the icons aligned across all four cards even though "Hiking" has a shorter title than "Translation & Languages".
- Footer row collapses from horizontal to vertical below `sm`.

**Touches that earn the "classy" label:**
- Images sit at 35% grayscale and slightly dimmed by default; on hover the photo restores to full color, scales up gently, and the card frame border warms to brass.
- Subtle film grain across the whole page (SVG noise filter, `mix-blend-mode: screen`).
- Numbered cards (01–04) with mono labels, top-aligned in each image.
- Pulsing "Available" dot in the header.
- Hover underline animation on footer links.

Photography placeholders are Unsplash — books for translation, an airplane window for travel, a misty trail for hiking, neural-network-style imagery for AI. Swap the URLs in the `.img-translation`, `.img-travel`, `.img-hiking`, `.img-ai` classes for your own photos when you're ready.

===================

Same site, no Tailwind. Pure HTML + CSS in one file.

What changed:

- **No CDN dependency**. The only external resource left is Google Fonts. If you want to drop that too, swap the font links for `serif`, `system-ui`, and `monospace` fallbacks — the layout will still hold up.
- **CSS architecture**: a small reset, design tokens as CSS custom properties (`--ink`, `--bone`, `--ember`, etc.), then component-scoped class rules (`.card`, `.card-frame`, `.card-img`, `.site-header`, `.site-footer`).
- **Responsive logic** lives in four `@media` blocks at 640 / 768 / 1024 / 1280 px — same breakpoints I was hitting with Tailwind's `sm` / `md` / `lg` / `xl`. Each block adjusts padding, the title size, the card `flex-basis`, and toggles the locale + availability indicators.
- **Flexbox is still doing all the layout work**: page wrapper is a column with `min-height: 100vh` and the footer pinned via `margin-top: auto`; cards use `flex-wrap` with `flex-basis: calc(50% - 12px)` (tablet) and `calc(25% - 24px)` (desktop); each card is itself a column with the social row pushed to the bottom by `margin-top: auto`.
- **Hover behavior preserved**: image desaturation/brightness lift, gentle scale, brass border warm-up, social icon color shift, footer link underline reveal.

Drop this file anywhere — open it directly in a browser, host it as a static file, attach it to anything. No build step.

===================

Two files. The HTML now references the stylesheet via <link rel="stylesheet" href="noth.css"> — keep them in the same directory and it'll work as-is.
If you host them at different paths (e.g. CSS in /assets/css/), update the href accordingly. The Google Fonts <link> stays in the HTML since it's tied to document loading rather than styling.

===================

Where would you like the portrait? A few natural spots:

Now the CSS — replace the `.brand-mark` rule with `.brand-avatar`:Avatar sits next to "noth.ch" — 36px circular, with a subtle 1px border that warms to brass on hover (along with a tiny scale lift so the whole brand block reacts as one). The `object-fit: cover` keeps the image properly cropped inside the circle regardless of the source aspect ratio.

The HTML references `assets/images/portrait-ia-gauche_small.jpg` relative to the page — make sure the folder structure matches when you deploy.

====================

A bigger image on hover usually means a popover/preview rather than just scaling the avatar in place. Quick check on what you want:

The avatar now scales to 2.6× its size on hover (36px → ~94px) and gets a soft drop shadow so it sits above the rest of the page. Two details worth noting:

- **`transform-origin: left center`** anchors the growth on the left edge so the image expands rightward and downward rather than from its center — this prevents it from drifting off the left side of the screen and keeps it visually anchored to the brand.
- **`cubic-bezier(0.34, 1.56, 0.64, 1)`** gives a slight overshoot bounce at the end of the scale-up, which feels more alive than a plain ease.

If 2.6× feels too aggressive or too tame, change the `scale()` value — anything between `2` and `3.5` will look reasonable.