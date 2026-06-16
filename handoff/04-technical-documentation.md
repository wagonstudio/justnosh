# Documentación técnica — Just Nosh

**Sitio:** [eatjustnosh.com](https://eatjustnosh.com)  
**Theme base:** Heritage **3.5.1** (`config/settings_schema.json`)  
**Capa custom:** sistema WAGON (CSS/JS + secciones propias)  
**Fecha del paquete:** Junio 2026

**Documentos relacionados:** `00-theme-audit.md` · `03-theme-design-guide.md` · `06-assets-inventory.md` · `05-third-party-services.md`

---

## 1. Theme structure overview

### Arquitectura en dos capas

```
layout/theme.liquid          ← Shell global (head, header-group, main, footer-group)
├── Heritage 3.5.1           ← base.css, snippets/scripts.liquid, Web Components, color schemes
└── WAGON custom layer       ← master.css, wagon.css, home.css, wagon.js + CDN libs
```

El layout principal es `layout/theme.liquid`. Carga recursos en este orden aproximado:

1. Heritage: `snippets/meta-tags.liquid`, `snippets/stylesheets.liquid` (`assets/base.css`), `snippets/fonts.liquid`, `snippets/scripts.liquid` (importmap + módulos ES), `snippets/theme-styles-variables.liquid`, `snippets/color-schemes.liquid`
2. CDN externos: Bootstrap 5.3.3, Splide 4.1.4, Adobe Typekit (`rji3kyn`), Google Fonts (Work Sans)
3. Assets WAGON: `assets/splitting.css`, `assets/splitting-cells.css`, `assets/wagon.css`, `assets/home.css`, `assets/master.css`
4. JS CDN: jQuery 3.6.0, GSAP 3.12.5 + ScrollTrigger, Lenis 1.0.44, Splide, Splitting 1.0.6, Bootstrap bundle
5. `assets/wagon.js` (defer)

### Estructura de carpetas del theme

| Carpeta | Contenido | Cantidad aprox. |
|---|---|---|
| `layout/` | `theme.liquid`, `password.liquid` | 2 |
| `templates/` | JSON templates + `gift_card.liquid` | 17 |
| `sections/` | Secciones Heritage + custom Just Nosh | 67 |
| `blocks/` | Bloques del sistema Heritage (header, PDP, cart) | 93 |
| `snippets/` | Fragmentos reutilizables Heritage + overrides | 103 |
| `assets/` | CSS, JS, SVG icons | 119 |
| `config/` | `settings_schema.json`, `settings_data.json` | 2 |
| `locales/` | Traducciones Heritage (51 archivos) | 51 |

### Section groups

| Archivo | Rol |
|---|---|
| `sections/header-group.json` | Header Heritage (`sections/header.liquid`) — sin announcement bar activa |
| `sections/footer-group.json` | Footer custom (`sections/foot.liquid`) en lugar del footer Heritage estándar |

### Punto de entrada del contenido

```liquid
<!-- layout/theme.liquid -->
<main id="MainContent" data-template="{{ template }}">
  {{ content_for_layout }}
</main>
```

Cada template JSON define qué secciones se renderizan en `content_for_layout`.

---

## 2. Templates principales

| Template | Ruta | Uso en sitio |
|---|---|---|
| Homepage | `templates/index.json` | `/` — 4 secciones custom |
| Producto (PDP) | `templates/product.json` | `/products/...` — PDP custom I–III + recommendations |
| Colección / Shop | `templates/collection.json` | `/collections/...` — sección `shop` |
| About | `templates/page.about.json` | `/pages/about` — secciones `about-i` … `about-v` |
| Contact | `templates/page.contact.json` | `/pages/contact` — `contact-i`, `contact-ii` |
| Affiliate | `templates/page.affiliate.json` | `/pages/affiliate` — `affiliate-i` … `affiliate-v` |
| Store Locator | `templates/page.locator.json` | `/pages/store-locator` — `locator-i`, `locator-ii` |
| Cart | `templates/cart.json` | `/cart` — Heritage `main-cart` |
| Style guide (interno) | `templates/page.master.json` | Página de referencia de clases WAGON |
| Página genérica | `templates/page.json` | Páginas sin template alternativo |
| Search | `templates/search.json` | `/search` |
| 404 | `templates/404.json` | Página no encontrada |
| Blog / Article | `templates/blog.json`, `templates/article.json` | Blog Heritage (uso en sitio: **Needs Shopify Admin confirmation**) |
| Gift card | `templates/gift_card.liquid` | Tarjetas regalo |
| Password | `templates/password.json` | Tienda con contraseña |
| List collections | `templates/list-collections.json` | Listado de colecciones |

### Detalles relevantes por template

**`templates/index.json`** — orden: `superhero` → `why` → `shopping` → `ourmission`

**`templates/product.json`** — orden activo: `pdp-i` → `pdp-ii` → `pdp-iii` → `pdp-recommendations`. La sección Heritage `product-information` está presente pero **`disabled: true`**.

**`templates/collection.json`** — una sola sección `shop` (grid de productos custom, no `main-collection` Heritage).

**`templates/page.*.json`** — en varios casos incluyen `main-page` Heritage con `disabled: true`; el contenido visible viene de secciones custom.

---

## 3. Sections principales

### Secciones custom Just Nosh (WAGON)

| Sección | Archivo | Páginas |
|---|---|---|
| Superhero (hero homepage) | `sections/superhero.liquid` | Homepage |
| Why | `sections/why.liquid` | Homepage |
| Shopping | `sections/shopping.liquid` | Homepage |
| Our Mission | `sections/ourmission.liquid` | Homepage |
| Shop (colección) | `sections/shop.liquid` | Collection |
| Minishop | `sections/minishop.liquid` | No referenciada en templates activos detectados |
| PDP I | `sections/pdp-i.liquid` | Product |
| PDP II | `sections/pdp-ii.liquid` | Product |
| PDP III | `sections/pdp-iii.liquid` | Product |
| PDP Recommendations | `sections/pdp-recommendations.liquid` | Product |
| About I–V | `sections/about-i.liquid` … `sections/about-v.liquid` | About |
| Contact I–II | `sections/contact-i.liquid`, `sections/contact-ii.liquid` | Contact |
| Affiliate I–V | `sections/affiliate-i.liquid` … `sections/affiliate-v.liquid` | Affiliate |
| Locator I–II | `sections/locator-i.liquid`, `sections/locator-ii.liquid` | Store Locator |
| Footer custom | `sections/foot.liquid` | Global (footer-group) |
| Style guide | `sections/master.liquid` | `page.master` |

### Secciones Heritage en uso activo

| Sección | Archivo | Uso |
|---|---|---|
| Header | `sections/header.liquid` | Global — `header-group.json` |
| Main cart | `sections/main-cart.liquid` | `/cart` |
| Main 404 | `sections/main-404.liquid` | 404 |
| Search | `sections/search-header.liquid`, `sections/search-results.liquid` | Search |
| Predictive search | `sections/predictive-search.liquid` | Modal búsqueda (header tiene `show_search: false` en settings actuales) |

### Secciones Heritage presentes pero no en templates live detectados

Incluyen entre otras: `slideshow.liquid`, `carousel.liquid`, `marquee.liquid`, `hero.liquid`, `announcement` (no en header-group), `layered-slideshow.liquid`, `product-hotspots.liquid`, `quick-order-list.liquid`. Forman parte del theme base y pueden añadirse desde Theme Editor.

---

## 4. Snippets principales

### Heritage — infraestructura core

| Snippet | Rol |
|---|---|
| `snippets/scripts.liquid` | Importmap `@theme/*`, preloads, carga condicional de módulos JS |
| `snippets/stylesheets.liquid` | Preload `overflow-list.css`, carga `base.css` |
| `snippets/theme-styles-variables.liquid` | Variables CSS Heritage (--font-*, --color-*) |
| `snippets/color-schemes.liquid` | Definición de color schemes |
| `snippets/fonts.liquid` | Preload Instrument Sans desde Shopify CDN |
| `snippets/meta-tags.liquid` | SEO / Open Graph |

### Header y navegación

| Snippet | Rol |
|---|---|
| `snippets/header-actions.liquid` | Cart icon, cart drawer component, estilos custom del drawer |
| `snippets/header-drawer.liquid` | Menú móvil — links custom, redes sociales, privacy policy hardcoded |
| `snippets/header-row.liquid` | Fila del header Heritage |

### Cart

| Snippet | Rol |
|---|---|
| `snippets/cart-products.liquid` | Líneas del carrito |
| `snippets/cart-summary.liquid` | Totales y checkout |
| `snippets/cart-items-component.liquid` | Wrapper del componente cart |
| `snippets/cart-bubble.liquid` | Contador de items |

### Producto (Heritage — usado dentro de bloques en PDP I)

| Snippet | Rol |
|---|---|
| `snippets/product-form` vía `blocks/buy-buttons.liquid` | Formulario add-to-cart |
| `snippets/variant-main-picker.liquid` | Selector de variantes |
| `snippets/add-to-cart-button.liquid` | Botón ATC Heritage |
| `snippets/price.liquid` | Precio formateado |

### Otros

| Snippet | Rol |
|---|---|
| `snippets/icon.liquid` | SVG inline para redes sociales |
| `snippets/search-modal.liquid` | Modal de búsqueda |
| `snippets/quick-add-modal.liquid` | Quick add (condicional por settings) |

---

## 5. Assets CSS principales

### Custom WAGON (carga global — `layout/theme.liquid`)

| Archivo | Responsabilidad |
|---|---|
| `assets/master.css` | Design system: variables `:root`, tipografía (`.he-*`, `.bo-*`), botones, contenedores, utilidades responsive VW |
| `assets/wagon.css` | Estilos por sección: header drawer, footer, why, shopping, about, affiliate, contact, locator, cart drawer overrides |
| `assets/home.css` | Homepage hero (`.superhero-i`, `.hero-ready`), PDP, cart, Seal Subscriptions overrides, animaciones CSS del hero |
| `assets/splitting.css` | Estilos base Splitting.js |
| `assets/splitting-cells.css` | Estilos celdas Splitting.js |

### Heritage

| Archivo | Responsabilidad |
|---|---|
| `assets/base.css` | Estilos core Heritage — cargado vía `snippets/stylesheets.liquid` |
| `assets/overflow-list.css` | Componente overflow-list del header |

### CSS inline en secciones

Varias secciones custom incluyen bloques `<style>` scoped:

- `sections/pdp-i.liquid` — colores por producto (metafields `color_*`), estilos Seal Subscriptions, Splide PDP
- `snippets/header-actions.liquid` — estilos extensos del cart drawer custom
- Otras secciones pueden incluir estilos puntuales

### CDN CSS (no en `assets/`)

- Bootstrap 5.3.3 (`cdn.jsdelivr.net`)
- Splide core CSS 4.1.4
- Adobe Typekit `rji3kyn.css`
- Google Fonts Work Sans

---

## 6. Assets JS principales

### Custom WAGON

| Archivo | Tipo | Rol |
|---|---|---|
| `assets/wagon.js` | IIFE global | Lenis, GSAP/ScrollTrigger, Splitting, Splide genérico, animaciones `data-anim`, page entrance, API `window.WagonTheme` |
| (inline en `wagon.js`, segunda IIFE) | — | Form success con `sessionStorage` |

### Heritage — módulos ES (`type="module"`)

Cargados vía `snippets/scripts.liquid` con importmap `@theme/*`. Archivos clave:

| Archivo | Rol |
|---|---|
| `assets/component.js` | Base Web Components |
| `assets/utilities.js` | Utilidades DOM, header height |
| `assets/cart-drawer.js` | Web Component `<cart-drawer-component>` |
| `assets/cart-icon.js` | Web Component `<cart-icon>` |
| `assets/product-form.js` | Web Component `<product-form-component>` |
| `assets/variant-picker.js` | Selector de variantes |
| `assets/section-renderer.js` | Re-render de secciones AJAX |
| `assets/section-hydration.js` | Hidratación de secciones |
| `assets/view-transitions.js` | View Transitions API (activo: `transition_to_main_product: true`) |
| `assets/morph.js` | DOM morphing para updates parciales |

Total en `assets/`: **76 archivos `.js`** (incluye ~70 módulos Heritage).

### CDN JS (carga global — `layout/theme.liquid`)

| Librería | Versión | Uso |
|---|---|---|
| jQuery | 3.6.0 | Dependencia cargada; uso directo en theme custom: **no detectado en `wagon.js`** |
| GSAP + ScrollTrigger | 3.12.5 | Animaciones scroll |
| Lenis | 1.0.44 | Smooth scroll desktop (deshabilitado en touch) |
| Splide | 4.1.4 | Carruseles genéricos + PDP (también init inline en `pdp-i`) |
| Splitting | 1.0.6 | Text split animations |
| Bootstrap | 5.3.3 | Grid + tabs PDP III (`data-bs-toggle="tab"`) |

### JS inline en secciones

| Sección | Contenido |
|---|---|
| `sections/pdp-i.liquid` | Init Splide PDP, sync variantes custom, integración Seal Subscriptions DOM |
| `layout/theme.liquid` | Header height CSS vars, menu style detection |

---

## 7. Custom scripts y animaciones

### Orquestador: `assets/wagon.js`

**API pública:**

```javascript
window.WagonTheme = {
  init,      // initAll()
  reinit,    // destroy + re-init animaciones
  refresh,   // ScrollTrigger.refresh()
  getLenis   // instancia Lenis o null
};
```

**Flujo de inicialización (`initAll`):**

1. `initGSAP()` — registra ScrollTrigger
2. `initLenis()` — solo desktop (no touch); integrado con `gsap.ticker`
3. `initScope(document)` — Splitting → Splide → todas las animaciones
4. `initPageEntrance()` — añade clases `wagon-page-ready`, `header-ready`, `hero-ready`

**Re-init en Theme Editor:** escucha `shopify:section:load`, `shopify:section:reorder`, `shopify:section:select`, `shopify:block:select`.

### Atributos HTML para animaciones

| Atributo | Comportamiento | Init en `wagon.js` |
|---|---|---|
| `data-splitting` | Splitting.js (chars/words) | `initSplitting()` |
| `data-splitting` + `data-split-animate` | Animación post-split | `initSplitTextAnimations()` |
| `data-reveal` | Fade-up genérico | `initRevealAnimations()` |
| `data-anim="chars"` | Entrada por caracteres | `initCharsAnimations()` |
| `data-anim="words"` | Entrada por palabras | `initWordsAnimations()` |
| `data-anim="fade-up"` | Fade + translateY | `initFadeUpAnimations()` |
| `data-anim="fade-in"` | Solo opacity | `initFadeInAnimations()` |
| `data-anim="stagger"` | Hijos con stagger | `initStaggerAnimations()` |
| `data-anim="title-mask"` | Chars con máscara | `initTitleMaskAnimations()` |
| `data-anim="title-reveal"` | Clip-path reveal | `initTitleRevealAnimations()` |
| `data-anim="decor-float"` | Decor con scale/rotation | `initDecorFloatAnimations()` |
| `data-anim="decor-pop"` | Imágenes internas pop | `initDecorPopAnimations()` |
| `data-parallax="0.3"` | Parallax scrub (solo desktop) | `initParallaxAnimations()` |
| `data-splide` + data attrs | Carrusel Splide genérico | `initSplide()` |

**Touch vs desktop:** en dispositivos touch, las animaciones con ScrollTrigger usan `IntersectionObserver` como fallback (`animateOnEnter`). Lenis y parallax se omiten en touch.

### Animaciones CSS (sin GSAP)

| Clase / evento | Archivo | Efecto |
|---|---|---|
| `.hero-ready` en `.superhero-i` | `assets/home.css` | Staggered entrance del hero homepage |
| `.header-ready` en `.header__row--top` | `assets/home.css` | Entrance del header |
| `.wagon-page-ready` en `body` | `assets/home.css` | Fade-in de página |
| Shopping card hover | `assets/wagon.css` | CSS transitions en `.item-shopping` |

### Splide en PDP

`sections/pdp-i.liquid` tiene su **propio script inline** para Splide main + thumbs (sync). No usa el sistema `data-splide` de `wagon.js`. Espera a que `window.Splide` esté disponible (poll hasta 40 intentos × 150ms).

---

## 8. Cart drawer customizations

### Configuración Heritage (`config/settings_data.json`)

| Setting | Valor live |
|---|---|
| `cart_type` | `drawer` |
| `auto_open_cart_drawer` | `true` |

### Implementación

El cart drawer se renderiza en `snippets/header-actions.liquid` usando Web Components Heritage:

- `<cart-drawer-component>` — lógica en `assets/cart-drawer.js`
- `<cart-icon>` — lógica en `assets/cart-icon.js`
- Contenido: `snippets/cart-products.liquid`, `snippets/cart-summary.liquid`, `snippets/cart-items-component.liquid`

### Customizaciones detectadas

| Cambio | Ubicación |
|---|---|
| Heading custom "Your order" | `snippets/header-actions.liquid` — reemplaza heading Heritage traducido |
| Clases tipográficas WAGON (`he-iv`, `sp-xxs`, `uppercase`) en cart icon | `snippets/header-actions.liquid` |
| Texto "Cart" visible junto al icono | `snippets/header-actions.liquid` — `.text-cart` |
| Estilos extensos del drawer (quantity selector, header, summary, view transitions) | `snippets/header-actions.liquid` — bloque `{% stylesheet %}` |
| Overrides adicionales (empty state, links) | `assets/wagon.css`, `assets/home.css` |
| Color de links en items | `assets/wagon.css` — `.cart-drawer__items p a { color: inherit }` |

### Comportamiento

- En páginas que no son `/cart`, el icono abre el drawer modal.
- `auto_open_cart_drawer: true` — el drawer se abre tras add-to-cart (comportamiento Heritage).
- View Transitions para empty cart drawer (`view-transitions.js` + estilos en `header-actions.liquid`).

### Riesgo

El cart drawer mezcla Heritage Web Components con estilos custom en `header-actions.liquid`. Actualizaciones del theme Heritage pueden requerir reconciliar esos estilos scoped.

---

## 9. Form behavior customizations

### Patrón Shopify Contact Form

Todas las formas custom usan `{% form 'contact' %}` — envían a la API de contacto de Shopify (notificaciones por email en Admin).

### Formularios con success UX custom (`sessionStorage`)

Implementado en la segunda IIFE de `assets/wagon.js`:

| Sección | Archivo | `form_success_id` |
|---|---|---|
| Contact I | `sections/contact-i.liquid` | `contact-i-{{ section.id }}` |
| Contact II (newsletter) | `sections/contact-ii.liquid` | `contact-ii-{{ section.id }}` |
| Locator II | `sections/locator-ii.liquid` | `locator-{{ section.id }}` |

**Mecanismo:**

1. Hidden input `contact[form_success_id]` identifica el formulario
2. Hidden inputs adicionales: `contact[form_type]`, `contact[category]` (según sección)
3. En submit, JS guarda el ID en `sessionStorage.lastSubmittedContactForm`
4. Tras redirect post-submit, JS muestra `[data-form-success-message]` y oculta `[data-form-submit-button]` si el ID coincide

**Markup requerido:**

```html
<div data-form-success-wrapper
     data-form-success-id="..."
     data-form-posted="true|false">
  <button data-form-submit-button>...</button>
  <div data-form-success-message hidden>...</div>
</div>
```

### Affiliate V — patrón diferente

`sections/affiliate-v.liquid` usa `{% if form.posted_successfully? %}` en Liquid (sin `form_success_id` ni `sessionStorage`). El mensaje de éxito se renderiza server-side tras el POST.

**Riesgo:** dos patrones de success coexisten; al modificar formularios, verificar cuál aplica.

### Campos custom en contact forms

| Campo `contact[...]` | Sección |
|---|---|
| `social_handle_or_website` | Affiliate V |
| `preferred_coupon_code` | Affiliate V |
| `subject` | Contact I |

Estos campos aparecen en notificaciones de Shopify Admin como parte del body del contacto.

---

## 10. Store Locator — notas técnicas

### Template

`templates/page.locator.json` — orden: `locator-i` → `locator-ii`

### Locator I — mapa (Stockist)

**Archivo:** `sections/locator-i.liquid`

- Widget: elemento custom `<stockist-store-locator data-stockist-widget-tag="...">`
- Tag live: `map_r3mn7jvq` (setting `stockist_widget_tag`)
- Script externo: `https://stockist.co/embed/v1/widget.min.js` (async, condicional por `load_stockist_script`)
- Contenedor con `data-lenis-prevent` — evita que Lenis capture scroll dentro del widget
- **Las ubicaciones de tienda no están en el theme** — gestionadas en el dashboard de Stockist

### Locator II — formulario

**Archivo:** `sections/locator-ii.liquid`

- Formulario `{% form 'contact' %}` con categoría "Store locator inquiry"
- Usa el patrón `sessionStorage` de success (ver sección 9)
- Email hardcoded en setting: `hello@jagprovisions.com`

### Dependencias externas

| Servicio | Carga | Riesgo |
|---|---|---|
| Stockist embed JS | CDN `stockist.co` | Si Stockist cambia API o el tag expira, el mapa deja de funcionar hasta actualizar configuración en Stockist/section settings |

---

## 11. PDP — notas técnicas

### Estructura del template

`templates/product.json` — secciones activas en orden:

1. **`pdp-i`** (`sections/pdp-i.liquid`) — hero PDP: galería Splide, info producto, buy buttons Heritage
2. **`pdp-ii`** (`sections/pdp-ii.liquid`) — Key Ingredients
3. **`pdp-iii`** (`sections/pdp-iii.liquid`) — Tabs (Nutrition / Collagen / Seed Oil) vía Bootstrap
4. **`pdp-recommendations`** (`sections/pdp-recommendations.liquid`) — grid de productos relacionados

`product-information` (Heritage) está en el JSON pero **`disabled: true`**.

### Metafields de producto usados en PDP

| Namespace/key | Sección | Uso |
|---|---|---|
| `custom.pdp_intro` | PDP I | Texto intro (fallback: description) |
| `custom.flavor_profile_title`, `flavor_profile_text` | PDP I | Bloque flavor profile |
| `custom.price_suffix_label` | PDP I | Sufijo de precio (ej. "/ bar") |
| `custom.pdp_highlights` | PDP I | Lista de highlights |
| `custom.color_primary` … `color_light` | PDP I | CSS inline por flavor |
| `custom.bars` | Shopping, PDP recommendations | Badge "12 BARS" |
| `custom.shopping_media` | Shopping | Imagen hover en cards |
| `custom.key_ingredients` | PDP II | Lista de ingredientes |
| `custom.key_ingredients_image`, `key_ingredients_image_responsive` | PDP II | Imágenes desktop/mobile |
| `custom.nutritional_facts_files` | PDP III | Imágenes nutrition facts (list metaobject/file) |

### Galería de medios — offset

En `sections/pdp-i.liquid`:

```liquid
{% for media in sorted_media offset: 1 %}
```

**La primera imagen del producto Media no aparece en el carrusel PDP.** Suele usarse como imagen principal en cards/SEO. Documentado como comportamiento intencional o legacy — confirmar antes de cambiar.

### Splide PDP (inline)

- Main carousel: `#pdp-main-splide-{{ section.id }}`
- Thumbnails: `#pdp-thumbs-splide-{{ section.id }}` — sync con main
- Init independiente de `wagon.js` — montaje con retry si Splide CDN tarda

### Variant sync custom

Script inline en `pdp-i` sincroniza radio buttons `[data-pdp-option-input]` con:

- `input[name="id"]` del `product-form-component`
- Precio en `[data-pdp-price]` vía `Shopify.formatMoney`

### Seal Subscriptions

La app inyecta DOM (`.sealsubs-container`) en el formulario de compra. `pdp-i.liquid` incluye:

- CSS overrides para `.sealsubs-container` (inline `<style>`)
- JS que reubica el widget dentro de `add-to-cart-component`

**Riesgo:** cambios en el DOM de Seal Subscriptions pueden romper el posicionamiento sin actualizar el script inline.

### Buy buttons

PDP I usa bloque Heritage `buy-buttons` (definido en `blocks/buy-buttons.liquid`) con sub-bloques `quantity`, `add-to-cart`, `accelerated-checkout` — renderiza `product-form-component` Heritage.

---

## 12. Homepage — notas técnicas

### Template

`templates/index.json` — 4 secciones, sin carousel Heritage ni announcement bar.

| Orden | Sección | Archivo | Datos clave |
|---|---|---|---|
| 1 | Superhero | `sections/superhero.liquid` | Heading, CTA, 3 bar PNGs, decor SVGs |
| 2 | Why | `sections/why.liquid` | 4 blocks `item` con iconos SVG |
| 3 | Shopping | `sections/shopping.liquid` | Grid productos de colección |
| 4 | Our Mission | `sections/ourmission.liquid` | Copy + CTA + 6 decor SVGs |

### Shopping — lógica de productos

`sections/shopping.liquid`:

- Colección: `section.settings.collection` — si vacío, fallback a `collections['frontpage']`
- Límite: `products_limit` (default 6 en homepage live)
- Badge: `product.metafields.custom.bars` — **no** usa `section.settings.badge_text` (setting existe en schema pero no se referencia en Liquid)
- Hover image: `product.metafields.custom.shopping_media`

### Hero — animación

- Clase raíz: `.superhero-i`
- Entrance: CSS-driven vía `.hero-ready` (añadida por `wagon.js` → `initPageEntrance` / `initSuperheroEntrance`)
- Estilos de posicionamiento de bars/decor: `assets/home.css`
- **No usa** atributos `data-anim` en `superhero.liquid` — animación puramente CSS

### Why — blocks

Cada block `item` en Theme Editor: `icon` (image_picker), `title`, `text`.

---

## 13. Funcionalidad no estándar de Shopify

| Funcionalidad | Implementación | Notas |
|---|---|---|
| **Doble stack CSS/JS** | Heritage + WAGON + CDN | Mayor superficie de dependencias; orden de carga importa (`master.css` después de Bootstrap) |
| **Smooth scroll Lenis** | `assets/wagon.js` | Solo desktop; desactivado en touch |
| **Adobe Typekit** | `layout/theme.liquid` | Fuentes Obviously no son Shopify Fonts — dependen de kit externo `rji3kyn` |
| **Bootstrap 5 en theme Liquid** | CDN en `layout/theme.liquid` | No es patrón Heritage estándar; usado para grid y tabs PDP |
| **jQuery global** | CDN en `layout/theme.liquid` | Cargado pero sin uso detectado en custom JS |
| **Stockist embed** | `sections/locator-i.liquid` | Custom element + script externo |
| **Seal Subscriptions DOM manipulation** | `sections/pdp-i.liquid` inline JS | Integración por posicionamiento DOM, no app block theme |
| **UpPromote Affiliate** | App block en `config/settings_data.json` | **Needs Shopify Admin confirmation** para estado activo |
| **View Transitions** | `assets/view-transitions.js`, settings activos | Transiciones entre páginas/producto |
| **Web Components Heritage** | `cart-drawer-component`, `product-form-component`, etc. | Patrón ES modules + custom elements |
| **Form success con sessionStorage** | `assets/wagon.js` | Workaround para múltiples forms contact en sitio |
| **Style guide interno** | `sections/master.liquid` + `templates/page.master.json` | Página de referencia de clases WAGON |
| **Footer custom** | `sections/foot.liquid` reemplaza `sections/footer.liquid` Heritage | Menús Shopify: `shop`, `learn`, `support` |
| **Header menu style `featured_products`** | `sections/header-group.json` | Menú Heritage con productos destacados en dropdown |
| **Search deshabilitado en header** | `show_search: false` en header settings | Search modal/snippet sigue en layout pero sin trigger visible |
| **Colores PDP por metafield** | Inline CSS en `pdp-i.liquid` | Cada flavor puede tener paleta propia |

---

## 14. Maintenance notes

### Actualizar contenido (sin código)

- **Theme Editor** — secciones custom en homepage, páginas, header, footer
- **Products → Metafields** — datos PDP (colores, ingredientes, badges)
- **Content → Files** — imágenes de marca (ver `06-assets-inventory.md`)
- **Stockist dashboard** — ubicaciones del mapa (no en theme)
- **Apps** — Seal Subscriptions, UpPromote (config en Shopify Admin)

### Actualizar código del theme

Flujo recomendado:

1. Clonar/descargar theme desde Shopify Admin o CLI
2. Editar en entorno de desarrollo (theme dev / branch)
3. Probar: homepage entrance, PDP Splide + add-to-cart + subscriptions, cart drawer, forms contact, locator map
4. Publicar theme

### Heritage theme upgrades

El theme base es Heritage 3.5.1. Al actualizar:

- Revisar conflictos en `layout/theme.liquid` (bloque WAGON añadido)
- Revisar `snippets/header-actions.liquid` si Heritage cambia cart drawer markup
- No sobrescribir secciones custom (`superhero`, `pdp-*`, `about-*`, etc.)
- Comparar `snippets/scripts.liquid` y `assets/base.css` con la nueva versión

### Dependencias CDN — versiones fijadas

| Recurso | Versión actual |
|---|---|
| Bootstrap | 5.3.3 |
| jQuery | 3.6.0 |
| GSAP | 3.12.5 |
| Lenis | 1.0.44 |
| Splide | 4.1.4 |
| Splitting | 1.0.6 |

Actualizar versiones CDN requiere regresión visual en animaciones y carruseles.

### Typekit

El kit `rji3kyn` debe permanecer activo en la cuenta Adobe vinculada. Si expira, las fuentes Obviously dejan de cargar en páginas custom.

### Checklist post-cambio

- [ ] Homepage hero anima correctamente (`.hero-ready`)
- [ ] Shopping grid muestra badges y hover media
- [ ] PDP: carrusel, variantes, precio, Seal widget visible
- [ ] Add to cart abre drawer
- [ ] Forms contact/affiliate/locator envían y muestran success
- [ ] Store locator mapa carga (Stockist)
- [ ] Mobile: sin errores JS, animaciones fallback funcionan

---

## 15. Developer notes

### Convenciones WAGON

- Clases tipográficas: `.he-*` (headings), `.bo-*` (body), `.sp-*` (UI/prices), `.tag-i` (badges)
- Layout: `.container-i`, `.flex-i`, `.gap-*`, grid Bootstrap (`w-12`, `w-lg-6`)
- Botones: `.btn-i` + variantes `.btn-pink-i`, `.btn-pink-ii`, `.outline-i`
- Secciones custom usan sufijo de clase descriptivo: `.superhero-i`, `.shopping-i`, `.pdp-i`, etc.

### Puntos de extensión seguros

| Tarea | Dónde |
|---|---|
| Nueva animación scroll | Añadir handler en `assets/wagon.js` → `initScope()` |
| Estilos nueva sección custom | `assets/wagon.css` o `assets/home.css` |
| Variables de color/tipo | `assets/master.css` `:root` |
| Nueva sección editable | Crear `sections/nombre.liquid` con `{% schema %}` |
| Cargar nueva librería CDN | `layout/theme.liquid` (mantener orden: libs → wagon assets → wagon.js) |

### Puntos frágiles (documentar, no dramatizar)

| Item | Detalle |
|---|---|
| `offset: 1` en PDP media loop | Primera imagen excluida del carrusel |
| Dos inits Splide | `wagon.js` (`data-splide`) + inline en `pdp-i` — no unificados |
| Seal DOM coupling | `pdp-i.liquid` asume selectores `.sealsubs-container` |
| Typo CSS posible | `var(--color-aazul-oscuro)` en `assets/master.css` `.btn-i.outline-i` |
| Albert Sans referenciada | `assets/master.css` `.btn-ii`/`.btn-iii` — fuente no cargada en layout |
| Email inconsistente | `hello@jagprovisions.com` (contact, locator) vs `hello@japrovisions.com` (affiliate setting) — verificar cuál es correcto |
| `badge_text` en Shopping schema | Setting sin uso en Liquid |
| Header height inline script | Duplica lógica de `assets/utilities.js` — comentario en `theme.liquid` pide mantener sincronía |
| Lenis + Stockist | `data-lenis-prevent` en locator — necesario para scroll del widget |

### Debugging

- Consola: mensajes `[wagon]` en warnings de librerías no cargadas
- `window.WagonTheme.refresh()` — forzar ScrollTrigger refresh
- `window.WagonTheme.getLenis()` — verificar si Lenis está activo
- Theme Editor: `shopify:section:load` re-inicializa animaciones en sección editada
- PDP: warning `PDP I: Splide no estuvo disponible a tiempo.` si CDN falla

### Herramientas de desarrollo

| Herramienta | Uso |
|---|---|
| Shopify CLI | `shopify theme dev` para preview local |
| Theme Check | Lint Liquid (theme-check-disable presente en `theme.liquid` para CDN) |
| `assets/jsconfig.json` | Aliases `@theme/*` para IDE |

### Archivos que no modificar sin revisión amplia

- `assets/base.css` — core Heritage
- `snippets/scripts.liquid` — importmap completo
- `snippets/theme-styles-variables.liquid` — variables globales Heritage
- `assets/cart-drawer.js`, `assets/product-form.js` — Web Components core

### Credenciales y accesos

**To be provided by JAG Provisions / Wagon:**

- Shopify Admin (theme editor, products, files)
- Adobe Typekit (kit `rji3kyn`)
- Stockist dashboard (widget tag `map_r3mn7jvq`)
- Seal Subscriptions app admin
- UpPromote app admin

---

## Referencia rápida de rutas críticas

```
layout/theme.liquid              ← Entry point global
config/settings_data.json        ← Settings live (cart, logo, fonts, schemes)
config/settings_schema.json      ← Theme info Heritage 3.5.1

templates/index.json             ← Homepage
templates/product.json             ← PDP
templates/page.locator.json      ← Store Locator

assets/wagon.js                  ← Animaciones + forms success
assets/master.css                ← Design system
assets/wagon.css                 ← Section styles
assets/home.css                  ← Hero + PDP + cart overrides

sections/pdp-i.liquid            ← PDP core + Splide + Seal
sections/locator-i.liquid        ← Stockist embed
sections/shopping.liquid         ← Product grid homepage
sections/foot.liquid             ← Footer custom

snippets/header-actions.liquid   ← Cart drawer custom
snippets/scripts.liquid          ← Heritage JS modules
```

---

*Última actualización: Junio 2026 · Basado en auditoría del codebase del theme Just Nosh*
