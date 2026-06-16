# Product Management Guide — Just Nosh

This guide explains how to add, edit, archive, and publish products on eatjustnosh.com — including product images, variants, and the custom metafields that power the product detail page (PDP).

**Related documents:** `02-cms-editing-guide.md` · `01-client-handoff-index.md` · `06-assets-inventory.md`

---

## Overview

On this site, a product is more than a title and price. Each flavor uses **Shopify product metafields** (namespace `custom`) to control PDP colors, intro text, ingredients, nutrition facts, shop card badges, and hover images.

| What | Where to edit |
|---|---|
| Product name, price, media, variants | **Products** in Shopify Admin |
| PDP intro, colors, ingredients, nutrition | **Products → Metafields** |
| Shop grid headings, collection assignment | **Theme Editor** (Shopping, Shop, Minishop sections) |
| Product order on shop pages | **Products → Collections** |

---

## 1. How to add a new product

### Step-by-step

1. In Shopify Admin, go to **Products**
2. Click **Add product**
3. Fill in the basics:
   - **Title** — flavor name (e.g. “Peanut Butter Chocolate”)
   - **Description** — full product description (used as fallback if `pdp_intro` metafield is empty)
   - **Media** — upload product images (see [Image guidelines](#image-guidelines))
   - **Pricing** — set price; add compare-at price if on sale
   - **Inventory** — set stock tracking as needed
4. Scroll to **Metafields** and complete all required fields (see [Product metafields reference](#product-metafields-reference))
5. Under **Product organization**:
   - Add **Product type**, **Vendor**, and **Tags** as your team uses them
   - Assign the product to the correct **Collections** (e.g. Shop, frontpage)
6. Set **Status** to **Active** when ready to publish
7. Click **Save**
8. Open the product on the live site and verify the PDP, shop card, and cart display

### After creating the product

- Add it to the collections used on the homepage, shop page, and cart (see [Section 3 — Reordering products](#3-how-to-reorder-products-via-collections))
- If the product has subscription options, configure plans in the **Seal Subscriptions** app (see `05-third-party-services.md`)

---

## 2. How to edit existing products

1. Go to **Products** in Shopify Admin
2. Click the product name
3. Edit any of the following:

| Field | What it affects on the site |
|---|---|
| **Title** | Product name on PDP, shop cards, cart, and browser tab |
| **Description** | Fallback intro text on PDP if `pdp_intro` metafield is empty |
| **Media** | Featured image on shop grids; gallery images on PDP carousel |
| **Pricing** | Price shown on PDP and shop cards |
| **Variants** | Flavor/size options, per-variant pricing and images |
| **Metafields** | PDP colors, intro, highlights, ingredients, nutrition, shop badges |
| **Collections** | Which shop grids and pages show this product |
| **Status** | Active, Draft, or Archived |

4. Click **Save**
5. Preview the product page on the storefront

> **Tip:** For text and images specific to the custom PDP layout, metafields are usually the right place — not the Theme Editor.

---

## 3. How to reorder products via collections

Product order on shop grids is controlled by **collections**, not by drag-and-drop on the homepage.

### Where collections are used

| Page / section | Section name | Collection setting |
|---|---|---|
| Homepage | **Shopping** | Collection picker (falls back to `frontpage` if empty) |
| Shop / Collection page | **Shop** | Collection picker (falls back to `frontpage` if empty) |
| About page | **Minishop** | Collection picker (falls back to `frontpage` if empty) |
| Cart page | **Shop** | Collection picker (falls back to `frontpage` if empty) |

Template files: `templates/index.json`, `templates/collection.json`, `templates/page.about.json`, `templates/cart.json`

### How to change product order

1. Go to **Products → Collections**
2. Open the collection used on your shop pages (confirm which one is selected in the Theme Editor, or use **frontpage** if none is set)
3. In the **Products** area, drag products into the order you want
4. Click **Save**
5. Refresh the homepage or shop page to confirm

### Sort order types

In **Products → Collections → [collection] → Product sorting**, you can choose:

| Sort type | When to use |
|---|---|
| **Manual** | Full control — drag to reorder (recommended for Just Nosh flavor lineup) |
| **Best selling** | Automatic by sales |
| **Created** | Newest or oldest first |
| **Price** | Low to high or high to low |

For the flavor grid, **Manual** sort is recommended so the lineup matches your merchandising plan.

### Products limit per section

| Section | Max products shown |
|---|---|
| Shopping (homepage) | 2–12 (setting: “Products to show”) |
| Shop (collection page) | Up to 12 (default in template) |
| Minishop (about) | Up to 20 (shows 3 random products from collection) |
| PDP Recommendations | 2 or 3 related products |

---

## 4. How to archive products correctly

Use **Draft** or **Archived** status — do not delete products that have order history.

### Option A — Draft (temporary)

Best when a flavor is out of stock or being updated.

1. Open the product in **Products**
2. Change **Status** from **Active** to **Draft**
3. Click **Save**

**Result:** Product is hidden from the storefront but remains in Admin and order history.

### Option B — Archive (discontinued)

Best when a flavor is permanently discontinued.

1. Open the product in **Products**
2. Click **⋯** (More actions) → **Archive product**
3. Confirm

**Result:** Product is removed from the storefront and moved to the **Archived** tab in Products.

### After archiving — checklist

- [ ] Remove the product from active **Collections** (or drag to bottom if keeping for reference)
- [ ] Check homepage **Shopping**, **Shop**, and **Minishop** sections still look correct
- [ ] Update navigation or featured links if they pointed to this product
- [ ] Disable related subscription plans in **Seal Subscriptions** if applicable

### Do not

- Delete products with past orders (breaks order records)
- Leave discontinued products **Active** in collections

---

## 5. How to update PDP images

The PDP uses images from two sources: **Product Media** (Shopify gallery) and **metafields** (ingredients and nutrition).

### A. Main product gallery (PDP I carousel)

**Where:** Products → [product] → **Media**

**How the theme uses media:**

| Position in gallery | Used for |
|---|---|
| **1st image** | **Featured image** on shop cards, cart, and recommendations (`product.featured_image`). This image is **not shown** in the PDP carousel. |
| **2nd image onward** | Shown in the PDP image carousel and thumbnails (`sections/pdp-i.liquid`) |

> **Important:** The theme skips the first media item in the PDP carousel. Always upload at least **two images** per product: one for shop grids (position 1) and at least one for the PDP gallery (position 2+).

**To update gallery images:**

1. Go to **Products → [product]**
2. In **Media**, drag images to reorder (position 1 = shop card image)
3. Click an image to replace it, or click **Add media** to upload new images
4. Add **alt text** to each image for accessibility
5. Save and preview the PDP

**Recommended specs for gallery images:**

| Use | Suggested size | Format |
|---|---|---|
| Shop card (image 1) | 900 × 900 px minimum, square or consistent ratio | JPG or PNG |
| PDP carousel (images 2+) | 1200 px wide minimum | JPG or PNG |
| Thumbnails | Auto-generated from uploads | — |

### B. Key ingredients image (PDP II)

**Where:** Products → [product] → **Metafields**

| Metafield | Key | Device |
|---|---|---|
| Key ingredients image | `custom.key_ingredients_image` | Desktop |
| Key ingredients image (responsive) | `custom.key_ingredients_image_responsive` | Mobile |

Upload a single image file to each metafield. Theme renders up to 1400 px wide.

### C. Nutrition facts images (PDP III)

**Where:** Products → [product] → **Metafields**

| Metafield | Key |
|---|---|
| Nutritional facts files | `custom.nutritional_facts_files` |

This is a **list of files** — upload one or more nutrition label images. The theme displays up to 4 images in the “Nutrition Facts” tab. Recommended width: 1600 px.

### D. Shop card hover images (not on PDP)

**Where:** Products → [product] → **Metafields**

| Metafield | Key |
|---|---|
| Shopping media | `custom.shopping_media` |

List of up to 4 images that animate on hover in shop grids (Homepage **Shopping**, **Shop**, **PDP Recommendations**). These are separate from the main product gallery.

---

## 6. How to handle variants

The theme supports products with multiple variants (e.g. different pack sizes or flavors as variants of one product).

### When variants appear

- If a product has **more than one variant**, PDP I shows option selectors (radio buttons) for each option (`sections/pdp-i.liquid`)
- Price updates automatically when the customer selects a variant
- If the carousel is active, it jumps to the image linked to the selected variant

### How to set up variants

1. Open the product in **Products**
2. In **Variants**, click **+ Add options** (e.g. “Size”, “Flavor”)
3. Add option values (e.g. “12 bars”, “24 bars”)
4. For each variant, set:
   - **Price**
   - **SKU** (if used)
   - **Inventory**
   - **Image** — assign a specific media item to each variant for carousel sync
5. Save

### Assigning images to variants

1. Upload all images in **Media** first
2. Scroll to **Variants**
3. Click a variant → assign its **Image** from the media library
4. The theme places the variant’s image first in the sort order and syncs the carousel on selection

### Single-variant products

Most Just Nosh flavors are likely **single-variant products** (one flavor = one product). In that case, no variant picker appears — just price and add-to-cart.

### Subscriptions

If **Seal Subscriptions** is enabled for a product, subscription options appear in the buy area below the variant picker. Manage subscription plans in the Seal Subscriptions app — not in the theme.

---

## 7. Product metafields reference

All custom metafields use the namespace **`custom`**. In Shopify Admin they appear under the product’s **Metafields** section.

> **Note:** Metafield definitions (type, validation) are configured in Shopify Admin under **Settings → Custom data → Products**. Exact field labels in Admin may differ slightly from the keys below.

### Complete metafield table

| Name (key) | Namespace.key | Type (expected) | Where it appears |
|---|---|---|---|
| Bars | `custom.bars` | Single line text | Badge on shop cards (e.g. “12 BARS”), cart line items |
| Shopping media | `custom.shopping_media` | List of files (images) | Hover images on shop product cards |
| PDP intro | `custom.pdp_intro` | Single line or multi-line text | Short intro under price on PDP I |
| Flavor profile title | `custom.flavor_profile_title` | Single line text | Flavor profile box label on PDP I |
| Flavor profile text | `custom.flavor_profile_text` | Multi-line text | Flavor profile description on PDP I |
| Price suffix label | `custom.price_suffix_label` | Single line text | Text after price (e.g. “/ 12 bars”) on PDP I |
| PDP highlights | `custom.pdp_highlights` | List of single line text | Benefit chips on PDP I |
| Color primary | `custom.color_primary` | Color | PDP I background color |
| Color secondary | `custom.color_secondary` | Color | Flavor profile box background on PDP I |
| Color tertiary | `custom.color_tertiary` | Color | Text, borders, and buttons on PDP I |
| Color tertiary contrast | `custom.color_tertiary_contrast` | Color | Button text color on PDP I |
| Color light | `custom.color_light` | Color | PDP III inner panel background |
| Key ingredients | `custom.key_ingredients` | List of single line text | Ingredient names in PDP II (left/right columns) |
| Key ingredients image | `custom.key_ingredients_image` | File (image) | Center image on PDP II (desktop) |
| Key ingredients image (responsive) | `custom.key_ingredients_image_responsive` | File (image) | Center image on PDP II (mobile) |
| Nutritional facts files | `custom.nutritional_facts_files` | List of files (images) | “Nutrition Facts” tab on PDP III |

### Theme files that read each metafield

| Metafield | Theme file(s) |
|---|---|
| `bars` | `sections/shopping.liquid`, `sections/shop.liquid`, `sections/pdp-recommendations.liquid`, `snippets/cart-products.liquid` |
| `shopping_media` | `sections/shopping.liquid`, `sections/shop.liquid`, `sections/pdp-recommendations.liquid`, `sections/minishop.liquid` |
| `pdp_intro`, `flavor_profile_*`, `price_suffix_label`, `pdp_highlights`, `color_*` | `sections/pdp-i.liquid` |
| `key_ingredients`, `key_ingredients_image`, `key_ingredients_image_responsive` | `sections/pdp-ii.liquid` |
| `nutritional_facts_files` | `sections/pdp-iii.liquid` |

### How to edit metafields

1. Go to **Products → [product]**
2. Scroll to the **Metafields** section (or click **Show all metafields**)
3. Find the field by name
4. Enter or upload the value
5. Click **Save**

### Copying metafields to a new flavor

When launching a new flavor based on an existing one:

1. **Duplicate** the existing product (Products → ⋯ → Duplicate)
2. Update **Title**, **Media**, and flavor-specific metafields
3. Update **color_*** metafields to match the new flavor’s palette
4. Update `key_ingredients`, `nutritional_facts_files`, and `pdp_intro`
5. Save and preview

### PDP sections with hardcoded copy (not metafields)

These PDP areas are **not** editable via metafields or Theme Editor:

| Section | Hardcoded content |
|---|---|
| PDP II | Heading “KEY INGREDIENTS” and intro paragraph |
| PDP III | Tab labels; “Why Collagen Protein” and “Seed Oil Free Certified” tab body copy |

Contact your developer to change this text.

---

## 8. Image guidelines

### Consistent size and ratio

| Image role | Recommendation |
|---|---|
| Shop card (media position 1) | Square or consistent aspect ratio across all flavors; min. 900 × 900 px |
| PDP gallery (media position 2+) | Same ratio across all flavors; min. 1200 px wide |
| Ingredients image | Portrait or square, min. 1200 px wide; provide desktop + mobile versions |
| Nutrition facts | Clear, readable label photo; min. 1200 px wide |
| Shopping media (hover) | Small decorative product shots; up to 500 px wide |

Using the same aspect ratio for all flavors keeps the shop grid aligned.

### Clear file names

Before uploading, rename files descriptively:

```
justnosh-peanut-butter-chocolate-shop.jpg
justnosh-peanut-butter-chocolate-gallery-01.jpg
justnosh-peanut-butter-chocolate-nutrition.jpg
justnosh-peanut-butter-chocolate-ingredients-desktop.jpg
```

Good names make files easier to find in **Content → Files** and in the media library.

### Compression

- Export JPGs at **80–85% quality** for photos
- Use **PNG** only for images that need transparency
- Aim for **under 500 KB** per shop card image and **under 1 MB** per gallery image
- Tools: TinyPNG, Squoosh, or Shopify’s built-in image optimization (automatic on upload)

### Image order (media gallery)

Recommended order for a typical flavor:

| Position | Purpose | Shows on |
|---|---|---|
| 1 | Pack shot / shop card hero | Shop grids, cart, recommendations |
| 2 | Primary PDP lifestyle or angle shot | PDP carousel |
| 3+ | Additional angles, detail shots, context | PDP carousel |
| — | Ingredients image | `key_ingredients_image` metafield (not in media gallery) |
| — | Nutrition label | `nutritional_facts_files` metafield (not in media gallery) |
| — | Hover decor shots | `shopping_media` metafield (not in media gallery) |

### Alt text

Add descriptive alt text to every image in **Media** and metafield uploads:

- Good: `JustNosh Peanut Butter Chocolate protein bar – 12 pack`
- Avoid: `IMG_4521.jpg`

---

## 9. Checklist before publishing a product

Use this checklist every time you launch or major-update a flavor.

### Basic product info

- [ ] Title is correct and matches packaging
- [ ] Price is correct
- [ ] Compare-at price set (if on sale)
- [ ] Inventory / SKU configured
- [ ] Status is **Active**
- [ ] Product is added to the correct **Collection(s)**

### Media

- [ ] At least **2 images** in Media (1 for shop card + 1 for PDP carousel)
- [ ] Image 1 is the correct shop card / pack shot
- [ ] Images 2+ are gallery shots for the PDP carousel
- [ ] All images have alt text
- [ ] Images are compressed and consistently sized

### Metafields — required for full PDP

- [ ] `pdp_intro` — short intro text
- [ ] `flavor_profile_title` and `flavor_profile_text`
- [ ] `pdp_highlights` — benefit chips filled in
- [ ] `color_primary`, `color_secondary`, `color_tertiary`, `color_tertiary_contrast`, `color_light`
- [ ] `price_suffix_label` (if showing “/ 12 bars” or similar)
- [ ] `bars` — badge text (e.g. “12 BARS”)
- [ ] `key_ingredients` — ingredient list
- [ ] `key_ingredients_image` and `key_ingredients_image_responsive`
- [ ] `nutritional_facts_files` — nutrition label image(s)
- [ ] `shopping_media` — hover images for shop card (optional but recommended)

### Variants & subscriptions

- [ ] Variants configured correctly (if more than one)
- [ ] Variant images assigned (if using variants)
- [ ] Seal Subscriptions plans active (if offering subscribe & save)

### Storefront verification

- [ ] PDP I — title, price, intro, highlights, flavor profile, colors, gallery, add-to-cart work
- [ ] PDP II — ingredients list and center image display
- [ ] PDP III — nutrition facts tab shows label image(s)
- [ ] Shop card appears on homepage **Shopping** grid with badge and hover images
- [ ] Product appears in correct order in collection
- [ ] Cart shows product name, image, and `bars` badge
- [ ] Mobile layout checked

### SEO (optional but recommended)

- [ ] Search engine listing has a clear page title and meta description
- [ ] URL handle is clean (e.g. `/products/peanut-butter-chocolate`)

---

## Quick troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| PDP carousel is empty | Only 1 image in Media | Add a 2nd image (position 2+) |
| Wrong image on shop card | Image order in Media | Drag the pack shot to position 1 |
| PDP shows wrong colors | Missing or incorrect `color_*` metafields | Update metafields for this product |
| No “12 BARS” badge | `bars` metafield empty | Fill in `custom.bars` |
| No hover effect on shop card | `shopping_media` empty | Upload images to `custom.shopping_media` |
| Ingredients section empty | `key_ingredients` not filled | Add ingredient list metafield |
| Nutrition tab empty | `nutritional_facts_files` empty | Upload nutrition label image(s) |
| Product not on homepage | Not in collection | Add to collection used by **Shopping** section |
| Product still visible after discontinue | Still Active in collection | Archive product and remove from collections |

---

## Need developer help?

Contact your developer for:

- New metafield definitions
- Changes to PDP hardcoded copy (PDP II / PDP III)
- New flavor color system or layout changes
- Seal Subscriptions integration issues
- Products with complex variant structures

---

*Last updated: June 2026 · Metafield keys verified from theme files in `sections/pdp-i.liquid`, `pdp-ii.liquid`, `pdp-iii.liquid`, `shopping.liquid`, `shop.liquid`, `pdp-recommendations.liquid`, `minishop.liquid`, and `snippets/cart-products.liquid`*
