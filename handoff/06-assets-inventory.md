# Assets Inventory — Just Nosh

> **Disclaimer:** This inventory is based on files present in the theme codebase and references detected in templates/config. Shopify-hosted images (`shopify://shop_images/...`) are stored in **Shopify Admin → Content → Files**, not in the local `assets/` folder. A complete Files inventory requires **Needs Shopify Admin confirmation**.

**Related documents:** `03-theme-design-guide.md` · `04-technical-documentation.md` · `03-homepage-management-guide.md`

---

## Asset storage locations

| Location | Contains |
|---|---|
| `assets/` (theme folder) | CSS, JS, SVG UI icons — 119 files |
| Shopify Files (`shop_images`) | Brand logos, favicon, photography, decorative SVGs/PNGs |
| Product Media | Per-product images (not listed here — see `08-product-management-guide.md`) |
| Adobe Typekit (CDN) | Obviously / Obviously Narrow fonts — kit `rji3kyn` |
| Google Fonts (CDN) | Work Sans |
| Shopify Fonts | Instrument Sans (via theme settings) |

---

## 1. Logos detected

### Theme settings (`config/settings_data.json`)

| Asset | Shopify Files path | Used for |
|---|---|---|
| Default logo | `shopify://shop_images/Justnosh-Logo.svg` | Header (`settings.logo`) |
| Inverse logo | `shopify://shop_images/Justnosh-Logo.svg` | Header on dark/transparent contexts (`settings.logo_inverse`) |
| Logo height (desktop) | — | 30px (`settings.logo_height`) |
| Logo height (mobile) | — | 25px (`settings.logo_height_mobile`) |

**Rendered via:** `blocks/_header-logo.liquid`, Heritage header system  
**Theme Customizer:** Theme settings → Logo, or Header section

### Footer logo (`sections/footer-group.json`)

| Asset | Shopify Files path |
|---|---|
| Footer logo | `shopify://shop_images/Justnosh-Logo_47daa248-8609-4652-9832-a80025e4c2a3.svg` |

**Note:** Footer uses a **different filename** than theme settings logo (UUID-suffixed variant). Both appear to be Just Nosh logo SVGs. **Needs Shopify Admin confirmation** whether they are identical artwork.

**Rendered via:** `sections/foot.liquid` → `section.settings.logo`  
**Theme Customizer:** Footer → **Foot** section → Logo

---

## 2. Favicons detected

| Asset | Path | Config reference |
|---|---|---|
| Favicon | `shopify://shop_images/fav.png` | `config/settings_data.json` → `settings.favicon` |

**Rendered via:** `layout/theme.liquid` → `<link rel="icon">` (32×32)  
**Theme Customizer:** Theme settings → Favicon

---

## 3. Custom images (Shopify Files)

Images referenced in theme JSON templates and section settings. Stored in Shopify Admin, not in `assets/`.

### Homepage (`templates/index.json`)

| Filename | Used in | Role |
|---|---|---|
| `52d456a12497efc00e453e73a2e8f82aef379e58.png` | Superhero → Bar 1 | Product bar image |
| `6cbb002ee2c6f5222dbd72126c0e3939c2797773.png` | Superhero → Bar 2 | Product bar image |
| `ff599838db21b7cefb7802f2c34fc47a1c78ce1a.png` | Superhero → Bar 3 | Product bar image |
| `chip-3.svg` | Superhero → Chocolate 1 | Decor |
| `chip-1.svg` | Superhero → Chocolate 2; Our mission → Detail 5 | Decor |
| `chip-2.svg` | Superhero → Chocolate 3 | Decor |
| `chocolate-1.svg` | Superhero → Chocolate 4; Our mission → Detail 3 | Decor |
| `chocolate-2.svg` | Superhero → Chocolate 5; Our mission → Detail 4 | Decor |
| `mani-2.svg` | Superhero → Peanut 1; Our mission → Detail 1; Contact II → Decor 1 | Decor |
| `mani-1.svg` | Superhero → Peanut 2; Our mission → Detail 2; Contact II → Decor 2 | Decor |
| `lemon-1.svg` | Superhero → Lemon 1; Our mission → Detail 6; Contact II → Decor 5 | Decor |
| `muscle.svg` | Why → Why item (block) | Benefit icon |
| `digestion.svg` | Why → Why item (block) | Benefit icon |
| `chemistry.svg` | Why → Why item (block) | Benefit icon |
| `oil.svg` | Why → Why item (block) | Benefit icon |

### About page (`templates/page.about.json`)

| Filename | Used in | Role |
|---|---|---|
| `chip-1.svg` | About I → Decor block | Floating decor |
| `lemon-1.svg` | About I → Decor block | Floating decor |
| `chocolate-salt-1.svg` | About I → Decor block | Floating decor |
| `chocolate-salt-2.svg` | About I → Decor block | Floating decor |
| `mani-1.svg` | About I → Decor block | Floating decor |
| `mani-2.svg` | About I → Decor block | Floating decor |
| `JustNosh-Context-Image-01.jpg` | About II | Side image |
| `muscle.svg` | About III → About item | Benefit icon |
| `digestion.svg` | About III → About item | Benefit icon |
| `chemistry.svg` | About III → About item | Benefit icon |
| `oil.svg` | About III → About item | Benefit icon |
| `1769a96bd6f28c707c8f768d2db9892e6d17ddec.jpg` | About V | Founder photo |

### Affiliate page (`templates/page.affiliate.json`)

| Filename | Used in | Role |
|---|---|---|
| `JustNosh-Context-Image-02.jpg` | Affiliate II | Side image |
| `sheet.svg` | Affiliate III → Item | Benefit icon |
| `heart.svg` | Affiliate III → Item | Benefit icon |
| `microphone.svg` | Affiliate III → Item | Benefit icon |
| `medal.svg` | Affiliate III → Item | Benefit icon |
| `3a77e42014d92935027022cb38e7b3c863b4a063.png` | Affiliate V → Bar image 1 | Form decor |
| `2cd7c54b442dd25b92ecc58009f6be8cc25465dc.png` | Affiliate V → Bar image 2 | Form decor |
| `02baf466d338bceb30ac7d64c5c807470d45df16.png` | Affiliate V → Bar image 3 | Form decor |

### Contact page (`templates/page.contact.json`)

| Filename | Used in | Role |
|---|---|---|
| `mani-2.svg` | Contact II → Decor image 1 | Decor |
| `mani-1.svg` | Contact II → Decor image 2 | Decor |
| `chocolate-salt-1.svg` | Contact II → Decor image 3 | Decor |
| `chocolate-salt-2.svg` | Contact II → Decor image 4 | Decor |
| `lemon-1.svg` | Contact II → Decor image 5 | Decor |

### Product images

Product photography and PDP-specific images (ingredients, nutrition labels, hover media) are managed per product via **Products → Media** and **product metafields** — not listed here. See `08-product-management-guide.md`.

**Needs Shopify Admin confirmation:** Full list of files in Shopify Content → Files beyond those referenced above.

---

## 4. Icons

### UI icons — theme `assets/` folder (Heritage set)

29 SVG files used by Heritage theme snippets via `{{ 'icon-*.svg' | inline_asset_content }}`:

| File | Typical usage |
|---|---|
| `assets/icon-account.svg` | Account button |
| `assets/icon-add-to-cart.svg` | Quick add |
| `assets/icon-arrow.svg` | Slideshow arrows |
| `assets/icon-available.svg` | Inventory status |
| `assets/icon-caret.svg` | Dropdowns, slideshow |
| `assets/icon-cart.svg` | Cart (if used) |
| `assets/icon-checkmark.svg` | Selected states, volume pricing |
| `assets/icon-checkmark-burst.svg` | Success burst |
| `assets/icon-chevron-left.svg` | Navigation |
| `assets/icon-chevron-right.svg` | Navigation |
| `assets/icon-close.svg` | Modals, drawers, cart close |
| `assets/icon-delete.svg` | Remove item |
| `assets/icon-discount.svg` | Discount badge |
| `assets/icon-double-chevron.svg` | Fast scroll |
| `assets/icon-error.svg` | Error states |
| `assets/icon-external.svg` | External links |
| `assets/icon-filter.svg` | Collection filters |
| `assets/icon-filters-close.svg` | Close filters |
| `assets/icon-grid-default.svg` | Grid view |
| `assets/icon-grid-dense.svg` | Dense grid view |
| `assets/icon-info.svg` | Info tooltips |
| `assets/icon-inventory.svg` | Stock indicators |
| `assets/icon-menu.svg` | Mobile menu |
| `assets/icon-minus.svg` | Quantity decrease |
| `assets/icon-one-col-mobile.svg` | Mobile layout |
| `assets/icon-orders.svg` | Order history |
| `assets/icon-pause.svg` | Video pause |
| `assets/icon-play.svg` | Video play |
| `assets/icon-plus.svg` | Quantity increase |
| `assets/icon-reset.svg` | Reset filters |
| `assets/icon-search.svg` | Search modal |
| `assets/icon-shopify.svg` | Shopify badge |
| `assets/icon-unavailable.svg` | Sold out |

**Referenced from:** `snippets/*.liquid` (cart, search, product media, sorting, video, etc.)

### Social icons — inline SVG (`snippets/icon.liquid`)

Rendered as inline SVG markup (not separate asset files): Instagram, Facebook, TikTok, Pinterest, LinkedIn, YouTube, and others.

**Used in:** `sections/foot.liquid`, `snippets/header-drawer.liquid`

### Benefit / decor icons — Shopify Files

See Section 3 (`muscle.svg`, `digestion.svg`, `sheet.svg`, `heart.svg`, etc.) — uploaded images/SVGs, not in `assets/`.

---

## 5. Fonts

### Loaded in `layout/theme.liquid`

| Font | Source URL / method | CSS family name |
|---|---|---|
| Obviously + Obviously Narrow | `https://use.typekit.net/rji3kyn.css` | `'obviously'`, `'obviously-narrow'` |
| Work Sans | `https://fonts.googleapis.com/css2?family=Work+Sans:...` | `'Work Sans'` |
| Bootstrap Icons | (via Bootstrap CSS, if any) | — |

### Loaded via Shopify (`snippets/fonts.liquid`)

| Font | Setting | File |
|---|---|---|
| Instrument Sans | `type_body_font`, `type_subheading_font`, `type_heading_font`, `type_accent_font` = `instrument_sans_n4` | Preloaded WOFF2 from Shopify CDN |

### Referenced but not loaded

| Font | Reference | Status |
|---|---|---|
| Albert Sans | `assets/master.css` → `.btn-ii`, `.btn-iii` | **Not loaded** in `layout/theme.liquid` — likely unused |

### Animation / library CSS in `assets/`

| File | Purpose |
|---|---|
| `assets/splitting.css` | Splitting.js text animation base styles |
| `assets/splitting-cells.css` | Splitting.js cell styles |

Loaded in `layout/theme.liquid` alongside CDN Splitting.js.

---

## 6. Custom CSS & JS assets (Just Nosh / WAGON)

| File | Type | Loaded in | Purpose |
|---|---|---|---|
| `assets/master.css` | CSS | `layout/theme.liquid` | Design system: colors, typography, buttons, layout utilities |
| `assets/wagon.css` | CSS | `layout/theme.liquid` | Section styles: header, footer, why, shopping, about, affiliate, contact, locator, cart |
| `assets/home.css` | CSS | `layout/theme.liquid` | Homepage hero, PDP, cart, Seal Subscriptions overrides, hero animations |
| `assets/wagon.js` | JS | `layout/theme.liquid` | Lenis, GSAP animations, Splide init, form success, page entrance |
| `assets/splitting.css` | CSS | `layout/theme.liquid` | Text splitting animations |
| `assets/splitting-cells.css` | CSS | `layout/theme.liquid` | Text splitting cell styles |

### Heritage base assets (also in `assets/`)

| File | Purpose |
|---|---|
| `assets/base.css` | Heritage core styles — loaded via `snippets/stylesheets.liquid` |
| `assets/overflow-list.css` | Overflow list component |
| `assets/template-giftcard.css` | Gift card template only (`templates/gift_card.liquid`) |

### Heritage JavaScript (`assets/*.js`)

~90 JS module files for Heritage functionality (cart-drawer, product-form, variant-picker, media-gallery, predictive-search, etc.). Loaded conditionally via `snippets/scripts.liquid` and section-specific script tags.

### Dev / config files in `assets/`

| File | Purpose |
|---|---|
| `assets/jsconfig.json` | JS path aliases for IDE |
| `assets/global.d.ts` | TypeScript declarations |

---

## 7. Assets potentially unused or low-use

Assessment based on code reference searches. “Potentially unused” means **no reference detected in Liquid/JSON/CSS** — the file may still be used dynamically or by Heritage in edge cases.

### Likely unused / dead references

| Item | Evidence |
|---|---|
| **Work Sans** (Google Fonts CDN) | Loaded in `layout/theme.liquid` but no `font-family: 'Work Sans'` found in theme CSS/Liquid |
| **Albert Sans** | Referenced in `assets/master.css` for `.btn-ii`/`.btn-iii` but never loaded |
| `.btn-ii`, `.btn-iii` button classes | Defined in `assets/master.css` and `sections/master.liquid` style guide; **no custom section uses them on live pages** |
| `assets/template-giftcard.css` | Only relevant if gift card template is actively used |

### Heritage JS — low traffic on Just Nosh custom pages

These Heritage modules are loaded globally or conditionally but have **minimal or no interaction** on the heavily customized Just Nosh page set:

| File | Notes |
|---|---|
| `assets/product-hotspot.js` | Hotspot sections not used in live templates |
| `assets/quick-order-list.js` | Quick order not in live templates |
| `assets/comparison-slider.js` | Comparison slider blocks not detected in live templates |
| `assets/jumbo-text.js` | Jumbo text blocks not in custom page templates |
| `assets/layered-slideshow.js` | Loaded in `snippets/scripts.liquid` but no layered slideshow in live `index.json` |
| `assets/marquee.js` | Marquee section not in live templates |

> Heritage assets are part of the base theme and should **not be deleted** without developer review — they support Theme Editor sections that may be added later.

### Duplicate / overlapping logo files

| File 1 | File 2 | Note |
|---|---|---|
| `Justnosh-Logo.svg` | `Justnosh-Logo_47daa248-....svg` | Two logo uploads; consolidate in Shopify Files if identical |

### Shopify Files images not in templates

Any files uploaded to Shopify Content → Files that are **not** listed in Section 3 may be unused, archived creative, or used only in product metafields. **Needs Shopify Admin confirmation.**

---

## 8. External CDN assets (not in `assets/`)

Loaded from `layout/theme.liquid`:

| Resource | URL |
|---|---|
| Bootstrap CSS 5.3.3 | `cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css` |
| Bootstrap JS 5.3.3 | `cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js` |
| Splide CSS 4.1.4 | `cdn.jsdelivr.net/npm/@splidejs/splide@4.1.4/dist/css/splide-core.min.css` |
| Splide JS 4.1.4 | `cdn.jsdelivr.net/npm/@splidejs/splide@4.1.4/dist/js/splide.min.js` |
| jQuery 3.6.0 | `code.jquery.com/jquery-3.6.0.min.js` |
| GSAP 3.12.5 | `cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js` |
| ScrollTrigger 3.12.5 | `cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js` |
| Lenis 1.0.44 | `unpkg.com/lenis@1.0.44/dist/lenis.min.js` |
| Splitting 1.0.6 | `cdn.jsdelivr.net/npm/splitting@1.0.6/dist/splitting.min.js` |
| Adobe Typekit | `use.typekit.net/rji3kyn.css` |
| Google Fonts (Work Sans) | `fonts.googleapis.com/css2?family=Work+Sans:...` |

### Per-page external assets

| Resource | Loaded from | Page |
|---|---|---|
| Stockist widget JS | `stockist.co/embed/v1/widget.min.js` | Store Locator (`sections/locator-i.liquid`) |

---

## Asset path quick reference

```
Theme folder (local):
  assets/master.css          ← Design system
  assets/wagon.css           ← Section styles
  assets/home.css            ← Homepage + PDP styles
  assets/wagon.js            ← Animations + interactions
  assets/base.css            ← Heritage base
  assets/icon-*.svg          ← UI icons (29 files)
  assets/*.js                ← Heritage JS modules (~90 files)

Shopify Admin → Files:
  Justnosh-Logo.svg
  Justnosh-Logo_47daa248-....svg
  fav.png
  chip-*.svg, chocolate-*.svg, mani-*.svg, lemon-*.svg
  chocolate-salt-*.svg
  muscle.svg, digestion.svg, chemistry.svg, oil.svg
  sheet.svg, heart.svg, microphone.svg, medal.svg
  JustNosh-Context-Image-01.jpg, -02.jpg
  [product bar PNGs — hashed filenames]
  [founder photo — hashed filename]

CDN:
  use.typekit.net/rji3kyn.css  ← Obviously fonts
  fonts.googleapis.com         ← Work Sans
  cdn.jsdelivr.net               ← Bootstrap, Splide, Splitting
  cdnjs.cloudflare.com           ← GSAP
  unpkg.com                      ← Lenis
```

---

## Maintenance notes

1. **Brand images** live in Shopify Files — update via Admin → Content → Files, then re-select in Theme Editor if needed.
2. **Theme `assets/` folder** is deployed with the theme — update via code editor or Shopify CLI.
3. **Do not delete Heritage `icon-*.svg` or JS files** without developer review.
4. **Consolidate duplicate logos** in Shopify Files if confirmed identical.
5. **Typekit kit** (`rji3kyn`) must remain active for custom pages to render correctly.
6. Export Shopify Files periodically as backup — they are not included in theme zip downloads.

---

*Last updated: June 2026 · Shopify Files list compiled from references in `templates/*.json`, `config/settings_data.json`, and `sections/footer-group.json`*
