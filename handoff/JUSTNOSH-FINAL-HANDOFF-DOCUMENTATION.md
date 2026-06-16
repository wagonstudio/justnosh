# Just Nosh — Website Handoff Documentation

**Website:** [eatjustnosh.com](https://eatjustnosh.com)  
**Platform:** Shopify  
**Theme:** Heritage 3.5.1 with custom Just Nosh development  
**Prepared for:** JAG Provisions / Just Nosh  
**Package date:** June 2026

---

This document is the **final client handoff summary** for the Just Nosh website. It consolidates the detailed guides in the `handoff/` folder into a single, readable reference for day-to-day management.

**Companion guides (detailed reference):**

| Guide | File |
|---|---|
| CMS & page editing | `02-cms-editing-guide.md` |
| Homepage | `03-homepage-management-guide.md` |
| Design system | `03-theme-design-guide.md` |
| Technical architecture | `04-technical-documentation.md` |
| Third-party services | `05-third-party-services.md` |
| Asset inventory | `06-assets-inventory.md` |
| Store locator | `07-store-locator-guide.md` |
| Product management | `08-product-management-guide.md` |
| About page | `10-about-page-guide.md` |
| Contact page | `11-contact-page-guide.md` |
| Affiliate page | `12-affiliate-page-guide.md` |
| Store Locator page | `13-store-locator-page-guide.md` |
| Master index | `01-client-handoff-index.md` |

---

## 1. Executive Summary

Just Nosh is a Shopify storefront for collagen protein bars. The site was built on Shopify’s **Heritage** theme and extended with custom pages, product layouts, animations, and integrations for subscriptions, affiliates, and retail store discovery.

### What you can manage without a developer

| Task | Where |
|---|---|
| Homepage text, images, and product highlights | Theme Editor |
| About, Contact, Affiliate, and Store Locator content | Theme Editor |
| Products, prices, images, variants, and metafields | Shopify Admin → Products |
| Navigation menus and footer links | Theme Editor + Navigation |
| Store locations on the map | Stockist dashboard (external) |
| Subscription plans | Seal Subscriptions app |
| Affiliate program | UpPromote app |

### What typically requires a developer

- Custom code changes (`wagon.js`, `master.css`, custom sections)
- New page layouts or animation behavior
- Theme upgrades from Heritage base
- Form field label changes (hardcoded in theme)

### Key pages

| Page | Expected URL | How content is managed |
|---|---|---|
| Homepage | `/` | Theme Editor — 4 custom sections |
| Shop | `/collections/...` | Theme Editor + Products |
| Product (PDP) | `/products/...` | Products + metafields |
| About | `/pages/about` | Theme Editor |
| Contact | `/pages/contact` | Theme Editor |
| Affiliate | `/pages/affiliate` | Theme Editor |
| Store Locator | `/pages/store-locator` | Stockist + Theme Editor (form copy) |
| Cart | `/cart` | Products + Theme Editor |

> **Needs confirmation:** Verify all page URL handles in **Shopify Admin → Online Store → Pages**.

---

## 2. Content & CMS Instructions

### Where to edit content

| Place | Best for |
|---|---|
| **Online Store → Themes → Customize** | Headlines, section images, footer, header, shop grid headings |
| **Products** | Product names, prices, images, metafields |
| **Online Store → Pages** | Page title, URL handle, theme template assignment |
| **Online Store → Navigation** | Menu links (header + footer columns) |
| **Content → Files** | Brand images and SVGs shared across pages |

### Important rule

Most Just Nosh pages (About, Contact, Affiliate, Store Locator) are built from **custom sections** in the Theme Editor. The **page body field** in Shopify Pages is **not used** for the live design on those pages.

### How to open the Theme Editor

1. Shopify Admin → **Online Store → Themes**
2. On the live theme, click **Customize**
3. Use the page dropdown to select Home page, a specific page, or a product
4. Click content in the preview — the matching section highlights in the left sidebar
5. Click **Save** when finished

### Page templates

| Page | Theme template | Must be assigned in Pages settings |
|---|---|---|
| About | `about` | Yes |
| Contact | `contact` | Yes |
| Affiliate | `affiliate` | Yes |
| Store Locator | `locator` | Yes |

### Forms on the site

All forms use Shopify’s built-in **contact form**. Submissions arrive via **Shopify Notifications** (Settings → Notifications).

| Form | Page / section | Purpose |
|---|---|---|
| General contact | Contact → Contact I | Customer inquiries |
| Newsletter signup | Contact → Contact II | Email signups |
| Store request | Store Locator → Locator II | Retailer / location requests |
| Affiliate application | Affiliate → Affiliate V | Partner applications |

> **Needs confirmation:** Who receives form notification emails and whether signups connect to an external email tool (not visible in theme code).

### Navigation

| Menu | Used for |
|---|---|
| `main-menu` | Header navigation |
| `shop` | Footer — Shop column |
| `learn` | Footer — Learn column |
| `support` | Footer — Support column |

**Detailed instructions:** `02-cms-editing-guide.md`

---

## 3. Product Management

Products are standard Shopify products. Custom **metafields** (namespace `custom`) control how each flavor appears on product pages, shop grids, and the cart.

### Quick workflow

| Action | Steps |
|---|---|
| Add a product | Products → Add product → fill basics → complete metafields → assign collections → set Active |
| Edit a product | Products → [product] → edit fields/metafields → Save |
| Reorder on shop grids | Products → Collections → drag products → Save |
| Hide temporarily | Set status to **Draft** |
| Discontinue | **Archive product** (do not delete if orders exist) |

### Product metafields reference

| Metafield | Used for |
|---|---|
| `bars` | Badge text (e.g. “12 BARS”) on shop cards |
| `shopping_media` | Hover images on shop cards |
| `pdp_intro` | Short intro on product page |
| `flavor_profile_title` / `flavor_profile_text` | Flavor profile block on PDP |
| `price_suffix_label` | Price suffix (e.g. per box) |
| `pdp_highlights` | Benefit chips on PDP |
| `color_primary` … `color_light` | Per-flavor color theming on PDP |
| `key_ingredients` | Ingredient list (PDP II) |
| `key_ingredients_image` / `key_ingredients_image_responsive` | Ingredients images desktop/mobile |
| `nutritional_facts_files` | Nutrition fact images in PDP tabs |

### Shop grids — where products appear

| Location | Section | Collection setting |
|---|---|---|
| Homepage | Shopping | Theme Editor — falls back to `frontpage` if empty |
| Shop page | Shop | Same |
| About page | Minishop | Same |
| Cart page | Shop | Same |
| Product page | PDP Recommendations | Related products |

> **Needs confirmation:** Confirm which collection is assigned (or that `frontpage` contains all active products).

### Subscriptions

**Seal Subscriptions** app controls subscribe-and-save options on the product page. Configure plans in the app; the theme styles the widget on PDP.

**Detailed instructions:** `08-product-management-guide.md`

---

## 4. Product Images / PDPs

Each product page has four custom sections:

| Section | What it shows | Where to update |
|---|---|---|
| **PDP I** | Main gallery, buy area, flavor info | Products → Media + metafields |
| **PDP II** | Key ingredients | Metafields `key_ingredients`, `key_ingredients_image*` |
| **PDP III** | Nutrition / Collagen / Seed Oil tabs | Metafield `nutritional_facts_files` |
| **PDP Recommendations** | Other flavors | Automatic + product images |

### Critical image rule

| Media position | Used for |
|---|---|
| **1st image** | Shop cards, cart, recommendations — **not shown in PDP carousel** |
| **2nd image onward** | PDP image carousel and thumbnails |

Always upload at least **two images** per product: one for grids (position 1) and at least one for the PDP gallery (position 2+).

### Do not change without developer

The default Heritage **Product information** section is **disabled** on the product template. The live buy experience is entirely in **PDP I**. Re-enabling the default section would duplicate the layout.

**Detailed instructions:** `08-product-management-guide.md`

---

## 5. Homepage Banners, Hero Images & Carousels

### Homepage structure (4 sections, fixed order)

| # | Section | What it controls |
|---|---|---|
| 1 | **Superhero** | Hero headline, subtitle, CTA, bar images, ingredient graphics |
| 2 | **Why** | Four benefit icons with titles and descriptions |
| 3 | **Shopping** | Product flavor grid from a collection |
| 4 | **Our Mission** | Mission copy, CTA, decorative graphics |

### Banners

There is **no active announcement bar** on the site. Promotional messaging lives within page sections (e.g. the Superhero), not as a site-wide banner.

### Carousels

| Location | Carousel? |
|---|---|
| Homepage | **No** — Shopping is a static product grid |
| Product pages (PDP I) | **Yes** — image gallery carousel |

### Editing hero images

Hero images are updated in **Theme Editor → Superhero**. Each image slot (bars, chips, peanuts, lemon) is pre-positioned in the design. Swapping images works; **adding or removing slots requires a developer**.

### Shopping section note

Product badges come from the product metafield `custom.bars` — not from the section’s `badge_text` setting (that setting exists but is unused in the current theme).

**Detailed instructions:** `03-homepage-management-guide.md`

---

## 6. Store Locator Management

### How it works

| Component | Managed in |
|---|---|
| Interactive map | **Stockist** (external dashboard) |
| Page heading and widget settings | Theme Editor → **Locator I** |
| “Don’t see us near you?” form | Theme Editor → **Locator II** |

Store locations are **not** stored in Shopify, metafields, or theme code. They are managed entirely in **Stockist**.

### Stockist configuration (in theme)

| Setting | Current value |
|---|---|
| Widget tag | `map_r3mn7jvq` |
| Page template | `locator` |
| Expected URL | `/pages/store-locator` |

### To add or edit a store location

1. Log in to the **Stockist** dashboard
2. Add, edit, or remove locations there
3. Changes appear on the website automatically — no Theme Editor update needed

### Store request form

Locator II sends submissions through Shopify’s contact form (category: Store locator inquiry). Email reference on page: `hello@jagprovisions.com`.

> **Needs confirmation:** Stockist account login and ownership.

**Detailed instructions:** `07-store-locator-guide.md`

---

## 7. Theme & Design

The site uses a custom design system (“WAGON”) layered on Heritage. Colors and typography below reflect **implemented site styles** from the theme — not a separate official brand guidelines PDF.

### Primary colors

| Color | Hex | Typical use |
|---|---|---|
| Dark navy | `#1F2B3A` | Header, hero, footer backgrounds |
| Off-white | `#F9F9F7` | Page backgrounds |
| Salmon / pink CTA | `#FEAA8F` | Primary buttons |
| Orange accent | `#F65B18` | Accent elements |

Product pages can override colors per flavor via product metafields.

### Typography

| Font | Source | Used for |
|---|---|---|
| Obviously / Obviously Narrow | Adobe Typekit (kit `rji3kyn`) | Headlines, buttons, custom pages |
| Instrument Sans | Shopify Fonts | Heritage theme areas |
| Work Sans | Google Fonts | Loaded globally; limited use detected |

> **Needs confirmation:** Adobe Typekit account ownership (kit `rji3kyn`).

### What to edit where

| Change | Edit in |
|---|---|
| Logo, favicon | Theme settings or Theme Editor → Header / Foot |
| Section images and copy | Theme Editor |
| Product PDP colors | Products → Metafields |
| Global colors, fonts, button styles | Theme code — requires developer |

**Detailed reference:** `03-theme-design-guide.md`

---

## 8. Asset Package

### Included in theme code backup

- All Liquid templates, sections, snippets, and blocks
- Custom CSS and JavaScript (`master.css`, `wagon.css`, `home.css`, `wagon.js`)
- Theme configuration files

### Hosted in Shopify (not in code zip)

| Asset type | Where |
|---|---|
| Brand logos, favicon | Content → Files (`shop_images`) |
| Product photography | Products → Media |
| Decorative SVGs (chips, icons) | Content → Files |
| Store locator data | Stockist dashboard |

### Key brand files referenced in theme

| File | Use |
|---|---|
| `Justnosh-Logo.svg` | Header logo |
| `Justnosh-Logo_47daa248-....svg` | Footer logo |
| `fav.png` | Favicon |
| Benefit icons (`muscle.svg`, `digestion.svg`, etc.) | Why / About sections |

> **Needs confirmation:** Export and backup of all files in **Content → Files**.

**Full inventory:** `06-assets-inventory.md`

---

## 9. Technical Documentation

For developers and technical staff, the following summarizes the site architecture. Non-technical users can skip this section.

### Architecture

```
Shopify Heritage 3.5.1 (base)
  + WAGON custom layer (sections, CSS, JS, animations)
  + External CDN libraries (Bootstrap, GSAP, Splide, etc.)
  + Third-party embeds (Stockist, Seal, UpPromote)
```

### Key technical files

| File | Role |
|---|---|
| `layout/theme.liquid` | Global layout — loads all CSS/JS |
| `assets/wagon.js` | Animations, smooth scroll, carousels, form success |
| `assets/master.css` | Design system (colors, typography, buttons) |
| `templates/product.json` | Product page structure |
| `templates/index.json` | Homepage structure |
| `config/settings_data.json` | Live theme settings |

### Cart behavior

- Cart type: **drawer** (slides in from side)
- Auto-opens after add-to-cart: **enabled**

**Full reference:** `04-technical-documentation.md`

---

## 10. Custom Code & Theme Modifications

### What was built custom

| Area | Summary |
|---|---|
| Homepage | 4 custom sections (Superhero, Why, Shopping, Our Mission) |
| Product pages | Full custom PDP (PDP I–III + recommendations) |
| About, Contact, Affiliate | Multi-section custom page layouts |
| Store Locator | Stockist embed + contact form |
| Footer | Custom footer (`Foot`) replacing Heritage default |
| Cart drawer | Custom typography and copy |
| Animations | GSAP scroll animations, hero entrance, smooth scroll |

### Custom section count

25+ custom sections across homepage, PDP, About (5), Affiliate (5), Contact (2), Locator (2), Shop, Footer, and internal style guide.

### What was not replaced

Standard Shopify checkout, cart logic, product form, variant picker, and search infrastructure remain from Heritage.

**Full reference:** `04-technical-documentation.md`

---

## 11. Third-Party Services

### Services confirmed in theme

| Service | Purpose | Status |
|---|---|---|
| **Shopify** | Platform, checkout, forms | Active |
| **Stockist** | Store locator map | Active in theme — confirm account |
| **Seal Subscriptions** | Subscribe & save on PDP | Detected in theme code — confirm app in Admin |
| **UpPromote Affiliate** | Affiliate tracking | Active in theme settings — confirm app in Admin |
| **Adobe Typekit** | Obviously fonts | Active in theme — confirm account |
| **Shopify Contact Forms** | All site forms | Active (native) |

### Not detected in theme code

The following may exist via Shopify Admin but were **not found in theme files**:

| Service | Status |
|---|---|
| Klaviyo / Mailchimp | Not detected |
| Google Analytics / GTM | Not detected |
| Meta / TikTok pixels | Not detected |
| Eventbrite | Not detected |
| Google Maps | Not detected (Stockist used instead) |

> **Needs confirmation:** Review **Settings → Customer events**, **Apps**, and **Theme → App embeds** in Shopify Admin for the complete list.

### CDN libraries (every page)

Bootstrap, jQuery, GSAP, Lenis, Splide, Splitting — loaded from public CDNs. No account required; site depends on these services being available.

**Full reference:** `05-third-party-services.md`

---

## 12. Ownership & Access

Credentials are **not included** in this documentation. The following access points are required to fully manage the site.

| System | Needed for | Owner |
|---|---|---|
| **Shopify Admin** | Products, orders, theme, content | **Needs confirmation** |
| **Theme Editor** | Page content, sections | **Needs confirmation** |
| **Stockist** | Store locations, map | **Needs confirmation** |
| **Seal Subscriptions** | Subscription plans | **Needs confirmation** |
| **UpPromote Affiliate** | Affiliate program | **Needs confirmation** |
| **Adobe Typekit** | Obviously font license | **Needs confirmation** |
| **Domain / DNS** | eatjustnosh.com | **Needs confirmation** |
| **GitHub repository** | Theme code version control | To be provided by JAG Provisions / Wagon |
| **Email notifications** | Form routing | **Needs confirmation** |

### Recommended Shopify staff roles

| Role | Permissions |
|---|---|
| Content editor | Online Store, Products (view/edit) |
| Catalog manager | Products, metafields, collections |
| Developer / agency | Theme code, apps, settings |

### Ownership confirmation checklist

- [ ] Shopify store owner and staff accounts documented
- [ ] Domain registrar and DNS access transferred or shared
- [ ] All installed Shopify apps listed with login credentials
- [ ] Stockist, Seal, UpPromote, and Typekit accounts confirmed
- [ ] Analytics / pixels reviewed in Customer events
- [ ] Form notification recipients confirmed
- [ ] GitHub repo access documented
- [ ] Theme backup downloaded from Shopify Admin
- [ ] Shopify Files (brand images) exported

**Full checklist:** `05-third-party-services.md` → Ownership confirmation required

---

## 13. Final Theme Backup

### What this handoff package includes

| Included | Not included |
|---|---|
| Theme code (Liquid, CSS, JS, JSON templates) | Live product data, orders, customers |
| `config/settings_data.json` (theme settings snapshot) | Images in Shopify Files (hosted on Shopify CDN) |
| Documentation in `handoff/` folder | App configurations (managed per app) |
| | Stockist location data |

### How to download a fresh backup from Shopify

1. Shopify Admin → **Online Store → Themes**
2. On the live theme → **⋯** → **Download theme file**
3. Save with date stamp (e.g. `justnosh-theme-2026-06.zip`)

### How to restore or duplicate

1. **Online Store → Themes → Add theme → Upload zip**, or
2. Connect via Shopify CLI / GitHub (if configured)

> **Needs confirmation:** Date and method of the last live theme export provided by JAG Provisions / Wagon.

---

## 14. Recommended Training Videos

Written guides in this package are ready to use immediately. Video training should cover the topics below.

| Topic | Suggested content | Status |
|---|---|---|
| Shopify Admin overview | Navigation, orders, products | To be provided by JAG Provisions / Wagon |
| Theme Editor basics | Editing homepage and page sections | To be provided by JAG Provisions / Wagon |
| Product & metafield management | Adding flavors, images, metafields | To be provided by JAG Provisions / Wagon |
| Store Locator (Stockist) | Adding/editing retail locations | To be provided by JAG Provisions / Wagon |
| Form submissions | Where notifications arrive, how to respond | To be provided by JAG Provisions / Wagon |
| Seal Subscriptions | Creating and editing subscription plans | To be provided by JAG Provisions / Wagon |

### Shopify official resources (free)

| Resource | URL |
|---|---|
| Shopify Help Center | [help.shopify.com](https://help.shopify.com) |
| Theme Editor guide | [help.shopify.com — Customize theme](https://help.shopify.com/en/manual/online-store/themes/customizing-themes/edit) |
| Product management | [help.shopify.com — Products](https://help.shopify.com/en/manual/products) |
| Metafields | [help.shopify.com — Metafields](https://help.shopify.com/en/manual/custom-data/metafields) |

---

## 15. Open Items / Needs Confirmation

The following items were identified during the theme audit and require confirmation from JAG Provisions / Wagon before or after handoff.

### High priority

| Item | Detail | Action |
|---|---|---|
| **Contact emails** | `hello@jagprovisions.com` (Contact, Locator) vs `hello@japrovisions.com` (Affiliate) vs `info@jagprovisions.com` (Contact default) | Confirm correct operational email(s) |
| **Shopify app inventory** | Apps may inject code not visible in theme files | List all apps in Admin → Settings → Apps |
| **Analytics & pixels** | GA, GTM, Meta, TikTok not in theme code | Review Settings → Customer events |
| **Account ownership** | Shopify, Stockist, Seal, UpPromote, Typekit, domain | Complete ownership checklist (Section 12) |
| **Collection assignment** | Shop sections fall back to `frontpage` if no collection selected | Confirm `frontpage` has all active products |

### Medium priority

| Item | Detail | Action |
|---|---|---|
| **Newsletter integration** | Contact II uses Shopify contact form | Confirm if connected to Klaviyo, Mailchimp, or Shopify Email |
| **Page URL handles** | Expected `/pages/store-locator`, `/pages/about`, etc. | Verify in Admin → Pages |
| **Style guide page** | Internal reference page exists (`page.master`) | Confirm if published or kept private |
| **Seal Subscriptions** | Custom JS repositions widget on PDP | Test PDP after any Seal app update |
| **UpPromote** | App block active in theme settings | Confirm affiliate program is live |
| **Typekit kit** | Kit `rji3kyn` must stay active | Confirm Adobe account and license |
| **Theme backup date** | Last export from live Shopify | Document export date and storage location |

### Low priority / awareness

| Item | Detail |
|---|---|
| **CDN dependency** | Nine external libraries load on every page; outage affects animations |
| **PDP first image** | Position 1 in Media is excluded from PDP carousel by design |
| **Shopping `badge_text`** | Section setting exists but badges come from product metafield `bars` |
| **No homepage carousel** | Shopping grid is static, not a slider |
| **Form field labels** | Hardcoded in theme — label changes need a developer |
| **Heritage product-information** | Disabled on product template — do not re-enable without developer |

---

## Page-Specific Documentation

Dedicated guides for each customer-facing content page live in `handoff/`. Each guide documents the page structure (sections in template order), editable Theme Editor content, images, forms, third-party dependencies, hardcoded content, and maintenance boundaries.

**Where to find maintenance instructions:**

| Need | Read |
|---|---|
| Client-safe updates (copy, images, Theme Editor settings) | **Maintenance Notes** in the relevant page guide below |
| Developer-only changes (forms, layout, integrations) | **Developer Notes** and **Hardcoded Content** in the same guide |
| General Theme Editor navigation | `02-cms-editing-guide.md` |
| Stockist location management (Store Locator) | `07-store-locator-guide.md` and `13-store-locator-page-guide.md` |

| Page | Guide | Summary |
|---|---|---|
| **About Us** | `10-about-page-guide.md` | Template `about` — sections **About I** through **About V** tell the brand story, values, collagen rationale, and founder narrative, followed by **Minishop** (three random products from a collection). No forms. Page body field in Shopify Pages is not used. |
| **Contact** | `11-contact-page-guide.md` | Template `contact` — **Contact I** (general inquiry form) and **Contact II** (newsletter signup with decor images). Both use Shopify contact forms; success handling via `sessionStorage` in theme JS. |
| **Affiliate** | `12-affiliate-page-guide.md` | Template `affiliate` — five sections (**Affiliate I–V**) covering audience, benefits, how it works, and an application form anchored at `#affiliate`. **UpPromote** handles affiliate tracking site-wide. |
| **Store Locator** | `13-store-locator-page-guide.md` | Template `locator` — **Locator I** (Stockist map, widget tag `map_r3mn7jvq`) and **Locator II** (store-request form). Store locations are edited only in the Stockist dashboard. |

---

## Quick reference card

```
EDIT CONTENT     → Theme Editor (Online Store → Themes → Customize)
EDIT PRODUCTS    → Products (+ metafields)
EDIT LOCATIONS   → Stockist dashboard (not Shopify)
EDIT MENUS       → Online Store → Navigation
EDIT SUBSCRIPTIONS → Seal Subscriptions app
EDIT AFFILIATES  → UpPromote app
FORM EMAILS      → Settings → Notifications
BRAND IMAGES     → Content → Files
THEME BACKUP     → Online Store → Themes → Download
```

---

*End of Just Nosh final handoff documentation. For detailed step-by-step instructions, refer to the companion guides listed at the top of this document.*

*Prepared June 2026 · Based on theme audit and handoff package in `handoff/`*
