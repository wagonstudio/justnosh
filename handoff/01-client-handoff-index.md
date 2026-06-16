# Just Nosh Shopify Website Handoff Documentation

**Website:** [eatjustnosh.com](https://eatjustnosh.com)  
**Platform:** Shopify  
**Theme base:** Heritage 3.5.1 (Shopify), with custom Just Nosh modifications  
**Prepared for:** JAG Provisions / Just Nosh  
**Documentation package date:** June 2026

---

This document is the master index for the Just Nosh website handoff. It explains what the site includes, where key content lives, and which companion guides to use for day-to-day management.

For a full technical audit of the theme codebase, see `00-theme-audit.md`.

---

## 1. Overview

### What this site is

Just Nosh is a Shopify storefront for collagen protein bars. The site was built on Shopify’s **Heritage** theme and extended with custom pages, product layouts, animations, and integrations for subscriptions, affiliates, and store locator functionality.

### What you can manage without a developer

Most day-to-day updates can be done in **Shopify Admin** and the **Theme Editor**:

- Homepage text, images, and product highlights
- About, Contact, Affiliate, and Store Locator page content
- Products, prices, images, and product metafields
- Navigation menus and footer links
- Cart behavior (drawer opens automatically after add-to-cart)

### What typically requires a developer

- Changes to custom code (`assets/wagon.js`, `assets/master.css`, custom sections)
- New page layouts or animation behavior
- Updates to third-party app embeds (Stockist, Seal Subscriptions, UpPromote)
- Theme upgrades from the base Heritage theme

### Key pages

| Page | URL handle (expected) | Template file |
|---|---|---|
| Homepage | `/` | `templates/index.json` |
| Shop / Collection | `/collections/...` | `templates/collection.json` |
| Product (PDP) | `/products/...` | `templates/product.json` |
| About | `/pages/about` | `templates/page.about.json` |
| Contact | `/pages/contact` | `templates/page.contact.json` |
| Affiliate | `/pages/affiliate` | `templates/page.affiliate.json` |
| Store Locator | `/pages/store-locator` | `templates/page.locator.json` |
| Cart | `/cart` | `templates/cart.json` |

> Page URL handles should be confirmed in **Shopify Admin → Online Store → Pages**.

### Companion documentation

| Document | Purpose |
|---|---|
| `00-theme-audit.md` | Full technical theme audit |
| `02-cms-editing-guide.md` | How to edit pages and sections in the Theme Editor |
| `03-theme-design-guide.md` | Brand colors, typography, and design system |
| `04-technical-documentation.md` | Technical architecture and file reference |
| `05-third-party-services.md` | Apps and external services |
| `06-assets-inventory.md` | Images, icons, and file inventory |
| `07-store-locator-guide.md` | Stockist store locator setup and updates |
| `08-product-management-guide.md` | Products, metafields, and PDP content |
| `09-known-limitations-and-notes.md` | Open items and known constraints |

---

## 2. Content & CMS

### Where content is managed

Content is managed in two places:

1. **Shopify Admin → Online Store → Themes → Customize** (Theme Editor)  
   Used for section-based pages: homepage, about, contact, affiliate, store locator, header, and footer.

2. **Shopify Admin → Content**  
   Used for pages, menus, files (images), and blog posts (if used).

### Page-by-page content map

| Page | Sections (Theme Editor) | Section files |
|---|---|---|
| **Homepage** | Superhero, Why, Shopping, Our Mission | `sections/superhero.liquid`, `sections/why.liquid`, `sections/shopping.liquid`, `sections/ourmission.liquid` |
| **About** | About I–V, Minishop | `sections/about-i.liquid` through `sections/about-v.liquid`, `sections/minishop.liquid` |
| **Contact** | Contact I, Contact II | `sections/contact-i.liquid`, `sections/contact-ii.liquid` |
| **Affiliate** | Affiliate I–V | `sections/affiliate-i.liquid` through `sections/affiliate-v.liquid` |
| **Store Locator** | Locator I, Locator II | `sections/locator-i.liquid`, `sections/locator-ii.liquid` |
| **Header** | Header (Heritage) | `sections/header.liquid` via `sections/header-group.json` |
| **Footer** | Footer (custom) | `sections/foot.liquid` via `sections/footer-group.json` |

### Forms and where submissions go

All site forms use Shopify’s built-in **contact form** (`{% form 'contact' %}`). Submissions are delivered via **Shopify Notifications** — not through a separate email marketing tool in the theme code.

| Form | Location | Purpose |
|---|---|---|
| General contact | Contact page → Contact I | Customer inquiries |
| Newsletter signup | Contact page → Contact II | Email list signups |
| Store request | Store Locator → Locator II | Retailer / location requests |
| Affiliate application | Affiliate page → Affiliate V | Partner applications |

**Notification routing:** To be provided by JAG Provisions / Wagon (Shopify Admin → Settings → Notifications).

> The newsletter form on the Contact page is a Shopify contact form. Any connection to Klaviyo, Mailchimp, or similar tools is **not visible in theme code** and must be confirmed separately.

### Navigation menus

The footer uses three menus configured in the Theme Editor:

- **Shop** (`shop`)
- **Learn** (`learn`)
- **Support** (`support`)

The header uses **Main menu** (`main-menu`).

Menu structure and links: To be provided by JAG Provisions / Wagon (Shopify Admin → Online Store → Navigation).

**Detailed editing instructions:** See `02-cms-editing-guide.md`.

---

## 3. Product Management

### How products appear on the site

Products are standard Shopify products. Custom product **metafields** control how each flavor looks and reads on product pages, shop grids, and the cart.

### Product metafields (namespace: `custom`)

These metafields drive the custom product experience. They should be filled in for every active product.

| Metafield | Used for |
|---|---|
| `bars` | Badge text (e.g. “12 BARS”) on shop cards and cart |
| `shopping_media` | Hover/decor images on shop grids |
| `pdp_intro` | Short intro text on product page |
| `flavor_profile_title` | Flavor profile label on PDP |
| `flavor_profile_text` | Flavor profile description on PDP |
| `price_suffix_label` | Price suffix (e.g. per box) on PDP |
| `pdp_highlights` | Benefit chips on PDP |
| `color_primary`, `color_secondary`, `color_tertiary`, `color_tertiary_contrast`, `color_light` | Per-flavor color theming on PDP |
| `key_ingredients` | Ingredient list on PDP II |
| `key_ingredients_image` | Desktop ingredients image on PDP II |
| `key_ingredients_image_responsive` | Mobile ingredients image on PDP II |
| `nutritional_facts_files` | Nutrition fact images in PDP III tabs |

### Shop grids

Products appear in custom shop sections on:

- Homepage (`sections/shopping.liquid`)
- Collection page (`sections/shop.liquid`)
- Cart page (`templates/cart.json`)
- About page minishop (`sections/minishop.liquid`)
- PDP recommendations (`sections/pdp-recommendations.liquid`)

If no collection is selected in a section, the theme falls back to the **frontpage** collection. Confirm this collection is assigned correctly in Shopify Admin.

### Subscriptions

The product page integrates with **Seal Subscriptions** (app). Subscription options appear within the buy area on PDP I. Styling is handled in theme CSS; functionality is managed in the Seal Subscriptions app.

**Detailed product instructions:** See `08-product-management-guide.md`.

---

## 4. PDP Images

### Product page structure

Each product page (`templates/product.json`) uses four custom sections:

| Section | File | Image / media source |
|---|---|---|
| PDP I — Main product area | `sections/pdp-i.liquid` | Shopify product media gallery (Splide carousel) |
| PDP II — Key ingredients | `sections/pdp-ii.liquid` | Metafields `key_ingredients_image`, `key_ingredients_image_responsive` |
| PDP III — Tabs | `sections/pdp-iii.liquid` | Metafield `nutritional_facts_files` (nutrition images) |
| PDP Recommendations | `sections/pdp-recommendations.liquid` | Other products’ featured images + `shopping_media` metafield |

### Where to update PDP images

| Image type | Where to update |
|---|---|
| Main product photos (carousel) | Shopify Admin → Products → [Product] → Media |
| Ingredients image (desktop) | Product metafield `key_ingredients_image` |
| Ingredients image (mobile) | Product metafield `key_ingredients_image_responsive` |
| Nutrition facts | Product metafield `nutritional_facts_files` |
| Per-flavor colors | Product metafields `color_primary`, `color_secondary`, etc. |

### Important note

The default Heritage **Product information** section is **disabled** on the product template. The live buy experience is entirely within **PDP I**. Do not re-enable the default section without developer review — it would duplicate the product layout.

**Detailed PDP instructions:** See `08-product-management-guide.md`.

---

## 5. Homepage Images, Banners & Carousels

### Homepage sections

The homepage (`templates/index.json`) has four sections in this order:

1. **Superhero** — hero headline, CTA, bar images, decorative ingredient graphics  
   - File: `sections/superhero.liquid`  
   - Images: configured in Theme Editor (bars, chocolate chips, peanuts, lemon SVGs/PNGs)

2. **Why** — four benefit icons with titles and descriptions  
   - File: `sections/why.liquid`  
   - Images: icon uploads per block (e.g. muscle, digestion, chemistry, oil icons)

3. **Shopping** — product flavor grid  
   - File: `sections/shopping.liquid`  
   - Images: pulled from product featured images and `shopping_media` metafield

4. **Our Mission** — mission statement with decorative background elements  
   - File: `sections/ourmission.liquid`  
   - Images: decorative detail images in section settings

### Banners

There is **no active announcement bar** in the current header configuration (`sections/header-group.json`). Promotional messaging lives within page sections (e.g. Superhero), not as a separate site-wide banner.

### Carousels

| Carousel | Location | Technology |
|---|---|---|
| PDP image gallery | Product pages → PDP I | Splide (custom init in `sections/pdp-i.liquid`) |
| Generic carousels | Any section using `[data-splide]` | Splide via `assets/wagon.js` |

Homepage sections do not currently use a carousel — the Shopping section is a static product grid.

### Animations on homepage

The homepage uses entrance animations on the Superhero section (CSS-driven via `.hero-ready` class) and optional scroll animations on other sections. Animation behavior is controlled in `assets/wagon.js` and `assets/home.css`.

**Image inventory:** See `06-assets-inventory.md`.  
**Editing instructions:** See `02-cms-editing-guide.md`.

---

## 6. Store Locator

### How it works

The Store Locator page uses two sections:

| Section | File | Function |
|---|---|---|
| Locator I | `sections/locator-i.liquid` | Interactive map via **Stockist** embed |
| Locator II | `sections/locator-ii.liquid` | “Don’t see us near you?” contact form |

### Stockist integration

- Widget tag configured: `map_r3mn7jvq`
- Script loaded from: `https://stockist.co/embed/v1/widget.min.js`
- Store locations are managed in the **Stockist** dashboard — not in Shopify theme code

### Store request form

Locator II sends submissions through Shopify’s contact form with category `Store locator inquiry`.

**Stockist account access:** To be provided by JAG Provisions / Wagon.  
**Detailed locator guide:** See `07-store-locator-guide.md`.

---

## 7. Theme & Design

### Design system

The custom design system (“WAGON”) is defined in:

| File | Contents |
|---|---|
| `assets/master.css` | Colors, typography scale, buttons, layout utilities |
| `assets/wagon.css` | Section-specific styles (header, footer, pages) |
| `assets/home.css` | Homepage and PDP-specific styles |

### Brand colors (CSS variables in `assets/master.css`)

| Variable | Color |
|---|---|
| `--color-azul-oscuro` | `#1F2B3A` |
| `--color-naranja` | `#F65B18` |
| `--color-rosa` | `#FEAA8F` |
| `--color-blanco` | `#F9F9F7` |

Product pages can override colors per flavor via product metafields.

### Typography

| Font | Source | Usage |
|---|---|---|
| Obviously / Obviously Narrow | Adobe Typekit (kit `rji3kyn`) | Headlines, buttons, cart |
| Work Sans | Google Fonts | Supporting text |
| Instrument Sans | Shopify font (Heritage settings) | Base theme typography settings |

**Typekit account access:** To be provided by JAG Provisions / Wagon.

### Style guide page

An internal style guide exists at `templates/page.master.json` using `sections/master.liquid`. This page shows all typography, buttons, and components. Confirm whether this page is published in Shopify Admin.

**Detailed design guide:** See `03-theme-design-guide.md`.

---

## 8. Assets

### Theme asset files

Custom assets live in the theme’s `assets/` folder:

| File | Type | Purpose |
|---|---|---|
| `assets/master.css` | CSS | Global design system |
| `assets/wagon.css` | CSS | Page and section styles |
| `assets/home.css` | CSS | Homepage and PDP styles |
| `assets/wagon.js` | JavaScript | Animations, carousels, smooth scroll, forms |
| `assets/splitting.css` | CSS | Text animation library styles |
| `assets/splitting-cells.css` | CSS | Text animation library styles |

### Shopify-hosted files

Brand images, icons, product photography, and decorative graphics are stored in **Shopify Admin → Content → Files** (`shopify://shop_images/...`). These are referenced in section settings and templates.

Examples referenced in the theme:

- Logo: `Justnosh-Logo.svg`
- Favicon: `fav.png`
- Benefit icons: `muscle.svg`, `digestion.svg`, `chemistry.svg`, `oil.svg`
- Decorative SVGs: chips, chocolate, peanuts, lemon

### External assets (loaded from CDN)

These are loaded on every page via `layout/theme.liquid`:

- Bootstrap 5.3.3
- jQuery 3.6.0
- GSAP 3.12.5 + ScrollTrigger
- Lenis (smooth scroll)
- Splide 4.1.4
- Splitting 1.0.6
- Adobe Typekit
- Google Fonts (Work Sans)

**Full asset inventory:** See `06-assets-inventory.md`.

---

## 9. Custom Code & Theme Modifications

### Summary of custom development

The Heritage theme was extended with a custom layer referred to as **WAGON** in the codebase. Key modification points:

| Area | File(s) | What was changed |
|---|---|---|
| Global layout | `layout/theme.liquid` | Loads WAGON CSS/JS stack and external libraries |
| Custom sections | `sections/superhero.liquid`, `why.liquid`, `shopping.liquid`, etc. | 25+ custom page sections |
| Product page | `sections/pdp-i.liquid`, `pdp-ii.liquid`, `pdp-iii.liquid` | Full custom PDP with metafield-driven content |
| Footer | `sections/foot.liquid` | Replaces default Heritage footer |
| Header drawer | `snippets/header-drawer.liquid` | Custom branding and privacy policy link |
| Cart drawer | `snippets/header-actions.liquid`, `snippets/cart-products.liquid` | Custom copy, typography, empty state CTA |
| Animation engine | `assets/wagon.js` | GSAP, Lenis, Splide, Splitting, form success handling |
| Design system | `assets/master.css` | Colors, typography, buttons |

### Custom sections inventory

**Homepage:** `superhero`, `why`, `shopping`, `ourmission`  
**PDP:** `pdp-i`, `pdp-ii`, `pdp-iii`, `pdp-recommendations`  
**About:** `about-i` through `about-v`, `minishop`  
**Affiliate:** `affiliate-i` through `affiliate-v`  
**Contact:** `contact-i`, `contact-ii`  
**Store Locator:** `locator-i`, `locator-ii`  
**Shop:** `shop`  
**Footer:** `foot`  
**Internal:** `master` (style guide)

### What was not changed

Standard Heritage functionality remains for:

- Cart drawer logic (`assets/cart-drawer.js`)
- Product form and variant picker (`assets/product-form.js`, `assets/variant-picker.js`)
- Search, predictive search, quick add
- Checkout (Shopify-hosted — not in theme)

**Full technical reference:** See `04-technical-documentation.md` and `00-theme-audit.md`.

---

## 10. Third-Party Services

### Services integrated in theme code

| Service | Purpose | Managed in |
|---|---|---|
| **Stockist** | Store locator map | Stockist dashboard + `sections/locator-i.liquid` |
| **Seal Subscriptions** | Subscribe & save on PDP | Seal Subscriptions app (Shopify) |
| **UpPromote Affiliate** | Affiliate tracking | UpPromote app (Shopify) |
| **Adobe Typekit** | Obviously font family | Adobe Fonts / Typekit account |
| **Google Fonts** | Work Sans | Google Fonts (no account needed) |

### CDN libraries (no account required)

Bootstrap, jQuery, GSAP, Lenis, Splide, Splitting — loaded from public CDNs in `layout/theme.liquid`.

### Services not found in theme code

The following may be installed via Shopify apps or `content_for_header` but are **not referenced in theme files**:

- Email marketing (Klaviyo, Mailchimp, etc.)
- Reviews (Yotpo, Judge.me, etc.)
- Analytics (Google Analytics, Meta Pixel, etc.)
- Customer support (Gorgias, etc.)

**App accounts and configuration:** To be provided by JAG Provisions / Wagon.

**Detailed service guide:** See `05-third-party-services.md`.

---

## 11. Ownership & Access

The following accounts and access points are required to fully manage the Just Nosh website. Credentials and invitations are **not included in this documentation**.

| System | Access needed for | Owner / provider |
|---|---|---|
| **Shopify Admin** | Products, orders, theme, content, settings | To be provided by JAG Provisions / Wagon |
| **Shopify Theme Editor** | Page content, section settings, menus | To be provided by JAG Provisions / Wagon |
| **Stockist** | Store locations, map widget | To be provided by JAG Provisions / Wagon |
| **Seal Subscriptions** | Subscription plans and pricing | To be provided by JAG Provisions / Wagon |
| **UpPromote Affiliate** | Affiliate program management | To be provided by JAG Provisions / Wagon |
| **Adobe Typekit** | Obviously font license | To be provided by JAG Provisions / Wagon |
| **Domain / DNS** | eatjustnosh.com | To be provided by JAG Provisions / Wagon |
| **Email notifications** | Form submission routing | To be provided by JAG Provisions / Wagon |

### Recommended access levels

| Role | Shopify permission level |
|---|---|
| Day-to-day content editor | Staff with “Online Store” and “Products” access |
| Product / catalog manager | Staff with “Products” and metafield access |
| Developer / agency | Theme code access (collaborator or staff with theme permissions) |

---

## 12. Final Theme Backup

A complete copy of the theme codebase is included in this handoff package (local theme files).

### What is included

- All Liquid templates, sections, snippets, and blocks
- Custom CSS and JavaScript (`assets/master.css`, `assets/wagon.css`, `assets/home.css`, `assets/wagon.js`)
- Theme configuration (`config/settings_schema.json`, `config/settings_data.json`)
- Template JSON files for all pages

### What is not included in code backup

- Shopify product data, orders, and customer records (live in Shopify Admin)
- Images uploaded to Shopify Files (referenced but hosted on Shopify CDN)
- App configurations (managed in each app’s dashboard)
- Stockist location data

### How to restore or duplicate the theme

1. In Shopify Admin → **Online Store → Themes**
2. **Add theme → Upload zip** (if providing a zip export), or
3. Connect via Shopify CLI / GitHub integration (if configured)

**Live theme export date and method:** To be provided by JAG Provisions / Wagon.

---

## 13. Video Instructions / Training Materials

Training materials for managing the Just Nosh site:

| Topic | Status |
|---|---|
| Shopify Admin overview | To be provided by JAG Provisions / Wagon |
| Theme Editor — editing homepage and pages | To be provided by JAG Provisions / Wagon |
| Product and metafield management | To be provided by JAG Provisions / Wagon |
| Store Locator updates (Stockist) | To be provided by JAG Provisions / Wagon |
| Form submission review | To be provided by JAG Provisions / Wagon |

Written guides in this package can be used immediately:

- `02-cms-editing-guide.md` — content editing
- `08-product-management-guide.md` — products and metafields
- `07-store-locator-guide.md` — store locator

---

## 14. Known Notes / Items Requiring Confirmation

The following items were identified during the theme audit and require confirmation from JAG Provisions / Wagon before or after handoff.

### Email addresses

Two different contact emails appear in theme templates:

| Email | Found in |
|---|---|
| `hello@jagprovisions.com` | Contact page, Store Locator |
| `hello@japrovisions.com` | Affiliate page |

**Action:** Confirm the correct operational email and update templates if needed.

### Newsletter integration

The Contact page newsletter form (Contact II) uses Shopify’s contact form. Any connection to an external email marketing platform is not visible in theme code.

**Action:** Confirm whether signups are routed to Klaviyo, Mailchimp, Shopify Email, or another tool.

### Collection assignment

Shop sections on the homepage, collection page, and cart have an empty collection setting — they fall back to the **frontpage** collection.

**Action:** Confirm `frontpage` collection is correctly assigned with all active products.

### Style guide page

`templates/page.master.json` exists as an internal component reference. Confirm whether this page is published and accessible.

### Heritage product-information section

The default Heritage product section is disabled on `templates/product.json`. The custom PDP I section handles all product display and purchase.

**Action:** Do not re-enable without developer review.

### Third-party apps not visible in code

Analytics, reviews, and additional marketing tools may be installed via Shopify apps. These inject code through `content_for_header` and are not auditable from theme files alone.

**Action:** Provide a complete list of installed Shopify apps.

### CDN dependency

The site loads nine external libraries on every page. If a CDN service is unavailable, animations and some UI components may not function.

**Action:** No immediate action required; note for future maintenance.

### Seal Subscriptions DOM integration

PDP I includes custom JavaScript that reorders the subscription widget relative to the quantity selector. Seal Subscriptions app updates could affect this layout.

**Action:** Test PDP after any Seal Subscriptions app update.

### UpPromote Affiliate

App block is registered in `config/settings_data.json`. Affiliate tracking depends on the app being installed and configured.

**Action:** Confirm UpPromote is active and affiliate program is configured.

### Page URL handles

Page handles (e.g. `/pages/store-locator`, `/pages/about`) should be verified in Shopify Admin.

**Action:** Confirm all page URLs match navigation links in header and footer menus.

---

**Full list of known limitations:** See `09-known-limitations-and-notes.md`.

---

*End of handoff index. For technical details, start with `00-theme-audit.md` and `04-technical-documentation.md`.*
