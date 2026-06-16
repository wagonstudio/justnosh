# Store Locator Guide — Just Nosh

This guide documents how the Store Locator works on eatjustnosh.com — where it lives in the theme, how store locations are managed, and what can be edited without a developer.

**Related documents:** `01-client-handoff-index.md` · `02-cms-editing-guide.md` · `05-third-party-services.md`

---

## Executive summary

| Question | Answer |
|---|---|
| Where are store locations stored? | **Stockist** (external app) — **not** in Shopify theme code |
| Are locations hardcoded in the theme? | **No** |
| Are locations in metafields, metaobjects, or JSON? | **No** — only the Stockist widget embed config is in the theme |
| How does the map appear on the site? | Stockist JavaScript widget embedded in **Locator I** |
| Where is the page? | Shopify page using template `locator` → `templates/page.locator.json` |
| Expected URL | `/pages/store-locator` (linked from footer) |

---

## 1. Where the functionality lives

### Page & template

| Item | Path / value |
|---|---|
| Shopify page template | `templates/page.locator.json` |
| Theme template suffix | `locator` |
| Expected page handle | `store-locator` (from `sections/footer-group.json` → `shopify://pages/store-locator`) |
| **Needs Shopify Admin confirmation** | Confirm the page exists, is published, and uses the **locator** theme template |

### Theme sections (2 sections on the page)

| Order | Section name (Theme Editor) | File | Purpose |
|---|---|---|---|
| 1 | **Locator I** | `sections/locator-i.liquid` | Page heading + **Stockist map widget** |
| 2 | **Locator II** | `sections/locator-ii.liquid` | “Don't see us near you?” **contact form** (not a location list) |

### Stockist embed (map data source)

| Item | Value in theme |
|---|---|
| Widget custom element | `<stockist-store-locator>` |
| Widget tag | `map_r3mn7jvq` (setting: `stockist_widget_tag`) |
| Embed script | `https://stockist.co/embed/v1/widget.min.js` |
| Loaded when | `load_stockist_script` = `true` (default) |

Configured in `templates/page.locator.json` and editable in Theme Editor → **Locator I**.

### Snippets

**No dedicated snippets** for the store locator. All markup is inline in:

- `sections/locator-i.liquid`
- `sections/locator-ii.liquid`

### CSS & JavaScript

| File | Usage |
|---|---|
| `assets/wagon.css` | Locator I page styles (`.section-locator-i`, `.head-locator`); Locator II reuses contact form styles (`.locator-ii`) |
| `assets/wagon.js` | Form success handling for Locator II contact form (`sessionStorage` pattern shared with Contact page) |
| `layout/theme.liquid` | Does not load Stockist globally — script loads only on pages with **Locator I** when `load_stockist_script` is enabled |

### Site links to Store Locator

| Location | File | Setting |
|---|---|---|
| Footer CTA button | `sections/foot.liquid` / `sections/footer-group.json` | CTA label: “STORE LOCATOR”, link: `shopify://pages/store-locator` |

### What is NOT the Store Locator

The theme also references **pickup locations** in `blocks/buy-buttons.liquid` (Shopify local pickup at checkout). That is **Shopify inventory pickup**, not the retail Store Locator map. Do not confuse the two.

---

## 2. How to add a location

Store locations are **not added in Shopify Admin or the Theme Editor**. They are added in **Stockist**.

### Prerequisites

- **Needs Shopify Admin confirmation:** Stockist account access (via [Stockist app on Shopify](https://stockist.co) or Stockist dashboard login)
- **Needs Shopify Admin confirmation:** Map provider key configured in Stockist (Google Maps or Mapbox — required by Stockist)

### Option A — Add one location (few stores)

1. Log in to the **Stockist dashboard**  
   *(If installed as a Shopify app: Shopify Admin → Apps → Stockist)*
2. Click **Add a location** in the left menu
3. Fill in location details (see [Section 6 — Location fields](#6-location-fields))
4. Click **Save**
5. The location should appear on the live map within Stockist’s normal sync time (usually shortly after save)
6. Verify on the live site: `/pages/store-locator`

### Option B — Bulk import (many stores)

1. In Stockist dashboard, open the **bulk import** tool
2. Prepare a spreadsheet following Stockist’s import format  
   → Stockist help: [Formatting your spreadsheet for import](https://help.stockist.co/article/47-formatting-your-spreadsheet-for-import)
3. Upload and review import results
4. Fix any geocoding errors Stockist reports
5. Publish the import when ready

### Option C — Google Sheet sync (Premium plan)

Stockist can sync from a linked Google Sheet once per day.  
→ Stockist help: [Linking Stockist to a Google Sheet](https://help.stockist.co/article/70-linking-stockist-to-a-google-sheet)

**Needs Shopify Admin confirmation:** Whether this account uses Google Sheet sync.

---

## 3. How to edit a location

### In Stockist (correct place for map pins)

1. Log in to **Stockist dashboard**
2. Go to **All locations** (or equivalent locations list)
3. Click the location to edit
4. Update fields (name, address, phone, website, etc.)
5. Click **Save**
6. Verify on `/pages/store-locator`

### Bulk edit multiple locations

1. In Stockist → **All locations**
2. Select locations via checkboxes
3. **Bulk actions → Make bulk changes**
4. Choose field(s) to update, preview, confirm

→ Stockist help: [How to bulk edit locations](https://help.stockist.co/article/102-how-to-bulk-edit-locations-in-stockist)

### In Shopify Theme Editor (page copy only — not locations)

These settings change **page text**, not store data:

| Section | Editable copy |
|---|---|
| **Locator I** | Heading (e.g. “FIND JUSTNOSH NEAR YOU”) |
| **Locator II** | Heading, subheading, button label, email note |

Theme Editor path: **Online Store → Themes → Customize → Pages → Store Locator**

---

## 4. How to delete a location

### In Stockist

1. Log in to **Stockist dashboard**
2. Go to **All locations**
3. Open the location to remove
4. Delete the location (or set **Visibility** to hidden — **Needs Shopify Admin confirmation** of exact UI label in your Stockist plan)

**Bulk delete:** Select multiple locations → **Bulk actions** → delete selected listings.

### Not in Shopify

Deleting a Shopify Page, editing the theme template, or removing **Locator I** will break or hide the map — it will **not** remove individual store listings. Location data lives in Stockist only.

---

## 5. How to reorder locations

Location order on the map and in search results is controlled in **Stockist**, not in the Shopify theme.

From Stockist’s bulk edit documentation, locations support a **Priority** field that can be set per location or via bulk actions.

**Needs Shopify Admin confirmation:**

- Whether **Priority** is configured for the Just Nosh Stockist account
- Default sort order in the widget (distance, priority, alphabetical, etc.) — configured in Stockist widget settings, not in theme code

### Reorder via Priority (if enabled in Stockist)

1. Stockist dashboard → edit location → set **Priority** value
2. Or bulk-edit **Priority** for multiple locations

### Reorder via bulk replace

For a full list replacement with a new order, use Stockist bulk import → publish as complete list.  
→ Stockist help: [Replacing your whole store list](https://help.stockist.co/article/29-replacing-your-whole-store-list)

---

## 6. Location fields

### Fields in the theme

The theme **does not define or store** location fields. The only Stockist-related values in theme code are:

| Theme setting | Label | Value (live) | File |
|---|---|---|---|
| `heading` | Heading | “FIND JUSTNOSH NEAR YOU” | `sections/locator-i.liquid` |
| `stockist_widget_tag` | Stockist widget tag | `map_r3mn7jvq` | `sections/locator-i.liquid` |
| `load_stockist_script` | Load Stockist script | `true` | `sections/locator-i.liquid` |

### Fields in Stockist (where location data actually lives)

Based on Stockist’s platform documentation (not visible in theme code). **Needs Shopify Admin confirmation** for which fields are actively used on the Just Nosh account.

| Field | In Stockist? | Notes |
|---|---|---|
| **Name** (store name) | Yes | Standard Stockist field |
| **Address** (street, city, state, postal code, country) | Yes | Used for map pin geocoding |
| **Coordinates** (lat/lng) | Yes | Typically auto-generated from address; may be editable |
| **Phone** | Yes | Standard field |
| **Website** | Yes | Standard field |
| **Email** | Yes | Standard field |
| **Image / logo URL** | Yes | Standard field |
| **Notes** | Yes | Free-text notes field |
| **Visibility** | Yes | Show/hide location on map |
| **Priority** | Yes | Affects sort order (if configured) |
| **Search filters** | Yes | Stockist filter tags for widget search |
| **Opening hours** | **Needs Shopify Admin confirmation** | Not a built-in theme field; may exist as a **Stockist custom field** if configured |
| **Custom fields** | Optional | Configured in Stockist → Settings → Custom Fields |

→ Stockist help: [Using Custom Fields](https://help.stockist.co/article/80-using-custom-fields)

### Locator II form fields (store requests — not locations)

The bottom form collects **customer inquiries**, not store data:

| Field | Hardcoded in theme | Sent as |
|---|---|---|
| Name | Yes — `sections/locator-ii.liquid` | `contact[name]` |
| Email | Yes | `contact[email]` |
| Message | Yes | `contact[body]` |
| Hidden: form type | Yes | `contact[form_type]` = “Store locator request” |
| Hidden: category | Yes | `contact[category]` = “Store locator inquiry” |

Submissions go to **Shopify Notifications** (contact form).  
**Needs Shopify Admin confirmation:** Which email inbox receives these submissions.

---

## 7. Data storage model

| Storage type | Used for store locations? | Evidence |
|---|---|---|
| Hardcoded Liquid | **No** | No location arrays in any `.liquid` file |
| Section blocks | **No** | `locator-i` has no blocks; `locator-ii` has no blocks |
| Shopify metafields | **No** | No `metafields` references for stores/locations |
| Shopify metaobjects | **No** | No metaobject references in theme |
| JSON data file in theme | **No** | `page.locator.json` only contains section settings, not location data |
| **Stockist app (external)** | **Yes** | `<stockist-store-locator>` + `widget.min.js` + widget tag `map_r3mn7jvq` |

```
Visitor → /pages/store-locator
    → Locator I (theme) loads Stockist script
    → Stockist widget (map_r3mn7jvq) fetches locations from Stockist servers
    → Map + search results rendered by Stockist (not Shopify)
```

---

## 8. Current limitations

### Theme / integration limitations

| Limitation | Detail |
|---|---|
| **No offline location management in Shopify** | All retailer locations must be managed in Stockist |
| **Widget tag hardcoded in template JSON** | `map_r3mn7jvq` in `templates/page.locator.json`; changing Stockist accounts requires updating Theme Editor or theme file |
| **Stockist script loads per-page only** | Script injected by `locator-i.liquid`, not globally — map only works where **Locator I** section is present |
| **No theme control over map UI** | Search filters, pin styles, result layout controlled in Stockist dashboard |
| **Lenis smooth scroll disabled on map** | `data-lenis-prevent` on widget shell — intentional so map scroll/zoom works |
| **Locator II form fields not editable in Theme Editor** | Labels, placeholders, success message hardcoded in `sections/locator-ii.liquid` |
| **Separate from Shopify pickup locations** | PDP “pickup available” uses Shopify locations, not Stockist retailers |

### Stockist / external dependencies

| Dependency | Risk |
|---|---|
| Stockist subscription active | Map stops working if account lapses |
| Map provider API key (Google/Mapbox) | Required by Stockist; key issues break map display |
| Third-party script CDN (`stockist.co`) | Map unavailable if Stockist CDN is down |
| Geocoding accuracy | Incorrect addresses = wrong pin placement (fixed in Stockist, not theme) |

### Needs Shopify Admin confirmation

- Stockist plan level and billing status
- Which map provider (Google Maps vs Mapbox) is configured
- Whether custom fields (e.g. opening hours) are set up
- Full list of current retailer locations
- Stockist account login / app install status in Shopify Admin → Apps

---

## 9. Maintenance recommendations

### For day-to-day content team

| Task | Where | Frequency |
|---|---|---|
| Add / edit / remove retailer locations | **Stockist dashboard** | As retailers change |
| Update page heading | Theme Editor → **Locator I** | Rarely |
| Update “contact us” form copy | Theme Editor → **Locator II** | Rarely |
| Review store request form submissions | Shopify Admin → contact form notifications | Weekly or as needed |
| Verify map on live site | `/pages/store-locator` | After any Stockist change |

### For developers / agency

| Task | When |
|---|---|
| Change Stockist widget tag | If migrating to a new Stockist account or widget |
| Toggle `load_stockist_script` | Only if Stockist script is loaded elsewhere (unlikely) |
| Edit Locator II form fields or success copy | If form labels or messaging needs change |
| Update footer CTA link | If page URL handle changes |
| Do **not** add locations to theme JSON/Liquid | Always use Stockist |

### Best practices

1. **Treat Stockist as the single source of truth** for all retailer locations — never duplicate location data in Shopify pages or metafields.
2. **After bulk imports**, always spot-check 3–5 pins on the live map before announcing updates.
3. **Keep a master spreadsheet** of retailers (export from Stockist periodically) as backup.
4. **Document Stockist login credentials** in your team password manager — not in the theme repo.
5. **When a retailer closes**, hide or delete in Stockist promptly — the theme has no separate list to update.
6. **Do not change `stockist_widget_tag`** without coordinating with whoever manages the Stockist account — wrong tag = empty or wrong map.
7. **Test mobile** after Stockist widget setting changes — map is inside `data-lenis-prevent` container with custom page styles.

### If Stockist is unavailable or being replaced

Replacing Stockist would require developer work:

- New embed or custom section in `sections/locator-i.liquid`
- New data source (metaobjects, app, or JSON)
- Update `templates/page.locator.json`

The current theme has **no fallback** location data in code.

---

## Quick reference: who edits what

| I want to… | Edit here |
|---|---|
| Add a new retail store to the map | **Stockist dashboard** |
| Change a store’s address or phone | **Stockist dashboard** |
| Hide a closed retailer | **Stockist dashboard** (visibility or delete) |
| Change map search behavior or pin style | **Stockist widget settings** |
| Change “FIND JUSTNOSH NEAR YOU” headline | **Theme Editor → Locator I** |
| Change the store request form intro text | **Theme Editor → Locator II** |
| Change form field labels (“Name”, “Email”) | **Developer** → `sections/locator-ii.liquid` |
| Change footer “STORE LOCATOR” button link | **Theme Editor → Foot** (footer section) |
| Change which Shopify page shows the map | **Shopify Admin → Pages** (assign `locator` template) |

---

## Theme Editor: Locator I settings

| Setting label | Current value | Safe to edit? |
|---|---|---|
| Heading | FIND JUSTNOSH NEAR YOU | ✅ Yes |
| Stockist widget tag | `map_r3mn7jvq` | ⚠️ Only with Stockist account coordination |
| Load Stockist script | Enabled | ⚠️ Keep enabled unless developer advises otherwise |

## Theme Editor: Locator II settings

| Setting label | Current value | Safe to edit? |
|---|---|---|
| Heading | DON'T SEE JUSTNOSH AT A STORE NEAR YOU? | ✅ Yes |
| Subheading | Let us know where you shop… | ✅ Yes |
| Button label | CONTACT US | ✅ Yes |
| Email note | hello@jagprovisions.com link | ✅ Yes |

---

## Troubleshooting

| Problem | Likely cause | Action |
|---|---|---|
| Map shows “Loading…” indefinitely | Stockist script blocked or widget tag wrong | Check `load_stockist_script` is on; verify widget tag in Stockist dashboard |
| Map is empty / no pins | No locations in Stockist or all hidden | Add locations in Stockist dashboard |
| Wrong stores showing | Wrong Stockist account or widget tag | Verify `map_r3mn7jvq` matches Stockist dashboard widget |
| Map scroll behaves oddly | Lenis interaction | Expected — `data-lenis-prevent` should remain on widget shell |
| Form submits but no email received | Shopify Notifications not configured | **Needs Shopify Admin confirmation** |
| Page 404 | Page unpublished or wrong handle | **Needs Shopify Admin confirmation** — check Pages admin |
| Map missing entirely | Page not using `locator` template | Assign template `page.locator.json` to the page |

---

## Files reference (theme audit)

| File | Role |
|---|---|
| `templates/page.locator.json` | Page structure and section settings |
| `sections/locator-i.liquid` | Stockist widget embed |
| `sections/locator-ii.liquid` | Store request contact form |
| `sections/footer-group.json` | Footer link to store locator page |
| `sections/foot.liquid` | Footer CTA settings schema |
| `assets/wagon.css` | Locator page styles |
| `assets/wagon.js` | Form success handling (Locator II) |

**Files searched with no store location data found:** all other `sections/`, `snippets/`, `templates/`, `config/` files.

---

*Last updated: June 2026 · Store location data is managed in Stockist, not in theme code. Field list for Stockist locations based on Stockist platform documentation — confirm active fields in your Stockist account.*
