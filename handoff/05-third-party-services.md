# Third-Party Services, Ownership & Access — Just Nosh

**Sitio:** [eatjustnosh.com](https://eatjustnosh.com)  
**Plataforma:** Shopify  
**Alcance de esta auditoría:** código del theme en repositorio local. Los scripts inyectados vía **Shopify Admin** (app embeds, pixels, `content_for_header`) pueden existir en producción sin aparecer en el theme — esos casos se marcan como **Needs confirmation**.

**Documentos relacionados:** `04-technical-documentation.md` · `07-store-locator-guide.md` · `08-product-management-guide.md`

---

## Resumen ejecutivo

| Categoría | Detectado en theme | No detectado en theme |
|---|---|---|
| Shopify apps (app blocks) | UpPromote Affiliate | — |
| Integraciones por DOM/CSS | Seal Subscriptions (`.sealsubs-container`) | — |
| Servicios externos embebidos | Stockist (store locator) | Klaviyo, Mailchimp, Eventbrite, Google Maps |
| Analytics / tracking hardcoded | — | GA, GTM, Meta Pixel, TikTok Pixel, Hotjar |
| Formularios | Shopify Contact API (nativo) | Typeform, HubSpot forms, etc. |
| Fuentes externas | Adobe Typekit, Google Fonts, Shopify Fonts | — |
| CDN de librerías | Bootstrap, GSAP, Splide, jQuery, Lenis, Splitting | — |

> **Importante:** `{{ content_for_header }}` en `layout/theme.liquid` carga scripts de Shopify y de apps instaladas en Admin. Revisar **Settings → Customer events** y **Online Store → Themes → Customize → App embeds** para el inventario completo en producción.

---

## Tabla de servicios

| Service / App | Purpose | Where it appears | Access owner | Notes | Status |
|---|---|---|---|---|---|
| **Shopify** | Hosting, checkout, productos, pedidos, contact forms | Toda la tienda | Needs confirmation | Plataforma base. Dominio esperado: `eatjustnosh.com` | Active (assumed) |
| **UpPromote Affiliate** | Programa de afiliados — tracking de referidos | App block global: `config/settings_data.json` → `blocks.3138269044488530766` — tipo `shopify://apps/uppromote-affiliate/blocks/core-script/64c32457-930d-4cb9-9641-e24c0d9cf1f4` | Needs confirmation | `disabled: false` en theme export. No hay referencias Liquid adicionales en secciones custom. Página `/pages/affiliate` usa formulario contact nativo, no UI de UpPromote en el theme | **Active in theme settings** — confirmar app en Admin |
| **Seal Subscriptions** | Suscripciones / selling plans en PDP | DOM inyectado por app: `.sealsubs-container`, `.sealsubs-target-element` — estilos en `assets/home.css`, `assets/master.css`, `sections/pdp-i.liquid` (CSS + JS DOM move) | Needs confirmation | **Detected in theme code only** — no app block en `settings_data.json`. La app se instala y embebe vía Shopify Admin | Needs confirmation |
| **Stockist** | Store locator — mapa y ubicaciones de retail | `sections/locator-i.liquid` — widget tag `map_r3mn7jvq`; script `https://stockist.co/embed/v1/widget.min.js`; template `templates/page.locator.json` | Needs confirmation | Ubicaciones gestionadas en dashboard Stockist, no en theme. `data-lenis-prevent` en contenedor del widget | **Active in theme code** — confirmar cuenta Stockist |
| **Adobe Fonts (Typekit)** | Tipografía Obviously / Obviously Narrow | `layout/theme.liquid` — `https://use.typekit.net/rji3kyn.css` | Needs confirmation | Kit ID: `rji3kyn`. Preconnect a `use.typekit.net`, `p.typekit.net` | **Active in theme code** |
| **Google Fonts** | Work Sans (cargada globalmente) | `layout/theme.liquid` — `fonts.googleapis.com/css2?family=Work+Sans:...` | N/A (público) | **No se detectó uso en CSS del theme** — posible carga redundante | Active in theme code (low use) |
| **Shopify Fonts** | Instrument Sans (Heritage) | `snippets/fonts.liquid` — preload WOFF2 vía `font_url` filter; settings `type_*_font: instrument_sans_n4` en `config/settings_data.json` | Shopify (incluido en plan) | CDN Shopify (`fonts.shopifycdn.com` en runtime) | Active |
| **Bootstrap (jsDelivr)** | Grid CSS/JS, tabs PDP III | `layout/theme.liquid` — v5.3.3 CDN | N/A (open source CDN) | Usado en layout custom y `sections/pdp-iii.liquid` (`data-bs-toggle="tab"`) | Active in theme code |
| **GSAP + ScrollTrigger (cdnjs)** | Animaciones scroll | `layout/theme.liquid` — v3.12.5; orquestado en `assets/wagon.js` | N/A | | Active in theme code |
| **Lenis (unpkg)** | Smooth scroll desktop | `layout/theme.liquid` — v1.0.44; `assets/wagon.js` | N/A | Desactivado en touch | Active in theme code |
| **Splide (jsDelivr)** | Carruseles homepage/PDP | `layout/theme.liquid` + init en `assets/wagon.js` y `sections/pdp-i.liquid` | N/A | v4.1.4 | Active in theme code |
| **Splitting (jsDelivr)** | Text split animations | `layout/theme.liquid` + `assets/splitting.css` | N/A | v1.0.6 | Active in theme code |
| **jQuery (code.jquery.com)** | Librería JS | `layout/theme.liquid` — v3.6.0 | N/A | Cargada globalmente; **sin uso detectado en `assets/wagon.js`** | Active in theme code (possibly unused) |
| **Shopify Contact Form API** | Formularios contacto, affiliate, locator, newsletter block | `sections/contact-i.liquid`, `contact-ii.liquid`, `affiliate-v.liquid`, `locator-ii.liquid` — `{% form 'contact' %}` | Shopify Admin (notificaciones email) | Campos custom: `contact[form_type]`, `contact[category]`, etc. Success UX: `sessionStorage` en `assets/wagon.js` | Active (native Shopify) |
| **Shopify Customer API** | Email signup (Heritage) | `blocks/email-signup.liquid` — `{% form 'customer' %}` | Shopify Admin | **No detectado en templates live** de páginas custom; bloque Heritage disponible | Available in theme — Needs confirmation if used |
| **Shop Pay / Accelerated checkout** | Checkout express (Apple Pay, etc.) | `blocks/accelerated-checkout.liquid` — `{{ form_obj \| payment_button }}`; activo en `templates/product.json` (bloque en `pdp-i`) | Shopify Payments / Shop channel | Setting `show_accelerated_checkout_buttons` no presente en export actual — comportamiento por defecto Heritage | Needs confirmation |
| **Shopify `content_for_header`** | Scripts de plataforma, apps, pixels configurados en Admin | `layout/theme.liquid` línea 70 | Shopify Admin | Incluye posibles analytics y app embeds **no visibles en theme files** | Needs confirmation |
| **Shopify Customer Events / Pixels** | Analytics, Meta, Google, etc. | Inyectado vía Admin → **no en theme code** | Needs confirmation | **No detectado:** `gtag`, `GTM-`, `klaviyo`, `mailchimp`, `fbevents`, `hotjar`, `tiktok` pixel en `.liquid`/`.js`/`.json` | Not detected in theme — Needs confirmation in Admin |
| **Klaviyo** | Email marketing | — | — | **No detectado** en theme | Not detected |
| **Mailchimp** | Email marketing | — | — | **No detectado** en theme | Not detected |
| **Eventbrite** | Eventos | — | — | **No detectado** en theme | Not detected |
| **Google Maps / Mapbox** | Mapas | — | — | Store locator usa **Stockist**, no Google Maps | Not detected |
| **YouTube embed** | Video | `snippets/video.liquid` — URL template `youtube.com/embed/` | N/A | Solo si se configura video YouTube en secciones Heritage | Available — Needs confirmation if used on live pages |
| **Instagram / Facebook / TikTok / LinkedIn** | Enlaces sociales (outbound) | `config/settings_data.json` (theme settings); `sections/foot.liquid`; `sections/header-group.json` (header menu block) | Needs confirmation (cuentas de marca) | URLs live: `@justnoshbar`, `/justnoshbar`, `@justnoshbar`, `/company/justnosh` — **no son pixels de tracking** | Active (links only) |
| **WAGON (custom dev)** | CSS/JS/animaciones custom | `assets/wagon.js`, `assets/master.css`, `assets/wagon.css`, `assets/home.css` | To be provided by JAG Provisions / Wagon | Desarrollo custom sobre Heritage — no es app Shopify | Active in theme code |
| **Heritage 3.5.1** | Theme base Shopify | Todo el theme | Shopify (licencia theme) | `config/settings_schema.json` | Active |

---

## Shopify apps — detalle de auditoría

### App blocks detectados en `config/settings_data.json`

| App | Block type | Disabled | Ubicación |
|---|---|---|---|
| **UpPromote Affiliate** | `shopify://apps/uppromote-affiliate/blocks/core-script/64c32457-930d-4cb9-9641-e24c0d9cf1f4` | `false` | Theme settings global (`current.blocks`) — típicamente **Theme Settings → App embeds** |

No se detectaron otros `shopify://apps/` en templates JSON ni secciones Liquid.

### App embeds (theme code)

| Servicio | Evidencia | Tipo |
|---|---|---|
| **Seal Subscriptions** | Clases `.sealsubs-container`, `.sls-*` en CSS/JS | **Detected in theme code only** — embed gestionado en Shopify Admin → Apps |
| **UpPromote** | App block en `settings_data.json` | Confirmado en theme export |

### App embeds probables vía Admin (no en theme files)

Cualquier app con script global (reviews, chat, analytics, popups) puede estar activa solo en **Online Store → Themes → Customize → App embeds** o vía `content_for_header`. **Needs confirmation** en Shopify Admin.

---

## Scripts externos — inventario completo (theme code)

### Cargados en `layout/theme.liquid` (todas las páginas)

| URL | Recurso |
|---|---|
| `cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css` | Bootstrap CSS |
| `cdn.jsdelivr.net/npm/@splidejs/splide@4.1.4/dist/css/splide-core.min.css` | Splide CSS |
| `use.typekit.net/rji3kyn.css` | Adobe Typekit |
| `fonts.googleapis.com/css2?family=Work+Sans:...` | Google Fonts |
| `code.jquery.com/jquery-3.6.0.min.js` | jQuery |
| `cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js` | GSAP |
| `cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js` | ScrollTrigger |
| `unpkg.com/lenis@1.0.44/dist/lenis.min.js` | Lenis |
| `cdn.jsdelivr.net/npm/@splidejs/splide@4.1.4/dist/js/splide.min.js` | Splide JS |
| `cdn.jsdelivr.net/npm/splitting@1.0.6/dist/splitting.min.js` | Splitting |
| `cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js` | Bootstrap JS |

### Cargados por página

| URL | Página / sección |
|---|---|
| `stockist.co/embed/v1/widget.min.js` | `sections/locator-i.liquid` — Store Locator (`load_stockist_script: true`) |

### Cargados por Heritage (`snippets/scripts.liquid`)

Módulos ES desde `assets/*.js` en el dominio de la tienda (`/cdn/shop/t/.../assets/`). Incluye cart drawer, product form, predictive search, view transitions, etc.

### Runtime Shopify (no en repo)

| Origen | Contenido típico |
|---|---|
| `{{ content_for_header }}` | Shopify core, app scripts, consent, pixels |
| `font_url` / Shopify CDN | Instrument Sans WOFF2 |
| Checkout Shopify | `checkout.shopify.com` |

---

## Analytics y tracking

### Detectado en theme code

| Tipo | Resultado |
|---|---|
| Google Analytics / gtag.js | **No detectado** |
| Google Tag Manager | **No detectado** |
| Meta Pixel / fbevents | **No detectado** |
| TikTok Pixel | **No detectado** |
| Hotjar / Segment / Mixpanel | **No detectado** |
| Klaviyo onsite script | **No detectado** |

### Posible tracking fuera del theme

| Canal | Dónde verificar |
|---|---|
| Shopify Customer events | Admin → **Settings → Customer events** |
| Meta / Google / TikTok pixels | Admin → Customer events, o apps instaladas |
| UpPromote | Tracking de afiliados vía app |
| Shopify Analytics | Admin → **Analytics** (incluido en plan) |

**Status:** Not detected in theme — **Needs confirmation** in Shopify Admin.

---

## Form integrations

| Formulario | Método | Archivo | Destino |
|---|---|---|---|
| Contacto general | `{% form 'contact' %}` | `sections/contact-i.liquid` | Email notificación Shopify Admin |
| Newsletter / offers | `{% form 'contact' %}` (hidden `form_type: Newsletter signup`) | `sections/contact-ii.liquid` | Email notificación Shopify Admin — **no Klaviyo/Mailchimp** |
| Affiliate application | `{% form 'contact' %}` | `sections/affiliate-v.liquid` | Email notificación Shopify Admin |
| Store locator inquiry | `{% form 'contact' %}` | `sections/locator-ii.liquid` | Email notificación Shopify Admin |
| Heritage email signup | `{% form 'customer' %}` | `blocks/email-signup.liquid` | Lista clientes Shopify |

**Emails de contacto referenciados en theme (verificar cuál es oficial):**

| Email | Ubicación |
|---|---|
| `hello@jagprovisions.com` | `templates/page.locator.json`, default `sections/locator-ii.liquid` |
| `info@jagprovisions.com` | Default `sections/contact-i.liquid` |
| `hello@japrovisions.com` | Default `sections/affiliate-v.liquid` |

---

## Map services

| Servicio | Uso | Archivo |
|---|---|---|
| **Stockist** | Mapa interactivo + búsqueda de tiendas | `sections/locator-i.liquid` |
| Google Maps / Mapbox | — | **No detectado** |

Widget tag configurado: `map_r3mn7jvq` (`templates/page.locator.json`).

---

## Fonts externos

| Proveedor | Familias | Carga |
|---|---|---|
| **Adobe Typekit** | Obviously, Obviously Narrow | `use.typekit.net/rji3kyn.css` |
| **Google Fonts** | Work Sans (variable) | `fonts.googleapis.com` |
| **Shopify Fonts** | Instrument Sans | `snippets/fonts.liquid` + theme settings |

---

## CDN externos — resumen de dependencias

| CDN | Servicios |
|---|---|
| `cdn.jsdelivr.net` | Bootstrap, Splide, Splitting |
| `cdnjs.cloudflare.com` | GSAP, ScrollTrigger |
| `unpkg.com` | Lenis |
| `code.jquery.com` | jQuery |
| `use.typekit.net` / `p.typekit.net` | Adobe Fonts |
| `fonts.googleapis.com` / `fonts.gstatic.com` | Google Fonts |
| `stockist.co` | Store locator widget |

**Riesgo operativo:** actualizaciones o caídas de estos CDN afectan animaciones, carruseles, tipografía y el mapa. Las versiones están fijadas en URLs del theme (ver `04-technical-documentation.md`).

---

## Ownership confirmation required

Checklist para completar el handoff con JAG Provisions / Wagon. Marcar cada ítem cuando el propietario y las credenciales estén confirmados.

### Shopify store

- [ ] Confirmar propietario de la tienda Shopify (cuenta admin principal)
- [ ] Confirmar plan Shopify activo
- [ ] Transferir o documentar accesos: Owner, Staff accounts, colaboradores del theme
- [ ] Confirmar URL de admin: `https://[store].myshopify.com/admin`
- [ ] Exportar lista de apps instaladas: **Settings → Apps**

### Domain

- [ ] Confirmar propietario del dominio `eatjustnosh.com`
- [ ] Confirmar registrador y acceso DNS
- [ ] Verificar dominio primario en **Settings → Domains**
- [ ] Documentar subdominios (www, checkout, etc.)

### Apps

- [ ] **UpPromote Affiliate** — cuenta, plan, propietario, estado activo
- [ ] **Seal Subscriptions** — cuenta, configuración selling plans, propietario
- [ ] **Stockist** — cuenta, widget tag `map_r3mn7jvq`, propietario, plan
- [ ] Revisar **Theme → Customize → App embeds** para embeds no reflejados en theme export
- [ ] Documentar cualquier otra app en Admin no detectada en este repo

### Analytics

- [ ] **Settings → Customer events** — listar pixels activos (Google, Meta, TikTok, etc.)
- [ ] Confirmar propietario de cuentas Google Analytics / GTM (si existen)
- [ ] Confirmar propietario de Meta Business / TikTok Ads (si existen)
- [ ] Documentar si Shopify Analytics es la única fuente de métricas

### Email services

- [ ] Confirmar destinatarios de notificaciones **Settings → Notifications** (contact form, orders)
- [ ] Confirmar si hay ESP externo (Klaviyo, Mailchimp, etc.) — **no detectado en theme**
- [ ] Verificar emails oficiales: `hello@jagprovisions.com` vs `hello@japrovisions.com` vs `info@jagprovisions.com`
- [ ] Confirmar dominio de envío (SPF/DKIM) si se usa email marketing externo

### DNS

- [ ] Acceso al panel DNS del dominio
- [ ] Registros SPF, DKIM, DMARC para email
- [ ] Registros CNAME/A hacia Shopify
- [ ] Registros de verificación de terceros (Stockist, Typekit, etc.) — **Needs confirmation**

### GitHub repository

- [ ] Confirmar propietario del repositorio del theme
- [ ] Documentar URL del repo y branch de producción
- [ ] Confirmar quién tiene acceso de escritura
- [ ] Confirmar si existe CI/CD o deploy manual a Shopify

### Theme backup

- [ ] Descargar theme live actual desde **Online Store → Themes → Actions → Download**
- [ ] Guardar copia con fecha y versión (Heritage 3.5.1 + custom WAGON)
- [ ] Documentar theme publicado vs theme de desarrollo
- [ ] Exportar `settings_data.json` tras cambios en Theme Editor

### Image assets

- [ ] Confirmar propietario de **Shopify Admin → Content → Files** (`shop_images`)
- [ ] Exportar / respaldar archivos de marca (logos, SVGs decorativos, fotos)
- [ ] Confirmar licencias de imágenes y fuentes (Adobe Typekit kit `rji3kyn`)
- [ ] Documentar acceso a banco de imágenes externo si aplica — **Needs confirmation**

---

## Accesos a solicitar (plantilla)

| Servicio | Qué solicitar | Owner confirmado |
|---|---|---|
| Shopify Admin | Owner o Staff con permisos Themes, Apps, Settings | Needs confirmation |
| UpPromote | Login dashboard afiliados | Needs confirmation |
| Seal Subscriptions | Login app + configuración planes | Needs confirmation |
| Stockist | Login dashboard + widget `map_r3mn7jvq` | Needs confirmation |
| Adobe Typekit | Cuenta con kit `rji3kyn` | Needs confirmation |
| Domain registrar | Panel DNS | Needs confirmation |
| GitHub | Repo theme | To be provided by JAG Provisions / Wagon |
| Google / Meta analytics | Si existen en Customer events | Needs confirmation |

---

## Notas para mantenimiento

1. **No eliminar app blocks** de `settings_data.json` sin verificar en Admin si la app sigue en uso.
2. **Seal Subscriptions:** cambios en el DOM de la app pueden romper el JS de `sections/pdp-i.liquid` — probar PDP tras actualizar la app.
3. **Stockist:** cambios de widget tag requieren actualizar `sections/locator-i.liquid` o Theme Editor → Locator I.
4. **Typekit:** si el kit deja de estar activo, las fuentes Obviously fallan en páginas custom.
5. **Auditoría incompleta sin Admin:** ejecutar revisión de **Apps**, **App embeds**, y **Customer events** en Shopify Admin para cerrar gaps de analytics y apps no reflejadas en el theme export.

---

*Última actualización: Junio 2026 · Auditoría basada en `layout/theme.liquid`, `config/settings_data.json`, secciones custom, y búsqueda en codebase del theme*
