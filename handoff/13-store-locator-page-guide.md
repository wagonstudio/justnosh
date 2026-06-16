# Store Locator Page Guide — Just Nosh

**Page:** Store Locator  
**Expected URL:** `/pages/store-locator`  
**Theme template:** `locator` (`templates/page.locator.json`)  
**Theme Editor path:** Online Store → Themes → Customize → Pages → Store Locator

**Related documents:** `07-store-locator-guide.md` · `02-cms-editing-guide.md` · `05-third-party-services.md`

---

# Overview

The Store Locator page helps customers find Just Nosh products at retail locations via an interactive map, and provides a contact form for store requests or partnership inquiries. The map data is **not stored in Shopify** — it is managed in **Stockist**.

---

# Page Structure

Sections in exact order per `templates/page.locator.json` → `"order"`:

| # | Theme Editor name | Section type | File | Visible on live site |
|---|---|---|---|---|
| 1 | **Locator I** | `locator-i` | `sections/locator-i.liquid` | Yes |
| 2 | **Locator II** | `locator-ii` | `sections/locator-ii.liquid` | Yes |

**Note:** The second section uses JSON key `"main"` but Theme Editor displays it as **Locator II** (schema `name: "Locator II"`).

No disabled Heritage sections on this template.

Global **Header** and **Foot** footer render on every page. Footer CTA links to this page (`shopify://pages/store-locator` in `sections/footer-group.json`).

> **Needs confirmation:** Page exists, is published, uses template **locator**, handle is `store-locator`.

---

# Editable Content

## Locator I

| Setting | Theme Editor label | Type | Live value (template) |
|---|---|---|---|
| Heading | Heading | Text | `FIND JUSTNOSH NEAR YOU` |
| Stockist widget tag | Stockist widget tag | Text | `map_r3mn7jvq` |
| Load Stockist script | Load Stockist script | Checkbox | `true` |

The map UI itself (search, pins, location details) is **not** editable in Theme Editor — managed in Stockist.

## Locator II

| Setting | Theme Editor label | Type | Live value (template) |
|---|---|---|---|
| Heading | Heading | Text | `DON'T SEE JUSTNOSH AT A STORE NEAR YOU?` |
| Subheading | Subheading | Textarea | Store request / partnership copy |
| Button label | Button label | Text | `CONTACT US` |
| Email note | Email note | Rich text | Link to `hello@jagprovisions.com` |

**Not editable in Theme Editor:** Form field labels, placeholders, success message, hidden field values.

---

# Images

## Editable images (Theme Editor)

**None.** Neither section has image picker settings.

## Map / location visuals

| Source | Content |
|---|---|
| **Stockist** | Map tiles, location pins, store details, search UI |
| Stockist dashboard | Store photos — **Needs confirmation** if used |

## Hardcoded images

**None** in theme sections. Stockist widget may load its own assets from `stockist.co`.

## Image sources summary

| Source | Used for |
|---|---|
| Stockist embed | Entire map experience |
| Theme CSS | Page layout only — no content images |

---

# Forms

## Form — Locator II (store request / inquiry)

| Property | Value |
|---|---|
| Type | Shopify `{% form 'contact' %}` |
| Class | `form-contact` |
| File | `sections/locator-ii.liquid` |

### Fields

| Label (hardcoded) | `name` attribute | Required | Placeholder (hardcoded) |
|---|---|---|---|
| Name | `contact[name]` | Yes | `John` |
| Email | `contact[email]` | Yes | `name@gmail.com` |
| Message | `contact[body]` | Yes | Store request / partnership placeholder text |

### Hidden fields

| Field | Value |
|---|---|
| `contact[form_success_id]` | `locator-{{ section.id }}` |
| `contact[form_type]` | `Store locator request` |
| `contact[category]` | `Store locator inquiry` |

### Submission behavior

1. Submits to Shopify Contact API
2. Success via `sessionStorage` in `assets/wagon.js` (same pattern as Contact I)
3. Success message (hardcoded): *"Thank you! Your message has been sent successfully. We'll get back to you as soon as possible."*
4. Required field note (hardcoded): "* Indicates a required field"

> **Needs confirmation:** Notification email routing in Shopify Admin.

**No form** on Locator I (map only).

---

# Dependencies

| Dependency | Used by | Notes |
|---|---|---|
| **Stockist** | Locator I | Widget tag `map_r3mn7jvq`; script `https://stockist.co/embed/v1/widget.min.js` |
| Shopify Contact Form API | Locator II | Native form |
| `assets/wagon.js` | Locator II form success | `sessionStorage` pattern |
| `assets/wagon.css` | Both sections | `.section-locator-i`, `.locator-ii` (reuses contact form styles) |
| Lenis smooth scroll | Locator I | `data-lenis-prevent` on widget shell — prevents scroll capture inside map |

### Stockist

| Item | Value |
|---|---|
| Custom element | `<stockist-store-locator>` |
| Widget tag (live) | `map_r3mn7jvq` |
| Script load | Conditional on `load_stockist_script` setting |
| Location data | Stockist dashboard only |

**Needs confirmation:** Stockist account login, plan, and widget ownership.

### Not the Store Locator

Shopify **local pickup** in `blocks/buy-buttons.liquid` is checkout pickup — unrelated to this page.

---

# Hardcoded Content

Requires a **developer** to change:

| Content | File |
|---|---|
| Stockist loading fallback text and link | `sections/locator-i.liquid` |
| `<stockist-store-locator>` markup | `sections/locator-i.liquid` |
| Form labels, placeholders, success message | `sections/locator-ii.liquid` |
| Hidden `form_type` and `category` values | `sections/locator-ii.liquid` |
| `data-lenis-prevent` attribute | `sections/locator-i.liquid` |
| Locator II uses CSS class `contact-i` on section element | `sections/locator-ii.liquid` |
| Map container layout | `assets/wagon.css` |

---

# Maintenance Notes

### Safe for client to update

| Task | Where |
|---|---|
| Add / edit / remove store locations | **Stockist dashboard** |
| Change page heading above map | Theme Editor → Locator I → Heading |
| Change form copy and email note | Theme Editor → Locator II |
| Update widget tag (if Stockist account changes) | Theme Editor → Locator I → Stockist widget tag |

### Use caution

- Disabling **Load Stockist script** breaks the map
- Changing widget tag without updating Stockist account breaks the map
- Email note references `hello@jagprovisions.com` — verify correct address

### Requires developer

- Replace Stockist with another map provider
- Change form fields or submission behavior
- Modify map container layout or Lenis interaction

### Client cannot update via Shopify

- Store addresses, coordinates, hours, retailer names (Stockist only)
- Map pin styling beyond Stockist dashboard options — **Needs confirmation** (Stockist plan features)

---

# Developer Notes

- Template: `templates/page.locator.json`
- Locator II section JSON key is `"main"` — unusual naming; type is `locator-ii`
- Stockist script loads **only** on pages with Locator I when checkbox enabled — not global in `layout/theme.liquid`
- Locator II reuses contact page form markup/classes (`.field-affiliate`, `.form-contact`, `.note-affiliate`)
- Footer link: `sections/footer-group.json` → CTA `shopify://pages/store-locator`
- No metafields or metaobjects used for locations
- No Google Maps / Mapbox integration

For step-by-step Stockist workflows, see [`07-store-locator-guide.md`](07-store-locator-guide.md).

---

*Last updated: June 2026 · Audit source: `templates/page.locator.json`, `sections/locator-i.liquid`, `sections/locator-ii.liquid`*
