# Original 1% by DYNAMISE — Shopify Theme Implementation Plan

**Theme:** Horizon export (`originalonepercent-com-horizon`, 10 MAY 2026)
**Working directory:** `theme_export__originalonepercent-com-horizon__10MAY2026-0954am/`
**Model:** claude-sonnet-4-6
**Prepared:** 2026-05-10

---

## Brand Quick-Reference

| | |
|---|---|
| **Voice** | Warm, grounded, first-person founder. Lead with emotion, close with clinical credibility. |
| **Use** | "ritual," "made by hand," "formulated by," "in our own facility," "the part of you that's irreplaceable" |
| **Avoid** | "synergy," "wellness journey," "magical," "potions," "elixirs," "vibes" |
| **Cosmic naming** | Reserved for existing product names only — do not coin new cosmic adjectives |
| **Hero product** | Cosmic Meditation Set — $149 CAD |
| **Primary audience** | Women 35–55, HHI $80K–$160K, Canadian metros, ingredient-aware |
| **Secondary** | Cancer survivors / chemo-sensitive — fragrance-free, post-treatment-safe |
| **Tertiary** | Gift buyers, $100–$200 bracket |

---

## Global Constraints (apply to every phase)

- **Mobile-first.** All sections must be fully functional and readable at 375 px.
- **No broken existing flows.** Checkout, cart drawer, and product variant logic must not be touched without explicit scope.
- **No hardcoded copy** the merchant will want to change — use section schema settings or metafields.
- **Semantic HTML + WCAG AA.** Alt text, aria-labels, ≥4.5:1 body contrast, ≥3:1 large-text contrast, visible focus states.
- **Performance.** `loading="lazy"` on all below-the-fold images, no render-blocking JS, respect existing asset pipeline.
- **No fabrication.** No fake reviews, invented certifications, or fictional press. Placeholders must be labelled.
- **No third-party app code.** Leave `<!-- APP EMBED -->` comments only.
- **No tracking pixels** without explicit merchant approval.

---

## Phase 1 — Homepage Redesign

### Objective
Replace the current homepage section stack with a founder-led, conversion-optimised sequence that communicates brand story, product range, and clinical credibility in one scroll.

### New `templates/index.json` section order

| Slot | Section file | Purpose |
|------|-------------|---------|
| 1 | `sections/hero-founder.liquid` | Full-bleed hero, primary CTA |
| 2 | `sections/trust-pillars.liquid` | 3-icon trust strip |
| 3 | `sections/rituals.liquid` | 3 bundle cards |
| 4 | `sections/founder-strip.liquid` | Founder story, 2-column |
| 5 | `sections/featured-products.liquid` | 4 product cards |
| 6 | `sections/reviews-wall.liquid` | Community reviews (app placeholder) |
| 7 | `sections/books-feature.liquid` | Book showcase + lead magnet hook |
| 8 | `sections/email-capture.liquid` | Email opt-in |
| 9 | `sections/certifications.liquid` | ECOCERT, Health Canada, ISO, Made in Canada |

### Files to create / modify

#### `templates/index.json` *(modify)*
Rebuild the `order` array and `sections` map to match the nine-slot sequence above. Preserve any existing section IDs that remain in use elsewhere.

#### `sections/hero-founder.liquid` *(create)*
- Full-bleed background image with dark overlay (opacity controlled via schema).
- **H1:** "Skincare and stillness, made by hand in Canada."
- **Subhead:** "Formulated by a certified esthetician and breast cancer survivor in our own Health-Canada-registered Canadian facility."
- **Primary CTA:** "Shop the Cosmic Meditation Set" → `/products/cosmic-meditation-set`
- **Secondary CTA:** "Read our story" → `/pages/about`
- Comment in markup: `<!-- REPLACE: founder portrait, soft natural light -->`
- **Schema settings:** `heading`, `subheading`, `cta_primary_text`, `cta_primary_link`, `cta_secondary_text`, `cta_secondary_link`, `background_image`, `overlay_opacity` (range 0–80), `text_alignment` (left / center).
- Mobile: stack CTAs vertically, reduce H1 to ~32 px, maintain legible contrast over image.

#### `sections/trust-pillars.liquid` *(create)*
- 3-column horizontal strip (stacks to 1-col on mobile).
- Default content via schema blocks (merchant-editable):

  | Icon | Heading | Description |
  |------|---------|-------------|
  | Maple leaf SVG | Made in Canada | Hand-formulated and bottled in our Ontario lab |
  | Certificate SVG | ECOCERT & Health Canada GMP | Manufactured under independent quality certifications |
  | Esthetician badge SVG | Formulated by an Esthetician | Every product designed by a certified skincare professional |

- **Schema:** repeatable `block` type with fields: `icon_svg` (image upload), `heading`, `description`.

#### `sections/rituals.liquid` *(create)*
- 3-card grid (stacks to 1-col on mobile, 2-col at tablet).
- Each card: image, bundle name, one-line description, price, "Build this ritual" CTA.
- Default cards (merchant-editable via blocks):

  | Bundle | Description | Link |
  |--------|-------------|------|
  | The Morning Ritual | "Begin the day with five minutes of stillness and skin that feels alive." | `/pages/morning-ritual` |
  | The Inner Work Bundle | "For the seasons when the work is internal." | `/pages/inner-work` |
  | The Survivor's Set | "Made for skin that's been through something. Fragrance-free, gentle, gift-ready." | `/pages/survivors-set` |

- **Schema:** repeatable `block` with fields: `image`, `title`, `description`, `link`, `price`, `savings_badge_text`.

#### `sections/founder-strip.liquid` *(create)*
- 2-column layout: portrait left, text right. Stacks portrait-above-text on mobile.
- **Heading:** "I built this from the bottom of a hospital bed."
- **Body (~70 words, editable in schema):**
  > "After a breast cancer diagnosis, I went looking for skincare I could actually trust on my changed, sensitive skin — and couldn't find it. So I built it. Original 1% is what came out of that search: clean, esthetician-formulated, made by hand in our own Canadian facility. The 1% is the part of you that's irreplaceable. This is for that part."
- **CTA:** "Read the full story" → `/pages/about`
- **Schema:** `portrait_image`, `heading`, `body`, `cta_text`, `cta_link`.

#### `sections/featured-products.liquid` *(create or modify existing)*
- 4 product cards max. Default SKUs: Cosmic Meditation Set, Skin Reviver, From Junk to Genius, Aeon Candle.
- Prices rendered from product object — never hardcoded.
- **Schema:** `section_heading`, up to 4 `product` picker settings, `columns_desktop` (2–4).

#### `sections/reviews-wall.liquid` *(create)*
- **Heading:** "Stories from our community."
- Body: `<div><!-- Klaviyo or Judge.me reviews widget embed goes here --></div>`
- Empty-state (shown when no embed is active): "Be one of the first to share yours." + CTA to product review form.
- **Schema:** `heading`, `show_empty_state` (checkbox).

#### `sections/books-feature.liquid` *(create)*
- 2-column book showcase. Each column: cover image, title, pull-quote (editable), "Read a chapter free" CTA → email capture anchor or `/pages/free-chapter`.
- **Schema:** 2 blocks — each with `cover_image`, `title`, `pull_quote`, `cta_text`, `cta_link`.

#### `sections/email-capture.liquid` *(create)*
- **Heading:** "Get our free 7-day meditation series and clean-beauty ingredient guide."
- Single email input + submit button.
- Privacy line: "We'll never share your email. Unsubscribe anytime."
- Comment: `<!-- Klaviyo form embed goes here -->`
- **Schema:** `heading`, `body`, `button_text`, `privacy_text`, `background_color`.
- Note: the actual form submission will be handled by the Klaviyo embed; the native HTML form is a fallback placeholder only.

#### `sections/certifications.liquid` *(create)*
- Subtle horizontal strip of badge SVGs/images.
- Default badges: ECOCERT, Health Canada GMP, ISO, Made in Canada.
- **Schema:** up to 6 `block` items — each with `badge_image` (upload), `alt_text`, `link` (optional).

### Merchant actions after Phase 1
- [ ] Upload founder portrait (recommended: soft natural light, square or 16:9 crop) in Customizer → Hero Founder → Background Image.
- [ ] Verify `/products/cosmic-meditation-set` handle matches live store.
- [ ] Create `/pages/morning-ritual`, `/pages/inner-work`, `/pages/survivors-set` placeholder pages in Shopify admin (content added in Phase 4).
- [ ] Create `/pages/about` if it doesn't exist.
- [ ] Install Klaviyo (or chosen ESP) and paste embed code into the email-capture section comment slot.
- [ ] Install Judge.me or Loox and paste embed code into the reviews-wall section comment slot.
- [ ] Upload ECOCERT, Health Canada, ISO, and Made-in-Canada badge SVGs in Customizer → Certifications.

---

## Phase 2 — Navigation Restructure

### Objective
Align menus with the new content architecture. All changes are in **Shopify admin → Navigation** — no theme files control menu items directly.

### Files to create / modify (theme side)

#### `templates/page.about.json` *(create if missing)*
Scaffold page template so the merchant can publish an About page that renders with the theme's standard page section.

#### `templates/page.reviews.json` *(create if missing)*
Scaffold reviews landing page template.

#### `templates/page.ingredient-philosophy.json` *(create if missing)*
#### `templates/page.sensitivity-guide.json` *(create if missing)*
#### `templates/page.faq.json` *(create if missing)*
#### `templates/page.wholesale.json` *(create if missing)*

Each scaffold uses the theme's existing `main-page` section so the merchant can author content in the rich-text editor. No custom section needed until Phase 3+.

### Merchant actions after Phase 2

**Main menu** — rebuild in admin → Navigation → Main menu:

| Label | URL |
|-------|-----|
| Shop the Rituals | `/collections/rituals` |
| Skincare | `/collections/skincare` |
| Meditate | `/collections/meditation` |
| Read | `/collections/books` |
| Our Story | `/pages/about` |
| Reviews | `/pages/reviews` |

**Footer menu** — rebuild in admin → Navigation → Footer menu:

| Label | URL |
|-------|-----|
| Shipping & Returns | `/pages/shipping-returns` |
| Ingredient Philosophy | `/pages/ingredient-philosophy` |
| Skin Sensitivity Guide | `/pages/sensitivity-guide` |
| FAQ | `/pages/faq` |
| Wholesale | `/pages/wholesale` |
| Contact | `/pages/contact` |

**Collections to create** (if not already live):

- [ ] `rituals` — manually curated, contains the three bundle products
- [ ] `skincare` — Skin Reviver, Elysium Nebula Botanical Spray, Frank Castor, rice water toner
- [ ] `meditation` — Cosmic Meditation Set, Aeon Candle
- [ ] `books` — both titles

---

## Phase 3 — Product Page Template

### Objective
Enrich every product page with founder-voice copy, ingredient transparency, and a mobile-sticky add-to-cart — all driven by metafields so no template changes are needed per SKU.

### Files to modify

#### `sections/main-product.liquid` *(modify — or equivalent in this theme)*
Audit which file this theme uses for the product form. Likely `sections/main-product.liquid` or `sections/product-template.liquid`.

**New render order within the section:**

1. **Image gallery** — 4–6 images, swipeable on mobile (CSS scroll snap or existing theme JS), lightbox/zoom on desktop.
2. **Title, price, star-rating placeholder** — `<!-- Judge.me star rating embed -->`, review count.
3. **Primary benefit line** — `{{ product.metafields.custom.benefit_line }}`
4. **"Why this exists" founder note** — `{{ product.metafields.custom.founder_note }}`
5. **3 hero benefit icons** — rendered from `product.metafields.custom.benefits` (list metafield).
6. **Full ingredients with origin** — `{{ product.metafields.custom.ingredients_with_origin }}`
7. **Free-from list** — `{{ product.metafields.custom.free_from }}` (default: parabens, phthalates, fragrance, dyes, sulfates — shown if metafield is blank).
8. **How to use** — 3 steps from `product.metafields.custom.how_to_use`.
9. **Reviews** — `<!-- Judge.me / Loox reviews widget embed -->`
10. **"You may also like"** — Shopify Product Recommendations API (`/recommendations/products.json?product_id=...&limit=4`).

**Mobile sticky add-to-cart:**
- Sticky bar appears at `≤768px` once user has scrolled past the primary ATC button.
- Contains: product title (truncated), price, "Add to cart" button.
- Implemented with a position: sticky or IntersectionObserver approach — no third-party JS.

### Required metafield definitions
Merchant creates these in **Settings → Custom data → Products**:

| Namespace | Key | Type | Example |
|-----------|-----|------|---------|
| `custom` | `benefit_line` | Single line text | "Clinically-formulated for sensitive, post-treatment skin." |
| `custom` | `founder_note` | Multi-line text (rich text) | "I made this because I needed it…" |
| `custom` | `benefits` | List of single line text | ["Fragrance-free", "Hand-bottled", "Esthetician-formulated"] |
| `custom` | `ingredients_with_origin` | Multi-line text | "Aqua (purified water), Rosa damascena (rose) hydrosol — Bulgaria, …" |
| `custom` | `free_from` | List of single line text | ["Parabens", "Phthalates", "Synthetic fragrance", "Dyes", "Sulfates"] |
| `custom` | `how_to_use` | List of single line text | ["Apply 2–3 drops to clean skin", "Press gently with fingertips", "Follow with SPF in the morning"] |

### Merchant actions after Phase 3
- [ ] Create all six metafield definitions above in Shopify admin.
- [ ] Populate metafields for each product via admin → Products → [Product] → Metafields.
- [ ] Install Judge.me or Loox and activate product review widget.
- [ ] Test sticky ATC on an iPhone (375 px) and Android (360 px).

---

## Phase 4 — Bundle Pages

### Objective
Give each ritual bundle a dedicated landing page that tells the story, shows what's inside, and makes adding all items to cart frictionless.

### Files to create

#### `sections/bundle-page.liquid` *(create — shared template)*
Reusable section used by all three bundle page templates. Renders:

1. **Hero** — bundle name + lifestyle image (schema: `image`, `heading`, `subheading`).
2. **"What's inside"** — 3 product cards: image, name, individual price (pulled from product object via product picker settings). Never hardcode prices.
3. **Bundle price + savings badge** — schema fields: `bundle_price`, `original_price`, `savings_label` (e.g. "Save $30" or "Save 10%").
4. **"Add bundle to cart" button** — adds multiple line items via `/cart/add.js` with a JSON array payload. Comment: `<!-- If Shopify Bundles app is installed, replace this with native bundle ATC -->`.
5. **Founder ritual note** — schema: `founder_note` (multi-line text).
6. **Reviews placeholder** — `<!-- Reviews widget embed -->`.
7. **Cross-sell** — "Explore more rituals": 2 cards linking to the other two bundle pages (schema: 2 page pickers).

#### `templates/page.morning-ritual.json` *(create)*
Uses `sections/bundle-page.liquid`. Default schema values pre-filled for The Morning Ritual.

#### `templates/page.inner-work.json` *(create)*
Uses `sections/bundle-page.liquid`. Default schema values pre-filled for The Inner Work Bundle.

#### `templates/page.survivors-set.json` *(create)*
Uses `sections/bundle-page.liquid`. Default schema values pre-filled for The Survivor's Set.

### Merchant actions after Phase 4
- [ ] Create the three pages in Shopify admin → Pages: `morning-ritual`, `inner-work`, `survivors-set` — assign each to its JSON template.
- [ ] Decide on bundle fulfilment method: (a) Shopify Bundles native app, (b) a single bundle product SKU, or (c) multi-line-item JS add. Advise the developer which path was chosen before Phase 4 code begins.
- [ ] Add lifestyle images for each bundle hero in Customizer → [Bundle Page] → Hero Image.

---

## Phase 5 — Cart & Checkout Improvements

### Objective
Lift average order value and reduce abandonment with a free-shipping bar, in-cart cross-sell, and a mobile-friendly sticky checkout button.

### Files to modify

#### `sections/cart-drawer.liquid` or `snippets/cart-drawer.liquid` *(modify — audit first)*
Identify the correct file in this theme export before touching anything.

**Changes:**

1. **Free-shipping progress bar**
   - Threshold: $95 CAD (configurable via theme setting `free_shipping_threshold`).
   - Progress bar fills as cart subtotal approaches threshold.
   - Copy states: "You're $X away from free shipping." / "You've unlocked free shipping!"
   - Add `free_shipping_threshold` to `config/settings_schema.json` under a "Cart" group.

2. **Cross-sell row — "Complete your ritual"**
   - Shows 1–2 products not already in cart.
   - Source: hand-picked via a theme setting (product pickers) or a `cart.cross_sell` metafield list. Do not use a third-party recommendation API.
   - Schema setting: up to 2 product pickers labelled "Cross-sell product 1 / 2".

3. **Express checkout buttons**
   - Audit whether Shop Pay, Apple Pay, Google Pay are rendered. If they are present but visually broken, document the fix. If absent, flag for merchant (requires Shopify Payments to be enabled).

4. **Sticky cart total on mobile**
   - On `≤768px`, the subtotal + checkout button are pinned to the bottom of the viewport within the drawer so they're always reachable without scrolling.

### Merchant actions after Phase 5
- [ ] Set `free_shipping_threshold` in Customizer → Cart → Free shipping threshold (enter `95`).
- [ ] Choose cross-sell products in Customizer → Cart → Cross-sell.
- [ ] Confirm Shopify Payments is enabled for Shop Pay / Apple Pay / Google Pay to render.

---

## Phase 6 — Out-of-Stock Handling

### Objective
Capture demand from out-of-stock products instead of dead-ending customers.

### Files to modify

#### `sections/main-product.liquid` *(modify)*

- When `product.available == false`:
  - Replace ATC button with a "Notify me when it's back" email form.
  - Comment: `<!-- Klaviyo Back-in-Stock form embed goes here -->`.
  - Keep price visible — do not hide or grey it out.
  - Keep variant selector visible so customers can select their preferred variant before signing up.

#### `sections/main-collection-product-grid.liquid` *(modify — or equivalent)*
Identify the correct collection grid section in this theme export.

- Add a "Sold out" pill overlay on product cards where `product.available == false`. Do not hide these cards — they capture demand.
- Add a theme section setting `hide_long_term_oos` (checkbox, default unchecked). When checked, the merchant can manually tag products `long-term-oos` to hide them from the grid. This is a manual opt-in tool, not an automatic date filter.

### Merchant actions after Phase 6
- [ ] Install Klaviyo Back-in-Stock flow and paste the form embed into the product template comment slot.
- [ ] When a SKU is confirmed discontinued (not temporarily OOS), tag it `long-term-oos` in admin and enable the hide setting in Customizer.

---

## Phase 7 — Accessibility & Performance Pass

### Objective
Meet or exceed Lighthouse and WCAG AA targets on all key page types before launch.

### Audit targets

| Page | Performance (mobile) | Performance (desktop) | Accessibility | Best Practices | SEO |
|------|---------------------|-----------------------|---------------|---------------|-----|
| Homepage | ≥85 | ≥90 | ≥95 | ≥90 | ≥90 |
| Product page | ≥85 | ≥90 | ≥95 | ≥90 | ≥90 |
| Cart drawer open | ≥85 | ≥90 | ≥95 | ≥90 | — |
| Bundle page | ≥85 | ≥90 | ≥95 | ≥90 | ≥90 |

### Checklist of known work items

**Images**
- [ ] Every content image has a meaningful `alt` attribute.
- [ ] Decorative images have `alt=""` and `aria-hidden="true"`.
- [ ] Hero image: explicit `width` and `height` attributes (or `aspect-ratio` CSS) to eliminate CLS.
- [ ] All below-the-fold images: `loading="lazy"`.
- [ ] Certification badge SVGs have descriptive `<title>` elements.

**Interactivity**
- [ ] Every button, link, and form input has a visible focus ring (`:focus-visible` CSS, not `outline: none`).
- [ ] Every icon-only button has `aria-label`.
- [ ] Cart drawer: `role="dialog"`, `aria-modal="true"`, `aria-label="Shopping cart"`, focus trap on open, focus returns to trigger on close.
- [ ] Mobile menu: same dialog/focus-trap pattern.
- [ ] Sticky ATC bar: `aria-label="Add to cart (sticky)"` to disambiguate from the inline button.

**Colour contrast**
- [ ] Body text on all backgrounds: ≥4.5:1.
- [ ] Large text (≥18 px regular / ≥14 px bold): ≥3:1.
- [ ] CTA button text on button background: ≥4.5:1.
- [ ] "Sold out" pill: ≥4.5:1.
- [ ] Free-shipping progress bar fill colour on track: ≥3:1 (UI component threshold).

**JavaScript**
- [ ] All non-critical scripts use `defer` or `async`.
- [ ] No synchronous scripts in `<head>` except those required for theme rendering (check existing `layout/theme.liquid`).
- [ ] Bundle ATC JS (`/cart/add.js`) is inlined or loaded with `defer` — not render-blocking.

**Core Web Vitals**
- [ ] LCP element identified on homepage and product page — is it the hero image? If so, that image should use `fetchpriority="high"` and NOT `loading="lazy"`.
- [ ] No large layout shifts (CLS <0.1) from late-loading fonts or images.
- [ ] INP: no synchronous cart mutations on the main thread — use `requestAnimationFrame` / async patterns.

**SEO**
- [ ] Every page template has a unique `<title>` and `<meta name="description">` via Shopify's SEO fields.
- [ ] Product pages: existing Open Graph tags are not broken by template changes.
- [ ] New page templates include canonical tags.

### Files potentially modified in this phase
Any file touched in Phases 1–6 may require accessibility or performance fixes. The specific list will be generated from the Lighthouse + axe audit results and documented in the Phase 7 delivery summary.

---

## Post-All-Phases Deliverables

### Full file inventory (populated at project close)

| File | Status | Phase |
|------|--------|-------|
| `templates/index.json` | Modified | 1 |
| `sections/hero-founder.liquid` | Created | 1 |
| `sections/trust-pillars.liquid` | Created | 1 |
| `sections/rituals.liquid` | Created | 1 |
| `sections/founder-strip.liquid` | Created | 1 |
| `sections/featured-products.liquid` | Created/Modified | 1 |
| `sections/reviews-wall.liquid` | Created | 1 |
| `sections/books-feature.liquid` | Created | 1 |
| `sections/email-capture.liquid` | Created | 1 |
| `sections/certifications.liquid` | Created | 1 |
| `templates/page.about.json` | Created | 2 |
| `templates/page.reviews.json` | Created | 2 |
| `templates/page.ingredient-philosophy.json` | Created | 2 |
| `templates/page.sensitivity-guide.json` | Created | 2 |
| `templates/page.faq.json` | Created | 2 |
| `templates/page.wholesale.json` | Created | 2 |
| `sections/main-product.liquid` | Modified | 3, 6 |
| `sections/bundle-page.liquid` | Created | 4 |
| `templates/page.morning-ritual.json` | Created | 4 |
| `templates/page.inner-work.json` | Created | 4 |
| `templates/page.survivors-set.json` | Created | 4 |
| `sections/cart-drawer.liquid` *(or equivalent)* | Modified | 5 |
| `config/settings_schema.json` | Modified | 5 |
| `sections/main-collection-product-grid.liquid` *(or equivalent)* | Modified | 6 |
| Multiple files | A11y/perf fixes | 7 |

### Complete merchant action checklist

**Shopify admin → Custom data → Products (metafields)**
- [ ] `custom.benefit_line` — Single line text
- [ ] `custom.founder_note` — Multi-line text (rich text)
- [ ] `custom.benefits` — List of single line text
- [ ] `custom.ingredients_with_origin` — Multi-line text
- [ ] `custom.free_from` — List of single line text
- [ ] `custom.how_to_use` — List of single line text

**Shopify admin → Navigation**
- [ ] Rebuild Main menu (6 items per Phase 2 spec)
- [ ] Rebuild Footer menu (6 items per Phase 2 spec)

**Shopify admin → Collections**
- [ ] Create `rituals` collection
- [ ] Create `skincare` collection
- [ ] Create `meditation` collection
- [ ] Create `books` collection

**Shopify admin → Pages**
- [ ] `about` — assign to `page.about` template, write content
- [ ] `reviews` — assign to `page.reviews` template
- [ ] `morning-ritual` — assign to `page.morning-ritual` template
- [ ] `inner-work` — assign to `page.inner-work` template
- [ ] `survivors-set` — assign to `page.survivors-set` template
- [ ] `ingredient-philosophy` — assign to template, write content
- [ ] `sensitivity-guide` — assign to template, write content
- [ ] `faq` — assign to template, write content
- [ ] `wholesale` — assign to template, write content
- [ ] `free-chapter` — assign to template (or set up Klaviyo lead magnet redirect)

**Shopify admin → Apps**
- [ ] Install **Klaviyo** — connect store, build email welcome + back-in-stock flows, paste embed codes into: email-capture section, reviews-wall section, back-in-stock product template slot.
- [ ] Install **Judge.me** or **Loox** — enable star ratings widget on product pages and reviews wall. Paste embed codes.
- [ ] Install **Shopify Bundles** (native, free) — or decide on alternate bundle method — before Phase 4 code begins.
- [ ] Optional: **Microsoft Clarity** — requires merchant approval before any tracking script is added.

**Shopify admin → Customizer (after theme publish)**
- [ ] Upload founder portrait → Hero Founder section
- [ ] Upload ECOCERT, Health Canada, ISO, Made in Canada badges → Certifications section
- [ ] Set `free_shipping_threshold` to `95` → Cart settings
- [ ] Choose 2 cross-sell products → Cart settings
- [ ] Set lifestyle images for each bundle hero
- [ ] Verify Shopify Payments is active for express checkout buttons

**Products — populate metafields for each SKU**
- [ ] Cosmic Meditation Set
- [ ] Skin Reviver
- [ ] Elysium Nebula Botanical Spray
- [ ] Frank Castor
- [ ] Rice water toner
- [ ] Aeon Candle
- [ ] Original 1%: From Junk to Genius
- [ ] Second book title

### Launch QA checklist

**Desktop (1440 px) and mobile (375 px)**
- [ ] Homepage: all 9 sections render, images load, CTAs link correctly
- [ ] Hero: primary + secondary CTA correct destinations
- [ ] Rituals section: all 3 bundle cards link to correct bundle pages
- [ ] Founder strip: portrait displays, "Read the full story" links to `/pages/about`
- [ ] Featured products: 4 products show with correct prices from Shopify
- [ ] Books section: CTAs link to email capture or free-chapter page
- [ ] Email capture: form visible; Klaviyo embed present
- [ ] Certifications: all 4 badges display with alt text

**Navigation**
- [ ] Main menu: all 6 items present and link correctly
- [ ] Footer menu: all 6 items present and link correctly
- [ ] Collections exist and contain correct products

**Product pages**
- [ ] Metafields render on at least 2 products
- [ ] Gallery swipe works on mobile
- [ ] Sticky ATC appears when scrolling past primary ATC on mobile
- [ ] Star rating placeholder visible (or live widget if Judge.me/Loox installed)
- [ ] "You may also like" renders (requires products to be published)

**Bundle pages**
- [ ] All 3 bundle pages load without error
- [ ] "What's inside" product cards show real prices
- [ ] Multi-item ATC adds correct items to cart (verify in cart)
- [ ] Cross-sell links to the other two bundles

**Cart**
- [ ] Free-shipping bar shows and updates as items are added
- [ ] Bar resets correct threshold at $95
- [ ] Cross-sell products display (not already in cart)
- [ ] Checkout button is reachable on mobile without scrolling
- [ ] Shop Pay / Apple Pay / Google Pay buttons visible (if Shopify Payments active)

**Out-of-stock**
- [ ] Mark a test product OOS — "Notify me" form appears, price remains visible
- [ ] Collection grid shows "Sold out" pill on OOS cards

**Accessibility**
- [ ] Tab through homepage — no focus traps, no missing focus rings
- [ ] Open cart drawer — focus moves into drawer, Esc closes it, focus returns
- [ ] Run axe DevTools on homepage: zero critical issues
- [ ] Check hero text contrast with overlay at minimum opacity setting

**Performance**
- [ ] Run Lighthouse on homepage (mobile): ≥85 Performance, ≥95 Accessibility
- [ ] Run Lighthouse on a product page (mobile): same targets
- [ ] Verify hero image is NOT lazy-loaded (it's LCP)
- [ ] Verify all images below the fold ARE lazy-loaded

**Checkout (end-to-end)**
- [ ] Add Cosmic Meditation Set to cart → proceed to checkout → complete test purchase (use Shopify test gateway)
- [ ] Confirm order confirmation email received
- [ ] Confirm Klaviyo welcome/confirmation flow triggers (if Klaviyo is live)

### Suggested next-iteration items (out of scope for this engagement)

- **Subscription on consumables** — Skin Reviver, Botanical Spray, toner are strong candidates for subscribe-and-save via Recharge or Shopify Subscriptions.
- **Loyalty programme** — post-purchase points for repurchase and referral; Smile.io or Yotpo Loyalty.
- **Faire wholesale page** — dedicated `/pages/wholesale` with Faire partner link and wholesale terms.
- **Localization** — French-language store for Quebec (Shopify Markets), given Canadian metro focus.
- **Gift wrapping / gift message** — high value for the Survivor's Set and gift buyer segment; Shopify native gift options or a lightweight app.
- **Quiz / skin finder** — "Find your ritual" product quiz routing to the right SKUs; Octane AI or Typeform embed.
- **Sustainability page** — packaging materials, facility certifications, refill/return programme if applicable.
- **Press / media kit page** — for earned media outreach as brand grows.

---

*End of implementation plan. Awaiting approval to begin Phase 1.*
