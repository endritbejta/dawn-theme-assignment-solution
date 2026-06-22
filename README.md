# Custom Bundle Set Creator — Shopify Section

A self-contained Shopify section that lets customers build a product bundle from a curated list, preview their selection with tiered discounts, and add the entire bundle to the cart in one click.

## Demo

**Video walkthrough:** [Watch on Loom](https://www.loom.com/share/0d91753f124244109ee6941b0104516d)

**Live store:** [endrit-flower-shop.myshopify.com](https://endrit-flower-shop.myshopify.com/) — Password: `endrit`

---

## Files

| File | Location | Purpose |
|------|----------|---------|
| `custom-bundle-set-creator.liquid` | `sections/` | Bundle builder section |
| `cart-drawer.liquid` | `snippets/` | Cart drawer with bundle grouping |
| `main-cart-items.liquid` | `sections/` | Cart page with bundle grouping |
| `header.liquid` | `sections/` | Bundle-aware cart count |
| `cart-icon-bubble.liquid` | `sections/` | Bundle-aware cart icon bubble |

---

## Setup

### 1. Create the Bundle Placeholder Product

In Shopify Admin, create a product:

- **Title:** `Custom Bundle`
- **Price:** `€0.00`
- **Track inventory:** Unchecked
- **Status:** Active on the Online Store channel

### 2. Add Files to Your Theme

1. Copy `custom-bundle-set-creator.liquid` → `sections/`
2. Replace `snippets/cart-drawer.liquid`
3. Replace `sections/main-cart-items.liquid`
4. Replace `sections/header.liquid`
5. Replace `sections/cart-icon-bubble.liquid`

### 3. Configure in Theme Customizer

1. Add the **"Bundle set creator"** section to your page
2. Assign your products (up to 6)
3. Assign the **Custom Bundle** placeholder product
4. Optionally enable category filtering and mobile auto-open

### 4. Create Automatic Discounts

In Shopify Admin → Discounts, create two automatic discounts:

- **10% OFF** — Amount off products, 10%, minimum quantity of 3
- **15% OFF** — Amount off products, 15%, minimum quantity of 5

These apply automatically at checkout — no coupon code needed.

---

## Discount Implementation

Discounts are tiered based on bundle item count:

| Items | Discount |
|-------|----------|
| 1 | Cannot add to cart (minimum 2) |
| 2 | No discount |
| 3–4 | 10% off |
| 5+ | 15% off |

Configured in the `DISCOUNT_TIERS` array in the section's JavaScript:

```javascript
const DISCOUNT_TIERS = [
  { qty: 3, percent: 10 },
  { qty: 5, percent: 15 }
];
```

Bundle items are linked via `_bundle_id`, `_bundle_parent`/`_bundle_child`, and `_bundle_discount` line item properties. The cart drawer and cart page group items server-side using Liquid — no client-side DOM manipulation needed for the grouping.

---

## Limitations

1. **Discounts require matching Shopify automatic discounts** — the bundle UI shows a preview; the actual reduction is applied by Shopify at checkout
2. **Items are separate line items at checkout** — bundle grouping is a presentation layer in the cart only
3. **Built for Dawn theme** — other themes may need CSS/markup adjustments
4. **Placeholder product must stay on Online Store channel** — removing it causes a JS error
5. **Max 6 products per bundle** (configurable in schema)
