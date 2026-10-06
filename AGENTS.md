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

Open a page directly in a browser to preview it. To see folder paths and redirects as GitHub Pages serves them, run `python3 -m http.server 8000` in the repository root and visit `http://localhost:8000`. That server shows its own error page for missing paths; GitHub Pages serves `404.html` instead, so preview it at `/404.html`. There is no build step.

## Pages

- **`index.html`** — The homepage: "AD MAIOREM DEI GLORIAM", centered.
- **`portfolio/index.html`** — Startup investments, served at `/portfolio/`.
- **`404.html`** — GitHub Pages serves this at any missing path, so every link and asset in it must start with `/`.
- **`resume/`, `x/`, `venmo/`, `flickr/`** — Redirects; see below.

## Redirects

GitHub Pages has no server-side redirects, so each short path is a folder holding an `index.html` with a zero-delay meta refresh. The folder form makes both `/x` and `/x/` work. A meta refresh replaces the current history entry, so Back skips the redirect page.

To add a redirect, copy `x/index.html` into a new folder and change the URL in its refresh tag, canonical link, and fallback link. Keep the rest identical:

- **Blank interstitial.** Safari keeps a redirect page on screen while the destination loads, so the page is a blank Sand screen with no margin and the title `Redirecting…`.
- **Fallback link.** It reads `Continue` and stays invisible for 3 seconds, then fades in for visitors whose browser blocks automatic refresh.
- **Viewport meta.** It stops phones from laying the page out 980px wide.
- **No web fonts.** A font download can delay the load event that the refresh waits for.

Do not add `noindex`, and do not list redirects in `sitemap.xml`.

## Design system

- **Color** — Radix Colors Sand, with light and dark mode via `prefers-color-scheme`. Steps follow Radix's usage roles: step 1 for the background, step 12 for text, step 7 for link underlines, and step 8 for focus outlines. On hover and focus, links ease to step 11 text and their underline fades out over 0.3s, with no easing under `prefers-reduced-motion`. Values (light / dark): step 1 `#fdfdfc` / `#111110`, step 7 `#cfceca` / `#494844`, step 8 `#bcbbb5` / `#62605b`, step 11 `#63635e` / `#b5b3ad`, step 12 `#21201c` / `#eeeeec`.
- **Tokens** — Each page declares only the CSS custom properties it uses on `:root`. Because there is no build step or shared stylesheet, the token block is repeated in each page; keep shared values identical across pages.
- **Typography** — Cinzel, a capitals-only serif, is the default font. Inter is the sans-serif counterpart, used for the portfolio list. Both load from Google Fonts at weight 400 with `display=block`.
- **Font subsetting** — Each page's Google Fonts URL uses the `text=` parameter to download only the characters that page displays. Give each font its own link and `text=` value, because one `text=` applies to every family in a request. When you change displayed text, update that page's `text=` value to match, or new characters will render in a fallback font.

## Small viewports

Every page must stay legible, with no sideways scrolling, down to the tiniest viewport. Headings hold a full and a short version in `.full` and `.short` spans, and media queries in `em` choose one:

- **Homepage** — The motto on one line, then two balanced lines, then `AMDG`. Below `6.25em` wide, `AMDG` stacks vertically if the screen is at least `8em` tall; otherwise it drops its padding and letter-spacing.
- **`404.html`** — `NON INVENTUM`, then `404` below `13em`, stacking below `4.5em` wide when at least `6.25em` tall.
- **Portfolio** — Below `10em`, no side padding or heading letter-spacing. Below `5.25em`, each row shows only its capitalized first letter: every heading and name is written as its first letter followed by `<span class="rest">`, which hides.

No heading or link may wrap onto a second line, except the homepage motto's two balanced lines. If you change displayed text, re-measure it and move its breakpoints, and keep every character it can show in the page's `text=` subset.

## Metadata

- `sitemap.xml` lists `/`, `/portfolio/`, and the résumé PDF. Update `lastmod` when a listed page changes.
- Pages have no meta description by design; search snippets come from the page text.
- The site has no icon files or web app manifest by design. Every page, including redirects, declares `<link rel="icon" href="data:," />` so browsers do not request a missing `/favicon.ico`.
- `robots.txt` points crawlers to the sitemap. `CNAME` sets the custom domain `edmundloo.com`.
