# CLAUDE.md — VeLuny Shopify Theme

This file is read automatically at the start of every Claude Code session in this repo. It's the fast-reference version of `VeLuny-Brand-Guide.md` and `design-system.md` (also in this repo — read those in full for anything not covered here, especially before building a new section type for the first time).

## Project

Shopify store for VeLuny, a baby sleep & soothing brand (flagship: portable clip-on white noise machine), built to expand into other parenting categories later. Theme base: **Shopify Dawn 15.2.0**, customized — not rebuilt from scratch. Sales channel: Shopify DTC only.

Priority order: **Homepage first**, then PDP, then cart/checkout polish.

## Non-negotiable brand rules

- Wordmark is `VeLuny.` — the period is permanent, never drop it, never recreate the logo in text/font
- Voice: warm, plain-spoken, reassuring, lightly playful. Never clinical/medical-device, never twee/baby-talk, never fear-based marketing
- No unsubstantiated product claims — trust-icon row content must come from real specs, not placeholders, before it ships
- Never combine two accent colors in the same section/viewport

## Design tokens (source of truth: `design-system.md` §1)

Do not hardcode hex values or px sizes in new CSS — always use the tokens below. If a needed token doesn't exist yet, add it to `veluny-tokens.css` first, don't invent an inline value.

```css
--color-ink: #222222        --color-dusty-sky: #A9C2CE
--color-cream: #F5F4F0      --color-sage: #B7C4A8
--color-white: #FFFFFF      --color-blush-nap: #E8C9C3
--color-border: rgba(34,34,34,0.12)  --color-warm-sand: #D8C9B0

--font-display: "Fraunces"      (headlines)
--font-heading: "Quicksand"     (short labels, nav, UI)
--font-body: "Nunito"           (paragraph copy)

--radius-sm: 8px   --radius-md: 16px   --radius-lg: 28px   --radius-pill: 999px
--shadow-soft / --shadow-lift
--space-1 (8px) through --space-8 (128px), 8px base unit
```

Full token file, type scale, breakpoints, and motion values: `design-system.md` §1.

## File conventions

- New/custom sections go in `sections/`, prefixed `veluny-` (e.g. `veluny-hero-video.liquid`) so Dawn base-theme updates never silently overwrite them
- Tokens live in one file, `assets/veluny-tokens.css`, loaded before `base.css` in `theme.liquid` — never scatter hex values into individual section CSS
- Mirror color tokens into `config/settings_schema.json` presets so the theme editor's color pickers stay locked to the approved palette
- Fonts are self-hosted via Shopify's asset CDN, not a Google Fonts `<link>`

## Components (spec: `design-system.md` §3)

Reuse these, don't re-derive styling per section:
- **Buttons**: Primary = Ink fill/Cream text/pill/48px min height. Secondary = Ink outline. Accent = Dusty Sky fill, used sparingly for sub-line CTAs
- **Cards**: `--radius-md`, White or Cream background (never both in one section), hover = `--shadow-soft` + translateY(-4px)
- **Badges**: pill-shaped, max one per product card

## Homepage build order

`Hero video → Product/Collection grid → Story scroll (alternating) → Trust/press strip (or trust-icon grid) → Talk to Us → Footer`

- `hero-video.liquid` and `story-scroll.liquid` are custom builds (Dawn has no equivalent) — see `design-system.md` §4.1 and §4.3 for exact schema/behavior specs before writing these
- `featured-collection.liquid` (product grid) is a Dawn section restyled via tokens, not rebuilt
- Scroll-reveal on the story section must respect `prefers-reduced-motion` (fallback: plain opacity fade, no translate)

## Workflow expectations for Claude Code in this repo

- Work in small, single-purpose commits (one token file, one component, one section at a time) — not one giant "build homepage" commit
- After each change, this should be verified in `shopify theme dev` live preview before moving to the next piece
- Don't introduce new dependencies, JS frameworks, or restructure Dawn's base file layout without flagging it first — this is a Liquid/CSS theme, not a rebuild
- If a request conflicts with a brand rule above (e.g., a claim with no source, a font outside the approved pairing, stacking two accents), flag the conflict instead of silently proceeding
