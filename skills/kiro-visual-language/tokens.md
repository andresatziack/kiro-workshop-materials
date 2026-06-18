# Kiro Visual Language — Tokens Reference

Human-readable companion to [`tokens.css`](./tokens.css). Use the CSS file as
the drop-in source of truth; use this file to understand intent, pick values,
and wire Tailwind. All values are recreated from observation of kiro.dev — no
proprietary assets.

> Colors below show **HSL channels** (the form stored in `tokens.css`, usable as
> `hsl(var(--token) / alpha)`) plus an approximate **hex** for quick reference.

---

## 1. Color

### 1.1 Brand violet — the only saturated color
Used for primary buttons, links, focus rings, selection, and a few highlighted
words. Do **not** introduce a second saturated hue for general UI.

| Token            | HSL            | Hex (approx) | Use                                  |
|------------------|----------------|--------------|--------------------------------------|
| `--k-purple-300` | `264 100% 81%` | `#C59EFF`    | link hover (dark), light accents     |
| `--k-purple-400` | `263 100% 69%` | `#9F63FF`    | link default (dark)                  |
| `--k-purple-500` | `264 100% 64%` | `#9046FF`    | **PRIMARY** — CTA, focus ring        |
| `--k-purple-600` | `270 79% 53%`  | `#870FFF`    | pressed / link (light)               |
| `--k-purple-700` | `274 87% 43%`  | `#7C00DB`    | gradient end, deep accent            |
| `--k-purple-900` | `264 64% 32%`  | `#4100A3`    | darkest tint, subtle fills           |

### 1.2 Neutrals — "prey" (purple-tinted grays)
The neutrals carry a faint cool/purple tint; this is why Kiro never looks like a
plain grayscale site. Avoid pure `#000`/`#888` neutrals.

| Token            | HSL          | Hex       | Typical role                      |
|------------------|--------------|-----------|-----------------------------------|
| `--k-prey-100`   | `260 12% 95%`| `#F2F0F4` | light bg subtle / light surface   |
| `--k-prey-200`   | `264 7% 86%` | `#DAD8DE` | light borders                     |
| `--k-prey-300`   | `263 7% 76%` | `#C1BEC6` | muted text (dark)                 |
| `--k-prey-400`   | `260 6% 58%` | `#938F9B` | subtle/tertiary text              |
| `--k-prey-600`   | `267 6% 29%` | `#4A464F` | strong border                     |
| `--k-prey-700`   | `266 13% 21%`| `#352F3D` | default border (dark), hairlines  |
| `--k-prey-900`   | `266 14% 10%`| `#19161D` | **signature dark background**     |

### 1.3 Dark surface elevation
Layer surfaces by lightening the purple-tinted near-black.

| Token            | Hex       | Use                         |
|------------------|-----------|-----------------------------|
| `--k-elevation-0`| `#000000` | full-bleed section bands    |
| `--k-elevation-1`| `#19161D` | page background             |
| `--k-elevation-2`| `#29242F` | cards, raised panels        |
| `--k-elevation-3`| `#352F3D` | popovers, menus, tooltips   |

### 1.4 Status / functional accents
Reserve these for semantics (success, error, info), not decoration.

| Token           | Hex       | Meaning      |
|-----------------|-----------|--------------|
| `--k-green-400` | `#33D480` | success      |
| `--k-red-400`   | `#FB4257` | error / destructive |
| `--k-blue-400`  | `#69A0F6` | info / links in code |
| `--k-teal-400`  | `#4FCBD6` | secondary accent |

### 1.5 Semantic theme tokens
Build with these, not raw scales, so light/dark swap automatically.

| Semantic            | Dark            | Light           |
|---------------------|-----------------|-----------------|
| `--k-bg`            | `#19161D`       | `#FFFFFF`       |
| `--k-bg-subtle`     | `#000000`       | `#F2F0F4`       |
| `--k-surface`       | `#29242F`       | `#FFFFFF`       |
| `--k-fg`            | `#FAFAFA`       | `#09090B`       |
| `--k-fg-muted`      | `#C1BEC6`       | `#4A464F`       |
| `--k-border`        | `#352F3D`       | `#DAD8DE`       |
| `--k-primary`       | `#9046FF`       | `#9046FF`       |
| `--k-link`          | `#9F63FF`       | `#870FFF`       |
| `--k-ring`          | `#9046FF`       | `#9046FF`       |

### 1.6 Contrast guidance (accessibility)
- Body text `--k-fg` (`#FAFAFA`) on `--k-bg` (`#19161D`) ≈ **15:1** — passes AAA.
- Muted text `--k-fg-muted` on dark bg ≈ **7:1** — passes AA for normal text.
- **Do not** put `--k-purple-500` text on dark bg for body copy (≈3.2:1, fails).
  Use purple only for large text/icons or on a light/white surface. For links on
  dark, prefer `--k-purple-300/400` which lift contrast.
- Primary button: white text on `#9046FF` ≈ 4.0:1 — acceptable for large/bold
  button labels (≥ 18.66px bold or ≥ 24px). For small labels, darken to
  `--k-purple-600`.

---

## 2. Typography

### 2.1 Families (open substitutes — never ship AWS fonts)
| Role    | Variable          | Recommended open font                         |
|---------|-------------------|-----------------------------------------------|
| Display | `--k-font-display`| **Space Grotesk** (or Geist Mono / JetBrains Mono) |
| Body    | `--k-font-body`   | **Inter** (or Geist, system UI)               |
| Code    | `--k-font-code`   | **JetBrains Mono** (or Fragment Mono, IBM Plex Mono) |

The signature trait is **monospaced / semi-mono headings**. Keep body legible.

### 2.2 Fluid scale (clamp)
Kiro scales type fluidly. The CSS uses `cqw` (container query width); if you have
no container-query context, swap `cqw` → `vw` (values still work well).

| Token          | clamp                                | Pixel range | Use            |
|----------------|--------------------------------------|-------------|----------------|
| `--k-text-xs`  | `clamp(.75rem, .8cqw, .75rem)`       | 12px        | labels, eyebrow|
| `--k-text-sm`  | `clamp(.75rem, 1cqw, .875rem)`       | 12–14px     | captions, meta |
| `--k-text-base`| `clamp(.875rem, 1.1cqw, 1rem)`       | 14–16px     | body           |
| `--k-text-lg`  | `clamp(1.2rem, 1.7cqw, 1.5rem)`      | 19–24px     | lead paragraph |
| `--k-text-2xl` | `clamp(1.6rem, 2.2cqw, 2rem)`        | 26–32px     | section title  |
| `--k-text-3xl` | `clamp(1.8rem, 2.5cqw, 2.25rem)`     | 29–36px     | large title    |
| `--k-text-4xl` | `clamp(2.4rem, 3.3cqw, 3rem)`        | 38–48px     | sub-hero       |
| `--k-text-5xl` | `clamp(3.4rem, 4.7cqw, 4.25rem)`     | 54–68px     | **hero**       |

### 2.3 Weights, line-height, tracking
- Weights: 400 regular, 500 medium (most headings), 600 semibold, 700 bold.
- Line-height: `1.1` display, `1.25` sub-heads, `1.6` body.
- Tracking: large mono display reads better slightly tight (`-0.02em`). Body: 0.
- Headings are typically **medium (500)**, not heavy — the mono face provides
  presence, so avoid 700+ on large display.

---

## 3. Spacing, radii, shadows, layout

### 3.1 Spacing (8px rhythm)
`4, 8, 12, 16, 24, 32, 48, 64, 96, 128` px → `--k-space-1 … --k-space-10`.
Section vertical padding is generous: **96–128px** desktop, ~64px mobile.

### 3.2 Radii
| Token            | Value   | Use                       |
|------------------|---------|---------------------------|
| `--k-radius`     | 4px     | inputs, small buttons     |
| `--k-radius-md`  | 8px     | buttons, badges, code box |
| `--k-radius-lg`  | 12px    | cards                     |
| `--k-radius-xl`  | 16px    | large cards, media        |
| `--k-radius-2xl` | 24px    | hero panels, big surfaces |
| `--k-radius-full`| pill    | tags, avatars, chips      |

### 3.3 Shadows (soft, low-contrast)
- `--k-shadow-md`: default raised card.
- `--k-shadow-lg` / `--k-shadow-dark`: floating panels, modals on dark.
- `--k-glow-purple`: optional violet glow for the primary CTA or active state.
- Prefer **borders over shadows** on dark surfaces; shadows read weakly on
  near-black, so a 1px `--k-border` often does the separation work.

### 3.4 Layout
- Container max-width **1280px** (`--k-container`); wide bands up to **1480px**.
- Prose/text measure **~720px** (`--k-measure`) for readability.
- Side gutters: 24px mobile, growing to auto-centered margins on desktop.
- Density: comfortable. Cards have 24–32px internal padding.

---

## 4. Tailwind mapping

Mirror tokens into `tailwind.config.{js,ts}`. Two common approaches:

### Option A — reference the CSS variables (recommended; keeps one source)
Import `tokens.css` globally, then:

```js
// tailwind.config.js
const hsl = (v) => `hsl(var(${v}) / <alpha-value>)`;

module.exports = {
  darkMode: ["class", '[data-theme="dark"]'],
  theme: {
    extend: {
      colors: {
        // semantic (recommended for components)
        bg:        "var(--k-bg)",
        surface:   "var(--k-surface)",
        fg:        "var(--k-fg)",
        "fg-muted":"var(--k-fg-muted)",
        border:    "var(--k-border)",
        primary:   "var(--k-primary)",
        link:      "var(--k-link)",
        // raw scales (when you need alpha math)
        purple: {
          300: hsl("--k-purple-300"),
          400: hsl("--k-purple-400"),
          500: hsl("--k-purple-500"),
          600: hsl("--k-purple-600"),
          700: hsl("--k-purple-700"),
          900: hsl("--k-purple-900"),
        },
        prey: {
          100: hsl("--k-prey-100"), 300: hsl("--k-prey-300"),
          400: hsl("--k-prey-400"), 600: hsl("--k-prey-600"),
          700: hsl("--k-prey-700"), 900: hsl("--k-prey-900"),
        },
      },
      fontFamily: {
        display: ["Space Grotesk", "Geist Mono", "JetBrains Mono", "monospace"],
        sans:    ["Inter", "Geist", "ui-sans-serif", "system-ui", "sans-serif"],
        mono:    ["JetBrains Mono", "Fragment Mono", "ui-monospace", "monospace"],
      },
      borderRadius: {
        DEFAULT: "0.25rem", md: "0.5rem", lg: "0.75rem",
        xl: "1rem", "2xl": "1.5rem",
      },
      maxWidth: { container: "1280px", wide: "1480px", measure: "720px" },
      boxShadow: {
        md: "0 4px 6px -1px rgba(0,0,0,.10), 0 2px 4px -1px rgba(0,0,0,.06)",
        lg: "0 12px 32px -8px rgba(0,0,0,.45)",
        glow: "0 0 0 1px hsl(var(--k-purple-500)/.35), 0 8px 30px -8px hsl(var(--k-purple-500)/.45)",
      },
      transitionTimingFunction: {
        out: "cubic-bezier(0,0,.2,1)",
        in:  "cubic-bezier(.4,0,1,1)",
        "in-out": "cubic-bezier(.4,0,.2,1)",
      },
    },
  },
};
```

### Option B — hardcode hex (no CSS-vars dependency)
Use the hex values from sections 1.1–1.4 directly in `theme.extend.colors`. You
lose automatic light/dark swapping and alpha-compositing, so prefer Option A.

### Fluid type in Tailwind
Add to `fontSize`:
```js
fontSize: {
  hero: ["clamp(3.4rem,4.7vw,4.25rem)", { lineHeight: "1.1", letterSpacing: "-0.02em" }],
  "display": ["clamp(2.4rem,3.3vw,3rem)", { lineHeight: "1.1" }],
  "title": ["clamp(1.6rem,2.2vw,2rem)", { lineHeight: "1.25" }],
}
```
(Use `vw` here unless your layout establishes a container-query context, then
swap to `cqw`.)
