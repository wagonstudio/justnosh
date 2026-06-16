# Affiliate Page Guide — Just Nosh

**Page:** Affiliate  
**Expected URL:** `/pages/affiliate`  
**Theme template:** `affiliate` (`templates/page.affiliate.json`)  
**Theme Editor path:** Online Store → Themes → Customize → Pages → Affiliate

**Related documents:** `02-cms-editing-guide.md` · `05-third-party-services.md`

---

# Overview

The Affiliate page recruits partners (creators, dietitians, wellness professionals) and collects applications. It explains who the program is for, benefits, how it works, and includes an application form. Content is built from five custom sections. The Shopify page body field is **not used** on the live site.

Affiliate **tracking and program management** is handled by the **UpPromote Affiliate** app (global app block in theme settings), not by visible UI on this page.

---

# Page Structure

Sections in exact order per `templates/page.affiliate.json` → `"order"`:

| # | Theme Editor name | Section type | File | Visible on live site |
|---|---|---|---|---|
| 1 | **Affiliate I** | `affiliate-i` | `sections/affiliate-i.liquid` | Yes |
| 2 | **Affiliate II** | `affiliate-ii` | `sections/affiliate-ii.liquid` | Yes |
| 3 | **Affiliate III** | `affiliate-iii` | `sections/affiliate-iii.liquid` | Yes |
| 4 | **Affiliate IV** | `affiliate-iv` | `sections/affiliate-iv.liquid` | Yes |
| 5 | **Affiliate V** | `affiliate-v` | `sections/affiliate-v.liquid` | Yes |

No disabled Heritage `main-page` section on this template.

Global **Header** and **Foot** footer render on every page.

> **Needs confirmation:** Page exists, is published, uses template **affiliate**, and URL matches navigation links.

---

# Editable Content

## Affiliate I

| Setting | Theme Editor label | Type | Live value (template) |
|---|---|---|---|
| Heading | Heading | Rich text | Partner headline with `<strong>` emphasis |
| Text | Text | Rich text | Intro paragraph |
| Button label | Button label | Text | `BECOME AN AFFILIATE` |
| Button link | Button link | URL | `#affiliate` (scroll to form) |

Schema internal name: **Affiliate** (Theme Editor shows **Affiliate I** from template JSON).

## Affiliate II

| Setting / block | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text — `WHO THIS IS FOR` |
| Subheading | Subheading | Textarea |
| Image | Image | Image picker |
| Image alt text | Image alt text | Text |
| Blocks: **List item** | Text | Up to 6 blocks |

## Affiliate III

| Setting / block | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text — `WHY PARTNER WITH JUSTNOSH` |
| Blocks: **Item** | Icon, Text | Up to 4 blocks |

## Affiliate IV

| Setting / block | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text — `HOW IT WORKS` |
| Blocks: **Step** | Number, Text | Up to 3 blocks |

## Affiliate V

| Setting | Theme Editor label | Type |
|---|---|---|
| Heading | Heading | Text — `APPLY TO BECOME AN AFFILIATE` |
| Text | Text | Textarea |
| Bar image 1–3 | Bar image 1–3 | Image picker |
| Image alt text | Image alt text | Text |
| Button label | Button label | Text — `SUBMIT APPLICATION` |
| Email note | Email note | Rich text — `hello@japrovisions.com` |

Section has HTML `id="affiliate"` for in-page anchor from Affiliate I CTA.

---

# Images

## Editable images (Theme Editor)

| Section | Slots | Source |
|---|---|---|
| Affiliate II | 1 context image | Section `image` picker |
| Affiliate III | 4 block icons | Block `icon` picker |
| Affiliate V | 3 bar images | Section `image_1`, `image_2`, `image_3` |

## Current live images (from template)

| Section | File |
|---|---|
| Affiliate II | `JustNosh-Context-Image-02.jpg` |
| Affiliate III | `sheet.svg`, `heart.svg`, `microphone.svg`, `medal.svg` |
| Affiliate V | `3a77e42014d92935027022cb38e7b3c863b4a063.png`, `2cd7c54b442dd25b92ecc58009f6be8cc25465dc.png`, `02baf466d338bceb30ac7d64c5c807470d45df16.png` |

Paths: `shopify://shop_images/...`

## Hardcoded images

| Item | Location | URL / notes |
|---|---|---|
| Checkmark icon (Affiliate II list) | `sections/affiliate-ii.liquid` | `https://cdn.shopify.com/s/files/1/0802/9647/6893/files/la_check.svg?v=1777399790` — **not** Theme Editor configurable |
| Bar image positions (Affiliate V) | `assets/wagon.css` | `.bar-1`, `.bar-2`, `.bar-3` absolute positioning |
| Bar image alt text in markup | `sections/affiliate-v.liquid` | Hardcoded `alt="bar 1"`, `bar 2`, `bar 3` — ignores `image_alt` setting for bar figures |

## Image sources summary

| Source | Used for |
|---|---|
| Theme Editor image pickers | Context photo, benefit icons, bar PNGs |
| Shopify Files | Hosted assets |
| Shopify CDN (hardcoded URL) | List checkmark icon in Affiliate II |

---

# Forms

## Form — Affiliate V (application)

| Property | Value |
|---|---|
| Type | Shopify `{% form 'contact' %}` |
| Class | `form-affiliate` |
| File | `sections/affiliate-v.liquid` |
| Anchor | Section `id="affiliate"` |

### Fields

| Label (hardcoded) | `name` attribute | Required | Placeholder (hardcoded) |
|---|---|---|---|
| Name | `contact[name]` | No* | `John` |
| Email | `contact[email]` | Yes | `name@gmail.com` |
| Social handle or website | `contact[social_handle_or_website]` | Yes | `@yourhandle or yourwebsite.com` |
| Tell us more about you | `contact[body]` | Yes | Long placeholder about partnership story |
| Preferred coupon code (optional) | `contact[preferred_coupon_code]` | No | `Your preferred code` |

*Label shows asterisk on Name but input has no `required` attribute — inconsistent markup.

### Hidden fields

| Field | Value |
|---|---|
| `contact[form_type]` | `Affiliate application` |

**Note:** No `contact[form_success_id]` — uses Liquid `form.posted_successfully?` instead of `sessionStorage` pattern.

### Submission behavior

1. Submits to Shopify Contact API
2. On success: server-side `{% if form.posted_successfully? %}` shows success message, hides button
3. Success message (hardcoded): *"Thank you! Your message has been sent successfully. We'll get back to you as soon as possible."*
4. Does **not** use `assets/wagon.js` sessionStorage pattern

> **Needs confirmation:** Notification email recipient in Shopify Admin → Settings → Notifications.

---

# Dependencies

| Dependency | Used by | Notes |
|---|---|---|
| Shopify Contact Form API | Affiliate V form | Native |
| **UpPromote Affiliate** | Site-wide affiliate tracking | App block in `config/settings_data.json` — `shopify://apps/uppromote-affiliate/blocks/core-script/...`, `disabled: false` |
| `assets/wagon.js` | Affiliate I CTA | `data-anim="fade-up"` scroll animation |
| `assets/wagon.css` | All affiliate sections | Layout, form, bar positioning |

### UpPromote

- Registered as global theme app embed — not rendered inside affiliate section Liquid
- Program links, commissions, and affiliate dashboard: **UpPromote app admin** — **Needs confirmation** (account access)
- Step 3 in Affiliate IV mentions "10% commissions" — marketing copy in Theme Editor, not enforced by theme

---

# Hardcoded Content

Requires a **developer** to change:

| Content | File |
|---|---|
| All form labels and placeholders | `sections/affiliate-v.liquid` |
| Required field note | `sections/affiliate-v.liquid` |
| Form success message | `sections/affiliate-v.liquid` |
| Checkmark icon URL (Affiliate II) | `sections/affiliate-ii.liquid` |
| Bar `alt` attributes (`bar 1`, `bar 2`, `bar 3`) | `sections/affiliate-v.liquid` |
| Section `id="affiliate"` anchor | `sections/affiliate-v.liquid` |
| Bar CSS positions | `assets/wagon.css` |
| Affiliate IV schema CSS class typo: `section-affiliate-v` on section IV | `sections/affiliate-iv.liquid` |

---

# Maintenance Notes

### Safe for client to update

- All headings, text, and rich text in Affiliate I–IV
- List items (Affiliate II), benefit items (Affiliate III), steps (Affiliate IV)
- Context image (Affiliate II), icons (Affiliate III), bar images (Affiliate V)
- Button label and email note (Affiliate V)
- Affiliate I CTA link (currently `#affiliate`)
- Commission rate copy in Affiliate IV step 3 (verify matches UpPromote settings)

### Use caution

- Email note uses `hello@japrovisions.com` — differs from Contact/Locator (`hello@jagprovisions.com`) — **Needs confirmation**
- UpPromote must stay installed for affiliate link tracking to work
- Application form does not create UpPromote accounts automatically — **Needs confirmation** (manual workflow)

### Requires developer

- Change form fields or success behavior
- Replace hardcoded checkmark icon
- Fix Name field required state vs label asterisk
- Wire `image_alt` setting to bar figure alt attributes

---

# Developer Notes

- Template: `templates/page.affiliate.json`
- Affiliate V schema class: `section-affiliate-vi` (preset name "Affiliate VI") — naming inconsistency with section type `affiliate-v`
- Two form success patterns on site: Affiliate V uses Liquid; Contact/Locator use `sessionStorage` in `wagon.js`
- Custom contact fields `social_handle_or_website`, `preferred_coupon_code` appear in notification email body as part of Shopify contact submission
- Affiliate I button uses `data-anim="fade-up"` — requires `assets/wagon.js` GSAP init
- No forms on Affiliate I–IV sections

---

*Last updated: June 2026 · Audit source: `templates/page.affiliate.json`, `sections/affiliate-*.liquid`, `config/settings_data.json`*
