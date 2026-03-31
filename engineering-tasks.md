# INVENTO - ENGINEERING TASK BREAKDOWN
## Complete Frontend + Backend Task Document
### Version 1.0 | March 2026

# TABLE OF CONTENTS

- [1. PROJECT OVERVIEW](#1-project-overview)
  - [Core Modules](#core-modules)
- [2. GLOBAL SYSTEM TASKS](#2-global-system-tasks)
  - [Backend](#backend)
    - [Project Setup](#project-setup)
    - [Database Setup](#database-setup)
    - [Authentication Integration](#authentication-integration)
    - [Multi-Tenancy Enforcement](#multi-tenancy-enforcement)
    - [Error Handling System](#error-handling-system)
    - [Outbox System](#outbox-system)
    - [Rate Limiting](#rate-limiting)
    - [Integrations Setup](#integrations-setup)
  - [Frontend](#frontend)
    - [Monorepo Setup](#monorepo-setup)
    - [Shared API Package](#shared-api-package)
    - [Shared Types Package](#shared-types-package)
    - [Shared Utils Package](#shared-utils-package)
    - [Global State Management](#global-state-management)
    - [WatermelonDB Setup](#watermelondb-setup)
    - [UI Layout System (Merchant App)](#ui-layout-system-merchant-app)
    - [UI Layout System (Consumer App)](#ui-layout-system-consumer-app)
    - [UI Layout System (Admin Web)](#ui-layout-system-admin-web)
- [3. FEATURE-BY-FEATURE TASK BREAKDOWN](#3-feature-by-feature-task-breakdown)
  - [FEATURE: AUTHENTICATION](#feature-authentication)
    - [Backend Tasks](#backend-tasks)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app)
    - [Frontend Tasks (Consumer App)](#frontend-tasks-consumer-app)
    - [Frontend Tasks (Admin Web)](#frontend-tasks-admin-web)
    - [Integration Notes](#integration-notes)
  - [FEATURE: MERCHANT ONBOARDING](#feature-merchant-onboarding)
    - [Backend Tasks](#backend-tasks-1)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-1)
    - [Integration Notes](#integration-notes-1)
  - [FEATURE: INVENTORY MANAGEMENT](#feature-inventory-management)
    - [Backend Tasks](#backend-tasks-2)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-2)
    - [Integration Notes](#integration-notes-2)
  - [FEATURE: POS SYSTEM](#feature-pos-system)
    - [Backend Tasks](#backend-tasks-3)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-3)
    - [Integration Notes](#integration-notes-3)
  - [FEATURE: ORDERS](#feature-orders)
    - [Backend Tasks](#backend-tasks-4)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-4)
    - [Frontend Tasks (Consumer App)](#frontend-tasks-consumer-app-1)
    - [Integration Notes](#integration-notes-4)
  - [FEATURE: PAYMENTS](#feature-payments)
    - [Backend Tasks](#backend-tasks-5)
    - [Frontend Tasks (Consumer App)](#frontend-tasks-consumer-app-2)
    - [Integration Notes](#integration-notes-5)
  - [FEATURE: CUSTOMER CRM](#feature-customer-crm)
    - [Backend Tasks](#backend-tasks-6)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-5)
    - [Integration Notes](#integration-notes-6)
  - [FEATURE: ANALYTICS](#feature-analytics)
    - [Backend Tasks](#backend-tasks-7)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-6)
    - [Integration Notes](#integration-notes-7)
  - [FEATURE: DASHBOARD](#feature-dashboard)
    - [Backend Tasks](#backend-tasks-8)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-7)
  - [FEATURE: STOREFRONT (PUBLIC MARKETPLACE)](#feature-storefront-public-marketplace)
    - [Backend Tasks](#backend-tasks-9)
    - [Frontend Tasks (Consumer App)](#frontend-tasks-consumer-app-3)
    - [Integration Notes](#integration-notes-8)
  - [FEATURE: SETTINGS](#feature-settings)
    - [Backend Tasks](#backend-tasks-10)
    - [Frontend Tasks (Merchant App)](#frontend-tasks-merchant-app-8)
  - [FEATURE: ADMIN DASHBOARD](#feature-admin-dashboard)
    - [Backend Tasks](#backend-tasks-11)
    - [Frontend Tasks (Admin Web)](#frontend-tasks-admin-web-1)
- [4. CROSS-CUTTING FEATURES](#4-cross-cutting-features)
  - [OFFLINE SUPPORT](#offline-support)
    - [Backend Tasks](#backend-tasks-12)
    - [Frontend Tasks](#frontend-tasks)
  - [CACHING](#caching)
    - [Backend Tasks](#backend-tasks-13)
    - [Frontend Tasks](#frontend-tasks-1)
  - [NOTIFICATIONS](#notifications)
    - [Backend Tasks](#backend-tasks-14)
    - [Frontend Tasks](#frontend-tasks-2)
- [5. BACKGROUND JOBS & EVENTS](#5-background-jobs-events)
    - [Backend Tasks](#backend-tasks-15)
- [6. ERROR HANDLING & EDGE CASES](#6-error-handling-edge-cases)
    - [Backend Tasks](#backend-tasks-16)
    - [Frontend Tasks](#frontend-tasks-3)
- [7. TESTING TASKS](#7-testing-tasks)
    - [Backend Tasks](#backend-tasks-17)
    - [Frontend Tasks](#frontend-tasks-4)
- [8. TASK DEPENDENCY ORDER](#8-task-dependency-order)
  - [Phase 1: Foundation (Week 1-2)](#phase-1-foundation-week-1-2)
  - [Phase 2: Authentication + Core Setup (Week 2-3)](#phase-2-authentication-core-setup-week-2-3)
  - [Phase 3: Core Features (Week 3-6)](#phase-3-core-features-week-3-6)
  - [Phase 4: Cross-Cutting + Polish (Week 6-8)](#phase-4-cross-cutting-polish-week-6-8)
  - [Phase 5: Testing + Hardening (Week 8-10)](#phase-5-testing-hardening-week-8-10)
- [9. INTEGRATION REFERENCE](#9-integration-reference)
  - [Request/Response Contracts](#requestresponse-contracts)
    - [POST /api/v1/pos/sales](#post-apiv1possales)
    - [POST /api/v1/pos/sales/sync](#post-apiv1possalessync)
    - [POST /api/v1/orders](#post-apiv1orders)
    - [PATCH /api/v1/orders/:id/status](#patch-apiv1ordersidstatus)
    - [POST /api/v1/payments/webhook](#post-apiv1paymentswebhook)
    - [GET /api/v1/pos/sales/summary](#get-apiv1possalessummary)
    - [GET /api/v1/sync/pull](#get-apiv1syncpull)

# 1. PROJECT OVERVIEW

## Core Modules

# 2. GLOBAL SYSTEM TASKS

## Backend

### Project Setup
- Initialize NestJS monorepo with `nest new backend`, enable strict TypeScript
- Install and configure: Prisma, @nestjs/config, Joi, nestjs-pino, Helmet, @upstash/ratelimit, Sentry
- Create folder structure: `common/`, `config/`, `prisma/`, `outbox/`, `modules/`, `integrations/`
- Configure `main.ts`: Helmet, CORS with ALLOWED_ORIGINS, global prefix `api/v1`, ValidationPipe (whitelist: true, transform: true), ResponseTransformInterceptor, GlobalExceptionFilter
- Create Joi validation schema in `config/validation.schema.ts` that validates all required env vars at startup and refuses to start if any are missing
- Create `.env.example` with all required variables: DATABASE_URL, DIRECT_URL, SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY, SUPABASE_JWT_SECRET, JWT_ADMIN_SECRET, PAYSTACK_SECRET_KEY, TERMII_API_KEY, REDIS_URL, SENTRY_DSN, ALLOWED_ORIGINS

### Database Setup
- Write complete Prisma schema in `prisma/schema.prisma` with all models: Merchant, Consumer, Admin, AdminRefreshToken, Market, Product, Sale, SaleItem, Customer, Order, OrderItem, Transaction, OutboxEvent, AuditLog, ImportJob, DailySalesSummary
- Run `prisma migrate dev` to create initial migration
- Create Prisma migration that adds `search_vector tsvector GENERATED ALWAYS AS (...)` column to Product table and creates GIN index `product_search_gin`
- Create Prisma migration that enables RLS on Product, Sale, Order, Customer, SaleItem tables and creates `merchant_isolation` policies using `current_setting('app.current_merchant_id', true)`
- Create `PrismaService` in `prisma/prisma.service.ts` that extends PrismaClient, implements OnModuleInit, and registers `$use` middleware that: reads merchantId from RequestContext, calls `set_config('app.current_merchant_id', merchantId)`, injects `WHERE merchantId` on findMany/findFirst/findUnique/count/aggregate/update/delete, injects `merchantId` on create

### Authentication Integration
- Create `SupabaseAuthGuard` in `common/guards/supabase-auth.guard.ts` that: checks `@Public()` decorator, extracts Bearer token, calls `jwt.verify(token, SUPABASE_JWT_SECRET)`, attaches payload to request, calls `RequestContext.set('merchantId', payload.merchantId)` if present
- Create `RolesGuard` in `common/guards/roles.guard.ts` that reads `@Roles()` decorator and compares against `request.user.role`
- Create `@CurrentUser()` decorator that extracts `request.user` from execution context
- Create `@Public()` decorator that sets metadata to bypass JWT guard
- Create `@Roles()` decorator that sets required roles metadata
- Create `RequestContext` class using AsyncLocalStorage to carry merchantId through request lifecycle
- Deploy Supabase Edge Function `send-sms-otp` that calls Termii API with OTP payload
- Deploy Supabase Edge Function `custom-access-token` that queries Merchant table by phone and injects `role`, `merchantId`, `plan` into JWT claims
- Register both Edge Functions in Supabase Dashboard under Authentication Hooks

### Multi-Tenancy Enforcement
- Verify Prisma middleware injects merchantId on all TENANT_SCOPED_MODELS: Product, Sale, SaleItem, Order, OrderItem, Customer
- Write integration test that creates two merchants, creates product for merchantA, sets context to merchantB, calls getProducts, asserts empty result

### Error Handling System
- Create `GlobalExceptionFilter` in `common/filters/global-exception.filter.ts` that: catches all exceptions, maps HttpException to status + code + message, maps Prisma P2002 to 409 DUPLICATE_ENTRY, maps Prisma P2025 to 404 NOT_FOUND, maps Prisma P2003 to 400 FOREIGN_KEY_VIOLATION, logs 5xx errors to Pino and Sentry, returns `{ success: false, error: { code, message, details }, timestamp, path }`
- Create `ResponseTransformInterceptor` that wraps all 2xx responses in `{ success: true, data, meta?, timestamp }`
- Create `LoggingInterceptor` that logs method, url, statusCode, responseTime, merchantId on every request
- Initialize Sentry in `main.ts` with DSN from env, tracesSampleRate: 0.1, Prisma integration

### Outbox System
- Create `OutboxService` in `outbox/outbox.service.ts` with method `createEvent(tx, eventType, aggregateId, payload, priority)` that writes OutboxEvent inside a passed Prisma transaction
- Create `OutboxRelayService` in `outbox/outbox-relay.service.ts` with `@Cron('* * * * * *')` method that: acquires Redis distributed lock `outbox:relay:lock` with 5s TTL using NX, resets stuck PROCESSING events older than 30s back to PENDING, fetches up to 100 PENDING events ordered by priority ASC then createdAt ASC where nextRetryAt is null or lte now, separates into critical (priority=1) and normal (priority>1), processes critical sequentially, processes normal with Promise.allSettled, marks each PROCESSED or sets nextRetryAt on failure, marks FAILED after maxAttempts and sends Sentry alert
- Create `RETRY_POLICY` map in `outbox/retry-policy.ts` with maxAttempts and baseDelayMs per event type
- Create `getNextRetryAt(eventType, attempts)` function using exponential backoff capped at 1 hour

### Rate Limiting
- Create `RateLimitGuard` using `@upstash/ratelimit` with: global sliding window 100 req/min per IP, per-tenant sliding window 60 req/min per merchantId, in-memory fallback when Redis is unavailable
- Create `SyncRateLimitGuard` with sliding window 10 req/min per merchantId for POST /pos/sales/sync

### Integrations Setup
- Create `PaystackService` in `integrations/paystack/paystack.service.ts` with methods: `initializeTransaction(email, amount, reference, metadata)`, `verifySignature(rawBody, signature)` using HMAC-SHA512
- Create `TermiiService` in `integrations/termii/termii.service.ts` with method `sendSMS(to, message)` calling Termii API
- Create `FirebaseService` in `integrations/firebase/firebase.service.ts` with method `send(token, title, body, messageId)` using Firebase Admin SDK, setting `collapseKey` on Android and `apns-collapse-id` on iOS for deduplication
- Create `RedisService` wrapper with typed `get<T>`, `set`, `del` methods

## Frontend

### Monorepo Setup
- Initialize npm workspaces monorepo with `package.json` at root defining workspaces: `apps/*`, `packages/*`
- Create `apps/merchant` with Expo Bare workflow: `npx create-expo-app merchant --template bare-minimum`
- Create `apps/consumer` with Expo Bare workflow
- Create `apps/web` with Next.js 14: `npx create-next-app web --typescript --tailwind --app`
- Create `packages/shared-ui`, `packages/shared-api`, `packages/shared-types`, `packages/shared-utils`, `packages/db`
- Create `tsconfig.base.json` at root with strict mode, path aliases for all packages
- Create `.eslintrc.base.js` with rules: `@typescript-eslint/no-explicit-any: error`, `react-hooks/exhaustive-deps: warn`, `no-console: warn`
- Configure Husky + lint-staged to run ESLint and Prettier on every commit

### Shared API Package
- Create Axios client in `packages/shared-api/client.ts` with: baseURL from env, request interceptor that reads `supabase.auth.getSession()` and attaches Bearer token, response interceptor that unwraps `res.data.data`, handles session expiry by navigating to login
- Create typed API hook factories using TanStack Query v5 for each module

### Shared Types Package
- Run `prisma-json-types-generator` to generate TypeScript types from Prisma schema into `packages/shared-types`
- Export all types: Product, Sale, Order, Customer, Merchant, Consumer, Market, Transaction, OutboxEvent

### Shared Utils Package
- Create `currency.ts` with `formatNaira(amount: Decimal): string` that formats as "₦1,500"
- Create `date.ts` with `formatRelative(date: Date): string` and `formatDate(date: Date): string`
- Create `validation.ts` with phone number validator for +234 format

### Global State Management
- Create `authStore.ts` in `packages/shared-api/store/` using Zustand with persist middleware using MMKV storage: fields `user`, `accessToken`, `merchantId`, `plan`; actions `setSession`, `clearSession`
- Create `cartStore.ts` in `apps/merchant/features/pos/store/` using Zustand in-memory (no persist): fields `items: CartItem[]`, `total: Decimal`; actions `addItem`, `removeItem`, `updateQuantity`, `clearCart`

### WatermelonDB Setup
- Define schema version 2 in `packages/db/schema.ts` with tables: products (server_id, merchant_id, name, sku, price, quantity, low_stock_at, category, image_url, is_active, updated_at), sales (local_id, merchant_id, customer_phone, total, payment_method, synced_at, sync_status, sync_error, created_at), sale_items (sale_id, product_id, quantity, unit_price, quantity_delta)
- Create migration file in `packages/db/migrations.ts` with toVersion 2 steps adding sync_status to sales and quantity_delta to sale_items
- Create `ProductModel`, `SaleModel`, `SaleItemModel` extending WatermelonDB Model with field decorators
- Create `database.ts` that initializes WatermelonDB with SQLiteAdapter, schema, and migrations

### UI Layout System (Merchant App)
- Create root `_layout.tsx` that wraps app in: QueryClientProvider, Supabase session listener, NetInfo connectivity listener, i18n provider
- Create bottom tab navigator in `(tabs)/_layout.tsx` with tabs: Dashboard, Inventory, POS (center, larger), Orders, More
- Create `OfflineBanner` component that shows amber banner when NetInfo.isConnected is false
- Configure NativeWind v4 with Tailwind config and design tokens

### UI Layout System (Consumer App)
- Create root `_layout.tsx` with QueryClientProvider and Supabase session listener
- Create bottom tab navigator with tabs: Home, Search, Cart (with badge), Orders, Profile

### UI Layout System (Admin Web)
- Create `(dashboard)/layout.tsx` with left sidebar (240px), top header, main content area
- Create sidebar navigation with links: Overview, Merchants, Markets, Orders, Payments, Settings
- Configure NextAuth.js with credentials provider for admin email + password login

# 3. FEATURE-BY-FEATURE TASK BREAKDOWN

## FEATURE: AUTHENTICATION

### Backend Tasks

1. Create `POST /api/v1/auth/register-fcm` endpoint in `auth.controller.ts` that accepts `{ expoPushToken: string }`, verifies Supabase JWT, updates `Merchant.fcmToken` or `Consumer.fcmToken` in DB
2. Create `POST /api/v1/admin/auth/login` endpoint that accepts `{ email, password }`, finds Admin by email, calls `bcrypt.compare`, issues JWT signed with `JWT_ADMIN_SECRET` with 1hr expiry, returns `{ accessToken }`
3. Create `AdminRefreshToken` model and cleanup cron job that deletes expired tokens daily
4. Create `RegisterFcmDTO` with `@IsString() @IsNotEmpty() expoPushToken`
5. Create `AdminLoginDTO` with `@IsEmail() email` and `@IsString() @MinLength(8) password`

### Frontend Tasks (Merchant App)

1. Build `app/(auth)/login.tsx`: phone input with +234 prefix, "Send Code" button that calls `supabase.auth.signInWithOtp({ phone })`, loading state on button, error toast on failure
2. Build `app/(auth)/verify-otp.tsx`: 6-box OTP input component with auto-advance, auto-submit on 6th digit, paste support, "Resend" button with 60s countdown, calls `supabase.auth.verifyOtp({ phone, token, type: 'sms' })`, on success stores session and navigates to dashboard or onboarding
3. Build `PhoneInput.tsx` component with +234 prefix chip, numeric keyboard, validates 10-digit format
4. Build `OtpInput.tsx` component with 6 individual TextInput boxes, auto-focus management, backspace handling
5. On successful OTP verification: check if merchant profile exists (merchantId in JWT), if not navigate to onboarding, if yes navigate to `(tabs)/dashboard`
6. Call `POST /api/v1/auth/register-fcm` after login to store Expo push token
7. Create `useAuth.ts` hook with `sendOtp(phone)` and `verifyOtp(phone, token)` mutations

### Frontend Tasks (Consumer App)

1. Build `app/(auth)/login.tsx` with same phone OTP flow as merchant
2. Build `app/(auth)/verify-otp.tsx` with same OTP verification
3. On success: if new user (no name), navigate to name input screen, then Home

### Frontend Tasks (Admin Web)

1. Build `app/(auth)/login/page.tsx` with email + password form, calls NextAuth `signIn('credentials', { email, password })`, redirects to dashboard on success
2. Configure NextAuth credentials provider to call `POST /api/v1/admin/auth/login` and return session

### Integration Notes

- `supabase.auth.signInWithOtp` triggers Edge Function which calls Termii
- `supabase.auth.verifyOtp` returns session with JWT containing `{ sub, role, merchantId, plan }`
- JWT is stored in MMKV via authStore, attached to every API request via Axios interceptor
- `POST /api/v1/auth/register-fcm` requires valid Supabase JWT in Authorization header

## FEATURE: MERCHANT ONBOARDING

### Backend Tasks

1. Create `GET /api/v1/merchants/me` that returns authenticated merchant's profile
2. Create `PATCH /api/v1/merchants/me` that accepts `UpdateMerchantDTO` with optional fields: name, shopName, shopDesc, logoUrl, marketId
3. Create `UpdateMerchantDTO` with all optional fields and validation decorators
4. Create `GET /api/v1/merchants/me/stats` that returns `{ totalSales, totalRevenue, totalProducts, totalCustomers }` for the authenticated merchant

### Frontend Tasks (Merchant App)

1. Build 3-step onboarding flow shown only on first login:
   - Step 1: Full name input, "Continue" button
   - Step 2: Shop name (required), description (optional), market selector (searchable dropdown from storefront markets API), "Continue" button
   - Step 3: Shop logo upload (optional, circular crop preview), "Skip for now" link, "Finish Setup" button
2. On "Finish Setup": call `PATCH /api/v1/merchants/me` with collected data, navigate to dashboard with welcome toast
3. Build `ShopProfile` screen under More tab: editable form with shop name, description, logo upload, "View your online shop" link
4. Create `useMerchant.ts` hook with `getMerchant()` query and `updateMerchant()` mutation

### Integration Notes

- `PATCH /api/v1/merchants/me` body: `{ name?, shopName?, shopDesc?, logoUrl?, marketId? }`
- Logo upload: upload to Supabase Storage first, get URL, then include in PATCH body
- Market list comes from `GET /api/v1/storefront/markets`

## FEATURE: INVENTORY MANAGEMENT

### Backend Tasks

1. Create `GET /api/v1/inventory/products` with query params: page, limit, category, search, lowStock (boolean), isActive; returns paginated list scoped to merchantId
2. Create `POST /api/v1/inventory/products` that: checks plan limit (FREE: 50 active products), creates product with merchantId from JWT, writes OutboxEvent `product.created` in same transaction
3. Create `GET /api/v1/inventory/products/:id` with ownership check (product.merchantId must equal JWT merchantId)
4. Create `PATCH /api/v1/inventory/products/:id` that does partial update and invalidates storefront cache keys for this merchant
5. Create `DELETE /api/v1/inventory/products/:id` that soft-deletes (isActive = false), rejects if product has active orders
6. Create `POST /api/v1/inventory/products/bulk-import` that: accepts multipart/form-data CSV file (max 500 rows, 5MB), parses with papaparse, validates each row against CreateProductDTO, batch upserts on SKU conflict, returns `{ imported, updated, failed, errors[] }`
7. Create `GET /api/v1/inventory/products/sku/:sku` that returns exact SKU match within merchant's products
8. Create `GET /api/v1/inventory/import-jobs/:jobId` that returns import job status
9. Create `CreateProductDTO` with: `@IsString() @MinLength(2) name`, `@IsDecimal() price`, `@IsInt() @Min(0) quantity`, `@IsInt() @Min(0) lowStockAt`, `@IsString() @IsOptional() sku`, `@IsString() @IsOptional() category`, `@IsString() @IsOptional() description`, `@IsUrl() @IsOptional() imageUrl`
10. Create `UpdateProductDTO` with all fields optional
11. Create `enforceProductLimit(merchantId)` method in InventoryService that throws 402 PLAN_LIMIT_EXCEEDED when FREE plan merchant has 50+ active products
12. After stock decrement, check if `product.quantity <= product.lowStockAt` and write OutboxEvent `inventory.low_stock` in same transaction

### Frontend Tasks (Merchant App)

1. Build `app/(tabs)/inventory/index.tsx`: FlashList of ProductCard rows, search bar (expands on tap), filter chips (All, Low Stock, Out of Stock, category chips), plan limit banner when >80% used, pull-to-refresh, empty state with "Add Product" CTA
2. Build `app/(tabs)/inventory/add.tsx`: form with photo picker (camera/gallery), name, category select, description, price (₦ prefix), quantity, low stock threshold, SKU with scan button; "Save Product" sticky footer button
3. Build `app/(tabs)/inventory/[id].tsx`: pre-filled edit form identical to add form, "Delete Product" destructive button at bottom with confirmation dialog
4. Build `ProductCard.tsx` component: product image (48x48), name + SKU, price (mono), stock badge (green/amber/red), swipe-left reveals Edit and Delete actions
5. Build `LowStockBadge.tsx` component: shows "Low (qty)" in amber or "Out of Stock" in red
6. Build `BulkImportSheet.tsx` bottom sheet: file upload area, "Download CSV Template" link, preview table of first 5 rows, import button, results summary
7. Build barcode scanner modal `app/(modals)/scan.tsx` using expo-barcode-scanner, on scan calls `GET /api/v1/inventory/products/sku/:sku`, adds product to context (inventory form or POS cart)
8. Create `useProducts.ts` with queries: `getProducts(filters)`, `getProduct(id)`, `getProductBySku(sku)`; mutations: `createProduct()`, `updateProduct()`, `deleteProduct()`, `bulkImport()`
9. Create `ProductModel.ts` WatermelonDB model with all product fields
10. On product create/update/delete: invalidate TanStack Query cache `['products', merchantId]`

### Integration Notes

- `POST /api/v1/inventory/products` body: `{ name, price, quantity, lowStockAt, sku?, category?, description?, imageUrl? }`
- Response: `{ success: true, data: Product }`
- `POST /api/v1/inventory/products/bulk-import` uses multipart/form-data with field name `file`
- Barcode scan in POS: `GET /api/v1/inventory/products/sku/:sku` returns product or 404

## FEATURE: POS SYSTEM

### Backend Tasks

1. Create `POST /api/v1/pos/sales` that runs Prisma transaction: verify each product belongs to merchant, check stock, create Sale + SaleItems, decrement product.quantity per item, upsert Customer if phone provided (update totalSpent, visitCount, lastSeenAt), write OutboxEvent `sale.created`, return created sale with items
2. Create `POST /api/v1/pos/sales/sync` with `SyncRateLimitGuard` (10/min per merchant) that: accepts `{ sales: CreateSaleDTO[] }` (max 100 items via `@ArrayMaxSize(100)`), processes each sale with idempotency check on localId, calls `processSaleDelta` for each new sale, returns `{ results: SyncSaleResult[], summary }`
3. Create `processSaleDelta(merchantId, dto)` method that runs Serializable transaction with SELECT FOR UPDATE, applies quantityDelta atomically, increments product.revision, classifies conflicts as SOFT/HARD/PHANTOM, returns SyncSaleResult with stockUpdates
4. Create `GET /api/v1/pos/sales` with query params: from, to, page, limit, paymentMethod; returns paginated sales with items
5. Create `GET /api/v1/pos/sales/summary` with query param `period` (today/week/month/custom): reads from DailySalesSummary read model, returns totalRevenue, totalSales, avgSaleValue, paymentBreakdown; calls `getTopProducts()` separately with 5-min Redis cache
6. Create `GET /api/v1/sync/pull` that returns product changes since lastPulledAt: created, updated, deleted arrays with WatermelonDB-compatible format
7. Create `POST /api/v1/pos/sales/:localId/reconcile` that accepts `{ action: 'FORCE_SYNC' | 'VOID', voidItemIds? }` for rejected offline sales
8. Create `CreateSaleDTO` with: `@IsUUID() localId`, `@IsEnum(PaymentMethod) paymentMethod`, `@IsArray() @ValidateNested items: SaleItemDTO[]`, optional customerPhone, customerName, createdAt
9. Create `SaleItemDTO` with: `@IsString() productId`, `@IsInt() quantity`, `@IsInt() quantityDelta`, `@IsDecimal() unitPrice`, `@IsInt() productRevision`
10. Create `SyncSalesDTO` with `@IsArray() @ArrayMaxSize(100) sales: CreateSaleDTO[]`
11. Update DailySalesSummary in OutboxRelayService when processing `sale.created` event using UPSERT with atomic increment

### Frontend Tasks (Merchant App)

1. Build `app/(tabs)/pos/index.tsx`: 2-column product grid using FlashList, category filter chips, search bar, offline indicator dot in top bar, sticky cart summary bar at bottom
2. Build `CartSheet.tsx` bottom sheet (70% height): list of CartItem rows with quantity steppers, payment method chips (Cash/Transfer/USSD/Card), optional customer phone input, "Complete Sale" primary button (56px height)
3. Build `CartItem.tsx`: product name, unit price, quantity stepper (−/qty/+), line total, remove (x) button
4. Build `PaymentMethodPicker.tsx`: horizontal chip row, single-tap selection
5. Build `NumericKeypad.tsx`: custom numeric keypad for quantity entry on long-press
6. Build `ReceiptView.tsx` modal: animated green checkmark, total amount, item list, receipt number, "Share via WhatsApp" button, "New Sale" primary button
7. On product tile tap: add to cartStore with haptic feedback, scale animation via Reanimated
8. On "Complete Sale": if online call `POST /api/v1/pos/sales`, if offline save to WatermelonDB with syncedAt null, show receipt from local data
9. Create `cartStore.ts` Zustand store: `items`, `total`, `addItem(product)`, `removeItem(productId)`, `updateQuantity(productId, qty)`, `clearCart()`
10. Create `SaleModel.ts` WatermelonDB model
11. Create `usePOS.ts` hook with `createSale()` mutation that handles both online and offline paths
12. Implement auto-sync in `packages/db/sync.ts`: listen to NetInfo, on reconnect call `syncDatabase()` which pushes unsynced sales with quantityDelta values, updates local records from stockUpdates response
13. Show sync status icon in top bar: cloud-check (synced), cloud-up (syncing), cloud-x (failed)
14. Build `SyncIssues` screen showing PARTIAL/REJECTED sales with FORCE_SYNC and VOID actions per item

### Integration Notes

- Online sale: `POST /api/v1/pos/sales` body: `{ localId, paymentMethod, items: [{ productId, quantity, quantityDelta, unitPrice, productRevision }], customerPhone? }`
- Offline sale: saved to WatermelonDB, synced via `POST /api/v1/pos/sales/sync`
- Sync pull: `GET /api/v1/sync/pull?lastPulledAt=<timestamp>` returns `{ changes: { products: { created, updated, deleted } }, timestamp }`
- After sync: update WatermelonDB product quantities from `stockUpdates` in response

## FEATURE: ORDERS

### Backend Tasks

1. Create `POST /api/v1/orders` that: validates all products exist and belong to merchantId, checks stock, runs Prisma transaction (create Order PENDING, create OrderItems with unitPrice snapshot, reserve stock), calls PaystackService.initializeTransaction, updates order.paystackRef, writes OutboxEvent `order.created`, returns `{ orderId, authorizationUrl, reference }`
2. Create `GET /api/v1/orders` that returns: for Consumer JWT - own orders; for Merchant JWT - incoming orders; with query params status, page, limit, from, to
3. Create `GET /api/v1/orders/:id` with ownership check (consumer sees own, merchant sees theirs)
4. Create `PATCH /api/v1/orders/:id/status` that: validates transition is allowed for role using ALLOWED_TRANSITIONS map, updates status, if CANCELLED restores reserved stock in transaction, writes OutboxEvent `order.status.changed`
5. Implement ALLOWED_TRANSITIONS: PENDING->[CONFIRMED,CANCELLED], CONFIRMED->[PROCESSING,CANCELLED], PROCESSING->[SHIPPED], SHIPPED->[DELIVERED], DELIVERED->[], CANCELLED->[]
6. Implement role-based transition rules: merchant can do CONFIRMED/PROCESSING/SHIPPED/DELIVERED/CANCELLED; consumer can only do CANCELLED from PENDING
7. Create `CreateOrderDTO` with: `@IsUUID() idempotencyKey`, `@IsString() merchantId`, `@IsArray() items: OrderItemDTO[]`, `@IsString() deliveryAddress`, `@IsString() @IsOptional() deliveryNote`
8. Create `UpdateOrderStatusDTO` with `@IsEnum(OrderStatus) status` and `@IsString() @IsOptional() cancelReason`
9. Add idempotency check in createOrder: `findFirst({ where: { consumerId, idempotencyKey } })`, return existing if found
10. Create cron job `cancelAbandonedOrders()` that runs hourly, cancels PENDING orders older than 2 hours with no paystackRef, restores stock in transaction

### Frontend Tasks (Merchant App)

1. Build `app/(tabs)/orders/index.tsx`: filter tabs (All/Pending/Confirmed/Processing/Shipped/Delivered/Cancelled) with badge count on Pending, FlashList of OrderCard rows, swipe-right on Pending reveals quick Confirm action
2. Build `app/(tabs)/orders/[id].tsx`: status timeline stepper, customer info card, order items list, payment card, context-sensitive action button (Confirm/Processing/Shipped/Delivered based on current status), Cancel button for PENDING and CONFIRMED
3. Build `OrderCard.tsx`: order ID, customer name, item count, total, status badge, time ago
4. Build `StatusBadge.tsx`: color-coded pill for each order status
5. On FCM push notification for new order: deep link to orders list, refetch orders query
6. Create `useOrders.ts` hook with `getOrders(filters)` query and `updateOrderStatus(id, status)` mutation

### Frontend Tasks (Consumer App)

1. Build `app/(tabs)/orders.tsx`: filter tabs (Active/Completed/Cancelled), list of OrderCard rows
2. Build `app/(stack)/orders/[id].tsx`: animated status timeline with pulsing dot on current step, shop info, items list, delivery address, payment summary, Cancel button (PENDING only)
3. Subscribe to Supabase Realtime on `Order` table filtered by `id=eq.${orderId}` in checkout screen, navigate to confirmation when status becomes CONFIRMED
4. Create `useOrders.ts` and `useCreateOrder.ts` hooks

### Integration Notes

- `POST /api/v1/orders` returns `{ orderId, authorizationUrl, reference }` - open authorizationUrl in Paystack WebView
- `PATCH /api/v1/orders/:id/status` body: `{ status: 'CONFIRMED' | 'PROCESSING' | 'SHIPPED' | 'DELIVERED' | 'CANCELLED', cancelReason? }`
- Consumer generates `idempotencyKey = uuid()` on checkout screen mount, includes in POST body

## FEATURE: PAYMENTS

### Backend Tasks

1. Create `POST /api/v1/payments/initialize` that: verifies order belongs to consumer, verifies order is PENDING, calls `PaystackService.initializeTransaction({ email, amount: total*100, reference, metadata: { orderId } })`, returns `{ authorizationUrl, reference, accessCode }`
2. Create `POST /api/v1/payments/webhook` as `@Public()` endpoint that: captures rawBody BEFORE JSON parsing using `express.raw({ type: 'application/json' })` in main.ts, verifies HMAC-SHA512 signature, checks replay attack (reject if event timestamp > 5 minutes old), checks idempotency (findUnique by paystackRef), processes `charge.success` and `charge.failed` events
3. In `handleChargeSuccess`: run Prisma transaction that updates Order to CONFIRMED, creates Transaction record, writes OutboxEvent `order.confirmed`
4. In `handleChargeFailed`: run Prisma transaction that cancels Order, restores reserved stock, writes OutboxEvent `order.cancelled`
5. Create `PaystackWebhookDTO` with `@IsString() event` and `@IsObject() data`

### Frontend Tasks (Consumer App)

1. Build `app/(stack)/checkout/payment.tsx`: embed Paystack WebView with `authorizationUrl`, handle redirect on payment completion, close WebView
2. On WebView close: check order status via Supabase Realtime subscription, navigate to confirmation if CONFIRMED or show error if FAILED
3. Build `app/(stack)/checkout/confirmed.tsx`: success animation, order ID, "Track Order" button, "Continue Shopping" link

### Integration Notes

- `POST /api/v1/payments/initialize` requires Consumer JWT
- Webhook is public but HMAC-verified - always return 200 to prevent Paystack retries
- Supabase Realtime subscription on Order table detects CONFIRMED status change instantly

## FEATURE: CUSTOMER CRM

### Backend Tasks

1. Customer records are created/updated automatically during POS sales: upsert by `[merchantId, phone]`, increment totalSpent and visitCount, update lastSeenAt
2. Create `GET /api/v1/customers` (implied from merchant profile) - query customers scoped to merchantId with search by name/phone, pagination
3. Create `GET /api/v1/customers/:id` with ownership check

### Frontend Tasks (Merchant App)

1. Build Customer List screen under More tab: search by name or phone, FlashList of CustomerCard rows (avatar with initials, name, phone, total spent, visit count, last seen)
2. Build Customer Profile screen: large avatar, stats row (total spent, visits, last visit), purchase history list
3. Create `useCustomers.ts` hook with `getCustomers(search)` query and `getCustomer(id)` query

### Integration Notes

- Customers are auto-created during POS sales when cashier enters customer phone
- No manual customer creation endpoint needed for MVP

## FEATURE: ANALYTICS

### Backend Tasks

1. `GET /api/v1/pos/sales/summary` reads from DailySalesSummary (pre-aggregated) for totalRevenue, totalSales, avgSaleValue, paymentBreakdown
2. Create `getTopProducts(merchantId, from, to)` method that queries SaleItem JOIN Sale JOIN Product, groups by productId, orders by unitsSold DESC, limits to 10, caches result in Redis with key `m:{merchantId}:top-products:{from}:{to}` for 5 minutes
3. DailySalesSummary is updated by OutboxRelayService on `sale.created` event using UPSERT with atomic increment per payment method column

### Frontend Tasks (Merchant App)

1. Build Analytics screen under More tab: period selector (Today/Week/Month/Custom), revenue bar chart using Recharts-compatible library, 4 metric cards (total revenue, sales count, avg sale, top day), payment breakdown donut chart, top products table, sales history list
2. Build `PeriodSelector.tsx` segmented control component
3. On custom period: show date range picker modal
4. Create `useDashboard.ts` hook with `getSalesSummary(period, from?, to?)` query

### Integration Notes

- `GET /api/v1/pos/sales/summary?period=today` returns `{ totalRevenue, totalSales, avgSaleValue, topProducts, paymentBreakdown, dailyRevenue }`
- Charts use `dailyRevenue: [{ date, revenue, count }]` array

## FEATURE: DASHBOARD

### Backend Tasks

No dedicated dashboard endpoint. Dashboard aggregates data from existing endpoints.

### Frontend Tasks (Merchant App)

1. Build `app/(tabs)/dashboard.tsx`: offline banner (conditional), 4 metric cards (today's revenue, sales count, avg sale, pending orders), 2x2 quick actions grid (New Sale, Add Product, View Orders, View Reports), low stock alerts section (up to 3 products), recent sales list (last 5), pending orders section (up to 3)
2. Build `SalesSummaryCard.tsx` metric card with label, value, trend indicator
3. Build `LowStockAlert.tsx` component showing product name, current qty, threshold
4. Build `RecentSalesList.tsx` showing time, items count, total, payment method badge
5. Create `useDashboard.ts` hook that calls `getSalesSummary('today')` and `getProducts({ lowStock: true, limit: 3 })`
6. Pull-to-refresh refreshes all dashboard data

## FEATURE: STOREFRONT (PUBLIC MARKETPLACE)

### Backend Tasks

1. Create `GET /api/v1/storefront/markets` with Redis cache TTL 1hr, returns `[{ id, name, city, state, shopCount, isActive }]`
2. Create `GET /api/v1/storefront/markets/:marketId/shops` with Redis cache TTL 15min, query params page/limit/category
3. Create `GET /api/v1/storefront/shops/:shopId` with Redis cache TTL 15min
4. Create `GET /api/v1/storefront/shops/:shopId/products` with Redis cache TTL 2min, query params page/limit/category/search/minPrice/maxPrice
5. Create `GET /api/v1/storefront/products/search` with Redis cache TTL 60s keyed by query hash, uses pre-computed `search_vector` column with GIN index
6. Create `GET /api/v1/storefront/products/featured` with Redis cache TTL 30min
7. Implement `invalidateShopCache(merchantId)` using Redis SCAN (not KEYS) to delete `storefront:shop:{merchantId}:*` keys
8. Implement global search cache invalidation using `sf:search:invalidate_at` timestamp key: set on any product change, check on search query to determine if cached result is stale
9. All storefront endpoints are `@Public()` - no auth required
10. Cache key format: `storefront:markets`, `storefront:market:{id}:shops:{page}`, `storefront:shop:{shopId}`, `storefront:shop:{shopId}:products:{page}:{hash}`, `storefront:search:{queryHash}`, `storefront:featured`

### Frontend Tasks (Consumer App)

1. Build `app/(tabs)/home.tsx`: search bar (navigates to search on tap), location banner with city name, featured markets horizontal scroll, trending products horizontal scroll, nearby shops vertical list
2. Build `app/(tabs)/search.tsx`: auto-focused search input, filter chips (category, price range, market), results toggle (Products/Shops), empty state, recent searches list
3. Build `app/(stack)/shop/[shopId]/index.tsx`: shop header (logo, name, market, description), product filter bar, 2-column product grid
4. Build `app/(stack)/shop/[shopId]/product/[id].tsx`: product image, name, price, shop name link, description, quantity stepper, "Add to Cart" sticky footer
5. Build `app/(tabs)/cart.tsx`: cart items list with quantity steppers, order summary, "Proceed to Checkout" button, empty state
6. Build `app/(stack)/checkout/index.tsx`: delivery address textarea, delivery note, order summary collapsible, "Continue to Payment" button
7. Build `ShopCard.tsx`, `ProductGrid.tsx`, `MarketBrowser.tsx`, `SearchBar.tsx`, `CartItemRow.tsx`, `OrderTracker.tsx` components
8. Create `useMarkets.ts`, `useShop.ts`, `useProductSearch.ts` hooks
9. Create `cartStore.ts` Zustand store for consumer cart: items, quantities, total, addItem, removeItem, updateQuantity, clearCart

### Integration Notes

- All storefront endpoints are public, no auth header needed
- `GET /api/v1/storefront/products/search?q=ankara&category=Textiles&market=marketId&page=1&limit=20`
- Cart is local state (Zustand), order is created on checkout

## FEATURE: SETTINGS

### Backend Tasks

1. `PATCH /api/v1/merchants/me` handles shop profile updates (name, shopName, shopDesc, logoUrl, marketId)
2. Staff management: create `POST /api/v1/merchants/staff` that creates a Cashier-role merchant account linked to owner's shop (Post-MVP detail, stub for MVP)
3. Subscription plan display: `GET /api/v1/merchants/me` returns current plan and planExpiresAt

### Frontend Tasks (Merchant App)

1. Build More Menu screen: list items for Shop Profile, Staff Management, Analytics, Customers, Subscription Plan, Language toggle, Help & Support, Logout
2. Build Shop Profile screen: editable form, logo upload, "View online shop" external link
3. Build Staff Management screen: list of staff with role badges, "Add Staff" button that opens bottom sheet with phone + name inputs
4. Build Subscription Plan screen: current plan card with usage progress bar, plan comparison table, "Upgrade" CTA
5. Language toggle: calls `i18n.changeLanguage('en' | 'ha')`, persists to MMKV

## FEATURE: ADMIN DASHBOARD

### Backend Tasks

1. Create `GET /api/v1/admin/merchants` with query params: page, limit, plan, isActive, search, marketId; requires Admin JWT
2. Create `PATCH /api/v1/admin/merchants/:id/plan` that updates plan and planExpiresAt, writes OutboxEvent `merchant.plan.changed`
3. Create `PATCH /api/v1/admin/merchants/:id/status` that updates isActive with reason
4. Create `GET /api/v1/admin/markets`, `POST /api/v1/admin/markets`, `PATCH /api/v1/admin/markets/:id`, `DELETE /api/v1/admin/markets/:id`
5. Create `GET /api/v1/admin/analytics/overview` that returns: totalMerchants, activeMerchants, merchantsByPlan, totalOrders, totalRevenue, totalTransactions, newMerchantsThisMonth, ordersThisMonth
6. Create `GET /api/v1/admin/orders` with query params: status, merchantId, consumerId, from, to, page, limit
7. Create `GET /api/v1/admin/outbox/failed` that returns FAILED OutboxEvents grouped by eventType
8. Create `POST /api/v1/admin/outbox/replay` that resets FAILED events matching filters back to PENDING
9. All admin endpoints require Admin JWT with appropriate adminRole (VIEWER/OPERATOR/SUPER)
10. Create `CreateMarketDTO` with: `@IsString() name`, `@IsString() city`, `@IsString() state`, optional description, latitude, longitude, imageUrl

### Frontend Tasks (Admin Web)

1. Build `app/(dashboard)/page.tsx` (Overview): 4 metric cards, merchants-by-plan donut chart, orders-over-time line chart, recent activity feed
2. Build `app/(dashboard)/merchants/page.tsx`: data table with search, plan filter, status filter, market filter, export CSV button; columns: Shop Name, Owner, Phone, Market, Plan, Status, Joined, Actions
3. Build `app/(dashboard)/merchants/[id]/page.tsx`: merchant profile card, plan management card (dropdown + date picker + Update button), account status toggle, recent orders table, recent sales table
4. Build `app/(dashboard)/markets/page.tsx`: data table, "Register Market" button
5. Build `app/(dashboard)/markets/new/page.tsx`: form with name, city, state, description, lat/lng, image upload
6. Build `app/(dashboard)/orders/page.tsx`: data table with status/merchant/consumer/date filters
7. Build `app/(dashboard)/payments/page.tsx`: transactions table with reference/status/date filters, summary bar
8. Build `app/(dashboard)/settings/page.tsx`: tabbed settings (General, Admin Accounts, Notifications)
9. All admin pages use TanStack Table v8 for data tables with sorting, pagination
10. Charts use Recharts

# 4. CROSS-CUTTING FEATURES

## OFFLINE SUPPORT

### Backend Tasks

1. `POST /api/v1/pos/sales/sync` processes batch of offline sales with idempotency on localId
2. `GET /api/v1/sync/pull?lastPulledAt=<ms>` returns product changes since timestamp in WatermelonDB-compatible format: `{ changes: { products: { created[], updated[], deleted[] } }, timestamp }`
3. `processSaleDelta` uses Serializable isolation level and SELECT FOR UPDATE to prevent race conditions
4. Sync endpoint has dedicated `SyncRateLimitGuard` (10 req/min per merchant)

### Frontend Tasks

1. `packages/db/sync.ts`: `syncDatabase()` function that calls WatermelonDB `synchronize()` with pullChanges (GET /sync/pull) and pushChanges (POST /pos/sales/sync with quantityDelta values)
2. Auto-trigger `syncDatabase()` when NetInfo.isConnected changes to true
3. Show "Offline Mode" amber banner when isConnected is false
4. Show sync status icon: cloud-check / cloud-up (animated) / cloud-x
5. After sync: update WatermelonDB product quantities from `stockUpdates` in response
6. Show toast "X sales synced successfully" on successful sync
7. Navigate to SyncIssues screen if any sales have status PARTIAL or REJECTED

## CACHING

### Backend Tasks

1. Cache all storefront endpoints in Redis with TTLs: markets 1hr, shops 15min, shop products 2min, search 60s, featured 30min
2. Invalidate `storefront:shop:{merchantId}:*` keys using SCAN (not KEYS) when merchant updates profile or products
3. Set `sf:search:invalidate_at` timestamp on any product change; check on search queries to determine cache staleness
4. Cache `topProducts` query result in Redis with key `m:{merchantId}:top-products:{from}:{to}` for 5 minutes

### Frontend Tasks

1. TanStack Query staleTime: products 5min, storefront markets 1hr, storefront shops 15min
2. Invalidate `['products', merchantId]` after create/update/delete product
3. Invalidate `['orders']` after order status update
4. Invalidate `['sales']` and `['products']` after POS sale

## NOTIFICATIONS

### Backend Tasks

1. OutboxRelayService dispatches to NotificationsService based on eventType:
   - `order.confirmed`: FCM to merchant (new order) and consumer (payment confirmed)
   - `order.status.changed`: FCM to consumer for SHIPPED/DELIVERED/CANCELLED; FCM to merchant for CANCELLED
   - `inventory.low_stock`: FCM to merchant with product name and quantity
   - `sale.created`: check low stock threshold, write `inventory.low_stock` OutboxEvent if triggered
   - `merchant.plan.changed`: FCM to merchant confirming plan change
2. FCM `send(token, title, body, messageId)` uses `messageId = event.id` as collapse key for deduplication
3. FCM circuit breaker: open after 10 consecutive failures, reset after 60s
4. Daily summary cron `@Cron('0 23 * * *', { timeZone: 'Africa/Lagos' })`: cursor-based batch processing of all active merchants with fcmToken, sends daily sales summary push

### Frontend Tasks

1. Request push notification permissions on app launch using Expo Notifications
2. Register Expo push token after login: call `POST /api/v1/auth/register-fcm` with token
3. Handle incoming push notifications: tap on "New Order" deep links to orders list, tap on "Low Stock" deep links to inventory filtered to low stock
4. Display in-app notification toasts for received push notifications when app is foregrounded

# 5. BACKGROUND JOBS & EVENTS

### Backend Tasks

1. OutboxRelayService cron every 1 second: acquire Redis lock, reset stuck PROCESSING events, fetch PENDING events by priority, dispatch critical sequentially and normal in parallel
2. Daily summary cron at 23:00 WAT: cursor-based batch of merchants, send FCM push with today's sales summary
3. Abandoned orders cron every hour: cancel PENDING orders older than 2 hours with no paystackRef, restore stock in transaction
4. Monthly archival cron on 1st of month at 02:00 WAT: cursor-based batch export of sales older than 2 years to Supabase Storage as JSONL, delete from hot DB
5. All cron jobs use Redis distributed lock with NX to prevent double-execution on scale-out
6. RETRY_POLICY per event type: order.confirmed (20 attempts, 5s base), order.status.changed (20, 5s), sale.created (10, 10s), inventory.low_stock (5, 30s), merchant.plan.changed (10, 10s)
7. Dead-letter: after maxAttempts, mark FAILED, send Sentry alert immediately

# 6. ERROR HANDLING & EDGE CASES

### Backend Tasks

1. GlobalExceptionFilter maps all errors to `{ success: false, error: { code, message, details }, timestamp, path }`
2. Prisma P2002 -> 409 DUPLICATE_ENTRY, P2025 -> 404 NOT_FOUND, P2003 -> 400 FOREIGN_KEY_VIOLATION
3. ValidationPipe returns 400 VALIDATION_ERROR with `details` array of field errors
4. Paystack webhook: return 200 even on processing errors to prevent Paystack retries
5. Sync endpoint: return per-sale status (SYNCED/PARTIAL/REJECTED/DUPLICATE) never throw 500 for individual sale failures
6. Plan limit: throw 402 PLAN_LIMIT_EXCEEDED with upgrade message
7. Insufficient stock: throw 400 INSUFFICIENT_STOCK with product name and available quantity
8. Ownership violation: throw 403 FORBIDDEN

### Frontend Tasks

1. Axios response interceptor: show error toast for 4xx/5xx responses using `error.code` and `error.message`
2. TanStack Query `onError` callbacks: show specific error messages per mutation
3. Network failure: show "Connection failed. Check your internet and try again." toast with retry button
4. Offline mode: block checkout and order status updates, show "Internet required" message
5. OTP wrong: show "Incorrect code. X attempts remaining." with attempt count
6. OTP expired: show "Code expired. Tap Resend." and enable resend button
7. Session expired: clear authStore, navigate to login with "Your session has expired" toast
8. Plan limit reached: disable "Save Product" button, show upgrade banner
9. Insufficient stock in POS: quantity stepper stops at max available, show toast
10. Sync conflict: show SyncIssues screen with FORCE_SYNC / VOID options per rejected sale

# 7. TESTING TASKS

### Backend Tasks

1. `inventory.service.spec.ts`: test `enforceProductLimit` throws 402 at 50 products on FREE plan; test `createProduct` increments count; test tenant isolation (merchantB cannot see merchantA products)
2. `pos.service.spec.ts`: test `processSaleDelta` deducts stock correctly; test REJECTED when stock insufficient; test DUPLICATE when localId already exists; test SYNCED returns correct stockUpdates
3. `orders.service.spec.ts`: test all valid state transitions; test invalid transitions throw 400; test consumer cannot transition to CONFIRMED; test stock restored on CANCELLED
4. `payments.service.spec.ts`: test HMAC signature verification rejects invalid signature; test idempotency (second webhook with same reference is skipped); test order confirmed on charge.success; test stock restored on charge.failed
5. `inventory.service.spec.ts` integration: `POST /inventory/products` with FREE plan at 50 products returns 402
6. `pos.service.spec.ts` integration: `POST /pos/sales/sync` with duplicate localId returns DUPLICATE status
7. `payments.service.spec.ts` integration: `POST /payments/webhook` with invalid HMAC returns 401
8. `outbox-relay.service.spec.ts`: test distributed lock prevents double-processing; test exponential backoff sets correct nextRetryAt; test FAILED after maxAttempts

### Frontend Tasks

1. `ProductCard.test.tsx`: renders product name, price, stock badge; swipe-left reveals edit/delete
2. `OtpInput.test.tsx`: auto-advances on digit entry; handles paste of 6 digits; shows error state
3. `cartStore.test.ts`: addItem increases total; removeItem at qty 0 removes item; clearCart empties store
4. `POS flow test`: add product to cart, complete sale online, verify receipt shown; add product offline, verify saved to WatermelonDB with syncedAt null
5. `Checkout flow test`: add to cart, enter address, open Paystack WebView, simulate payment success, verify navigation to confirmation

# 8. TASK DEPENDENCY ORDER

## Phase 1: Foundation (Week 1-2)

**Backend:**
1. Initialize NestJS project, install all dependencies
2. Write Prisma schema, run initial migration
3. Create PrismaService with tenant middleware
4. Create GlobalExceptionFilter, ResponseTransformInterceptor, LoggingInterceptor
5. Create SupabaseAuthGuard, RolesGuard, @CurrentUser, @Public, @Roles decorators
6. Create RequestContext with AsyncLocalStorage
7. Create OutboxService (write events)
8. Create RedisService wrapper
9. Create RateLimitGuard
10. Deploy Supabase Edge Functions (send-sms-otp, custom-access-token)

**Frontend:**
1. Initialize monorepo with npm workspaces
2. Create apps/merchant, apps/consumer, apps/web with Expo Bare and Next.js
3. Create packages/shared-api, shared-types, shared-utils, db
4. Configure WatermelonDB schema and migrations
5. Create authStore (Zustand + MMKV)
6. Create Axios client with Supabase session interceptors
7. Configure NativeWind, Expo Router, bottom tab navigators
8. Create root layouts with providers

## Phase 2: Authentication + Core Setup (Week 2-3)

**Backend:**
1. Create auth.controller.ts with register-fcm and admin login endpoints
2. Create merchants.controller.ts with GET/PATCH /me
3. Create PaystackService, TermiiService, FirebaseService integrations
4. Create OutboxRelayService with cron and dispatch logic

**Frontend:**
1. Build login.tsx and verify-otp.tsx (both apps)
2. Build merchant onboarding 3-step flow
3. Build admin login page (web)
4. Configure NextAuth for admin web

## Phase 3: Core Features (Week 3-6)

**Backend (in order):**
1. Inventory module: all CRUD endpoints + bulk import + SKU lookup
2. POS module: createSale, syncSales, getSales, getSalesSummary, syncPull
3. Orders module: createOrder, getOrders, getOrder, updateOrderStatus
4. Payments module: initialize, webhook handler
5. Storefront module: all public endpoints with Redis caching
6. Notifications module: FCM and Termii dispatch via OutboxRelayService
7. Admin module: merchant management, market CRUD, analytics overview

**Frontend (in order):**
1. Inventory screens: product list, add, edit, bulk import, barcode scanner
2. POS screen: product grid, cart sheet, receipt, offline sale flow
3. Sync engine: WatermelonDB sync with backend
4. Dashboard screen: metrics, quick actions, alerts
5. Orders screens (merchant): list, detail, status update
6. Consumer storefront: home, search, shop profile, product detail
7. Consumer cart and checkout flow
8. Consumer orders: history, detail, tracking
9. Analytics screen
10. Admin web: all pages

## Phase 4: Cross-Cutting + Polish (Week 6-8)

**Backend:**
1. Cron jobs: daily summary, abandoned orders, archival
2. RLS policies migration
3. DailySalesSummary UPSERT in OutboxRelayService
4. Metrics endpoint (/metrics Prometheus format)
5. Deep health check (/health with all dependencies)

**Frontend:**
1. Offline banner and sync status indicator
2. Push notification handling and deep links
3. Error states for all screens
4. Settings screens: shop profile, staff, subscription
5. Customer CRM screens
6. i18n setup with Hausa translations
7. Admin web: outbox dead-letter panel

## Phase 5: Testing + Hardening (Week 8-10)

1. Backend unit tests for all services
2. Backend integration tests for all critical endpoints
3. Frontend component tests
4. Frontend user flow tests
5. Load test: 50 merchants syncing simultaneously
6. Security review: RLS policies, HMAC verification, rate limits
7. EAS Build configuration for Android production
8. CI/CD pipeline: GitHub Actions test + migrate + deploy

# 9. INTEGRATION REFERENCE

## Request/Response Contracts

### POST /api/v1/pos/sales
Request: `{ localId: string, paymentMethod: 'CASH'|'TRANSFER'|'USSD'|'CARD', items: [{ productId, quantity, quantityDelta, unitPrice, productRevision }], customerPhone?, customerName?, createdAt? }`
Response: `{ success: true, data: { id, localId, total, paymentMethod, items, createdAt } }`

### POST /api/v1/pos/sales/sync
Request: `{ sales: CreateSaleDTO[] }` (max 100)
Response: `{ success: true, data: { results: [{ localId, status, reason?, conflictItems?, stockUpdates }], summary: { synced, partial, rejected, duplicate } } }`

### POST /api/v1/orders
Request: `{ idempotencyKey, merchantId, items: [{ productId, quantity }], deliveryAddress, deliveryNote? }`
Response: `{ success: true, data: { orderId, authorizationUrl, reference } }`

### PATCH /api/v1/orders/:id/status
Request: `{ status: OrderStatus, cancelReason? }`
Response: `{ success: true, data: Order }`

### POST /api/v1/payments/webhook
Request: raw JSON body with `x-paystack-signature` header
Response: `{ received: true }` (always 200)

### GET /api/v1/pos/sales/summary
Query: `period=today|week|month|custom&from=ISO&to=ISO`
Response: `{ success: true, data: { totalRevenue, totalSales, avgSaleValue, topProducts, paymentBreakdown, dailyRevenue } }`

### GET /api/v1/sync/pull
Query: `lastPulledAt=<unix_ms>`
Response: `{ changes: { products: { created[], updated[], deleted[] } }, timestamp }`

*Document ready for immediate team assignment. Backend and frontend teams can work in parallel from Phase 3 onwards using the integration contracts above.*
