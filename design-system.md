# VeLuny — Design System
*Version 0.1 · July 2026 · Draft for review*

Built on **Shopify Dawn 15.2.0**, extended into a distinct, brand-owned system. Everything here derives from `VeLuny-Brand-Guide.md` (Sections 5–10) plus the direction from Coterie, Muum, Plumeti, and Apple AirPods Max references. Treat this as the single source of truth for tokens, components, and homepage structure — Dawn's defaults should be overridden, not designed around.

**Inspiration synthesis (what we're taking from each):**
| Reference | What we're borrowing |
|---|---|
| **Coterie** | Overall homepage skeleton — video hero with direct shop CTA, "product line" cards, alternating scroll-reveal story section, trust/press strip, email capture |
| **Muum** | Soft rounded shapes, pill buttons, warm color-blocked sections, icon+label trust grid, oversized friendly headline type |
| **Plumeti** | Calm PDP layout — thumbnail rail, soft neutral (not stark white) backgrounds, gentle "you may also like" grid |
| **Apple (AirPods Max)** | Confidence in negative space, large display type overlapping imagery, restrained alternating full-bleed sections |

Velora (the fashion PDP) is noted but **not** a strong reference — its dark, high-contrast, sharp-edged aesthetic runs against VeLuny's calm/rounded/warm brand personality. Flagging so we consciously exclude it rather than accidentally drifting toward it.

---

## 1. Design Tokens

All tokens below should live as CSS custom properties in Dawn's `base.css` / `theme.css` (or a new `veluny-tokens.css` loaded first), and mirrored into `config/settings_schema.json` where Dawn expects theme-editor-controlled values (colors, type scale).

### 1.1 Color

```css
:root {
  /* Core */
  --color-ink:        #222222; /* text, primary buttons, headers on cream */
  --color-cream:       #F5F4F0; /* primary site background */
  --color-white:       #FFFFFF; /* secondary background, cards on cream */

  /* Supporting accents — small doses only, never combined in one section */
  --color-dusty-sky:   #A9C2CE; /* Sleep sub-line, calm CTAs, icons */
  --color-sage:        #B7C4A8; /* organic/nature accents */
  --color-blush-nap:   #E8C9C3; /* gifting/registry, warm highlights */
  --color-warm-sand:   #D8C9B0; /* card/section fill between Ink and Cream */

  /* Functional (derived, not in brand guide — proposed) */
  --color-ink-70:       rgba(34,34,34,0.70);  /* secondary text on cream */
  --color-ink-40:       rgba(34,34,34,0.40);  /* disabled / placeholder */
  --color-border:       rgba(34,34,34,0.12);  /* hairline borders, card edges */
  --color-success:      #7C9A73; /* order confirmation, in-stock */
  --color-error:        #C67B6B; /* form errors — warm, not clinical red */
}
```

**Usage rule (from brand guide, carried forward):** Ink + Cream carry the site. One accent per section, max. Never stack Dusty Sky and Blush Nap in the same viewport.

### 1.2 Typography

| Role | Typeface | Fallback stack | Notes |
|---|---|---|---|
| Display / H1–H2 | **Fraunces** (variable, opsz axis) | `Georgia, "Times New Roman", serif` | Use `font-optical-sizing: auto` — larger opsz for hero headlines reads warmer/rounder |
| H3–H5, UI, body | **Quicksand** (headings) / **Nunito** (body copy) | `-apple-system, "Segoe UI", sans-serif` | Quicksand for short punchy labels/nav, Nunito for paragraph-length copy — Quicksand gets visually thin at body-copy length |
| Numerals / prices | Nunito, tabular-nums | — | Keeps price columns aligned in cart/PDP |

**Type scale (fluid, `clamp()`-based so it works across Dawn's breakpoints without separate mobile overrides):**

```css
:root {
  --font-display: "Fraunces", Georgia, serif;
  --font-heading: "Quicksand", -apple-system, sans-serif;
  --font-body:    "Nunito", -apple-system, sans-serif;

  --text-h1: clamp(2.25rem, 4vw + 1rem, 3.5rem);   /* 36–56px */
  --text-h2: clamp(1.75rem, 2.5vw + 1rem, 2.5rem);  /* 28–40px */
  --text-h3: clamp(1.375rem, 1.5vw + 1rem, 1.75rem);/* 22–28px */
  --text-h4: 1.25rem;   /* 20px */
  --text-body-lg: 1.125rem; /* 18px */
  --text-body: 1rem;        /* 16px */
  --text-caption: 0.875rem; /* 14px */

  --leading-tight: 1.15;   /* display headlines */
  --leading-body: 1.6;     /* paragraph copy — "calm, unhurried" per brand guide */
}
```

**Licensing note:** Fraunces and Quicksand are both open-source (Google Fonts / OFL), Nunito likewise — no licensing blocker, self-host via Shopify's asset CDN for performance rather than a Google Fonts `<link>` (avoids the render-blocking request and matches Dawn's existing font-loading pattern in `theme.liquid`).

### 1.3 Spacing

8px base unit, matching Dawn's existing rhythm so section padding overrides don't fight the grid:

```css
:root {
  --space-1: 0.5rem;  /* 8px */
  --space-2: 1rem;    /* 16px */
  --space-3: 1.5rem;  /* 24px */
  --space-4: 2rem;    /* 32px */
  --space-5: 3rem;    /* 48px */
  --space-6: 4.5rem;  /* 72px */
  --space-7: 6rem;    /* 96px  — desktop section vertical padding */
  --space-8: 8rem;    /* 128px — large hero/story section breathing room */
}
```

### 1.4 Radius, Shadow, Border

VeLuny is soft and rounded, not sharp — this is the single biggest visual departure from Dawn's default (which ships fairly square).

```css
:root {
  --radius-sm: 8px;   /* inputs, badges, small buttons */
  --radius-md: 16px;  /* cards, product images */
  --radius-lg: 28px;  /* hero panels, large image blocks */
  --radius-pill: 999px; /* primary CTAs — per Muum reference */

  --shadow-soft: 0 8px 24px rgba(34,34,34,0.06);   /* card hover, dropdown */
  --shadow-lift: 0 16px 40px rgba(34,34,34,0.10);  /* modal, cart drawer */

  --border-hairline: 1px solid var(--color-border);
}
```

### 1.5 Grid & Breakpoints

Keep Dawn's existing breakpoint values (don't fight the theme's responsive JS/CSS that ships with it) — just restyle within them:

| Breakpoint | Width | Container max-width | Columns |
|---|---|---|---|
| Mobile | 0–749px | 100% (16px gutter) | 4 |
| Tablet | 750–989px | 100% (32px gutter) | 8 |
| Desktop | 990–1399px | 1200px | 12 |
| Wide | 1400px+ | 1400px | 12 |

### 1.6 Motion

Restrained, matching brand tone (calm, not flashy) — used mainly for the scroll-reveal story section and hover states, not everywhere:

```css
:root {
  --ease-soft: cubic-bezier(0.22, 1, 0.36, 1);
  --duration-fast: 180ms;
  --duration-base: 320ms;
  --duration-slow: 600ms; /* scroll-reveal fades */
}
```
Respect `prefers-reduced-motion`: scroll-reveal sections should fall back to a simple opacity fade-in (no translate/parallax) when set.

---

## 2. Foundations Recap

- **Logo:** Black wordmark on Cream/White sections, Cream (reversed) wordmark on Ink or photography-dark sections. Icon `v.` mark used for favicon, mobile menu trigger corner mark, and as a low-opacity repeating watermark on the story section (per brand guide Section 9).
- **Photography:** Warm, natural light, product-in-motion (clipped to stroller, in diaper bag). No stark white studio backgrounds — product images should sit on Cream or Warm Sand, even in PDP zoom/gallery. Since real photography isn't shot yet, all placeholder imagery in this build should use warm-toned lifestyle stock (avoid cool-white/clinical stock) so layout decisions aren't invalidated once real photos arrive.
- **Iconography:** Simple rounded-line icons (sound wave, clock/timer, battery, stroller clip) — 1.5–2px stroke, rounded caps, matching the logo's soft terminals. Use for PDP feature call-outs and the homepage trust-icon row.

---

## 3. Components

### 3.1 Buttons

| Variant | Style | Use |
|---|---|---|
| Primary | Ink fill, Cream text, `--radius-pill`, `--shadow-soft` on hover (slight lift, no color change) | "Shop The [Product]", Add to Cart |
| Secondary | 1.5px Ink border, transparent fill, Ink text, `--radius-pill` | "Learn More", secondary PDP actions |
| Accent (sparingly) | Dusty Sky fill, Ink text, `--radius-pill` | Sub-line specific CTA (e.g. future "Shop Sleep") |
| Text link | Ink, underline on hover only, Quicksand medium | Nav, footer, inline links |

All buttons: `--font-heading` (Quicksand, medium/semibold), letter-spacing +0.01em, min height 48px for tap targets.

### 3.2 Cards (Product / Story)

- `--radius-md`, background White or Cream (never a hard white against a cream page — pick one per section and hold it)
- Image fills top of card at `--radius-md` on top corners only, product image itself sits on Cream/Warm Sand backdrop (per Section 8), not transparent/white-cutout
- On hover (desktop): `--shadow-soft`, translateY(-4px), `--duration-base` `--ease-soft`
- Price in Nunito tabular-nums, sale price uses `--color-blush-nap` strike-through accent rather than red

### 3.3 Navigation (Header)

- Cream background, sticky, hairline border-bottom only on scroll (not always-on, keeps hero clean per Coterie reference)
- Logo **centered** — reinforces the wordmark as hero, matches the calm/considered feel over a left-aligned utilitarian layout (confirmed)
- Cart icon shows count in a small Ink pill badge, not a raw number
- Mobile menu: full-screen drawer, Cream background, Fraunces for top-level category labels (gives even the nav a touch of warmth instead of a generic sans list)

### 3.4 Trust Icon Row

Directly inspired by Muum's 4-icon benefit grid — adapt to VeLuny's actual claims (pull real ones before build: e.g. "Dermatologist-safe materials," "20+ hr battery," "Clips anywhere," "30-night trial"):

- 4-across desktop / 2x2 mobile, each: rounded-square icon tile (Warm Sand fill, `--radius-md`), Quicksand bold label, Nunito caption below
- No claim should be added here that isn't substantiated — matches brand guide's "don't over-promise" voice rule

### 3.5 Forms & Inputs

- `--radius-sm`, 1.5px border `--color-border`, Ink focus ring (2px, offset) — never a harsh blue browser-default focus
- Quantity selector: pill-shaped stepper, matches button radius language
- Newsletter/email capture input pairs with a pill submit button, sits on a Warm Sand or Dusty Sky section background (not stacked directly on Cream, needs the contrast per Coterie's approach)

### 3.6 Badges

- Pill-shaped, `--radius-pill`, small caps or Quicksand medium, 12–13px
- "New" → Ink fill / Cream text; "Bestseller" → Dusty Sky fill / Ink text; Sale → Blush Nap fill / Ink text
- Never more than one badge per product card

### 3.7 Cart Drawer / Modal

- Slides from right, Cream background, `--shadow-lift`, `--radius-lg` on the left edge only
- Line items use the card pattern (3.2) at a smaller scale
- Sticky footer within the drawer for subtotal + checkout button (Primary button style, full width)

### 3.8 Footer

- Ink background, Cream text/wordmark (reversed logo) — this is the one place a full-bleed Ink block is expected/on-brand, echoing packaging direction in Section 10 of the brand guide
- Four columns desktop (Shop, About, Help, Newsletter) collapsing to accordion on mobile
- "Talk to us" contact block sits directly above the footer, not inside it — treated as its own warm (Warm Sand or Blush Nap) section per your homepage brief, so it doesn't get visually buried in the dark footer

---

## 4. Homepage — Section-by-Section (Coterie-Structured, VeLuny-Styled)

Mapped to what you described, with a recommended Dawn section/block build approach for each.

### 4.1 Hero — Video/GIF + Direct Shop CTA
**Content:** Looping silent product video or GIF (product in motion — clipped to stroller, per Section 8), large Fraunces headline overlapping or beside it (Apple reference — confident negative space, oversized display type), one Primary pill button ("Shop [Product Name]") linking straight to the PDP.
**Build:** Not a native Dawn section — Dawn's `image-banner.liquid` doesn't support autoplay video well. Build a custom `hero-video.liquid` section (video/gif upload setting, fallback poster image, heading/subheading richtext, single CTA link) modeled on Dawn's `video.liquid` schema but stripped down to hero use.
**Placeholder asset note:** use a generic soft-motion lifestyle clip or a slow product-rotation placeholder until real footage exists — reserve space at the hero's actual aspect ratio now so the layout doesn't reflow when real video drops in.

### 4.2 Product / Collection Section
**Content:** Clean grid of other products (or the flagship's variants, if only one SKU today) — 3–4 cards using the Card component (3.2), Cream section background.
**Build:** Dawn's `featured-collection.liquid` works well here largely as-is — restyle via tokens rather than rebuilding; add the rounded card treatment and Warm Sand product backdrop via CSS override.

### 4.3 Scroll-Reveal Brand Story Section
**Content:** Alternating full-bleed image / text blocks that fade or slide in as the user scrolls — lifestyle imagery + short copy on the brand's mission (Section 1) and the "solve one real problem" positioning (Section 2), written in the warm/plain-spoken voice (Section 4).
**Build:** Custom `story-scroll.liquid` section with repeatable blocks (image + eyebrow + heading + short paragraph), each block alternating image-left/image-right at desktop, stacking at mobile. Use `IntersectionObserver` for the reveal (opacity + 24px translateY, `--duration-slow`, `--ease-soft`), respecting `prefers-reduced-motion`. This is the one section worth the extra JS — matches the "spend your boldness in one place" principle; keep every other section calmer by comparison.

### 4.4 Trust / Press Strip (recommended addition)
Coterie leans on this hard (award logos, "as seen in") and it's cheap trust-signal real estate — flag as optional if you don't have press/awards yet; can be swapped for the trust-icon grid (3.4) instead until you do.

### 4.5 Talk to Us
**Content:** Short, warm invitation to reach out (matches "Customer support: patient, human, zero jargon" from Section 4) + contact CTA (email, or a simple contact form link). Sits on Warm Sand or Blush Nap, directly above the footer, full-width, generous padding (`--space-7`/`--space-8`).
**Build:** Simple custom `contact-cta.liquid` section — heading, short richtext, one button. Don't overbuild this one; it's a breath before the footer, not a feature section.

### 4.6 Footer
Per Component 3.8 — links, newsletter signup, social, VeLuny wordmark reversed.

**Full homepage order:**
`Hero video → Product/Collection grid → Story scroll (alternating) → Trust/press (or trust-icon grid) → Talk to Us → Footer`

---

## 5. Dawn Implementation Notes

- Keep all new custom sections in `sections/`, prefixed `veluny-` where they diverge meaningfully from Dawn's originals, so future theme updates from Shopify/Dawn's base don't silently overwrite them
- Centralize tokens in one CSS file loaded before `base.css` in `theme.liquid`, rather than scattering hex values through section CSS — makes future sub-line accent-color swaps (Section 11 of brand guide) a one-file change
- Mirror the color tokens into `config/settings_schema.json` presets so merchant-side theme editor color pickers stay locked to the approved palette instead of open color pickers inviting off-brand choices later
- Audit Dawn's default button/card border-radius overrides in `component-card.css`, `component-button.css` — these will need near-full overrides given how far the pill/rounded direction is from Dawn's default squared-off look

---

## 6. Open Items / Next Steps

1. Pull 3–4 real, substantiated product claims for the trust-icon row (3.4) — placeholders shouldn't ship
2. Source or shoot placeholder-tier lifestyle imagery matching Section 8 mood for hero video, story section, and product backdrops
3. Decide whether the Trust/Press strip (4.4) is included at launch or deferred until real press mentions exist
4. Once this doc is approved, next deliverable is a PDP-specific pass (Plumeti-style layout: thumbnail rail, feature icons, "you may also like") — flagged as the logical follow-up given homepage is priority #1
