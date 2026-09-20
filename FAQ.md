# FAQ

## 1. What kind of files are supposed to be stored in `assets/` in this standard structure?

In a standard web project structure, `assets/` is typically a catch-all for static files that don't fit the more specific folders. Common conventions:

`assets/` holds:

- **Fonts** — `.woff`, `.woff2`, `.ttf`, `.eot`
- **Icons / favicons** — `favicon.ico`, `.svg` icon sets
- **Documents for download** — `.pdf`, `.docx`, `.zip`
- **Data files** — `.json`, `.xml`, `.csv` (if served statically)
- **Media** — `.mp4`, `.mp3`, `.webm` (if not large enough to warrant their own folder)
- **Vendor/third-party libs** — sometimes placed here instead of `js/` or `css/`

The distinction from `images/` is that `images/` is reserved for visual content displayed on the page (photos, illustrations, backgrounds), while `assets/` is for supporting files the user downloads or the page depends on structurally.

Some teams skip `assets/` entirely and use more specific subfolders (`fonts/`, `files/`, `media/`) — there's no single universal rule. The pattern in your prompt seems to anticipate GPX/KMZ files and maps (given your hiking site), so `assets/` or `files/` would be the natural home for those downloadable attachments.

---

## 2. What should I be aware of if I replace the placeholder images with my own photos?

A few practical things to keep in mind:

### File size & performance

- Raw photos from a camera are typically 5–20 MB — compress them before deploying (tools: Squoosh, ImageMagick, or a build-time plugin)
- Use modern formats: `.webp` for broad browser support, `.avif` for even better compression
- Serve different sizes for different screens using `srcset` / `<picture>` — a mobile visitor shouldn't download a 3000px wide image

### Dimensions

- If the layout was built around placeholder images with specific aspect ratios (e.g. 16:9 hero, square thumbnails), your photos need to match or the layout will shift/stretch
- CSS `object-fit: cover` helps fill containers without distortion, but you need to be intentional about which part of the photo gets cropped

### File naming

- Avoid spaces and special characters — use lowercase with hyphens (`col-de-la-croix.webp` not `Col de la Croix (1).JPG`)
- Be consistent so paths in HTML don't break

### Color profile

- Cameras embed color profiles (Adobe RGB, Display P3) that browsers may render differently — convert to sRGB before exporting for the web

### Copyright & EXIF metadata

- Your photos may contain GPS coordinates, camera model, and timestamps in EXIF data — strip it if privacy matters (tools: `exiftool`, most export dialogs have an option)
- If they're your own photos you own the copyright, but worth adding a visible or meta credit if you care about attribution

### Alt text

- Placeholder images often have generic or empty `alt` attributes — replace them with meaningful descriptions for accessibility and SEO

> **Note for this hiking site specifically:** your photos likely have embedded GPS data worth stripping, and the file sizes from a hiking camera or phone will need aggressive compression before they're web-ready.

---

## 3. How to create WebP files?

Several options depending on your workflow:

### Online (simplest)

[Squoosh](https://squoosh.app) — drag and drop, choose WebP, adjust quality slider, instant preview of size vs. quality tradeoff. Best for one-off conversions.

### Desktop apps

| App | Platform | How to |
|---|---|---|
| **IrfanView** | Windows (free) | `File → Batch Conversion`, select WebP as output |
| **GIMP** | Cross-platform (free) | `File → Export As → filename.webp` |
| **Photoshop** | Cross-platform (CC 2021+) | `File → Export → Export As → WebP` |

### Command line (best for bulk/automation)

Install [cwebp](https://developers.google.com/speed/webp/docs/cwebp) (Google's official tool):

```bash
# Single file, quality 80 (0–100)
cwebp -q 80 photo.jpg -o photo.webp

# Batch convert all JPGs in a folder
for f in *.jpg; do cwebp -q 80 "$f" -o "${f%.jpg}.webp"; done
```

Or with **ImageMagick**:

```bash
magick photo.jpg -quality 80 photo.webp
```

### In a build pipeline

- VS Code extension: **Webp Converter**
- npm: `sharp` or `imagemin-webp` for automating conversion at build time

> **Recommended quality setting:** 75–85 strikes a good balance. Below 70 you'll see visible artifacts on photos; above 85 the file size savings over JPEG diminish.
