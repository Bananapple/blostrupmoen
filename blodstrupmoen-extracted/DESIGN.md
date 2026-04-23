# Blostrupmoen — Design Source of Truth

> This document is the canonical reference for the visual system of the Blostrupmoen static prototype.
> All values here are live in `index.html` `:root`. Any AI agent or developer editing this site
> must read this file first and stay within the rules below. Do not introduce new font sizes,
> colors, spacing values, or border radii without updating this document.

---

## 0. Project Context

- **File:** `index.html` (single-file static prototype — no build step, no framework)
- **Preview:** `python3 -m http.server` at `http://localhost:8001`
- **Fonts loaded:** Manrope (400, 500, 600, 700, 800) + Source Serif 4 (400, 500, 600 — regular and italic)
- **Purpose:** Visual redesign prototype for Blostrupmoen Norge AS, a Norwegian B2B safety company. All CTAs lead to `#kontakt`. No cart, no checkout. Design decisions favor trust-building for enterprise buyers.

---

## 1. Spacing Logic — The P = 2F Rule

All component padding is derived from the font size of the text it contains.

**Rule:** `P ≥ 2 × F`
where `P` = component padding and `F` = the font size of the primary text inside.

| Text inside component | Font size (F) | Minimum padding (2F) | Nearest token |
|-----------------------|---------------|----------------------|---------------|
| Body text | 17px | 34px | 32px |
| Label / UI text | 14px | 28px | 24–28px |
| Small / eyebrow text | 12px | 24px | 24px |

**Why this exists:** Cramped cards feel cheap. The P = 2F rule ensures every content container breathes proportionally. When in doubt, round up to the next step on the 4px scale, never down.

### The 4px Base Scale

All spacing values must be multiples of 4px. The allowed steps are:

```
4  8  12  16  24  32  48  64  96  112
```

Use these and nothing else. Values like 7px, 11px, 14px, 18px, 22px, 26px, 28px, 46px are legacy and have been removed. If a new component needs spacing, pick the closest step above.

**Section-level spacing is tokenised:**
- `--space-section: 112px` — vertical padding for all `<section>` elements
- `--space-section-sm: 64px` — section-head `margin-bottom`, compressed contexts

---

## 2. Color Tokens

All tokens live in `:root` in `index.html`. Never use raw hex or rgba values in rules — always use the variable.

### Brand

| Token | Hex | Semantic name | Use |
|-------|-----|---------------|-----|
| `--orange` | `#E97410` | brand-primary | CTAs, highlights, eyebrow decorators, hover accents |
| `--orange-deep` | `#C85F08` | brand-primary-dark | Hover states on orange elements, eyebrow text colour |
| `--orange-soft` | `#FBE7D2` | brand-primary-tint | Active nav bg, filter pill active bg, soft fills |

### Foreground

| Token | Hex | Semantic name | Use |
|-------|-----|---------------|-----|
| `--ink` | `#141311` | text-primary | Headings, primary body, dark CTAs |
| `--ink-2` | `#3A3733` | text-secondary | Body copy, descriptions, secondary labels |
| `--ink-3` | `#6B6661` | text-muted | Meta labels, captions, timestamps, role titles |

### Surface

| Token | Hex | Semantic name | Use |
|-------|-----|---------------|-----|
| `--bg` | `#FDFBF6` | surface-page | Page background (warm off-white) |
| `--white` | `#ffffff` | surface-card | Card backgrounds, explicit white fills |
| `--cream` | `#FAF1E6` | surface-muted | Section backgrounds (story, partners, quotes) |
| `--cream-deep` | `#F4E4CE` | surface-muted-deep | Float frame bg, pressed/active fills |
| `--rule` | `#E8E1D5` | border | All dividers, card borders, dashed rules |

### Utility

| Token | Hex | Semantic name | Use |
|-------|-----|---------------|-----|
| `--red` | `#C33027` | danger | Error states, alert indicators (currently unused) |

### Elevation

| Token | Value | Use |
|-------|-------|-----|
| `--shadow` | 2-layer drop, 12px spread | Card resting state, badge lift |
| `--shadow-lg` | 2-layer drop, 60px spread | Hero visual, large card hover, major elevation |

### Glass Surfaces

These tokens represent frosted-glass overlays used on the nav pill, badges, and labels. Always pair with `backdrop-filter: blur()`.

| Token | Value | Use |
|-------|-------|-----|
| `--glass-white` | `rgba(255,255,255,0.72)` | Nav pill, floating logo |
| `--glass-white-heavy` | `rgba(255,255,255,0.92)` | Hero badge, stock badge, team card hover button |
| `--glass-bg` | `rgba(253,251,246,0.92)` | Equipment label, team card label (warm glass over cream) |
| `--overlay-dark` | `rgba(20,19,17,0.45)` | Team card hover scrim |
| `--overlay-dark-light` | `rgba(20,19,17,0.08)` | Nav pill box-shadow |

### Dark Section Palette (AED + dark infocard)

These tokens are only used inside the dark-background sections (`.aed`, `.infocard.dark`). Do not use them on light surfaces.

| Token | Value | Use |
|-------|-------|-----|
| `--aed-text` | `#FBF5EA` | Primary text on dark surface |
| `--aed-accent` | `#F9A76B` | Warm orange accent on dark (eyebrows, em, bullet icons) |
| `--aed-surface` | `#1f1d1a` | Dark visual card background |

---

## 3. Typography Tiers

Two typefaces only:
- **Manrope** (sans-serif) — UI, body, labels, navigation
- **Source Serif 4** (serif) — headlines, display numbers, pull quotes, card headings

There are exactly **4 allowed tiers**. Do not introduce a fifth.

### Tier 1 — Display

Used for: H1, section H2, AED H2, stats numbers, pull quotes.

```css
font-family: 'Source Serif 4', Georgia, serif;
font-weight: 500;
font-size: clamp(min, vw, max);  /* See per-section values below */
line-height: 1.02–1.05;
letter-spacing: -0.02em to -0.025em;
```

Specific clamp values in use:
- Hero H1: `clamp(44px, 5.6vw, 78px)`
- Section H2: `clamp(32px, 3.6vw, 52px)`
- AED H2: `clamp(36px, 4vw, 56px)`
- Story pull: `clamp(28px, 2.6vw, 36px)`
- CTA H2: `clamp(42px, 4.5vw, 64px)`
- Stats numbers: `clamp(46px, 5vw, 72px)`

Do not replace clamp with fixed values. They are intentional fluid type.

**Fixed display sizes** (non-clamp, kept as exceptions):
- `34px` — hero-meta numbers, infocard h3
- `30px` — AED stat numbers
- `84px` — decorative quote mark (ornamental, not prose)

### Tier 2 — Title

Used for: Card headings (`.svc h3`, `.course-grid h4`), hero badge copy, infocard h3, partner logo names.

```css
font-family: 'Source Serif 4', Georgia, serif;
font-weight: 500;
font-size: 22px–26px;
line-height: 1.15;
letter-spacing: -0.015em;
```

- `26px` — service card headings (`.svc h3`)
- `22px` — course card headings (`.course-grid h4`)

### Tier 3 — Body

Used for: All paragraph text, section lede, list items, form labels, footer copy.

```css
font-family: 'Manrope', system-ui, sans-serif;
font-weight: 400;
font-size: 17px;  /* set on body; do NOT override with font-size: 17px on children */
line-height: 1.55;
```

**Rule:** Do not write `font-size: 17px` on any element inside `<body>`. The cascade handles it. Only write a font-size override when you need to deviate from 17px intentionally (Title, Label, or Display tiers).

Exception: `font-size: 15px` is allowed on `<button>`, `<input>`, `<select>`, `<textarea>` — these are functional form elements, not prose.

### Tier 4 — Label

Used for: Navigation links, eyebrows, tags, card meta, filter pills, footer labels, captions.

Two sub-sizes, both Manrope:

```css
/* Label — UI / nav / card meta */
font-family: 'Manrope', system-ui, sans-serif;
font-weight: 500–700;
font-size: 14px;
line-height: 1.3;

/* Label — Eyebrow / uppercase tag */
font-family: 'Manrope', system-ui, sans-serif;
font-weight: 600–700;
font-size: 12px;
text-transform: uppercase;
letter-spacing: 0.10em–0.14em;
```

The `12px` size is **only used with `text-transform: uppercase`** and a positive `letter-spacing`. Never use 12px for lowercase sentence-case text — it is too small to read comfortably.

---

## 4. Border Radius Scale

Five values only. All implemented as CSS variables.

| Token | Value | Shape | Use |
|-------|-------|-------|-----|
| `--radius-pill` | `999px` | Pill | Buttons, nav links, filter pills, badges, stock badge |
| `--radius-lg` | `20px` | Large card | Hero visual, service cards, equipment tiles, quote cards, infocard, CTA form, mobile menu |
| `--radius-md` | `16px` | Medium card | Course cards, hero badge, team float frame, mobile menu links |
| `--radius-sm` | `12px` | Small chip | Svc icon, mobile nav links, equipment label |
| `--radius-xs` | `8px` | Tiny chip | Direct-row icon in contact form |

**Exception:** Team cards (`.tcard`) use `border-radius: 14px` as a deliberate signal that "this is a person, not a content card." Do not change this to `--radius-md`.

**Never use:** `border-radius: 50%` on anything other than circular avatar-style elements (founder avatar, bullet icons, social icons). Everything else uses the scale above.

---

## 5. Component Specs

### 5a. Button — 3 variants

Base class: `.btn` (always required)

```html
<a class="btn btn-primary">Label <span class="arrow">→</span></a>
<a class="btn btn-ghost">Label</a>
<a class="btn btn-orange">Label <span class="arrow">→</span></a>
```

| Variant | Class | Resting bg | Text | Hover bg | Use |
|---------|-------|------------|------|----------|-----|
| Primary | `.btn-primary` | `--ink` | white | `--orange` | Main CTA (1 per page section max) |
| Ghost | `.btn-ghost` | transparent | `--ink` | `--ink` (filled) | Secondary action alongside primary |
| Orange | `.btn-orange` | `--orange` | white | `--orange-deep` | Dark section CTAs (AED section) |

Rules:
- `.btn-primary` has a one-shot glow animation on page load (`cta-glow` keyframe). Do not remove.
- The `.arrow` span adds the `→` and animates `translateX(3px)` on hover.
- Button `font-size` is `15px` — the only allowed exception to the Body 17px rule.
- Padding: `14px 22px` — satisfies P = 2F for `15px` button text (2×15 = 30px ≈ 28px, close enough).

### 5b. Inline Link

Not a button. Used for "see more" actions within cards and sections.

```html
<span class="svc-link">Se utvalget →</span>
```

Spec: `font-size: 14px; font-weight: 600; display: inline-flex; gap: 6px; color: --ink`. Hover: `color: --orange`.

### 5c. Section Card (content card)

Two canonical radii for cards. Choose based on content type.

**Large card — `--radius-lg` (20px)**
Used for: `.svc`, `.infocard`, `.qcard`, `.eq` (equipment tiles).

```html
<div class="svc">
  <div class="svc-icon"><!-- svg --></div>
  <span class="svc-num">01 / Category</span>
  <h3>Heading</h3>
  <p>Description</p>
  <span class="svc-link">Action →</span>
</div>
```

Padding: `28–48px` (≥ 2× body font size per P = 2F rule). Background: `--white`. Border: `1px solid --rule`. Hover: `translateY(-4px)` + `--shadow-lg` + border colour → `--orange`.

**Medium card — `--radius-md` (16px)**
Used for: `.c` (course cards).

Padding: `24px 22px`. Same border/hover pattern. Course cards include a `.course-footer` with flex-row layout for tag + action button.

### 5d. Quote Card — `.qcard`

Standalone — do not merge with section cards.

```html
<div class="qcard">
  <p class="body">Quote text.</p>
  <div class="who">
    <div class="name">NAME</div>
    <div class="role">TITLE</div>
    <div class="company">Company name</div>
  </div>
</div>
```

- `border-radius: var(--radius-lg)`
- `padding: 46px 28px 28px` — the top padding is intentionally large; it creates space for the 84px decorative `"` quote mark positioned at `top: 6px; left: 20px`. Do not reduce this top padding.
- Quote body: Source Serif 4, `17px`, italic, `font-weight: 400`
- Author name: Manrope, `14px`, `700 weight`, uppercase, `0.06em` tracking
- Role: Manrope, `12px`, uppercase, `0.1em` tracking, `--ink-3`
- Company: Source Serif 4, `17px`, italic, `--orange-deep`

### 5e. Team Card — `.tcard`

Uses `border-radius: 14px` (not a token — intentional person-card signal).

```html
<div class="tcard">
  <div class="tcard-photo"><!-- or tcard-photo is-placeholder -->
    <img src="..." />
    <div class="tcard-hover">
      <a class="tcard-btn"><!-- phone svg --></a>
      <a class="tcard-btn"><!-- email svg --></a>
    </div>
  </div>
  <div class="tcard-label">
    <div class="tcard-name">Full Name</div>
    <div class="tcard-title">Job Title</div>
  </div>
</div>
```

- Photo fills the square: `object-fit: cover; object-position: top center`
- Placeholder (no photo): `class="tcard-photo is-placeholder"` — white bg, `assets/favicon-192.png` at 45% size
- Hover overlay: `--overlay-dark` scrim with two `.tcard-btn` circles (phone + email)
- Label bar: `--glass-bg` backdrop, `border-radius: 0 0 14px 14px`
- Name: Manrope, `14px`, `700 weight`
- Title: Manrope, `12px`, `700 weight`, uppercase, `0.1em` tracking, `--ink-3`

### 5f. Eyebrow — `.eyebrow`

Used as a section label above every major heading.

```html
<span class="eyebrow">Category label</span>
```

Spec: Manrope `12px`, `600 weight`, uppercase, `0.14em` tracking, `--orange-deep`. Has a `22px × 1.5px` line before it via `::before`.

On dark sections: add inline override `style="color: var(--aed-accent)"` or use the `.aed .eyebrow` rule which handles it automatically.

### 5g. Filter Pill — `.team-pill` / `.faq-pill`

Both classes share identical CSS defined once in the main `<style>` block.

```html
<button class="team-pill is-active" data-group="Alle">Alle</button>
<button class="faq-pill" data-filter="Kurs">Kurs</button>
```

Spec: Manrope `14px`, `600 weight`, `var(--radius-pill)`, `padding: 8px 18px`, `1.5px` border. Resting: `--white` bg, `--rule` border, `--ink-2` text. Active: `--orange-soft` bg, `--orange-deep` text/border. Hover: `--orange` border, `--orange-deep` text.

### 5h. Glass Nav — `.pill-glass`

Shared material class applied to nav bar elements and mobile menu.

```html
<div class="... pill-glass">...</div>
```

Provides: `--glass-white` bg, `saturate(1.8) blur(18px)` backdrop filter, `rgba(255,255,255,0.55)` border, `--overlay-dark-light` box-shadow. Never add a second `background` rule on an element that already has `.pill-glass` — it will override the glass effect.

---

## 6. Section Structure Pattern

Every full-width section follows this HTML structure:

```html
<section id="section-id">
  <div class="container">
    <div class="section-head reveal">
      <span class="eyebrow">Category</span>
      <h2>Section heading.</h2>
      <p>Supporting description, max 62ch.</p>
    </div>
    <!-- section-specific content grid -->
  </div>
</section>
```

- `section` — `padding: var(--space-section) 0` (112px top/bottom)
- `.container` — `max-width: 1240px; padding: 0 32px`
- `.section-head` — `max-width: 780px; margin-bottom: var(--space-section-sm)` (64px)
- Section description (`p`) — inherits body (17px), `--ink-2`, `max-width: 62ch`
- Add `class="reveal"` to animate on scroll via IntersectionObserver

**Dark sections** (`.aed`, `.cta`, footer) override the colour palette internally. Use `--aed-accent` and `--aed-text` tokens. Never bring `--orange` or `--ink` into a dark section directly.

---

## 7. Dos and Don'ts

**Do:**
- Use CSS variables for every colour, radius, and spacing token
- Keep font sizes to the 4-tier system: Display (clamp), Title (22–26px), Body (17px inherited), Label (14px or 12px uppercase)
- Round spacing up to the nearest 4px step, never down
- Use `--radius-pill` for all interactive pill shapes (buttons, pills, badges)
- Test with `python3 -m http.server` before marking any UI work done

**Don't:**
- Introduce a new hex value — map it to an existing token or add a new one to `:root` and this document simultaneously
- Use `font-size: 17px` explicitly on body-level elements — the cascade inherits it
- Create a new card radius — use the 5 tokens in the scale
- Use `font-size` values ending in `.5` (13.5px, 14.5px, etc.) — these are eliminated
- Add a second `background` or `border-radius` on elements using `.pill-glass`
- Use `rgba()` directly in component rules — all rgba values are tokenised in `:root`
