# INVENTO UX/UI DESIGN SPECIFICATION

# TABLE OF CONTENTS

- [1. PRODUCT OVERVIEW](#1-product-overview)
  - [What Invento Does](#what-invento-does)
  - [Key User Goals](#key-user-goals)
  - [Core Features (User Perspective)](#core-features-user-perspective)
- [2. USER ROLES & PERMISSIONS](#2-user-roles-permissions)
  - [Merchant Owner](#merchant-owner)
  - [Cashier (Staff)](#cashier-staff)
  - [Consumer (Buyer)](#consumer-buyer)
  - [Platform Admin](#platform-admin)
- [3. GLOBAL DESIGN SYSTEM](#3-global-design-system)
  - [3.1 Design Principles](#31-design-principles)
  - [3.2 Color Palette](#32-color-palette)
  - [3.3 Typography](#33-typography)
  - [3.4 Spacing & Layout](#34-spacing-layout)
  - [3.5 Navigation Structure](#35-navigation-structure)
    - [Merchant Mobile App  Bottom Tab Bar](#merchant-mobile-app-bottom-tab-bar)
    - [Consumer Mobile App  Bottom Tab Bar](#consumer-mobile-app-bottom-tab-bar)
    - [Admin Web Dashboard  Left Sidebar](#admin-web-dashboard-left-sidebar)
  - [3.6 Component Library](#36-component-library)
    - [Buttons](#buttons)
    - [Form Inputs](#form-inputs)
    - [Cards](#cards)
    - [Tables](#tables)
    - [Badges & Status Pills](#badges-status-pills)
    - [Modals & Dialogs](#modals-dialogs)
    - [Toasts & Alerts](#toasts-alerts)
  - [3.7 Universal Component States](#37-universal-component-states)
- [4. MERCHANT MOBILE APP](#4-merchant-mobile-app)
  - [4.1 Authentication](#41-authentication)
    - [Screen: Phone Login](#screen-phone-login)
    - [Screen: OTP Verification](#screen-otp-verification)
    - [Screen: Onboarding Step 1 - Personal Info](#screen-onboarding-step-1-personal-info)
    - [Screen: Onboarding Step 2 - Shop Info](#screen-onboarding-step-2-shop-info)
    - [Screen: Onboarding Step 3 - Shop Photo](#screen-onboarding-step-3-shop-photo)
  - [4.2 Dashboard](#42-dashboard)
  - [4.3 Inventory](#43-inventory)
    - [Screen: Product List](#screen-product-list)
    - [Screen: Add Product](#screen-add-product)
    - [Screen: Edit Product](#screen-edit-product)
    - [Screen: Import Job Status](#screen-import-job-status)
    - [Screen: Bulk Import](#screen-bulk-import)
    - [Screen: Barcode Scanner](#screen-barcode-scanner)
  - [4.4 POS (Point of Sale)](#44-pos-point-of-sale)
    - [Screen: POS Main](#screen-pos-main)
    - [Screen: Cart Sheet](#screen-cart-sheet)
    - [Screen: Receipt](#screen-receipt)
    - [Screen: Sync Issues](#screen-sync-issues)
  - [4.5 Orders](#45-orders)
    - [Screen: Orders List](#screen-orders-list)
    - [Screen: Order Detail](#screen-order-detail)
    - [Screen: Sale Detail (Read-Only)](#screen-sale-detail-read-only)
  - [4.6 Analytics](#46-analytics)
  - [4.7 Customer Management](#47-customer-management)
    - [Screen: Customer List](#screen-customer-list)
    - [Screen: Customer Profile](#screen-customer-profile)
  - [4.8 Settings & More](#48-settings-more)
    - [Screen: More Menu](#screen-more-menu)
    - [Screen: Shop Profile](#screen-shop-profile)
    - [Screen: Staff Management](#screen-staff-management)
    - [Screen: Subscription Plan](#screen-subscription-plan)
    - [Screen: Notifications](#screen-notifications)
- [5. CONSUMER MOBILE APP](#5-consumer-mobile-app)
  - [5.1 Authentication](#51-authentication)
    - [Screen: Phone Login (Consumer)](#screen-phone-login-consumer)
    - [Screen: OTP Verification (Consumer)](#screen-otp-verification-consumer)
    - [Screen: Name Setup](#screen-name-setup)
  - [5.2 Home](#52-home)
  - [5.3 Search & Discovery](#53-search-discovery)
  - [5.4 Market & Shop Browsing](#54-market-shop-browsing)
    - [Screen: Market Detail](#screen-market-detail)
    - [Screen: Shop Profile (Consumer)](#screen-shop-profile-consumer)
    - [Screen: Product Detail](#screen-product-detail)
  - [5.5 Cart](#55-cart)
  - [5.6 Checkout](#56-checkout)
    - [Screen: Delivery Details](#screen-delivery-details)
    - [Screen: Payment (Paystack WebView)](#screen-payment-paystack-webview)
    - [Screen: Order Confirmation](#screen-order-confirmation)
  - [5.7 Orders (Consumer)](#57-orders-consumer)
    - [Screen: Order History](#screen-order-history)
    - [Screen: Order Detail (Consumer)](#screen-order-detail-consumer)
  - [5.8 Profile](#58-profile)
- [6. ADMIN WEB DASHBOARD](#6-admin-web-dashboard)
  - [6.1 Layout Shell](#61-layout-shell)
  - [6.2 Authentication](#62-authentication)
    - [Page: Admin Login](#page-admin-login)
  - [6.3 Overview](#63-overview)
    - [Page: Platform Overview](#page-platform-overview)
  - [6.4 Merchants](#64-merchants)
    - [Page: Merchant List](#page-merchant-list)
    - [Page: Merchant Detail](#page-merchant-detail)
  - [6.5 Markets](#65-markets)
    - [Page: Market List](#page-market-list)
    - [Page: Register / Edit Market](#page-register-edit-market)
  - [6.6 Orders (Admin)](#66-orders-admin)
    - [Page: All Orders](#page-all-orders)
    - [Page: Order Detail (Admin)](#page-order-detail-admin)
  - [6.7 Payments](#67-payments)
    - [Page: Transactions](#page-transactions)
  - [6.8 Outbox Monitor](#68-outbox-monitor)
    - [Page: Dead-Letter Queue](#page-dead-letter-queue)
  - [6.9 Settings](#69-settings)
    - [Page: Platform Settings](#page-platform-settings)
- [7. USER FLOWS](#7-user-flows)
  - [7.1 New Merchant Onboarding](#71-new-merchant-onboarding)
  - [7.2 POS Sale (Online)](#72-pos-sale-online)
  - [7.3 POS Sale (Offline) + Sync](#73-pos-sale-offline-sync)
  - [7.4 Adding a Product](#74-adding-a-product)
  - [7.5 Consumer Buying Online](#75-consumer-buying-online)
  - [7.6 Merchant Fulfilling an Order](#76-merchant-fulfilling-an-order)
  - [7.7 Viewing Analytics](#77-viewing-analytics)
  - [7.8 Admin Managing a Merchant](#78-admin-managing-a-merchant)
- [8. EDGE CASES & STATES](#8-edge-cases-states)
  - [8.1 Universal States](#81-universal-states)
  - [8.2 POS Edge Cases](#82-pos-edge-cases)
  - [8.3 Order Edge Cases](#83-order-edge-cases)
  - [8.4 Auth Edge Cases](#84-auth-edge-cases)
  - [8.5 Offline Edge Cases](#85-offline-edge-cases)
- [9. MOBILE UX PRINCIPLES](#9-mobile-ux-principles)
  - [9.1 Touch Standards](#91-touch-standards)
  - [9.2 POS Speed Rules](#92-pos-speed-rules)
  - [9.3 Offline Usability](#93-offline-usability)
  - [9.4 Navigation Patterns](#94-navigation-patterns)
  - [9.5 Form UX](#95-form-ux)
  - [9.6 Language Support](#96-language-support)
- [10. INTERACTION DESIGN & ANIMATION](#10-interaction-design-animation)
  - [10.1 Micro-Interactions](#101-micro-interactions)
  - [10.2 Optimistic Updates](#102-optimistic-updates)
  - [10.3 Search & Input Performance](#103-search-input-performance)
  - [10.4 Permission Request Flows](#104-permission-request-flows)
- [11. ACCESSIBILITY GUIDELINES](#11-accessibility-guidelines)
  - [11.1 Color & Contrast](#111-color-contrast)
  - [11.2 Touch & Motor Accessibility](#112-touch-motor-accessibility)
  - [11.3 Screen Reader Support](#113-screen-reader-support)
  - [11.4 Dynamic Font Size](#114-dynamic-font-size)
  - [11.5 One-Handed Use (Mobile)](#115-one-handed-use-mobile)
- [12. DESIGN QUALITY STANDARDS](#12-design-quality-standards)
  - [12.1 Spacing Consistency Rules](#121-spacing-consistency-rules)
  - [12.2 Elevation & Shadow System](#122-elevation-shadow-system)
  - [12.3 Border Radius System](#123-border-radius-system)
  - [12.4 Icon System](#124-icon-system)
  - [12.5 Image Guidelines](#125-image-guidelines)
- [13. NOTIFICATION & FEEDBACK CATALOG](#13-notification-feedback-catalog)
  - [13.1 Push Notifications](#131-push-notifications)
  - [13.2 In-App Toasts](#132-in-app-toasts)
  - [13.3 Confirmation Dialogs](#133-confirmation-dialogs)
  - [13.4 Progress Indicators](#134-progress-indicators)
- [APPENDIX A: COMPLETE SCREEN INVENTORY](#appendix-a-complete-screen-inventory)
  - [Merchant Mobile App (26 screens)](#merchant-mobile-app-26-screens)
  - [Consumer Mobile App (16 screens)](#consumer-mobile-app-16-screens)
  - [Admin Web Dashboard (12 pages)](#admin-web-dashboard-12-pages)
- [APPENDIX B: COMPONENT CHECKLIST](#appendix-b-component-checklist)
  - [Atoms](#atoms)
  - [Molecules](#molecules)
  - [Organisms](#organisms)

# 1. PRODUCT OVERVIEW

## What Invento Does

Invento is a three-layer SaaS platform built for Nigerian market traders and shop owners.

| Layer | Primary Users | Core Job |
|---|---|---|
| Merchant Suite (Mobile App) | Shop owners, cashiers | Run a shop: manage stock, sell at POS, track orders, view analytics |
| Consumer Marketplace (Mobile App) | Buyers | Browse markets, buy from shops, track deliveries |
| Admin Dashboard (Web) | Platform operators | Manage merchants, markets, plans, and platform health |

## Key User Goals

- Merchant Owner: "I want to know what I have in stock, sell quickly, and see how my business is doing  even when the internet is down."
- Cashier: "I want to ring up a sale as fast as possible with minimal steps."
- Consumer: "I want to find a product in my local market, buy it, and know when it's coming."
- Admin: "I want to see all merchants, manage their plans, and keep the platform healthy."

## Core Features (User Perspective)

- Phone number login with OTP  no passwords to remember
- POS that works offline  sell even without internet
- Inventory with barcode scanning  add and find products instantly
- Auto-generated online shop  every merchant gets a storefront
- Consumer marketplace  browse all shops in a market
- Sales analytics  daily, weekly, monthly charts
- Order management  from placement to delivery
- Push notifications  low stock, new orders, payment confirmations
- English + Hausa language support

# 2. USER ROLES & PERMISSIONS

## Merchant Owner

Full access to their own shop. Cannot access other merchants' data or platform settings.

| Can Do | Cannot Do |
|---|---|
| Manage all inventory (add, edit, delete) | Access another merchant's data |
| Run POS sales | Manage platform-level settings |
| View all analytics and sales history | Change their own subscription plan (admin does this) |
| Manage customers and CRM | |
| View and fulfill online orders | |
| Manage staff accounts | |
| Configure shop profile and storefront | |
| Import products via CSV | |

## Cashier (Staff)

POS operator hired by the merchant owner. Restricted to selling only.

| Can Do | Cannot Do |
|---|---|
| Run POS sales | View analytics |
| Look up products by barcode | Add, edit, or delete products |
| View their own sales session | Export customer data |
| View incoming orders (read-only) | Manage staff or settings |

## Consumer (Buyer)

End customer buying from the marketplace.

| Can Do | Cannot Do |
|---|---|
| Browse all markets and shops | Access merchant tools |
| Search products across all shops | View other consumers' orders |
| Add to cart and checkout | Manage inventory |
| Track their own orders | |
| View their order history | |

## Platform Admin

Internal Invento team. Three sub-roles with escalating access.

| Role | Access Level |
|---|---|
| Admin Viewer | Read-only: see all merchants, orders, analytics |
| Admin Operator | Read + write: manage merchants, register markets, update order status |
| Admin Super | Full access: change plans, replay dead-letter events, all settings |

# 3. GLOBAL DESIGN SYSTEM

## 3.1 Design Principles

1. Speed first  Every POS interaction must complete in under 3 taps. Cashiers cannot wait.
2. Offline-aware  The UI always communicates connectivity status. Nothing silently fails.
3. Clarity over cleverness  Labels are plain language. Icons always have text labels on mobile.
4. Density where needed  POS and inventory lists are dense. Analytics and settings are spacious.
5. Nigerian context  Currency is always ₦ with comma formatting (₦1,500). Phone numbers use +234 format.
6. Enterprise quality  Every screen must feel as polished as Stripe, Shopify, or Square.

## 3.2 Color Palette

| Token | Hex | Usage |
|---|---|---|
| `brand-primary` | `#1A56DB` | Primary actions, active nav, links |
| `brand-secondary` | `#0E9F6E` | Success states, confirmed badges |
| `brand-accent` | `#FF5A1F` | Alerts, low stock badges, destructive actions |
| `neutral-900` | `#111928` | Primary text |
| `neutral-700` | `#374151` | Secondary headings |
| `neutral-600` | `#6B7280` | Secondary text, placeholders |
| `neutral-300` | `#D1D5DB` | Disabled borders |
| `neutral-200` | `#E5E7EB` | Borders, dividers |
| `neutral-100` | `#F3F4F6` | Hover backgrounds |
| `neutral-50` | `#F9FAFB` | Page backgrounds |
| `white` | `#FFFFFF` | Card backgrounds, modals |
| `offline-amber` | `#F59E0B` | Offline mode indicator |
| `status-pending` | `#F59E0B` | Order pending badge |
| `status-confirmed` | `#1A56DB` | Order confirmed badge |
| `status-processing` | `#7E3AF2` | Order processing badge |
| `status-shipped` | `#0E9F6E` | Order shipped badge |
| `status-delivered` | `#057A55` | Order delivered badge |
| `status-cancelled` | `#E02424` | Order cancelled badge |
| `low-stock-amber` | `#D97706` | Low stock warning |
| `out-of-stock-red` | `#DC2626` | Out of stock |

## 3.3 Typography

| Style | Font | Size | Weight | Line Height | Usage |
|---|---|---|---|---|---|
| `display` | Inter | 28px | 700 | 1.2 | Page titles (web) |
| `heading-1` | Inter | 22px | 700 | 1.3 | Section headers |
| `heading-2` | Inter | 18px | 600 | 1.4 | Card titles, modal headers |
| `heading-3` | Inter | 16px | 600 | 1.4 | List item titles |
| `body` | Inter | 14px | 400 | 1.5 | Body text, descriptions |
| `body-sm` | Inter | 13px | 400 | 1.5 | Secondary text, captions |
| `label` | Inter | 12px | 500 | 1.4 | Form labels, table headers |
| `mono` | JetBrains Mono | 14px | 400 | 1.4 | Prices, quantities, IDs, codes |
| `price-lg` | Inter | 28px | 700 | 1.2 | POS totals, receipt amounts |
| `price-md` | Inter | 20px | 700 | 1.3 | Dashboard metric values |

Mobile base body size: 15px (slightly larger for readability on small screens).

## 3.4 Spacing & Layout

- Base unit: 4px
- Common spacings: 4, 8, 12, 16, 20, 24, 32, 48, 64px
- Mobile screen padding: 16px horizontal
- Card padding: 16px
- Section gap: 24px
- Bottom tab bar height: 64px (mobile)
- Top navigation bar height: 56px (mobile), 64px (web)
- Sidebar width (web): 240px, collapses to 64px icon-only mode
- Max content width (web): 1280px, centered
- Web content padding: 32px

## 3.5 Navigation Structure

### Merchant Mobile App  Bottom Tab Bar
```
[ Dashboard ] [ Inventory ] [ POS ] [ Orders ] [ More ]
```
- Dashboard: home icon
- Inventory: box icon
- POS: shopping cart icon  center position, slightly larger (56px), brand-primary background pill
- Orders: receipt icon
- More: grid icon  leads to Settings, Customers, Analytics, Profile, Logout

### Consumer Mobile App  Bottom Tab Bar
```
[ Home ] [ Search ] [ Cart (badge) ] [ Orders ] [ Profile ]
```
- Cart tab shows item count badge when cart is not empty

### Admin Web Dashboard  Left Sidebar
```
[Invento Logo + "Admin" label]
Overview
Merchants
Markets
Orders
Payments
Outbox
Settings
```
- Active item: brand-primary background, white text, left accent bar
- Collapsed mode (64px): icons only, tooltip on hover

## 3.6 Component Library

### Buttons

**Primary Button**
- Background: `brand-primary` | Text: white, 14px, 600 weight
- Height: 48px (mobile), 40px (web) | Border radius: 8px
- States: default / hover (darken 8%) / pressed (darken 15%, scale 0.97) / disabled (opacity 40%) / loading (spinner replaces label, width locked)
- Full-width on mobile by default

**Secondary Button**
- Background: white | Border: 1.5px `brand-primary` | Text: `brand-primary`
- Same sizing as primary

**Destructive Button**
- Background: `#FEF2F2` | Border: 1.5px `#E02424` | Text: `#E02424`
- Used for: delete product, cancel order, remove staff

**Ghost Button**
- No background, no border | Text: `brand-primary`
- Used for: "View all" links, "Skip for now", inline secondary actions

**Icon Button**
- 44x44px touch target (mobile), 36x36px (web) | Icon: 20px centered
- Used for: edit, delete, scan, close, share

### Form Inputs

**Text Input**
- Height: 48px (mobile), 40px (web) | Border: 1.5px `neutral-200`, radius 8px
- Focus: border `brand-primary`, box-shadow `0 0 0 3px rgba(26,86,219,0.15)`
- Error: border `#E02424`, error message below in `#E02424` 12px
- Placeholder: `neutral-600` | Label: above input, 12px, 500 weight, `neutral-900`
- Disabled: background `neutral-50`, text `neutral-400`

**Phone Input**
- Country code prefix chip (+234)  non-editable, `neutral-100` background
- Numeric keyboard auto-opens on mobile
- Validates 10-digit format on blur

**OTP Input**
- 6 individual boxes, each 48x56px (mobile), 44x52px (web)
- Border: 1.5px `neutral-200`, radius 8px | Active box: border `brand-primary`
- Auto-advance on digit entry | Paste support (fills all 6 at once)
- Error state: all boxes border `#E02424`

**Search Input**
- Leading search icon (16px, `neutral-400`) | Clear (x) button when text present
- Height: 44px | Full-width | Border radius: 24px (pill shape)
- Background: `neutral-50` on mobile, white on web

**Select / Dropdown**
- Native picker on mobile | Custom dropdown on web with search for lists > 10 items
- Same height and border as text input

**Textarea**
- Min height: 80px, auto-grows to max 160px | Resize: vertical only (web)
- Used for: product description, delivery notes, cancel reason

**File Upload**
- Dashed border box (`neutral-300`), 120px height, radius 8px
- "Tap to upload" (mobile) / "Click or drag to upload" (web)
- Shows file name + size after selection | Progress bar during upload
- Error state: red dashed border + error message

**Numeric Input**
- Right-aligned text | Numeric keyboard on mobile
- Optional ₦ prefix for price fields | Optional unit suffix (e.g., "units")

### Cards

**Standard Card**
- Background: white | Border: 1px `neutral-200` | Border radius: 12px
- Padding: 16px | Shadow: `0 1px 3px rgba(0,0,0,0.08)`

**Metric Card**
- Same as standard card
- Label: 12px, `neutral-600` | Value: 24px, 700, `neutral-900`
- Trend indicator: up arrow (green) or down arrow (red) + percentage

**Product Card (list row)**
- Horizontal: image (48x48px, radius 8px) | name + SKU | price (mono) | stock badge
- Swipe left (mobile): reveals Edit and Delete actions

**Product Tile (POS grid)**
- Vertical: image (full width, 120px height) | name (2 lines max) | price | stock count
- Out-of-stock: greyed out, "Out of Stock" overlay, not tappable

**Order Card**
- Order ID (truncated) | customer name | item count + total | status badge | time ago
- Swipe right (merchant, pending only): quick Confirm action

**Shop Card (consumer)**
- Logo (48x48px) | shop name | market name | product count | category tags

### Tables

Web-only component used in admin dashboard.

- Header: `neutral-50` background, 12px uppercase, `neutral-600`, 500 weight
- Row height: 52px | Alternating: white / `neutral-50`
- Hover: `#EBF5FF` row highlight
- Sortable columns: sort icon on hover, active sort shows direction arrow
- Pagination: "Showing 1–20 of 150" + Previous / Next + page size selector (20/50/100)
- Sticky header on scroll

### Badges & Status Pills

- Height: 22px | Padding: 0 8px | Border radius: 11px (pill)
- Font: 12px, 500 weight
- Colors: match status color tokens from palette

| Badge | Background | Text |
|---|---|---|
| FREE plan | `#F3F4F6` | `#374151` |
| BASIC plan | `#EFF6FF` | `#1D4ED8` |
| PRO plan | `#F5F3FF` | `#6D28D9` |
| ENTERPRISE | `#FFF7ED` | `#C2410C` |
| Active | `#ECFDF5` | `#065F46` |
| Inactive | `#FEF2F2` | `#991B1B` |
| In Stock | `#ECFDF5` | `#065F46` |
| Low Stock | `#FFFBEB` | `#92400E` |
| Out of Stock | `#FEF2F2` | `#991B1B` |

### Modals & Dialogs

**Standard Modal**
- Max width: 480px (web) | Full-width bottom sheet (mobile)
- Header: title (18px, 600) + close (x) button top-right
- Body: scrollable if content overflows | Max height: 80vh
- Footer: action buttons right-aligned (web), stacked full-width (mobile)
- Backdrop: `rgba(0,0,0,0.5)`, tap to close (unless destructive confirmation)

**Confirmation Dialog**
- Max width: 360px | Centered
- Icon (warning/danger, 40px) | Title (18px, 600) | Description (14px, `neutral-600`)
- Two buttons: Cancel (secondary, left) | Destructive action (red, right)
- Cannot be dismissed by tapping backdrop

**Bottom Sheet (Mobile)**
- Slides up from bottom with spring animation
- Handle bar: 32x4px, `neutral-300`, radius 2px, centered at top
- Max height: 80% of screen | Swipe down to dismiss
- Backdrop tap also dismisses (unless form has unsaved data)

### Toasts & Alerts

**Toast**
- Position: top-center (mobile), bottom-right (web)
- Width: 320px (web), screen width minus 32px (mobile)
- Min height: 52px | Border radius: 8px | Shadow: `0 4px 12px rgba(0,0,0,0.15)`
- Duration: 3s (success/info), 5s (error), manual dismiss button
- Types: success (green left border + check icon), error (red), warning (amber), info (blue)
- Content: icon + message text + optional action link

**Inline Alert Banner**
- Full-width within a section | Border radius: 8px | Padding: 12px 16px
- Types match toast types
- Used for: plan limit warnings, offline sync results, form-level errors, important notices

## 3.7 Universal Component States

Every interactive component must implement all five states:

**Loading State**
- Lists: skeleton loaders  grey animated rectangles matching real content shape
- Buttons: spinner replaces label, width locked, pointer-events none
- Cards: shimmer animation over entire card
- Full-screen: centered spinner + "Loading..." only for initial app load

**Empty State**
- Illustration (simple, on-brand SVG) + heading + description + optional CTA button
- Never show a blank screen

**Error State**
- Network error: toast + retry button
- Form validation: red border + inline message below field (never above)
- Full-screen error: illustration + "Something went wrong" + "Try Again" button

**Success State**
- Form submit: success toast + navigate or reset
- Destructive action: success toast + item removed from list
- Sale complete: animated checkmark + receipt screen

**Offline State**
- Persistent amber banner at top: "You're offline  [feature] still works" or "Internet required"
- Never silently fail  always communicate what works and what doesn't
- Cached data shown with "Last updated X min ago" label

# 4. MERCHANT MOBILE APP

## 4.1 Authentication

### Screen: Phone Login

**Purpose:** Entry point for all merchant and cashier users.
**Route:** `/(auth)/login`

**Layout:** Single-column, vertically centered. Safe area padding top and bottom.

**Sections:**
1. Invento logo (80px, centered) + tagline: "Manage your shop, anywhere."
2. Phone number input with +234 prefix chip
3. "Send Code" primary button (full-width)
4. "New to Invento? Register your shop" ghost link at bottom

**Actions:**
- Tap "Send Code" → validate phone format → loading spinner on button → navigate to OTP screen
- Invalid phone → inline error: "Enter a valid Nigerian phone number"
- Tap register link → navigate to Onboarding Step 1

**States:**
- Default: empty input, button disabled
- Typing: button enables when 10+ digits entered
- Loading: button shows spinner, input disabled
- Error: red border on input, error message below

### Screen: OTP Verification

**Purpose:** Verify phone ownership via 6-digit SMS code.
**Route:** `/(auth)/verify-otp`

**Layout:** Single-column. Back arrow top-left. Subtitle shows masked phone number.

**Sections:**
1. "Enter your code" heading (heading-1)
2. Subtitle: "Code sent to +234 801 *** 5678" + "Change" ghost link
3. 6-box OTP input (auto-focused on first box)
4. "Verify" primary button (full-width)
5. "Resend code" ghost link  disabled for 60s, shows countdown "Resend in 0:45"

**Actions:**
- Auto-submit when all 6 digits entered
- "Verify" tap → loading → success → navigate to Dashboard (existing user) or Onboarding Step 1 (new user)
- "Change" link → back to phone screen
- "Resend" → re-sends OTP, resets 60s countdown

**States:**
- Incomplete: Verify button disabled
- Complete: Verify button enabled
- Loading: spinner, all inputs disabled
- Error: all boxes turn red border, error message below

### Screen: Onboarding Step 1 - Personal Info

**Purpose:** Collect merchant's full name. Shown only on first login.
**Route:** `/(auth)/onboarding/step-1`

**Layout:** Single-column. Progress indicator at top: "Step 1 of 3" with dot indicators.

**Sections:**
1. Progress dots (3 dots, first filled)
2. "Tell us about yourself" heading
3. Full name text input (required)
4. "Continue" primary button (full-width)

**Actions:**
- "Continue" → validates name not empty → navigate to Step 2

### Screen: Onboarding Step 2 - Shop Info

**Purpose:** Collect shop details and market assignment.
**Route:** `/(auth)/onboarding/step-2`

**Layout:** Single-column. Progress indicator: Step 2 of 3.

**Sections:**
1. Progress dots (second filled)
2. "Set up your shop" heading
3. Shop name text input (required)
4. Shop description textarea (optional, max 200 chars, char counter shown)
5. Market selector  searchable dropdown showing registered markets by city
6. "Continue" primary button

**Actions:**
- "Continue" → validates shop name → navigate to Step 3
- Back arrow → Step 1 (does not go back to login)

### Screen: Onboarding Step 3 - Shop Photo

**Purpose:** Optional shop logo upload to complete setup.
**Route:** `/(auth)/onboarding/step-3`

**Layout:** Single-column. Progress indicator: Step 3 of 3.

**Sections:**
1. Progress dots (all three filled)
2. "Add your shop photo" heading
3. Circular image upload area (120px diameter)  dashed border, camera icon + "Add Photo"
4. After selection: circular crop preview with "Change" overlay
5. "Skip for now" ghost link
6. "Finish Setup" primary button

**Actions:**
- Tap image area → bottom sheet: "Take Photo" / "Choose from Gallery"
- "Finish Setup" → save profile → navigate to Dashboard with welcome toast: "Welcome to Invento, [Shop Name]!"
- "Skip for now" → navigate to Dashboard without photo

## 4.2 Dashboard

**Purpose:** At-a-glance view of today's business performance and key alerts.
**Route:** `/(tabs)/dashboard`

**Layout:** Scrollable single column. Top bar: shop name (left), notification bell with badge (right).

**Sections:**

**Offline Banner** (conditional  shown only when offline)
- Amber background, full-width: "You're offline. Sales will sync when connected."

**Sync Status Bar** (conditional  shown when sync is in progress or failed)
- Cloud icon + status text: "Syncing 3 sales..." / "Sync failed  tap to retry"

**Today's Summary** (horizontal scroll row of 4 metric cards)
- Today's Revenue: ₦ amount, `price-md` font
- Sales Count: number of transactions
- Avg. Sale Value: ₦ amount
- Pending Orders: count, orange badge if > 0

**Quick Actions** (2x2 grid)
- "New Sale" → POS tab (brand-primary background, white icon + label)
- "Add Product" → Add Product screen
- "View Orders" → Orders tab
- "View Reports" → Analytics screen

**Low Stock Alerts** (conditional  shown if any products below threshold)
- Section header: "Low Stock" + count badge
- Up to 3 product rows: name | current qty | threshold
- "View All" ghost link → Inventory filtered to low stock

**Recent Sales**
- Section header: "Recent Sales" + "View All" ghost link
- Last 5 sales: time ago | items count | total (mono) | payment method badge
- Tap row → Sale Detail (read-only receipt view)

**Pending Orders** (conditional  shown if pending orders exist)
- Section header: "New Orders" + count badge
- Up to 3 order cards: order ID | customer name | total | time ago
- Tap → Order Detail screen

**Actions:**
- Pull to refresh → refreshes all sections
- Tap notification bell → Notifications screen
- Tap any metric card → relevant detail screen

**Empty State (new merchant):**
- Welcome illustration + "Welcome to Invento!" heading
- "Add your first product to get started" CTA button

## 4.3 Inventory

### Screen: Product List

**Purpose:** View and manage all products in the merchant's inventory.
**Route:** `/(tabs)/inventory`

**Layout:** Full-screen list. Top bar: "Inventory" title, search icon, filter icon, "+" add button.

**Sections:**

**Search Bar** (expands when search icon tapped)
- Full-width search input with clear button
- Searches by name, SKU, category in real-time

**Filter Bar** (shown when filter icon tapped or always visible on tablet)
- Horizontal chip row: All | Low Stock | Out of Stock | [Category chips from merchant's products]
- Active chip: filled brand-primary background, white text

**Plan Limit Banner** (FREE plan only, shown when > 80% of limit used)
- "You're using 42/50 products on the Free plan. Upgrade to add more."
- "Upgrade" link on right

**Product List**
- FlashList of ProductCard rows
- Each row: product image (48x48, radius 8px) | name (bold) + SKU (12px, neutral-600) | price (mono, right-aligned) | stock badge
- Swipe left → reveals Edit (blue) and Delete (red) action buttons
- Long press → multi-select mode: checkboxes appear, bulk delete in top bar

**Empty State:**
- Box-with-plus illustration
- "No products yet"
- "Add your first product" primary button

**Actions:**
- Tap product row → Edit Product screen
- Tap "+" → Add Product screen
- Swipe left → Edit / Delete quick actions
- Pull to refresh

### Screen: Add Product

**Purpose:** Create a new product in inventory.
**Route:** `/(tabs)/inventory/add`

**Layout:** Full-screen scrollable form. Sticky "Save Product" button at bottom. Back arrow top-left.

**Sections:**

**Photo Section**
- Large dashed rectangle (160x160px, centered, radius 12px)
- Camera icon + "Add Photo" label
- Tap → bottom sheet: "Take Photo" / "Choose from Gallery"
- After selection: image preview with "Change" overlay tap target

**Basic Info**
- Product Name (text input, required)
- Category (select: Textiles, Electronics, Food, Clothing, Beauty, Other + custom entry)
- Description (textarea, optional, max 500 chars)

**Pricing & Stock**
- Price (₦ prefix, numeric input, required, 2 decimal places)
- Quantity in Stock (numeric input, required, integer)
- Low Stock Alert At (numeric input, default 5, integer)

**Identification**
- SKU / Barcode (text input, optional)
- "Scan Barcode" icon button → opens Barcode Scanner modal → auto-fills SKU

**Sticky Footer**
- "Save Product" primary button (full-width, 56px height)

**Actions:**
- Tap "Save Product" → validate all required fields → loading → success toast "Product added" → back to Product List
- Validation errors shown inline below each field
- Plan limit reached → button disabled, plan limit banner shown

### Screen: Edit Product

**Purpose:** Modify an existing product.
**Route:** `/(tabs)/inventory/[id]`

**Layout:** Identical to Add Product, pre-filled with existing data. Title: "Edit Product".

**Additional Section (below Save button):**
- "Delete Product" destructive button (full-width, red border)

**Additional Actions:**
- "Delete Product" → confirmation dialog: "Delete [Product Name]? This cannot be undone." → Confirm (red) / Cancel
- On confirm: soft-delete (isActive = false), product removed from list, success toast

### Screen: Import Job Status

**Purpose:** Track the progress of an ongoing or completed bulk import job.
**Route:** `/(tabs)/inventory/import-job/[jobId]`

**Layout:** Full-screen. Title: "Import Status". Back arrow. Auto-refreshes every 3s while PROCESSING.

**Sections:**

**Status Card**
- Job status badge: PENDING / PROCESSING / COMPLETED / FAILED
- Progress bar (animated fill while PROCESSING)
- "Importing products..." label with animated dots while in progress

**Results Summary** (shown when COMPLETED or FAILED)
- Imported count: green checkmark + number
- Updated count: blue refresh icon + number
- Failed count: red x + number (expandable)

**Error List** (expandable, shown when failed > 0)
- Each row: Row # | Product Name | Error reason
- "Download error report" ghost link

**Actions:**
- "Done" primary button → back to Product List
- "Try Again" secondary button (shown on FAILED) → back to Bulk Import

### Screen: Bulk Import

**Purpose:** Import multiple products from a CSV file.
**Route:** `/(tabs)/inventory/bulk-import`

**Layout:** Full-screen. Title: "Bulk Import". Back arrow. Three-step flow.

**Step 1  Upload**
- File upload area (dashed box, 160px height)
- "Download CSV Template" ghost link (downloads template file)
- Limits: "Max 500 products, 5MB file size"
- "Upload File" primary button

**Step 2  Preview** (after file selected)
- Summary: "Found 47 products in file"
- Preview table: first 5 rows (Name | SKU | Price | Qty)
- "Import All" primary button
- "Cancel" secondary button

**Step 3  Results** (after import completes)
- Success count: "44 products imported"
- Updated count: "2 products updated (SKU match)"
- Failed count: "1 failed"  expandable list showing row number + error reason
- "Done" primary button → back to Product List

### Screen: Barcode Scanner

**Purpose:** Scan a product barcode to look up or auto-fill SKU.
**Route:** `/(modals)/scan`

**Layout:** Full-screen camera view. Overlay with scan frame (240x240px, rounded corners, animated corner brackets). Top bar: "Scan Barcode" title + close (x) button.

**Sections:**
- Camera viewfinder (full screen)
- Scan frame overlay with animated scanning line
- "Point camera at barcode" instruction text below frame
- "Enter manually" ghost link at bottom

**Actions:**
- Successful scan → close modal → auto-fill SKU field in calling screen (Add Product or POS)
- Barcode not found in inventory (POS context) → toast: "No product found for this barcode" + "Add as new product" option
- "Enter manually" → close modal, focus SKU input

## 4.4 POS (Point of Sale)

### Screen: POS Main

**Purpose:** Primary selling interface. Must be fast, minimal, and fully functional offline.
**Route:** `/(tabs)/pos`

**Layout:** Single-panel on phone. Top bar + scrollable product grid + sticky cart summary bar.

**Top Bar**
- "POS" title (left)
- Offline indicator: amber dot + "Offline" label (shown when no internet)
- Sync status icon: cloud-check / cloud-up (animated) / cloud-x (right side)
- Scan icon button → Barcode Scanner modal
- Search icon → expands full-width search input

**Category Filter Bar**
- Horizontal scroll chips: All | [Category names from merchant's products]
- Active chip: brand-primary filled

**Product Grid**
- 2-column FlashList grid
- Each ProductTile: product image (full width, 120px height) | name (2 lines max, 13px) | price (mono, brand-primary) | stock count (12px, bottom-right corner)
- Out-of-stock tiles: 40% opacity, "Out of Stock" overlay text, not tappable
- Tap tile → add 1 unit to cart with haptic feedback + brief scale animation (0.95 → 1.0)

**Cart Summary Bar** (sticky bottom, always visible)
- Left: cart icon + item count badge (brand-primary)
- Center: "X items" text
- Right: total amount (₦ mono font, bold)
- Tap anywhere on bar → opens Cart Sheet

**Empty State (no products):**
- "No products in inventory"
- "Go to Inventory" button

### Screen: Cart Sheet

**Purpose:** Review cart, select payment method, and complete the sale.
**Trigger:** Tap Cart Summary Bar on POS Main

**Layout:** Bottom sheet, 70% screen height. Handle bar at top.

**Sections:**

**Header**
- "Cart" title (left) | "Clear All" ghost link (red, right)

**Cart Items List**
- Each CartItem row:
  - Product name (bold, 14px) | unit price (mono, neutral-600)
  - Quantity stepper: [−] [qty] [+] | line total (mono, right) | remove [x] button
  - Stepper: − removes item when qty reaches 0

**Subtotal Row**
- "Subtotal" label | ₦ amount (mono, right)

**Payment Method**
- Label: "Payment Method"
- Horizontal chip row: Cash | Transfer | USSD | Card
- Single-tap selection, selected chip: brand-primary filled

**Customer (Optional)**
- "Add Customer" ghost link → expands inline phone input
- Phone input with +234 prefix | "Add" button
- After adding: shows customer name (if found) or "New customer"

**Complete Sale Button**
- "Complete Sale" primary button (full-width, 56px height)
- Shows total amount: "Complete Sale  ₦1,500"

**Actions:**
- Tap "Complete Sale" → validate stock → loading → success → Receipt screen
- Insufficient stock → toast: "Only 3 units of [Product] available"
- Offline → save to local DB → show receipt from local data → "Syncing..." indicator

### Screen: Receipt

**Purpose:** Show sale confirmation and digital receipt after a completed sale.
**Route:** `/(modals)/receipt/[saleId]`

**Layout:** Full-screen modal (slides up from bottom). White background.

**Sections:**

**Header**
- Animated green checkmark (64px, spring animation)
- "Sale Complete!" heading (heading-1)
- Total amount (price-lg, brand-secondary)

**Receipt Body**
- Date and time (body-sm, neutral-600)
- Receipt number: #RCP-XXXXXX (mono)
- Divider
- Item list: name | qty | unit price | line total (all mono for numbers)
- Divider
- Subtotal | Payment Method | Amount Paid

**Customer Section** (if customer was added)
- Customer name and phone

**Sync Status** (if offline sale)
- Amber inline alert: "Pending sync  will upload when online"

**Footer Actions**
- "Share Receipt" secondary button (WhatsApp share icon)
- "New Sale" primary button → clears cart, dismisses modal, back to POS Main

### Screen: Sync Issues

**Purpose:** Review and resolve offline sales that failed to sync.
**Route:** `/(tabs)/pos/sync-issues`

**Layout:** Full-screen list. Title: "Sync Issues". Back arrow.

**Sections:**

**Summary Banner**
- Amber inline alert: "X sales need your attention"

**Issue List**
- Each SyncIssueCard:
  - Sale date/time | total amount
  - Status badge: PARTIAL or REJECTED
  - Reason: "Insufficient stock for [Product Name]: had 5, needed 8"
  - Conflicted items list (expandable)
  - Two action buttons: "Force Record" (secondary) | "Void Sale" (destructive)

**Force Record Flow:**
- Confirmation dialog: "Record this sale even though stock is insufficient? Stock will go negative."
- Confirm → sale recorded, stock updated

**Void Sale Flow:**
- Confirmation dialog: "Void this sale? It will be permanently removed."
- Confirm → sale removed from list

**Empty State:**
- "All sales synced successfully" + green checkmark illustration

## 4.5 Orders

### Screen: Orders List

**Purpose:** View and manage incoming online orders from consumers.
**Route:** `/(tabs)/orders`

**Layout:** Full-screen. Top bar: "Orders" title.

**Sections:**

**Filter Tabs** (horizontal scroll)
- All | Pending (badge) | Confirmed | Processing | Shipped | Delivered | Cancelled
- Badge count on Pending tab when > 0 (brand-accent background)

**Order List**
- FlashList of OrderCard rows
- Each card: Order ID (#INV-XXXX) | time ago | customer name | item count + total (mono) | status badge
- Swipe right on Pending order → green "Confirm" quick action

**Empty State (per tab):**
- "No [status] orders" + appropriate illustration

**Actions:**
- Tap order → Order Detail screen
- Pull to refresh
- New order push notification → deep links here, refetches list

### Screen: Order Detail

**Purpose:** View full order information and update its status.
**Route:** `/(tabs)/orders/[id]`

**Layout:** Scrollable. Top bar: back arrow | "Order #INV-XXXX" | status badge.

**Sections:**

**Status Timeline**
- Horizontal stepper: Pending → Confirmed → Processing → Shipped → Delivered
- Current step: brand-primary filled circle + label
- Completed steps: brand-secondary filled
- Future steps: neutral-200 empty circle
- Cancelled: all steps neutral-300, "Cancelled" label in red below

**Customer Info Card**
- Customer name | phone number (tappable → opens dialer)
- Delivery address (full)
- Delivery note (if provided, italic)

**Order Items Card**
- List: product image (40x40) | name | qty | unit price | line total
- Subtotal row at bottom

**Payment Card**
- "Paid via Paystack" with green badge
- Transaction reference (mono, copyable)
- Amount paid (mono)

**Action Button** (context-sensitive, sticky bottom)
- PENDING → "Confirm Order" (primary, blue)
- CONFIRMED → "Mark as Processing" (primary)
- PROCESSING → "Mark as Shipped" (primary)
- SHIPPED → "Mark as Delivered" (primary)
- DELIVERED → no action button (terminal state)
- CANCELLED → no action button (terminal state)

**Cancel Order** (secondary destructive, shown for PENDING and CONFIRMED only)
- Tap → bottom sheet: optional "Reason for cancellation" textarea → "Confirm Cancellation" red button

### Screen: Sale Detail (Read-Only)

**Purpose:** View a completed sale's full receipt from sales history.
**Route:** `/(modals)/receipt/[saleId]` (same component as post-sale receipt, read-only mode)

**Layout:** Full-screen modal. Top bar: back arrow | "Sale #RCP-XXXXXX".

**Sections:**
- Date and time (body-sm, neutral-600)
- Receipt number (mono)
- Item list: name | qty | unit price | line total
- Subtotal | Payment Method | Amount Paid
- Customer section (if customer was linked): name + phone
- Sync status: "Synced" (green) or "Pending sync" (amber)

**No footer action buttons**  read-only view. No "New Sale" button.

## 4.6 Analytics

**Purpose:** View sales performance over time with charts and breakdowns.
**Route:** `/(more)/analytics`

**Layout:** Scrollable. Top bar: "Analytics" title.

**Sections:**

**Period Selector**
- Segmented control: Today | Week | Month | Custom
- Custom → date range picker modal (calendar UI, two-date selection)
- Selected period shown as subtitle: "Mar 1 – Mar 31, 2026"

**Revenue Chart**
- Bar chart (daily bars for week/month, hourly bars for today)
- Y-axis: ₦ amounts (formatted with K suffix for thousands)
- X-axis: dates or times
- Tap bar → tooltip showing exact revenue + sale count for that period
- Chart height: 200px

**Summary Metrics Row** (horizontal scroll, 4 metric cards)
- Total Revenue | Total Sales | Avg. Sale Value | Best Day/Hour

**Payment Breakdown**
- Donut chart (160px diameter)
- Segments: Cash / Transfer / USSD / Card
- Legend below: color dot | method name | ₦ amount | percentage

**Top Products Table**
- Rank | Product Name | Units Sold | Revenue
- Top 10 products for the selected period
- Tap row → product detail (read-only)

**Sales History List**
- Paginated list of all sales in period
- Each row: date/time | items count | total (mono) | payment method badge
- "Load more" button at bottom

**Empty State:**
- "No sales data for this period" + chart illustration

## 4.7 Customer Management

### Screen: Customer List

**Purpose:** View all customers who have purchased from this shop.
**Route:** `/(more)/customers`

**Layout:** Full-screen list. Top bar: "Customers" title, search icon.

**Sections:**

**Search Bar**
- Search by name or phone number (real-time filter)

**Customer List**
- FlashList of CustomerCard rows
- Each card: avatar (initials, colored circle, 40px) | name (bold) + phone | total spent (mono) | visit count | last seen date

**Empty State:**
- "No customers yet"
- "Customers are added automatically when you record a sale with a phone number."

### Screen: Customer Profile

**Purpose:** View a customer's full purchase history with this shop.
**Route:** `/(more)/customers/[id]`

**Layout:** Scrollable. Top bar: back arrow | customer name.

**Sections:**

**Profile Header**
- Large avatar (initials, 72px, colored)
- Name (heading-1) | phone number
- "Member since [date]" (body-sm, neutral-600)

**Stats Row** (3 metric cards)
- Total Spent (₦ mono) | Total Visits | Last Visit (relative date)

**Purchase History**
- Section header: "Purchase History"
- List of all sales: date | items count | total (mono) | payment method badge
- Tap row → Sale Detail (read-only receipt)

**Empty State (no purchases):**
- "No purchases recorded yet"

## 4.8 Settings & More

### Screen: More Menu

**Purpose:** Navigation hub for secondary features not in the main tab bar.
**Route:** `/(tabs)/more`

**Layout:** Full-screen list. Title: "More".

**Menu Items** (each: icon + label + chevron):
- My Shop Profile (store icon)
- Staff Management (people icon)  Owner only
- Analytics (chart-bar icon)
- Customers (person icon)
- Subscription Plan (star icon)  shows current plan badge on right
- Language (globe icon)  shows current language: "English" or "Hausa"
- Help & Support (question-circle icon)
- Logout (arrow-right-from-bracket icon, red text)

### Screen: Shop Profile

**Purpose:** Edit shop information visible on the public storefront.
**Route:** `/(more)/shop-profile`

**Layout:** Scrollable form. Top bar: "Shop Profile" title | "Save" button (top-right, disabled until changes made).

**Sections:**

**Shop Photo**
- Circular image (96px) with edit overlay (camera icon)
- Tap → image picker bottom sheet

**Basic Info**
- Shop Name (text input, required)
- Description (textarea, optional, max 300 chars)
- Market (read-only field, shows assigned market name)

**Contact**
- Phone (read-only, shown for reference)

**Storefront Link**
- "View your online shop" ghost link with external link icon → opens consumer storefront in browser

**Actions:**
- "Save" → validate → loading → success toast "Profile updated"

### Screen: Staff Management

**Purpose:** Add and manage cashier accounts. Owner only.
**Route:** `/(more)/staff`

**Layout:** Full-screen list. Title: "Staff". "+" add button top-right.

**Sections:**

**Staff List**
- Each StaffCard: avatar (initials) | name | phone | "Cashier" role badge | Active/Inactive status
- Tap → Staff Detail (view only, toggle active/inactive)

**Add Staff Flow** (bottom sheet)
- "Add Staff Member" title
- Phone number input (+234 prefix)
- Name input
- Role: "Cashier" (only option for MVP)
- "Add Staff" primary button → sends OTP to their phone for first login

**Empty State:**
- "No staff members yet"
- "Add your first cashier" CTA

### Screen: Subscription Plan

**Purpose:** View current plan, usage, and upgrade options.
**Route:** `/(more)/subscription`

**Layout:** Scrollable. Title: "Subscription".

**Sections:**

**Current Plan Card**
- Plan name badge (FREE / BASIC / PRO / ENTERPRISE)
- Expiry date (if applicable): "Valid until Apr 30, 2026"
- Usage progress bar: "42 / 50 products used" (brand-primary fill, neutral-200 track)

**Plan Comparison Table**

| Feature | FREE | BASIC | PRO |
|---|---|---|---|
| Products | 50 | Unlimited | Unlimited |
| Online Storefront | No | Yes | Yes |
| Analytics | 7 days | 30 days | Full history |
| CRM | Basic | Full | Full + Export |
| Support | Community | Email | Priority |
| Price | Free | ₦3,000/mo | ₦8,500/mo |

**Upgrade CTA**
- "Upgrade to BASIC  ₦3,000/month" primary button
- "Contact us for Enterprise" ghost link

**Note:** Plan changes are processed by the platform admin. This screen is informational + request-based for MVP.

### Screen: Notifications

**Purpose:** View all past push notifications and in-app alerts.
**Route:** `/(more)/notifications`

**Layout:** Full-screen list. Title: "Notifications". "Mark all read" ghost link (top-right).

**Sections:**

**Notification List**
- Each row: icon (colored by type) | title (bold if unread) | body text | time ago
- Unread rows: `neutral-50` background
- Read rows: white background
- Tap → deep link to relevant screen

**Empty State:**
- "No notifications yet"

# 5. CONSUMER MOBILE APP

## 5.1 Authentication

### Screen: Phone Login (Consumer)

**Purpose:** Entry point for consumer buyers.
**Route:** `/(auth)/login`

**Layout:** Single-column, vertically centered.

**Sections:**
1. Invento consumer logo (80px) + tagline: "Shop from your local market."
2. Phone number input (+234 prefix)
3. "Continue" primary button
4. Fine print: "By continuing, you agree to our Terms of Service."

**Actions:**
- "Continue" → validate phone → loading → navigate to OTP screen

### Screen: OTP Verification (Consumer)

**Purpose:** Verify phone ownership.
**Route:** `/(auth)/verify-otp`

**Layout:** Identical to merchant OTP screen.

**Actions:**
- Verify → success → if new user (no name): navigate to Name Setup → else navigate to Home

### Screen: Name Setup

**Purpose:** Collect consumer's name on first login.
**Route:** `/(auth)/name-setup`

**Layout:** Single-column, centered.

**Sections:**
1. "What's your name?" heading
2. First name text input (required)
3. "Get Started" primary button

**Actions:**
- "Get Started" → save name → navigate to Home with welcome toast

## 5.2 Home

**Purpose:** Discovery hub  featured shops, trending products, market browsing.
**Route:** `/(tabs)/home`

**Layout:** Scrollable. Top bar: Invento logo (left) | location chip center ("Kano") | notification bell (right).

**Sections:**

**Search Bar**
- Full-width, prominent, pill shape
- Placeholder: "Search products or shops..."
- Tap → navigate to Search screen (does not open keyboard inline)

**Location Banner**
- "Shopping in Kano" with map-pin icon
- "Change" ghost link → city/market selector bottom sheet

**Featured Markets** (horizontal scroll)
- MarketCard: market image (120x80px, radius 8px) | market name | city | shop count
- Tap → Market Detail screen

**Trending Products** (horizontal scroll)
- ProductCard: image (120x120px) | name (2 lines) | shop name (12px, neutral-600) | price (mono)
- Tap → Product Detail screen

**Nearby Shops** (vertical list)
- ShopCard: logo (48x48) | shop name (bold) | market name | product count | category tags (chips)
- Tap → Shop Profile screen

**Empty State (no shops in area):**
- "No shops in your area yet. Check back soon."

## 5.3 Search & Discovery

**Purpose:** Cross-shop product and shop search with filters.
**Route:** `/(tabs)/search`

**Layout:** Full-screen. Search input auto-focused on arrival. Back arrow.

**Sections:**

**Search Input**
- Auto-focused, keyboard open immediately
- Clear (x) button when text present
- Placeholder: "Search products, shops, markets..."

**Filter Bar** (appears after first search)
- Category chips (horizontal scroll)
- Price range chip → bottom sheet with min/max inputs
- Market filter chip → bottom sheet with market list

**Results Toggle** (after search)
- Segmented control: Products | Shops

**Products Results**
- 2-column grid of ProductCards: image | name | shop name | price
- Tap → Product Detail

**Shops Results**
- List of ShopCards
- Tap → Shop Profile

**Recent Searches** (before typing, if history exists)
- "Recent" section header + "Clear" ghost link
- List of last 5 searches: clock icon + search term
- Tap → re-run that search

**Empty State (no results):**
- "No results for '[query]'"
- "Try a different search or browse markets" + "Browse Markets" button

## 5.4 Market & Shop Browsing

### Screen: Market Detail

**Purpose:** Browse all shops in a specific market.
**Route:** `/(stack)/market/[id]`

**Layout:** Scrollable. Hero image at top (200px height, gradient overlay). Back arrow (white, over image).

**Sections:**

**Hero Section**
- Market image with dark gradient overlay (bottom 60%)
- Market name (white, heading-1, over gradient)
- City, State | Shop count (white, body-sm)

**Filter Bar**
- Category chips: All | [categories from shops in this market]

**Shop Grid**
- 2-column grid of ShopCards
- Each: logo | shop name | product count | top category

**Empty State:**
- "No shops in this market yet"

### Screen: Shop Profile (Consumer)

**Purpose:** View a specific shop and browse its products.
**Route:** `/(stack)/shop/[id]`

**Layout:** Scrollable. Top bar: back arrow | share icon.

**Sections:**

**Shop Header**
- Shop logo (80px, circular, border: 2px white)
- Shop name (heading-1)
- Market name + city (body-sm, neutral-600)
- Description (2 lines, "Show more" if longer)
- Product count badge

**Product Filter Bar**
- Search input (within this shop only)
- Category chips

**Product Grid**
- 2-column grid
- Each ProductCard: image | name | price | "Add to Cart" button (small, secondary)

**Empty State:**
- "No products available in this shop"

### Screen: Product Detail

**Purpose:** Full product information before adding to cart.
**Route:** `/(stack)/shop/[shopId]/product/[id]`

**Layout:** Scrollable. Top bar: back arrow | share icon | cart icon (with badge).

**Sections:**

**Product Image**
- Full-width image (300px height, object-fit: cover)
- Placeholder illustration if no image

**Product Info**
- Product name (heading-1)
- Price (price-md, brand-primary)
- Shop name (body, underlined link → Shop Profile)
- Stock status badge (In Stock / Low Stock / Out of Stock)

**Description**
- Full description text
- "Show more" / "Show less" toggle if > 3 lines

**Quantity Selector**
- [−] | qty (mono, 20px) | [+] stepper
- Max = available stock | Min = 1
- Disabled if out of stock

**Sticky Footer**
- "Add to Cart" primary button (full-width, 56px)
- Disabled + "Out of Stock" label if stock = 0

**Actions:**
- "Add to Cart" → add to cartStore → cart badge updates → success toast "Added to cart"

## 5.5 Cart

**Purpose:** Review items before checkout.
**Route:** `/(tabs)/cart`

**Layout:** Full-screen. Top bar: "Cart" title | "Clear All" ghost link (red, right).

**Sections:**

**Cart Items List**
- Each CartItemRow:
  - Product image (48x48) | name (bold) | shop name (12px, neutral-600)
  - Unit price (mono) | quantity stepper [−][qty][+] | line total (mono, right) | remove [x]

**Order Summary Card**
- Subtotal (mono)
- Delivery fee: "Calculated at checkout"
- Total (bold, larger)

**Checkout Button** (sticky bottom)
- "Proceed to Checkout" primary button (full-width, 56px)

**Empty State:**
- Cart illustration
- "Your cart is empty"
- "Start Shopping" primary button → Home

**Offline State:**
- Cart accessible (local state)
- "Proceed to Checkout" disabled with tooltip: "Internet required to place an order"

## 5.6 Checkout

### Screen: Delivery Details

**Purpose:** Enter delivery address and review order before payment.
**Route:** `/(stack)/checkout`

**Layout:** Scrollable. Top bar: "Checkout" title | back arrow. Step indicator: "1 of 2".

**Sections:**

**Delivery Address**
- Full address textarea (required, min 10 chars)
- Placeholder: "Enter your full delivery address"

**Delivery Note**
- Optional textarea: "e.g. Call when you arrive, gate code is 1234"

**Order Summary** (collapsible)
- Header: "2 items from Musa Fabrics" + chevron
- Expanded: item list with images, names, quantities, prices

**Total**
- Subtotal | Total (bold)

**"Continue to Payment" primary button** (sticky bottom, 56px)

### Screen: Payment (Paystack WebView)

**Purpose:** Complete payment via Paystack embedded checkout.
**Route:** `/(stack)/checkout/payment`

**Layout:** Full-screen WebView. Top bar: "Payment" title | back arrow (with confirmation dialog if tapped).

**Sections:**
- Paystack WebView (full screen)
- Loading state: centered spinner + "Loading payment..." while WebView initializes

**After Payment:**
- Success → navigate to Order Confirmation
- Failed → error screen: "Payment failed" + "Try Again" button + "Cancel Order" ghost link

**Back Arrow Behavior:**
- Confirmation dialog: "Cancel payment? Your order will not be placed." → Cancel / Yes, go back

### Screen: Order Confirmation

**Purpose:** Confirm successful order placement and payment.
**Route:** `/(stack)/checkout/confirmed`

**Layout:** Full-screen, centered content. No back arrow (replaces checkout stack).

**Sections:**

**Success Animation**
- Large animated green checkmark (64px, spring animation)
- "Order Placed!" heading (heading-1)
- "Your payment was received. [Shop Name] will prepare your order." (body, neutral-600)

**Order Summary**
- Order ID: #INV-XXXX (mono, copyable)
- Items summary: "2 items"
- Total paid (mono, bold)

**Actions**
- "Track Order" primary button → Order Detail screen
- "Continue Shopping" ghost link → Home

## 5.7 Orders (Consumer)

### Screen: Order History

**Purpose:** View all past and active orders.
**Route:** `/(tabs)/orders`

**Layout:** Full-screen list. Top bar: "Orders" title.

**Filter Tabs:** Active | Completed | Cancelled

**Order List**
- Each OrderCard:
  - Shop logo (32x32) + shop name (bold)
  - Order ID (mono, 12px) | date
  - Items summary: "2 items"
  - Total (mono) | Status badge
- Tap → Order Detail

**Empty State (per tab):**
- Active: "No active orders. Start shopping!" + "Browse Markets" button
- Completed: "No completed orders yet."
- Cancelled: "No cancelled orders."

### Screen: Order Detail (Consumer)

**Purpose:** Track a specific order in real-time.
**Route:** `/(stack)/orders/[id]`

**Layout:** Scrollable. Top bar: back arrow | "Order #INV-XXXX".

**Sections:**

**Status Timeline**
- Horizontal stepper: Placed → Confirmed → Processing → Shipped → Delivered
- Current step: pulsing animated dot (brand-primary)
- Completed steps: brand-secondary filled
- Future steps: neutral-200

**Estimated Delivery** (shown when status is SHIPPED)
- "Your order is on the way!" with truck icon

**Shop Info Card**
- Shop logo + name | phone number (tappable → dialer)

**Order Items**
- List: image | name | qty | unit price | line total

**Delivery Address**
- Full address text

**Payment Summary**
- Total paid | Payment method | Transaction reference (mono)

**Cancel Button** (shown only when status is PENDING)
- "Cancel Order" destructive button (full-width)
- Confirmation dialog before cancelling

**Realtime Updates:**
- Status timeline updates automatically via Supabase Realtime subscription
- Push notification received → status updates without manual refresh

## 5.8 Profile

**Purpose:** Manage consumer account settings.
**Route:** `/(tabs)/profile`

**Layout:** Scrollable. Top bar: "Profile" title.

**Sections:**

**Profile Header**
- Avatar (initials, 72px, colored circle)
- Name (heading-2) | phone number (body-sm, neutral-600)
- "Edit Name" ghost link → inline name edit

**Menu Items:**
- Order History (receipt icon) → Orders tab
- Language (globe icon) → shows current, tap to toggle English/Hausa
- Help & Support (question-circle icon) → external link or in-app FAQ
- Logout (arrow-right icon, red text) → confirmation dialog

# 6. ADMIN WEB DASHBOARD

## 6.1 Layout Shell

**Persistent across all admin pages.**

**Left Sidebar (240px, collapses to 64px)**
- Top: Invento logo + "Admin" label
- Navigation items with icons and labels
- Active item: brand-primary background, white text, 3px left accent bar
- Bottom: admin name + role badge + logout button
- Collapse toggle button at bottom of sidebar

**Top Header Bar (64px height)**
- Page title (left, heading-2)
- Breadcrumb (below title, body-sm, neutral-600)
- Admin name + role badge (right)
- Notification bell (right)

**Main Content Area**
- Background: `neutral-50`
- Max width: 1280px, centered
- Padding: 32px
- Scrollable independently of sidebar

## 6.2 Authentication

### Page: Admin Login

**Purpose:** Secure login for internal admin team. Email + password only (not OTP).
**Route:** `/login`

**Layout:** Centered card (400px wide) on `neutral-50` background. Invento logo above card.

**Sections:**
- "Admin Portal" heading (heading-1, centered)
- Email text input (required)
- Password text input with show/hide toggle (required)
- "Sign In" primary button (full-width)
- Error state: red inline alert below password: "Invalid email or password"

**No "Forgot password" link**  admin credentials managed internally.

## 6.3 Overview

### Page: Platform Overview

**Purpose:** Platform-wide health and business metrics at a glance.
**Route:** `/dashboard`

**Layout:** Grid layout. 4-column metric row + 2-column chart row + activity feed.

**Sections:**

**Top Metrics Row** (4 metric cards, equal width)
- Total Merchants (active count) | trend vs last month
- Total Orders (this month) | trend
- Total Revenue (this month, ₦) | trend
- New Merchants (this month) | trend

**Charts Row** (2 columns)
- Left: "Merchants by Plan" donut chart  FREE / BASIC / PRO / ENTERPRISE with count + percentage legend
- Right: "Orders Over Time" line chart  last 30 days, Y-axis: order count, X-axis: dates

**Recent Activity Feed** (full width)
- Section header: "Recent Activity"
- Last 20 events: icon | description | time ago
- Event types: merchant joined | order placed | plan changed | market registered | payment received
- "Load more" button

## 6.4 Merchants

### Page: Merchant List

**Purpose:** View and manage all registered merchants.
**Route:** `/merchants`

**Layout:** Full-width. Toolbar above table.

**Toolbar**
- Search input (by name, phone, shop name)  left
- Filter: All Plans | FREE | BASIC | PRO | ENTERPRISE  dropdown
- Filter: All Status | Active | Inactive  dropdown
- Filter: Market  searchable dropdown
- "Export CSV" secondary button  right

**Merchants Table**

Columns: Shop Name | Owner | Phone | Market | Plan | Status | Joined | Actions

- Shop Name: shop logo (24px) + shop name (bold)
- Plan: colored plan badge
- Status: Active (green badge) / Inactive (red badge)
- Joined: formatted date
- Actions: "View" link → Merchant Detail

**Pagination**
- "Showing 1–20 of 347 merchants"
- Previous / Next | Page size: 20 / 50 / 100

### Page: Merchant Detail

**Purpose:** View and manage a specific merchant's account.
**Route:** `/merchants/[id]`

**Layout:** Two-column (info left, actions right) + full-width tables below.

**Left Column  Merchant Info**

Profile Card:
- Shop logo (64px) | shop name (heading-2) | owner name | phone
- Market | city | joined date

Plan Card:
- Current plan badge | expiry date
- Usage progress bar: "42 / 50 products" (FREE plan only)

Stats Card:
- Total Sales (all time) | Total Orders | Total Revenue (₦)

**Right Column  Actions**

Plan Management Card:
- "Change Plan" label
- Plan dropdown: FREE / BASIC / PRO / ENTERPRISE
- Plan expiry date picker (optional)
- "Update Plan" primary button

Account Status Card:
- Toggle: Active / Inactive
- Reason textarea (shown when switching to Inactive)
- "Update Status" primary button

**Bottom  Full Width**

Recent Orders Table:
- Last 10 orders: Order ID | Consumer | Total | Status | Date | "View" link

Recent Sales Table:
- Last 10 POS sales: Date | Items | Total | Payment Method

## 6.5 Markets

### Page: Market List

**Purpose:** View and manage all registered markets.
**Route:** `/markets`

**Layout:** Full-width. Toolbar above table.

**Toolbar**
- Search by name or city  left
- "Register New Market" primary button  right

**Markets Table**

Columns: Market Name | City | State | Shops | Status | Actions

- Status: Active (green) / Inactive (red)
- Actions: "Edit" link | "Deactivate" ghost link (red)

### Page: Register / Edit Market

**Purpose:** Create or update a market entry.
**Route:** `/markets/new` or `/markets/[id]/edit`

**Layout:** Form card (600px max width, centered).

**Sections:**
- Market Name (text input, required)
- City (text input, required)
- State (select dropdown  Nigerian states list)
- Description (textarea, optional)
- Latitude (numeric input, optional  for Phase 3 map)
- Longitude (numeric input, optional)
- Market Image (file upload, optional, max 2MB)

**Footer Actions:**
- "Save Market" primary button
- "Cancel" secondary button → back to Market List

## 6.6 Orders (Admin)

### Page: All Orders

**Purpose:** View all orders across all merchants with filtering.
**Route:** `/orders`

**Layout:** Full-width. Toolbar above table.

**Toolbar**
- Search by order ID or consumer phone
- Filter: Status (all statuses)
- Filter: Merchant (searchable dropdown)
- Date range picker (from / to)

**Orders Table**

Columns: Order ID | Consumer | Merchant | Items | Total | Status | Date | Actions

- Order ID: mono font, copyable
- Status: colored badge
- Actions: "View" link

### Page: Order Detail (Admin)

**Purpose:** View full order details. Operator/Super can override status.
**Route:** `/orders/[id]`

**Layout:** Two-column. Order info left, actions right.

**Left Column:**
- Status timeline (same as merchant view)
- Customer info card
- Order items table
- Payment card

**Right Column:**
- "Override Status" card (Operator/Super only)
  - Status dropdown (all valid statuses)
  - Reason textarea (required)
  - "Override" primary button

**Audit Trail Section** (full width, below)
- Table: Timestamp | Actor | Action | Previous Status | New Status
- Shows full history of status changes

## 6.7 Payments

### Page: Transactions

**Purpose:** View all payment transactions across the platform.
**Route:** `/payments`

**Layout:** Full-width. Summary bar above table.

**Summary Bar** (4 metric cards)
- Total Processed (₦) | Success Count | Failed Count | Pending Count

**Toolbar**
- Search by reference or order ID
- Filter: Status (Pending / Success / Failed / Refunded)
- Date range picker

**Transactions Table**

Columns: Reference | Order ID | Merchant | Consumer | Amount | Channel | Status | Date

- Reference: mono font, copyable
- Amount: mono font
- Channel badge: Card / Bank Transfer / USSD
- Status badge: color-coded

## 6.8 Outbox Monitor

### Page: Dead-Letter Queue

**Purpose:** View and replay failed background events. Super admin only.
**Route:** `/outbox`

**Layout:** Full-width. Summary bar above table.

**Summary Bar** (3 metric cards)
- Pending Events | Processing Events | Failed (Dead-Letter) Events

**Toolbar**
- Filter: Event Type (all types)
- Date range picker
- "Replay Selected" primary button (enabled when rows selected)
- "Replay All Failed" destructive button

**Dead-Letter Table**

Columns: Event ID | Event Type | Aggregate ID | Attempts | Last Error | Created | Actions

- Event Type: mono badge
- Attempts: number (red if at max)
- Actions: "Replay" button | "View Payload" link (opens JSON modal)

**Payload Modal:**
- JSON viewer (syntax highlighted)
- "Close" button

## 6.9 Settings

### Page: Platform Settings

**Purpose:** Platform configuration and admin account management.
**Route:** `/settings`

**Layout:** Tabbed page. Three tabs.

**Tab 1  General**
- Platform name (text input)
- Support email (text input)
- "Save Changes" primary button

**Tab 2  Admin Accounts**

Admin Accounts Table:
- Columns: Name | Email | Role | Last Login | Status | Actions
- Actions: "Edit Role" (dropdown) | "Deactivate" ghost link (red)

"Add Admin" primary button (top-right) → modal:
- Email input (required)
- Name input (required)
- Role selector: Viewer / Operator / Super
- "Send Invite" primary button

**Tab 3  Notifications**
- Alert threshold: "Send alert when outbox pending > [number]" (numeric input)
- Alert threshold: "Send alert when failed events > [number]"
- Alert email (text input)
- "Save" primary button

# 7. USER FLOWS

## 7.1 New Merchant Onboarding

```
1. Download Invento Merchant app from Play Store

2. Phone Login screen
   → Enter phone number → tap "Send Code"
   → System sends OTP via SMS

3. OTP Verification screen
   → Enter 6-digit code → auto-submit
   → System detects new user

4. Onboarding Step 1 (Personal Info)
   → Enter full name → "Continue"

5. Onboarding Step 2 (Shop Info)
   → Enter shop name, description, select market → "Continue"

6. Onboarding Step 3 (Shop Photo)
   → Upload logo (optional) → "Finish Setup"
   → Welcome toast: "Welcome to Invento, [Shop Name]!"

7. Dashboard (empty state)
   → "Add your first product" CTA shown prominently

8. Add Product screen
   → Fill form → "Save Product"
   → Product appears in inventory and POS grid

Total: 8 screens, ~3 minutes to first product
```

## 7.2 POS Sale (Online)

```
1. Tap "POS" tab

2. Find product:
   Option A: Browse grid → tap product tile
   Option B: Tap search → type name → tap result
   Option C: Tap scan icon → scan barcode → product auto-added

3. Build cart:
   → Tap products to add (each tap = +1 unit)
   → Tap cart bar to open Cart Sheet
   → Adjust quantities with steppers

4. Add customer (optional):
   → Tap "Add Customer" → enter phone

5. Select payment method:
   → Tap chip: Cash | Transfer | USSD | Card

6. Tap "Complete Sale  ₦X,XXX"
   → Validate stock → loading (1–2s) → success

7. Receipt screen:
   → Animated checkmark + total
   → "Share Receipt" (WhatsApp) or "New Sale"

Target: Under 60 seconds from opening POS to receipt
```

## 7.3 POS Sale (Offline) + Sync

```
1–5. Same as online flow
   → Offline indicator (amber dot) visible in top bar

6. Tap "Complete Sale"
   → Saved to local WatermelonDB immediately
   → No network request
   → Receipt shown instantly from local data
   → Receipt shows "Pending sync" amber indicator

7. When internet restored:
   → Auto-sync triggers in background
   → Toast: "3 sales synced successfully"
   → If conflict: "1 sale needs review" → Sync Issues screen

8. Sync Issues screen (if needed):
   → Review rejected sales with reason
   → Choose: "Force Record" or "Void Sale" per item
```

## 7.4 Adding a Product

```
1. Tap "+" in Inventory screen
   OR tap "Add Product" quick action on Dashboard

2. Add photo (optional):
   → Tap photo area → camera or gallery
   → Image uploads in background

3. Fill basic info:
   → Product name (required)
   → Category (select)
   → Description (optional)

4. Set price & stock:
   → Price in ₦ (required)
   → Quantity (required)
   → Low stock threshold (default: 5)

5. Add SKU (optional):
   → Type manually OR tap scan → camera → auto-fills

6. Tap "Save Product"
   → Validation → loading → success toast
   → Back to inventory list
   → Product immediately visible in list and POS grid
```

## 7.5 Consumer Buying Online

```
1. Home screen → tap market → Market Detail
   → Tap shop → Shop Profile

2. Browse product grid → tap product → Product Detail

3. Adjust quantity → tap "Add to Cart"
   → Cart badge updates

4. Tap cart tab → Cart screen
   → Review items and total
   → Tap "Proceed to Checkout"

5. Delivery Details screen:
   → Enter delivery address
   → Add delivery note (optional)
   → Tap "Continue to Payment"

6. Payment screen (Paystack WebView):
   → Choose: Card / Bank Transfer / USSD
   → Complete payment

7. Order Confirmation screen:
   → Animated checkmark + "Order Placed!"
   → "Track Order" button

8. Order Detail screen:
   → Status timeline updates in real-time
   → Push notifications at each status change
```

## 7.6 Merchant Fulfilling an Order

```
1. Push notification: "New Order  ₦2,500 from Kano"
   → Tap → deep links to Order Detail

2. Review order:
   → Items, quantities, delivery address, customer info
   → Payment status: "Paid ✓"

3. Tap "Confirm Order"
   → Status: CONFIRMED
   → Consumer push: "Your order has been confirmed"

4. Tap "Mark as Processing"
   → Status: PROCESSING

5. Tap "Mark as Shipped"
   → Status: SHIPPED
   → Consumer push: "Your order is on the way!"

6. Tap "Mark as Delivered"
   → Status: DELIVERED (terminal)
   → Consumer push: "Your order has arrived"
```

## 7.7 Viewing Analytics

```
1. Tap "More" tab → "Analytics"

2. Select period:
   → Tap: Today | Week | Month | Custom
   → Custom: date range picker appears

3. Read charts:
   → Revenue bar chart for selected period
   → Metric cards: total revenue, sales count, avg sale

4. Drill down:
   → Tap bar → tooltip with exact value
   → Scroll to Payment Breakdown donut
   → Scroll to Top Products table

5. View sales history:
   → Paginated list at bottom
   → Tap row → sale receipt (read-only)
```

## 7.8 Admin Managing a Merchant

```
1. Admin login → Overview dashboard

2. Navigate to Merchants → Merchant List

3. Search for merchant by name or phone

4. Tap "View" → Merchant Detail page

5. Review merchant stats and recent activity

6. Change plan (if needed):
   → Select new plan from dropdown
   → Set expiry date (optional)
   → Tap "Update Plan"
   → Merchant receives push notification

7. Deactivate account (if needed):
   → Toggle to Inactive
   → Enter reason
   → Tap "Update Status"
```

# 8. EDGE CASES & STATES

## 8.1 Universal States

**Loading State  per context:**
- List screens: skeleton loaders matching real content shape (6 rows for lists, 4 cards for dashboard)
- Form submit: button spinner, width locked, all inputs disabled
- Full-screen initial load: centered spinner + "Loading..." (only on app launch)
- Inline data refresh: subtle activity indicator in top bar

**Empty State  all screens:**

| Screen | Message | CTA |
|---|---|---|
| Product List | "No products yet. Add your first product." | "Add Product" |
| POS Grid | "No products in inventory." | "Go to Inventory" |
| Orders List | "No orders yet. Share your shop link." | "Share Shop" |
| Customer List | "No customers yet. Added automatically on sale." | None |
| Analytics | "No sales data for this period." | None |
| Consumer Home | "No shops in your area yet." | None |
| Consumer Cart | "Your cart is empty." | "Start Shopping" |
| Consumer Orders | "No orders yet." | "Start Shopping" |
| Admin Merchants | "No merchants found." | None |
| Sync Issues | "All sales synced successfully." | None |
| Notifications | "No notifications yet." | None |

**Error State:**
- Network error: toast "Connection failed. Check your internet and try again." + Retry button
- Server error: toast "Something went wrong. Please try again." + Retry button
- Full-screen error (critical): illustration + "Something went wrong" + "Try Again" button
- Form validation: red border + inline message below field (never above, never on keystroke  only on blur or submit)

**Success State:**
- Form save: success toast + navigate or reset form
- Destructive action: success toast + item removed from list with slide-out animation
- Sale complete: animated checkmark + receipt screen

## 8.2 POS Edge Cases

**Insufficient Stock:**
- Quantity stepper stops at max available stock
- Toast: "Only 3 units of [Product Name] available"
- Product tile shows current stock count to prevent this

**Out of Stock Product:**
- Tile greyed out (40% opacity) with "Out of Stock" overlay
- Not tappable  no add-to-cart action
- Still visible in grid (not hidden)

**Plan Limit Reached (FREE plan):**
- "Save Product" button disabled
- Plan limit banner on Add Product screen: "You've reached the 50-product limit on the Free plan."
- "Upgrade Plan" link in banner

**Sync Conflict (Offline Sale):**
- Sale marked REJECTED or PARTIAL in sync response
- Toast: "1 sale needs your attention"
- Sync Issues screen shows: product name | units attempted | units available | reason
- Options per item: "Force Record" / "Void Sale"

**Barcode Not Found:**
- Scanner closes
- Toast: "No product found for this barcode"
- Option: "Add as new product" with SKU pre-filled

## 8.3 Order Edge Cases

**Payment Timeout (Consumer):**
- PENDING orders with no paystackRef older than 2 hours → auto-cancelled by system
- Consumer sees order as "Cancelled" in history
- No notification (order was never confirmed)

**Order Cancellation (Consumer):**
- Only available when status is PENDING
- Confirmation dialog required
- After cancellation: status "Cancelled", no further actions

**Order Cancellation (Merchant):**
- Available for PENDING and CONFIRMED
- Reason field (optional)
- Consumer receives push notification on cancellation

**Duplicate Order (Double-tap):**
- Consumer taps "Place Order" twice quickly
- Second tap ignored via idempotency key
- Only one order created, one payment initiated

## 8.4 Auth Edge Cases

**Wrong OTP:**
- 1st wrong: "Incorrect code. 2 attempts remaining."
- 2nd wrong: "Incorrect code. 1 attempt remaining."
- 3rd wrong: "Too many attempts. Request a new code."  Resend button activates immediately

**Expired OTP:**
- Error: "This code has expired. Tap Resend to get a new one."
- Resend button shown and enabled

**Session Expired:**
- Next API call fails → user redirected to login screen
- Toast: "Your session has expired. Please log in again."
- authStore cleared

## 8.5 Offline Edge Cases

**What works offline (Merchant App):**
- Full POS: add products, complete sales, view receipt
- Inventory browsing (cached data)
- Dashboard (cached data with "Last updated X min ago")
- Order history (cached data, read-only)

**What requires internet (Merchant App):**
- Syncing offline sales
- Adding/editing products
- Updating order status
- Receiving push notifications

**What works offline (Consumer App):**
- Cart (local state)
- Browsing cached data

**What requires internet (Consumer App):**
- Checkout and payment (blocked with "Internet required" message)
- Real-time order tracking

**Offline Indicator Design:**
- Persistent amber banner at top: "You're offline  POS still works" (merchant) or "You're offline" (consumer)
- Disappears automatically when connection restored
- Replaced by brief green toast: "Back online. Syncing..."

# 9. MOBILE UX PRINCIPLES

## 9.1 Touch Standards

- Minimum touch target: 44x44px for all interactive elements. Never smaller.
- Tap feedback: buttons scale to 0.97 on press (Reanimated spring, 100ms)
- List items: background flash to `neutral-100` on press
- POS product tiles: scale to 0.95 + brief green flash on add-to-cart
- Swipe gestures: product list swipe-left (edit/delete), order swipe-right (confirm), bottom sheets swipe-down (dismiss)
- Long press: product list → multi-select mode; POS tile → custom quantity dialog
- Haptic feedback: light impact on add-to-cart, medium impact on sale complete, error vibration on failed action

## 9.2 POS Speed Rules

The POS is the most time-critical screen. Every decision prioritizes speed.

1. Product grid loads from local WatermelonDB cache  zero network wait
2. Adding to cart is instant  no loading state, no confirmation
3. "Complete Sale" is the only action requiring a network call
4. Offline: "Complete Sale" saves locally and shows receipt immediately
5. Cart total updates in real-time as items are added/removed
6. Payment method selection is a single tap (no dropdown)
7. Customer addition is optional and non-blocking
8. Receipt screen has "New Sale" as primary action  one tap to reset
9. Search in POS: keyboard opens but product grid remains visible above keyboard
10. Numeric inputs: numeric keyboard auto-opens

## 9.3 Offline Usability

- Offline indicator is always visible (amber banner)  never hidden
- Offline sales show "Pending sync" on receipt  merchant knows it's queued
- WatermelonDB product quantities update locally on sale  merchant sees accurate stock
- Sync status icon in POS top bar: cloud-check / cloud-up (animated) / cloud-x
- Tap cloud-x → Sync Issues screen
- Toast on successful sync: "X sales synced successfully"

## 9.4 Navigation Patterns

- Hardware back button (Android): always works, navigates up the stack
- Swipe right from left edge: back navigation on all stack screens
- Back arrow: always present on non-tab screens
- Tab persistence: each tab maintains its own navigation stack
- Deep linking: push notifications deep-link directly to relevant screens
- Modal dismissal: tap backdrop or swipe down (except destructive confirmations)

## 9.5 Form UX

- Validation timing: errors shown on blur (field leave), not on every keystroke
- Format errors (phone, price): shown on blur
- Server errors: shown after submission attempt
- Keyboard avoidance: forms scroll up to keep active input visible
- "Next" keyboard button: advances to next field
- "Done" on last field: submits form or enables submit button
- Auto-formatting: price fields auto-insert comma separators (₦1,500); phone fields auto-format
- Required fields: asterisk (*) in label, validated before submit

## 9.6 Language Support

- Language toggle: Settings / More screen → Language
- Supported: English (default) | Hausa
- All UI text must be translatable: navigation labels, button labels, form labels, error messages, empty states, toasts
- Numbers and currency: always ₦ symbol regardless of language, comma formatting (₦1,500)
- Hausa is LTR  no RTL layout changes needed
- Language preference persisted to MMKV storage

# 10. INTERACTION DESIGN & ANIMATION

## 10.1 Micro-Interactions

Every meaningful action has a micro-interaction that confirms it happened.

| Interaction | Animation | Duration | Easing |
|---|---|---|---|
| Button tap | Scale 1.0 → 0.97 → 1.0 | 120ms | Spring (stiffness 300, damping 20) |
| Add to cart (POS tile) | Scale 0.95 → 1.0 + green flash | 150ms | Spring |
| Cart badge increment | Scale 1.0 → 1.3 → 1.0 | 200ms | Spring |
| Swipe action reveal | Slide with rubber-band at limits | 250ms | Ease-out |
| Bottom sheet open | Slide up from bottom | 300ms | Spring (stiffness 200, damping 25) |
| Bottom sheet close | Slide down | 250ms | Ease-in |
| Modal open | Fade in + scale 0.95 → 1.0 | 200ms | Ease-out |
| Toast appear | Slide in from top (mobile) / right (web) | 250ms | Spring |
| Toast dismiss | Fade out + slide | 200ms | Ease-in |
| Success checkmark | Draw stroke animation | 400ms | Ease-in-out |
| Skeleton shimmer | Left-to-right gradient sweep | 1200ms | Linear, infinite |
| Tab switch | Crossfade content | 150ms | Ease |
| Status badge change | Color crossfade | 300ms | Ease |
| Offline banner appear | Slide down from top | 300ms | Spring |
| Sync icon (uploading) | Rotate 360° | 1000ms | Linear, infinite |

**Rules:**
- Never animate more than 2 elements simultaneously on mobile
- All animations must respect the OS "Reduce Motion" accessibility setting  replace with instant transitions
- Loading spinners: 1000ms rotation, linear, infinite

## 10.2 Optimistic Updates

For speed-critical actions, update the UI immediately before the server confirms.

| Action | Optimistic Behavior | Rollback on Error |
|---|---|---|
| Add to cart | Cart count increments instantly | Decrement + error toast |
| Remove from cart | Item disappears instantly | Re-add item + error toast |
| Update quantity | Quantity updates instantly | Revert + error toast |
| Mark order as Confirmed | Status badge updates instantly | Revert + error toast |
| Delete product | Product slides out of list instantly | Re-insert + error toast |
| Toggle staff active/inactive | Badge updates instantly | Revert + error toast |

**Rule:** Never use optimistic updates for financial operations (sale creation, payment). Always wait for server confirmation.

## 10.3 Search & Input Performance

- Search inputs: debounce 300ms before firing API call
- Search inputs: minimum 2 characters before searching
- Autocomplete: show results after 300ms debounce, max 8 suggestions
- Price inputs: format on blur (not on keystroke) to avoid cursor jumping
- Phone inputs: format as user types (0801 234 5678 pattern)
- Large lists (> 100 items): use FlashList with `estimatedItemSize` for performance
- Images: lazy load with placeholder, progressive loading via Expo Image
- Infinite scroll: load next page when user is 5 items from the bottom

## 10.4 Permission Request Flows

Permissions must be requested in context, not on app launch.

**Camera Permission (Barcode Scanner):**
- Trigger: user taps "Scan Barcode" button
- Pre-prompt: "Invento needs camera access to scan product barcodes." + "Allow" / "Not Now"
- If denied: show inline message "Camera access required. Enable in Settings." + "Open Settings" link
- Never request camera on app launch

**Notification Permission (Push Notifications):**
- Trigger: after successful first login, on Dashboard
- Pre-prompt card (not OS dialog): "Get notified about new orders and low stock alerts." + "Enable Notifications" primary button + "Maybe Later" ghost link
- If "Enable Notifications": show OS permission dialog
- If denied: show in Settings screen as "Enable Notifications" toggle with explanation
- Never block the user  notifications are optional

**Photo Library Permission (Product Photo):**
- Trigger: user taps "Choose from Gallery" in image picker
- Standard OS permission dialog
- If denied: show toast "Photo access required. Enable in Settings." + "Open Settings" link

# 11. ACCESSIBILITY GUIDELINES

## 11.1 Color & Contrast

All text and interactive elements must meet WCAG 2.1 AA minimum contrast ratios.

| Element | Minimum Contrast | Our Values |
|---|---|---|
| Body text on white | 4.5:1 | `neutral-900` (#111928) on white = 18.1:1 ✓ |
| Secondary text on white | 4.5:1 | `neutral-600` (#6B7280) on white = 5.9:1 ✓ |
| White text on brand-primary | 4.5:1 | White on #1A56DB = 5.1:1 ✓ |
| Placeholder text | 3:1 | `neutral-600` on white = 5.9:1 ✓ |
| Disabled text | No requirement | `neutral-400`  clearly non-interactive |
| Error text | 4.5:1 | `#E02424` on white = 4.6:1 ✓ |
| Status badges | 3:1 (large text) | All badge combinations verified ✓ |

**Color blindness considerations:**
- Never use color alone to convey meaning  always pair with icon or text label
- Status badges: use both color AND text (e.g., "Low Stock" not just amber color)
- Error states: use both red border AND error message text
- Charts: use patterns or labels in addition to color segments

## 11.2 Touch & Motor Accessibility

- All touch targets: minimum 44x44px (Apple HIG) / 48x48dp (Material Design)
- Spacing between adjacent targets: minimum 8px
- Swipe actions: always have a tap alternative (e.g., swipe-to-delete also has a delete button in detail view)
- Long press: always has a tap alternative
- Drag interactions: always have a button alternative

## 11.3 Screen Reader Support

- All images: descriptive `accessibilityLabel` (e.g., "Product photo of Ankara Fabric")
- Icon buttons: `accessibilityLabel` describing the action (e.g., "Scan barcode", "Close modal")
- Status badges: `accessibilityLabel` includes full text (e.g., "Order status: Pending")
- Charts: provide text summary below chart (e.g., "Total revenue ₦45,200 this week")
- Loading states: `accessibilityLiveRegion="polite"` on loading indicators
- Error messages: `accessibilityLiveRegion="assertive"` on error toasts
- Form inputs: `accessibilityLabel` matches visible label text
- Lists: `accessibilityRole="list"` on FlashList containers

## 11.4 Dynamic Font Size

- All text uses relative units (sp on Android, pt on iOS)  never hardcoded px
- Layouts tested at 85%, 100%, 125%, 150% system font scale
- At 150% scale: single-line labels may wrap  all layouts must accommodate 2-line labels
- Minimum font size: 12px (label style)  never smaller regardless of scale
- POS product tiles: product name truncates at 2 lines with ellipsis at any font scale

## 11.5 One-Handed Use (Mobile)

The POS is used by cashiers holding the phone in one hand.

- Primary actions (Complete Sale, Add to Cart) are in the bottom 40% of screen
- Cart Summary Bar is at the very bottom  thumb-reachable
- Category filter chips are horizontally scrollable  reachable with thumb
- Search icon in top bar: acceptable (two-handed)  not a primary POS action
- Bottom sheets: all primary actions in bottom 50% of sheet
- Floating action button (+): bottom-right corner, 56px, thumb zone

# 12. DESIGN QUALITY STANDARDS

## 12.1 Spacing Consistency Rules

- Never use arbitrary spacing values  always use the 4px grid (4, 8, 12, 16, 20, 24, 32, 48, 64)
- Section gaps: always 24px between major sections
- Card internal padding: always 16px
- List item padding: 12px vertical, 16px horizontal
- Icon-to-text gap: always 8px
- Form field gap (label to input): 4px; field to field: 16px

## 12.2 Elevation & Shadow System

| Level | Usage | Shadow |
|---|---|---|
| 0 | Flat elements, dividers | None |
| 1 | Cards, list items | `0 1px 3px rgba(0,0,0,0.08)` |
| 2 | Dropdowns, tooltips | `0 4px 12px rgba(0,0,0,0.12)` |
| 3 | Modals, bottom sheets | `0 8px 24px rgba(0,0,0,0.16)` |
| 4 | Toasts, floating buttons | `0 12px 32px rgba(0,0,0,0.20)` |

## 12.3 Border Radius System

| Element | Radius |
|---|---|
| Buttons | 8px |
| Inputs | 8px |
| Cards | 12px |
| Chips / badges | 11px (pill) |
| Images (product) | 8px |
| Images (avatar) | 50% (circle) |
| Bottom sheets | 16px top corners only |
| Modals | 12px |
| Toasts | 8px |
| Search input | 24px (pill) |

## 12.4 Icon System

- Icon library: Lucide Icons (consistent stroke width, open source)
- Sizes: 16px (inline), 20px (standard), 24px (navigation), 32px (feature icons)
- Stroke width: 1.5px for all sizes
- Color: inherits from parent text color by default
- Navigation icons: 24px, `neutral-600` inactive, `brand-primary` active
- Action icons: 20px
- Status icons: 16px inline with text

## 12.5 Image Guidelines

- Product images: 1:1 ratio (square), minimum 400x400px upload
- Shop logos: 1:1 ratio (square, displayed circular), minimum 200x200px
- Market images: 16:9 ratio, minimum 800x450px
- All images: WebP format preferred, JPEG fallback
- Placeholder: neutral-100 background + product/shop icon in neutral-300
- Loading: progressive blur-up (low-res placeholder → full resolution)
- Error: placeholder illustration, never broken image icon

# 13. NOTIFICATION & FEEDBACK CATALOG

## 13.1 Push Notifications

| Trigger | Title | Body | Deep Link |
|---|---|---|---|
| New online order received | "New Order" | "₦2,500 order received from Kano" | Order Detail |
| Order confirmed (consumer) | "Order Confirmed" | "Your payment was received. [Shop] is preparing your order." | Order Detail |
| Order shipped (consumer) | "Order Shipped" | "Your order is on the way!" | Order Detail |
| Order delivered (consumer) | "Order Delivered" | "Your order has arrived. Enjoy!" | Order Detail |
| Order cancelled (consumer) | "Order Cancelled" | "Your order #INV-0042 was cancelled." | Order Detail |
| Order cancelled (merchant) | "Order Cancelled" | "An order was cancelled by the customer." | Order Detail |
| Low stock alert | "Low Stock Alert" | "[Product Name] has only 3 units left." | Inventory (filtered to low stock) |
| Daily sales summary | "Today's Summary" | "You made ₦15,400 in 12 sales today." | Analytics |
| Plan changed (merchant) | "Plan Updated" | "Your plan has been upgraded to BASIC." | Subscription screen |
| Sync complete | "Sales Synced" | "3 offline sales have been synced." | Dashboard |

## 13.2 In-App Toasts

| Trigger | Type | Message |
|---|---|---|
| Product saved | Success | "Product added successfully" |
| Product updated | Success | "Product updated" |
| Product deleted | Success | "Product removed" |
| Sale completed (online) | Success | "Sale recorded" |
| Sales synced | Success | "X sales synced successfully" |
| Order status updated | Success | "Order marked as [Status]" |
| Profile saved | Success | "Profile updated" |
| Added to cart | Success | "Added to cart" |
| Back online | Success | "Back online. Syncing your data..." |
| Insufficient stock | Warning | "Only X units of [Product] available" |
| Plan limit reached | Warning | "You've reached your product limit. Upgrade to add more." |
| Network error | Error | "Connection failed. Check your internet and try again." |
| OTP wrong | Error | "Incorrect code. X attempts remaining." |
| Sync conflict | Error | "X sale(s) need your attention" |
| Session expired | Info | "Your session has expired. Please log in again." |
| Barcode not found | Info | "No product found for this barcode" |

## 13.3 Confirmation Dialogs

All destructive actions require a confirmation dialog before executing.

| Action | Title | Body | Confirm Button |
|---|---|---|---|
| Delete product | "Delete Product?" | "This will remove [Product Name] from your inventory. This cannot be undone." | "Delete" (red) |
| Cancel order (merchant) | "Cancel Order?" | "This will cancel order #INV-0042 and notify the customer." | "Cancel Order" (red) |
| Cancel order (consumer) | "Cancel Order?" | "Are you sure you want to cancel this order?" | "Yes, Cancel" (red) |
| Clear cart | "Clear Cart?" | "This will remove all items from your cart." | "Clear" (red) |
| Logout | "Log Out?" | "You'll need to verify your phone number to log back in." | "Log Out" (red) |
| Force record sale | "Force Record?" | "Record this sale even though stock is insufficient? Stock will go negative." | "Force Record" (red) |
| Void sale | "Void Sale?" | "This sale will be permanently removed." | "Void" (red) |
| Deactivate merchant (admin) | "Deactivate Merchant?" | "This will prevent [Shop Name] from accessing their account." | "Deactivate" (red) |
| Cancel payment (consumer) | "Cancel Payment?" | "Your order will not be placed if you go back." | "Yes, Cancel" (red) |

## 13.4 Progress Indicators

**Sync Status Icon (Merchant App top bar):**
- Cloud + checkmark: all data synced (brand-secondary)
- Cloud + up arrow (animated): syncing in progress (brand-primary)
- Cloud + x: sync failed (brand-accent)  tap for Sync Issues screen

**Bulk Import Progress:**
- Progress bar with percentage: "Importing 47 products... 23/47"
- Cannot navigate away during import (modal blocks navigation)
- Completes with results summary

**Image Upload Progress:**
- Circular progress overlay on image preview
- Percentage shown in center
- Form can be filled while upload runs in background
- "Save Product" button disabled until upload completes

**Order Status Timeline:**
- Animated pulsing dot on current step (consumer view)
- Smooth transition animation when status changes via Realtime

# APPENDIX A: COMPLETE SCREEN INVENTORY

## Merchant Mobile App (26 screens)

| # | Screen | Route |
|---|---|---|
| 1 | Phone Login | `/(auth)/login` |
| 2 | OTP Verification | `/(auth)/verify-otp` |
| 3 | Onboarding Step 1  Personal Info | `/(auth)/onboarding/step-1` |
| 4 | Onboarding Step 2  Shop Info | `/(auth)/onboarding/step-2` |
| 5 | Onboarding Step 3  Shop Photo | `/(auth)/onboarding/step-3` |
| 6 | Dashboard | `/(tabs)/dashboard` |
| 7 | Product List | `/(tabs)/inventory` |
| 8 | Add Product | `/(tabs)/inventory/add` |
| 9 | Edit Product | `/(tabs)/inventory/[id]` |
| 10 | Bulk Import | `/(tabs)/inventory/bulk-import` |
| 10b | Import Job Status | `/(tabs)/inventory/import-job/[jobId]` |
| 11 | Barcode Scanner | `/(modals)/scan` |
| 12 | POS Main | `/(tabs)/pos` |
| 13 | Cart Sheet | (bottom sheet on POS Main) |
| 14 | Sale Detail (Read-Only) | `/(modals)/receipt/[saleId]` |
| 15 | Sync Issues | `/(tabs)/pos/sync-issues` |
| 16 | Orders List | `/(tabs)/orders` |
| 17 | Order Detail | `/(tabs)/orders/[id]` |
| 18 | Analytics | `/(more)/analytics` |
| 19 | Customer List | `/(more)/customers` |
| 20 | Customer Profile | `/(more)/customers/[id]` |
| 21 | More Menu | `/(tabs)/more` |
| 22 | Shop Profile | `/(more)/shop-profile` |
| 23 | Staff Management | `/(more)/staff` |
| 24 | Subscription Plan | `/(more)/subscription` |
| 25 | Notifications | `/(more)/notifications` |

## Consumer Mobile App (16 screens)

| # | Screen | Route |
|---|---|---|
| 1 | Phone Login | `/(auth)/login` |
| 2 | OTP Verification | `/(auth)/verify-otp` |
| 3 | Name Setup | `/(auth)/name-setup` |
| 4 | Home | `/(tabs)/home` |
| 5 | Search | `/(tabs)/search` |
| 6 | Market Detail | `/(stack)/market/[id]` |
| 7 | Shop Profile | `/(stack)/shop/[id]` |
| 8 | Product Detail | `/(stack)/shop/[shopId]/product/[id]` |
| 9 | Cart | `/(tabs)/cart` |
| 10 | Delivery Details | `/(stack)/checkout` |
| 11 | Payment (Paystack WebView) | `/(stack)/checkout/payment` |
| 12 | Order Confirmation | `/(stack)/checkout/confirmed` |
| 13 | Order History | `/(tabs)/orders` |
| 14 | Order Detail | `/(stack)/orders/[id]` |
| 15 | Profile | `/(tabs)/profile` |
| 16 | Notifications | (accessible from profile) |

## Admin Web Dashboard (12 pages)

| # | Page | Route |
|---|---|---|
| 1 | Admin Login | `/login` |
| 2 | Platform Overview | `/dashboard` |
| 3 | Merchant List | `/merchants` |
| 4 | Merchant Detail | `/merchants/[id]` |
| 5 | Market List | `/markets` |
| 6 | Register / Edit Market | `/markets/new` or `/markets/[id]/edit` |
| 7 | All Orders | `/orders` |
| 8 | Order Detail (Admin) | `/orders/[id]` |
| 9 | Transactions | `/payments` |
| 10 | Dead-Letter Queue | `/outbox` |
| 11 | Platform Settings | `/settings` |

**Total: 54 screens across all platforms**

# APPENDIX B: COMPONENT CHECKLIST

## Atoms
- [ ] Button  Primary, Secondary, Destructive, Ghost, Icon
- [ ] Input  Text, Phone, OTP, Search, Numeric, Textarea, File Upload
- [ ] Badge  Plan, Status, Stock, Payment Method
- [ ] Avatar  initials-based, colored
- [ ] Icon  16px, 20px, 24px sizes
- [ ] Spinner / Loader
- [ ] Divider  horizontal, vertical
- [ ] Checkbox
- [ ] Toggle / Switch
- [ ] Progress Bar  linear, circular
- [ ] Skeleton Loader  text line, card, list row

## Molecules
- [ ] Form Field  label + input + error message
- [ ] Search Bar  with clear button
- [ ] Filter Chip  single and multi-select
- [ ] Metric Card  label + value + trend
- [ ] Product Card  list row (inventory)
- [ ] Product Tile  grid (POS)
- [ ] Order Card  list row
- [ ] Customer Card  list row
- [ ] Shop Card  consumer browse
- [ ] Market Card  consumer browse
- [ ] Cart Item Row  with stepper
- [ ] Sale Row  history list
- [ ] Staff Card  list row
- [ ] Toast Notification  success, error, warning, info
- [ ] Inline Alert Banner  full-width
- [ ] Status Timeline  horizontal stepper
- [ ] Sync Status Icon  cloud variants

## Organisms
- [ ] Bottom Tab Bar  Merchant (5 tabs)
- [ ] Bottom Tab Bar  Consumer (5 tabs)
- [ ] Admin Sidebar  with collapse
- [ ] Top Navigation Bar  mobile and web
- [ ] Product List  with search + filter + empty state
- [ ] POS Product Grid  with category filter
- [ ] Cart Sheet  bottom sheet with items + payment
- [ ] Order Status Timeline  with animation
- [ ] Analytics Chart Section  bar + donut
- [ ] Data Table  web, with sort + pagination
- [ ] Confirmation Dialog  with icon
- [ ] Bottom Sheet  with handle bar
- [ ] Offline Banner  persistent amber
- [ ] Plan Limit Banner  inline warning
- [ ] Sync Issues List  with actions
- [ ] Onboarding Progress  3-step dots
- [ ] Permission Request Card  pre-OS-dialog prompt
- [ ] Import Job Status Card  with progress bar
- [ ] Sale Detail View  read-only receipt
- [ ] Payload JSON Viewer  admin outbox modal

*Document Version 2.0  March 2026*
*Invento UX/UI Specification  Confidential*
*Ready for Google AI Stitch input*
