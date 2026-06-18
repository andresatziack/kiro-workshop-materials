---
name: kiro-visual-language
description: >-
  Recreate the "Kiro" visual design language (kiro.dev) in any front-end
  project. Use this skill when a user asks to build a landing page, marketing
  site, docs site, app shell, or UI components that should look like Kiro / Kiro
  CLI — a dark-first, purple-accented, mono-display, developer-tool aesthetic
  with code blocks, terminal motifs, fluid type, and subtle fade/gradient
  motion. Provides framework-agnostic design tokens (CSS variables), component
  recipes (plain CSS + Tailwind), and motion specs. Trigger on phrases like
  "Kiro style", "estilo Kiro", "looks like kiro.dev", "developer tool landing
  page", or when the user references this skill directly.
---

# Kiro Visual Language

A reusable design system that lets any agent reproduce the *look and feel* of
Kiro (kiro.dev / kiro.dev/cli) in a brand-new front-end project. It captures the
tokens, components, and motion patterns — **not** proprietary copy, logos, or
font binaries. Everything here is recreated from observation so output is
"recognizably Kiro" without copying assets.

> **Important — do not copy proprietary assets.** Never reproduce Kiro/AWS
> logos, marketing copy, screenshots, or the proprietary fonts
> (`AWS Diatype`, `AWS Diatype Rounded Semi Mono`). Recreate the *system* using
> the open-font substitutes and tokens documented here.

---

## The Kiro look in one paragraph

Dark-first, near-black backgrounds with a faint purple tint; a single vivid
**violet** as the only saturated brand color; **monospaced display
typography** that gives a "terminal / developer tool" personality; generous
whitespace and a centered, narrow content column; rounded-but-restrained
corners; very subtle elevation; **code blocks and terminal windows treated as
hero imagery**; and quiet, fast motion — short opacity fades, a slow animated
rainbow gradient on a few accent words, and a marquee logo strip. The mood is
*precise, technical, calm, premium*.

## Core design principles

1. **Dark-first, purple-accented.** Design the dark theme first; light is a
   secondary mapping. Purple (`#9046FF`) is used sparingly — for primary
   actions, links, focus rings, and a few highlighted words. Everything else is
   a purple-tinted neutral.
2. **Mono as personality.** The display/headings use a rounded *monospaced*
   typeface. This is the single biggest signal of the Kiro look. Body text may
   be a clean sans, but headings and UI labels lean mono.
3. **Code is the hero.** Syntax-highlighted code blocks and terminal windows
   are first-class visuals, not afterthoughts. Use them where other sites use
   stock photos.
4. **Quiet, fast motion.** Micro-interactions are short (100–350ms) and mostly
   opacity/transform fades. One or two "delightful" flourishes (animated
   gradient text, marquee) carry the energy. Respect `prefers-reduced-motion`.
5. **Restraint over decoration.** Subtle borders (1px, low-contrast), soft
   shadows, low color count. Let type and spacing do the work.

---

## How to use this skill (progressive disclosure)

Read this overview first. Then load the specific resource file only for the
sub-task you are doing — do not pull all files up front.

| If the task involves...                              | Read this resource |
|------------------------------------------------------|--------------------|
| Colors, theming, type scale, spacing, radii, shadows | [`tokens.md`](./tokens.md) (human reference) + [`tokens.css`](./tokens.css) (drop-in variables) |
| Building hero, buttons, cards, navbar, code block, badges, sections | [`components.md`](./components.md) |
| Transitions, durations, easing, keyframes, micro-interactions, reduced-motion | [`motion.md`](./motion.md) |

### Recommended workflow for "build a Kiro-style page"

1. **Scaffold tokens.** Copy [`tokens.css`](./tokens.css) into the project
   (e.g. `styles/kiro-tokens.css`) and import it once globally. If the project
   uses Tailwind, mirror the tokens into `tailwind.config` (mapping shown in
   `tokens.md`).
2. **Set the foundation.** Apply the dark theme by default, wire up the three
   font families (using the open substitutes), set base `background`/`color`,
   and establish the centered container + vertical rhythm.
3. **Compose sections** using recipes from [`components.md`](./components.md):
   navbar → hero (with a code/terminal visual) → feature cards → logo marquee →
   CTA → footer.
4. **Add motion** from [`motion.md`](./motion.md): entrance fades on sections,
   hover transitions on interactive elements, and at most one signature
   flourish (gradient text or marquee).
5. **Verify** against the checklist at the bottom of this file.

---

## Fonts (substitution policy)

Kiro uses proprietary AWS fonts. **Never ship those.** Use these open
substitutes that preserve the personality:

| Role     | Kiro (do NOT use)                  | Open substitute (use these)                            |
|----------|------------------------------------|--------------------------------------------------------|
| Display / headings | `AWS Diatype Rounded Semi Mono` | **`Space Grotesk`** (geometric, semi-mono feel) or **`Geist Mono`** / **`JetBrains Mono`** |
| Body     | `AWS Diatype`                      | **`Inter`**, **`Geist`**, or system UI sans            |
| Code     | `Fragment Mono`                    | **`JetBrains Mono`**, **`IBM Plex Mono`**, or `Fragment Mono` (open, on Google Fonts) |

The defining trait is **mono or semi-mono headings**. If you only change one
thing to evoke Kiro, make the headings monospaced.

---

## Success checklist

A page built from this skill should pass all of these:

- [ ] Dark background is near-black with a subtle purple/cool tint (not pure
      `#000`, not neutral gray) — e.g. `#19161D`.
- [ ] Exactly one saturated accent (violet `#9046FF`) used for primary CTA,
      links, and focus rings; everything else neutral.
- [ ] Headings use a **monospaced or semi-mono** typeface.
- [ ] Hero contains a code block or terminal window as a primary visual.
- [ ] Corners are rounded but modest (8–16px on cards, ~4–6px on small
      controls).
- [ ] Borders are 1px and low-contrast; shadows are soft and subtle.
- [ ] Content sits in a centered container (~1280px max) with a narrow text
      measure for prose (~720–768px).
- [ ] Section entrances use short opacity fades; interactive elements have
      150–350ms transitions; `prefers-reduced-motion` is honored.
- [ ] At most one "delight" flourish (animated gradient text or a logo
      marquee).
- [ ] No Kiro/AWS logos, screenshots, marketing copy, or proprietary fonts.

---

## Platform installation (Agent Skill format)

This skill follows the generic **Agent Skill** convention: a `SKILL.md` with
`name` + `description` frontmatter, plus referenced resource files in the same
folder. Installation differs slightly per platform — notes below.

### Kiro
Place the folder under one of:
- `~/.kiro/skills/kiro-visual-language/` (user-level, all workspaces), or
- `<workspace>/.kiro/skills/kiro-visual-language/` (workspace-level).
Kiro auto-discovers `SKILL.md`; activate it by name when the task matches the
`description`.

### Claude / Claude Code (Anthropic Agent Skills)
Place under `~/.claude/skills/kiro-visual-language/` (personal) or
`<project>/.claude/skills/kiro-visual-language/` (project), or zip and upload as
a Skill where supported. Same `SKILL.md` + resources layout; the model loads
the body on demand via progressive disclosure.

### Cursor / Windsurf / generic IDE agents
These do not (yet) have a native "skill" loader. Install by adding the folder to
the repo (e.g. `docs/skills/kiro-visual-language/`) and referencing it from your
rules file (`.cursor/rules`, `.windsurfrules`, or `AGENTS.md`) with a line such
as: "For Kiro-style UI, follow `docs/skills/kiro-visual-language/SKILL.md` and
its resource files." The content is plain Markdown/CSS, so it works as context
for any agent.

### Differences summary
- **Auto-discovery:** Kiro and Claude load `SKILL.md` automatically from their
  skills directories; IDE agents need a manual rules reference.
- **Activation:** Kiro/Claude use the `description` for matching; IDE agents
  rely on you pointing at the file.
- **Packaging:** Claude supports zipped/uploaded skills; Kiro and IDE agents use
  on-disk folders. The folder contents are identical in all cases.
