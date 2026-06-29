# Thec Laire

A distraction-free web novel reader, built on one principle: the best reading UI is invisible.

**Live site:** https://zedchuu.github.io/04-Thec-Laire/

Thec Laire is a lightweight reading platform for serial fiction. It's plain HTML, CSS, and JavaScript, with no backend, no frameworks, and no build step. It was built both to publish the serial *Resonance* and as a way to practice UI/UX and accessibility from the ground up.

## Design principle

Everything serves the reading experience. The interface stays out of the way until you reach for it, then offers exactly the controls you'd want (theme, type size, spacing) and nothing more. No accounts, no popups, no clutter.

## Features

- Three reading themes: day, sepia, and night. On a first visit it falls back to your operating system's dark-mode preference.
- Adjustable font size and line spacing.
- Scroll progress bar so you always know where you are in a chapter.
- Auto-hiding top bar that gets out of the way while you read.
- Slide-out settings panel for quick adjustments.
- Typographic touches: drop caps and styled scene breaks.
- Focus mode (press F) to strip the page down to just the text.
- Theme, font size, and line spacing persist across visits via localStorage.

## Accessibility

Accessibility is a first-class concern, not an afterthought:

- Skip-to-content link for keyboard and screen-reader users.
- Visible focus rings via :focus-visible.
- prefers-reduced-motion support to honor system motion settings.
- ARIA-pressed states on toggle controls.
- WCAG-compliant color contrast across all themes, including night mode.

## How it works

The core idea is a strict separation of content from presentation. Chapter text lives in Markdown files; the reader is just a template that displays any of them.

- **index.html** is the homepage: the chapter list and the theme switcher.
- **reader.html** is a single dynamic template. It reads a `?ch=` parameter from the URL, looks that chapter up in a `CHAPTERS` manifest array, then uses `fetch()` to pull the matching `.md` file and `marked.js` to render it to HTML.
- **base.css** is the single source of truth for design tokens (colors, fonts, timing). All theming is driven by CSS variables, so swapping a theme is swapping roughly eight variables.

## Tech stack

- Vanilla HTML, CSS, and JavaScript (no frameworks, no build tooling)
- marked.js (v9.1.6) for Markdown rendering
- GitHub Pages for hosting

## Project structure

```
04-Thec-Laire/
├─ index.html        Homepage and chapter list
├─ reader.html       Dynamic reader template (?ch=...)
├─ base.css          Design tokens and theming (CSS variables)
├─ lib/
│  └─ marked.min.js  Markdown renderer
└─ arc 1/
   ├─ chapter1-weird.md
   ├─ chapter2-weird-too.md
   └─ ...             One Markdown file per chapter
```

## Running locally

Because the reader uses `fetch()` to load Markdown files, opening `index.html` directly with the `file://` protocol will not work, the browser blocks those requests. Serve the folder over HTTP instead:

```bash
python3 -m http.server
# then open http://localhost:8000
```

Any static server works (for example VS Code Live Server or `npx serve`).

## Adding a chapter

1. Add a new `.md` file to the `arc 1/` folder.
2. Register it in the `CHAPTERS` manifest in `reader.html` (id, title, and order).
3. Add a link to it on `index.html`.

## Roadmap

- Text-to-speech
- Reader annotations

## About the story

Thec Laire currently hosts *Resonance*, a first-person YA fantasy serial. Arc 1 is complete at eight chapters. A note for readers: the informal grammar, run-ons, and loose punctuation are deliberate stylistic choices meant to read like a kid's interior voice, not errors.

## Credits

Built by Adam ([zedchuu](https://github.com/zedchuu)). *Resonance* and its text are original work. marked.js is MIT-licensed.
