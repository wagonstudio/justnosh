# Theme Design Guide — Just Nosh

> **Disclaimer:** This document describes **implemented site styles** detected in the theme codebase. It is not an official brand guidelines PDF from Just Nosh / JAG Provisions. For formal brand standards beyond what is coded, contact the brand owner.

**Related documents:** `06-assets-inventory.md` · `03-homepage-management-guide.md` · `04-technical-documentation.md`

---

## Design system overview

Just Nosh uses a **dual-layer design system**:

| Layer | Scope | Primary files |
|---|---|---|
| **WAGON custom system** | Custom pages (homepage, about, PDP, contact, affiliate, locator, footer) | `assets/master.css`, `assets/wagon.css`, `assets/home.css` |
| **Heritage theme system** | Header infrastructure, cart drawer, product form blocks, color schemes | `assets/base.css`, `snippets/theme-styles-variables.liquid`, `config/settings_data.json` |

Custom pages rely heavily on **CSS class utilities** (`.he-m`, `.bo-m`, `.btn-i`, `.container-i`) rather than Heritage typography presets.

**Base width reference:** 1440px (noted in `assets/master.css` header comment)  
**Responsive model:** VW-based typography and spacing at desktop breakpoints; Bootstrap 5 grid classes (`w-12`, `w-lg-6`, etc.) for layout

---

## 1. Brand colors (implemented site styles)

### WAGON custom palette (`assets/master.css` → `:root`)

These CSS variables drive custom Just Nosh sections:

| Variable | Hex | Typical usage |
|---|---|---|
| `--color-azul-profundo` | `#02021D` | Deepest navy |
| `--color-azul-oscuro` | `#1F2B3A` | Primary dark background (hero, footer, header scheme) |
| `--color-azul-medio-oscuro` | `#04204E` | Mid navy |
| `--color-azul-corporativo` | `#04204E` | Corporate blue (duplicate value) |
| `--color-azul-llamativo` | `#116AF8` | Accent blue |
| `--color-azul-claro` | `#C7E1F1` | Light blue |
| `--color-gris-oscuro` | `#334155` | Dark gray text |
| `--color-gris-medio` | `#687180` | Medium gray |
| `--color-gris-medio-claro` | `#CECECE` | Light gray border/divider |
| `--color-gris-claro` | `#F0F1F1` | Light gray background |
| `--color-gris-muy-claro` | `#F5F5F5` | Near-white gray |
| `--color-blanco` | `#F9F9F7` | Off-white page background |
| `--color-naranja` | `#F65B18` | Orange accent |
| `--color-naranja-oscuro` | `#DE4706` | Dark orange |
| `--color-naranja-claro` | `#FF7B42` | Light orange |
| `--color-rosa-claro` | `#FEEEE9` | Light pink/cream |
| `--color-rosa-oscuro` | `#DD9988` | Dark pink (secondary button) |
| `--color-rosa` | `#FEAA8F` | Primary CTA pink/salmon |

### Hardcoded accent colors (not in `:root`)

| Color | Hex | Where |
|---|---|---|
| Required field asterisk | `#F2272B` | `.field-affiliate label>strong`, `.action-affiliate .bo-s span` — `assets/wagon.css` |
| Button hover (pink II) | `#F3B9AA` | `.btn-i.btn-pink-ii:hover` — `assets/master.css` |
| Form success icon bg | `#0f5132` | `.form-success-message__icon` — `assets/home.css` |

### Heritage color schemes (`config/settings_data.json`)

Six schemes (`scheme-1` through `scheme-6`) control Heritage components. Key live values:

**scheme-1** (light — used in many section defaults):
- Background: `#F9F9F7`
- Foreground: `#1F2B3A`
- Primary button: `#FEAA8F` on `#1F2B3A` text

**scheme-5** (dark — header `color_scheme_top`):
- Background: `#1F2B3A`
- Foreground: `#F9F9F7`

**Per-product PDP colors:** Product metafields `custom.color_primary`, `color_secondary`, `color_tertiary`, `color_tertiary_contrast`, `color_light` override PDP styling inline per flavor (`sections/pdp-i.liquid`).

### Color usage patterns (implemented)

| Area | Background | Text |
|---|---|---|
| Homepage hero (Superhero) | `--color-azul-oscuro` | `--color-blanco` |
| Why / Shopping / Locator | `--color-blanco` | `--color-azul-oscuro` |
| Footer (Foot) | `--color-azul-oscuro` | `--color-blanco` |
| Primary CTA buttons | `--color-rosa` (`.btn-pink-i`) | `--color-azul-oscuro` |
| Secondary CTA buttons | `--color-rosa-oscuro` (`.btn-pink-ii`) | `--color-azul-oscuro` |
| Header (live) | scheme-5 (`#1F2B3A`) | `#F9F9F7` |

---

## 2. Typography (implemented site styles)

### Font stack summary

| Font family | Source | Used for |
|---|---|---|
| **Obviously Narrow** | Adobe Typekit kit `rji3kyn` — `layout/theme.liquid` | Headings (`.he-*`), specials (`.sp-*`), tags (`.tag-i`), nav, cart count |
| **Obviously** | Adobe Typekit kit `rji3kyn` | Body text classes (`.bo-*`), buttons (`.btn-i`), cart |
| **Instrument Sans** | Shopify font (`instrument_sans_n4`) — `config/settings_data.json` | Heritage theme components via `snippets/fonts.liquid` |
| **Work Sans** | Google Fonts CDN — `layout/theme.liquid` | Loaded globally; **no CSS reference detected in theme files** |
| **Albert Sans** | Referenced in `assets/master.css` (`.btn-ii`, `.btn-iii`) | **Not loaded in `layout/theme.liquid`** — likely unused or incomplete |

**Needs Shopify Admin confirmation:** Whether Typekit kit `rji3kyn` is active and licensed under the correct Adobe account.

### WAGON typography scale (`assets/master.css`)

Classes use **VW font sizes** at breakpoints. Base reference: 1440px viewport.

#### Headings — Obviously Narrow (`.he-*`)

| Class | Role | ~Size at XXL (>1600px) |
|---|---|---|
| `.he-xl` | Extra large display | 6vw |
| `.he-l` | Large hero headline | 5.3vw |
| `.he-m` | Section headings | 3.7vw |
| `.he-s` | Sub-headings | 2.2vw |
| `.he-xs` | Small headings | 1.5vw |

#### Body — Obviously (`.bo-*`)

| Class | Role | ~Size at XXL |
|---|---|---|
| `.bo-xl` | Extra large body | 1.9vw |
| `.bo-l` | Large body | 1.1vw |
| `.bo-m` | Default body (weight 500) | 0.9vw |
| `.bo-s` | Small body | 0.8vw |
| `.bo-xs` | Extra small / form inputs | 0.7vw |

#### Special / UI — Obviously Narrow (`.sp-*`)

| Class | Role | ~Size at XXL |
|---|---|---|
| `.sp-s` | Price, labels | 1.25vw |
| `.sp-xs` | Smaller UI text | 1.7vw |
| `.sp-xxs` | Variant labels, micro UI | 1.1vw |

#### Tags

| Class | Font | Usage |
|---|---|---|
| `.tag-i` | Obviously Narrow | Product badges (“12 BARS”), pills |

### Font weight utilities (`.fw-*`)

Applied via utility classes on headings and text:

| Class | Weight |
|---|---|
| `.fw-thin` | 100 |
| `.fw-extralight` | 200 |
| `.fw-light` | 300 |
| `.fw-regular` | 400 |
| `.fw-medium` | 500 |
| `.fw-semibold` | 600 |
| `.fw-bold` | 700 |
| `.fw-black` | 800 |
| `.fw-extrablack` | 900 |

### Line height tokens

| Variable | Value |
|---|---|
| `--lh-tight` | 100% |
| `--lh-base` | 110% |
| `--lh-loose` | 120% |

### Text transforms

| Class | Effect |
|---|---|
| `.uppercase` | `text-transform: uppercase` — widely used on headings and buttons |

### Heritage typography (`config/settings_data.json`)

Configured but primarily affects **non-custom Heritage sections**:

| Setting | Value |
|---|---|
| Body font | Instrument Sans |
| H1 size | 72px |
| H2 size | 48px |
| H3 size | 32px |
| Paragraph size | 14px |
| H6 case | uppercase |

### Style guide page

Internal reference at `sections/master.liquid` / `templates/page.master.json` demonstrates all typography classes and button variants.

---

## 3. Spacing & layout conventions

### Spacing scale (`assets/master.css` → `:root`)

| Token | Value | Usage |
|---|---|---|
| `--gap-i` | 1vw | Default flex/grid gap |
| `--gap-ii` | 1.5vw | Medium gap |
| `--gap-iii` | 2vw | Large gap |
| `--radius-i` | 0.5vw | Small border radius |
| `--radius-ii` | 0.8vw | Medium radius |
| `--radius-iii` | 1vw | Large radius |
| `--radius-iv` | 1.6vw | XL radius |

### Gap utility classes

| Class | Gap |
|---|---|
| `.gap-i` | 1vw |
| `.gap-ii` | 2vw |
| `.gap-iii` | 3vw |
| `.gap-iv` | 4vw |
| `.gap-v` | 5vw |
| `.gap-vi` | 6vw |
| `.gap-vii` | 7vw |
| `.gap-viii` | 8vw |
| `.gap-ix` | 9vw |
| `.gap-x` | 10vw |

### Container widths

| Class | Behavior |
|---|---|
| `.container-i` | ~90% width (at XXL breakpoint) |
| `.container-ii` | ~70% width |
| `.container-iii` | ~70% width |
| `.container-i-fix` | max 1500px at XXL; used on locator page |

### Layout utilities

| Class | Effect |
|---|---|
| `.flex-i` | `display: flex; flex-wrap: wrap` |
| `.space-between` | `justify-content: space-between` |
| `.align-center` | `align-items: center` |
| `.flex-column` / `.flex-row` | Direction |
| `.w-100` / `.h-100` | Full width/height |

### Bootstrap integration

`layout/theme.liquid` loads Bootstrap 5.3.3. Custom sections use Bootstrap grid classes alongside WAGON utilities (e.g. `w-12 w-lg-6`, `d-none d-md-block`).

### Breakpoints (`assets/master.css`)

| Range | Media query |
|---|---|
| XXL | `min-width: 1600px` |
| XL | `1200px – 1600px` |
| LG | `992px – 1200px` |
| MD | `768px – 992px` |
| SM | `576px – 768px` |
| XS | `340px – 576px` |

Plus Bootstrap breakpoints at 576, 768, 992, 1200px for grid columns.

### Section padding pattern

Most custom sections use `5vw` vertical padding (e.g. `.section-locator-i`, hero min-height transitions in `assets/home.css`).

---

## 4. Buttons, forms, cards & banners

### Buttons (`assets/master.css`)

| Class | Style | Usage |
|---|---|---|
| `.btn-i` | Pill shape (`border-radius: 999rem`), uppercase, Obviously font, weight 600 | Primary site CTA |
| `.btn-i.btn-pink-i` | Rosa background, azul-oscuro text | Hero CTA (“Shop bars”) |
| `.btn-i.btn-pink-ii` | Rosa-oscuro background; hover `#F3B9AA` | Forms, shop cards, cart CTA |
| `.btn-i.outline-i` | Border outline, no uppercase | Our Mission CTA |
| `.btn-ii` | Smaller padding variant | Style guide / rare use |
| `.btn-iii` | Smallest button variant | Style guide / rare use |
| `.btn-continue` | Rosa-oscuro text, centered | Cart continue shopping |

**Heritage buttons** (buy buttons, accelerated checkout) use scheme colors from `config/settings_data.json` — styled via `assets/base.css` and block snippets.

### Forms (`.field-affiliate`)

Used on Contact, Affiliate, and Store Locator forms:

| Element | Style |
|---|---|
| `.field-affiliate` | Flex column; label uppercase (`.bo-m`) |
| Required `*` | Red `#F2272B` |
| Inputs / textareas | Class `.bo-xs`; placeholder opacity 0.5 |
| Submit | `.btn-i.btn-pink-ii.w-12` |
| Success message | `.form-success-message` — green icon, centered text |

Form field labels and placeholders are **hardcoded in Liquid** — not Theme Editor settings.

### Product cards (`.item-shopping`)

Shopping grid cards (`sections/shopping.liquid`, `assets/wagon.css`):

| Element | Class / behavior |
|---|---|
| Card wrapper | `.item-shopping` |
| Badge | `.tag-i` — from product metafield `custom.bars` |
| Image area | `.media-shopping` — hover reveals `.media-metafield` decor images |
| Title | `.he-s.fw-extrablack.uppercase` |
| Price | `.sp-s.fw-semibold.uppercase` |
| CTA | `.btn-i.btn-pink-ii` |
| Hover | Background overlay via `:before` pseudo; decor images animate |

### Benefit cards (`.item-why`)

Why section and About III: icon + title (`.he-xs`) + text (`.bo-m`).

### Banners

| Type | Status |
|---|---|
| Announcement bar | **Not active** on live site (`header-group.json` has no announcements section) |
| Hero banner | **Superhero** section — full-viewport hero, not a slim banner |
| Footer CTA | **Foot** section — “FIND US NEAR YOU” retailer CTA block |
| PDP per-flavor theming | Inline styles from product color metafields |

### Icons (UI)

Heritage UI icons: SVG files in `assets/icon-*.svg`, rendered via `inline_asset_content` in snippets (cart, search, close, chevrons, etc.). See `06-assets-inventory.md`.

Social icons: rendered inline in `snippets/icon.liquid` (Instagram, Facebook, TikTok, LinkedIn).

---

## 5. Visual consistency notes

### Two systems coexist

- **Custom pages** use WAGON classes + Obviously type + `--color-*` variables.
- **Heritage components** (cart drawer internals, variant picker in disabled product-information, search modal) use Instrument Sans + Heritage color schemes.

When editing, match the system for the area you are changing.

### Uppercase convention

Headings, nav links, buttons, prices, and badges consistently use `.uppercase` on custom pages.

### Color alignment

`--color-blanco` (`#F9F9F7`) matches scheme-1 background. `--color-azul-oscuro` (`#1F2B3A`) matches scheme-5 header and scheme-1 foreground. The palette is internally consistent between WAGON and Heritage schemes.

### Per-product PDP colors

Each flavor can have unique PDP colors via metafields. This is intentional for flavor differentiation but means PDP pages will not look identical across products.

### Decorative ingredient SVGs

The same chip, peanut, chocolate, and lemon SVG assets are reused across homepage, about, contact, and affiliate pages for visual consistency.

### Animation consistency

- Hero entrance: CSS `.hero-ready` stagger (`assets/home.css`)
- Scroll animations: `data-anim` attributes via `assets/wagon.js`
- Shopping card hover: CSS-only transitions (`assets/wagon.css`)
- Reduced motion: `@media (prefers-reduced-motion: reduce)` respected on hero (`assets/home.css`)

### Known inconsistencies (from code)

| Issue | Detail |
|---|---|
| Albert Sans referenced but not loaded | `assets/master.css` `.btn-ii`/`.btn-iii` |
| Work Sans loaded but unused in CSS | `layout/theme.liquid` Google Fonts link |
| Typo in CSS | `.btn-i.outline-i` uses `var(--color-aazul-oscuro)` (double "a") — may break outline color |
| Two logo file references | `Justnosh-Logo.svg` (theme settings) vs `Justnosh-Logo_47daa248-....svg` (footer section) |

---

## 6. Theme Customizer vs CSS

### Edit in Theme Customizer (no developer)

| Item | Where |
|---|---|
| Logo | Theme settings → Logo, or **Header** / **Foot** section |
| Favicon | Theme settings → Favicon |
| Heritage color schemes | Theme settings → Colors (schemes 1–6) |
| Header color scheme | Theme Editor → Header → `color_scheme_top` (live: scheme-5) |
| Section images & copy | Theme Editor per page section |
| Product PDP colors | Products → Metafields (`color_*`) |
| Footer social URLs | Theme Editor → **Foot** |
| Instrument Sans sizes | Theme settings → Typography (affects Heritage areas) |

### Edit in CSS / requires developer

| Item | File(s) |
|---|---|
| WAGON color palette (`--color-*`) | `assets/master.css` |
| Typography VW scale (`.he-*`, `.bo-*` sizes) | `assets/master.css` |
| Button styles (`.btn-i`, variants) | `assets/master.css` |
| Section-specific layout & hover effects | `assets/wagon.css`, `assets/home.css` |
| Hero image positions | `assets/home.css` |
| Form field labels & placeholders | Section `.liquid` files |
| Animation behavior | `assets/wagon.js`, `assets/home.css` |
| Header drawer custom styles | `assets/wagon.css`, `snippets/header-drawer.liquid` |
| Cart drawer typography overrides | `assets/wagon.css`, `assets/home.css`, `snippets/header-actions.liquid` |
| Adobe Typekit kit / font loading | `layout/theme.liquid` |
| Bootstrap version | `layout/theme.liquid` |

### Edit in Shopify Files (Content → Files)

| Item | Examples |
|---|---|
| Brand logo SVG | `Justnosh-Logo.svg` |
| Favicon | `fav.png` |
| Decorative ingredient SVGs | `chip-1.svg`, `mani-1.svg`, etc. |
| Hero bar PNGs | Hashed PNG filenames in homepage template |
| Benefit icons | `muscle.svg`, `digestion.svg`, etc. |
| Context photography | `JustNosh-Context-Image-01.jpg`, etc. |

### Do not edit for design changes

| Item | Reason |
|---|---|
| `assets/base.css` | Heritage core — changes affect all Heritage components globally |
| `snippets/theme-styles-variables.liquid` | Heritage CSS variable generator — high impact |

---

## Quick reference: class cheat sheet

```
Headings:     .he-l .he-m .he-s .he-xs  + .fw-extrablack .uppercase
Body:         .bo-m .bo-s .bo-xs         + .fw-medium
Prices:       .sp-s .fw-semibold .uppercase
Badges:       .tag-i .sp-s .uppercase
Buttons:      .btn-i .btn-pink-i | .btn-pink-ii | .outline-i
Layout:       .container-i .flex-i .gap-i
Colors:       var(--color-azul-oscuro) var(--color-blanco) var(--color-rosa)
```

---

*Last updated: June 2026 · Implemented styles detected from `assets/master.css`, `assets/wagon.css`, `assets/home.css`, `layout/theme.liquid`, and `config/settings_data.json`*
