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

The marketplace is protected by an access token passed via query string. The token will be manually created and manually updated regularlry. So there is no individual token by shop user.

Example:

https://marketplace.blogtec.io/?access=591251512

The access expires after 7 days.

This provides controlled access without authentication complexity.

---

# Routing Strategy

Vite SPA using React Router with nested layout.

---

# Layout Structure

## Sidebar

Menu:
- Back to Blogtec App
- Database
- Link Assistant (Phase 2)

Active sidebar fill color:
`#FFE2E9`

---

## Topbar
Left:
- Page title

Right:
- Notification icon
- Cart icon (with counter badge)

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

Font: Lato

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
- Referring Domains

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
- Search by domain name
- 3 views, General, Deals, and Saved
- Filter bar (DR, Traffic, Niche, Type)
- Sortable columns (DR, Traffic, Price)
- Add to cart button
- Save domain button
- Internal scroll container

Filters initially handled client-side.

---

## Cart (Topbar Dropdown)

Cart contains:
- Domain
- Price

Features:
- Remove item
- Display total
- Checkout button

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

Woo remains the single source of truth for:

- Payments
- VAT
- Refunds
- Accounting

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
- Saved domains and backlink deals

## Phase 2 – Checkout and Login Functionality
- Signed access token security
- Woo checkout redirect

## Phase 3 – LinkFinder
- Suggest best link opportunities based on a questionnaire


---

# Architectural Principles

1. Marketplace is product discovery only.
2. WooCommerce is the commerce engine.
3. Keep complexity low.
4. Build for scalability (15,000+ domains).
5. Separate UI, business logic, and Woo integration clearly.
