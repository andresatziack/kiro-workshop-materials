# Kiro Visual Language — Component Recipes

Markup + CSS recipes (with Tailwind equivalents) for the core components that
make a page read as "Kiro". Assumes [`tokens.css`](./tokens.css) is imported
globally so `--k-*` variables resolve. Motion details live in
[`motion.md`](./motion.md).

**Recreate, don't copy:** all text, icons, and logos here are generic
placeholders. Never use Kiro/AWS marks or copy.

Contents: [Layout primitives](#0-layout-primitives) ·
[Buttons](#1-buttons) · [Badges](#2-badges--pills) · [Navbar](#3-navbar) ·
[Hero](#4-hero) · [Code block](#5-code-block) · [Terminal](#6-terminal-window) ·
[Cards](#7-cards) · [Section](#8-section--eyebrow) ·
[Logo marquee](#9-logo-marquee) · [Footer](#10-footer)

---

## 0. Layout primitives

```css
.k-container {            /* centered content column */
  width: 100%;
  max-width: var(--k-container);   /* 1280px */
  margin-inline: auto;
  padding-inline: var(--k-gutter); /* 24px */
}
.k-section { padding-block: var(--k-space-9); }      /* 96px */
@media (min-width: 768px) { .k-section { padding-block: var(--k-space-10); } } /* 128px */
.k-prose { max-width: var(--k-measure); }            /* 720px text measure */
.k-stack > * + * { margin-top: var(--k-space-5); }   /* vertical rhythm */
```
**Tailwind:** `mx-auto w-full max-w-container px-6` · section `py-24 md:py-32` ·
prose `max-w-measure` · stack `space-y-6`.

---

## 1. Buttons

Three variants: **primary** (solid violet), **secondary** (outline/ghost on
surface), **link/tertiary**. Modest radius, mono or sans label, 150–250ms
transitions. Keep labels short.

```html
<a class="k-btn k-btn--primary" href="#">Get started</a>
<a class="k-btn k-btn--secondary" href="#">Documentation</a>
<button class="k-btn k-btn--ghost">Learn more</button>
```

```css
.k-btn {
  --_h: 2.75rem;
  display: inline-flex; align-items: center; gap: .5rem;
  height: var(--_h); padding-inline: 1.25rem;
  font-family: var(--k-font-display);
  font-size: var(--k-text-sm); font-weight: var(--k-weight-medium);
  letter-spacing: .01em;
  border-radius: var(--k-radius-md);     /* 8px */
  border: 1px solid transparent;
  cursor: pointer; text-decoration: none; white-space: nowrap;
  transition: background-color var(--k-dur-base) var(--k-ease-out),
              border-color var(--k-dur-base) var(--k-ease-out),
              color var(--k-dur-base) var(--k-ease-out),
              transform var(--k-dur-fast) var(--k-ease-out);
}
.k-btn:active { transform: translateY(1px); }

.k-btn--primary {
  background: var(--k-primary); color: var(--k-primary-fg);
}
.k-btn--primary:hover {
  background: hsl(var(--k-purple-600));
  box-shadow: var(--k-glow-purple);       /* optional violet glow */
}

.k-btn--secondary {
  background: var(--k-surface);
  color: var(--k-fg);
  border-color: var(--k-border);
}
.k-btn--secondary:hover { border-color: var(--k-border-strong); background: var(--k-surface-raised); }

.k-btn--ghost {
  background: transparent; color: var(--k-fg-muted);
  border-color: transparent;
}
.k-btn--ghost:hover { color: var(--k-fg); background: hsl(var(--k-prey-700) / .5); }
```
**Tailwind primary:**
```html
<a class="inline-flex h-11 items-center gap-2 rounded-md bg-primary px-5
          font-display text-sm font-medium text-white transition-colors
          duration-200 ease-out hover:bg-purple-600 hover:shadow-glow
          active:translate-y-px">Get started</a>
```

---

## 2. Badges / pills

Used for "New", version tags, category labels. Often a subtle violet-tinted
fill with a hairline border. Frequently sits above the hero headline as an
"announcement" pill.

```html
<span class="k-badge">v1.0 — Now in public preview</span>
<span class="k-badge k-badge--solid">New</span>
```
```css
.k-badge {
  display: inline-flex; align-items: center; gap: .375rem;
  padding: .25rem .625rem;
  font-family: var(--k-font-code);
  font-size: var(--k-text-xs); font-weight: 500;
  color: var(--k-fg-muted);
  background: hsl(var(--k-purple-500) / .10);
  border: 1px solid hsl(var(--k-purple-500) / .30);
  border-radius: var(--k-radius-full);
}
.k-badge--solid { color: var(--k-primary-fg); background: var(--k-primary); border-color: transparent; }
```
**Tailwind:** `inline-flex items-center gap-1.5 rounded-full border
border-purple-500/30 bg-purple-500/10 px-2.5 py-1 font-mono text-xs
font-medium text-fg-muted`.

---

## 3. Navbar

Sticky, transparent-to-blurred on scroll, thin bottom hairline. Left wordmark
(text, not a logo), center/inline nav links, right CTAs. Compact height (~64px).

```html
<header class="k-nav">
  <div class="k-container k-nav__inner">
    <a class="k-nav__brand" href="/">yourbrand</a>
    <nav class="k-nav__links">
      <a href="#">Product</a><a href="#">Docs</a><a href="#">Pricing</a><a href="#">Blog</a>
    </nav>
    <div class="k-nav__actions">
      <a class="k-btn k-btn--ghost" href="#">Sign in</a>
      <a class="k-btn k-btn--primary" href="#">Download</a>
    </div>
  </div>
</header>
```
```css
.k-nav {
  position: sticky; top: 0; z-index: 50;
  background: hsl(var(--k-elevation-1) / .72);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--k-border);
}
.k-nav__inner { display: flex; align-items: center; justify-content: space-between; height: 64px; }
.k-nav__brand { font-family: var(--k-font-display); font-weight: 600; font-size: 1.125rem; color: var(--k-fg); letter-spacing: -0.01em; }
.k-nav__links { display: none; gap: 1.75rem; }
.k-nav__links a { color: var(--k-fg-muted); font-size: var(--k-text-sm); transition: color var(--k-dur-base) var(--k-ease-out); }
.k-nav__links a:hover { color: var(--k-fg); }
.k-nav__actions { display: flex; align-items: center; gap: .75rem; }
@media (min-width: 900px) { .k-nav__links { display: flex; } }
```
**Tailwind shell:** `sticky top-0 z-50 border-b border-border
bg-bg/70 backdrop-blur-md` → inner `flex h-16 items-center justify-between`.

---

## 4. Hero

The signature section: announcement pill → large **mono** headline (often with
one gradient/violet highlighted word) → muted sub-headline at prose width →
button pair → a **code block or terminal** as the visual. Centered layout is
typical.

```html
<section class="k-section k-hero">
  <div class="k-container k-hero__inner">
    <span class="k-badge">Public preview</span>
    <h1 class="k-hero__title">
      Build software with <span class="k-grad-text">agents</span>, not boilerplate
    </h1>
    <p class="k-hero__sub k-prose">
      Turn prompts into structured plans your team can review, run, and trust —
      from first draft to production.
    </p>
    <div class="k-hero__actions">
      <a class="k-btn k-btn--primary" href="#">Get started</a>
      <a class="k-btn k-btn--secondary" href="#">Read the docs</a>
    </div>
    <!-- visual: drop in the .k-code or .k-terminal component here -->
  </div>
</section>
```
```css
.k-hero__inner { display: flex; flex-direction: column; align-items: center; text-align: center; gap: var(--k-space-5); }
.k-hero__title {
  font-family: var(--k-font-display);
  font-size: var(--k-text-5xl);          /* clamp 54–68px */
  line-height: var(--k-leading-tight);
  letter-spacing: var(--k-tracking-tight);
  max-width: 16ch; margin: 0;
}
.k-hero__sub { color: var(--k-fg-muted); font-size: var(--k-text-lg); margin: 0; }
.k-hero__actions { display: flex; gap: .75rem; flex-wrap: wrap; justify-content: center; }

/* Gradient/violet highlight on one or two words */
.k-grad-text {
  background: var(--k-gradient-brand);   /* or var(--k-gradient-rainbow) for the playful variant */
  -webkit-background-clip: text; background-clip: text; color: transparent;
}
```
For the animated rainbow word, see the `.unicorn-text` recipe in
[`motion.md`](./motion.md).

---

## 5. Code block

Treat code as hero imagery. Dark slate background, rounded, optional window
"chrome" dots, mono font, and the Kiro syntax palette. Keep snippets short and
realistic.

```html
<figure class="k-code">
  <div class="k-code__bar">
    <span class="k-dot"></span><span class="k-dot"></span><span class="k-dot"></span>
    <span class="k-code__name">example.ts</span>
  </div>
  <pre class="k-code__body"><code><span class="tok-kw">export const</span> <span class="tok-fn">plan</span> = <span class="tok-kw">async</span> (<span class="tok-var">prompt</span>) =&gt; {
  <span class="tok-kw">const</span> <span class="tok-var">spec</span> = <span class="tok-kw">await</span> <span class="tok-fn">toRequirements</span>(<span class="tok-var">prompt</span>);
  <span class="tok-kw">return</span> <span class="tok-str">"ready to build"</span>; <span class="tok-cm">// validated</span>
}</code></pre>
</figure>
```
```css
.k-code {
  background: var(--k-code-bg);          /* #020617 */
  border: 1px solid var(--k-border);
  border-radius: var(--k-radius-lg);     /* 12px */
  overflow: hidden; box-shadow: var(--k-shadow-lg);
  font-family: var(--k-font-code);
  text-align: left;
}
.k-code__bar { display: flex; align-items: center; gap: .5rem; padding: .625rem .875rem; border-bottom: 1px solid hsl(0 0% 100% / .06); }
.k-dot { width: 11px; height: 11px; border-radius: 50%; background: hsl(0 0% 100% / .18); }
.k-code__name { margin-left: .5rem; font-size: var(--k-text-xs); color: var(--k-code-comment); }
.k-code__body { margin: 0; padding: 1.25rem 1.25rem; font-size: var(--k-text-sm); line-height: 1.7; color: var(--k-code-text); overflow-x: auto; }

/* Syntax tokens (map your highlighter's classes to these) */
.tok-kw  { color: var(--k-code-keyword); }   /* keywords  #c2a0fd */
.tok-fn  { color: var(--k-code-function); }  /* functions #8dc8fb */
.tok-str { color: var(--k-code-string); }    /* strings   #80ffb5 */
.tok-var { color: var(--k-code-variable); }  /* variables #80f4ff */
.tok-num { color: var(--k-code-number); }    /* numbers   #ffafd1 */
.tok-cm  { color: var(--k-code-comment); font-style: italic; } /* comments */
```
**Tailwind shell:** `overflow-hidden rounded-lg border border-border
bg-[#020617] font-mono shadow-lg` with a `border-b border-white/5` title bar.

---

## 6. Terminal window

A close cousin of the code block — emphasizes CLI usage (great for a CLI
product). Prompt sign, command, output lines, and a blinking cursor.

```html
<div class="k-terminal">
  <div class="k-code__bar">
    <span class="k-dot"></span><span class="k-dot"></span><span class="k-dot"></span>
    <span class="k-code__name">zsh — yourbrand</span>
  </div>
  <pre class="k-terminal__body"><span class="k-prompt">$</span> yourbrand init
<span class="tok-cm">✓ workspace ready</span>
<span class="k-prompt">$</span> yourbrand run "add auth"<span class="k-cursor"></span></pre>
</div>
```
```css
.k-terminal { background: var(--k-code-bg); border: 1px solid var(--k-border); border-radius: var(--k-radius-lg); overflow: hidden; box-shadow: var(--k-shadow-lg); }
.k-terminal__body { margin: 0; padding: 1.25rem; font-family: var(--k-font-code); font-size: var(--k-text-sm); line-height: 1.8; color: var(--k-code-text); white-space: pre-wrap; }
.k-prompt { color: hsl(var(--k-green-400)); margin-right: .5rem; }
.k-cursor { display: inline-block; width: .55em; height: 1.1em; margin-left: 2px; vertical-align: text-bottom; background: hsl(0 0% 100% / .6); animation: k-cursor-blink 1s step-end infinite; }
/* keyframes defined in motion.md */
```

---

## 7. Cards

Feature/benefit cards. Surface fill, 1px border, generous padding, mono
title, muted body, optional icon. Hover lifts the border (not a big shadow).

```html
<article class="k-card">
  <div class="k-card__icon" aria-hidden="true">◆</div>
  <h3 class="k-card__title">Structured planning</h3>
  <p class="k-card__body">Prompts become reviewable requirements and sequenced tasks.</p>
</article>
```
```css
.k-card {
  background: var(--k-surface);
  border: 1px solid var(--k-border);
  border-radius: var(--k-radius-xl);     /* 16px */
  padding: var(--k-space-6);             /* 32px */
  transition: border-color var(--k-dur-base) var(--k-ease-out),
              transform var(--k-dur-slow) var(--k-ease-out),
              background-color var(--k-dur-base) var(--k-ease-out);
}
.k-card:hover { border-color: hsl(var(--k-purple-500) / .5); transform: translateY(-2px); }
.k-card__icon { display: grid; place-items: center; width: 2.5rem; height: 2.5rem; border-radius: var(--k-radius-md); background: hsl(var(--k-purple-500) / .12); color: hsl(var(--k-purple-300)); margin-bottom: var(--k-space-4); }
.k-card__title { font-family: var(--k-font-display); font-size: var(--k-text-lg); font-weight: 500; margin: 0 0 .5rem; }
.k-card__body { color: var(--k-fg-muted); font-size: var(--k-text-base); margin: 0; }

/* Grid wrapper */
.k-card-grid { display: grid; gap: var(--k-space-5); grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); }
```
**Tailwind card:** `rounded-xl border border-border bg-surface p-8
transition-colors duration-200 hover:border-purple-500/50 hover:-translate-y-0.5`
· grid `grid gap-6 sm:grid-cols-2 lg:grid-cols-3`.

---

## 8. Section + eyebrow

Standard content section: small mono "eyebrow" label, title, lead, then content.

```html
<section class="k-section">
  <div class="k-container">
    <p class="k-eyebrow">Why teams choose it</p>
    <h2 class="k-section__title">Engineering, not just autocomplete</h2>
    <p class="k-section__lead k-prose">A short, muted supporting sentence.</p>
    <!-- content / card grid -->
  </div>
</section>
```
```css
.k-eyebrow { font-family: var(--k-font-code); font-size: var(--k-text-xs); text-transform: uppercase; letter-spacing: .14em; color: hsl(var(--k-purple-400)); margin: 0 0 .75rem; }
.k-section__title { font-family: var(--k-font-display); font-size: var(--k-text-3xl); font-weight: 500; line-height: var(--k-leading-snug); margin: 0 0 .75rem; }
.k-section__lead { color: var(--k-fg-muted); font-size: var(--k-text-lg); margin: 0 0 var(--k-space-7); }
```

---

## 9. Logo marquee

An infinitely scrolling strip of partner/tech logos (use generic placeholders).
Edges fade out via a mask. Pauses on hover. (Motion in [`motion.md`](./motion.md).)

```html
<div class="k-marquee">
  <div class="k-marquee__track">
    <!-- duplicate the set twice for a seamless -50% loop -->
    <span>alpha</span><span>beta</span><span>gamma</span><span>delta</span><span>epsilon</span>
    <span>alpha</span><span>beta</span><span>gamma</span><span>delta</span><span>epsilon</span>
  </div>
</div>
```
```css
.k-marquee {
  overflow: hidden;
  -webkit-mask-image: linear-gradient(90deg, transparent 0, #000 128px, #000 calc(100% - 128px), transparent);
          mask-image: linear-gradient(90deg, transparent 0, #000 128px, #000 calc(100% - 128px), transparent);
}
.k-marquee__track { display: flex; gap: 3rem; width: max-content; animation: k-scroll 45s linear infinite; }
.k-marquee__track span { font-family: var(--k-font-display); color: var(--k-fg-subtle); font-size: 1.25rem; }
.k-marquee:hover .k-marquee__track { animation-play-state: paused; }
/* @keyframes k-scroll in motion.md */
```

---

## 10. Footer

Dark, multi-column link footer with a top hairline; brand wordmark + muted
small print. Mono column headings.

```html
<footer class="k-footer">
  <div class="k-container k-footer__grid">
    <div><a class="k-nav__brand" href="/">yourbrand</a><p class="k-footer__tag">Agentic engineering.</p></div>
    <nav class="k-footer__col"><h4>Product</h4><a href="#">Features</a><a href="#">Pricing</a><a href="#">Changelog</a></nav>
    <nav class="k-footer__col"><h4>Resources</h4><a href="#">Docs</a><a href="#">Guides</a><a href="#">Blog</a></nav>
    <nav class="k-footer__col"><h4>Company</h4><a href="#">About</a><a href="#">Careers</a><a href="#">Contact</a></nav>
  </div>
</footer>
```
```css
.k-footer { border-top: 1px solid var(--k-border); padding-block: var(--k-space-8); background: var(--k-bg); }
.k-footer__grid { display: grid; gap: var(--k-space-6); grid-template-columns: 1.5fr repeat(3, 1fr); }
.k-footer__tag { color: var(--k-fg-subtle); font-size: var(--k-text-sm); margin-top: .5rem; }
.k-footer__col h4 { font-family: var(--k-font-code); font-size: var(--k-text-xs); text-transform: uppercase; letter-spacing: .12em; color: var(--k-fg-subtle); margin: 0 0 .75rem; }
.k-footer__col a { display: block; color: var(--k-fg-muted); font-size: var(--k-text-sm); padding-block: .25rem; transition: color var(--k-dur-base) var(--k-ease-out); }
.k-footer__col a:hover { color: var(--k-fg); }
@media (max-width: 720px) { .k-footer__grid { grid-template-columns: 1fr 1fr; } }
```

---

## Composition cheat-sheet

A typical Kiro-style landing page stacks these top to bottom:

1. **Navbar** (sticky, blurred)
2. **Hero** (pill → mono headline w/ one highlight → lead → button pair → code/terminal visual)
3. **Logo marquee** (social proof)
4. **Feature card grid** (eyebrow + title + 3–6 cards)
5. **Alternating feature rows** (text one side, code/terminal the other)
6. **CTA band** (centered, optional violet glow / gradient backdrop)
7. **Footer** (hairline top, mono column heads)

Keep the saturated violet rare, let the mono type and dark surfaces carry the
identity, and use exactly one motion flourish (gradient word or marquee).
