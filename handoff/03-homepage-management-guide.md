# Homepage Management Guide — Just Nosh

This guide documents how to update hero images, text, featured products, and decorative graphics on the Just Nosh homepage.

**Template file:** `templates/index.json`  
**Theme Editor path:** Online Store → Themes → Customize → **Home page**

**Related documents:** `02-cms-editing-guide.md` · `08-product-management-guide.md` · `06-assets-inventory.md`

---

## Summary

The homepage has **4 custom sections** in a fixed order. There is **no announcement banner** and **no carousel** on the homepage. The hero is a layered **image composition** (not a slider). Featured products appear in a **static product grid**.

| Topic | Homepage reality |
|---|---|
| Hero | **Superhero** section — layered bar + ingredient images |
| Site-wide banner | **Not active** (no announcement bar in `sections/header-group.json`) |
| Carousel | **None** on homepage (Splide carousels exist on PDP only) |
| Featured products | **Shopping** section — collection-driven grid |
| Snippets | Homepage sections do **not** render theme snippets — all markup is in section files |
| Global theme settings | `config/settings_schema.json` has no homepage-specific settings |

---

## 1. Homepage sections (in order)

From `templates/index.json` → `"order"`:

| # | Section name (Theme Editor) | Section type | File |
|---|---|---|---|
| 1 | **Superhero** | `superhero` | `sections/superhero.liquid` |
| 2 | **Why** | `why` | `sections/why.liquid` |
| 3 | **Shopping** | `shopping` | `sections/shopping.liquid` |
| 4 | **Our mission** | `ourmission` | `sections/ourmission.liquid` |

The homepage also includes the global **Header** (`sections/header-group.json`) and **Foot** footer (`sections/footer-group.json`), which are shared across all pages.

---

## 2. What each section controls

### 1 — Superhero (hero)

**Visual role:** Full-width hero at the top of the homepage. Dark blue background with headline, subtitle, CTA button, and a composed scene of product bars plus floating ingredient graphics.

**Layout structure:**
- Left column: headline, text, button
- Right column: layered images (bars + decorative chips, chocolate, peanuts, lemon)

**CSS positioning:** Each image slot (`bar-1`, `chocolate-1`, etc.) has fixed positions defined in `assets/home.css` (desktop and mobile breakpoints). Images are absolutely positioned — swapping images works, but **adding or removing slots requires a developer**.

**Animation:** Entrance animation is CSS-driven via the `.hero-ready` class, triggered by `assets/wagon.js` on page load. Stagger delays are defined in `assets/home.css`.

---

### 2 — Why (benefits)

**Visual role:** Four-column benefit grid with icons, titles, and short descriptions.

**Layout:** Section heading + intro text, then up to **4 blocks** (Why item).

---

### 3 — Shopping (featured products)

**Visual role:** Product flavor grid — the homepage “shop” section.

**Layout:** Section heading + subheading, then a responsive grid of product cards pulled from a collection.

**Per card shows:**
- Badge (from product metafield `custom.bars`)
- Featured product image (position 1 in product Media)
- Hover decor images (from product metafield `custom.shopping_media`)
- Product title and price (from product record)
- “Shop flavor” button (label from section setting)

**Not a carousel** — products display in a static flex/grid layout with CSS hover effects (`assets/wagon.css`).

---

### 4 — Our mission

**Visual role:** Mission statement with CTA button and floating decorative ingredient graphics in the background.

**Animation:** CTA button has scroll animation (`data-anim="fade-up"`) handled by `assets/wagon.js`.

---

## 3. Editable fields in Theme Customizer

### Superhero

| Setting label | Type | Controls |
|---|---|---|
| Title | Rich text | Main headline (supports bold) |
| Text | Textarea | Subtitle paragraph |
| Button text | Text | CTA label |
| Button link | URL | CTA destination |
| Media | Select | `Image composition` or `Video` |
| Bar 1 / Bar 2 / Bar 3 | Image | Product bar PNGs in hero composition |
| Chocolate 1–5 | Image | Decorative chip/chocolate SVGs |
| Peanut 1 / Peanut 2 | Image | Decorative peanut SVGs |
| Lemon 1 | Image | Decorative lemon SVG |
| Video | Video | Background video (only if Media = Video) |

**Current live config** (`templates/index.json`): Media is set to **Image composition**. Button links to `shopify://collections/shop`.

---

### Why

**Section settings:**

| Setting label | Controls |
|---|---|
| Heading | Section title (e.g. “WHY JUSTNOSH”) |
| Text | Intro paragraph below heading |

**Blocks — Why item** (max 4):

| Block field | Controls |
|---|---|
| Icon | Benefit icon image |
| Title | Benefit headline |
| Text | Benefit description |

---

### Shopping

| Setting label | Controls |
|---|---|
| Heading | Section title (e.g. “SHOP FLAVORS”) |
| Subheading | Line below heading |
| Collection | Which collection’s products to display |
| Products to show | Count from 2 to 12 (currently **6**) |
| Badge text | ⚠️ **Defined in schema but not used in code** — see note below |
| Button label | Text on each product card button |

> **Note on “Badge text”:** The setting exists in `sections/shopping.liquid` schema but is **not referenced** in the section Liquid template. Product badges on the homepage come from the product metafield **`custom.bars`** only. To change a badge, edit the product metafield — not this setting.

**Current live config:** Collection is **empty** → theme falls back to the **`frontpage`** collection.

---

### Our mission

| Setting label | Controls |
|---|---|
| Eyebrow | Small label above headline (e.g. “OUR MISSION”) |
| Heading | Main mission statement (rich text) |
| Button label | CTA text |
| Button link | CTA URL (currently About page) |
| Detail image 1–6 | Decorative background ingredient graphics |

---

## 4. Images updatable from Theme Customizer

### Superhero — image composition map

Each picker controls a specific visual slot. Use the **same slot** when replacing — do not rename slots.

| Setting | Visual slot (CSS class) | Current asset (from template) | Suggested format |
|---|---|---|---|
| Bar 1 | `.bar-1` | PNG product bar | PNG, transparent background |
| Bar 2 | `.bar-2` | PNG product bar | PNG, transparent background |
| Bar 3 | `.bar-3` | PNG product bar | PNG, transparent background |
| Chocolate 1 | `.chocolate-1` | SVG chip | SVG or PNG |
| Chocolate 2 | `.chocolate-2` | SVG chip | SVG or PNG |
| Chocolate 3 | `.chocolate-3` | SVG chip | SVG or PNG |
| Chocolate 4 | `.chocolate-4` | SVG chocolate | SVG or PNG |
| Chocolate 5 | `.chocolate-5` | SVG chocolate | SVG or PNG |
| Peanut 1 | `.peanut-1` | SVG peanut | SVG or PNG |
| Peanut 2 | `.peanut-2` | SVG peanut | SVG or PNG |
| Lemon 1 | `.lemon-1` | SVG lemon | SVG or PNG |

Rendered at up to **1200 px** wide (`sections/superhero.liquid`).

### Why — block icons

Each **Why item → Icon** picker. Rendered at up to **180 px** wide.

### Our mission — detail images

**Detail image 1–6** pickers. Rendered at up to **600 px** wide. Same decorative ingredient assets often reused across homepage sections.

### Shopping — product images (not in Theme Customizer)

Product card images are **not** section image pickers. They come from:

| Image | Source |
|---|---|
| Main card image | Product → Media (position 1 / featured image) |
| Hover decor images | Product metafield `custom.shopping_media` |

See `08-product-management-guide.md` for product image workflow.

---

## 5. Content source map

| Content | Source | Editable where |
|---|---|---|
| Hero headline, subtitle, CTA | Section settings | Theme Editor → **Superhero** |
| Hero bar + decor images | Section image pickers | Theme Editor → **Superhero** |
| Hero video (optional) | Section video picker | Theme Editor → **Superhero** |
| Benefit icons, titles, text | Section blocks | Theme Editor → **Why** |
| “SHOP FLAVORS” heading | Section settings | Theme Editor → **Shopping** |
| Product names | Product records | **Products** |
| Product prices | Product records | **Products** |
| Product card images | Product media | **Products → Media** |
| Product hover images | `custom.shopping_media` | **Products → Metafields** |
| Product badges (“12 BARS”) | `custom.bars` | **Products → Metafields** |
| Product card order | Collection sort | **Products → Collections** |
| Mission text + CTA | Section settings | Theme Editor → **Our mission** |
| Mission decor images | Section image pickers | Theme Editor → **Our mission** |
| Header logo, menu | Header section | Theme Editor → **Header** |
| Footer content | Foot section | Theme Editor → **Footer** |

### Hardcoded in Liquid (not editable in Theme Customizer)

| Text / behavior | File | Notes |
|---|---|---|
| Image alt text: `Bar 1`, `Chocolate 1`, `Peanut 1`, etc. | `sections/superhero.liquid` | Generic alt strings — developer needed for custom alt per image |
| Empty collection message: “No products found in this collection.” | `sections/shopping.liquid` | Shown when collection has no products |
| Hero image slot positions and sizes | `assets/home.css` | CSS classes `.bar-1`, `.chocolate-1`, etc. |
| Hero entrance animation timing | `assets/home.css`, `assets/wagon.js` | `.hero-ready` class and stagger delays |
| Shopping hover animation | `assets/wagon.css` | CSS transitions on `.item-shopping` |
| Homepage min-height transition on hero | `assets/home.css` | `95vh` → `50vh` on page ready |

---

## 6. What requires a developer

| Change | Why |
|---|---|
| Add/remove hero image slots (e.g. a 4th bar) | Fixed slots in Liquid + CSS positions in `assets/home.css` |
| Change hero image layout or positions | CSS in `assets/home.css` tied to class names |
| Custom alt text per hero image | Alt strings hardcoded in `sections/superhero.liquid` |
| Enable homepage announcement banner | Not configured — would need `header-announcements` section added to `header-group.json` |
| Add a carousel to homepage | No carousel exists — would need new section or `[data-splide]` markup |
| Wire up “Badge text” setting in Shopping | Setting exists in schema but unused in template |
| Change hero entrance animation behavior | `assets/wagon.js` + `assets/home.css` |
| Change empty-state message copy | `sections/shopping.liquid` |
| Switch hero from image composition to video | Possible via Theme Editor, but video layout/CSS may need review |

---

## 7. Step-by-step guides

### How to open the homepage editor

1. Shopify Admin → **Online Store → Themes**
2. Click **Customize** on the live theme
3. In the top page dropdown, select **Home page**
4. Click any section in the preview or left sidebar to edit it
5. Click **Save** when done

---

### How to update the hero (Superhero section)

The hero is an **image composition**, not a slider. You replace individual images in fixed slots.

1. Open Theme Editor → **Home page**
2. In the left sidebar, click **Superhero**
3. **Update text:**
   - Edit **Title** (headline)
   - Edit **Text** (subtitle)
   - Edit **Button text** and **Button link** if needed
4. **Update hero images:**
   - Confirm **Media** is set to **Image composition**
   - Replace images one slot at a time:
     - **Bar 1 / Bar 2 / Bar 3** — main product bar PNGs (transparent background recommended)
     - **Chocolate 1–5** — small decorative chips/chocolate
     - **Peanut 1 / Peanut 2** — peanut decor
     - **Lemon 1** — lemon decor
   - Use PNG for bars, SVG or PNG for small decor elements
   - Keep similar proportions to existing assets so CSS positioning still looks correct
5. **Optional — switch to video hero:**
   - Set **Media** to **Video**
   - Upload/select a video in the **Video** field
   - Preview on desktop and mobile
6. Click **Save**
7. Hard-refresh the homepage and verify the entrance animation plays correctly

**Tips:**
- Replace one image at a time and save to isolate issues
- Bar images should have transparent backgrounds
- Do not remove all images from a slot category without developer review — empty slots leave gaps in the composition

---

### How to update “banners” on the homepage

There is **no separate announcement banner** on the homepage. Promotional messaging lives inside page sections:

| If you want to change… | Edit this |
|---|---|
| Main hero message + visual | **Superhero** section |
| Benefit highlights | **Why** section |
| Shop section intro | **Shopping** section heading/subheading |
| Mission call-to-action | **Our mission** section |

To add a **site-wide announcement bar** above the header (e.g. “Free shipping over $50”), that feature is **not currently enabled**. Contact your developer — it would use the Heritage `header-announcements` section.

---

### How to update featured products (Shopping section)

The homepage product grid is **not a carousel**. Products are shown in a static grid from a collection.

1. Open Theme Editor → **Home page** → **Shopping**
2. Edit **Heading** and **Subheading** if needed
3. Under **Collection**, select the collection to feature (or leave blank to use **frontpage**)
4. Set **Products to show** (2–12)
5. Edit **Button label** (e.g. “SHOP FLAVOR”)
6. Click **Save**

**Then manage the products themselves:**

| Task | Where |
|---|---|
| Change which products appear | **Products → Collections** → add/remove products from the collection used above |
| Change product order | **Products → Collections** → drag products (use **Manual** sort) |
| Change product image on card | **Products → [product] → Media** (position 1) |
| Change hover decor images | **Products → [product] → Metafields → `shopping_media`** |
| Change badge (“12 BARS”) | **Products → [product] → Metafields → `bars`** |
| Change product name or price | **Products → [product]** |

7. Refresh the homepage to confirm the grid looks correct

---

### How to update carousels on the homepage

**There are no carousels on the homepage.**

The theme supports Splide carousels elsewhere (PDP image gallery via `sections/pdp-i.liquid`, and generic `[data-splide]` elements via `assets/wagon.js`), but none are used in `templates/index.json`.

If you need a scrolling product carousel on the homepage, that would be a **new development task**.

---

### How to update the Why section

1. Theme Editor → **Home page** → **Why**
2. Edit **Heading** and **Text**
3. Expand each **Why item** block:
   - Replace **Icon** image
   - Edit **Title** and **Text**
4. To reorder blocks, drag them in the sidebar
5. Save (max 4 blocks)

---

### How to update Our mission

1. Theme Editor → **Home page** → **Our mission**
2. Edit **Eyebrow**, **Heading**, **Button label**, **Button link**
3. Replace **Detail image 1–6** for background decor graphics
4. Save

---

## Assets used by homepage sections

Homepage sections do not load dedicated JS files. They rely on global theme assets:

| Asset file | Homepage usage |
|---|---|
| `assets/home.css` | Superhero layout, positions, entrance animation, min-height transition |
| `assets/wagon.css` | Why, Shopping, Our mission section styles; shopping hover effects |
| `assets/master.css` | Global typography (`.he-l`, `.bo-m`, `.btn-i`), colors, layout utilities |
| `assets/wagon.js` | Page entrance (`.hero-ready`, `.wagon-page-ready`); `fade-up` animation on Our mission CTA |
| `assets/base.css` | Heritage base styles (via `snippets/stylesheets.liquid`) |

**Snippets:** None of the four homepage sections call `{% render %}` snippets. They are self-contained in their section files.

**settings_schema.json:** No homepage-specific theme settings. Header logo can also be set under **Theme settings → Logo** (`config/settings_data.json`).

---

## Header & footer on homepage

Edited separately from homepage body sections:

| Area | Section name | File | Notable settings |
|---|---|---|---|
| Header | Header | `sections/header.liquid` | Logo, main-menu, sticky header (always on), transparent header **disabled** for homepage |
| Footer | Foot | `sections/foot.liquid` | Logo, shop/learn/support menus, CTA, social links, copyright |

Header config: `sections/header-group.json`  
Footer config: `sections/footer-group.json`

---

## Quick reference checklist

### After updating the hero
- [ ] Headline and CTA link are correct
- [ ] All bar images have transparent backgrounds
- [ ] Decor images appear in correct positions (desktop + mobile)
- [ ] Entrance animation plays smoothly

### After updating featured products
- [ ] Correct collection is selected (or `frontpage` fallback is intentional)
- [ ] Products appear in desired order
- [ ] Each product has featured image (media position 1)
- [ ] `custom.bars` metafield shows correct badge
- [ ] `custom.shopping_media` hover images work

### After updating Why / Our mission
- [ ] All text proofread
- [ ] Icons and detail images display correctly
- [ ] CTA links go to the right page

---

## Troubleshooting

| Issue | Likely cause | Fix |
|---|---|---|
| Hero images overlap or look misaligned | New images have very different proportions | Use images similar in size/ratio to originals, or contact developer |
| Product grid is empty | Collection has no products or wrong collection selected | Add products to collection; check **Shopping → Collection** setting |
| Badge not showing on product card | `custom.bars` metafield empty | Fill metafield on product |
| “Badge text” setting does nothing | Setting not wired in code | Use `custom.bars` metafield instead |
| Hover effect missing on product card | `custom.shopping_media` empty | Upload metafield images on product |
| Hero video not showing | Media type still set to Image composition | Switch Media to Video and assign video file |
| Changes not visible | Forgot to save | Click Save in Theme Editor |

---

*Last updated: June 2026 · Audited from `templates/index.json`, `sections/superhero.liquid`, `sections/why.liquid`, `sections/shopping.liquid`, `sections/ourmission.liquid`, `assets/home.css`, `assets/wagon.css`, `assets/wagon.js`, and `sections/header-group.json`*
