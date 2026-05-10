# Original 1% by DYNAMISE — Manual Launch Checklist

Everything the developer has built is done. This file is everything **you** need to do
before the store is live. Work top to bottom — later steps depend on earlier ones.

---

## 1. Upload the theme to Shopify

1. Zip the entire theme folder.
2. Shopify admin → **Online Store → Themes → Add theme → Upload zip file**.
3. Do **not** publish yet — keep it as a preview until every step below is done.

---

## 2. Install required apps (do this first — other steps depend on them)

| App | Why you need it | Where to paste the embed |
|-----|----------------|--------------------------|
| **Klaviyo** | Email capture, back-in-stock flows, welcome series | Homepage → Email Capture section code comment · Product page → Back-in-stock section code comment |
| **Judge.me** or **Loox** | Product star ratings and review widget | Homepage → Reviews Wall section code comment · Product page → Product Reviews section code comment · Bundle pages → Reviews section code comment |
| **Shopify Bundles** (free, native) | Bundle add-to-cart on the three ritual pages — OR decide to keep the multi-line-item JS already built | Bundle page sections (swap comment if using native app) |

---

## 3. Shopify admin → Custom data → Products (metafield definitions)

Create each of these in **Settings → Custom data → Products → Add definition**:

| Namespace & key | Type | Purpose |
|----------------|------|---------|
| `custom.benefit_line` | Single line text | One-line clinical claim shown above the description |
| `custom.founder_note` | Multi-line text (rich text) | Founder's personal note on why the product exists |
| `custom.benefits` | List · Single line text | 3 benefit icons shown under the founder note |
| `custom.ingredients_with_origin` | Multi-line text | Expandable full INCI list with country of origin |
| `custom.free_from` | List · Single line text | Expandable free-from list (falls back to parabens etc. if blank) |
| `custom.how_to_use` | List · Single line text | Numbered how-to steps |

Once definitions exist, populate them for every product SKU:

- [ ] Cosmic Meditation Set
- [ ] Skin Reviver
- [ ] Elysium Nebula Botanical Spray
- [ ] Frank Castor
- [ ] Rice water toner
- [ ] Aeon Candle
- [ ] Original 1%: From Junk to Genius
- [ ] Second book title (confirm handle)

---

## 4. Shopify admin → Pages

Create each page and assign it to the matching template in the page settings:

| Page title | Handle (URL slug) | Assign to template |
|-----------|-------------------|--------------------|
| Our Story | `about` | `page.about` |
| Reviews | `reviews` | `page.reviews` |
| The Morning Ritual | `morning-ritual` | `page.morning-ritual` |
| The Inner Work Bundle | `inner-work` | `page.inner-work` |
| The Survivor's Set | `survivors-set` | `page.survivors-set` |
| Ingredient Philosophy | `ingredient-philosophy` | `page.ingredient-philosophy` |
| Skin Sensitivity Guide | `sensitivity-guide` | `page.sensitivity-guide` |
| FAQ | `faq` | `page.faq` |
| Wholesale | `wholesale` | `page.wholesale` |
| Free Chapter | `free-chapter` | any standard page or redirect to Klaviyo lead magnet |

Write the actual content for each page in the rich-text editor.

---

## 5. Shopify admin → Collections

Create these collections and add the right products to each:

| Collection title | Handle | Products to include |
|-----------------|--------|---------------------|
| Shop the Rituals | `rituals` | Morning Ritual, Inner Work, Survivor's Set bundle products |
| Skincare | `skincare` | Skin Reviver, Botanical Spray, Frank Castor, Rice Toner |
| Meditate | `meditation` | Cosmic Meditation Set, Aeon Candle |
| Read | `books` | From Junk to Genius, second book title |

---

## 6. Shopify admin → Navigation

Rebuild both menus completely — the current menus will not match the new site structure.

**Main menu** (admin → Navigation → Main menu):

| Label | Link |
|-------|------|
| Shop the Rituals | `/collections/rituals` |
| Skincare | `/collections/skincare` |
| Meditate | `/collections/meditation` |
| Read | `/collections/books` |
| Our Story | `/pages/about` |
| Reviews | `/pages/reviews` |

**Footer menu** (admin → Navigation → Footer menu):

| Label | Link |
|-------|------|
| Shipping & Returns | `/pages/shipping-returns` |
| Ingredient Philosophy | `/pages/ingredient-philosophy` |
| Skin Sensitivity Guide | `/pages/sensitivity-guide` |
| FAQ | `/pages/faq` |
| Wholesale | `/pages/wholesale` |
| Contact | `/pages/contact` |

---

## 7. Shopify admin → Online Store → Customizer

Open the theme preview in the Customizer and set each of these.

### Homepage

**Hero — Founder section**
- [ ] Upload founder portrait (soft natural light, 2400 × 1600 minimum) → Background image
- [ ] Confirm primary CTA links to `/products/cosmic-meditation-set` (should be pre-filled)
- [ ] Confirm secondary CTA links to `/pages/about`

**Rituals section**
- [ ] Upload lifestyle image for The Morning Ritual card
- [ ] Upload lifestyle image for The Inner Work Bundle card
- [ ] Upload lifestyle image for The Survivor's Set card
- [ ] Set the link for each card to `/pages/morning-ritual`, `/pages/inner-work`, `/pages/survivors-set`
- [ ] Set the price display for each card (cosmetic only — e.g. "From $149 CAD")
- [ ] Add savings badge text if applicable (e.g. "Save $30")

**Founder Strip section**
- [ ] Upload founder portrait (4:5 crop recommended) → Portrait image
- [ ] Confirm CTA link goes to `/pages/about`

**Featured Products section**
- [ ] Add products to the product list picker: Cosmic Meditation Set, Skin Reviver, From Junk to Genius, Aeon Candle (or your preferred 4)

**Books Feature section**
- [ ] Upload cover image for *From Junk to Genius*
- [ ] Upload cover image for the second book
- [ ] Fill in subtitle and pull quote for each book
- [ ] Set CTA links to `/pages/free-chapter` or your Klaviyo lead magnet URL

**Certifications section**
- [ ] Upload ECOCERT badge image
- [ ] Upload Health Canada GMP badge image
- [ ] Upload ISO badge image
- [ ] Confirm Made in Canada badge (SVG already in assets as `cert-canada.svg` — upload it via the picker)

### Cart settings

- [ ] Customizer → Cart (or Theme settings) → Set **Free shipping threshold** to `95`
- [ ] Set **Free shipping progress text** — e.g. "You're {{ remaining }} away from free shipping."
- [ ] Set **Free shipping unlocked text** — e.g. "You've unlocked free shipping! 🎉"
- [ ] Set **Cross-sell product 1** — pick a product not likely to already be in cart
- [ ] Set **Cross-sell product 2** — same
- [ ] Set **Cross-sell heading** — e.g. "Complete your ritual"

### Bundle pages (open each in Customizer)

**The Morning Ritual** (`/pages/morning-ritual`):
- [ ] Upload hero lifestyle image
- [ ] Confirm the three products are set (Cosmic Meditation Set, Rice Toner, Rice Silk Serum — or your preferred three)
- [ ] Set bundle price if offering a discount (leave blank to sum individual prices)
- [ ] Fill in cross-sell card images for the other two ritual pages

**The Inner Work Bundle** (`/pages/inner-work`):
- [ ] Same as above — hero image, products, price, cross-sell images

**The Survivor's Set** (`/pages/survivors-set`):
- [ ] Same as above

### Product pages

- [ ] Open any product page in Customizer → confirm **Back-in-stock** section is visible (it only appears when the product is marked out of stock — mark a test product OOS to verify)
- [ ] Confirm **Sticky ATC bar** appears on mobile scroll (test on a real phone or responsive preview)

---

## 8. Shopify Payments

- [ ] Confirm Shopify Payments is enabled — **Settings → Payments**
- [ ] If enabled, Shop Pay, Apple Pay, and Google Pay express checkout buttons will appear automatically at checkout and in the cart. No theme change needed.
- [ ] If not enabled, customers will not see accelerated checkout buttons. Consider enabling before launch.

---

## 9. Out-of-stock tagging

The theme supports hiding long-discontinued products from collection grids. To use it:

- [ ] For any product that is permanently discontinued (not just temporarily OOS), add the tag `long-term-oos` in the product admin
- [ ] In Customizer → the relevant collection page → enable **"Hide long-term out-of-stock products"**
- [ ] Temporarily OOS products should NOT have this tag — they will show a "Sold out" pill automatically and the back-in-stock form will appear on their product page

---

## 10. Klaviyo flows to build (after Klaviyo is installed)

- [ ] **Welcome series** — triggered on newsletter signup from the homepage email capture form
- [ ] **Back-in-stock alert** — triggered when a product tagged with a variant ID comes back in stock (the native Shopify customer form on the product page stores the product handle and variant ID in the contact's tags)
- [ ] **Abandoned cart** — standard Klaviyo flow
- [ ] **Post-purchase** — thank-you sequence linking to the free chapter page and the founder's story

---

## 11. Judge.me / Loox (after review app is installed)

- [ ] Enable the product star rating widget and confirm it appears on product pages
- [ ] Paste the reviews widget embed code into:
  - Homepage → Reviews Wall section (replace the `<!-- APP EMBED -->` comment in the section code)
  - Product page → Product Reviews section (same)
  - Each bundle page → Reviews section (same)
- [ ] Import any existing reviews from your previous platform if applicable
- [ ] In Customizer, disable the **"Show empty-state text"** checkbox in each reviews section once the widget is live

---

## 12. Pre-launch QA checklist

Run through this on both desktop (1440 px) and mobile (375 px — a real iPhone if possible).

**Homepage**
- [ ] All 9 sections render with no broken images or missing text
- [ ] Hero primary CTA → correct destination
- [ ] Hero secondary CTA → `/pages/about`
- [ ] All 3 ritual cards link to the correct bundle pages
- [ ] Founder strip portrait loads and CTA links to About
- [ ] Featured products show real prices from Shopify
- [ ] Both book covers load; CTAs link to the free chapter
- [ ] Klaviyo email capture embed is live OR the fallback Shopify form is visible
- [ ] All 4 certification badges display

**Navigation**
- [ ] Main menu: all 6 items present and link correctly
- [ ] Footer menu: all 6 items present and link correctly
- [ ] Mobile hamburger menu opens and closes cleanly
- [ ] All linked collections and pages exist and have content

**Product pages**
- [ ] Metafields render on at least 2 products (benefit line, founder note, ingredients, free-from, how-to)
- [ ] Gallery swipe works on mobile
- [ ] Sticky ATC bar appears on mobile after scrolling past the buy button
- [ ] Mark a product as out of stock → confirm "Notify me" form appears and price stays visible
- [ ] Back-in-stock form submits without error
- [ ] "You may also like" recommendations render

**Bundle pages**
- [ ] All 3 bundle pages load
- [ ] Product images and real prices appear in "What's inside"
- [ ] Add bundle to cart → all items appear in cart
- [ ] Cross-sell cards link to the other two bundle pages

**Cart**
- [ ] Free-shipping bar shows and updates as items are added
- [ ] Threshold resets at $95 CAD
- [ ] Cross-sell products display (and disappear when added)
- [ ] On mobile: checkout button is visible without scrolling
- [ ] Shop Pay / Apple Pay buttons appear (if Shopify Payments active)

**Collection grid**
- [ ] "Sold out" pill appears on OOS product cards
- [ ] Adding `long-term-oos` tag + enabling the Customizer setting hides the product

**Accessibility spot check**
- [ ] Tab through the homepage — every interactive element gets a visible focus ring
- [ ] Open cart drawer — focus moves inside; Escape closes it; focus returns to the cart icon
- [ ] Run axe DevTools browser extension on the homepage: zero critical issues
- [ ] Check hero section: text is readable at minimum overlay opacity (40% default)

**Performance spot check**
- [ ] Run Lighthouse (mobile) on the homepage: target ≥ 85 Performance, ≥ 95 Accessibility
- [ ] Run Lighthouse on a product page: same targets
- [ ] Hero image is NOT lazy-loaded (verify in DevTools Network tab — it should load immediately)
- [ ] Product card images below the fold ARE lazy-loaded

**End-to-end checkout**
- [ ] Add Cosmic Meditation Set to cart → proceed to checkout → complete a test purchase using Shopify's Bogus Gateway
- [ ] Confirm order confirmation email is received
- [ ] Confirm Klaviyo post-purchase flow triggers (if Klaviyo is live)

---

## 13. Go live

- [ ] All QA items above checked off
- [ ] All app embeds pasted in (Klaviyo, reviews widget)
- [ ] DNS pointed to Shopify (if using a custom domain — Settings → Domains)
- [ ] Shopify admin → Online Store → Themes → **Publish** the new theme
- [ ] Monitor for 404 errors in Analytics → the old URL structure may have changed
- [ ] Post-launch: set up Google Search Console and submit your sitemap (`yourdomain.com/sitemap.xml`)

---

## Suggested next steps after launch (out of scope for this build)

- **Subscriptions** on Skin Reviver, Botanical Spray, Rice Toner — Recharge or Shopify Subscriptions
- **Loyalty programme** — Smile.io or Yotpo, works well with the survivor/community audience
- **Wholesale page** with Faire partner link
- **French-language store** for Quebec via Shopify Markets
- **Gift wrapping / gift message** — especially valuable for the Survivor's Set
- **Skin finder quiz** — "Find your ritual" using Octane AI or Typeform embed
- **Microsoft Clarity** heatmaps — add only after getting customer consent (PIPEDA compliance for Canada)
