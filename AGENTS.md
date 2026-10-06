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

GitHub Pages builds the repository with Jekyll, which would publish `AGENTS.md` and `CLAUDE.md` as pages. `_config.yml` excludes them; exclude any new repository-only file the same way.

## Development

Open a page directly in a browser to preview it. To see folder paths and redirects as GitHub Pages serves them, run `python3 -m http.server 8000` in the repository root and visit `http://localhost:8000`. That preview differs from GitHub Pages in two ways:

- It shows its own error page for missing paths. GitHub Pages serves `404.html` instead, so preview that page at `/404.html`.
- On macOS's case-insensitive filesystem it serves `/X/` as `/x/`. GitHub Pages matches paths case-sensitively, so live, only the exact lowercase paths work.

## Pages

- **`index.html`** — The homepage: "AD MAIOREM DEI GLORIAM", centered.
- **`portfolio/index.html`** — Startup investments, served at `/portfolio/`.
- **`404.html`** — GitHub Pages serves this at any missing path, so every same-site link or asset in it must start with `/`; a document-relative path would break below the root.
- **`resume/`, `x/`, `venmo/`, `flickr/`** — Redirects; see below.

The homepage and 404 headings are Latin and marked `lang="la"`. Every styled page carries `translate="no"`, so browser translation cannot replace text that the font subsets and breakpoints were built for.

## Redirects

GitHub Pages has no server-side redirects, so each short path is a folder holding an `index.html` with a zero-delay meta refresh. The folder form makes both `/x` and `/x/` work. A meta refresh replaces the current history entry, so Back skips the redirect page.

To add a redirect, copy `x/index.html` into a new folder and change the URL in its refresh tag, canonical link, and fallback link, and the destination named in its `og:title`. Keep the rest identical:

- **Blank interstitial.** WebKit keeps a redirect page on screen while the destination loads, so the page is a blank Sand screen with no margin and the title `Redirecting…`.
- **Preview title.** Link previews in Messages, Slack and similar apps do not follow meta refresh, so `og:title` names the destination (for example "Edmund Loo on X") while the tab reads `Redirecting…`.
- **Fallback link.** It reads `Continue` (on `/resume`, `Résumé`, because browsers that download the PDF leave the visitor on this page) and is hidden with `visibility`, not just opacity, for 3 seconds, so a tap during the hop cannot add a history entry. It then fades in, which serves browsers that block automatic refresh and destinations slower than 3 seconds. A tap on the visible link does add an entry; no CSS can prevent that.
- **Viewport meta.** It stops phones from laying the page out 980px wide.
- **No web fonts.** A font download can delay the load event that the refresh waits for.
- **Literal colors.** The page repeats the Sand values from the token blocks as literals; change both together.

Do not add `noindex`, and do not list redirects in `sitemap.xml`.

## Design system

- **Color** — Radix Colors Sand, with light and dark mode via `prefers-color-scheme`. Steps follow Radix's usage roles: step 1 for the background, step 12 for text, step 7 for link underlines, and step 8 for focus outlines. Values (light / dark): step 1 `#fdfdfc` / `#111110`, step 7 `#cfceca` / `#494844`, step 8 `#bcbbb5` / `#62605b`, step 11 `#63635e` / `#b5b3ad`, step 12 `#21201c` / `#eeeeec`.
- **Links** — Portfolio links ease to step 11 text and fade their underline over 0.3s on keyboard focus, and on hover only inside `@media (hover: hover)`, because touch screens keep `:hover` after a tap. Reduced motion removes the easing.
- **Tokens** — Each styled page declares the CSS custom properties it uses on `:root`. Because there is no build step or shared stylesheet, the token block is repeated in each page. Keep shared values identical across pages and the redirect pages' literals.
- **Typography** — Cinzel, a capitals-only serif, is the default font. Inter is the sans-serif counterpart, used for the portfolio list. Both are SIL Open Font License fonts at weight 400.
- **Inlined font subsets** — Each styled page embeds, as a base64 `data:` URL in an `@font-face` rule, a subset containing only the characters set in that font (including no-break spaces), so pages make no font requests. The comment above each rule records the license and the Google Fonts `text=` value it was generated from. When you change displayed text, regenerate the subset: request `https://fonts.googleapis.com/css2?family=<Family>&text=<characters>` with a current Chrome User-Agent, download the woff2 file it references, and base64-encode it into the rule. Otherwise new characters render in a fallback font. The Content Security Policy allows fonts only from `data:` URLs.
- **Letter-spacing** — Tracked text gets an extra leading inset equal to its letter-spacing, because current browsers add the spacing after the last letter. The CSS Text specification is moving to symmetric spacing trimmed at line edges, and Firefox Nightly already uses it; when browsers ship that, remove the extra inset.

## Small viewports

Every page must stay legible down to the tiniest viewport, with no sideways scrolling, and one-heading pages must not scroll at all. Media queries in `em`, with inclusive `<=` bounds, switch to a shorter form using one of two patterns:

- **Initials** — The homepage motto, the portfolio rows, and the redirect link write each word as its first letter followed by `<span class="rest">…</span>`, which hides on narrow screens. The motto's initials spell `AMDG`. No-break spaces in the motto leave a single break, between `MAIOREM` and `DEI`.
- **Two spans** — The 404 heading holds `NON INVENTUM` in `.full` and `404` in `.short`.

Narrower still, the homepage and 404 headings stack vertically when the screen is tall enough, and otherwise drop their padding and letter-spacing. No heading or link may wrap, except the homepage motto's two lines. Each breakpoint is that tier's rendered size, measured with the final font subsets loaded, plus a small margin. If you change displayed text, re-measure and move the breakpoints.

## Metadata

- `sitemap.xml` lists `/`, `/portfolio/`, and the résumé PDF, and `robots.txt` points crawlers to it. Update `lastmod` when a listed page changes.
- By design the styled pages have no meta description, Open Graph or Twitter tags, or structured data; search snippets and link previews use the title and page text. Only the redirect pages carry an `og:title`.
- The site has no icon files or web app manifest by design. Every HTML page declares `<link rel="icon" href="data:," />` so browsers do not request the missing `/favicon.ico`. The résumé PDF cannot declare one, so viewing it still requests `/favicon.ico` and gets the 404 page.
- `BingSiteAuth.xml` verifies the site with Bing Webmaster Tools; keep it. `CNAME` sets the custom domain `edmundloo.com`.
