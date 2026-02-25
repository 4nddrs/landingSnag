# SNAG — Landing Page

Official landing page for [SNAG](https://github.com/4nddrs/snag), a lightweight, Vim-inspired TUI API client built with Go and Bubble Tea.

## What is SNAG?

SNAG is an open source terminal UI for instant OpenAPI discovery and testing. Feed it a local file or remote URL pointing to any OpenAPI 3.x spec and it surfaces every endpoint, schema, and parameter — all navigable from the keyboard without ever leaving the terminal.

Key features:
- **Vim-style navigation** — `j/k` to move, `tab` to switch panels, `r` to fire a request
- **Universal OpenAPI support** — works with FastAPI, Echo, Express, Axum, or any compliant generator
- **No-JSON editing** — interactive prompts for every parameter, auto-filled from the spec
- **Single binary** — no runtime, no heavy dependencies, starts in milliseconds
- **Cross-platform** — macOS, Linux, and Windows

## About this landing page

A single-page dark-mode site that showcases SNAG with an interactive TUI simulation, feature cards, installation instructions, and a video trailer. Built to match the aesthetic of the app itself: terminal-native, Catppuccin Mocha palette, JetBrains Mono throughout.

**Tech stack:**
- [Astro](https://astro.build) 5.17.1
- [Tailwind CSS](https://tailwindcss.com) v4 (CSS-first, no config file)
- JetBrains Mono via Google Fonts
- Pure CSS animations — no animation libraries

## Project Structure

```
src/
  styles/global.css       <- design tokens, animations, custom utilities
  layouts/Layout.astro    <- HTML shell, SEO, favicons, scroll reveal
  pages/index.astro       <- single page entry point
  components/
    Hero.astro            <- full-screen hero with install command
    Showcase.astro        <- interactive TUI demo + video trailer
    Features.astro        <- feature cards
    TechSpecs.astro       <- specs grid + installation methods
    Footer.astro          <- links and attribution
  assets/
    favicon/              <- favicon sources (copied to public/)
    trailerSnag.mov       <- product trailer video
public/                   <- static assets served at root
```

> See [Agent.md](./Agent.md) for the full design system reference: palette, typography, animations, reusable patterns, and code conventions.

## Commands

Run from the root of the project:

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start dev server at `localhost:4321`        |
| `npm run build`   | Build for production to `./dist/`           |
| `npm run preview` | Preview the production build locally        |

## Links

- App repository: [github.com/4nddrs/snag](https://github.com/4nddrs/snag)
- Author: [4nddrs](https://4nddrs.netlify.app)
- License: MIT
