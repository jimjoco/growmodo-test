# Shopify Theme Development Documentation

## Project Overview

Store: `growmodo-test`
Theme: Custom development on top of the base Shopify theme
Branch: `main`

---

## Development Workflow

### Tools Used

- **Shopify CLI** — local development and theme preview
- **Git + GitHub** — version control and deployment pipeline
- **VS Code** — code editing

### Local Development Setup

A two-theme workflow was established to separate development from production:

| Theme | Purpose |
|---|---|
| Dev/Preview theme | Connected to `shopify theme dev` for live local previewing |
| Production theme | Connected to the GitHub repository; only updates on `git push` |

**Dev command used:**
```bash
shopify theme dev -s growmodo-test-v8hlpgtg -t 189567304043 --live-reload full-page
```

- `-s` targets the specific store
- `-t` targets the dev theme ID (not the live theme)
- `--live-reload full-page` ensures full page reloads on file save, which correctly reflects Liquid changes

**Deployment flow:**
1. Edit files locally
2. Preview changes on the dev theme via `shopify theme dev`
3. When satisfied, commit and push to GitHub
4. GitHub integration automatically deploys to the production theme

---

## Customizations Made

### 1. Inline Header Search Bar

**File modified:** `snippets/search.liquid`

Replaced the default popup/modal search trigger button with a fully inline predictive search bar rendered directly in the header.

Key changes:
- Replaced `<search-button>` (modal trigger) with a `<predictive-search-component>` form rendered inline
- Search input uses `data-section-id="predictive-search"` to power predictive results via Shopify's section renderer
- Predictive results dropdown is hidden by default and only appears on `:focus-within` — so it activates only when the customer interacts with the input
- `load_empty_state: false` prevents products from rendering on page load
- Placeholder text set to: `Search for products...`
- Reset/clear button hidden on desktop, visible on mobile only

### 2. Custom Icons

**File modified:** `snippets/search.liquid`, `snippets/header-actions.liquid`

Replaced the default theme icons with custom SVG assets. Icons are referenced via `inline_asset_content` so they inherit `currentColor` from the header's text color.

| Icon | Asset File |
|---|---|
| Search | `assets/icon-svg-search.svg` |
| Cart | `assets/icon-svg-cart.svg` |
| Account | `assets/icon-svg-account.svg` |

The account icon was previously a hardcoded inline SVG — it was replaced with an asset reference to match the same pattern as the other icons.

### 3. Header Actions Order

**File modified:** `snippets/header-actions.liquid`

Reordered the header action icons to: **Search → Cart → Account**

Previously the order was account then cart. The cart block was moved before the account block in the template.

### 4. CSS Customizations

**File modified:** `assets/custom-styles.scss`

All custom styles are consolidated in `custom-styles.scss` under the `//** Header Search Inline **//` and `//** Header **//` sections.

| Customization | Value | Selector |
|---|---|---|
| Search bar max width | `577px` | `.header-search-inline` |
| Search bar min width | `180px` | `.header-search-inline` |
| Gap between search and header-actions | `40px` | `.header__column--right` |
| Gap between header action icons | `0` with `7px` padding per side (14px visual) | `header-actions`, `.header-actions__action` |
| Search input border | `none` | `.search-action .predictive-search-form__header` |
| Search input background | `#F0F0F0` | `.search-action .predictive-search-form__header-inner` |
| Search input border radius | `62px` | `.search-action .predictive-search-form__header-inner` |

Note on icon gap: The header action buttons use a 44x44px touch target for accessibility, while the icons themselves are 22x22px. Setting `gap: 0` with `padding-inline: 7px` on each action results in a clean 14px visual gap between icons while preserving touch target sizes.

---

## Files Modified

| File | Changes |
|---|---|
| `snippets/search.liquid` | Full replacement of modal button with inline predictive search component |
| `snippets/header-actions.liquid` | Custom icon references, reordered cart and account |
| `assets/custom-styles.scss` | All header and search custom styles |
