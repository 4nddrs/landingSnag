# Agent.md — SNAG Landing Page

Complete reference document for continuing development or replicating the style of this landing page.

---

## Tech Stack

| Piece | Version / detail |
|---|---|
| Framework | Astro 5.17.1 |
| CSS | Tailwind CSS v4 (`@tailwindcss/vite`, **CSS-first**, no `tailwind.config.*`) |
| Typography | JetBrains Mono (Google Fonts) |
| Language | TypeScript (frontmatter in `.astro` files) |
| Bundler | Vite (via Astro) |
| Dynamic assets | `import url from '../assets/file'` — Vite resolves the hash |

### Install and run
```bash
npm install
npm run dev      # localhost:4321
npm run build    # dist/
npm run preview  # serves dist/
```

---

## File Structure

```
src/
  styles/
    global.css          ← @theme tokens, fonts, animations, custom utilities
  layouts/
    Layout.astro        ← HTML shell, SEO meta, favicons, IntersectionObserver
  pages/
    index.astro         ← single page, composes the 5 components in order
  components/
    Hero.astro          ← full-screen hero section
    Showcase.astro      ← interactive TUI demo + video trailer tab
    Features.astro      ← 3 feature cards
    TechSpecs.astro     ← specs grid + installation methods
    Footer.astro        ← links, attribution, terminal easter egg
  assets/
    favicon/            ← favicon sources (copied to public/)
    trailerSnag.mov     ← trailer video (imported via Vite)
public/
  favicon-96x96.png
  favicon.svg
  favicon.ico
  apple-touch-icon.png
  site.webmanifest
  web-app-manifest-192x192.png
  web-app-manifest-512x512.png
```

---

## Color Palette — Catppuccin Mocha / Dracula-adjacent

The entire palette lives as CSS tokens in `src/styles/global.css` inside the `@theme {}` block.

### Backgrounds (darkest to lightest)
| Token | Hex | Usage |
|---|---|---|
| `--color-bg-secondary` | `#0e0e16` | Deepest background, near-black |
| `--color-bg-terminal` | `#0d0d14` | Code boxes / terminal surfaces |
| `--color-bg-primary` | `#11111b` | Base background for the entire page — `body` |
| `--color-bg-panel` | `#131320` | Inner panels |
| `--color-bg-card` | `#1a1a2e` | Cards |
| `#1e1e2e` | — | Active tab, elevated surface (Tailwind inline) |
| `#0a0f1e` | — | Code blocks in TechSpecs (Tailwind inline) |

### Borders
| Token | Hex | Usage |
|---|---|---|
| `--color-border` | `#2a2a3d` | General borders |
| `--color-border-muted` | `#1a1a28` | Very subtle borders |
| `#313244` | — | Most-used value directly in Tailwind (`border-[#313244]`) |

### Text
| Token | Hex | Usage |
|---|---|---|
| `--color-text-primary` | `#cdd6f4` | Main text, soft blue-white |
| `--color-text-sub` | `#6c7086` | Subtitles, secondary text |
| `--color-text-muted` | `#585b70` | Dimmed text, placeholders, nav links |
| `--color-text-faint` | `#313244` | Near-invisible text, dividers |
| `#45475a` | — | Stats row in hero, footer links |

### Accents / semantic colors
| Token | Hex | Name | Primary usage |
|---|---|---|---|
| `--color-accent-cyan` | `#89dceb` | Sky cyan | Main brand color, active borders, glows |
| `--color-accent-green` | `#a6e3a1` | Catppuccin green | GET, Homebrew badge, checkmarks ✓, copy success |
| `--color-accent-blue` | `#89b4fa` | Lavender blue | PUT, compatibility, specs |
| `--color-accent-yellow` | `#f9e2af` | Peach yellow | POST, Scoop badge, Go badge |
| `--color-accent-red` | `#f38ba8` | Pink red | DELETE, errors |
| `--color-accent-mauve` | `#cba6f7` | Mauve | Available, not actively used |
| `--color-method-patch` | `#fab387` | Peach | PATCH |

### Color usage rules
- **Cyan** → anything focused, active, selected, primary brand action
- **Green** → success, GET, confirmation, checks
- **Yellow** → POST, soft warning, Windows
- **Red** → DELETE, error
- **Blue** → PUT, compatibility, technical information

---

## Typography

```css
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;600;700&display=swap');

font-family: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace;
```

- **Single font family** across the entire page: JetBrains Mono. No sans-serif.
- Weights used: 300 (inactive), 400 normal, 500 medium, 600 semibold, 700 bold.
- Representative sizes:
  - Hero title: `text-4xl md:text-6xl lg:text-7xl font-bold`
  - Section h2: `text-3xl md:text-4xl font-bold`
  - Section subtitles: `text-xs tracking-widest uppercase` (e.g. `// Why SNAG`)
  - Body / cards: `text-sm` or `text-xs`
  - Long URLs / inline code: `text-[10px]` or `text-[11px]`

---

## Animations & Effects

### Custom utility classes (in `global.css`)

```css
/* Entry with fade + slide-up */
.animate-fade-up  { animation: fadeUp 0.7s ease-out forwards; }
.animate-fade-in  { animation: fadeIn 0.7s ease-out forwards; }

/* Staggered delays */
.delay-100 … .delay-600  { animation-delay: 0.1s … 0.6s; }

/* Blinking terminal cursor — block █ */
.cursor-blink::after {
  content: '█';
  animation: blink 1.1s step-end infinite;
  color: #06b6d4;
}

/* Cyan box glow */
.glow-cyan        { box-shadow: 0 0 30px rgba(137,220,235,0.15); }
.glow-cyan-strong { box-shadow: 0 0 50px rgba(137,220,235,0.25), 0 0 1px rgba(137,220,235,0.4); }

/* Text glow */
.text-glow-cyan  { text-shadow: 0 0 20px rgba(137,220,235,0.5); }
.text-glow-green { text-shadow: 0 0 20px rgba(166,227,161,0.5); }

/* Dot/line grid background */
.grid-bg {
  background-image:
    linear-gradient(rgba(137,220,235,0.025) 1px, transparent 1px),
    linear-gradient(90deg, rgba(137,220,235,0.025) 1px, transparent 1px);
  background-size: 40px 40px;
}
```

### Scroll reveal (IntersectionObserver)
Any element with class `.reveal` starts invisible and appears when it enters the viewport:

```css
.reveal         { opacity: 0; transform: translateY(20px); transition: opacity 0.6s ease, transform 0.6s ease; }
.reveal.visible { opacity: 1; transform: translateY(0); }
```

Observer wired in `Layout.astro`:
```js
const observer = new IntersectionObserver(
  (entries) => entries.forEach((e) => { if (e.isIntersecting) e.target.classList.add('visible'); }),
  { threshold: 0.1, rootMargin: '0px 0px -40px 0px' }
);
document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));
```

### Radial background glows
Implemented as absolute `<div>`s with `blur-[Xpx]` and low opacity:
```html
<!-- Hero example -->
<div class="absolute top-1/3 left-1/2 -translate-x-1/2 -translate-y-1/2
            w-[700px] h-[500px] rounded-full bg-[#89dceb]/5 blur-[120px]">
</div>
```
**Calibration rule**: glow opacity should stay between `/4` and `/5` (very subtle). More than that turns "neon" and breaks the minimalist aesthetic.

---

## General Aesthetic — Design Principles

1. **Terminal-native**: the entire UI mimics a real terminal. macOS window chrome, monospace font, syntax-highlight colors.
2. **Extreme dark**: background `#11111b`, near-black but never pure black. Never use `#000000`.
3. **Minimalism with character**: no aggressive border-radius or flashy shadows. Typical radii: `rounded-lg` (8px), `rounded-xl` (12px), `rounded-2xl` (16px).
4. **Subtle glow, not neon**: glow effects exist but are faint. Rule: if it's too noticeable, halve the opacity.
5. **Hierarchy through color, not size**: accent colors guide the eye. Cyan is always the most visually important element.
6. **No decorative images**: all decoration is pure CSS (gradients, blur, grids). Zero stock photos or third-party icons (except inline GitHub SVGs).

---

## Components — Technical Summary

### `Layout.astro`
- HTML shell with SEO meta, full favicons, and `<slot />`.
- Imports `global.css` in the Astro frontmatter (loaded via Vite).
- IntersectionObserver script for `.reveal`.
- `<meta name="theme-color" content="#0f172a">` for mobile browser bar color.

### `Hero.astro`
- Full-screen (`min-h-screen`) with `grid-bg` background.
- **Nav**: `⚡ SNAG` logo left + internal anchor links + GitHub button right.
- Animated **badge** with `animate-pulse` green dot.
- **Title**: line 1 in `#cdd6f4`, line 2 `#89dceb` + `.cursor-blink` + subtle `text-shadow`.
- **Install box**: `brew install 4nddrs/tap/snag` with copy-to-clipboard button (`copyInstall()` on `window`).
- **CTAs**: "View on GitHub" (bg `#cdd6f4`, text `#11111b`) + "Watch Demo" (border `#89dceb/20`).
- **Stats row**: 4 items with green `✓`, separated by `border-t`.

### `Showcase.astro`
The most complex section. Simulates the real SNAG TUI.

**Tab switcher** between two panels:
- `tab-active`: `bg-[#1e1e2e] text-[#cdd6f4]`
- `tab-inactive`: `bg-transparent text-[#585b70]`
- Global `switchTab(tab)` function on `window`, pauses/plays the video.

**Demo panel** (`id="panel-demo"`): simulated terminal with:
- macOS chrome (3 dots: red `#ff5f57`, yellow `#febc2e`, green `#28c840`)
- Title bar `SNAG — API Client`
- Left panel (260px): grouped endpoint list with method colors
- Top-right panel: Request section with URL, Method, Headers, Body fields
- Bottom-right panel: Response section with cyan focus border (`border-[#89dceb]`)
- Bottom keybindings bar: `j/k scroll · d/u page · g/G top/bottom · y copy · tab switch panel · 200 5ms`

**Trailer panel** (`id="panel-trailer"`): same macOS chrome + `<video>` with `controls`, initial volume `0.5` set via JS. Auto-plays when clicking the tab.

**HTTP method colors** (map in frontmatter):
```ts
const methodColor: Record<string, string> = {
  GET:    '#a6e3a1',  // green
  POST:   '#f9e2af',  // yellow
  DELETE: '#f38ba8',  // red
  PUT:    '#89b4fa',  // blue
  PATCH:  '#fab387',  // peach
};
```

### `Features.astro`
3-column grid (`md:grid-cols-3`). Each card has:
- Icon in a box with `background: {accent}15; border-color: {accent}30`
- Tag badge with the same opacity scheme
- Title, description with `set:html` (allows inline `<kbd>`)
- Key/value keybindings list
- Italic quote at the bottom (`text-[#585b70] italic border-l border-[#313244]`)

### `TechSpecs.astro`
- 4-column specs card grid with hover `scale-[1.02]`
- Two install columns:
  - **Left**: Homebrew (macOS/Linux) + Scoop (Windows, 2 lines) + Go install (All platforms)
  - **Right**: From Source (git clone → go build → ./snag)
- Each command line has a cyan `$` prompt + code in `#cdd6f4`

### `Footer.astro`
- Centered logo + GitHub / MIT License / Issues links
- Attribution: "Created by 4nddrs · Built with Go & Bubble Tea"
- Terminal easter egg: `~ snag --version v1.0.1` in `text-[11px]` with syntax colors

---

## Favicons

Source files in `src/assets/favicon/`. Must be copied to `public/` for Astro to serve them:
```powershell
Copy-Item "src\assets\favicon\*" "public\" -Force
```

Tags in `Layout.astro`:
```html
<link rel="icon" type="image/png" href="/favicon-96x96.png" sizes="96x96" />
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
<link rel="shortcut icon" href="/favicon.ico" />
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
<meta name="apple-mobile-web-app-title" content="snag" />
<link rel="manifest" href="/site.webmanifest" />
```

---

## Video Assets

The trailer is imported with Vite to get the build-hashed URL:
```astro
---
import trailerUrl from '../assets/trailerSnag.mov';
---
<video controls preload="metadata">
  <source src={trailerUrl} type="video/mp4" />
  <source src={trailerUrl} type="video/quicktime" />
</video>
```

> **Compatibility**: `.mov` only plays natively in Safari. For Chrome/Firefox, convert to H.264 `.mp4`:
> ```bash
> ffmpeg -i trailerSnag.mov -vcodec h264 -acodec aac trailerSnag.mp4
> ```

---

## Reusable Patterns

### Code / terminal command line
```html
<div class="flex items-start gap-3 px-3 py-2 rounded-lg bg-[#0a0f1e] border border-[#313244]">
  <span class="text-[#89dceb] shrink-0 mt-0.5">$</span>
  <span class="text-[#cdd6f4]">command here</span>
</div>
```

### Platform / version badge
```html
<span class="text-[10px] px-2 py-0.5 rounded-full
             bg-[#a6e3a1]/10 text-[#a6e3a1] border border-[#a6e3a1]/20">
  macOS / Linux
</span>
```
Swap the accent color (`#a6e3a1`) for any other palette color to create variants.

### Section header (uniform pattern)
```html
<div class="text-center mb-16 reveal">
  <p class="text-xs text-[#a6e3a1] tracking-widest uppercase mb-3 font-medium">// Subtitle</p>
  <h2 class="text-3xl md:text-4xl font-bold text-[#cdd6f4]">Main Title</h2>
  <p class="text-[#585b70] text-sm mt-3 max-w-md mx-auto">Brief description.</p>
</div>
```

### Radial section background glow
```html
<div class="absolute inset-0 pointer-events-none">
  <div class="absolute bottom-0 left-1/2 -translate-x-1/2
              w-[600px] h-[300px] rounded-full bg-[#89b4fa]/5 blur-[100px]">
  </div>
</div>
```

### Card with hover elevation
```html
<div class="reveal group p-5 rounded-xl border border-[#313244] bg-[#0d0d14]
            hover:scale-[1.02] transition-all duration-300 cursor-default">
  <!-- content -->
</div>
```

### macOS window chrome
```html
<div class="px-4 py-3 bg-[#0d0d14] border-b border-[#313244] flex items-center gap-2">
  <div class="w-3 h-3 rounded-full bg-[#ff5f57]"></div>
  <div class="w-3 h-3 rounded-full bg-[#febc2e]"></div>
  <div class="w-3 h-3 rounded-full bg-[#28c840]"></div>
  <span class="text-[#45475a] text-xs mx-auto">Window Title</span>
</div>
```

---

## Global Scrollbar
```css
::-webkit-scrollbar       { width: 6px; }
::-webkit-scrollbar-track { background: #0f172a; }
::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: #06b6d4; }
```

---

## Code Conventions

- **Astro frontmatter**: data and logic in TypeScript inside `---`.
- **Tailwind v4**: no config file. Custom tokens in `@theme {}` inside `global.css`. Referenced as `bg-bg-primary` (with the token prefix) or directly as `bg-[#hex]`.
- **Interactive scripts in components**: global functions are assigned to `(window as any).functionName = fn` inside a `<script>` block in the `.astro` file.
- **Dynamic HTML**: use Astro's `set:html={string}` when content includes HTML tags (e.g. `<kbd>`, `<span>`).
- **Staggered animation delays**: use `.delay-100` through `.delay-600` + `opacity-0` on the element so the animation waits (Tailwind doesn't include these delays by default — they are defined in `global.css`).
