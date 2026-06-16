# About Page Guide — Just Nosh

**Page:** About Us  
**Expected URL:** `/pages/about`  
**Theme template:** `about` (`templates/page.about.json`)  
**Theme Editor path:** Online Store → Themes → Customize → Pages → About

**Related documents:** `02-cms-editing-guide.md` · `08-product-management-guide.md`

---

# Overview

The About page tells the Just Nosh brand story: mission, values, collagen protein positioning, founder narrative, and a small product showcase at the bottom. Content is built entirely from **custom theme sections** in the Theme Editor. The standard Shopify page body field is **not used** on the live site.

---

# Page Structure

Sections appear in this exact order per `templates/page.about.json` → `"order"`:

| # | Theme Editor name | Section type | File | Visible on live site |
|---|---|---|---|---|
| 1 | *(Heritage main page)* | `main-page` | `sections/main-page.liquid` | **No** — `disabled: true` |
| 2 | **About I** | `about-i` | `sections/about-i.liquid` | Yes |
| 3 | **About II** | `about-ii` | `sections/about-ii.liquid` | Yes |
| 4 | **About III** | `about-iii` | `sections/about-iii.liquid` | Yes |
| 5 | **About IV** | `about-iv` | `sections/about-iv.liquid` | Yes |
| 6 | **About V** | `about-v` | `sections/about-v.liquid` | Yes |
| 7 | **Minishop** | `minishop` | `sections/minishop.liquid` | Yes |

Global **Header** and **Foot** footer render on every page (not part of this template JSON).

> **Needs confirmation:** Page exists in Shopify Admin, is published, and uses theme template **about**.

---

# Editable Content

## About I

| Setting / block | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Rich text |
| Text | Text | Textarea |
| Blocks: **Decor image** | Image, Alt text, Parallax intensity | Up to 8 blocks |

## About II

| Setting | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text |
| Text | Text | Rich text |
| Image | Image | Image picker |
| Image alt text | Image alt text | Text |

## About III

| Setting / block | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text |
| Text | Text | Rich text |
| Blocks: **About item** | Icon, Title, Text | Up to 4 blocks |

## About IV

| Setting | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text |
| Lead text | Lead text | Rich text |
| Body text | Body text | Rich text |

## About V

| Setting | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text |
| Main text | Main text | Rich text |
| Highlight text | Highlight text | Textarea |
| Signature | Signature | Text |
| Image | Image | Image picker |
| Image alt text | Image alt text | Text |

## Minishop

| Setting | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text |
| Collection | Collection | Collection picker |
| Badge text | Badge text | Text |
| Button label | Button label | Text |

**Not editable in Theme Editor:** Product titles, prices, and product images (from **Products** admin). Hover decor on cards comes from product metafield `custom.shopping_media` when set.

---

# Images

## Editable images (Theme Editor)

| Section | Image slots | Source |
|---|---|---|
| About I | Up to 8 **Decor image** blocks | Section block image picker → Shopify Files |
| About II | 1 side image | Section setting `image` |
| About III | 4 block icons | Block setting `icon` |
| About V | 1 founder photo | Section setting `image` |
| Minishop | Product featured images + hover media | **Products → Media** and metafield `custom.shopping_media` |

## Current live images (from `templates/page.about.json`)

| Section | File / reference |
|---|---|
| About I decor | `chip-1.svg`, `lemon-1.svg`, `chocolate-salt-1.svg`, `chocolate-salt-2.svg`, `mani-1.svg`, `mani-2.svg` |
| About II | `JustNosh-Context-Image-01.jpg` |
| About III icons | `muscle.svg`, `digestion.svg`, `chemistry.svg`, `oil.svg` |
| About V | `1769a96bd6f28c707c8f768d2db9892e6d17ddec.jpg` |

All paths: `shopify://shop_images/...` (Shopify Admin → Content → Files).

## Hardcoded images

| Item | Location | Notes |
|---|---|---|
| Decor slot positions (About I) | `assets/wagon.css` | CSS classes `.item-about-1` … `.item-about-6` — swapping images OK; adding slots needs developer |
| Founder image parallax | `sections/about-v.liquid` | `data-parallax="-0.08"` — not editable in Theme Editor |

## Image sources summary

| Source | Used for |
|---|---|
| Theme Editor image pickers | About decor, context photo, benefit icons, founder image |
| Shopify Files (`shop_images`) | SVG/PNG assets referenced in template |
| Products admin | Minishop product cards |

---

# Forms

**No forms** on the About page.

---

# Dependencies

| Dependency | Used by | Notes |
|---|---|---|
| Product collection | Minishop | Falls back to `collections['frontpage']` if collection setting is empty (current template: empty) |
| Product metafield `custom.shopping_media` | Minishop hover images | Optional per product |
| `assets/wagon.css` | All About sections | Layout and styling |
| `assets/wagon.js` | About I parallax (`data-parallax` on decor blocks) | Scroll animation on desktop |
| Inline script | Minishop | Randomly displays **3 products** from collection; not Theme Editor configurable |

**No third-party apps** detected on this page.

---

# Hardcoded Content

Requires a **developer** to change:

| Content | File |
|---|---|
| Minishop empty state: "No products found in this collection." | `sections/minishop.liquid` |
| Minishop logic: show 3 random products from collection | `sections/minishop.liquid` (inline `<script>`) |
| Minishop product limit pool: 20 products max before random pick | `sections/minishop.liquid` |
| About V image parallax value (`-0.08`) | `sections/about-v.liquid` |
| Decor image CSS positions (About I) | `assets/wagon.css` |
| Section layout and typography classes | Section Liquid + `assets/wagon.css` |
| Heritage `main-page` section (disabled) | `templates/page.about.json` |

---

# Maintenance Notes

### Safe for client to update

- All headings, body copy, and rich text in About I–V
- All images via Theme Editor image pickers
- About I decor blocks: swap images, adjust parallax intensity (0–0.5)
- About III benefit items: icons, titles, text
- Minishop heading, badge text, button label, collection assignment
- Product data for Minishop cards via **Products** admin

### Use caution

- **Minishop collection:** If empty, theme uses `frontpage` collection — confirm it contains active products
- **Minishop random display:** Three products shown change on each page load — not manual curation
- **Minishop badge:** Uses section `badge_text` setting (not product metafield `custom.bars`)

### Requires developer

- Add/remove About I decor slots beyond 8 blocks
- Change Minishop from random 3 to fixed product selection
- Layout, animation, or new sections

---

# Developer Notes

- Template: `templates/page.about.json`
- Disabled `main-page` block can be removed from JSON but serves no live purpose
- About III reuses Why-section styling (`.why-i`, `.item-why`)
- About I parallax: `block.settings.parallax_speed` → `data-parallax` attribute; disabled when speed is 0
- Minishop differs from homepage **Shopping** section: uses `badge_text` from settings (Shopping uses `custom.bars` metafield)
- Minishop inline script shuffles `.item-shopping` elements client-side — affects DOM order and SEO visibility of hidden items

---

*Last updated: June 2026 · Audit source: `templates/page.about.json`, `sections/about-*.liquid`, `sections/minishop.liquid`*
