# AGENTS.md

Instructions for AI coding agents working in this repository.

## Principles

- **No JavaScript.** HTML and CSS only.
- **No build system, bundler, or dependencies.** Keep it zero-dependency.
- **Minimalist.** Less is more. Don't add what isn't needed.
- **Fast.** Every byte matters. Inline CSS, no external stylesheets, minimal markup.
- **High quality code.** Clean, semantic HTML. Well-organized CSS. No hacks.

## Overview

Personal website for Edmund Loo, hosted via GitHub Pages at edmundloo.com. Every page is a hand-written HTML file with inline CSS. The homepage shows only the motto. The other paths are shared by word of mouth; do not link to them from the homepage.

## Development

Open a page directly in a browser to preview it. To see folder paths and redirects as GitHub Pages serves them, run `python3 -m http.server 8000` in the repository root and visit `http://localhost:8000`. There is no build step.

## Pages

- **`index.html`** — The homepage: "AD MAIOREM DEI GLORIAM", centered.
- **`portfolio/index.html`** — Startup investments, served at `/portfolio/`.
- **`404.html`** — GitHub Pages serves this at any missing path, so every link and asset in it must start with `/`.
- **`resume/`, `x/`, `venmo/`, `flickr/`** — Redirects; see below.

## Redirects

GitHub Pages has no server-side redirects, so each short path is a folder holding an `index.html` with a zero-delay meta refresh. The folder form makes both `/x` and `/x/` work. A meta refresh replaces the current history entry, so Back skips the redirect page. Safari keeps the redirect page on screen while the destination loads, so the template sets the Sand background and lets the fallback link take the text color. Use this template:

```html
<!DOCTYPE html>
<html lang="en-US">

<head>
  <meta charset="UTF-8" />
  <meta name="color-scheme" content="light dark" />
  <meta http-equiv="refresh" content="0; url=https://example.com/target" />
  <link rel="canonical" href="https://example.com/target" />
  <title>Name</title>
  <style>
    body { background-color: #fdfdfc; }
    @media (prefers-color-scheme: dark) { body { background-color: #111110; } }
    a { color: inherit; }
  </style>
</head>

<body>
  <a href="https://example.com/target">example.com/target</a>
</body>

</html>
```

Do not add `noindex`, and do not list redirects in `sitemap.xml`.

## Design system

- **Color** — Radix Colors Sand, with light and dark mode via `prefers-color-scheme`. Steps follow Radix's usage roles: step 1 for the background, step 12 for text, step 7 for link underlines, and step 8 for hovered underlines and focus outlines. Values (light / dark): step 1 `#fdfdfc` / `#111110`, step 7 `#cfceca` / `#494844`, step 8 `#bcbbb5` / `#62605b`, step 12 `#21201c` / `#eeeeec`.
- **Tokens** — Each page declares only the CSS custom properties it uses on `:root`. Because there is no build step or shared stylesheet, the token block is repeated in each page; keep shared values identical across pages.
- **Typography** — Cinzel, a capitals-only serif, is the default font. Inter is the sans-serif counterpart, used for the portfolio list. Both load from Google Fonts at weight 400 with `display=block`.
- **Font subsetting** — Each page's Google Fonts URL uses the `text=` parameter to download only the characters that page displays. When you change displayed text, update that page's `text=` value to match, or new characters will render in a fallback font.

## Metadata

- `sitemap.xml` lists `/`, `/portfolio/`, and the résumé PDF. Update `lastmod` when a listed page changes.
- Pages have no meta description by design; search snippets come from the page text.
- `site.webmanifest`, `robots.txt`, and the favicons complete the site. `CNAME` sets the custom domain `edmundloo.com`.
