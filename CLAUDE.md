# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file static HTML mockup for City Square Shopping Center (Honolulu, HI). The entire project lives in `citysquare_mockup.html` — no build process, no dependencies, no server required. Open the file directly in a browser to preview.

## Architecture

**Everything is in one file:** HTML structure, CSS (in `<style>` tags), and no JavaScript. Changes to layout, styles, and content are all made in `citysquare_mockup.html`.

**Design tokens** are defined as CSS custom properties at the top of the `<style>` block:
- `--ink` / `--paper` — primary dark/light backgrounds
- `--sun` / `--leaf` / `--coral` / `--ocean` / `--gold` — accent colors
- Typography: 'Fraunces' (display/headings) + 'Inter Tight' (body), both from Google Fonts

**Responsive breakpoint:** `@media (max-width: 900px)` handles mobile layout.

**Page sections (top to bottom):**
1. Mockup banner label
2. Top bar (open status + language)
3. Navigation (logo, links, CTA)
4. Hero (headline, collage, stats)
5. Marquee ticker (CSS `scroll` animation, infinite)
6. Categories grid
7. Marketplace feature block
8. Visit/hours/map section (pulsing pin via `pulse-pin` animation)
9. Footer

**CSS animations defined:** `pulse` (status dot), `wobble` (sticker), `scroll` (marquee), `pulse-pin` (map pin).
