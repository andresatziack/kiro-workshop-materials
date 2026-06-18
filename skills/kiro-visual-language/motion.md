# Kiro Visual Language — Motion

Motion specs and ready-to-use keyframes. The Kiro feel is **quiet and fast**:
short opacity/transform transitions for interaction, gentle staggered fades for
entrances, and at most one "delight" flourish (animated gradient text or a logo
marquee). Assumes [`tokens.css`](./tokens.css) is imported.

**Golden rule:** motion should feel like a fast confirmation, not a show. If in
doubt, make it shorter and more subtle. Always honor `prefers-reduced-motion`.

---

## 1. Duration & easing scale

| Token (in tokens.css) | Value  | Use                                        |
|-----------------------|--------|--------------------------------------------|
| `--k-dur-fast`        | 150ms  | hover color, small state changes, tap      |
| `--k-dur-base`        | 250ms  | most transitions (color/border/opacity)    |
| `--k-dur-slow`        | 350ms  | transform/lift, larger reveals, accordions |

Other observed durations you can use deliberately: **100ms** (instant feedback),
**200ms** (accordions, dropdowns), **500–750ms** (one-off entrance slides),
**8s** (gradient cycle), **45s** (logo marquee).

| Easing token        | Curve                     | Use                          |
|---------------------|---------------------------|------------------------------|
| `--k-ease-out`      | `cubic-bezier(0,0,.2,1)`  | enter / appear (default)     |
| `--k-ease-in`       | `cubic-bezier(.4,0,1,1)`  | exit / disappear             |
| `--k-ease-in-out`   | `cubic-bezier(.4,0,.2,1)` | move between two on-screen states |
| `linear`            | —                         | marquee, gradient cycling, spinners |

Default to **`--k-ease-out`** for anything entering or responding to the user.

---

## 2. Micro-interactions (transitions)

Apply transitions to specific properties (not `all`) where you can, for
performance and predictability.

```css
/* Links & nav: color only */
a { transition: color var(--k-dur-base) var(--k-ease-out); }

/* Buttons: color/border + a tiny press */
.k-btn {
  transition: background-color var(--k-dur-base) var(--k-ease-out),
              border-color     var(--k-dur-base) var(--k-ease-out),
              color            var(--k-dur-base) var(--k-ease-out),
              transform        var(--k-dur-fast) var(--k-ease-out);
}
.k-btn:active { transform: translateY(1px); }

/* Cards: border tint + subtle lift */
.k-card {
  transition: border-color var(--k-dur-base) var(--k-ease-out),
              transform     var(--k-dur-slow) var(--k-ease-out);
}
.k-card:hover { transform: translateY(-2px); }

/* Generic "soft" hover used widely on kiro.dev */
.k-soft { transition: all var(--k-dur-base) linear; }
```

Guidelines:
- Hover feedback ≈ 150–250ms; never longer than 350ms.
- Lifts are tiny: `translateY(-2px)`; never bounce or overshoot.
- Prefer **border-color** and **opacity** changes over heavy shadows on dark UI.

---

## 3. Entrance: staggered fade-in

Sections and hero elements fade up subtly as they appear. Stagger children by
~50–100ms. Pair with an in-view trigger (IntersectionObserver or a framework
in-view hook) by toggling a class.

```css
@keyframes k-fade-in {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}

.k-animate-in { opacity: 0; }                 /* initial state */
.k-animate-in.is-visible {
  animation: k-fade-in var(--k-dur-base) var(--k-ease-out) forwards;
}

/* Stagger utility: set --i on each child (0,1,2,...) */
.k-stagger > .is-visible { animation-delay: calc(var(--i, 0) * 80ms); }
```
```html
<div class="k-stagger">
  <h2 class="k-animate-in" style="--i:0">Title</h2>
  <p  class="k-animate-in" style="--i:1">Lead</p>
  <a  class="k-animate-in" style="--i:2">Button</a>
</div>
```
Observed delay ladder on kiro.dev: `.1s, .15s, .2s, .25s, .3s, .35s …` up to
~`.55s` — i.e. ~50ms steps. Keep total stagger under ~600ms.

**Tailwind:** use `motion-safe:animate-[k-fade-in_.25s_ease-out_forwards]` or a
plugin like `tailwindcss-animate` (`animate-in fade-in slide-in-from-bottom-2`).

---

## 4. Signature flourish A — animated gradient text

The playful "rainbow" word: a wide gradient slowly cycling its position. Use on
**one** word/phrase max.

```css
@keyframes k-gradient {
  0%   { background-position: 0 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0 50%; }
}
.k-unicorn-text {
  background: var(--k-gradient-rainbow); /* 300deg multi-stop, see tokens.css */
  background-size: 200% 200%;
  -webkit-background-clip: text; background-clip: text;
  color: transparent;
  animation: k-gradient 8s ease infinite;
}
```
A calmer, on-brand alternative is the static violet gradient
(`var(--k-gradient-brand)`) without animation — preferred for a more
"enterprise" tone.

---

## 5. Signature flourish B — logo marquee

Seamless infinite scroll. Duplicate the content set, translate the track by
`-50%`, and mask the edges (markup in [`components.md`](./components.md)).

```css
@keyframes k-scroll {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
.k-marquee__track { animation: k-scroll 45s linear infinite; }
.k-marquee:hover .k-marquee__track { animation-play-state: paused; }
```
Speed: 30–60s for a calm drift. Faster reads as urgent/cheap.

---

## 6. Terminal cursor blink

```css
@keyframes k-cursor-blink {
  0%, 49%   { opacity: 1; }
  50%, 100% { opacity: 0; }
}
.k-cursor { animation: k-cursor-blink 1s step-end infinite; }
```
`step-end` gives the hard on/off blink of a real terminal (no fading).

---

## 7. Disclosure: accordion / dropdown / popover

```css
@keyframes k-accordion-down { from { height: 0; } to { height: var(--radix-accordion-content-height, auto); } }
@keyframes k-accordion-up   { from { height: var(--radix-accordion-content-height, auto); } to { height: 0; } }
@keyframes k-slide-up   { from { opacity: 0; transform: translateY(-.25rem); } to { opacity: 1; transform: translateY(0); } }
@keyframes k-fade-out   { from { opacity: 1; } to { opacity: 0; } }

.k-accordion__content[data-state="open"]  { animation: k-accordion-down .2s var(--k-ease-out); }
.k-accordion__content[data-state="closed"]{ animation: k-accordion-up   .2s var(--k-ease-out); }
.k-popover[data-state="open"]  { animation: k-slide-up .1s var(--k-ease-out); }
.k-popover[data-state="closed"]{ animation: k-fade-out .15s var(--k-ease-in); }
```
Opens are fast (~100–200ms, ease-out); closes are slightly faster (ease-in).

---

## 8. Accessibility — `prefers-reduced-motion`

**Required.** When the user prefers reduced motion, neutralize transforms and
loops; keep instant opacity changes only.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: .001ms !important;
    scroll-behavior: auto !important;
  }
  /* Stop the looping flourishes outright */
  .k-marquee__track,
  .k-unicorn-text,
  .k-cursor { animation: none !important; }
  /* Ensure faded-in content is simply visible */
  .k-animate-in { opacity: 1 !important; transform: none !important; }
}
```
Tailwind exposes `motion-safe:` / `motion-reduce:` variants — prefer guarding
animations with `motion-safe:` so they are opt-in for users who allow motion.

### Other a11y notes for motion
- Never convey information by motion alone (also use text/color/state).
- Keep focus-visible rings instant (no transition) so keyboard users get
  immediate feedback: `:focus-visible` outline is defined in `tokens.css`.
- Avoid parallax and large auto-playing movement; the Kiro look doesn't need it.

---

## Motion budget per page (rule of thumb)

- ✅ Section entrance fades (subtle, once).
- ✅ Hover/press transitions on interactive elements.
- ✅ One flourish: gradient word **or** marquee (not both prominently).
- ✅ Terminal cursor blink if a terminal is shown.
- ❌ Multiple competing loops, bouncy springs, long slides, parallax.
