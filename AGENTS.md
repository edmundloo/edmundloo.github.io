# AGENTS.md

Instructions for AI coding agents working in this repository.

## Principles

- **No JavaScript.** HTML and CSS only.
- **No build system, bundler, or dependencies.** Keep it zero-dependency.
- **Minimalist.** Less is more. Don't add what isn't needed.
- **Fast.** Every byte matters. Inline CSS, no external stylesheets, minimal markup.
- **High quality code.** Clean, semantic HTML. Well-organized CSS. No hacks.

## Overview

Personal website for Edmund Loo, hosted via GitHub Pages at edmundloo.com. The entire site is a single `index.html` with inline CSS.

## Development

Open `index.html` directly in a browser to preview. No build step, no local server required.

## Architecture

- **`index.html`** — The entire site: markup, styles (inline `<style>` block), and structured data (JSON-LD).
- **Color theme** — Everforest, with light/dark mode via `prefers-color-scheme` media queries. CSS custom properties defined in `:root` and overridden in the dark media query.
- **Typography** — IBM Plex Mono loaded from Google Fonts.
- **Responsive design** — Progressive degradation at 300px, 200px, and 100px breakpoints. Footer stacks vertically at narrow widths; content hides at very narrow widths showing only initials "E.L.".
- **SEO/meta** — Open Graph, Twitter cards, JSON-LD Person schema, sitemap.xml, robots.txt, web app manifest.
- **CNAME** — Custom domain `edmundloo.com`.
