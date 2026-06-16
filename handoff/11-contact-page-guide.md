# Contact Page Guide — Just Nosh

**Page:** Contact  
**Expected URL:** `/pages/contact`  
**Theme template:** `contact` (`templates/page.contact.json`)  
**Theme Editor path:** Online Store → Themes → Customize → Pages → Contact

**Related documents:** `02-cms-editing-guide.md` · `05-third-party-services.md`

---

# Overview

The Contact page provides a general inquiry form and a newsletter signup block with decorative ingredient graphics. All visible content comes from two custom sections. The Shopify page body field is **not used** on the live site.

---

# Page Structure

Sections in exact order per `templates/page.contact.json` → `"order"`:

| # | Theme Editor name | Section type | File | Visible on live site |
|---|---|---|---|---|
| 1 | *(Heritage main page)* | `main-page` | `sections/main-page.liquid` | **No** — `disabled: true` |
| 2 | **Contact I** | `contact-i` | `sections/contact-i.liquid` | Yes |
| 3 | **Contact II** | `contact-ii` | `sections/contact-ii.liquid` | Yes |

Global **Header** and **Foot** footer render on every page.

> **Needs confirmation:** Page exists, is published, and uses theme template **contact**.

---

# Editable Content

## Contact I

| Setting | Theme Editor label | Type | Live value (template) |
|---|---|---|---|
| Heading | Heading | Text | `CONTACT US` |
| Subheading | Subheading | Textarea | Help / feedback intro copy |
| Button label | Button label | Text | `CONTACT US` |
| Email note | Email note | Rich text | Link to `hello@jagprovisions.com` |

## Contact II

| Setting | Theme Editor label | Type | Live value (template) |
|---|---|---|---|
| Heading | Heading | Textarea | `WANT EXCLUSIVE OFFERS AND PRODUCT UPDATES?` |
| Text | Text | Text | `Unsubscribe anytime.` |
| Button label | Button label | Text | `Subscribe` |
| Decor image 1–5 | Decor image 1–5 | Image picker | SVG decor assets |
| Decor image 1–5 alt | Decor image alt fields | Text | Empty in live template |

**Not editable in Theme Editor:** Form field labels, placeholders, required-field note, success messages (hardcoded in Liquid).

---

# Images

## Editable images (Theme Editor)

| Section | Slots | Source |
|---|---|---|
| Contact II | Decor image 1–5 + alt text | Section image pickers → Shopify Files |

## Current live images (Contact II)

| Slot | File |
|---|---|
| Decor image 1 | `mani-2.svg` |
| Decor image 2 | `mani-1.svg` |
| Decor image 3 | `chocolate-salt-1.svg` |
| Decor image 4 | `chocolate-salt-2.svg` |
| Decor image 5 | `lemon-1.svg` |

Paths: `shopify://shop_images/...`

## Hardcoded images

| Item | Location | Notes |
|---|---|---|
| Decor positions (5 figures) | `assets/wagon.css` | CSS classes `.decor-contact-1` … `.decor-contact-5` — swap images OK; repositioning needs developer |
| Dark background band below card | `sections/contact-ii.liquid` + `assets/wagon.css` | Pseudo-element layout |

## Image sources summary

| Source | Used for |
|---|---|
| Theme Editor (Contact II) | Decorative ingredient SVGs |
| Shopify Files | Hosted SVG assets |
| Contact I | No images |

---

# Forms

## Form 1 — Contact I (general inquiry)

| Property | Value |
|---|---|
| Type | Shopify `{% form 'contact' %}` |
| Class | `form-contact` |
| File | `sections/contact-i.liquid` |

### Fields

| Label (hardcoded) | `name` attribute | Required | Placeholder (hardcoded) |
|---|---|---|---|
| Name | `contact[name]` | No | `John` |
| Email | `contact[email]` | Yes | `name@gmail.com` |
| Subject | `contact[subject]` | Yes | `Tell us your request (order, delivery, stores...)` |
| Message | `contact[body]` | Yes | `Share your message...` |

### Hidden fields

| Field | Value |
|---|---|
| `contact[form_success_id]` | `contact-i-{{ section.id }}` |
| `contact[form_type]` | `Contact request` |

### Submission behavior

1. Submits to Shopify Contact API
2. Notification email sent per **Settings → Notifications** — **Needs confirmation** (recipient)
3. On success: `assets/wagon.js` uses `sessionStorage` to show success message and hide submit button
4. Success message (hardcoded): *"Thank you! Your message has been sent successfully. We'll get back to you as soon as possible."*

---

## Form 2 — Contact II (newsletter signup)

| Property | Value |
|---|---|
| Type | Shopify `{% form 'contact' %}` |
| Class | `form-contact` |
| File | `sections/contact-ii.liquid` |

### Fields

| Label (hardcoded) | `name` attribute | Required | Placeholder (hardcoded) |
|---|---|---|---|
| Name | `contact[name]` | No | `John` |
| Email | `contact[email]` | Yes | `name@gmail.com` |

### Hidden fields

| Field | Value |
|---|---|
| `contact[form_success_id]` | `contact-ii-{{ section.id }}` |
| `contact[form_type]` | `Newsletter signup` |
| `contact[category]` | `Exclusive offers and product updates` |

### Submission behavior

1. Submits to Shopify Contact API (not Shopify Customer API)
2. Success via `sessionStorage` pattern in `assets/wagon.js`
3. Success message (hardcoded): *"Successfully subscribed."*

> **Needs confirmation:** Whether signups are routed to Klaviyo, Mailchimp, Shopify Email, or only Shopify notification emails. No external ESP detected in theme code.

---

# Dependencies

| Dependency | Used by | Notes |
|---|---|---|
| Shopify Contact Form API | Both forms | Native Shopify |
| `assets/wagon.js` | Form success UX | `sessionStorage` + `data-form-success-wrapper` pattern |
| `assets/wagon.css` | Section layout, form styles | `.field-affiliate`, `.form-success-message` |
| `assets/home.css` | Contact II wrap styling | Optional overrides |

**No third-party apps** on this page.

---

# Hardcoded Content

Requires a **developer** to change:

| Content | File |
|---|---|
| Form field labels (Name, Email, Subject, Message) | `sections/contact-i.liquid`, `contact-ii.liquid` |
| Placeholder text on all inputs | Same |
| Required field note: "* Indicates a required field" | `sections/contact-i.liquid` |
| Form success messages | Section Liquid files |
| Hidden field names and `form_type` values | Section Liquid files |
| Email in Contact I schema default (`info@jagprovisions.com`) vs live template (`hello@jagprovisions.com`) | Schema vs `page.contact.json` — live uses template value |
| Decor CSS positioning | `assets/wagon.css` |
| Contact II background split layout | `assets/wagon.css` |

---

# Maintenance Notes

### Safe for client to update

- Contact I: heading, subheading, button label, email note (rich text)
- Contact II: heading, text, button label, all 5 decor images and alt text
- Form notification recipients via **Shopify Admin → Settings → Notifications**

### Use caution

- Email note in Contact I currently points to `hello@jagprovisions.com` — verify correct address
- Newsletter form is a **contact form**, not a marketing app integration — confirm list management workflow

### Requires developer

- Change form fields, labels, or placeholders
- Change success message copy
- Connect newsletter to external ESP (would need app or custom integration)
- Reposition decor images in layout

---

# Developer Notes

- Template: `templates/page.contact.json`
- Contact II uses class `contact-ii` on outer section; shares form patterns with affiliate/locator
- Red asterisk color: `#F2272B` in `assets/wagon.css` (`.field-affiliate label>strong`)
- Form success IIFE at bottom of `assets/wagon.js` — re-inits on `shopify:section:load`
- Contact II email field has `required` but name field does not — intentional asymmetry
- Section schema default for Contact I `email_note` differs from live JSON export

---

*Last updated: June 2026 · Audit source: `templates/page.contact.json`, `sections/contact-i.liquid`, `sections/contact-ii.liquid`*
