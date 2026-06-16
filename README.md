# Just Nosh — Shopify Theme

**Website:** [eatjustnosh.com](https://eatjustnosh.com)  
**Client:** JAG Provisions  
**Platform:** Shopify  
**Base theme:** Heritage 3.5.1 (Shopify)  
**Custom layer:** WAGON STUDIO

---

Shopify theme for Just Nosh, a collagen protein bar brand. The site extends Shopify's Heritage theme with custom pages, product layouts, animations, and integrations for subscriptions, affiliates, and retail store discovery.

**Client-facing handoff documentation** lives in [`handoff/`](handoff/). Start with [`handoff/JUSTNOSH-FINAL-HANDOFF-DOCUMENTATION.md`](handoff/JUSTNOSH-FINAL-HANDOFF-DOCUMENTATION.md) for a consolidated overview.

---

## 1. Project Overview

Just Nosh is a Shopify storefront built for ingredient-focused consumers. The theme combines Heritage's cart, checkout, and product infrastructure with a custom design and page system developed under the **WAGON** layer.

| Area | Approach |
|---|---|
| Homepage | Four custom sections (hero, benefits, product grid, mission) |
| Product pages | Custom PDP with metafield-driven content and per-flavor theming |
| Content pages | Custom multi-section layouts for About, Contact, Affiliate, and Store Locator |
| Cart | Heritage drawer with custom typography and copy |
| Footer | Custom footer section replacing Heritage default |

Most day-to-day content updates are done in **Shopify Admin** and the **Theme Editor**. Structural, animation, and design-system changes require theme code access.

---

## 2. Theme Architecture

The theme runs as a **dual-layer stack**:

```
layout/theme.liquid
├── Heritage 3.5.1          base.css, Web Components, color schemes, cart/product logic
└── WAGON custom layer      master.css, wagon.css, home.css, wagon.js + custom sections
```

### Heritage layer

- Core styles: `assets/base.css` via `snippets/stylesheets.liquid`
- ES module system: `snippets/scripts.liquid` (importmap `@theme/*`)
- Web Components: cart drawer, product form, variant picker, search
- Color schemes and typography settings: `config/settings_data.json`

### WAGON layer

Loaded globally from `layout/theme.liquid`:

| Asset | Role |
|---|---|
| `assets/master.css` | Design system — colors, typography, buttons, layout utilities |
| `assets/wagon.css` | Section-specific styles (header, footer, pages, cart overrides) |
| `assets/home.css` | Homepage hero, PDP styles, Seal Subscriptions overrides |
| `assets/wagon.js` | GSAP animations, Lenis smooth scroll, Splide, form success handling |

### External CDN dependencies

Bootstrap 5.3.3, jQuery 3.6.0, GSAP 3.12.5, Lenis 1.0.44, Splide 4.1.4, Splitting 1.0.6, Adobe Typekit (`rji3kyn`), and Google Fonts (Work Sans) are loaded from `layout/theme.liquid`.

### Entry point

```liquid
<!-- layout/theme.liquid -->
<main id="MainContent" data-template="{{ template }}">
  {{ content_for_layout }}
</main>
```

---

## 3. Repository Structure

```
justnosh/
├── assets/           CSS, JS, SVG icons (~119 files)
├── blocks/           Heritage block system (93 files)
├── config/           settings_schema.json, settings_data.json
├── handoff/          Client and developer documentation
├── layout/           theme.liquid, password.liquid
├── locales/          Heritage translations (51 files)
├── sections/         Heritage + custom Just Nosh sections (67 files)
├── snippets/         Reusable Liquid fragments (103 files)
└── templates/        JSON templates + gift_card.liquid (17 files)
```

### Key configuration files

| File | Purpose |
|---|---|
| `config/settings_schema.json` | Theme settings definition (Heritage 3.5.1) |
| `config/settings_data.json` | Live theme settings snapshot (colors, logo, app blocks) |
| `sections/header-group.json` | Header section group |
| `sections/footer-group.json` | Custom footer (`foot`) section group |

### Custom vs Heritage sections

Custom Just Nosh sections use descriptive names: `superhero`, `why`, `shopping`, `pdp-i`, `about-i`, `affiliate-i`, `locator-i`, `foot`, etc. Heritage sections (`header`, `main-cart`, `slideshow`, etc.) remain available but many are not used on live page templates.

---

## 4. Homepage Architecture

**Template:** `templates/index.json`

Four custom sections in fixed order:

| Order | Section | File | Role |
|---|---|---|---|
| 1 | Superhero | `sections/superhero.liquid` | Hero headline, CTA, layered bar and ingredient images |
| 2 | Why | `sections/why.liquid` | Four benefit blocks with icons |
| 3 | Shopping | `sections/shopping.liquid` | Product flavor grid from a collection |
| 4 | Our Mission | `sections/ourmission.liquid` | Mission copy, CTA, decorative graphics |

### Notes

- **No announcement bar** is active in `sections/header-group.json`.
- **No carousel** on the homepage — the Shopping section is a static product grid.
- Hero entrance animation is CSS-driven (`.hero-ready` class) via `assets/wagon.js` and `assets/home.css`.
- Product badges on shop cards come from the product metafield `custom.bars`, not from section settings.
- If no collection is selected in Shopping, the theme falls back to `collections['frontpage']`.

**Guide:** [`handoff/03-homepage-management-guide.md`](handoff/03-homepage-management-guide.md)

---

## 5. Product Page Architecture

**Template:** `templates/product.json`

| Order | Section | File | Role |
|---|---|---|---|
| 1 | PDP I | `sections/pdp-i.liquid` | Main gallery (Splide), product info, buy buttons, Seal Subscriptions widget |
| 2 | PDP II | `sections/pdp-ii.liquid` | Key ingredients list and images |
| 3 | PDP III | `sections/pdp-iii.liquid` | Tabs: Nutrition Facts, Collagen, Seed Oil Free |
| 4 | PDP Recommendations | `sections/pdp-recommendations.liquid` | Related flavor grid |

The Heritage `product-information` section is present in the template but **disabled**. Do not re-enable without developer review.

### Product metafields (namespace: `custom`)

Metafields drive PDP content, shop card badges, hover images, and per-flavor colors. Key fields:

`bars`, `shopping_media`, `pdp_intro`, `flavor_profile_title`, `flavor_profile_text`, `price_suffix_label`, `pdp_highlights`, `color_primary` through `color_light`, `key_ingredients`, `key_ingredients_image`, `key_ingredients_image_responsive`, `nutritional_facts_files`

### Image behavior

| Media position | Used for |
|---|---|
| 1st image | Featured image on shop cards and cart — **excluded from PDP carousel** |
| 2nd onward | PDP Splide carousel and thumbnails |

The carousel uses `offset: 1` in `sections/pdp-i.liquid`. Upload at least two images per product.

### Seal Subscriptions

The app injects DOM (`.sealsubs-container`) into the buy area. Theme CSS in `assets/home.css` and inline JS in `pdp-i.liquid` style and reposition the widget. Test PDP after Seal app updates.

**Guide:** [`handoff/08-product-management-guide.md`](handoff/08-product-management-guide.md)

---

## 6. Store Locator Architecture

**Template:** `templates/page.locator.json`  
**Expected URL:** `/pages/store-locator`

| Section | File | Role |
|---|---|---|
| Locator I | `sections/locator-i.liquid` | Page heading + **Stockist** map embed |
| Locator II | `sections/locator-ii.liquid` | "Don't see us near you?" contact form |

### Stockist integration

- Widget element: `<stockist-store-locator>`
- Widget tag: `map_r3mn7jvq` (setting: `stockist_widget_tag`)
- Script: `https://stockist.co/embed/v1/widget.min.js`
- Store locations are managed in the **Stockist dashboard** — not in Shopify theme code, metafields, or JSON.

Locator II uses Shopify's native contact form with category `Store locator inquiry`. Form success handling uses `sessionStorage` in `assets/wagon.js`.

**Guide:** [`handoff/07-store-locator-guide.md`](handoff/07-store-locator-guide.md)

---

## 7. Third-Party Services

| Service | Purpose | Where in theme |
|---|---|---|
| **Stockist** | Retail store locator map | `sections/locator-i.liquid` |
| **Seal Subscriptions** | Subscribe & save on PDP | DOM/CSS in `pdp-i.liquid`, `home.css`, `master.css` |
| **UpPromote Affiliate** | Affiliate referral tracking | App block in `config/settings_data.json` |
| **Adobe Typekit** | Obviously / Obviously Narrow fonts | `layout/theme.liquid` — kit `rji3kyn` |
| **Shopify Contact Forms** | Contact, affiliate, locator, newsletter forms | Custom section Liquid files |
| **Shop Pay** | Accelerated checkout on PDP | `blocks/accelerated-checkout.liquid` |

### Not detected in theme code

Klaviyo, Mailchimp, Google Analytics, GTM, Meta Pixel, TikTok Pixel, and Eventbrite were not found in theme files. Additional scripts may be injected via Shopify Admin (`content_for_header`, Customer events, App embeds). Confirm in production Admin.

**Guide:** [`handoff/05-third-party-services.md`](handoff/05-third-party-services.md)

---

## 8. Handoff Documentation

All client and developer documentation is in [`handoff/`](handoff/).

| Document | Audience | Contents |
|---|---|---|
| [`JUSTNOSH-FINAL-HANDOFF-DOCUMENTATION.md`](handoff/JUSTNOSH-FINAL-HANDOFF-DOCUMENTATION.md) | Client / stakeholders | Consolidated handoff summary |
| [`01-client-handoff-index.md`](handoff/01-client-handoff-index.md) | All | Master index and quick reference |
| [`02-cms-editing-guide.md`](handoff/02-cms-editing-guide.md) | Content editors | Theme Editor and CMS workflows |
| [`03-homepage-management-guide.md`](handoff/03-homepage-management-guide.md) | Content editors | Homepage sections and images |
| [`03-theme-design-guide.md`](handoff/03-theme-design-guide.md) | Design / dev | Colors, typography, design system |
| [`04-technical-documentation.md`](handoff/04-technical-documentation.md) | Developers | Architecture, files, animations, forms |
| [`05-third-party-services.md`](handoff/05-third-party-services.md) | All | Apps, CDN, ownership checklist |
| [`06-assets-inventory.md`](handoff/06-assets-inventory.md) | Design / dev | Logos, icons, fonts, file paths |
| [`07-store-locator-guide.md`](handoff/07-store-locator-guide.md) | Content / ops | Stockist setup and updates |
| [`08-product-management-guide.md`](handoff/08-product-management-guide.md) | Catalog managers | Products, metafields, PDP images |
| [`10-about-page-guide.md`](handoff/10-about-page-guide.md) | Content editors | About page structure, content, and Minishop |
| [`11-contact-page-guide.md`](handoff/11-contact-page-guide.md) | Content editors | Contact forms and newsletter section |
| [`12-affiliate-page-guide.md`](handoff/12-affiliate-page-guide.md) | Content / ops | Affiliate page and application form |
| [`13-store-locator-page-guide.md`](handoff/13-store-locator-page-guide.md) | Content / ops | Store Locator page audit and maintenance |

### Page-Specific Documentation

Dedicated guides for each customer-facing content page. Each file documents section order, Theme Editor settings, images, forms, dependencies, hardcoded content, and what the client can safely update versus what requires a developer.

**Where to find maintenance instructions:** Open the guide for the page you are editing in [`handoff/`](handoff/). Use the **Maintenance Notes** section for client-safe updates and the **Developer Notes** section for technical constraints. General Theme Editor workflows are in [`02-cms-editing-guide.md`](handoff/02-cms-editing-guide.md).

| Page | Guide | Summary |
|---|---|---|
| **About Us** | [`10-about-page-guide.md`](handoff/10-about-page-guide.md) | Five story sections (About I–V) plus a **Minishop** product showcase. Brand narrative, values, collagen positioning, and founder story are editable in the Theme Editor. Minishop displays three random products from a collection. |
| **Contact** | [`11-contact-page-guide.md`](handoff/11-contact-page-guide.md) | **Contact I** general inquiry form and **Contact II** newsletter signup with decorative images. Both forms use Shopify's contact API; submissions go to Shopify Notifications. |
| **Affiliate** | [`12-affiliate-page-guide.md`](handoff/12-affiliate-page-guide.md) | Five sections from program overview through application form (**Affiliate V**). Affiliate tracking runs via **UpPromote** (global app embed). Application submissions use Shopify contact forms. |
| **Store Locator** | [`13-store-locator-page-guide.md`](handoff/13-store-locator-page-guide.md) | **Locator I** embeds the **Stockist** map (`map_r3mn7jvq`). **Locator II** provides a store-request contact form. Retail locations are managed in the Stockist dashboard, not in Shopify. |

---

## 9. Development Notes

### Local setup

1. Install [Shopify CLI](https://shopify.dev/docs/api/shopify-cli)
2. Clone this repository
3. Create `shopify.theme.toml` locally (gitignored) with store credentials
4. Run `shopify theme dev` to preview against a development theme

### WAGON conventions

| Pattern | Usage |
|---|---|
| `.he-*` | Heading typography (Obviously Narrow) |
| `.bo-*` | Body typography (Obviously) |
| `.sp-*` | UI text / prices |
| `.btn-i` | Primary button (+ `.btn-pink-i`, `.btn-pink-ii`, `.outline-i`) |
| `.container-i` | Content width containers |
| `data-anim` | GSAP scroll animation triggers in `assets/wagon.js` |

### JavaScript API

`assets/wagon.js` exposes a public API:

```javascript
window.WagonTheme.init()     // Full initialization
window.WagonTheme.reinit()   // Destroy and re-init animations
window.WagonTheme.refresh()  // ScrollTrigger.refresh()
window.WagonTheme.getLenis() // Lenis instance (desktop only)
```

Theme Editor events (`shopify:section:load`, etc.) re-initialize animations automatically.

### Sensitive files — edit with care

| File | Risk |
|---|---|
| `assets/base.css` | Heritage core — global impact |
| `snippets/scripts.liquid` | Full importmap and module loading |
| `snippets/header-actions.liquid` | Cart drawer custom markup and styles |
| `layout/theme.liquid` | Global asset load order |

### Known fragile areas

- PDP media `offset: 1` — first gallery image excluded from carousel by design
- Seal Subscriptions DOM coupling in `pdp-i.liquid` — breaks if app changes markup
- Dual Splide init — generic (`wagon.js`) and PDP-specific (inline in `pdp-i.liquid`)
- Header height inline script in `theme.liquid` duplicates `utilities.js` logic — keep in sync

---

## 10. Deployment Notes

### Deploy to Shopify

```bash
# Push to a development theme
shopify theme push --development

# Push to the live theme (use with caution)
shopify theme push --live
```

Alternatively: zip the theme folder and upload via **Shopify Admin → Online Store → Themes → Add theme → Upload zip**.

### Pre-deploy checklist

- [ ] Homepage hero animates correctly
- [ ] PDP carousel, variant selection, and add-to-cart work
- [ ] Seal Subscriptions widget displays and functions
- [ ] Cart drawer opens after add-to-cart
- [ ] Contact, affiliate, and locator forms submit and show success state
- [ ] Stockist map loads on Store Locator page
- [ ] Mobile layout and animations (touch fallback) work

### What is not deployed with the theme

- Product data, orders, and customers (live in Shopify Admin)
- Images in **Content → Files** (`shop_images`)
- Stockist location data
- App configurations (Seal, UpPromote, Stockist dashboards)

Download a theme backup from **Online Store → Themes → Download theme file** before major deployments.

---

## 11. Maintenance Notes

### Heritage theme upgrades

Heritage is currently at **3.5.1** (`config/settings_schema.json`). When upgrading:

1. Compare `layout/theme.liquid` — WAGON block must be preserved
2. Review `snippets/header-actions.liquid` for cart drawer conflicts
3. Do not overwrite custom sections (`superhero`, `pdp-*`, `about-*`, etc.)
4. Diff `snippets/scripts.liquid` and `assets/base.css` against the new Heritage release

### CDN version pins

External libraries are version-pinned in `layout/theme.liquid`. Updating CDN URLs requires visual regression testing on animations, carousels, and PDP tabs.

### App maintenance

| App | Maintenance action |
|---|---|
| **Stockist** | Manage locations in Stockist dashboard; widget tag in Locator I if account changes |
| **Seal Subscriptions** | Test PDP layout after app updates |
| **UpPromote** | Confirm app block remains enabled in theme settings after theme publish |

### Adobe Typekit

Kit `rji3kyn` must remain active. If the kit expires, Obviously fonts fail on custom pages.

### Ownership and access

Account ownership for Shopify, Stockist, Seal Subscriptions, UpPromote, Adobe Typekit, domain/DNS, and analytics is documented in [`handoff/05-third-party-services.md`](handoff/05-third-party-services.md). Credentials are not stored in this repository.

---

## Development Credits

This project was delivered by Wagon Studio for Just Nosh.

The storefront is built on top of the Heritage Shopify theme and includes custom Shopify development, Liquid architecture, frontend implementation, custom sections, styling, integrations, animations, and supporting documentation developed specifically for this project.

**Project ownership and delivery:** [github.com/wagonstudio](https://github.com/wagonstudio)

**Technical implementation and development collaboration:** [github.com/codevamon](https://github.com/codevamon)

This repository serves as the source code and documentation archive for ongoing maintenance and future enhancements.

---

## License and attribution

- **Heritage** theme by Shopify — [theme documentation](https://help.shopify.com/manual/online-store/themes)
- **WAGON** custom development layer — proprietary to the Just Nosh / JAG Provisions project

---

*Last updated: June 2026*
