# CMS & Page Editing Guide — Just Nosh

This guide explains how to update website copy, images, and product content for eatjustnosh.com using Shopify Admin and the Theme Editor.

**Related documents:** `01-client-handoff-index.md` · `08-product-management-guide.md` · `07-store-locator-guide.md`

---

## Before you start

### Two main places to edit content

| Where | Best for |
|---|---|
| **Online Store → Themes → Customize** (Theme Editor) | Page headlines, section text, images, footer, header, shop grid headings |
| **Products** (Shopify Admin) | Product names, prices, images, variants, and product metafields (PDP content) |
| **Online Store → Pages** | Page title, URL handle, and which template a page uses — **not** the main visual content |

Most Just Nosh pages are built from **custom sections** in the Theme Editor. The body field on a Page record is **not used** on the live site for About, Contact, Affiliate, or Store Locator.

### How to open the Theme Editor

1. Log in to **Shopify Admin**
2. Go to **Online Store → Themes**
3. On your live theme, click **Customize**

### How to navigate to a specific page

In the Theme Editor top bar, use the page dropdown:

| Page | Select in Theme Editor |
|---|---|
| Homepage | **Home page** |
| About | **Pages → About** (or the page assigned template `about`) |
| Contact | **Pages → Contact** (template `contact`) |
| Affiliate | **Pages → Affiliate** (template `affiliate`) |
| Store Locator | **Pages → Store Locator** (template `locator`) |
| Any product | **Products → [product name]** |
| Header / Footer | Click **Header** or **Footer** in the left sidebar |

> If a page does not appear correctly, confirm its template under **Online Store → Pages → [page] → Theme template** (see table in [Page templates](#page-templates)).

### How to identify which section controls a block

1. In the Theme Editor, **click the content** you want to change on the page preview.
2. The matching section name will highlight in the **left sidebar**.
3. Section names on this site match the labels saved in the theme — for example **Superhero**, **Why**, **Contact I**, **PDP I**.
4. If a section contains **blocks** (icons, list items, steps), click the section first, then expand individual blocks in the sidebar (e.g. **Why item**, **List item**, **Decor image**).
5. If clicking text does nothing, that content may come from a **Product** or **metafield** — edit it under **Products** instead.

### When to use Theme Editor vs Products vs Pages

| Content type | Edit here | Example |
|---|---|---|
| Homepage hero headline | Theme Editor → **Superhero** | “Protein Bars Made For…” |
| Product name on PDP | **Products** → Title | “Peanut Butter Chocolate” |
| Product intro on PDP | **Products** → Metafields | `pdp_intro` |
| Product gallery images | **Products** → Media | Carousel photos |
| About page story text | Theme Editor → **About V** | Founder story |
| Contact form heading | Theme Editor → **Contact I** | “CONTACT US” |
| Form field labels (Name, Email…) | **Not in Theme Editor** — hardcoded in theme | Requires developer to change |
| Store map locations | **Stockist dashboard** (external) | See `07-store-locator-guide.md` |
| Footer menu links | Theme Editor → **Foot** + **Online Store → Navigation** | Shop / Learn / Support menus |
| Page URL (`/pages/about`) | **Online Store → Pages** → Search engine listing | URL handle |

### Saving and publishing

- Click **Save** in the top-right corner of the Theme Editor after making changes.
- Product changes save from the product detail screen in **Products**.
- Always preview on desktop and mobile before publishing major updates.

---

## Page templates

Each custom page uses a JSON template file in the theme. Confirm the correct template is assigned to each page:

| Page | Theme template (suffix) | Template file |
|---|---|---|
| About | `about` | `templates/page.about.json` |
| Contact | `contact` | `templates/page.contact.json` |
| Affiliate | `affiliate` | `templates/page.affiliate.json` |
| Store Locator | `locator` | `templates/page.locator.json` |
| Homepage | (default index) | `templates/index.json` |
| Product pages | (default product) | `templates/product.json` |

**To assign a template:** Online Store → Pages → [page] → right sidebar → **Theme template** → choose the matching template → Save.

---

## Global: Header & Footer

These appear on every page and are edited separately from page content.

### Header

- **Theme Editor:** click **Header** in the left sidebar
- **Section:** Header (Heritage)
- **Template group:** `sections/header-group.json`
- **File:** `sections/header.liquid`

Editable items include logo (also under **Theme settings → Logo**), main navigation menu, and social links in the mobile drawer.

### Footer

- **Theme Editor:** click **Footer** in the left sidebar
- **Section name:** **Foot**
- **File:** `sections/foot.liquid`

| Setting label | What it controls |
|---|---|
| Logo | Footer logo image |
| Shop heading / Shop menu | First footer column |
| Learn heading / Learn menu | Second footer column |
| Support heading / Support menu | Third footer column |
| CTA heading / CTA text / CTA button label / CTA button link | “Find us near you” call-to-action |
| Instagram URL / Facebook URL / TikTok URL / LinkedIn URL | Social icons |
| Copyright text | Footer copyright line |

Menu links themselves are managed under **Online Store → Navigation** (menus: `shop`, `learn`, `support`).

---

## Homepage

**Template file:** `templates/index.json`  
**Theme Editor:** Home page

Sections appear in this order:

### 1. Superhero

| | |
|---|---|
| **Section name in editor** | Superhero |
| **File** | `sections/superhero.liquid` |

| Setting label | What to edit |
|---|---|
| Title | Main hero headline (supports bold formatting) |
| Text | Subtitle below headline |
| Button text | CTA button label (e.g. “Shop bars”) |
| Button link | Where the button goes (e.g. Shop collection) |
| Media | Image composition or Video |
| Bar 1 / Bar 2 / Bar 3 | Product bar images in hero |
| Chocolate 1–5 | Decorative chip graphics |
| Peanut 1 / Peanut 2 | Decorative peanut graphics |
| Lemon 1 | Decorative lemon graphic |
| Video | Hero video (if Media is set to Video) |

### 2. Why

| | |
|---|---|
| **Section name in editor** | Why |
| **File** | `sections/why.liquid` |

| Setting label | What to edit |
|---|---|
| Heading | Section title (e.g. “WHY JUSTNOSH”) |
| Text | Intro paragraph below heading |

**Blocks — Why item** (up to 4):

| Block field | What to edit |
|---|---|
| Icon | Benefit icon image |
| Title | Benefit headline (e.g. “14-16g protein”) |
| Text | Benefit description |

### 3. Shopping

| | |
|---|---|
| **Section name in editor** | Shopping |
| **File** | `sections/shopping.liquid` |

| Setting label | What to edit |
|---|---|
| Heading | Section title (e.g. “SHOP FLAVORS”) |
| Subheading | Line below heading |
| Collection | Which collection’s products to show |
| Products to show | Number of products (2–12) |
| Badge text | Default badge if product has no `bars` metafield |
| Button label | Button on each product card (e.g. “SHOP FLAVOR”) |

> Product images, names, and prices come from **Products**. Per-product badge text comes from the `bars` metafield. Hover images come from the `shopping_media` metafield. See [Product Detail Pages](#product-detail-pages--pdps).

### 4. Our mission

| | |
|---|---|
| **Section name in editor** | Our mission |
| **File** | `sections/ourmission.liquid` |

| Setting label | What to edit |
|---|---|
| Eyebrow | Small label above headline (e.g. “OUR MISSION”) |
| Heading | Main mission statement |
| Button label | CTA button text |
| Button link | CTA destination (e.g. About page) |
| Detail image 1–6 | Decorative background graphics |

---

## About Us

**Template file:** `templates/page.about.json`  
**Theme Editor:** Pages → About

> The default **main-page** section on this template is **disabled**. Do not rely on the Page body field — all content is in the sections below.

Sections in order:

### About I

| Setting label | What to edit |
|---|---|
| Heading | Top headline |
| Text | Intro paragraph |

**Blocks — Decor image:** floating ingredient graphics (Image, Alt text, Parallax intensity).

### About II

| Setting label | What to edit |
|---|---|
| Heading | e.g. “WHY WE EXIST” |
| Text | Body copy (rich text) |
| Image | Side image |
| Image alt text | Accessibility description |

### About III

| Setting label | What to edit |
|---|---|
| Heading | e.g. “WHAT WE STAND FOR” |
| Text | Intro paragraph |

**Blocks — About item:**

| Block field | What to edit |
|---|---|
| Icon | Benefit icon |
| Title | Benefit title |
| Text | Benefit description |

### About IV

| Setting label | What to edit |
|---|---|
| Heading | e.g. “WHY COLLAGEN PROTEIN” |
| Lead text | Opening paragraph |
| Body text | Remaining copy |

### About V

| Setting label | What to edit |
|---|---|
| Heading | e.g. “OUR STORY” |
| Main text | Founder story body |
| Highlight text | Pull quote / closing line |
| Signature | e.g. “JENNIFER NEWMAN, FOUNDER” |
| Image | Founder photo |
| Image alt text | Photo description |

### Minishop

| Setting label | What to edit |
|---|---|
| Heading | e.g. “SHOP JUSTNOSH” |
| Collection | Products to display |
| Badge text | Default badge label |
| Button label | Card button text |

---

## Contact

**Template file:** `templates/page.contact.json`  
**Theme Editor:** Pages → Contact

> The default **main-page** section is **disabled**.

### Contact I

| | |
|---|---|
| **Section name in editor** | Contact I |
| **File** | `sections/contact-i.liquid` |

| Setting label | What to edit |
|---|---|
| Heading | e.g. “CONTACT US” |
| Subheading | Short intro above the form |
| Button label | Submit button text |
| Email note | Text below form (supports email link) |

**Form fields** (Name, Email, Subject, Message, placeholders, success message) are **not editable in the Theme Editor**. They are defined in the theme code. To change field labels or placeholders, contact your developer.

Form submissions are sent via **Shopify Notifications**.

### Contact II

| | |
|---|---|
| **Section name in editor** | Contact II |
| **File** | `sections/contact-ii.liquid` |

| Setting label | What to edit |
|---|---|
| Heading | e.g. “WANT EXCLUSIVE OFFERS AND PRODUCT UPDATES?” |
| Text | e.g. “Unsubscribe anytime.” |
| Button label | e.g. “Subscribe” |
| Decor image 1–5 (+ alt text fields) | Decorative ingredient graphics around the form |

This is a newsletter signup form (Name + Email). It uses Shopify’s contact form — not a separate email app visible in the theme.

---

## Affiliate

**Template file:** `templates/page.affiliate.json`  
**Theme Editor:** Pages → Affiliate

### Affiliate I

> In the theme code the section preset is named **Affiliate**; on the live page it appears as **Affiliate I**.

| Setting label | What to edit |
|---|---|
| Heading | Hero headline |
| Text | Intro paragraph |
| Button label | e.g. “BECOME AN AFFILIATE” |
| Button link | Scroll target (default: `#affiliate`) |

### Affiliate II

| Setting label | What to edit |
|---|---|
| Heading | e.g. “WHO THIS IS FOR” |
| Subheading | Short intro |
| Image | Side image |
| Image alt text | Image description |

**Blocks — List item:** audience type labels (e.g. “Dietitians and nutrition professionals”).

### Affiliate III

| Setting label | What to edit |
|---|---|
| Heading | e.g. “WHY PARTNER WITH JUSTNOSH” |

**Blocks — Item:**

| Block field | What to edit |
|---|---|
| Icon | Benefit icon |
| Text | Benefit description |

### Affiliate IV

| Setting label | What to edit |
|---|---|
| Heading | e.g. “HOW IT WORKS” |

**Blocks — Step:**

| Block field | What to edit |
|---|---|
| Number | Step number (1, 2, 3) |
| Text | Step description |

### Affiliate V

| Setting label | What to edit |
|---|---|
| Heading | e.g. “APPLY TO BECOME AN AFFILIATE” |
| Text | Short intro above form |
| Bar image 1 / 2 / 3 | Decorative product images |
| Image alt text | Alt for bar images |
| Button label | e.g. “SUBMIT APPLICATION” |
| Email note | Alternative contact email text |

**Application form fields** (Name, Email, Social handle, Message, Preferred coupon code) are **hardcoded in the theme** and not editable via Theme Editor. Form submissions go to Shopify Notifications.

---

## Store Locator

**Template file:** `templates/page.locator.json`  
**Theme Editor:** Pages → Store Locator

### Locator I

| Setting label | What to edit |
|---|---|
| Heading | e.g. “FIND JUSTNOSH NEAR YOU” |
| Stockist widget tag | Stockist map ID (`map_r3mn7jvq`) — change only with developer guidance |
| Load Stockist script | Keep enabled unless instructed otherwise |

**Store locations** (addresses, pins, retailer names) are managed in the **Stockist** dashboard — not in Shopify. See `07-store-locator-guide.md`.

### Locator II

> This section may appear in the sidebar as **Locator II** (template key: `main`).

| Setting label | What to edit |
|---|---|
| Heading | e.g. “DON'T SEE JUSTNOSH AT A STORE NEAR YOU?” |
| Subheading | Intro above the form |
| Button label | Submit button text |
| Email note | Alternative email text below form |

**Form fields** (Name, Email, Message) are hardcoded in the theme. Submissions go to Shopify Notifications with category “Store locator inquiry”.

---

## Product Detail Pages / PDPs

**Template file:** `templates/product.json`  
**Theme Editor:** Products → [select a product]

Sections in order:

| Order | Section name | Editable in Theme Editor? | Where content comes from |
|---|---|---|---|
| 1 | **PDP I** | Minimal (buy button block only) | **Products** + metafields |
| 2 | **PDP II** | No section settings | **Products** metafields + hardcoded headings |
| 3 | **PDP III** | No section settings | **Products** metafields + hardcoded tab copy |
| 4 | **PDP Recommendations** | Yes — heading & settings | **Products** + section settings |
| 5 | Product information | **Disabled** — do not enable | Heritage default (unused) |

### Editing a product in Shopify Admin

1. Go to **Products** in Shopify Admin
2. Click the product you want to edit
3. Update as needed:

| Field | What it controls on the PDP |
|---|---|
| **Title** | Product name shown in PDP I |
| **Description** | Fallback for `pdp_intro` if metafield is empty |
| **Media** | Main image carousel in PDP I |
| **Pricing** | Price shown on PDP |
| **Variants** | Flavor/size options and variant images |

4. Scroll to **Metafields** (product metafields, namespace `custom`):

| Metafield | What it controls |
|---|---|
| `pdp_intro` | Short intro text under the price |
| `flavor_profile_title` | Flavor profile label |
| `flavor_profile_text` | Flavor profile description |
| `price_suffix_label` | Text after price (e.g. “/ 12 bars”) |
| `pdp_highlights` | Benefit chips (list) |
| `color_primary` | PDP background color |
| `color_secondary` | Flavor profile box color |
| `color_tertiary` | Text and button colors |
| `color_tertiary_contrast` | Button text color |
| `color_light` | PDP III inner background |
| `key_ingredients` | Ingredient names in PDP II |
| `key_ingredients_image` | Ingredients image (desktop) |
| `key_ingredients_image_responsive` | Ingredients image (mobile) |
| `nutritional_facts_files` | Nutrition label images in PDP III |
| `bars` | Badge on shop grids and cart |
| `shopping_media` | Hover images on shop cards |

> Full metafield reference: `08-product-management-guide.md`

### PDP I — Main product area

- **Section name:** PDP I (`sections/pdp-i.liquid`)
- **No editable text fields** in the Theme Editor for this section
- Product title, price, intro, highlights, flavor profile, colors, and gallery all pull from the **product record and metafields**
- Buy button / quantity / subscription widget: managed via product settings and the **Seal Subscriptions** app

### PDP II — Key ingredients

- **Section name:** PDP II (`sections/pdp-ii.liquid`)
- **Hardcoded in theme (not editable in Theme Editor):**
  - Section heading: “KEY INGREDIENTS”
  - Intro paragraph about flavoring and ingredients
- **Editable via product metafields:** `key_ingredients`, `key_ingredients_image`, `key_ingredients_image_responsive`

### PDP III — Tabs

- **Section name:** PDP III (`sections/pdp-iii.liquid`)
- Tab labels are hardcoded: “Nutrition Facts”, “Why Collagen Protein”, “Seed Oil Free Certified”
- **Nutrition Facts tab:** images from `nutritional_facts_files` metafield
- **Collagen and Seed Oil tabs:** body copy is hardcoded in the theme — contact a developer to change

### PDP Recommendations

- **Section name:** PDP Recommendations (`sections/pdp-recommendations.liquid`)

| Setting label | What to edit |
|---|---|
| Heading | e.g. “Explore other flavors” |
| Product | Optional — leave blank to use current product for recommendations |
| Recommendation type | Related or Complementary |
| Products to show | 2 or 3 |
| Badge text | Default badge label |
| Button label | Card button text |

---

## Quick reference: what lives where

| I want to change… | Go to… |
|---|---|
| Homepage hero text | Theme Editor → Home → **Superhero** |
| Homepage product grid | Theme Editor → Home → **Shopping** + **Products** |
| About founder story | Theme Editor → About → **About V** |
| Contact page headline | Theme Editor → Contact → **Contact I** |
| Newsletter section copy | Theme Editor → Contact → **Contact II** |
| Affiliate program steps | Theme Editor → Affiliate → **Affiliate IV** blocks |
| Store locator headline | Theme Editor → Store Locator → **Locator I** |
| Store locations on map | **Stockist dashboard** |
| Product name or price | **Products** → [product] |
| Product photos | **Products** → [product] → Media |
| Product intro / ingredients / nutrition | **Products** → [product] → Metafields |
| Footer links | Theme Editor → **Foot** + **Navigation** |
| Menu structure | **Online Store → Navigation** |

---

## Recommended video walkthroughs to record

Use this checklist to record short screen-share videos for your team. Suggested length: 2–5 minutes each.

| # | Video title | What to show | Status |
|---|---|---|---|
| 1 | Opening the Theme Editor | Shopify Admin → Online Store → Themes → Customize | ☐ To record |
| 2 | Editing the Homepage | Superhero, Why, Shopping, Our mission sections | ☐ To record |
| 3 | Editing the About page | About I through V, Minishop | ☐ To record |
| 4 | Editing the Contact page | Contact I and Contact II settings | ☐ To record |
| 5 | Editing the Affiliate page | Affiliate I through V sections and blocks | ☐ To record |
| 6 | Store Locator overview | Locator I heading + Stockist dashboard for locations | ☐ To record |
| 7 | Editing a Product (basics) | Title, media, price, variants in Products | ☐ To record |
| 8 | Editing Product Metafields | `pdp_intro`, colors, ingredients, nutrition facts | ☐ To record |
| 9 | Footer & Navigation | Foot section + Navigation menus | ☐ To record |
| 10 | Reviewing form submissions | Shopify Admin → Customers or Notifications inbox | ☐ To record |

**Video hosting:** To be provided by JAG Provisions / Wagon

---

## Need developer help?

Contact your developer when you need to change:

- Form field labels, placeholders, or validation messages
- PDP II / PDP III hardcoded headings and tab body copy
- Stockist widget tag or script settings
- New sections, pages, or layout changes
- Animation behavior or new carousel configurations

---

*Last updated: June 2026 · Theme files referenced from local codebase audit*
