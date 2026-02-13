# Blogtec Link Center

## Project Goal

The Blogtec Link Center is a lightweight B2B backlink database and ordering tool.

It allows agencies to:

- Browse a curated backlink database
- Filter by SEO metrics (DR, Traffic, Niche, Type)
- Add backlinks to a cart
- Checkout via the main Blogtec WooCommerce shop

The marketplace acts as:

> A product discovery and filtering tool  
> that hands over checkout and order handling to WooCommerce.

The system is intentionally simple and optimized for scalability.

---

# Architecture Overview

**Marketplace:**  
`linkcenter.blogtec.io` (Vite + React SPA)

**Main Shop (WooCommerce):**  
`app.blogtec.io`

Checkout and order management are handled exclusively in WooCommerce.

Marketplace handles discovery and cart building only.

---

# Tech Stack

- Vite + React (SPA)
- React Router (nested layouts)
- TypeScript
- Tailwind CSS
- Radix UI
- Supabase (database)
- Lucide Icons

---

# Security Layer

The marketplace is protected by a signed access token passed via query string.

Example:

https://marketplace.blogtec.io/?access=SIGNED_TOKEN

yaml
Copy code

## Token Rules

- Token must be HMAC signed.
- Token must include expiration timestamp.
- Token validation must happen server-side (NOT client-only).
- If valid → set secure cookie (e.g. 7 days).
- If invalid → show “Access Required” screen.

Do NOT use plain-text passwords in query strings.

This provides controlled access without full authentication complexity.

---

# Routing Strategy

Vite SPA using React Router with nested layout.

## Root Behavior

`/` → redirects to `/database`

---

# Layout Structure

## Sidebar

Menu:

- Database
- Orders (links to Woo My Account)
- LinkFinder (Phase 2)

Active sidebar fill color:
`#FFE2E9`

---

## Topbar

Left:
- Page title

Right:
- Notification icon
- Cart icon (with counter badge)
- User dropdown (future use)

Cart opens as a right-side dropdown.

---

# UI System

## Design Philosophy

- Clean SaaS interface
- Data-dense
- Table-first layout
- No flashy marketplace visuals
- Built for agencies

---

## Typography

Font: **Lato**

Weights:

- 500 – Table rows, metadata
- 600 – Table headers, sidebar items, buttons
- 700 – Page titles, prices, DR badges

Sizes:

- Table text: 14px
- Table header: 14px
- Page title: 18px

Use tabular numbers for:

- DR
- Traffic
- Price

---

## Colors

Primary button:  
`#E9204F`

Active sidebar fill:  
`#FFE2E9`

Table header background:  
`#F9FAFB`

Borders:  
`#F3F4F6`

Background:  
White

---

## DR Badge Logic

- DR 60+ → Green
- DR 30–59 → Yellow
- DR 0–29 → Grey

Badges should be subtle and professional.

---

# Core Pages

## Database Page

Main screen of the application.

Features:

- Search input
- Filter bar (DR, Traffic, Niche, Type)
- Sortable columns (DR, Traffic, Price)
- Add to cart button
- Sticky table header
- Internal scroll container

Filters initially handled client-side.

---

## Cart (Topbar Dropdown)

Cart contains:

- Domain
- DR
- Traffic
- Price

Features:

- Remove item
- Display total
- Checkout button

No dedicated cart page for MVP.

---

# Checkout Flow

Checkout is handled entirely in WooCommerce.

## Flow

1. User selects backlinks in marketplace.
2. Clicks Checkout.
3. Redirect to custom Woo endpoint:
   - Endpoint builds Woo cart items.
   - Each backlink added as cart item.
   - Individual prices applied.
   - Backlink metadata stored in cart item.
4. Redirect to Woo `/checkout`.
5. Woo handles:
   - VAT
   - Stripe
   - Invoices
   - Emails.
6. After payment → Woo Thank You page.

---

# Orders

Orders live in WooCommerce.

Marketplace “Orders” menu item links to:

https://blogtec.io/my-account/orders

markdown
Copy code

Woo remains the single source of truth for:

- Payments
- VAT
- Refunds
- Accounting

Future phase may sync orders into Supabase for internal display.

---

# Traffic Strategy (Future Phase)

Traffic will support historical tracking.

Plan:

- Keep current traffic snapshot in main domain record.
- Create separate traffic history table.
- Insert one row per month per domain.
- Never overwrite historical data.

This enables:

- Growth trend charts
- LinkFinder scoring logic
- Traffic stability evaluation

Not implemented in MVP.

---

# Development Phases

## Phase 1 – Core Marketplace

- UI shell
- Database table
- Filters
- Cart dropdown
- Signed access token security
- Woo checkout redirect

## Phase 2 – LinkFinder

- Advanced filtering
- Value scoring
- Best-match suggestions
- Saved filters

## Phase 3 – Intelligence Layer

- Traffic history tracking
- Order syncing
- Agency-specific pricing
- Credit system

---

# Architectural Principles

1. Marketplace is product discovery only.
2. WooCommerce is the commerce engine.
3. No VAT logic inside React app.
4. Keep complexity low.
5. Build for scalability (15,000+ domains).
6. Avoid overengineering authentication in MVP.
7. Separate UI, business logic, and Woo integration clearly.
