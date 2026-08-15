# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file static site for City Square Shopping Center (Honolulu, HI). Everything lives in `index.html` — no build process, no dependencies, no server required for basic preview.

Preview over HTTP rather than `file://` so the hero video and `photo/` assets load the way they will in production:

```
python -m http.server 8000
```

## Deployment

`.github/workflows/static.yml` publishes the **entire repo root** to GitHub Pages on every push to `master`. `index.html` is what visitors get.

Two consequences worth remembering:

- Anything committed under `photo/` ships to production, whether or not a page references it.
- GitHub rejects any file over **100 MB**. Raw camera/drone footage must be re-encoded before it can be committed at all.

## Architecture

Three parts, all in `index.html`:

1. **HTML** structure
2. **CSS** in a single `<style>` block (lines ~10–1075)
3. **JavaScript** — a one-liner in `<head>` that disables scroll restoration, plus a ~45-line block before `</body>`

The JS handles: mobile hamburger menu toggle, scroll up/down arrow buttons with show/hide at the extremes, and hero video resume after tab backgrounding or bfcache restore (mobile browsers pause autoplaying video).

### Design tokens

CSS custom properties on `:root` at the top of the `<style>` block:

- Neutrals — `--ink`, `--white`, `--gray-1/2/3`, `--border`
- Greens — `--leaf`, `--leaf-deep`, `--leaf-light`
- Accents — `--sun` (orange), `--gold`
- Shape — `--radius`, `--radius-lg`, `--shadow`, `--shadow-hover`

Typography: **Fraunces** (display/headings) + **Inter Tight** (body), loaded from Google Fonts.

### Responsive breakpoints

`1024px`, `900px` (the main one — hamburger nav, single-column layouts), and `600px`.

The mobile nav dropdown is `position: fixed; top: 96px`, hardcoded to match `.nav { height: 96px }`. **Change one and you must change the other.**

### Page sections (top to bottom)

Top bar → Navigation → Hero (video) → Category tabs → Marketplace highlight strip → Construction notice → Marketplace vendors → Restaurants → Shops & Services → Visit → Leasing (intro/contact, why, available spaces) → Footer → Scroll nav arrows

Section boundaries are marked with `<!-- ─── Name ─── -->` comments.

### Hero video

Two `<video>` elements, CSS-switched by breakpoint — `.hero-video-desktop` (1080p) and `.hero-video-mobile` (720p). Both need `autoplay muted loop playsinline`; without `muted`, browsers block autoplay.

### Animations

`pulse` — the open-status dot in the top bar. Defined right below `.status-dot`.

## Assets

```
photo/
  logo/           logo2.jpg — used in nav and footer
  market place/   vendor and shop card photos (note the SPACE in the directory name)
  *.mp4           hero video encodes
  Glenn/          raw 2026-08-14 photo/video drop — NOT web-ready, see below
```

Paths with spaces must be URL-encoded in HTML: `photo/market%20place/veggie2.jpg`.

### `photo/Glenn/` — untracked on purpose

A 2.0 GB drop of raw source media (33 photos at 2.4–15.3 MB, 13 videos up to 255 MB). It is **not committed and not yet used by any page.** Ten of the videos exceed GitHub's 100 MB limit.

**Never run `git add .` or `git add -A` in this repo** — it stages the whole 2 GB drop and the push fails. Stage explicit paths.

Before any of it ships, it needs re-encoding: photos down to ~1600px WebP, video to 1080p/720p web encodes. `ffmpeg` is not installed (`winget install Gyan.FFmpeg`); Pillow is available for the photos.
