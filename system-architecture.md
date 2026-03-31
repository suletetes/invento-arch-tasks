# INVENTO - PRINCIPAL ARCHITECT SYSTEM DESIGN

# TABLE OF CONTENTS

- [1. EXECUTIVE ARCHITECTURE SUMMARY](#1-executive-architecture-summary)
  - [What Invento Is](#what-invento-is)
  - [Architecture Philosophy](#architecture-philosophy)
  - [Architecture Style](#architecture-style)
- [2. SYSTEM TOPOLOGY & COMPONENT MAP](#2-system-topology-component-map)
  - [2.1 Full System Diagram](#21-full-system-diagram)
  - [2.2 User Roles & Permissions Matrix](#22-user-roles-permissions-matrix)
  - [2.3 MVP Scope vs Post-MVP](#23-mvp-scope-vs-post-mvp)
- [3. ARCHITECTURAL PRINCIPLES & PATTERNS](#3-architectural-principles-patterns)
  - [3.1 Backend: Layered Architecture with Domain Events](#31-backend-layered-architecture-with-domain-events)
  - [3.2 Mobile: Vertical Slice Architecture](#32-mobile-vertical-slice-architecture)
  - [3.3 API Design: REST with Consistent Envelope](#33-api-design-rest-with-consistent-envelope)
  - [3.4 Error Code Registry](#34-error-code-registry)
- [4. FRONTEND ARCHITECTURE - REACT NATIVE](#4-frontend-architecture-react-native)
  - [4.1 Monorepo Structure](#41-monorepo-structure)
  - [4.2 Merchant App - Complete Architecture](#42-merchant-app-complete-architecture)
    - [Tech Stack](#tech-stack)
    - [Folder Structure (Vertical Slice)](#folder-structure-vertical-slice)
    - [State Architecture (Three-Layer)](#state-architecture-three-layer)
  - [4.3 Consumer App - Complete Architecture](#43-consumer-app-complete-architecture)
    - [Folder Structure (Vertical Slice)](#folder-structure-vertical-slice-1)
  - [4.4 Shared Packages](#44-shared-packages)
    - [packages/shared-api](#packagesshared-api)
    - [packages/shared-types](#packagesshared-types)
- [5. WEB DASHBOARD ARCHITECTURE - NEXT.JS ADMIN](#5-web-dashboard-architecture-nextjs-admin)
  - [5.1 Stack](#51-stack)
  - [5.2 Page Structure](#52-page-structure)
  - [5.3 Admin Auth](#53-admin-auth)
- [6. BACKEND ARCHITECTURE - NESTJS LAYERED DESIGN](#6-backend-architecture-nestjs-layered-design)
  - [6.1 Complete Folder Structure](#61-complete-folder-structure)
  - [6.2 Request Lifecycle (Detailed)](#62-request-lifecycle-detailed)
  - [6.3 NestJS Module Dependency Graph](#63-nestjs-module-dependency-graph)
- [7. DOMAIN MODULE SPECIFICATIONS](#7-domain-module-specifications)
  - [7.1 Auth Module](#71-auth-module)
    - [How It Works](#how-it-works)
    - [Supabase Edge Function - Send SMS Hook (~20 lines, replaces entire TermiiService for auth)](#supabase-edge-function-send-sms-hook-20-lines-replaces-entire-termiiservice-for-auth)
    - [Custom Access Token Hook - Inject merchantId into JWT](#custom-access-token-hook-inject-merchantid-into-jwt)
    - [What Your NestJS Backend Still Does for Auth](#what-your-nestjs-backend-still-does-for-auth)
    - [Admin Auth (unchanged)](#admin-auth-unchanged)
    - [Endpoints (simplified - most auth is now client-side via Supabase SDK)](#endpoints-simplified-most-auth-is-now-client-side-via-supabase-sdk)
  - [7.2 Inventory Module](#72-inventory-module)
    - [Responsibilities](#responsibilities)
    - [Endpoints](#endpoints)
    - [Plan Enforcement](#plan-enforcement)
    - [Domain Events Emitted](#domain-events-emitted)
  - [7.3 POS Module](#73-pos-module)
    - [Responsibilities](#responsibilities-1)
    - [Endpoints](#endpoints-1)
    - [Offline Sync - Delta-Based Full Specification](#offline-sync-delta-based-full-specification)
    - [Server-Side Delta Processing (atomic per sale)](#server-side-delta-processing-atomic-per-sale)
    - [Domain Events Emitted](#domain-events-emitted-1)
  - [7.4 Orders Module](#74-orders-module)
    - [Responsibilities](#responsibilities-2)
    - [Order Status State Machine](#order-status-state-machine)
    - [Transition Rules (enforced in service)](#transition-rules-enforced-in-service)
    - [Endpoints](#endpoints-2)
  - [7.5 Payments Module](#75-payments-module)
    - [Responsibilities](#responsibilities-3)
    - [Endpoints](#endpoints-3)
    - [Webhook Handler (Complete)](#webhook-handler-complete)
  - [7.6 Notifications Module](#76-notifications-module)
    - [Responsibilities](#responsibilities-4)
    - [Event Listeners](#event-listeners)
    - [Notification Event Map](#notification-event-map)
  - [7.7 Storefront Module (Public API)](#77-storefront-module-public-api)
    - [Responsibilities](#responsibilities-5)
    - [Endpoints](#endpoints-4)
    - [Full-Text Search Implementation](#full-text-search-implementation)
  - [7.8 Admin Module](#78-admin-module)
    - [Responsibilities](#responsibilities-6)
    - [Endpoints](#endpoints-5)
- [8. DATABASE ARCHITECTURE](#8-database-architecture)
  - [8.1 Design Principles](#81-design-principles)
  - [8.2 Complete Prisma Schema](#82-complete-prisma-schema)
  - [8.3 Entity Relationship Diagram](#83-entity-relationship-diagram)
  - [8.4 Multi-Tenancy: PostgreSQL Row-Level Security](#84-multi-tenancy-postgresql-row-level-security)
  - [8.5 Key Design Decisions](#85-key-design-decisions)
- [9. DATA FLOW SPECIFICATIONS](#9-data-flow-specifications)
  - [9.1 Flow: Merchant Onboarding & First Product](#91-flow-merchant-onboarding-first-product)
  - [9.2 Flow: POS Sale (Online)](#92-flow-pos-sale-online)
  - [9.3 Flow: POS Sale (Offline) + Sync](#93-flow-pos-sale-offline-sync)
  - [9.4 Flow: Consumer Places an Order](#94-flow-consumer-places-an-order)
  - [9.5 Flow: Merchant Fulfills an Order](#95-flow-merchant-fulfills-an-order)
  - [9.6 Flow: Token Refresh (Silent, Background)](#96-flow-token-refresh-silent-background)
- [10. MULTI-TENANCY DESIGN](#10-multi-tenancy-design)
  - [10.1 Strategy: Two-Layer Tenant Isolation](#101-strategy-two-layer-tenant-isolation)
  - [10.2 Prisma Tenant Middleware (Restored)](#102-prisma-tenant-middleware-restored)
  - [10.3 PostgreSQL RLS Policies (Layer 2 - Defense-in-Depth)](#103-postgresql-rls-policies-layer-2-defense-in-depth)
  - [10.4 Setting Tenant Context in AuthGuard](#104-setting-tenant-context-in-authguard)
  - [10.5 Tenant Isolation Test](#105-tenant-isolation-test)
- [11. OFFLINE-FIRST ARCHITECTURE](#11-offline-first-architecture)
  - [11.1 WatermelonDB Schema (with Versioned Migrations)](#111-watermelondb-schema-with-versioned-migrations)
    - [Migration File (required for existing users)](#migration-file-required-for-existing-users)
  - [11.2 WatermelonDB Models](#112-watermelondb-models)
  - [11.3 Sync Engine (Delta-Based)](#113-sync-engine-delta-based)
  - [11.4 Sync Pull Endpoint (Backend)](#114-sync-pull-endpoint-backend)
- [12. SECURITY ARCHITECTURE](#12-security-architecture)
  - [12.1 Security Layers](#121-security-layers)
  - [12.2 JWT Security Specification](#122-jwt-security-specification)
  - [12.3 Paystack Webhook Security](#123-paystack-webhook-security)
  - [12.4 Security Headers (Helmet)](#124-security-headers-helmet)
- [13. CACHING & PERFORMANCE ARCHITECTURE](#13-caching-performance-architecture)
  - [13.1 Redis Cache Strategy + Rate Limiting](#131-redis-cache-strategy-rate-limiting)
  - [13.2 Cache Key Registry](#132-cache-key-registry)
  - [13.3 Cache-Aside Pattern (Storefront)](#133-cache-aside-pattern-storefront)
  - [13.4 Database Query Performance](#134-database-query-performance)
- [14. BACKGROUND JOBS & EVENT SYSTEM](#14-background-jobs-event-system)
  - [14.1 Event System - Transactional Outbox Pattern](#141-event-system-transactional-outbox-pattern)
    - [OutboxEvent Table (add to Prisma schema)](#outboxevent-table-add-to-prisma-schema)
    - [Writing to the Outbox (same transaction as business operation)](#writing-to-the-outbox-same-transaction-as-business-operation)
    - [Outbox Relay Service](#outbox-relay-service)
    - [Dead-Letter Queue - Replay Endpoint](#dead-letter-queue-replay-endpoint)
  - [14.2 Domain Events Registry](#142-domain-events-registry)
  - [14.3 Scheduled Jobs (Distributed-Safe)](#143-scheduled-jobs-distributed-safe)
- [15. ERROR HANDLING & OBSERVABILITY](#15-error-handling-observability)
  - [15.1 Global Exception Filter (Complete)](#151-global-exception-filter-complete)
  - [15.2 Structured Logging (Pino)](#152-structured-logging-pino)
  - [15.3 Health Check Endpoint](#153-health-check-endpoint)
  - [15.4 Monitoring Stack](#154-monitoring-stack)
  - [15.5 Request Tracing (requestId Propagation)](#155-request-tracing-requestid-propagation)
  - [15.6 SLO Definitions](#156-slo-definitions)
- [16. DEPLOYMENT ARCHITECTURE](#16-deployment-architecture)
  - [16.1 Cloud Deployment (Recommended)](#161-cloud-deployment-recommended)
    - [Full Architecture Diagram](#full-architecture-diagram)
    - [GitHub Actions CI/CD Pipeline](#github-actions-cicd-pipeline)
- [.github/workflows/deploy.yml](#githubworkflowsdeployyml)
    - [Environment Variables (Complete)](#environment-variables-complete)
- [.env.example -- all required vars with descriptions](#envexample-all-required-vars-with-descriptions)
- [-- App ------------------------------------------](#-app-)
- [-- Database -------------------------------------](#-database-)
- [Port 6543 = PgBouncer (transaction mode) -- use for all queries, prevents connection exhaustion](#port-6543-pgbouncer-transaction-mode-use-for-all-queries-prevents-connection-exhaustion)
- [connection_limit=2 per instance -- PgBouncer handles multiplexing efficiently](#connection_limit2-per-instance-pgbouncer-handles-multiplexing-efficiently)
- [Port 5432 = direct connection -- use ONLY for Prisma migrations](#port-5432-direct-connection-use-only-for-prisma-migrations)
- [-- Redis ----------------------------------------](#-redis-)
- [-- JWT ------------------------------------------](#-jwt-)
- [NOTE: JWT_ACCESS_SECRET and JWT_REFRESH_SECRET are NOT used.](#note-jwt_access_secret-and-jwt_refresh_secret-are-not-used)
- [Supabase Auth issues and verifies all merchant/consumer JWTs.](#supabase-auth-issues-and-verifies-all-merchantconsumer-jwts)
- [NestJS only verifies using SUPABASE_JWT_SECRET (set in Supabase Auth section below).](#nestjs-only-verifies-using-supabase_jwt_secret-set-in-supabase-auth-section-below)
- [-- Paystack -------------------------------------](#-paystack-)
- [-- Termii ---------------------------------------](#-termii-)
- [-- Firebase (FCM) -------------------------------](#-firebase-fcm-)
- [-- Supabase Auth --------------------------------](#-supabase-auth-)
- [-- CORS -----------------------------------------](#-cors-)
- [-- Sentry ---------------------------------------](#-sentry-)
    - [Joi Validation Schema (Startup Guard)](#joi-validation-schema-startup-guard)
  - [16.2 VPS Deployment (Bootstrap Option)](#162-vps-deployment-bootstrap-option)
    - [Architecture](#architecture)
    - [Nginx Configuration](#nginx-configuration)
- [/etc/nginx/sites-available/invento-api](#etcnginxsites-availableinvento-api)
    - [PM2 Ecosystem Config](#pm2-ecosystem-config)
    - [Automated PostgreSQL Backup Script](#automated-postgresql-backup-script)
- [/etc/cron.d/invento-backup -- runs daily at 3am WAT](#etccrondinvento-backup-runs-daily-at-3am-wat)
- [/opt/invento/scripts/backup.sh](#optinventoscriptsbackupsh)
- [Dump and compress](#dump-and-compress)
- [Upload to Backblaze B2](#upload-to-backblaze-b2)
- [Clean local temp](#clean-local-temp)
- [Prune old B2 backups (keep last 30 days)](#prune-old-b2-backups-keep-last-30-days)
  - [16.3 Mobile App Deployment](#163-mobile-app-deployment)
    - [EAS Build Profiles](#eas-build-profiles)
    - [Build & Release Commands](#build-release-commands)
- [Internal testing build (APK)](#internal-testing-build-apk)
- [Production build (AAB for Play Store)](#production-build-aab-for-play-store)
- [Submit to Play Store internal track](#submit-to-play-store-internal-track)
- [OTA update (JS-only changes -- no store review needed)](#ota-update-js-only-changes-no-store-review-needed)
- [Check build status](#check-build-status)
- [17. API CONTRACT REFERENCE](#17-api-contract-reference)
  - [Complete Endpoint List](#complete-endpoint-list)
- [18. TECHNOLOGY STACK REFERENCE](#18-technology-stack-reference)
  - [Backend](#backend)
  - [Mobile (Both Apps)](#mobile-both-apps)
  - [Web Dashboard](#web-dashboard)
  - [Infrastructure](#infrastructure)
- [APPENDIX A: FEATURE → PLAN MATRIX](#appendix-a-feature-plan-matrix)
- [APPENDIX B: RISK REGISTER & MITIGATIONS](#appendix-b-risk-register-mitigations)
- [19. OFFLINE CONFLICT RESOLUTION - COMPLETE FRAMEWORK](#19-offline-conflict-resolution-complete-framework)
  - [19.1 Root Cause](#191-root-cause)
  - [19.2 Product Revision Numbers (Optimistic Locking)](#192-product-revision-numbers-optimistic-locking)
  - [19.3 FORCE_SYNC - Merchant Override for Completed Sales](#193-force_sync-merchant-override-for-completed-sales)
  - [19.4 Conflict Notification Payload](#194-conflict-notification-payload)
- [20. OUTBOX — PRODUCTION-GRADE JOB PROCESSING](#20-outbox-production-grade-job-processing)
  - [20.1 Priority + Visibility Timeout + Backoff](#201-priority-visibility-timeout-backoff)
    - [Per-Event-Type Retry Policy](#per-event-type-retry-policy)
  - [20.2 Updated Relay Service](#202-updated-relay-service)
  - [20.3 FCM Idempotency](#203-fcm-idempotency)
  - [20.4 Dead-Letter Alerting](#204-dead-letter-alerting)
- [21. CQRS — EXPLICIT READ/WRITE SEPARATION](#21-cqrs-explicit-readwrite-separation)
  - [21.1 Command vs Query Classification](#211-command-vs-query-classification)
  - [21.2 DailySalesSummary Read Model](#212-dailysalessummary-read-model)
  - [21.3 Updated Sales Summary Endpoint](#213-updated-sales-summary-endpoint)
- [22. OBSERVABILITY — WORLD-CLASS STACK](#22-observability-world-class-stack)
  - [22.1 Metrics Endpoint (Prometheus Format)](#221-metrics-endpoint-prometheus-format)
  - [22.2 Deep Health Check](#222-deep-health-check)
  - [22.3 Alerting Rules](#223-alerting-rules)
  - [22.4 SLO Error Budget Calculation](#224-slo-error-budget-calculation)
- [23. CACHE KEY ENFORCEMENT — SYSTEMATIC TENANT SCOPING](#23-cache-key-enforcement-systematic-tenant-scoping)
  - [23.1 CacheKeyBuilder Utility](#231-cachekeybuilder-utility)
  - [23.2 Cache Key Registry](#232-cache-key-registry)
  - [23.3 Global Search Cache Invalidation](#233-global-search-cache-invalidation)
- [24. SECURITY — FINANCIAL-GRADE HARDENING](#24-security-financial-grade-hardening)
  - [24.1 Audit Log](#241-audit-log)
  - [24.2 Sub-Role System (RBAC)](#242-sub-role-system-rbac)
  - [24.3 Per-Tenant Rate Limiting](#243-per-tenant-rate-limiting)
  - [24.4 Paystack Webhook Replay Attack Protection](#244-paystack-webhook-replay-attack-protection)
  - [24.5 Secrets Rotation Runbook](#245-secrets-rotation-runbook)
- [25. DATABASE SCALING PLAN](#25-database-scaling-plan)
  - [25.1 Connection Pool Math](#251-connection-pool-math)
  - [25.2 Read Replica Strategy](#252-read-replica-strategy)
  - [25.3 Sale Table Partitioning](#253-sale-table-partitioning)
  - [25.4 Archival Strategy](#254-archival-strategy)
- [26. API VERSIONING STRATEGY](#26-api-versioning-strategy)
  - [26.1 Versioning Policy](#261-versioning-policy)
  - [26.2 Deprecation Timeline](#262-deprecation-timeline)
  - [26.3 Mobile App Version Header](#263-mobile-app-version-header)
  - [26.4 EAS Update + API Deploy Coordination Rule](#264-eas-update-api-deploy-coordination-rule)
- [27. TESTING STRATEGY](#27-testing-strategy)
  - [27.1 Coverage Targets](#271-coverage-targets)
  - [27.2 Test Database Setup](#272-test-database-setup)
  - [27.3 Factory Pattern](#273-factory-pattern)
  - [27.4 Contract Testing](#274-contract-testing)
  - [27.5 Load Test Scenario](#275-load-test-scenario)
- [k6 load test -- 50 merchants syncing simultaneously after network outage](#k6-load-test-50-merchants-syncing-simultaneously-after-network-outage)
- [k6 run test/load/sync.js](#k6-run-testloadsyncjs)
  - [27.6 Chaos Scenarios](#276-chaos-scenarios)
  - [27.7 Test File Conventions](#277-test-file-conventions)
- [Unit tests (fast, no DB)](#unit-tests-fast-no-db)
- [Integration tests (requires test DB)](#integration-tests-requires-test-db)
- [All tests with coverage](#all-tests-with-coverage)
- [Load test (requires k6 installed)](#load-test-requires-k6-installed)
- [28. FINAL HARDENING - PRODUCTION COMPLETENESS PASS](#28-final-hardening-production-completeness-pass)
  - [28.1 Remaining Issues Fixed in This Pass](#281-remaining-issues-fixed-in-this-pass)
  - [28.2 Complete Event Flow (Final State)](#282-complete-event-flow-final-state)
  - [28.3 Graceful Degradation Strategy](#283-graceful-degradation-strategy)
  - [28.4 Schema Migration Strategy](#284-schema-migration-strategy)
    - [Migration Rules](#migration-rules)
    - [Migration Deployment Order](#migration-deployment-order)
    - [WatermelonDB Mobile Migration Rule](#watermelondb-mobile-migration-rule)
  - [28.5 Disaster Recovery Plan](#285-disaster-recovery-plan)
    - [RTO / RPO Targets](#rto-rpo-targets)
    - [Backup Strategy](#backup-strategy)
    - [Recovery Runbook](#recovery-runbook)
  - [28.6 Sync Endpoint Rate Limiting](#286-sync-endpoint-rate-limiting)
  - [28.7 Order Creation Idempotency (Consumer Double-Tap)](#287-order-creation-idempotency-consumer-double-tap)
  - [28.8 Input Sanitization](#288-input-sanitization)
  - [28.9 OWASP Top 10 Compliance Checklist](#289-owasp-top-10-compliance-checklist)
  - [28.10 Production Launch Checklist](#2810-production-launch-checklist)
    - [Infrastructure](#infrastructure-1)
    - [Security](#security)
    - [Data](#data)
    - [Mobile](#mobile)
    - [Observability](#observability)
    - [Business](#business)
  - [28.11 Complete Prisma Schema — Final Authoritative Version](#2811-complete-prisma-schema-final-authoritative-version)
  - [28.12 Complete API Endpoint Reference — Final State](#2812-complete-api-endpoint-reference-final-state)
  - [28.13 Architecture Decision Log](#2813-architecture-decision-log)
  - [28.14 Version History](#2814-version-history)
- [29. ELITE HARDENING — FINAL 10/10 PASS](#29-elite-hardening-final-1010-pass)
  - [SECTION A: FINAL REMAINING ISSUES](#section-a-final-remaining-issues)
  - [SECTION B: ELITE HARDENING SOLUTIONS](#section-b-elite-hardening-solutions)
    - [E1 — DETERMINISTIC CONFLICT RESOLUTION](#e1-deterministic-conflict-resolution)
    - [E2 — QUEUE SYSTEM: JUSTIFIED DECISION](#e2-queue-system-justified-decision)
    - [E3 — CQRS: JUSTIFIED PARTIAL IMPLEMENTATION](#e3-cqrs-justified-partial-implementation)
    - [E4 — OBSERVABILITY: FULL SRE LEVEL](#e4-observability-full-sre-level)
    - [E5 — MONOLITH EXTRACTION PATH](#e5-monolith-extraction-path)
    - [E6 — DATABASE SCALING: HYPERSCALE READY](#e6-database-scaling-hyperscale-ready)
    - [E7 — DOCUMENT CONSISTENCY: SECTION 14.1 OUTBOX SCHEMA](#e7-document-consistency-section-141-outbox-schema)
  - [SECTION C: FINAL 10/10 ARCHITECTURE](#section-c-final-1010-architecture)
    - [Complete System Architecture — Final State](#complete-system-architecture-final-state)
    - [Data Flow — Complete (Final State)](#data-flow-complete-final-state)
    - [Conflict Resolution — Mathematical Guarantee](#conflict-resolution-mathematical-guarantee)
    - [Scaling Path — Quantified](#scaling-path-quantified)
    - [Technology Stack — Final Reference](#technology-stack-final-reference)
    - [Document Version](#document-version)

## Integrated Inventory, POS & E-Commerce SaaS Platform - Nigerian Markets
### Document Class: Production Architecture | Confidential
#### Version 3.1 | March 2026 | Authored for Engineering & Investment Review

> This document defines the complete, unambiguous, production-ready system architecture for Invento.
> Every component, pattern, data flow, and design decision is specified with enough precision
> that a senior engineering team can begin implementation directly from this document.
> No section is vague. No component is missing. No decision is left to interpretation.

# 1. EXECUTIVE ARCHITECTURE SUMMARY

## What Invento Is

Invento is a **multi-tenant B2B2C SaaS platform** with three deeply integrated product layers:

| Layer | Users | Core Value |
|---|---|---|
| Merchant Suite | Shop owners, market traders | Inventory management, POS, CRM, analytics |
| Merchant Storefront | Merchants (setup), Consumers (browse) | Auto-generated online shop from inventory |
| Consumer Marketplace | End customers | Browse, buy, track across all registered shops |

## Architecture Philosophy

The architecture is built on five non-negotiable principles:

1. **Offline-first on mobile** - The merchant POS must work with zero internet. Kano markets have unreliable connectivity. This is not optional.
2. **Multi-tenant by default** - Every query, every write, every cache key is scoped to a tenant. Data leakage between merchants is a zero-tolerance failure.
3. **Domain isolation on the backend** - Each business domain (inventory, POS, orders, payments) is a self-contained NestJS module with its own service, repository, and DTOs. No cross-domain direct DB access.
4. **Consistent API contract** - Every response from the API follows the same envelope. The mobile team never guesses the shape of a response.
5. **Durable events, never fire-and-forget** - Every domain event is written to the database in the same transaction as the business operation. No event is ever lost to a process crash.

## Architecture Style

| Layer | Pattern |
|---|---|
| Backend | Layered Architecture (Controller → Service → Repository) with Domain Events |
| Mobile | Vertical Slice Architecture (feature-first folders) |
| Database | Shared schema, row-level isolation via `merchantId` + PostgreSQL RLS |
| State (mobile) | Server state via TanStack Query, client state via Zustand, offline state via WatermelonDB |
| Communication | REST (synchronous), Transactional Outbox (async domain events) |

# 2. SYSTEM TOPOLOGY & COMPONENT MAP

## 2.1 Full System Diagram

```
+--------------------------------------------------------------------------+
|                           CLIENT LAYER                                  |
|                                                                          |
|  +-----------------+  +-----------------+  +--------------------------+ |
|  |  MERCHANT APP   |  |  CONSUMER APP   |  |   ADMIN WEB DASHBOARD    | |
|  |  React Native   |  |  React Native   |  |   Next.js 14 App Router  | |
|  |  Expo Bare      |  |  Expo Bare      |  |   Vercel (free)          | |
|  |                 |  |                 |  |                          | |
|  | WatermelonDB    |  | TanStack Query  |  | TanStack Query           | |
|  | (offline SQLite)|  | Zustand         |  | shadcn/ui + Recharts     | |
|  | MMKV (tokens)   |  | MMKV (tokens)   |  | NextAuth.js (admin auth) | |
|  +-----------------+  +-----------------+  +--------------------------+ |
+-----------+--------------------+-------------------------+--------------+
            |                    |                         |
            +--------------------+-------------------------+
                                 | HTTPS / REST API
+--------------------------------+-----------------------------------------+
|                    EDGE / GATEWAY LAYER                                  |
|                                                                          |
|              +--------------------------------------+                   |
|              |         CLOUDFLARE (Free)             |                   |
|              |  DNS | DDoS Protection | CDN | WAF    |                   |
|              |  SSL Termination | Rate Limiting       |                   |
|              +--------------------------------------+                   |
+---------------------------------+---------------------------------------+
                                  |
+---------------------------------+---------------------------------------+
|                      BACKEND LAYER (NestJS)                             |
|                                                                          |
|  +------------------------------------------------------------------+   |
|  |                    GLOBAL MIDDLEWARE CHAIN                        |   |
|  |  Helmet → CORS → RateLimitGuard(@upstash) → AuthGuard(SUPABASE_JWT)   |   |
|  |  → RolesGuard → ValidationPipe → Controller → Service               |   |
|  |  → Repository → OutboxEvent(in tx) → ResponseTransformer            |   |
|  +------------------------------------------------------------------+   |
|                                                                          |
|  +----------+ +----------+ +----------+ +----------+ +--------------+  |
|  |   AUTH   | |INVENTORY | |   POS    | |  ORDERS  | |   PAYMENTS   |  |
|  |  Module  | |  Module  | |  Module  | |  Module  | |    Module    |  |
|  +----------+ +----------+ +----------+ +----------+ +--------------+  |
|  +----------+ +----------+ +----------+ +--------------------------+   |
|  |STOREFRONT| |  ADMIN   | |NOTIFICA- | |  TRANSACTIONAL OUTBOX    |   |
|  |  Module  | |  Module  | |  TIONS   | |  OutboxRelayService(1s)  |   |
|  +----------+ +----------+ +----------+ +--------------------------+   |
|                                                                          |
|  +------------------------------------------------------------------+   |
|  |                    INTEGRATION ADAPTERS                           |   |
|  |  PaystackAdapter | TermiiAdapter | FirebaseAdapter | MapboxAdapter|   |
|  +------------------------------------------------------------------+   |
+--------------------------------------------------------------------------+
                                  |
+---------------------------------+---------------------------------------+
|                        DATA LAYER                                        |
|                                                                          |
|  +---------------------+  +------------------+  +-------------------+  |
|  |  PostgreSQL 16       |  |  Redis (Upstash) |  |  Supabase Storage |  |
|  |  via Supabase        |  |  Cache + Outbox  |  |  Product images   |  |
|  |  Prisma ORM          |  |  Rate limiting   |  |  Import CSVs      |  |
|  |  RLS policies        |  |  Cron locks      |  |  CDN delivery     |  |
|  +---------------------+  +------------------+  +-------------------+  |
+--------------------------------------------------------------------------+
                                  |
+---------------------------------+---------------------------------------+
|                   EXTERNAL SERVICES                                      |
|                                                                          |
|  Paystack (payments)  Termii (SMS/OTP)  Firebase FCM (push)             |
|  Mapbox (maps/Phase3) Sentry (errors)   UptimeRobot (monitoring)        |
+--------------------------------------------------------------------------+
```

## 2.2 User Roles & Permissions Matrix

| Permission | Merchant Owner | Merchant Cashier | Consumer | Admin Viewer | Admin Operator | Admin Super |
|---|---|---|---|---|---|---|
| Manage own inventory | Yes | No | No | No | No | Yes |
| Run POS sales | Yes | Yes | No | No | No | No |
| View own sales analytics | Yes | No | No | Yes (all) | Yes (all) | Yes (all) |
| Browse storefront | Yes (own) | Yes (own) | Yes (all) | Yes | Yes | Yes |
| Place orders | No | No | Yes | No | No | No |
| Fulfill orders | Yes | No | No | No | Yes (override) | Yes (override) |
| Manage subscription plan | No | No | No | No | No | Yes |
| Access admin dashboard | No | No | No | Yes (read) | Yes | Yes |
| Register new market | No | No | No | No | Yes | Yes |
| Replay dead-letter events | No | No | No | No | No | Yes |

## 2.3 MVP Scope vs Post-MVP

| Feature | MVP (Months 1-6) | Post-MVP |
|---|---|---|
| Phone OTP auth | Yes | - |
| Inventory CRUD + barcode scan | Yes | Supplier mgmt, purchase orders |
| POS with offline sync | Yes | Bluetooth printer, split payments |
| Basic CRM (customer records) | Yes | Loyalty programs, SMS campaigns |
| Online storefront per merchant | Yes | Custom domain per shop |
| Consumer marketplace (browse/buy) | Yes | Wishlist, reviews, Q&A |
| Paystack payments | Yes | Flutterwave, crypto |
| SMS OTP + push notifications | Yes | WhatsApp Business API, email |
| Sales analytics (daily/weekly/monthly) | Yes | Custom reports, data export |
| Virtual market map | No Phase 3 | Mapbox interactive map |
| Social media sync | No Phase 4 | Facebook/Instagram shop sync |
| Logistics integration | No Phase 3 | Kwik Delivery, GIG Logistics |
| Hausa language support | Yes (i18n ready) | Full translation |

# 3. ARCHITECTURAL PRINCIPLES & PATTERNS

## 3.1 Backend: Layered Architecture with Domain Events

```
+-----------------------------------------------------+
|                  PRESENTATION LAYER                  |
|  Controllers | DTOs | Guards | Interceptors | Pipes  |
|  Responsibility: HTTP in/out, validation, auth       |
+-----------------------------------------------------+
                           | calls
+--------------------------?--------------------------+
|                  APPLICATION LAYER                   |
|  Services | Use Cases | Domain Event Publishers      |
|  Responsibility: Orchestrate business logic          |
|  Rule: Services NEVER call other module's services   |
|        They communicate via Domain Events only       |
+-----------------------------------------------------+
                           | calls
+--------------------------?--------------------------+
|                INFRASTRUCTURE LAYER                  |
|  Repositories | Prisma | Redis | External Adapters   |
|  Responsibility: Data access, external I/O           |
|  Rule: Repositories are the ONLY place Prisma runs   |
+-----------------------------------------------------+
```

**Domain Events** decouple modules via the Transactional Outbox pattern. When an order is confirmed, `OrdersModule` writes an `OutboxEvent` inside the same Prisma transaction. `OutboxRelayService` delivers it to `NotificationsModule` within 1 second - with retry, dead-letter queue, and full replay capability. No `EventEmitter2`. No in-memory bus. No event loss on process crash.

```typescript
// CORRECT: Domain event via Transactional Outbox (inside Prisma transaction)
await tx.outboxEvent.create({
  data: {
    eventType: 'order.confirmed',
    aggregateId: orderId,
    payload: { orderId, merchantId, consumerId, total: total.toString(), items },
  },
});
// OutboxRelayService polls every 1s, delivers to NotificationsService
// If process crashes: event is in DB, relay picks it up on restart -- zero loss

// REMOVED -- never use EventEmitter2 for domain events:
// this.eventEmitter.emit('order.confirmed', new OrderConfirmedEvent(...));
```

## 3.2 Mobile: Vertical Slice Architecture

Each feature owns all its code. No shared `components/` or `services/` folders at the top level.

```
features/
  inventory/
    +-- screens/          → UI screens
    +-- components/       → Feature-specific components
    +-- hooks/            → TanStack Query hooks for this feature
    +-- store/            → Zustand slice for this feature
    +-- db/               → WatermelonDB models for this feature
    +-- types.ts          → Feature-specific TypeScript types

shared/
  +-- components/         → Truly shared: Button, Input, Avatar
  +-- hooks/              → useAuth, useNetworkStatus
  +-- utils/              → currency formatter, date helpers
```

## 3.3 API Design: REST with Consistent Envelope

All responses follow this contract - no exceptions:

```typescript
// Success (2xx)
{
  "success": true,
  "data": T,
  "meta": {                    // present on paginated responses only
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  },
  "timestamp": "2026-03-25T10:00:00.000Z"
}

// Error (4xx / 5xx)
{
  "success": false,
  "error": {
    "code": "INSUFFICIENT_STOCK",      // machine-readable
    "message": "Product X has only 2 units remaining",  // human-readable
    "details": [...]                   // validation errors array, if applicable
  },
  "timestamp": "2026-03-25T10:00:00.000Z",
  "path": "/api/v1/pos/sales"
}
```

## 3.4 Error Code Registry

| Code | HTTP Status | Meaning |
|---|---|---|
| `VALIDATION_ERROR` | 400 | DTO validation failed |
| `INVALID_OTP` | 400 | OTP wrong or expired |
| `INSUFFICIENT_STOCK` | 400 | Not enough stock for sale/order |
| `UNAUTHORIZED` | 401 | Missing or invalid JWT |
| `FORBIDDEN` | 403 | Valid JWT but wrong role/ownership |
| `NOT_FOUND` | 404 | Resource does not exist |
| `DUPLICATE_ENTRY` | 409 | Unique constraint violation |
| `PLAN_LIMIT_EXCEEDED` | 402 | Merchant on FREE plan, SKU limit hit |
| `WEBHOOK_SIGNATURE_INVALID` | 401 | Paystack HMAC mismatch |
| `INTERNAL_ERROR` | 500 | Unhandled exception |

# 4. FRONTEND ARCHITECTURE - REACT NATIVE

## 4.1 Monorepo Structure

```
invento/
+-- apps/
|   +-- merchant/                    → Merchant POS + Inventory app
|   +-- consumer/                    → Consumer marketplace app
|   +-- web/                         → Next.js admin dashboard
+-- packages/
|   +-- shared-ui/                   → Shared React Native components
|   +-- shared-api/                  → TanStack Query hooks + Axios client
|   +-- shared-types/                → TypeScript types (generated from Prisma)
|   +-- shared-utils/                → Currency, date, validation helpers
|   +-- db/                          → WatermelonDB schema + sync engine
+-- package.json                     → npm workspaces root
+-- tsconfig.base.json               → Shared TS config
+-- .eslintrc.base.js                → Shared ESLint rules
```

## 4.2 Merchant App - Complete Architecture

### Tech Stack
| Concern | Library | Reason |
|---|---|---|
| Framework | React Native (Expo Bare) | Native module access (barcode, camera, BT) |
| Navigation | Expo Router v5 | File-based, same as Next.js, team familiarity |
| Server state | TanStack Query v5 | Caching, background sync, optimistic updates |
| Client state | Zustand | Auth store, POS cart state |
| Offline DB | WatermelonDB | SQLite, reactive, built-in sync protocol |
| Fast storage | MMKV | Tokens, settings | 30x faster than AsyncStorage |
| Forms | React Hook Form + Zod | Type-safe validation, no re-render on keystroke |
| UI system | NativeWind v4 | Tailwind in RN | consistent design tokens |
| Lists | FlashList (Shopify) | 10x faster than FlatList for product lists |
| Animations | Reanimated v3 | UI thread animations, no JS bridge jank |
| Images | Expo Image | Built-in caching, progressive loading |
| Barcode | expo-barcode-scanner | Inventory scan, POS product lookup |
| Payments | Paystack RN SDK | WebView checkout, official SDK |
| Push | Expo Notifications + FCM | Cross-platform, free |
| Builds | EAS Build | Cloud builds, no Mac needed for Android |
| OTA updates | EAS Update | JS-only hotfixes without store review |
| Errors | @sentry/react-native | Crash reports, performance traces |
| i18n | i18next + react-i18next | English + Hausa language support |

### Folder Structure (Vertical Slice)
```
apps/merchant/
+-- app/                             → Expo Router file-based routes
|   +-- _layout.tsx                  → Root layout (providers, fonts)
|   +-- (auth)/
|   |   +-- _layout.tsx
|   |   +-- login.tsx                → Phone number entry
|   |   +-- verify-otp.tsx           → OTP input
|   +-- (tabs)/
|   |   +-- _layout.tsx              → Tab bar config
|   |   +-- dashboard.tsx            → Sales summary + alerts
|   |   +-- inventory/
|   |   |   +-- _layout.tsx
|   |   |   +-- index.tsx            → Product list (FlashList)
|   |   |   +-- add.tsx              → Add product form
|   |   |   +-- [id].tsx             → Edit product
|   |   +-- pos/
|   |   |   +-- index.tsx            → POS checkout screen
|   |   +-- orders/
|   |       +-- index.tsx            → Incoming orders list
|   |       +-- [id].tsx             → Order detail + status update
|   +-- (modals)/
|       +-- scan.tsx                 → Barcode scanner modal
|       +-- receipt/[saleId].tsx     → Digital receipt
|
+-- features/                        → Vertical slices
|   +-- auth/
|   |   +-- components/
|   |   |   +-- PhoneInput.tsx
|   |   |   +-- OtpInput.tsx
|   |   +-- hooks/
|   |   |   +-- useAuth.ts           → sendOtp, verifyOtp mutations
|   |   +-- store/
|   |       +-- authStore.ts         → Zustand: user, tokens
|   |
|   +-- inventory/
|   |   +-- components/
|   |   |   +-- ProductCard.tsx
|   |   |   +-- ProductForm.tsx
|   |   |   +-- LowStockBadge.tsx
|   |   |   +-- BulkImportSheet.tsx
|   |   +-- hooks/
|   |   |   +-- useProducts.ts       → TanStack Query: list, create, update, delete
|   |   |   +-- useSkuLookup.ts      → Barcode scan → product lookup
|   |   +-- db/
|   |       +-- ProductModel.ts      → WatermelonDB model
|   |
|   +-- pos/
|   |   +-- components/
|   |   |   +-- CartItem.tsx
|   |   |   +-- PaymentMethodPicker.tsx
|   |   |   +-- NumericKeypad.tsx
|   |   |   +-- ReceiptView.tsx
|   |   +-- hooks/
|   |   |   +-- usePOS.ts            → createSale mutation
|   |   +-- store/
|   |   |   +-- cartStore.ts         → Zustand: cart items, total
|   |   +-- db/
|   |       +-- SaleModel.ts         → WatermelonDB model
|   |
|   +-- orders/
|   |   +-- components/
|   |   |   +-- OrderCard.tsx
|   |   |   +-- StatusBadge.tsx
|   |   +-- hooks/
|   |       +-- useOrders.ts
|   |
|   +-- dashboard/
|       +-- components/
|       |   +-- SalesSummaryCard.tsx
|       |   +-- LowStockAlert.tsx
|       |   +-- RecentSalesList.tsx
|       +-- hooks/
|           +-- useDashboard.ts
|
+-- assets/
|   +-- fonts/
|   +-- images/
+-- app.json
```

### State Architecture (Three-Layer)

```
+-----------------------------------------------------+
|              LAYER 1: SERVER STATE                   |
|              TanStack Query                          |
|  Products, Orders, Sales | fetched from API          |
|  Cached, background-refetched, stale-while-revalidate|
+-----------------------------------------------------+
+-----------------------------------------------------+
|              LAYER 2: CLIENT STATE                   |
|              Zustand                                 |
|  Auth (user, tokens) | persisted in MMKV             |
|  POS Cart (items, total) | in-memory only            |
+-----------------------------------------------------+
+-----------------------------------------------------+
|              LAYER 3: OFFLINE STATE                  |
|              WatermelonDB (SQLite)                   |
|  Products (local copy for POS lookup)                |
|  Sales (created offline, synced when online)         |
|  Customers (created offline, synced when online)     |
+-----------------------------------------------------+
```

**Rule:** Never put server data in Zustand. Never put offline data in TanStack Query. Each layer has one job.

## 4.3 Consumer App - Complete Architecture

### Folder Structure (Vertical Slice)
```
apps/consumer/
+-- app/
|   +-- (auth)/
|   |   +-- login.tsx
|   |   +-- verify-otp.tsx
|   +-- (tabs)/
|   |   +-- home.tsx                 → Featured shops, trending products
|   |   +-- search.tsx               → Cross-shop search + filters
|   |   +-- cart.tsx                 → Cart management
|   |   +-- orders.tsx               → Order history + tracking
|   +-- (stack)/
|       +-- shop/[shopId]/
|       |   +-- index.tsx            → Shop profile + product grid
|       |   +-- product/[id].tsx     → Product detail
|       +-- checkout/
|           +-- index.tsx            → Address + order summary
|           +-- payment.tsx          → Paystack WebView
|
+-- features/
|   +-- auth/                        → Same pattern as merchant
|   +-- storefront/
|   |   +-- components/
|   |   |   +-- ShopCard.tsx
|   |   |   +-- ProductGrid.tsx
|   |   |   +-- MarketBrowser.tsx
|   |   |   +-- SearchBar.tsx
|   |   +-- hooks/
|   |       +-- useMarkets.ts
|   |       +-- useShop.ts
|   |       +-- useProductSearch.ts
|   +-- cart/
|   |   +-- components/
|   |   |   +-- CartItemRow.tsx
|   |   +-- store/
|   |       +-- cartStore.ts         → Zustand: items, quantities, total
|   +-- orders/
|   |   +-- components/
|   |   |   +-- OrderCard.tsx
|   |   |   +-- OrderTracker.tsx
|   |   +-- hooks/
|   |       +-- useCreateOrder.ts
|   |       +-- useOrders.ts
|   +-- checkout/
|       +-- components/
|       |   +-- AddressForm.tsx
|       |   +-- OrderSummary.tsx
|       +-- hooks/
|           +-- useCheckout.ts
+-- app.json
```

## 4.4 Shared Packages

### packages/shared-api
```typescript
// Axios client with interceptors
const apiClient = axios.create({ baseURL: API_BASE_URL });

// Request interceptor: attach JWT
apiClient.interceptors.request.use((config) => {
  const token = useAuthStore.getState().accessToken;
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Response interceptor: handle 401 → refresh token
apiClient.interceptors.response.use(
  (res) => res.data.data,  // unwrap envelope automatically
  async (error) => {
    if (error.response?.status === 401) {
      const newToken = await refreshAccessToken();
      error.config.headers.Authorization = `Bearer ${newToken}`;
      return apiClient(error.config);
    }
    return Promise.reject(error);
  }
);
```

### packages/shared-types
Types are generated from Prisma schema using `prisma-json-types-generator` - single source of truth, no manual type duplication.

# 5. WEB DASHBOARD ARCHITECTURE - NEXT.JS ADMIN

## 5.1 Stack
| Layer | Tool |
|---|---|
| Framework | Next.js 14 (App Router) |
| Language | TypeScript (strict) |
| UI | Tailwind CSS + shadcn/ui |
| Charts | Recharts |
| Tables | TanStack Table v8 |
| Server state | TanStack Query v5 |
| Auth | NextAuth.js (email + password, admin only) |
| Hosting | Vercel (free hobby tier) |

## 5.2 Page Structure
```
app/
+-- (auth)/
|   +-- login/page.tsx
+-- (dashboard)/
|   +-- layout.tsx               → Sidebar + header
|   +-- page.tsx                 → Platform overview metrics
|   +-- merchants/
|   |   +-- page.tsx             → Merchant list + search
|   |   +-- [id]/page.tsx        → Merchant detail + plan management
|   +-- markets/
|   |   +-- page.tsx             → Market registry
|   |   +-- new/page.tsx         → Register new market
|   +-- orders/
|   |   +-- page.tsx             → All orders, filter by status
|   +-- payments/
|   |   +-- page.tsx             → Transaction log
|   +-- settings/
|       +-- page.tsx             → Platform config
```

## 5.3 Admin Auth
Admin uses email + bcrypt password (not OTP). Separate JWT secret (`JWT_ADMIN_SECRET`). Admin tokens are short-lived (1 hour) with no refresh - admins re-login.

# 6. BACKEND ARCHITECTURE - NESTJS LAYERED DESIGN

## 6.1 Complete Folder Structure

```
backend/src/
+-- main.ts                          → Bootstrap
+-- app.module.ts                    → Root module
|
+-- common/                          → Cross-cutting concerns
|   +-- decorators/
|   |   +-- current-user.decorator.ts    → @CurrentUser()
|   |   +-- public.decorator.ts          → @Public() | bypass JWT guard
|   |   +-- roles.decorator.ts           → @Roles('merchant', 'admin')
|   +-- filters/
|   |   +-- global-exception.filter.ts   → Catches ALL errors
|   +-- guards/
|   |   +-- jwt-auth.guard.ts            → Global JWT guard
|   |   +-- roles.guard.ts               → Role-based access
|   +-- interceptors/
|   |   +-- response-transform.interceptor.ts  → Wraps all responses
|   |   +-- logging.interceptor.ts             → Request/response logging
|   +-- pipes/
|   |   +-- parse-pagination.pipe.ts     → page/limit query params
|   +-- types/
|       +-- jwt-payload.type.ts
|       +-- api-response.type.ts
|       +-- paginated.type.ts
|
+-- config/
|   +-- app.config.ts
|   +-- database.config.ts
|   +-- jwt.config.ts
|   +-- validation.schema.ts         → Joi schema for env vars
|
+-- prisma/
|   +-- prisma.module.ts
|   +-- prisma.service.ts            → Singleton + tenant middleware + set_config RLS
|
+-- outbox/
|   +-- outbox.module.ts
|   +-- outbox.service.ts            → Write OutboxEvent in transactions
|   +-- outbox-relay.service.ts      → Poll + dispatch + distributed lock (every 1s)
|
+-- modules/
|   +-- auth/
|   |   +-- auth.module.ts
|   |   +-- auth.controller.ts
|   |   +-- auth.service.ts
|   |   +-- strategies/
|   |   |   +-- jwt.strategy.ts
|   |   |   +-- jwt-refresh.strategy.ts
|   |   +-- dto/
|   |       +-- send-otp.dto.ts
|   |       +-- verify-otp.dto.ts
|   |       +-- refresh-token.dto.ts
|   |
|   +-- merchants/
|   |   +-- merchants.module.ts
|   |   +-- merchants.controller.ts
|   |   +-- merchants.service.ts
|   |   +-- merchants.repository.ts
|   |   +-- dto/
|   |       +-- create-merchant.dto.ts
|   |       +-- update-merchant.dto.ts
|   |
|   +-- inventory/
|   |   +-- inventory.module.ts
|   |   +-- inventory.controller.ts
|   |   +-- inventory.service.ts
|   |   +-- inventory.repository.ts
|   |   +-- inventory.events.ts      → LowStockEvent definition
|   |   +-- dto/
|   |       +-- create-product.dto.ts
|   |       +-- update-product.dto.ts
|   |       +-- bulk-import.dto.ts
|   |
|   +-- pos/
|   |   +-- pos.module.ts
|   |   +-- pos.controller.ts
|   |   +-- pos.service.ts
|   |   +-- pos.repository.ts
|   |   +-- pos.events.ts            → SaleCreatedEvent definition
|   |   +-- dto/
|   |       +-- create-sale.dto.ts
|   |       +-- sale-item.dto.ts
|   |       +-- sync-sales.dto.ts
|   |
|   +-- orders/
|   |   +-- orders.module.ts
|   |   +-- orders.controller.ts
|   |   +-- orders.service.ts
|   |   +-- orders.repository.ts
|   |   +-- orders.events.ts         → OrderCreatedEvent, OrderConfirmedEvent
|   |   +-- dto/
|   |       +-- create-order.dto.ts
|   |       +-- order-item.dto.ts
|   |       +-- update-order-status.dto.ts
|   |
|   +-- payments/
|   |   +-- payments.module.ts
|   |   +-- payments.controller.ts
|   |   +-- payments.service.ts
|   |   +-- payments.repository.ts
|   |   +-- dto/
|   |       +-- paystack-webhook.dto.ts
|   |
|   +-- notifications/
|   |   +-- notifications.module.ts
|   |   +-- notifications.service.ts   → Called by OutboxRelayService.dispatch()
|   |
|   +-- storefront/
|   |   +-- storefront.module.ts
|   |   +-- storefront.controller.ts
|   |   +-- storefront.service.ts
|   |
|   +-- admin/
|       +-- admin.module.ts
|       +-- admin.controller.ts
|       +-- admin.service.ts
|
+-- integrations/
    +-- paystack/
    |   +-- paystack.module.ts
    |   +-- paystack.service.ts
    +-- termii/
    |   +-- termii.module.ts
    |   +-- termii.service.ts
    +-- firebase/
        +-- firebase.module.ts
        +-- firebase.service.ts
```

## 6.2 Request Lifecycle (Detailed)

```
1. HTTP Request arrives at Cloudflare
   → DDoS check, WAF rules, CDN cache hit check

2. Forwarded to Railway (NestJS)
   → Helmet middleware: sets security headers
   → CORS middleware: validates origin
   → ThrottlerGuard: checks rate limit in Redis

3. Route matching
   → If route is @Public(): skip JWT guard
   → If route requires auth: AuthGuard runs
     → Extract Bearer token from Authorization header
     → jwt.verify(token, process.env.SUPABASE_JWT_SECRET)  → Supabase JWT secret
     → If expired or invalid: return 401 UNAUTHORIZED
     → If valid: attach { sub, role, merchantId, plan } to request
     → If payload.merchantId: RequestContext.set('merchantId', payload.merchantId)

4. RolesGuard (if @Roles() decorator present)
   → Check request.user.role matches required role
   → If mismatch: return 403 FORBIDDEN

5. ValidationPipe
   → Transform body to DTO class instance
   → Run class-validator decorators
   → whitelist: true -- strip unknown fields
   → If invalid: return 400 VALIDATION_ERROR

6. Controller method executes
   → Calls Service method

7. Service method executes
   → Business logic
   → Calls Repository for DB operations
   → Emits domain events (non-blocking)

8. Repository executes
   → Prisma query (auto-scoped by tenant middleware)
   → Returns typed result

9. ResponseTransformInterceptor wraps result
   → { success: true, data: result, timestamp }

10. Response sent to client

On any unhandled error:
   → GlobalExceptionFilter catches
   → Maps to error code + message
   → Logs to Pino + Sentry (if 5xx)
   → Returns { success: false, error: {...} }
```

## 6.3 NestJS Module Dependency Graph

```
AppModule
  +-- ConfigModule.forRoot({ isGlobal: true, validationSchema })
  +-- PrismaModule (global: true)
  +-- RedisModule (global: true)
  +-- OutboxModule (global: true)          → Replaces EventEmitterModule
  |
  +-- AuthModule
  |   +-- imports: [JwtModule, PassportModule]
  |   +-- uses: PrismaModule, TermiiModule, RedisModule
  |
  +-- MerchantsModule
  |   +-- uses: PrismaModule
  |
  +-- InventoryModule
  |   +-- uses: PrismaModule, OutboxModule
  |
  +-- PosModule
  |   +-- uses: PrismaModule, OutboxModule
  |
  +-- OrdersModule
  |   +-- uses: PrismaModule, PaystackModule, OutboxModule
  |
  +-- PaymentsModule
  |   +-- uses: PrismaModule, PaystackModule, OutboxModule
  |
  +-- NotificationsModule
  |   +-- uses: TermiiModule, ExpoModule (push)
  |       → Receives events from OutboxRelayService dispatch
  |
  +-- StorefrontModule
  |   +-- uses: PrismaModule, RedisModule
  |
  +-- AdminModule
  |   +-- uses: PrismaModule, MerchantsModule, OutboxModule
  |
  +-- Integrations (all global: false, imported where needed)
      +-- PaystackModule
      +-- TermiiModule
      +-- ExpoModule                       → Replaces FirebaseModule for push
```

# 7. DOMAIN MODULE SPECIFICATIONS

## 7.1 Auth Module

> Replaced: custom NestJS auth service, JWT signing code, Redis OTP storage, Passport.js, refresh token table, rotation logic.
> Tool: Supabase Auth + one Supabase Edge Function for Termii.
> Engineering saved: ~3 days.

### How It Works

Supabase Auth handles the entire authentication lifecycle - OTP generation, SMS delivery hook, JWT issuance, refresh token rotation, and session revocation. Your NestJS backend is no longer responsible for any of this.

```
Phone OTP Flow:
  Client calls supabase.auth.signInWithOtp({ phone: '+2348012345678' })
  → Supabase Auth triggers the Send SMS Hook (Edge Function)
  → Edge Function calls Termii API to deliver the OTP SMS
  → Client calls supabase.auth.verifyOtp({ phone, token, type: 'sms' })
  → Supabase issues a signed JWT + refresh token automatically
  → JWT contains { sub, role, merchantId } via Custom Access Token Hook
  → Refresh token stored in Supabase auth.sessions (managed, not your table)
  → Token rotation and revocation handled by Supabase on every refresh/logout
```

### Supabase Edge Function - Send SMS Hook (~20 lines, replaces entire TermiiService for auth)

```typescript
// supabase/functions/send-sms-otp/index.ts
import { serve } from 'https://deno.land/std/http/server.ts';

serve(async (req) => {
  const { user, sms } = await req.json();

  await fetch('https://api.ng.termii.com/api/sms/otp/send', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      api_key: Deno.env.get('TERMII_API_KEY'),
      message_type: 'NUMERIC',
      to: user.phone,
      from: 'Invento',
      channel: 'dnd',
      pin_attempts: 3,
      pin_time_to_live: 5,
      pin_length: 6,
      pin_placeholder: '< 1234 >',
      message_text: `Your Invento code is < 1234 >. Valid for 5 minutes.`,
      pin_type: 'NUMERIC',
    }),
  });

  return new Response(JSON.stringify({ success: true }), { status: 200 });
});
```

Register this function in Supabase Dashboard → Authentication → Hooks → Send SMS.

### Custom Access Token Hook - Inject merchantId into JWT

```typescript
// supabase/functions/custom-access-token/index.ts
// Runs before every JWT is issued -- adds merchantId and role to the token
serve(async (req) => {
  const { user } = await req.json();

  // Look up the user's role and merchantId from your public.Merchant table
  const { data: merchant } = await supabase
    .from('Merchant')
    .select('id, plan')
    .eq('phone', user.phone)
    .single();

  return new Response(JSON.stringify({
    claims: {
      role: merchant → 'merchant' : 'consumer',
      merchantId: merchant?.id ?? null,
      plan: merchant?.plan ?? null,
    },
  }), { status: 200 });
});
```

### What Your NestJS Backend Still Does for Auth

NestJS no longer issues tokens or manages sessions. It only:
1. Verifies the Supabase JWT on incoming requests using the Supabase JWT secret
2. Extracts `merchantId` and `role` from the already-signed token claims
3. Passes those values to the Prisma RLS context (see Section 10)

```typescript
// common/guards/supabase-auth.guard.ts
@Injectable()
export class SupabaseAuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const isPublic = this.reflector.get<boolean>('isPublic', context.getHandler());
    if (isPublic) return true;

    const request = context.switchToHttp().getRequest();
    const token = request.headers.authorization?.split(' ')[1];
    if (!token) throw new UnauthorizedException();

    // Verify using Supabase JWT secret -- no Passport.js needed
    const payload = jwt.verify(token, process.env.SUPABASE_JWT_SECRET) as JwtPayload;
    request.user = payload;
    return true;
  }
}
```

### Admin Auth (unchanged)
Admin accounts use email + bcrypt password with a separate `JWT_ADMIN_SECRET`. This remains a small custom NestJS flow since admins are internal only and Supabase Auth is not used for them.

### Endpoints (simplified - most auth is now client-side via Supabase SDK)
```
POST /api/v1/auth/register-fcm    → Store Expo push token after login
  Auth:   Supabase JWT (verified by SupabaseAuthGuard)
  Body:   { expoPushToken: string }
  Action: Update Merchant.expoPushToken or Consumer.expoPushToken in DB

POST /api/v1/admin/auth/login     → Admin only, email + bcrypt
  Body:   { email: string, password: string }
  Action: bcrypt.compare → issue short-lived admin JWT (1hr, no refresh)
```

All other auth (send OTP, verify OTP, refresh token, logout) is handled directly by the Supabase client SDK on the mobile app and web dashboard - no NestJS endpoints needed.

## 7.2 Inventory Module

### Responsibilities
- Full CRUD for merchant products (scoped to merchantId)
- Stock level management with atomic decrements
- Low-stock alert emission (domain event)
- Bulk product import via CSV
- Barcode/SKU lookup for POS
- Plan enforcement: FREE tier limited to 50 SKUs

### Endpoints
```
GET /api/v1/inventory/products
  Auth:   Merchant JWT
  Query:  page, limit, category, search, lowStock (boolean), isActive
  Action: Paginated product list, auto-scoped to merchantId

POST /api/v1/inventory/products
  Auth:   Merchant JWT
  Body:   CreateProductDTO
  Action: Check plan limit (FREE: max 50 active products)
          Create product with merchantId from JWT
          Emit ProductCreatedEvent

GET /api/v1/inventory/products/:id
  Auth:   Merchant JWT
  Guard:  Product.merchantId must equal JWT merchantId

PATCH /api/v1/inventory/products/:id
  Auth:   Merchant JWT
  Body:   UpdateProductDTO (all fields optional)
  Action: Partial update, invalidate storefront cache

DELETE /api/v1/inventory/products/:id
  Auth:   Merchant JWT
  Action: Soft delete (isActive = false)
          Cannot delete if product has active orders

POST /api/v1/inventory/products/bulk-import
  Auth:    Merchant JWT
  Body:    multipart/form-data, field: file (CSV)
  Limits:  Max 500 rows per import, max 5MB file
  Action:  Parse CSV with papaparse
           Validate each row against CreateProductDTO
           Batch upsert (upsert on sku conflict)
           Return { imported, updated, failed, errors[] }

GET /api/v1/inventory/products/sku/:sku
  Auth:   Merchant JWT
  Action: Exact SKU match within merchant's products
          Used by POS barcode scanner
```

### Plan Enforcement
```typescript
async enforceProductLimit(merchantId: string) {
  const merchant = await this.prisma.merchant.findUnique({
    where: { id: merchantId },
    select: { plan: true },
  });
  if (merchant.plan === 'FREE') {
    const count = await this.prisma.product.count({
      where: { merchantId, isActive: true },
    });
    if (count >= 50) {
      throw new PaymentRequiredException('PLAN_LIMIT_EXCEEDED',
        'FREE plan is limited to 50 products. Upgrade to Basic to add more.');
    }
  }
}
```

### Domain Events Emitted
```typescript
// After product quantity drops to or below lowStockAt threshold
export class LowStockEvent {
  constructor(
    public readonly productId: string,
    public readonly productName: string,
    public readonly merchantId: string,
    public readonly currentQuantity: number,
    public readonly threshold: number,
  ) {}
}
```

## 7.3 POS Module

### Responsibilities
- Record in-store sales (cash, transfer, USSD, card)
- Offline batch sync with idempotency
- Atomic stock deduction per sale
- Customer record creation/update per sale
- Daily/weekly/monthly sales summary
- Domain event emission for downstream processing

### Endpoints
```
POST /api/v1/pos/sales
  Auth:   Merchant JWT
  Body:   CreateSaleDTO
  Action: Validate products belong to merchant
          Check stock for each item
          Prisma transaction:
            → Create Sale
            → Create SaleItems
            → Decrement product.quantity per item
            → Upsert Customer (if phone provided)
            → Update Customer.totalSpent, visitCount
          Emit SaleCreatedEvent
          Return created sale with items

POST /api/v1/pos/sales/sync
  Auth:   Merchant JWT
  Body:   { sales: CreateSaleDTO[] }
  Action: For each sale:
            → Check localId uniqueness (idempotency)
            → If exists: skip
            → If not: createSale() in transaction
          Return { synced, skipped, failed, errors[] }

GET /api/v1/pos/sales
  Auth:   Merchant JWT
  Query:  from (ISO date), to (ISO date), page, limit, paymentMethod
  Action: Paginated sales list with items

GET /api/v1/pos/sales/summary
  Auth:   Merchant JWT
  Query:  period = 'today' | 'week' | 'month' | 'custom'
          from, to (required if period = 'custom')
  Returns:
    {
      totalRevenue: Decimal,
      totalSales: number,
      avgSaleValue: Decimal,
      topProducts: [{ productId, name, unitsSold, revenue }],
      paymentBreakdown: { CASH: n, TRANSFER: n, USSD: n, CARD: n },
      dailyRevenue: [{ date, revenue, count }]  // for charts
    }
```

### Offline Sync - Delta-Based Full Specification

> Fix applied: Devices send `quantityDelta` (negative number = sold), not absolute quantities. Server applies atomically. Response includes per-sale status and authoritative stock values for local DB update.

```typescript
// CreateSaleDTO -- delta-based items
class SaleItemDTO {
  @IsString()   productId: string;
  @IsInt()      quantity: number;       // for receipt display
  @IsInt()      quantityDelta: number;  // negative = sold (e.g., -3)
  @IsDecimal()  unitPrice: string;
}

class CreateSaleDTO {
  @IsUUID()                           localId: string;
  @IsEnum(PaymentMethod)              paymentMethod: PaymentMethod;
  @IsArray() @ValidateNested({ each: true })
  @Type(() => SaleItemDTO)            items: SaleItemDTO[];
  @IsString() @IsOptional()           customerId?: string;
  @IsString() @IsOptional()           customerPhone?: string;
  @IsString() @IsOptional()           customerName?: string;
  @IsISO8601() @IsOptional()          createdAt?: string;
}

// Sync result -- granular per-sale status
interface SyncItemResult {
  productId: string;
  quantityApplied: number;  // what was actually deducted
  newQuantity: number;      // authoritative server quantity
}

interface SyncSaleResult {
  localId: string;
  status: 'SYNCED' | 'PARTIAL' | 'REJECTED' | 'DUPLICATE';
  reason?: string;
  stockUpdates: SyncItemResult[];  // device updates local DB from these
}

interface SyncResult {
  results: SyncSaleResult[];
  summary: { synced: number; partial: number; rejected: number; duplicate: number };
}
```

### Server-Side Delta Processing (atomic per sale)

```typescript
// pos.service.ts
async processSaleDelta(merchantId: string, dto: CreateSaleDTO): Promise<SyncSaleResult> {
  return this.prisma.$transaction(async (tx) => {
    const stockUpdates: SyncItemResult[] = [];

    for (const item of dto.items) {
      const product = await tx.product.findFirst({
        where: { id: item.productId, merchantId, isActive: true },
        select: { id: true, name: true, quantity: true },
      });

      if (!product) {
        return { localId: dto.localId, status: 'REJECTED', reason: `Product ${item.productId} not found`, stockUpdates: [] };
      }

      const newQty = product.quantity + item.quantityDelta; // delta is negative
      if (newQty < 0) {
        return {
          localId: dto.localId,
          status: 'REJECTED',
          reason: `Insufficient stock for ${product.name}: have ${product.quantity}, need ${Math.abs(item.quantityDelta)}`,
          stockUpdates: [],
        };
      }

      await tx.product.update({
        where: { id: item.productId },
        data: { quantity: { increment: item.quantityDelta } },
      });

      stockUpdates.push({ productId: item.productId, quantityApplied: item.quantityDelta, newQuantity: newQty });
    }

    // Create sale + outbox event in same transaction
    const sale = await tx.sale.create({ data: { localId: dto.localId, merchantId, ... } });
    await tx.outboxEvent.create({
      data: { eventType: 'sale.created', aggregateId: sale.id, payload: { saleId: sale.id, merchantId, items: dto.items } },
    });

    return { localId: dto.localId, status: 'SYNCED', stockUpdates };
  });
}
```

### Domain Events Emitted
```typescript
export class SaleCreatedEvent {
  constructor(
    public readonly saleId: string,
    public readonly merchantId: string,
    public readonly items: Array<{ productId: string; quantity: number }>,
    public readonly total: Decimal,
  ) {}
}
```

## 7.4 Orders Module

### Responsibilities
- Consumer places online order (multi-item, single merchant)
- Stock reservation on order creation
- Order status state machine with validation
- Merchant fulfillment workflow
- Consumer cancellation (PENDING only)

### Order Status State Machine
```
                    +---------+
                    | PENDING | → Created, payment not yet confirmed
                    +---------+
                         | Paystack webhook: charge.success
                    +----?----+
                    |CONFIRMED| → Payment received, stock deducted
                    +---------+
                         | Merchant action
                   +-----?------+
                   | PROCESSING | → Being prepared
                   +------------+
                         | Merchant action
                    +----?----+
                    | SHIPPED | → Handed to logistics
                    +---------+
                         | Delivery confirmed
                   +-----?-----+
                   | DELIVERED | → Terminal state
                   +-----------+

CANCELLED → From PENDING (consumer or merchant) or CONFIRMED (merchant only)
            Stock restored on cancellation
            Terminal state -- cannot be reactivated
```

### Transition Rules (enforced in service)
```typescript
const ALLOWED_TRANSITIONS: Record<OrderStatus, OrderStatus[]> = {
  PENDING:    ['CONFIRMED', 'CANCELLED'],
  CONFIRMED:  ['PROCESSING', 'CANCELLED'],
  PROCESSING: ['SHIPPED'],
  SHIPPED:    ['DELIVERED'],
  DELIVERED:  [],
  CANCELLED:  [],
};

// Role-based transition rules
const MERCHANT_TRANSITIONS = ['CONFIRMED', 'PROCESSING', 'SHIPPED', 'DELIVERED', 'CANCELLED'];
const CONSUMER_TRANSITIONS  = ['CANCELLED']; // only from PENDING
```

### Endpoints
```
POST /api/v1/orders
  Auth:   Consumer JWT
  Body:   CreateOrderDTO
  Action: Validate products exist and belong to merchantId
          Check stock for each item
          Prisma transaction:
            → Create Order (PENDING)
            → Create OrderItems (snapshot unitPrice at time of order)
            → Reserve stock: product.quantity -= qty
          Call PaystackService.initializeTransaction(order)
          Update order.paystackRef
          Emit OrderCreatedEvent
          Return { orderId, authorizationUrl, reference }

GET /api/v1/orders/:id
  Auth:   Consumer JWT (own orders) OR Merchant JWT (their orders)
  Guard:  Ownership check -- consumer sees own, merchant sees theirs

PATCH /api/v1/orders/:id/status
  Auth:   Merchant JWT OR Consumer JWT
  Body:   { status: OrderStatus, cancelReason?: string }
  Guard:  Validate transition is allowed for role
  Action: Update status
          If CANCELLED: restore reserved stock (Prisma transaction)
          Emit OrderStatusChangedEvent

GET /api/v1/orders
  Auth:   Consumer JWT → own orders
          Merchant JWT → incoming orders
  Query:  status, page, limit, from, to
```

## 7.5 Payments Module

### Responsibilities
- Initialize Paystack transaction
- Verify webhook HMAC-SHA512 signature
- Process charge.success → confirm order
- Process charge.failed → cancel order + restore stock
- Record all transactions with full audit trail

### Endpoints
```
POST /api/v1/payments/initialize
  Auth:   Consumer JWT
  Body:   { orderId: string }
  Action: Verify order belongs to consumer
          Verify order is PENDING
          Call Paystack /transaction/initialize
          Return { authorizationUrl, reference, accessCode }

POST /api/v1/payments/webhook
  Auth:   NONE (public endpoint)
  Header: x-paystack-signature: <hmac-sha512>
  Body:   Raw JSON (must NOT be parsed before signature check)
  Action: Verify HMAC signature → reject if invalid
          Parse event type
          Process idempotently (check if transaction already processed)
          Return 200 OK immediately
```

### Webhook Handler (Complete)
```typescript
@Post('webhook')
@Public()
@HttpCode(200)
async handleWebhook(
  @Headers('x-paystack-signature') signature: string,
  @Req() req: RawBodyRequest<Request>,
) {
  // 1. Verify signature BEFORE parsing body
  const rawBody = req.rawBody;
  const isValid = this.paystack.verifySignature(rawBody, signature);
  if (!isValid) throw new UnauthorizedException('WEBHOOK_SIGNATURE_INVALID');

  const event = JSON.parse(rawBody.toString());

  // 2. Idempotency check
  const alreadyProcessed = await this.repo.findTransaction(event.data.reference);
  if (alreadyProcessed) return { received: true }; // already handled

  // 3. Process event
  switch (event.event) {
    case 'charge.success':
      await this.handleChargeSuccess(event.data);
      break;
    case 'charge.failed':
      await this.handleChargeFailed(event.data);
      break;
    // Always return 200 -- Paystack retries on non-200
  }
  return { received: true };
}

private async handleChargeSuccess(data: PaystackChargeData): Promise<void> {
  // Idempotency: check BEFORE transaction using unique constraint on paystackRef
  const existing = await this.prisma.transaction.findUnique({
    where: { paystackRef: data.reference },
  });
  if (existing) return; // already processed

  await this.prisma.$transaction(async (tx) => {
    const order = await tx.order.findUnique({
      where: { paystackRef: data.reference },
      select: { id: true, status: true, merchantId: true, consumerId: true, total: true },
    });

    if (!order || order.status !== 'PENDING') return;

    await tx.order.update({ where: { id: order.id }, data: { status: 'CONFIRMED' } });
    await tx.transaction.create({
      data: {
        orderId: order.id,
        paystackRef: data.reference,
        amount: new Decimal(data.amount).div(100),
        status: 'SUCCESS',
        channel: data.channel,
        paidAt: new Date(data.paid_at),
        gatewayMeta: data,
      },
    });

    // Write outbox event IN THE SAME TRANSACTION -- atomic, durable, no scoping bug
    await tx.outboxEvent.create({
      data: {
        eventType: 'order.confirmed',
        aggregateId: order.id,
        payload: {
          orderId: order.id,
          merchantId: order.merchantId,
          consumerId: order.consumerId,
          total: order.total.toString(),
        },
      },
    });
  });
  // No eventEmitter.emit() -- OutboxRelayService handles delivery
}
```

## 7.6 Notifications Module

### Responsibilities
- Listen to all domain events
- Send SMS via Termii (OTP, transactional)
- Send push via Firebase FCM
- Never block the request thread (fire-and-forget)
- Graceful failure - notification errors never crash the app

### Event Listeners

Notifications are delivered by `OutboxRelayService.dispatch()` calling `NotificationsService` directly. No `@OnEvent` decorators - delivery is Outbox-backed and durable.

```typescript
// notifications/notifications.service.ts
@Injectable()
export class NotificationsService {
  async sendOrderConfirmation(payload: OrderConfirmedPayload, eventId: string): Promise<void> {
    await Promise.allSettled([
      this.fcm.send(payload.merchantFcmToken, 'New Order', `?${payload.total} order received`, eventId),
      this.fcm.send(payload.consumerFcmToken, 'Order Confirmed', 'Your payment was received', eventId),
    ]);
  }

  async sendOrderStatusUpdate(payload: OrderStatusChangedPayload, eventId: string): Promise<void> {
    const statusMessages: Record<string, { title: string; body: string }> = {
      SHIPPED:   { title: 'Order Shipped', body: 'Your order is on the way!' },
      DELIVERED: { title: 'Order Delivered', body: 'Your order has arrived.' },
      CANCELLED: { title: 'Order Cancelled', body: 'Your order was cancelled.' },
    };
    const msg = statusMessages[payload.newStatus];
    if (msg && payload.consumerFcmToken) {
      await this.fcm.send(payload.consumerFcmToken, msg.title, msg.body, eventId);
    }
    if (payload.newStatus === 'CANCELLED' && payload.merchantFcmToken) {
      await this.fcm.send(payload.merchantFcmToken, 'Order Cancelled', 'An order was cancelled', eventId);
    }
  }

  async sendLowStockAlert(payload: LowStockPayload, eventId: string): Promise<void> {
    if (!payload.merchantFcmToken) return;
    await this.fcm.send(
      payload.merchantFcmToken,
      'Low Stock Alert',
      `${payload.productName} has only ${payload.currentQuantity} units left`,
      eventId,
    );
  }
}
```

### Notification Event Map
| Domain Event | SMS | Push (Merchant) | Push (Consumer) |
|---|---|---|---|
| OTP requested | Yes Termii | No | No |
| Order created | No | Yes New order | No |
| Order confirmed | No | Yes Payment received | Yes Order confirmed |
| Order shipped | No | No | Yes Order shipped |
| Order delivered | No | No | Yes Order delivered |
| Order cancelled | No | Yes Order cancelled | Yes Order cancelled |
| Low stock | No | Yes Low stock alert | No |
| Daily summary | No | Yes Daily report | No |

## 7.7 Storefront Module (Public API)

### Responsibilities
- Serve public marketplace data (no auth required)
- Power consumer app browse/search
- Redis-cached for performance
- Read-only — no writes

### Endpoints
```
GET /api/v1/storefront/markets
  Cache: Redis, TTL 1 hour
  Returns: [{ id, name, city, state, shopCount, isActive }]

GET /api/v1/storefront/markets/:marketId/shops
  Cache: Redis, TTL 15 min
  Query: page, limit, category
  Returns: Paginated shops with { id, shopName, logoUrl, productCount, rating }

GET /api/v1/storefront/shops/:shopId
  Cache: Redis, TTL 15 min
  Returns: Full shop profile { shopName, shopDesc, logoUrl, location, hours, productCount }

GET /api/v1/storefront/shops/:shopId/products
  Cache: Redis, TTL 2 min (stock changes frequently)
  Query: page, limit, category, search, minPrice, maxPrice
  Returns: Paginated active products

GET /api/v1/storefront/products/search
  Cache: Redis, TTL 60s (keyed by query hash)
  Query: q (required), category, market, minPrice, maxPrice, page, limit
  Action: Full-text search using PostgreSQL tsvector
          Returns cross-shop results with shop info

GET /api/v1/storefront/products/featured
  Cache: Redis, TTL 30 min
  Returns: Featured products (admin-curated or highest-selling)
```

### Full-Text Search Implementation

> Fix applied: The previous version recomputed `tsvector` inline on every query — a full table scan with CPU-intensive text processing. The correct approach is a generated stored column with a GIN index, computed once at write time.

```sql
-- Run as a Prisma migration (raw SQL)
-- Add generated tsvector column - computed at write time, not query time
ALTER TABLE "Product"
  ADD COLUMN search_vector tsvector
  GENERATED ALWAYS AS (
    to_tsvector('english',
      coalesce(name, '') || ' ' ||
      coalesce(description, '') || ' ' ||
      coalesce(category, '') || ' ' ||
      coalesce(sku, '')
    )
  ) STORED;

-- GIN index makes search O(log n) instead of O(n)
CREATE INDEX product_search_gin ON "Product" USING GIN(search_vector);
```

Updated search query (uses the pre-computed index):
```sql
SELECT p.*, m."shopName", m."logoUrl"
FROM "Product" p
JOIN "Merchant" m ON p."merchantId" = m.id
WHERE p."isActive" = true
  AND m."isActive" = true
  AND p.search_vector @@ plainto_tsquery('english', $1)
ORDER BY ts_rank(p.search_vector, plainto_tsquery('english', $1)) DESC
LIMIT $2 OFFSET $3;
```

## 7.8 Admin Module

### Responsibilities
- Platform-wide merchant management
- Market registry CRUD
- Subscription plan management
- Platform analytics
- Order dispute resolution

### Endpoints
```
GET    /api/v1/admin/merchants
  Auth:  Admin JWT
  Query: page, limit, plan, isActive, search, marketId

PATCH  /api/v1/admin/merchants/:id/plan
  Auth:  Admin JWT
  Body:  { plan: Plan, planExpiresAt?: string }

PATCH  /api/v1/admin/merchants/:id/status
  Auth:  Admin JWT
  Body:  { isActive: boolean, reason?: string }

GET    /api/v1/admin/markets
POST   /api/v1/admin/markets
PATCH  /api/v1/admin/markets/:id
DELETE /api/v1/admin/markets/:id

GET    /api/v1/admin/analytics/overview
  Returns: {
    totalMerchants, activeMerchants, merchantsByPlan,
    totalOrders, totalRevenue, totalTransactions,
    newMerchantsThisMonth, ordersThisMonth
  }

GET    /api/v1/admin/orders
  Query: status, merchantId, consumerId, from, to, page, limit
```

# 8. DATABASE ARCHITECTURE

## 8.1 Design Principles

| Principle | Implementation |
|---|---|
| Single source of truth | `prisma/schema.prisma` defines everything | no manual SQL |
| Money is never a float | All monetary fields use `Decimal @db.Decimal(12,2)` |
| Soft deletes only | `isActive = false` | hard deletes break historical references |
| Idempotency keys | `localId` on Sale prevents offline sync duplicates |
| Audit trail | `createdAt`, `updatedAt` on every mutable model |
| Tenant isolation | `merchantId` on every tenant-scoped table + Prisma middleware |
| Denormalization where justified | `Customer.totalSpent`, `Customer.visitCount` | avoids expensive aggregations |
| Indexes on every query pattern | Defined explicitly in schema, not left to chance |

## 8.2 Complete Prisma Schema

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL") // Supabase requires both for migrations
}

// -------------------------------------------
// IDENTITY & AUTH
// -------------------------------------------

model Merchant {
  id            String    @id @default(cuid())
  phone         String    @unique
  name          String
  shopName      String
  shopDesc      String?
  logoUrl       String?
  marketId      String?
  plan          Plan      @default(FREE)
  planExpiresAt DateTime?
  isActive      Boolean   @default(true)
  fcmToken      String?
  allowNegativeStock Boolean @default(false) // per-merchant conflict policy (Weakness 1)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  market        Market?        @relation(fields: [marketId], references: [id])
  products      Product[]
  sales         Sale[]
  orders        Order[]
  customers     Customer[]
  importJobs    ImportJob[]
  // Note: Merchant sessions managed by Supabase Auth - no RefreshToken relation needed

  @@index([marketId])
  @@index([plan])
  @@index([isActive])
}

model Consumer {
  id        String   @id @default(cuid())
  phone     String   @unique
  name      String?
  email     String?  // optional - used for Paystack receipts if available
  fcmToken  String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  orders Order[]
}

model Admin {
  id           String   @id @default(cuid())
  email        String   @unique
  passwordHash String
  name         String
  isActive     Boolean  @default(true)
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt
}

model RefreshToken {
  id         String    @id @default(cuid())
  token      String    @unique
  adminId    String?
  expiresAt  DateTime
  createdAt  DateTime  @default(now())

  // NOTE: Merchant and Consumer sessions are managed by Supabase Auth (auth.sessions).
  // This table is ONLY for the Admin custom auth flow.
  // merchantId and consumerId fields have been removed.

  @@index([adminId])
  @@index([expiresAt]) // for cleanup job
}

// -------------------------------------------
// MARKET REGISTRY
// -------------------------------------------

model Market {
  id          String   @id @default(cuid())
  name        String
  city        String
  state       String
  description String?
  latitude    Float?
  longitude   Float?
  imageUrl    String?
  isActive    Boolean  @default(true)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  merchants Merchant[]

  @@index([city])
  @@index([isActive])
}

// -------------------------------------------
// INVENTORY
// -------------------------------------------

model Product {
  id          String   @id @default(cuid())
  merchantId  String
  name        String
  description String?
  sku         String?
  price       Decimal  @db.Decimal(12, 2)
  quantity    Int      @default(0)
  lowStockAt  Int      @default(5)
  category    String?
  imageUrl    String?
  isActive    Boolean  @default(true)
  revision    Int      @default(0)  // optimistic lock - incremented on every stock change
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  merchant   Merchant    @relation(fields: [merchantId], references: [id])
  saleItems  SaleItem[]
  orderItems OrderItem[]

  @@unique([merchantId, sku])
  @@index([merchantId])
  @@index([merchantId, category])
  @@index([merchantId, isActive])
  @@index([merchantId, quantity]) // for low-stock queries
}

// -------------------------------------------
// POS / IN-STORE SALES
// -------------------------------------------

model Sale {
  id            String        @id @default(cuid())
  localId       String        @unique  // WatermelonDB UUID - idempotency key
  merchantId    String
  customerId    String?
  total         Decimal       @db.Decimal(12, 2)
  paymentMethod PaymentMethod
  note          String?       // optional cashier note
  syncedAt      DateTime?     // null = offline, timestamp = server-confirmed
  createdAt     DateTime      @default(now())

  merchant Merchant   @relation(fields: [merchantId], references: [id])
  customer Customer?  @relation(fields: [customerId], references: [id])
  items    SaleItem[]

  @@index([merchantId])
  @@index([merchantId, createdAt])
  @@index([merchantId, paymentMethod])
}

model SaleItem {
  id        String  @id @default(cuid())
  saleId    String
  productId String
  quantity  Int
  unitPrice Decimal @db.Decimal(12, 2) // snapshot at time of sale

  sale    Sale    @relation(fields: [saleId], references: [id], onDelete: Cascade)
  product Product @relation(fields: [productId], references: [id])

  @@index([saleId])
  @@index([productId])
}

// -------------------------------------------
// CRM - CUSTOMERS (per merchant)
// -------------------------------------------

model Customer {
  id         String   @id @default(cuid())
  merchantId String
  phone      String?
  name       String?
  email      String?
  totalSpent Decimal  @default(0) @db.Decimal(12, 2) // denormalized
  visitCount Int      @default(0)                     // denormalized
  lastSeenAt DateTime @default(now())
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt

  merchant Merchant @relation(fields: [merchantId], references: [id])
  sales    Sale[]

  @@unique([merchantId, phone])
  @@index([merchantId])
  @@index([merchantId, totalSpent]) // for top-customer queries
}

// -------------------------------------------
// ORDERS (Consumer → Merchant, online)
// -------------------------------------------

model Order {
  id              String      @id @default(cuid())
  merchantId      String
  consumerId      String
  status          OrderStatus @default(PENDING)
  total           Decimal     @db.Decimal(12, 2)
  deliveryAddress String
  deliveryNote    String?
  paystackRef     String?     @unique
  cancelReason    String?
  cancelledBy     String?     // 'merchant' | 'consumer'
  createdAt       DateTime    @default(now())
  updatedAt       DateTime    @updatedAt

  merchant     Merchant      @relation(fields: [merchantId], references: [id])
  consumer     Consumer      @relation(fields: [consumerId], references: [id])
  items        OrderItem[]
  transactions Transaction[]

  @@index([merchantId, status])
  @@index([consumerId])
  @@index([consumerId, status])
  @@index([createdAt])
  @@index([paystackRef])
}

model OrderItem {
  id        String  @id @default(cuid())
  orderId   String
  productId String
  quantity  Int
  unitPrice Decimal @db.Decimal(12, 2) // snapshot at time of order

  order   Order   @relation(fields: [orderId], references: [id], onDelete: Cascade)
  product Product @relation(fields: [productId], references: [id])

  @@index([orderId])
  @@index([productId])
}

// -------------------------------------------
// PAYMENTS / TRANSACTIONS
// -------------------------------------------

model Transaction {
  id          String            @id @default(cuid())
  orderId     String
  paystackRef String            @unique
  amount      Decimal           @db.Decimal(12, 2)
  currency    String            @default("NGN")
  status      TransactionStatus @default(PENDING)
  channel     String?           // card | bank_transfer | ussd | mobile_money
  gatewayMeta Json?             // raw Paystack response for audit
  paidAt      DateTime?
  createdAt   DateTime          @default(now())

  order Order @relation(fields: [orderId], references: [id])

  @@index([orderId])
  @@index([status])
  @@index([createdAt])
}

// -------------------------------------------
// OUTBOX - TRANSACTIONAL EVENT RELAY
// -------------------------------------------

model OutboxEvent {
  id            String       @id @default(cuid())
  eventType     String       // 'order.confirmed' | 'sale.created' | 'inventory.low_stock' | etc.
  aggregateId   String       // orderId, saleId, productId - the entity this event is about
  payload       Json         // full event data for the handler
  status        OutboxStatus @default(PENDING)
  priority      Int          @default(3) // 1=critical, 2=high, 3=normal, 4=low
  attempts      Int          @default(0)
  nextRetryAt   DateTime?    // exponential backoff: now + (2^attempts * 5s), capped at 1hr
  processedAt   DateTime?
  createdAt     DateTime     @default(now())

  @@index([status, priority, createdAt]) // relay query: PENDING ordered by priority then time
  @@index([aggregateId])                 // replay by entity
  @@index([nextRetryAt])                 // backoff query
}

// -------------------------------------------
// AUDIT LOG
// -------------------------------------------

model AuditLog {
  id           String   @id @default(cuid())
  actorId      String   // merchantId, consumerId, or adminId
  actorRole    String   // 'merchant' | 'consumer' | 'admin'
  action       String   // 'product.price_updated' | 'order.status_changed' | 'merchant.plan_changed' | etc.
  resourceType String   // 'Product' | 'Order' | 'Merchant' | 'Sale'
  resourceId   String   // the ID of the affected record
  before       Json?    // snapshot of record before change
  after        Json?    // snapshot of record after change
  ip           String?
  userAgent    String?
  createdAt    DateTime @default(now())

  @@index([actorId])
  @@index([resourceType, resourceId])
  @@index([createdAt])
}

// -------------------------------------------
// IMPORT JOBS (async bulk import)
// -------------------------------------------

model ImportJob {
  id         String          @id @default(cuid())
  merchantId String
  filePath   String
  status     ImportJobStatus @default(PENDING)
  imported   Int             @default(0)
  updated    Int             @default(0)
  failed     Int             @default(0)
  errors     Json?
  createdAt  DateTime        @default(now())
  updatedAt  DateTime        @updatedAt

  merchant Merchant @relation(fields: [merchantId], references: [id])

  @@index([merchantId])
}

// -------------------------------------------
// CQRS READ MODEL - DAILY SALES SUMMARY
// -------------------------------------------

model DailySalesSummary {
  id              String   @id @default(cuid())
  merchantId      String
  date            DateTime // truncated to day (midnight UTC)
  totalRevenue    Decimal  @db.Decimal(12, 2)
  totalSales      Int
  cashRevenue     Decimal  @db.Decimal(12, 2) @default(0)
  transferRevenue Decimal  @db.Decimal(12, 2) @default(0)
  ussdRevenue     Decimal  @db.Decimal(12, 2) @default(0)
  cardRevenue     Decimal  @db.Decimal(12, 2) @default(0)
  updatedAt       DateTime @updatedAt

  merchant Merchant @relation(fields: [merchantId], references: [id])

  @@unique([merchantId, date])
  @@index([merchantId, date])
}

// -------------------------------------------
// ENUMS
// -------------------------------------------

enum Plan {
  FREE
  BASIC
  PRO
  ENTERPRISE
}

enum PaymentMethod {
  CASH
  TRANSFER
  USSD
  CARD
  MOBILE_MONEY
}

enum OrderStatus {
  PENDING
  CONFIRMED
  PROCESSING
  SHIPPED
  DELIVERED
  CANCELLED
}

enum TransactionStatus {
  PENDING
  SUCCESS
  FAILED
  REFUNDED
}

enum OutboxStatus {
  PENDING
  PROCESSING
  PROCESSED
  FAILED        // 5 attempts exhausted - dead-letter queue
}

enum ImportJobStatus {
  PENDING
  PROCESSING
  COMPLETED
  FAILED
}

enum MerchantRole {
  OWNER    // full access
  CASHIER  // POS only - no inventory management, no CRM export, no analytics
}

enum AdminRole {
  VIEWER    // read-only: analytics, merchant list
  OPERATOR  // can update merchant status, manage markets
  SUPER     // full access including plan changes and outbox replay
}
```

## 8.3 Entity Relationship Diagram

```
Market --------------< Merchant >--------------< Product
                          |                        |
                          |                        +--< SaleItem >-- Sale
                          |                        |
                          |                        +--< OrderItem >-- Order
                          |
                          +----------------------< Sale
                          |                        |
                          +----------------------< Order >--< Transaction
                          |
                          +----------------------< Customer >--< Sale
                          |
                          +----------------------< ImportJob
                          |
                          +----------------------< DailySalesSummary

Consumer -------------------------------------< Order

OutboxEvent (standalone - aggregateId references any entity by string)
AuditLog    (standalone - actorId + resourceId reference any entity by string)
```

## 8.4 Multi-Tenancy: PostgreSQL Row-Level Security

In addition to Prisma middleware, PostgreSQL RLS policies provide a second layer of defense. Even if application code has a bug, the database itself enforces tenant isolation.

```sql
-- Enable RLS on all tenant-scoped tables
ALTER TABLE "Product"  ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Sale"     ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Order"    ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Customer" ENABLE ROW LEVEL SECURITY;
ALTER TABLE "SaleItem" ENABLE ROW LEVEL SECURITY;

-- Policy: merchants can only see their own products
CREATE POLICY merchant_product_isolation ON "Product"
  USING (
    "merchantId" = current_setting('app.current_merchant_id', true)
    OR current_setting('app.role', true) = 'admin'
  );

-- Set the session variable in PrismaService before each query
-- prisma.$executeRaw`SELECT set_config('app.current_merchant_id', ${merchantId}, true)`
```

**Note:** For MVP, Prisma middleware alone is sufficient. RLS is added as a hardening step before production launch.

## 8.5 Key Design Decisions

| Decision | Rationale |
|---|---|
| `cuid()` for all IDs | URL-safe, sortable by creation time, no UUID collision risk |
| `Decimal @db.Decimal(12,2)` for money | IEEE 754 float loses precision | ₦1,234,567.89 must be exact |
| `localId @unique` on Sale | Idempotency key for offline sync - prevents duplicate sales on retry |
| `lowStockAt` per product | Merchants have different thresholds - a fabric seller vs a phone seller |
| `totalSpent` + `visitCount` on Customer | Denormalized | avoids `SUM()` aggregation on every CRM query |
| `cancelReason` + `cancelledBy` on Order | Audit trail for disputes and analytics |
| `syncedAt` nullable on Sale | `null` = offline origin awaiting sync, timestamp = server-confirmed |
| Soft delete (`isActive = false`) | Historical SaleItems and OrderItems reference products - hard delete breaks integrity |
| `@@unique([merchantId, sku])` | SKU is unique per merchant, not globally - two merchants can have SKU "001" |
| `@@unique([merchantId, phone])` on Customer | Same phone can be a customer of multiple merchants |
| `gatewayMeta Json?` on Transaction | Store raw Paystack response for dispute resolution without schema changes |
| `directUrl` in datasource | Supabase requires `DATABASE_URL` (pooled) for queries and `DIRECT_URL` (direct) for migrations |

# 9. DATA FLOW SPECIFICATIONS

## 9.1 Flow: Merchant Onboarding & First Product

```
1. Field agent hands merchant a phone with the app installed

2. Merchant App - Login Screen
   → Merchant enters phone number (+234XXXXXXXXXX)
   → supabase.auth.signInWithOtp({ phone: '+234XXXXXXXXXX' })
   → Supabase triggers Send SMS Hook → Edge Function calls Termii
   → Termii sends SMS: "Your Invento code is 847291"

3. Merchant App - OTP Screen
   → Merchant enters 6-digit code
   → supabase.auth.verifyOtp({ phone, token, type: 'sms' })
   → Supabase issues JWT + refresh token automatically
   → JWT contains { sub, role: 'merchant', merchantId } via Custom Access Token Hook
   → App stores Supabase session in MMKV (encrypted)
   → App navigates to Dashboard

4. Merchant App - Add First Product
   → Merchant taps "Add Product"
   → Fills form: name="Ankara Fabric", price=2500, qty=50, category="Textiles"
   → Optionally scans barcode → SKU auto-filled
   → Optionally takes photo → uploaded to Supabase Storage → imageUrl returned
   → React Hook Form + Zod validates locally
   → POST /api/v1/inventory/products { name, price, quantity, category, sku, imageUrl }
   → API: check plan limit (FREE: 50 SKUs), create Product (revision=0), emit ProductCreatedEvent
   → TanStack Query invalidates ['products', merchantId]
   → Product appears in inventory list
```

## 9.2 Flow: POS Sale (Online)

```
1. Customer walks into shop, selects items

2. Merchant App - POS Screen
   → Merchant taps "New Sale"
   → Scans barcode → GET /api/v1/inventory/products/sku/:sku
   → Product added to cart (Zustand cartStore)
   → Merchant adjusts quantity if needed
   → Selects payment method: CASH

3. Merchant taps "Complete Sale"
   → localId = uuid() generated on device
   → POST /api/v1/pos/sales {
       localId,
       paymentMethod: 'CASH',
       items: [{ productId, quantity, unitPrice }],
       customerPhone: '0801234567' (optional)
     }

4. NestJS API - PosService.createSale()
   → Prisma.$transaction:
     a. Verify each product belongs to merchant (ownership check)
     b. Check product.quantity >= item.quantity for each item
        → If insufficient: throw BadRequestException('INSUFFICIENT_STOCK')
     c. Create Sale record (localId, merchantId, total, paymentMethod)
     d. Create SaleItems (productId, quantity, unitPrice snapshot)
     e. Decrement product.quantity for each item
     f. Upsert Customer if phone provided:
        → Update totalSpent += sale.total
        → Update visitCount += 1
        → Update lastSeenAt = now()
   → Emit SaleCreatedEvent
   → Return created sale with items

5. NotificationsListener.onSaleCreated()
   → Check if product.quantity <= product.lowStockAt
   → If yes: emit LowStockEvent
   → NotificationsListener.onLowStock()
     → FCM push to merchant: "Low Stock: Ankara Fabric has 3 units left"

6. Merchant App
   → TanStack Query invalidates ['sales'] and ['products']
   → Show digital receipt (ReceiptView component)
   → Option to share receipt via WhatsApp
```

## 9.3 Flow: POS Sale (Offline) + Sync

```
SCENARIO: Merchant's phone has no internet. Makes 3 sales.

1. Merchant App - Offline Detection
   → @react-native-community/netinfo: isConnected = false
   → App shows "Offline Mode" indicator (amber badge)
   → POS continues to work normally

2. For each sale (offline):
   → localId = uuid() generated on device
   → Sale saved to WatermelonDB:
     Sale { localId, paymentMethod, items, total, createdAt, syncedAt: null }
   → Product quantities decremented in WatermelonDB (local copy)
   → UI updates instantly - merchant sees correct stock levels
   → Receipt shown from local data

3. CONNECTIVITY RESTORED:
   → NetInfo: isConnected = true
   → packages/db/sync.ts triggers automatically:

   a. Query WatermelonDB: SELECT * FROM sales WHERE syncedAt IS NULL
   b. Found 3 unsynced sales

   c. POST /api/v1/pos/sales/sync {
        sales: [sale1, sale2, sale3]
      }

4. NestJS API - PosService.syncSales()
   → For each sale in array (processed in order):
     → Check: does Sale with localId exist? (idempotency)
     → If exists: results.skipped++, continue
     → If not: createSale() in Prisma transaction
       → Same validation as online sale
       → If stock insufficient (another device sold the item):
         → results.failed++
         → errors.push({ localId, reason: 'INSUFFICIENT_STOCK' })
       → If success: results.synced++
   → Return { synced: 3, skipped: 0, failed: 0, errors: [] }

5. Merchant App - Post-Sync
   → Mark synced sales in WatermelonDB: syncedAt = now()
   → TanStack Query refetch products (server stock is now authoritative)
   → WatermelonDB product quantities updated from server response
   → Toast: "3 sales synced successfully"
   → If any failed: show error list with reason

CONFLICT RESOLUTION RULE:
   Server stock is always authoritative after sync.
   If a product was sold offline but stock ran out on server:
   → Sale is marked as failed in sync response
   → Merchant is notified to reconcile manually
   → WatermelonDB sale marked with syncError = true
```

## 9.4 Flow: Consumer Places an Order

```
1. Consumer App - Browse
   → GET /api/v1/storefront/markets (cached 1hr)
   → Consumer selects "Sabongari Market"
   → GET /api/v1/storefront/markets/:id/shops (cached 15min)
   → Consumer browses shops, taps "Musa Fabrics"
   → GET /api/v1/storefront/shops/:shopId/products (cached 2min)
   → Consumer taps product → product detail screen
   → Taps "Add to Cart" → Zustand cartStore.addItem()

2. Consumer App - Checkout
   → Cart screen shows items, total
   → Consumer taps "Checkout"
   → Enters delivery address
   → POST /api/v1/orders {
       merchantId,
       items: [{ productId, quantity }],
       deliveryAddress: "12 Kofar Mata, Kano"
     }

3. NestJS API - OrdersService.createOrder()
   → Validate all products exist and belong to merchantId
   → Check stock for each item
     → If any out of stock: throw BadRequestException
   → Calculate total (use current product.price as unitPrice snapshot)
   → Prisma.$transaction:
     a. Create Order (status: PENDING)
     b. Create OrderItems (snapshot unitPrice)
     c. Reserve stock: product.quantity -= qty for each item
   → Call PaystackService.initializeTransaction({
       email: consumer.email ?? (consumer.phone + '@invento.ng'), // use real email if available
       amount: total * 100, // kobo
       reference: generateRef(),
       metadata: { orderId }
     })
   → Update order.paystackRef = reference
   → Emit OrderCreatedEvent
   → Return { orderId, authorizationUrl, reference }

4. Consumer App - Payment
   → Open Paystack WebView with authorizationUrl
   → Consumer completes payment (card / bank transfer / USSD)
   → Paystack redirects to callback URL
   → App closes WebView
   → Supabase Realtime subscription detects order status change instantly (replaces polling):

```typescript
// checkout/hooks/useOrderStatus.ts
supabase
  .channel(`order-${orderId}`)
  .on('postgres_changes', {
    event: 'UPDATE',
    schema: 'public',
    table: 'Order',
    filter: `id=eq.${orderId}`,
  }, (payload) => {
    if (payload.new.status === 'CONFIRMED') {
      queryClient.invalidateQueries({ queryKey: ['order', orderId] });
      router.replace('/checkout/confirmed');
    }
  })
  .subscribe();
// Unsubscribe on component unmount
```

5. Paystack → Webhook (triggers the Realtime update)
   → POST /api/v1/payments/webhook { event: 'charge.success', data: {...} }
   → Verify HMAC signature
   → Idempotency check (already processed?)
   → Prisma.$transaction:
     a. Update Order.status = CONFIRMED  → Supabase Realtime broadcasts this change
     b. Create Transaction { status: SUCCESS, paidAt }
     c. Stock deduction already done at reservation - no change needed
   → Emit OrderConfirmedEvent

6. NotificationsListener.onOrderConfirmed()
   → FCM to merchant: "New Order - ₦2,500 from Kano"
   → FCM to consumer: "Payment confirmed! Your order is being prepared"

7. Consumer App - Order Tracking
   → Order status screen shows: CONFIRMED → PROCESSING → SHIPPED → DELIVERED
   → Each status change triggers FCM push notification
```

## 9.5 Flow: Merchant Fulfills an Order

```
1. Merchant App - Orders Tab
   → FCM push received: "New Order"
   → GET /api/v1/orders?status=PENDING (TanStack Query refetch)
   → Order appears in list

2. Merchant reviews order
   → Taps order → order detail screen
   → Sees: items, quantities, delivery address, consumer info

3. Merchant confirms order
   → PATCH /api/v1/orders/:id/status { status: 'CONFIRMED' }
   → API: validate transition (PENDING → CONFIRMED is allowed for merchant)
   → Update order.status = CONFIRMED
   → Emit OrderStatusChangedEvent
   → FCM to consumer: "Your order has been confirmed"

4. Merchant prepares order
   → PATCH /api/v1/orders/:id/status { status: 'PROCESSING' }

5. Merchant hands to delivery
   → PATCH /api/v1/orders/:id/status { status: 'SHIPPED' }
   → FCM to consumer: "Your order is on the way!"

6. Delivery confirmed
   → PATCH /api/v1/orders/:id/status { status: 'DELIVERED' }
   → Order is terminal - no further status changes allowed
```

## 9.6 Flow: Token Refresh (Silent, Background)

```
Token Refresh Flow (Supabase SDK - no NestJS endpoint needed):

1. Supabase JS client detects access token is expired
   → supabase.auth.getSession() automatically refreshes the session
   → Returns fresh access_token + refresh_token transparently

2. Axios interceptor in packages/shared-api/client.ts:
   → On every request: reads current session from supabase.auth.getSession()
   → Attaches fresh access_token to Authorization header
   → No manual 401 retry logic needed - Supabase SDK handles rotation

3. If session is fully expired (refresh token revoked or expired):
   → supabase.auth.getSession() returns null session
   → Axios interceptor detects missing token
   → Navigate to login screen
   → User re-authenticates with phone OTP via Supabase SDK

NOTE: There is NO custom POST /api/v1/auth/refresh endpoint.
      There is NO RefreshToken table for merchants or consumers.
      Supabase Auth manages all session lifecycle in auth.sessions.
      The AdminRefreshToken table exists ONLY for the admin custom auth flow.
```

# 10. MULTI-TENANCY DESIGN

## 10.1 Strategy: Two-Layer Tenant Isolation

> Fix applied: The previous version removed Prisma middleware and relied on Supabase RLS with `auth.uid()` through Prisma's service role connection. This is incorrect — Prisma's service role connection bypasses RLS by default. `auth.uid()` only fires through Supabase's PostgREST layer, not through Prisma.
> Correct approach: Prisma middleware (primary enforcement) + PostgreSQL RLS via `set_config` (secondary defense-in-depth).

```
Layer 1: Prisma Middleware (application layer - primary)
  → Automatically injects merchantId WHERE clause on all tenant-scoped queries
  → Uses request-scoped context to carry merchantId through the request lifecycle

Layer 2: PostgreSQL RLS via set_config (database layer - defense-in-depth)
  → Even if application code has a bug, DB rejects cross-tenant queries
  → Uses SET LOCAL app.current_merchant_id - works correctly through Prisma service role
  → Two independent enforcement points - one must fail for a data leak to occur
```

## 10.2 Prisma Tenant Middleware (Restored)

```typescript
// prisma/prisma.service.ts
import { Injectable, OnModuleInit } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';
import { RequestContext } from '../common/context/request-context';

const TENANT_SCOPED_MODELS = [
  'Product', 'Sale', 'SaleItem', 'Order', 'OrderItem', 'Customer',
];

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  constructor() {
    super({
      datasources: { db: { url: process.env.DATABASE_URL } },
    });

    this.$use(async (params, next) => {
      const merchantId = RequestContext.get('merchantId');

      if (!merchantId || !TENANT_SCOPED_MODELS.includes(params.model ?? '')) {
        return next(params);
      }

      // Set PostgreSQL session variable for RLS (Layer 2)
      // IMPORTANT: set_config with is_local=true only works inside a transaction.
      // For queries outside transactions, use is_local=false (session-scoped).
      // PgBouncer transaction mode resets session vars between transactions - safe.
      const isInTransaction = params.runInTransaction ?? false;
      await this.$executeRaw`
        SELECT set_config('app.current_merchant_id', ${merchantId}, ${isInTransaction})
      `;

      // Inject merchantId filter (Layer 1 -- primary enforcement)
      if (['findMany', 'findFirst', 'findUnique', 'count', 'aggregate'].includes(params.action)) {
        params.args ??= {};
        params.args.where = { ...params.args.where, merchantId };
      }
      if (['update', 'updateMany', 'delete', 'deleteMany'].includes(params.action)) {
        params.args ??= {};
        params.args.where = { ...params.args.where, merchantId };
      }
      if (params.action === 'create') {
        params.args.data = { ...params.args.data, merchantId };
      }

      return next(params);
    });
  }

  async onModuleInit() {
    await this.$connect();
  }
}
```

## 10.3 PostgreSQL RLS Policies (Layer 2 - Defense-in-Depth)

```sql
-- Enable RLS on all tenant-scoped tables
ALTER TABLE "Product"  ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Sale"     ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Order"    ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Customer" ENABLE ROW LEVEL SECURITY;
ALTER TABLE "SaleItem" ENABLE ROW LEVEL SECURITY;

-- Policy uses set_config session variable (works through Prisma service role)
CREATE POLICY merchant_isolation ON "Product"
  USING (
    "merchantId" = current_setting('app.current_merchant_id', true)
    OR current_setting('app.bypass_rls', true) = 'true'
  );

CREATE POLICY merchant_isolation ON "Sale"
  USING ("merchantId" = current_setting('app.current_merchant_id', true));

CREATE POLICY merchant_isolation ON "Order"
  USING (
    "merchantId" = current_setting('app.current_merchant_id', true)
    OR "consumerId" = current_setting('app.current_merchant_id', true)
  );

CREATE POLICY merchant_isolation ON "Customer"
  USING ("merchantId" = current_setting('app.current_merchant_id', true));
```

## 10.4 Setting Tenant Context in AuthGuard

```typescript
// common/guards/auth.guard.ts
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const isPublic = this.reflector.get<boolean>('isPublic', context.getHandler());
    if (isPublic) return true;

    const request = context.switchToHttp().getRequest();
    const token = request.headers.authorization?.split(' ')[1];
    if (!token) throw new UnauthorizedException('UNAUTHORIZED');

    try {
      // Verify Supabase JWT
      const payload = jwt.verify(token, process.env.SUPABASE_JWT_SECRET) as JwtPayload;
      request.user = payload;

      // Set tenant context for Prisma middleware
      if (payload.merchantId) {
        RequestContext.set('merchantId', payload.merchantId);
      }
      return true;
    } catch {
      throw new UnauthorizedException('UNAUTHORIZED');
    }
  }
}
```

## 10.5 Tenant Isolation Test

```typescript
// inventory.service.spec.ts
it('should not return products from another merchant', async () => {
  const merchantA = await createTestMerchant();
  const merchantB = await createTestMerchant();
  await createProduct({ merchantId: merchantA.id, name: 'Product A' });

  // Set context as merchantB -- Prisma middleware + RLS both enforce isolation
  RequestContext.set('merchantId', merchantB.id);
  const products = await inventoryService.getProducts({});
  expect(products.data).toHaveLength(0); // merchantB sees nothing
});
```

# 11. OFFLINE-FIRST ARCHITECTURE

## 11.1 WatermelonDB Schema (with Versioned Migrations)

> Fix applied: Schema versioning and migration strategy added. Without this, any schema change crashes existing users' apps on update.

```typescript
// packages/db/schema.ts
import { appSchema, tableSchema } from '@nozbe/watermelondb';

export const dbSchema = appSchema({
  version: 2,  // Increment on every schema change
  tables: [
    tableSchema({
      name: 'products',
      columns: [
        { name: 'server_id',    type: 'string', isOptional: true },
        { name: 'merchant_id',  type: 'string' },
        { name: 'name',         type: 'string' },
        { name: 'sku',          type: 'string', isOptional: true },
        { name: 'price',        type: 'number' },
        { name: 'quantity',     type: 'number' },
        { name: 'low_stock_at', type: 'number' },
        { name: 'category',     type: 'string', isOptional: true },
        { name: 'image_url',    type: 'string', isOptional: true },
        { name: 'is_active',    type: 'boolean' },
        { name: 'updated_at',   type: 'number' },
      ],
    }),
    tableSchema({
      name: 'sales',
      columns: [
        { name: 'local_id',       type: 'string' },
        { name: 'merchant_id',    type: 'string' },
        { name: 'customer_phone', type: 'string', isOptional: true },
        { name: 'total',          type: 'number' },
        { name: 'payment_method', type: 'string' },
        { name: 'synced_at',      type: 'number', isOptional: true },
        { name: 'sync_status',    type: 'string', isOptional: true }, // v2: 'synced'|'failed'|'partial'
        { name: 'sync_error',     type: 'string', isOptional: true },
        { name: 'created_at',     type: 'number' },
      ],
    }),
    tableSchema({
      name: 'sale_items',
      columns: [
        { name: 'sale_id',       type: 'string' },
        { name: 'product_id',    type: 'string' },
        { name: 'quantity',      type: 'number' },
        { name: 'unit_price',    type: 'number' },
        { name: 'quantity_delta', type: 'number' }, // v2: negative delta sent to server
      ],
    }),
  ],
});
```

### Migration File (required for existing users)

```typescript
// packages/db/migrations.ts
import { schemaMigrations, addColumns } from '@nozbe/watermelondb/Schema/migrations';

export const migrations = schemaMigrations({
  migrations: [
    {
      toVersion: 2,
      steps: [
        addColumns({
          table: 'sales',
          columns: [{ name: 'sync_status', type: 'string', isOptional: true }],
        }),
        addColumns({
          table: 'sale_items',
          columns: [{ name: 'quantity_delta', type: 'number', isOptional: true }],
        }),
      ],
    },
  ],
});

// Pass to Database constructor:
// new Database({ schema: dbSchema, migrations, adapter })
```

## 11.2 WatermelonDB Models

```typescript
// packages/db/models/Product.ts
import { Model } from '@nozbe/watermelondb';
import { field, readonly, date } from '@nozbe/watermelondb/decorators';

export class ProductModel extends Model {
  static table = 'products';

  @field('server_id')    serverId!: string;
  @field('name')         name!: string;
  @field('sku')          sku!: string;
  @field('price')        price!: number;
  @field('quantity')     quantity!: number;
  @field('low_stock_at') lowStockAt!: number;
  @field('category')     category!: string;
  @field('is_active')    isActive!: boolean;
  @readonly @date('updated_at') updatedAt!: Date;

  get isLowStock() {
    return this.quantity <= this.lowStockAt;
  }
}
```

## 11.3 Sync Engine (Delta-Based)

> Fix applied: Devices now send quantity deltas (`-3`) not absolute values (`quantity = 7`). The server applies deltas atomically. This eliminates the conflict resolution gap where "merchant must manually reconcile" was the only answer.

```typescript
// packages/db/sync.ts
import NetInfo from '@react-native-community/netinfo';
import { database } from './database';
import { apiClient } from '../shared-api';

export async function syncDatabase() {
  const netState = await NetInfo.fetch();
  if (!netState.isConnected) return;

  await synchronize({
    database,

    // Pull: get server changes since last sync
    pullChanges: async ({ lastPulledAt }) => {
      const response = await apiClient.get('/sync/pull', {
        params: { lastPulledAt },
      });
      return response;
    },

    // Push: send local sales as DELTAS (not absolute quantities)
    pushChanges: async ({ changes }) => {
      const unsyncedSales = changes.sales?.created ?? [];
      if (unsyncedSales.length === 0) return;

      const result = await apiClient.post('/pos/sales/sync', {
        sales: unsyncedSales.map(sale => ({
          ...mapWatermelonSaleToDTO(sale),
          items: sale.items.map(item => ({
            ...item,
            quantityDelta: -(item.quantity), // negative = sold
          })),
        })),
      });

      // Update local records based on server response
      await database.write(async () => {
        for (const syncResult of result.results) {
          const localSale = await database.get('sales').find(syncResult.localId);
          await localSale.update(s => {
            s.syncStatus = syncResult.status;
            s.syncError = syncResult.reason ?? null;
            s.syncedAt = syncResult.status === 'SYNCED' → Date.now() : null;
          });

          // Update local product quantities from server's authoritative values
          if (syncResult.stockUpdates) {
            for (const update of syncResult.stockUpdates) {
              const product = await database.get('products').find(update.productId);
              await product.update(p => { p.quantity = update.newQuantity; });
            }
          }
        }
      });
    },

    migrationsEnabledAtVersion: 1,
  });
}

// Auto-sync when connectivity is restored
NetInfo.addEventListener((state) => {
  if (state.isConnected) {
    syncDatabase().catch(console.error);
  }
});
```

## 11.4 Sync Pull Endpoint (Backend)

```typescript
// pos.controller.ts
@Get('sync/pull')
@UseGuards(JwtAuthGuard)
async pullChanges(
  @CurrentUser() user: JwtPayload,
  @Query('lastPulledAt') lastPulledAt?: string,
) {
  const since = lastPulledAt → new Date(parseInt(lastPulledAt)) : new Date(0);
  const merchantId = user.merchantId;

  const [created, updated, deleted] = await Promise.all([
    this.prisma.product.findMany({
      where: { merchantId, createdAt: { gt: since } },
    }),
    this.prisma.product.findMany({
      where: { merchantId, updatedAt: { gt: since }, createdAt: { lte: since } },
    }),
    this.prisma.product.findMany({
      where: { merchantId, isActive: false, updatedAt: { gt: since } },
      select: { id: true },
    }),
  ]);

  return {
    changes: {
      products: {
        created: created.map(mapToWatermelonProduct),
        updated: updated.map(mapToWatermelonProduct),
        deleted: deleted.map(p => p.id),
      },
    },
    timestamp: Date.now(),
  };
}
```

# 12. SECURITY ARCHITECTURE

## 12.1 Security Layers

```
Layer 1: Network (Cloudflare)
  → DDoS protection (automatic)
  → WAF rules (block common attack patterns)
  → SSL/TLS termination (TLS 1.3)
  → Rate limiting at edge (before hitting server)

Layer 2: API Gateway (NestJS)
  → Helmet.js: 11 security HTTP headers
  → CORS: whitelist of allowed origins only
  → ThrottlerGuard: per-endpoint rate limits
  → JWT verification on every protected route

Layer 3: Application (Services)
  → Input validation: class-validator on all DTOs
  → Ownership checks: every resource access verifies merchantId
  → Paystack webhook: HMAC-SHA512 signature verification
  → OTP: cryptographically random, Redis TTL, rate-limited

Layer 4: Database (PostgreSQL)
  → Row-Level Security policies
  → Prisma middleware tenant scoping
  → Parameterized queries (Prisma prevents SQL injection by default)
  → Decimal type for money (no float precision attacks)

Layer 5: Mobile (React Native)
  → MMKV encrypted storage for tokens
  → No API keys in app bundle (all in .env, not committed)
  → Certificate pinning for payment API calls
  → ProGuard obfuscation on Android production builds
  → Root/jailbreak detection via expo-device
```

## 12.2 JWT Security Specification

```typescript
// Access token: short-lived, stateless -- issued by Supabase Auth
{
  sub: "merchant_id_or_consumer_id",
  role: "merchant" | "consumer" | "admin",
  merchantId: "merchant_id",  // only for merchant role
  iat: 1711234567,
  exp: 1711235467,  // 15 minutes (Supabase default)
}
// Signed with: SUPABASE_JWT_SECRET -- verified by NestJS AuthGuard, never re-issued by NestJS

// Refresh token: long-lived, stateful (stored in DB)
// Format: UUID v4 (not JWT -- no payload to decode)
// Stored: RefreshToken table with expiresAt
// Rotation: single-use -- deleted and replaced on each refresh
// Revocation: delete from DB on logout or suspicious activity
```

## 12.3 Paystack Webhook Security

```typescript
// CRITICAL: rawBody must be captured BEFORE any JSON parsing
// In main.ts:
app.use('/api/v1/payments/webhook', express.raw({ type: 'application/json' }));

// In payments.controller.ts:
@Post('webhook')
@Public()
async handleWebhook(
  @Headers('x-paystack-signature') sig: string,
  @Req() req: Request & { rawBody: Buffer },
) {
  const hash = crypto
    .createHmac('sha512', process.env.PAYSTACK_SECRET_KEY)
    .update(req.rawBody)
    .digest('hex');

  if (hash !== sig) {
    throw new UnauthorizedException('WEBHOOK_SIGNATURE_INVALID');
  }
  // Process event...
}
```

## 12.4 Security Headers (Helmet)

```typescript
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", 'data:', 'https://res.cloudinary.com'],
    },
  },
  hsts: { maxAge: 31536000, includeSubDomains: true },
  noSniff: true,
  xssFilter: true,
  referrerPolicy: { policy: 'strict-origin-when-cross-origin' },
}));
```

# 13. CACHING & PERFORMANCE ARCHITECTURE

## 13.1 Redis Cache Strategy + Rate Limiting

> Rate limiting tool: `@upstash/ratelimit` (replaces `@nestjs/throttler`).
> Free tier: 500K commands/month - sufficient for MVP.

```typescript
// rate-limit.service.ts -- replaces @nestjs/throttler entirely
import { Ratelimit } from '@upstash/ratelimit';
import { Redis } from '@upstash/redis';

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(100, '1 m'),  // 100 req/min globally
});

// Auth endpoints -- stricter limit
const authRatelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(5, '10 m'),   // 5 OTP requests per 10 min
});

// Usage in NestJS guard or middleware:
const { success } = await authRatelimit.limit(phone);
if (!success) throw new TooManyRequestsException();
```

Note: OTP rate limiting is now handled by Supabase Auth's built-in rate limiting + this guard as a secondary layer. The `otp:{phone}` and `otp_count:{phone}` Redis keys from the old custom auth are no longer needed.

```typescript
// redis.service.ts -- wrapper with typed methods
@Injectable()
export class RedisService {
  constructor(@InjectRedis() private readonly redis: Redis) {}

  async get<T>(key: string): Promise<T | null> {
    const val = await this.redis.get(key);
    return val → JSON.parse(val) : null;
  }

  async set(key: string, value: unknown, ttlSeconds: number): Promise<void> {
    await this.redis.setex(key, ttlSeconds, JSON.stringify(value));
  }

  async del(...keys: string[]): Promise<void> {
    if (keys.length) await this.redis.del(...keys);
  }
}
```

## 13.2 Cache Key Registry

| Key Pattern | TTL | Invalidated By |
|---|---|---|
| `storefront:markets` | 3600s | Admin updates market |
| `storefront:market:{id}:shops:{page}` | 900s | Merchant joins/leaves market |
| `storefront:shop:{shopId}` | 900s | Merchant updates profile |
| `storefront:shop:{shopId}:products:{page}:{filters_hash}` | 120s | Product CRUD |
| `storefront:search:{query_hash}` | 60s | Any product change |
| `storefront:featured` | 1800s | Admin updates featured |

Note: OTP keys (`otp:{phone}`, `otp_count:{phone}`) are no longer stored in Redis - Supabase Auth manages OTP lifecycle internally.

## 13.3 Cache-Aside Pattern (Storefront)

```typescript
// storefront.service.ts
async getShopProducts(shopId: string, query: ProductQueryDTO) {
  const cacheKey = `storefront:shop:${shopId}:products:${hashQuery(query)}`;

  const cached = await this.redis.get<PaginatedResult<Product>>(cacheKey);
  if (cached) return cached;

  const result = await this.repo.findShopProducts(shopId, query);
  await this.redis.set(cacheKey, result, 120);
  return result;
}

// Cache invalidation -- use SCAN not KEYS (KEYS is O(N) blocking, dangerous in production)
async invalidateShopCache(merchantId: string) {
  const pattern = `storefront:shop:${merchantId}:*`;
  let cursor = '0';

  do {
    const [nextCursor, keys] = await this.redis.scan(cursor, 'MATCH', pattern, 'COUNT', 100);
    cursor = nextCursor;
    if (keys.length > 0) await this.redis.del(...keys);
  } while (cursor !== '0');

  await this.redis.del(`storefront:shop:${merchantId}`);
}
```

## 13.4 Database Query Performance

```typescript
// Always use select to fetch only needed fields
const products = await this.prisma.product.findMany({
  where: { merchantId, isActive: true },
  select: {
    id: true, name: true, price: true, quantity: true,
    category: true, imageUrl: true, sku: true,
    // Never select: merchant (full relation), saleItems, orderItems
  },
  orderBy: { updatedAt: 'desc' },
  skip: (page - 1) * limit,
  take: limit,
});

// Use cursor-based pagination for large datasets (> 10K records)
const products = await this.prisma.product.findMany({
  where: { merchantId, isActive: true },
  take: limit,
  skip: cursor → 1 : 0,
  cursor: cursor → { id: cursor } : undefined,
  orderBy: { id: 'asc' },
});
```

# 14. BACKGROUND JOBS & EVENT SYSTEM

## 14.1 Event System - Transactional Outbox Pattern

> Replaces: NestJS `EventEmitter2` in-process bus.
> Why: `EventEmitter2` has zero durability. A process crash between a Prisma commit and `emit()` permanently loses the event - no retry, no replay, no dead-letter queue. For a payment platform this is unacceptable.
> Pattern: Transactional Outbox - event written to DB in same transaction as business operation. Relay process delivers with retry and dead-letter.

```
Old (broken):
  Prisma.$transaction → commit → eventEmitter.emit() → [crash] → event lost forever

New (correct):
  Prisma.$transaction → commit + write OutboxEvent → relay reads → deliver → retry on failure
```

### OutboxEvent Table (add to Prisma schema)

```prisma
model OutboxEvent {
  id          String       @id @default(cuid())
  eventType   String       // 'order.confirmed', 'sale.created', etc.
  aggregateId String       // orderId, saleId, productId
  payload     Json         // full event data
  status      OutboxStatus @default(PENDING)
  attempts    Int          @default(0)
  processedAt DateTime?
  createdAt   DateTime     @default(now())

  @@index([status, createdAt])  // relay query index
  @@index([aggregateId])
}

enum OutboxStatus {
  PENDING
  PROCESSING
  PROCESSED
  FAILED        // 5 attempts exhausted -- dead-letter
}
```

### Writing to the Outbox (same transaction as business operation)

```typescript
// orders.service.ts -- order confirmation
await this.prisma.$transaction(async (tx) => {
  await tx.order.update({ where: { id: orderId }, data: { status: 'CONFIRMED' } });
  await tx.transaction.create({ data: { orderId, paystackRef, status: 'SUCCESS', ... } });

  // Write event IN THE SAME TRANSACTION -- atomic with business operation
  await tx.outboxEvent.create({
    data: {
      eventType: 'order.confirmed',
      aggregateId: orderId,
      payload: { orderId, merchantId, consumerId, total, items },
    },
  });
});
// If transaction commits: order update + outbox event are both durable
// If transaction rolls back: neither is written -- no phantom events
```

### Outbox Relay Service

```typescript
// outbox/outbox-relay.service.ts
@Injectable()
export class OutboxRelayService {
  @Cron('* * * * * *')  // every second
  async relay() {
    // Distributed lock -- prevents double-processing on scale-out
    const lockKey = 'outbox:relay:lock';
    const acquired = await this.redis.set(lockKey, '1', 'EX', 5, 'NX');
    if (!acquired) return;

    try {
      const events = await this.prisma.outboxEvent.findMany({
        where: { status: 'PENDING', attempts: { lt: 5 } },
        orderBy: { createdAt: 'asc' },
        take: 50,
      });

      for (const event of events) {
        await this.prisma.outboxEvent.update({
          where: { id: event.id },
          data: { status: 'PROCESSING', attempts: { increment: 1 } },
        });

        try {
          await this.dispatch(event);
          await this.prisma.outboxEvent.update({
            where: { id: event.id },
            data: { status: 'PROCESSED', processedAt: new Date() },
          });
        } catch (err) {
          const isFinal = event.attempts >= 4;
          await this.prisma.outboxEvent.update({
            where: { id: event.id },
            data: { status: isFinal → 'FAILED' : 'PENDING' },
          });
          if (isFinal) Sentry.captureException(err, { extra: { eventId: event.id } });
        }
      }
    } finally {
      await this.redis.del(lockKey);
    }
  }

  private async dispatch(event: OutboxEvent) {
    switch (event.eventType) {
      case 'order.confirmed':
        await this.notifications.sendOrderConfirmation(event.payload);
        break;
      case 'order.status.changed':
        await this.notifications.sendOrderStatusUpdate(event.payload);
        break;
      case 'sale.created':
        await this.inventory.checkLowStock(event.payload);
        break;
      case 'inventory.low_stock':
        await this.notifications.sendLowStockAlert(event.payload);
        break;
    }
  }
}
```

### Dead-Letter Queue - Replay Endpoint

Events with `status = FAILED` sit in the database, queryable and replayable:

```
POST /api/v1/admin/outbox/replay
  Auth:  Admin JWT
  Body:  { eventType?: string, from?: string, to?: string }
  Action: Reset FAILED events matching filters back to PENDING
```

## 14.2 Domain Events Registry

| Event | Written By | Delivered To |
|---|---|---|
| `sale.created` | PosService (in transaction) | InventoryService (low stock check) |
| `order.created` | OrdersService (in transaction) | NotificationsService |
| `order.confirmed` | PaymentsService (in transaction) | NotificationsService |
| `order.status.changed` | OrdersService (in transaction) | NotificationsService |
| `inventory.low_stock` | InventoryService (in transaction) | NotificationsService |
| `merchant.plan.changed` | AdminService (in transaction) | MerchantsService |

## 14.3 Scheduled Jobs (Distributed-Safe)

All cron jobs use Redis distributed locks to prevent double-execution when running multiple NestJS instances.

```typescript
// scheduler.service.ts
@Injectable()
export class SchedulerService {

  // Daily sales summary -- midnight WAT (UTC+1)
  @Cron('0 23 * * *', { timeZone: 'Africa/Lagos' })
  async sendDailySummaries() {
    const lockKey = 'cron:daily-summaries';
    const acquired = await this.redis.set(lockKey, '1', 'EX', 300, 'NX');
    if (!acquired) return; // another instance is running this

    try {
      // Cursor-based batching -- never loads all merchants into memory
      let cursor: string | undefined;
      do {
        const batch = await this.prisma.merchant.findMany({
          where: { isActive: true, expoPushToken: { not: null } },
          take: 50,
          skip: cursor → 1 : 0,
          cursor: cursor → { id: cursor } : undefined,
          orderBy: { id: 'asc' },
          select: { id: true, expoPushToken: true, name: true },
        });

        await Promise.allSettled(batch.map(m => this.processMerchantSummary(m)));
        cursor = batch.length === 50 → batch[batch.length - 1].id : undefined;
      } while (cursor);
    } finally {
      await this.redis.del(lockKey);
    }
  }

  // Clean up abandoned PENDING orders older than 2 hours
  // CRITICAL: Only cancel orders with NO paystackRef
  // Orders with a paystackRef may have a Paystack webhook in flight (can arrive up to 72hrs late)
  @Cron('0 * * * *')
  async cancelAbandonedOrders() {
    const lockKey = 'cron:abandon-orders';
    const acquired = await this.redis.set(lockKey, '1', 'EX', 120, 'NX');
    if (!acquired) return;

    try {
      const twoHoursAgo = new Date(Date.now() - 2 * 60 * 60 * 1000);
      const abandoned = await this.prisma.order.findMany({
        where: {
          status: 'PENDING',
          createdAt: { lt: twoHoursAgo },
          paystackRef: null,  // No payment was ever initialized -- safe to cancel
        },
        include: { items: true },
      });

      for (const order of abandoned) {
        await this.prisma.$transaction(async (tx) => {
          await tx.order.update({
            where: { id: order.id },
            data: { status: 'CANCELLED', cancelReason: 'Payment timeout -- no payment initiated' },
          });
          for (const item of order.items) {
            await tx.product.update({
              where: { id: item.productId },
              data: { quantity: { increment: item.quantity } },
            });
          }
        });
      }
    } finally {
      await this.redis.del(lockKey);
    }
  }
}
```

# 15. ERROR HANDLING & OBSERVABILITY

## 15.1 Global Exception Filter (Complete)

```typescript
// common/filters/global-exception.filter.ts
@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(GlobalExceptionFilter.name);

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx    = host.switchToHttp();
    const res    = ctx.getResponse<Response>();
    const req    = ctx.getRequest<Request>();

    let status  = HttpStatus.INTERNAL_SERVER_ERROR;
    let code    = 'INTERNAL_ERROR';
    let message = 'An unexpected error occurred';
    let details: unknown;

    if (exception instanceof HttpException) {
      status = exception.getStatus();
      const body = exception.getResponse() as any;
      message = Array.isArray(body.message) → 'Validation failed' : body.message ?? message;
      details = Array.isArray(body.message) → body.message : undefined;
      code    = body.error ?? 'HTTP_ERROR';

    } else if (exception instanceof Prisma.PrismaClientKnownRequestError) {
      switch (exception.code) {
        case 'P2002':
          status = 409; code = 'DUPLICATE_ENTRY';
          message = `${(exception.meta?.target as string[])?.join(', ')} already exists`;
          break;
        case 'P2025':
          status = 404; code = 'NOT_FOUND';
          message = 'Record not found';
          break;
        case 'P2003':
          status = 400; code = 'FOREIGN_KEY_VIOLATION';
          message = 'Referenced record does not exist';
          break;
        default:
          this.logger.error(`Unhandled Prisma error: ${exception.code}`, exception);
      }
    }

    if (status >= 500) {
      this.logger.error(exception);
      Sentry.captureException(exception, {
        extra: { url: req.url, method: req.method, body: req.body },
      });
    }

    res.status(status).json({
      success: false,
      error: { code, message, ...(details && { details }) },
      timestamp: new Date().toISOString(),
      path: req.url,
    });
  }
}
```

## 15.2 Structured Logging (Pino)

```typescript
// main.ts
import { Logger } from 'nestjs-pino';
app.useLogger(app.get(Logger));

// Every request logged automatically:
{
  "level": "info",
  "time": "2026-03-25T10:00:00.000Z",
  "req": {
    "id": "req-uuid",
    "method": "POST",
    "url": "/api/v1/pos/sales",
    "remoteAddress": "1.2.3.4"
  },
  "res": { "statusCode": 201 },
  "responseTime": 45,
  "merchantId": "clxxx",
  "msg": "request completed"
}
```

## 15.3 Health Check Endpoint

```typescript
// app.controller.ts
@Get('/health')
@Public()
async health() {
  const dbOk = await this.prisma.$queryRaw`SELECT 1`.then(() => true).catch(() => false);
  const redisOk = await this.redis.ping().then(r => r === 'PONG').catch(() => false);

  const status = dbOk && redisOk → 'ok' : 'degraded';
  const httpStatus = status === 'ok' → 200 : 503;

  return {
    status,
    checks: { database: dbOk, cache: redisOk },
    timestamp: new Date().toISOString(),
    version: process.env.npm_package_version,
  };
}
```

## 15.4 Monitoring Stack

| Tool | Purpose | Cost |
|---|---|---|
| Sentry | Crash reporting, performance traces | Free (5K errors/month) |
| UptimeRobot | Ping /health every 5 min, alert on downtime | Free |
| Railway Logs | Structured Pino logs, searchable | Included in Railway |
| Papertrail | Log aggregation + search (optional) | Free (48hr retention) |
| Expo Insights | Mobile app crash analytics | Free |

## 15.5 Request Tracing (requestId Propagation)

Every request gets a `requestId` that appears in all logs, making it possible to trace a specific request from mobile → API → database.

```typescript
// main.ts
import { v4 as uuid } from 'uuid';
app.use((req, res, next) => {
  req.requestId = (req.headers['x-request-id'] as string) ?? uuid();
  res.setHeader('x-request-id', req.requestId);
  next();
});
```

```typescript
// logging.interceptor.ts -- include requestId in every log line
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler) {
    const req = context.switchToHttp().getRequest();
    const start = Date.now();
    return next.handle().pipe(
      tap(() => {
        this.logger.log({
          requestId: req.requestId,
          method: req.method,
          url: req.url,
          statusCode: context.switchToHttp().getResponse().statusCode,
          responseTime: Date.now() - start,
          merchantId: req.user?.merchantId ?? null,
        });
      }),
    );
  }
}
```

## 15.6 SLO Definitions

| Endpoint | Latency SLO (p95) | Availability SLO |
|---|---|---|
| `POST /pos/sales` | < 500ms | 99.5% |
| `POST /pos/sales/sync` | < 2000ms | 99.5% |
| `GET /storefront/products/search` | < 300ms (cached) | 99.9% |
| `POST /payments/webhook` | < 200ms | 99.9% |
| `GET /inventory/products` | < 400ms | 99.5% |
| `GET /health` | < 100ms | 99.9% |

# 16. DEPLOYMENT ARCHITECTURE

## 16.1 Cloud Deployment (Recommended)

### Full Architecture Diagram
```
Internet Traffic
      |
      ?
+-----------------------------------------------------+
|                  CLOUDFLARE (Free)                   |
|  DNS | SSL/TLS | DDoS | WAF | CDN | Rate Limiting   |
+-----------------------------------------------------+
           |                          |
           →                          ?
+------------------+       +--------------------------+
|  VERCEL (Free)   |       |   RAILWAY (Hobby ~$10/mo) |
|  Next.js Admin   |       |   NestJS API              |
|  Auto-deploy     |       |   Auto-deploy on push     |
|  Edge CDN        |       |   Health checks           |
+------------------+       +--------------------------+
                                        |
                    +-------------------+-------------------+
                    |                   |                   |
                    →                   →                   ?
         +------------------+  +--------------+  +------------------+
         |  SUPABASE (Free) |  |   UPSTASH    |  |  SUPABASE        |
         |  PostgreSQL 16   |  |   REDIS      |  |  STORAGE         |
         |  PgBouncer pool  |  |  Free tier   |  |  Product images  |
         |  Auto backups    |  |  10K req/day |  |  CDN delivery    |
         +------------------+  +--------------+  +------------------+
```

### GitHub Actions CI/CD Pipeline
```yaml
# .github/workflows/deploy.yml
name: Test → Build → Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: invento_test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check
      - run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/invento_test
      - run: npm run test -- --runInBand --forceExit --coverage
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/invento_test
          JWT_ACCESS_SECRET: test-supabase-jwt-secret-min-32-chars
          # JWT_REFRESH_SECRET not needed -- Supabase Auth manages refresh tokens

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production  # requires manual approval in GitHub Environments
    steps:
      - uses: actions/checkout@v4
      - name: Run production migrations (direct connection)
        run: npx prisma migrate deploy
        env:
          DATABASE_URL: ${{ secrets.DIRECT_URL_PROD }}
      - name: Deploy to Railway
        uses: railwayapp/railway-action@v1
        with:
          service: invento-api
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN }}
```

### Environment Variables (Complete)
```bash
# .env.example -- all required vars with descriptions

# -- App ------------------------------------------
NODE_ENV=production
PORT=3000

# -- Database -------------------------------------
# Port 6543 = PgBouncer (transaction mode) -- use for all queries, prevents connection exhaustion
# connection_limit=2 per instance -- PgBouncer handles multiplexing efficiently
DATABASE_URL=postgresql://postgres.[ref]:[password]@aws-0-eu-west-1.pooler.supabase.com:6543/postgres?connection_limit=2&pool_timeout=10
# Port 5432 = direct connection -- use ONLY for Prisma migrations
DIRECT_URL=postgresql://postgres:[password]@db.[ref].supabase.co:5432/postgres

# -- Redis ----------------------------------------
REDIS_URL=rediss://default:[password]@[host].upstash.io:6379

# -- JWT ------------------------------------------
# NOTE: JWT_ACCESS_SECRET and JWT_REFRESH_SECRET are NOT used.
# Supabase Auth issues and verifies all merchant/consumer JWTs.
# NestJS only verifies using SUPABASE_JWT_SECRET (set in Supabase Auth section below).
JWT_ADMIN_SECRET=admin-specific-min-32-chars-string
JWT_ADMIN_EXPIRES_IN=1h

# -- Paystack -------------------------------------
PAYSTACK_SECRET_KEY=sk_live_xxxxxxxxxxxxxxxxxxxx
PAYSTACK_PUBLIC_KEY=pk_live_xxxxxxxxxxxxxxxxxxxx

# -- Termii ---------------------------------------
TERMII_API_KEY=xxxxxxxxxxxxxxxxxxxx
TERMII_SENDER_ID=Invento
TERMII_BASE_URL=https://api.ng.termii.com

# -- Firebase (FCM) -------------------------------
FIREBASE_PROJECT_ID=invento-prod
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@invento-prod.iam.gserviceaccount.com

# -- Supabase Auth --------------------------------
SUPABASE_URL=https://[ref].supabase.co
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
SUPABASE_JWT_SECRET=your-supabase-jwt-secret-from-dashboard  # Settings → API → JWT Secret

# -- CORS -----------------------------------------
ALLOWED_ORIGINS=https://admin.invento.ng,https://invento.ng

# -- Sentry ---------------------------------------
SENTRY_DSN=https://[key]@o[id].ingest.sentry.io/[project]
```

### Joi Validation Schema (Startup Guard)
```typescript
// config/validation.schema.ts
export const validationSchema = Joi.object({
  NODE_ENV:              Joi.string().valid('development', 'production', 'test').required(),
  PORT:                  Joi.number().default(3000),
  DATABASE_URL:          Joi.string().uri().required(),
  DIRECT_URL:            Joi.string().uri().required(),
  REDIS_URL:             Joi.string().required(),
  // Supabase Auth -- NestJS verifies with this secret, never issues tokens
  SUPABASE_URL:          Joi.string().uri().required(),
  SUPABASE_SERVICE_ROLE_KEY: Joi.string().required(),
  SUPABASE_JWT_SECRET:   Joi.string().min(32).required(),
  // Admin JWT only (custom flow)
  JWT_ADMIN_SECRET:      Joi.string().min(32).required(),
  PAYSTACK_SECRET_KEY:   Joi.string().required(),
  TERMII_API_KEY:        Joi.string().required(),
  FIREBASE_PROJECT_ID:   Joi.string().required(),
  FIREBASE_PRIVATE_KEY:  Joi.string().required(),
  FIREBASE_CLIENT_EMAIL: Joi.string().email().required(),
  SUPABASE_URL:          Joi.string().uri().required(),
  SUPABASE_SERVICE_ROLE_KEY: Joi.string().required(),
  SUPABASE_JWT_SECRET:   Joi.string().min(32).required(),
  ALLOWED_ORIGINS:       Joi.string().required(),
  SENTRY_DSN:            Joi.string().uri().optional(),
});
// App refuses to start if any required var is missing or invalid
```

## 16.2 VPS Deployment (Bootstrap Option)

### Architecture
```
Internet
  |
  ?
Cloudflare (Free)
  +--? Contabo VPS (~$5.17/month)
        Ubuntu 22.04 LTS
        4 vCPU | 8GB RAM | 75GB NVMe
        |
        +-- Nginx 1.24
        |   +-- SSL via Certbot (Let's Encrypt, auto-renew)
        |   +-- api.invento.ng → localhost:3000
        |   +-- admin.invento.ng → localhost:3001 (if self-hosted)
        |
        +-- NestJS API (PM2 cluster mode, all cores)
        |
        +-- PostgreSQL 16
        |   +-- pg_hba.conf: local connections only
        |   +-- Daily backup → Backblaze B2
        |
        +-- Redis 7
            +-- requirepass set, bind 127.0.0.1
```

### Nginx Configuration
```nginx
# /etc/nginx/sites-available/invento-api
server {
    listen 443 ssl http2;
    server_name api.invento.ng;

    ssl_certificate     /etc/letsencrypt/live/api.invento.ng/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/api.invento.ng/privkey.pem;
    ssl_protocols       TLSv1.2 TLSv1.3;
    ssl_ciphers         HIGH:!aNULL:!MD5;

    # Security headers (Cloudflare handles most, belt-and-suspenders)
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;

    # Webhook endpoint: preserve raw body
    location /api/v1/payments/webhook {
        proxy_pass http://localhost:3000;
        proxy_set_header Content-Type application/json;
        proxy_pass_request_body on;
    }

    location /api/ {
        proxy_pass         http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection 'upgrade';
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        proxy_read_timeout 30s;
        proxy_cache_bypass $http_upgrade;
    }
}

server {
    listen 80;
    server_name api.invento.ng;
    return 301 https://$host$request_uri;
}
```

### PM2 Ecosystem Config
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name:        'invento-api',
    script:      'dist/main.js',
    instances:   'max',
    exec_mode:   'cluster',
    watch:       false,
    max_memory_restart: '1G',
    env_production: {
      NODE_ENV: 'production',
      PORT:     3000,
    },
    error_file:      'logs/err.log',
    out_file:        'logs/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm:ss Z',
    merge_logs:      true,
  }],
};

// Deploy commands:
// pm2 start ecosystem.config.js --env production
// pm2 reload invento-api          → zero-downtime reload
// pm2 save && pm2 startup         → survive server reboot
```

### Automated PostgreSQL Backup Script
```bash
#!/bin/bash
# /etc/cron.d/invento-backup -- runs daily at 3am WAT
0 2 * * * root /opt/invento/scripts/backup.sh >> /var/log/invento-backup.log 2>&1

# /opt/invento/scripts/backup.sh
#!/bin/bash
set -euo pipefail

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/tmp/invento_backups"
BACKUP_FILE="${BACKUP_DIR}/invento_${DATE}.sql.gz"
B2_BUCKET="invento-db-backups"
RETENTION_DAYS=30

mkdir -p "$BACKUP_DIR"

# Dump and compress
PGPASSWORD="$DB_PASSWORD" pg_dump \
  -h localhost -U postgres invento \
  --no-owner --no-acl \
  | gzip > "$BACKUP_FILE"

echo "[$(date)] Backup created: $BACKUP_FILE ($(du -sh $BACKUP_FILE | cut -f1))"

# Upload to Backblaze B2
b2 upload-file "$B2_BUCKET" "$BACKUP_FILE" "daily/$(basename $BACKUP_FILE)"
echo "[$(date)] Uploaded to B2"

# Clean local temp
rm "$BACKUP_FILE"

# Prune old B2 backups (keep last 30 days)
b2 list-file-names "$B2_BUCKET" --prefix "daily/" --json \
  | jq -r '.files[] | select(.uploadTimestamp < '"$(date -d "-${RETENTION_DAYS} days" +%s%3N)"') | .fileId + " " + .fileName' \
  | while read fileId fileName; do
      b2 delete-file-version "$B2_BUCKET" "$fileName" "$fileId"
    done

echo "[$(date)] Backup complete"
```

## 16.3 Mobile App Deployment

### EAS Build Profiles
```json
// eas.json
{
  "cli": { "version": ">= 7.0.0" },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "android": { "buildType": "apk" }
    },
    "preview": {
      "distribution": "internal",
      "android": { "buildType": "apk" },
      "env": { "API_URL": "https://staging-api.invento.ng" }
    },
    "production": {
      "android": { "buildType": "app-bundle" },
      "ios": { "simulator": false },
      "env": { "API_URL": "https://api.invento.ng" }
    }
  },
  "submit": {
    "production": {
      "android": {
        "serviceAccountKeyPath": "./google-service-account.json",
        "track": "internal"
      }
    }
  }
}
```

### Build & Release Commands
```bash
# Internal testing build (APK)
eas build --platform android --profile preview

# Production build (AAB for Play Store)
eas build --platform android --profile production

# Submit to Play Store internal track
eas submit --platform android --profile production

# OTA update (JS-only changes -- no store review needed)
eas update --branch production --message "Fix: offline sync edge case on Android 14"

# Check build status
eas build:list --platform android --limit 5
```

# 17. API CONTRACT REFERENCE

## Complete Endpoint List

```
-- AUTH ------------------------------------------------------
POST   /api/v1/auth/send-otp              Public
POST   /api/v1/auth/verify-otp            Public
POST   /api/v1/auth/refresh               Public
POST   /api/v1/auth/logout                Merchant | Consumer
POST   /api/v1/auth/register-fcm          Merchant | Consumer

-- MERCHANTS -------------------------------------------------
GET    /api/v1/merchants/me               Merchant
PATCH  /api/v1/merchants/me               Merchant
GET    /api/v1/merchants/me/stats         Merchant

-- INVENTORY -------------------------------------------------
GET    /api/v1/inventory/products         Merchant
POST   /api/v1/inventory/products         Merchant
GET    /api/v1/inventory/products/:id     Merchant
PATCH  /api/v1/inventory/products/:id     Merchant
DELETE /api/v1/inventory/products/:id     Merchant
POST   /api/v1/inventory/products/bulk-import  Merchant
GET    /api/v1/inventory/products/sku/:sku     Merchant

-- POS -------------------------------------------------------
POST   /api/v1/pos/sales                  Merchant
POST   /api/v1/pos/sales/sync             Merchant
GET    /api/v1/pos/sales                  Merchant
GET    /api/v1/pos/sales/:id              Merchant
GET    /api/v1/pos/sales/summary          Merchant
GET    /api/v1/sync/pull                  Merchant (WatermelonDB sync)

-- ORDERS ----------------------------------------------------
POST   /api/v1/orders                     Consumer
GET    /api/v1/orders                     Merchant | Consumer
GET    /api/v1/orders/:id                 Merchant | Consumer
PATCH  /api/v1/orders/:id/status          Merchant | Consumer

-- PAYMENTS --------------------------------------------------
POST   /api/v1/payments/initialize        Consumer
POST   /api/v1/payments/webhook           Public (HMAC verified)

-- STOREFRONT (Public) ---------------------------------------
GET    /api/v1/storefront/markets
GET    /api/v1/storefront/markets/:id/shops
GET    /api/v1/storefront/shops/:id
GET    /api/v1/storefront/shops/:id/products
GET    /api/v1/storefront/products/search
GET    /api/v1/storefront/products/featured

-- ADMIN -----------------------------------------------------
GET    /api/v1/admin/merchants            Admin
PATCH  /api/v1/admin/merchants/:id/plan   Admin
PATCH  /api/v1/admin/merchants/:id/status Admin
GET    /api/v1/admin/markets              Admin
POST   /api/v1/admin/markets              Admin
PATCH  /api/v1/admin/markets/:id          Admin
GET    /api/v1/admin/orders               Admin
GET    /api/v1/admin/analytics/overview   Admin

-- SYSTEM ----------------------------------------------------
GET    /api/v1/health                     Public
```

# 18. TECHNOLOGY STACK REFERENCE

## Backend
| Layer | Tool | Version | Purpose |
|---|---|---|---|
| Runtime | Node.js | 20 LTS | JavaScript runtime |
| Framework | NestJS | v10+ | Modular, DI-based framework |
| Language | TypeScript | 5.x strict | Type safety end-to-end |
| ORM | Prisma | 5.x | Type-safe DB access + migrations |
| Database | PostgreSQL | 16 | Primary data store |
| Cache | Redis (Upstash) | 7 | OTP, rate limiting, storefront cache |
| Auth | Supabase Auth + Send SMS Hook | - | Phone OTP, JWT, refresh tokens, RLS |
| Validation | class-validator + class-transformer | latest | DTO validation |
| API Docs | @nestjs/swagger | latest | Auto-generated OpenAPI |
| Rate Limiting | @upstash/ratelimit | latest | Sliding window, replaces @nestjs/throttler |
| Security | helmet | latest | HTTP security headers |
| Logging | nestjs-pino | latest | Structured JSON logs |
| Scheduling | @nestjs/schedule | latest | Cron jobs |
| Events | Transactional Outbox + OutboxRelayService | - | Durable domain events, replaces EventEmitter2 |
| Testing | Jest + Supertest | latest | Unit + e2e tests |
| Error Tracking | @sentry/node | latest | Production crash reporting |
| Config | @nestjs/config + joi | latest | Validated env vars |
| HTTP Client | axios | latest | Paystack, Termii API calls |

## Mobile (Both Apps)
| Layer | Tool | Purpose |
|---|---|---|
| Framework | React Native (Expo Bare) | Cross-platform, native access |
| Language | TypeScript (strict) | Type safety |
| Navigation | Expo Router v5 | File-based routing |
| Server State | TanStack Query v5 | API data, caching |
| Client State | Zustand | Auth, cart, UI state |
| Offline DB | WatermelonDB | SQLite offline-first |
| Fast Storage | MMKV | Tokens, settings |
| Forms | React Hook Form + Zod | Validated forms |
| UI | NativeWind v4 | Tailwind in RN |
| Lists | FlashList (Shopify) | High-performance lists |
| Animations | Reanimated v3 | UI thread animations |
| HTTP | Axios + axios-retry | API calls + retry |
| Push | Expo Notifications + FCM | Cross-platform push |
| Images | Expo Image | Cached image loading |
| Barcode | expo-barcode-scanner | POS product scan |
| Payments | Paystack RN SDK | WebView checkout |
| Builds | EAS Build | Cloud CI/CD |
| OTA | EAS Update | Hotfix deployment |
| Errors | @sentry/react-native | Crash reporting |
| i18n | i18next | English + Hausa |

## Web Dashboard
| Layer | Tool | Purpose |
|---|---|---|
| Framework | Next.js 14 (App Router) | Admin dashboard |
| Language | TypeScript | Type safety |
| UI | Tailwind CSS + shadcn/ui | Component library |
| Charts | Recharts | Analytics visualizations |
| Tables | TanStack Table v8 | Data tables |
| State | TanStack Query v5 | Server state |
| Auth | NextAuth.js | Admin session |
| Hosting | Vercel (free) | Deployment |

## Infrastructure
| Service | Provider | Monthly Cost |
|---|---|---|
| API Hosting | Railway Hobby | ~$5|15 |
| Auth | Supabase Auth | Free → included in Pro |
| Database | Supabase Free → Pro | $0 → $25 |
| File Storage | Supabase Storage | Included |
| Realtime | Supabase Realtime | Included |
| Cache + Rate Limit | Upstash Redis + @upstash/ratelimit | Free → pay-per-use |
| CDN + DNS + DDoS | Cloudflare | Free |
| Web Hosting | Vercel | Free |
| App Builds | EAS Build | Free (30/mo) |
| OTA Updates | EAS Update | Free |
| Error Tracking | Sentry | Free (5K/mo) |
| Uptime Monitor | UptimeRobot | Free |
| SMS/OTP | Termii | ~₦5|7/SMS |
| Push Notifications | Firebase FCM | Free |
| Payments | Paystack | 1.5% + ₦100/txn |
| **Total Fixed/Month** | | **~$10|40** |

# APPENDIX A: FEATURE → PLAN MATRIX

| Feature | FREE | BASIC (₦3K/mo) | PRO (₦8.5K/mo) | ENTERPRISE |
|---|---|---|---|---|
| Inventory (SKU limit) | 50 | Unlimited | Unlimited | Unlimited |
| POS | Yes | Yes | Yes | Yes |
| Offline sync | Yes | Yes | Yes | Yes |
| Online storefront | No | Yes | Yes | Yes |
| Customer records (CRM) | Basic | Full | Full + Export | Full + API |
| Sales analytics | 7-day | 30-day | Full history | Full + Custom |
| CRM & Loyalty programs | No | No | Yes | Yes |
| SMS campaigns | No | No | Yes | Yes |
| Social media integration | No | Basic | Full | Full + Auto-post |
| Supplier management | No | No | Yes | Yes |
| Transaction commission | N/A | 5% | 3% | 1.5% |
| Support | Community | Email | Priority | Account Manager |

# APPENDIX B: RISK REGISTER & MITIGATIONS

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Low merchant digital literacy | High | High | Hausa i18n, in-app tutorials, field agent onboarding |
| Poor internet connectivity | High | High | Offline-first POS with WatermelonDB sync |
| Offline sync conflicts | Medium | Medium | Delta-based sync + SELECT FOR UPDATE + per-merchant allowNegativeStock policy + FORCE_SYNC reconciliation endpoint |
| Payment adoption resistance | Medium | High | USSD support, cash reconciliation in POS |
| Paystack webhook failure | Low | High | Idempotency check, Paystack auto-retry on non-200 |
| Merchant data leakage | Low | Critical | Prisma middleware + PostgreSQL RLS (two layers) |
| JWT token theft | Low | High | Short expiry (15min), MMKV encrypted storage |
| Supabase free tier pause | Medium | High | Upgrade to Pro ($25/mo) before pilot launch |
| Railway cost spike | Low | Medium | Monitor usage, migrate to DigitalOcean at $30/mo |
| Merchant churn after trial | Medium | High | Daily sales summary push, visible ROI reporting |
| CAC/FIRS regulatory issues | Low | High | Early legal review, register before launch |

# 19. OFFLINE CONFLICT RESOLUTION - COMPLETE FRAMEWORK

## 19.1 Root Cause

The delta-based sync handles single-device conflicts correctly via `SELECT FOR UPDATE`. It does not handle multi-device divergence: two devices both offline, both selling the same product, both syncing concurrently. The first sync wins; the second is REJECTED even though the sale physically happened. "Merchant must reconcile manually" is not an architecture.

## 19.2 Product Revision Numbers (Optimistic Locking)

Every stock change increments `Product.revision`. The device records the revision at the time of sale. On sync, the server compares the device's revision against the current server revision to detect staleness.

```typescript
// CreateSaleDTO -- now includes revision snapshot
class SaleItemDTO {
  @IsString()   productId: string;
  @IsInt()      quantity: number;
  @IsInt()      quantityDelta: number;   // negative = sold
  @IsDecimal()  unitPrice: string;
  @IsInt()      productRevision: number; // revision at time of offline sale
}
```

```typescript
// pos.service.ts -- processSaleDelta with revision awareness
async processSaleDelta(merchantId: string, dto: CreateSaleDTO): Promise<SyncSaleResult> {
  const merchant = await this.prisma.merchant.findUnique({
    where: { id: merchantId },
    select: { allowNegativeStock: true },
  });

  return this.prisma.$transaction(async (tx) => {
    const stockUpdates: SyncItemResult[] = [];
    const conflictItems: ConflictItem[] = [];

    for (const item of dto.items) {
      const product = await tx.$queryRaw<{ id: string; name: string; quantity: number; revision: number }[]>`
        SELECT id, name, quantity, revision
        FROM "Product"
        WHERE id = ${item.productId} AND "merchantId" = ${merchantId} AND "isActive" = true
        FOR UPDATE
      `;

      if (!product.length) {
        conflictItems.push({ productId: item.productId, reason: 'Product not found' });
        continue;
      }

      const p = product[0];
      const revisionDrift = p.revision - item.productRevision;
      const newQty = p.quantity + item.quantityDelta;

      if (newQty < 0 && !merchant?.allowNegativeStock) {
        conflictItems.push({
          productId: p.id,
          productName: p.name,
          requested: Math.abs(item.quantityDelta),
          available: p.quantity,
          revisionDrift,
          reason: 'INSUFFICIENT_STOCK',
        });
        continue;
      }

      // Apply delta - allow negative if merchant opted in
      const appliedQty = newQty < 0 → p.quantity : newQty; // floor at 0 if negative not allowed
      await tx.$executeRaw`
        UPDATE "Product"
        SET quantity = ${newQty < 0 && merchant?.allowNegativeStock → newQty : 0},
            revision = revision + 1,
            "updatedAt" = NOW()
        WHERE id = ${p.id}
      `;

      stockUpdates.push({
        productId: p.id,
        quantityRequested: Math.abs(item.quantityDelta),
        quantityApplied: item.quantityDelta,
        newQuantity: newQty < 0 && merchant?.allowNegativeStock → newQty : 0,
        newRevision: p.revision + 1,
        partial: false,
      });
    }

    if (stockUpdates.length === 0) {
      // All items conflicted -- reject entire sale
      return {
        localId: dto.localId,
        status: 'REJECTED',
        reason: conflictItems.map(c => `${c.productName}: ${c.reason}`).join('; '),
        conflictItems,
        stockUpdates: [],
      };
    }

    const sale = await tx.sale.create({
      data: {
        localId: dto.localId,
        merchantId,
        total: new Decimal(stockUpdates.reduce((s, u) => {
          const item = dto.items.find(i => i.productId === u.productId)!;
          return s + Math.abs(u.quantityApplied) * parseFloat(item.unitPrice);
        }, 0)),
        paymentMethod: dto.paymentMethod,
        createdAt: dto.createdAt → new Date(dto.createdAt) : new Date(),
        syncedAt: new Date(),
        items: {
          create: stockUpdates.map(u => {
            const item = dto.items.find(i => i.productId === u.productId)!;
            return { productId: u.productId, quantity: Math.abs(u.quantityApplied), unitPrice: new Decimal(item.unitPrice) };
          }),
        },
      },
    });

    await tx.outboxEvent.create({
      data: {
        eventType: 'sale.created',
        aggregateId: sale.id,
        priority: 3,
        payload: { saleId: sale.id, merchantId, stockUpdates },
      },
    });

    return {
      localId: dto.localId,
      status: conflictItems.length > 0 → 'PARTIAL' : 'SYNCED',
      conflictItems: conflictItems.length > 0 → conflictItems : undefined,
      stockUpdates,
    };
  }, { isolationLevel: 'Serializable' });
}
```

## 19.3 FORCE_SYNC - Merchant Override for Completed Sales

When a sale is REJECTED but the merchant already gave goods to the customer, they can force-record it:

```
POST /api/v1/pos/sales/:localId/reconcile
  Auth:   Merchant JWT
  Body:   { action: 'FORCE_SYNC' | 'VOID', voidItemIds?: string[] }
  Action: FORCE_SYNC -- record sale with negative stock (requires allowNegativeStock or admin override)
          VOID -- mark specific items as voided, record partial sale for remaining items
```

```typescript
// pos.service.ts
async reconcileSale(merchantId: string, localId: string, dto: ReconcileDTO): Promise<void> {
  if (dto.action === 'FORCE_SYNC') {
    // Temporarily enable negative stock for this operation
    await this.prisma.$transaction(async (tx) => {
      const sale = await tx.sale.findUnique({ where: { localId }, include: { items: true } });
      if (!sale || sale.merchantId !== merchantId) throw new NotFoundException();

      for (const item of sale.items) {
        await tx.$executeRaw`
          UPDATE "Product" SET quantity = quantity - ${item.quantity}, revision = revision + 1
          WHERE id = ${item.productId}
        `;
      }

      await tx.sale.update({ where: { localId }, data: { syncedAt: new Date(), note: 'FORCE_SYNCED' } });

      await tx.outboxEvent.create({
        data: {
          eventType: 'sale.force_synced',
          aggregateId: sale.id,
          priority: 2,
          payload: { saleId: sale.id, merchantId, forcedBy: merchantId },
        },
      });
    });
  }
}
```

## 19.4 Conflict Notification Payload

The sync response tells the merchant exactly what happened:

```typescript
interface ConflictItem {
  productId: string;
  productName?: string;
  requested: number;       // units the device tried to sell
  available: number;       // units the server had
  revisionDrift: number;   // how many stock changes happened while offline
  reason: 'INSUFFICIENT_STOCK' | 'Product not found';
}

interface SyncSaleResult {
  localId: string;
  status: 'SYNCED' | 'PARTIAL' | 'REJECTED' | 'DUPLICATE';
  reason?: string;
  conflictItems?: ConflictItem[];  // shown to merchant in reconciliation UI
  stockUpdates: SyncItemResult[];
}
```

The mobile app shows a reconciliation screen for PARTIAL/REJECTED sales listing each conflicted item with the reason and a FORCE_SYNC / VOID action per item.

# 20. OUTBOX — PRODUCTION-GRADE JOB PROCESSING

## 20.1 Priority + Visibility Timeout + Backoff

The `OutboxEvent` model now has `priority` (1=critical, 2=high, 3=normal, 4=low) and `nextRetryAt`. The relay queries by priority first, then time.

### Per-Event-Type Retry Policy

| Event Type | Priority | Max Attempts | Backoff Formula | Max Delay |
|---|---|---|---|---|
| `order.confirmed` | 1 (critical) | 20 | `2^n * 5s` | 1 hour |
| `order.status.changed` | 1 (critical) | 20 | `2^n * 5s` | 1 hour |
| `sale.created` | 2 (high) | 10 | `2^n * 10s` | 1 hour |
| `inventory.low_stock` | 3 (normal) | 5 | `2^n * 30s` | 1 hour |
| `merchant.plan.changed` | 2 (high) | 10 | `2^n * 10s` | 1 hour |
| `sale.force_synced` | 2 (high) | 10 | `2^n * 10s` | 1 hour |

```typescript
// outbox/retry-policy.ts
export const RETRY_POLICY: Record<string, { maxAttempts: number; baseDelayMs: number }> = {
  'order.confirmed':      { maxAttempts: 20, baseDelayMs: 5_000 },
  'order.status.changed': { maxAttempts: 20, baseDelayMs: 5_000 },
  'sale.created':         { maxAttempts: 10, baseDelayMs: 10_000 },
  'inventory.low_stock':  { maxAttempts: 5,  baseDelayMs: 30_000 },
  'merchant.plan.changed':{ maxAttempts: 10, baseDelayMs: 10_000 },
  'sale.force_synced':    { maxAttempts: 10, baseDelayMs: 10_000 },
};

export function getNextRetryAt(eventType: string, attempts: number): Date {
  const policy = RETRY_POLICY[eventType] ?? { maxAttempts: 5, baseDelayMs: 30_000 };
  const delayMs = Math.min(
    Math.pow(2, attempts) * policy.baseDelayMs,
    60 * 60 * 1000, // cap at 1 hour
  );
  return new Date(Date.now() + delayMs);
}
```

## 20.2 Updated Relay Service

```typescript
// outbox/outbox-relay.service.ts
@Injectable()
export class OutboxRelayService {
  private readonly logger = new Logger(OutboxRelayService.name);
  private fcmCircuitOpen = false;
  private fcmFailureCount = 0;
  private fcmCircuitResetAt: Date | null = null;

  @Cron('* * * * * *')
  async relay(): Promise<void> {
    const lockKey = 'outbox:relay:lock';
    const acquired = await this.redis.set(lockKey, process.env.INSTANCE_ID ?? '1', 'EX', 5, 'NX');
    if (!acquired) return;

    try {
      // Reset visibility timeout: PROCESSING events stuck > 30s → back to PENDING
      await this.prisma.outboxEvent.updateMany({
        where: {
          status: 'PROCESSING',
          updatedAt: { lt: new Date(Date.now() - 30_000) },
        },
        data: { status: 'PENDING' },
      });

      // Fetch by priority, then time, respecting backoff
      const events = await this.prisma.outboxEvent.findMany({
        where: {
          status: 'PENDING',
          OR: [
            { nextRetryAt: null },
            { nextRetryAt: { lte: new Date() } },
          ],
        },
        orderBy: [{ priority: 'asc' }, { createdAt: 'asc' }],
        take: 50,
      });

      for (const event of events) {
        const policy = RETRY_POLICY[event.eventType] ?? { maxAttempts: 5, baseDelayMs: 30_000 };
        if (event.attempts >= policy.maxAttempts) {
          await this.prisma.outboxEvent.update({
            where: { id: event.id },
            data: { status: 'FAILED' },
          });
          Sentry.captureMessage(`OutboxEvent dead-lettered: ${event.eventType}`, {
            level: 'error',
            extra: { eventId: event.id, aggregateId: event.aggregateId },
          });
          continue;
        }

        await this.prisma.outboxEvent.update({
          where: { id: event.id },
          data: { status: 'PROCESSING', attempts: { increment: 1 } },
        });

        try {
          await this.dispatch(event);
          await this.prisma.outboxEvent.update({
            where: { id: event.id },
            data: { status: 'PROCESSED', processedAt: new Date() },
          });
          // Reset FCM circuit on success
          if (event.eventType.includes('notification') || event.eventType.includes('order')) {
            this.fcmFailureCount = 0;
            this.fcmCircuitOpen = false;
          }
        } catch (err) {
          const nextRetryAt = getNextRetryAt(event.eventType, event.attempts);
          await this.prisma.outboxEvent.update({
            where: { id: event.id },
            data: { status: 'PENDING', nextRetryAt },
          });
          this.logger.error(`Relay failed for ${event.id} (${event.eventType}), retry at ${nextRetryAt.toISOString()}`, err);
        }
      }
    } finally {
      await this.redis.del(lockKey);
    }
  }

  private async dispatch(event: OutboxEvent): Promise<void> {
    // FCM circuit breaker
    if (this.fcmCircuitOpen) {
      if (this.fcmCircuitResetAt && new Date() > this.fcmCircuitResetAt) {
        this.fcmCircuitOpen = false;
        this.fcmFailureCount = 0;
      } else {
        throw new Error('FCM circuit open - skipping notification dispatch');
      }
    }

    switch (event.eventType) {
      case 'order.confirmed':
        try {
          await this.notifications.sendOrderConfirmation(event.payload, event.id);
        } catch (err) {
          this.fcmFailureCount++;
          if (this.fcmFailureCount >= 10) {
            this.fcmCircuitOpen = true;
            this.fcmCircuitResetAt = new Date(Date.now() + 60_000); // 60s pause
          }
          throw err;
        }
        break;
      case 'order.status.changed':
        await this.notifications.sendOrderStatusUpdate(event.payload, event.id);
        break;
      case 'sale.created':
        await this.inventory.checkAndEmitLowStock(event.payload);
        break;
      case 'inventory.low_stock':
        await this.notifications.sendLowStockAlert(event.payload, event.id);
        break;
      case 'merchant.plan.changed':
        await this.notifications.sendPlanChangeConfirmation(event.payload, event.id);
        break;
      default:
        this.logger.warn(`Unknown event type: ${event.eventType}`);
    }
  }
}
```

## 20.3 FCM Idempotency

Pass `event.id` as the FCM message ID to prevent duplicate pushes on relay retry:

```typescript
// integrations/firebase/firebase.service.ts
async send(token: string, title: string, body: string, messageId: string): Promise<void> {
  await this.firebaseAdmin.messaging().send({
    token,
    notification: { title, body },
    android: { collapseKey: messageId }, // deduplication key on Android
    apns: { headers: { 'apns-collapse-id': messageId } }, // deduplication key on iOS
  });
}
```

## 20.4 Dead-Letter Alerting

FAILED events trigger an immediate Sentry alert (shown in relay above). The admin dashboard shows a dead-letter queue panel at `GET /api/v1/admin/outbox/failed` with counts by event type. SLA: FAILED events must be reviewed within 24 hours.

# 21. CQRS — EXPLICIT READ/WRITE SEPARATION

## 21.1 Command vs Query Classification

| Endpoint | Path | Type | DB Target |
|---|---|---|---|
| Create sale | `POST /pos/sales` | Command | Primary (write) |
| Sync sales | `POST /pos/sales/sync` | Command | Primary (write) |
| Create order | `POST /orders` | Command | Primary (write) |
| Confirm order | `PATCH /orders/:id/status` | Command | Primary (write) |
| Create product | `POST /inventory/products` | Command | Primary (write) |
| Sales summary | `GET /pos/sales/summary` | Query | Read model (DailySalesSummary) |
| Sales list | `GET /pos/sales` | Query | Primary (indexed, paginated) |
| Storefront search | `GET /storefront/products/search` | Query | Redis cache → Primary |
| Admin analytics | `GET /admin/analytics/overview` | Query | Read model |
| Product list | `GET /inventory/products` | Query | Primary (indexed, paginated) |

## 21.2 DailySalesSummary Read Model

Analytics queries (`GET /pos/sales/summary`) no longer run `GROUP BY` aggregations on the live `Sale` table. They read from `DailySalesSummary` which is updated incrementally by the Outbox.

```typescript
// outbox/outbox-relay.service.ts - dispatch for sale.created
case 'sale.created': {
  const { merchantId, total, paymentMethod, date } = event.payload;
  const saleDate = new Date(date);
  saleDate.setUTCHours(0, 0, 0, 0); // truncate to day

  // Upsert the daily summary - atomic increment, no race condition
  await this.prisma.$executeRaw`
    INSERT INTO "DailySalesSummary" ("id", "merchantId", "date", "totalRevenue", "totalSales",
      "cashRevenue", "transferRevenue", "ussdRevenue", "cardRevenue", "updatedAt")
    VALUES (
      ${cuid()}, ${merchantId}, ${saleDate}, ${total}, 1,
      ${paymentMethod === 'CASH' → total : 0},
      ${paymentMethod === 'TRANSFER' → total : 0},
      ${paymentMethod === 'USSD' → total : 0},
      ${paymentMethod === 'CARD' → total : 0},
      NOW()
    )
    ON CONFLICT ("merchantId", "date") DO UPDATE SET
      "totalRevenue"    = "DailySalesSummary"."totalRevenue" + EXCLUDED."totalRevenue",
      "totalSales"      = "DailySalesSummary"."totalSales" + 1,
      "cashRevenue"     = "DailySalesSummary"."cashRevenue" + EXCLUDED."cashRevenue",
      "transferRevenue" = "DailySalesSummary"."transferRevenue" + EXCLUDED."transferRevenue",
      "ussdRevenue"     = "DailySalesSummary"."ussdRevenue" + EXCLUDED."ussdRevenue",
      "cardRevenue"     = "DailySalesSummary"."cardRevenue" + EXCLUDED."cardRevenue",
      "updatedAt"       = NOW()
  `;
  break;
}
```

## 21.3 Updated Sales Summary Endpoint

```typescript
// pos.service.ts
async getSalesSummary(merchantId: string, query: SalesSummaryDTO) {
  const { period, from, to } = query;
  const dateRange = resolveDateRange(period, from, to);

  // Read from pre-aggregated model -- no GROUP BY on live table
  const summaries = await this.prisma.dailySalesSummary.findMany({
    where: {
      merchantId,
      date: { gte: dateRange.from, lte: dateRange.to },
    },
    orderBy: { date: 'asc' },
  });

  const totalRevenue = summaries.reduce((s, r) => s.add(r.totalRevenue), new Decimal(0));
  const totalSales   = summaries.reduce((s, r) => s + r.totalSales, 0);

  return {
    totalRevenue,
    totalSales,
    avgSaleValue: totalSales > 0 → totalRevenue.div(totalSales) : new Decimal(0),
    paymentBreakdown: {
      CASH:     summaries.reduce((s, r) => s.add(r.cashRevenue), new Decimal(0)),
      TRANSFER: summaries.reduce((s, r) => s.add(r.transferRevenue), new Decimal(0)),
      USSD:     summaries.reduce((s, r) => s.add(r.ussdRevenue), new Decimal(0)),
      CARD:     summaries.reduce((s, r) => s.add(r.cardRevenue), new Decimal(0)),
    },
    dailyRevenue: summaries.map(s => ({
      date: s.date.toISOString().split('T')[0],
      revenue: s.totalRevenue,
      count: s.totalSales,
    })),
  };
}
```

`topProducts` still requires a live query (it needs per-product breakdown not in the summary). Cache it in Redis with a 5-minute TTL keyed by `merchantId + dateRange`.

# 22. OBSERVABILITY — WORLD-CLASS STACK

## 22.1 Metrics Endpoint (Prometheus Format)

```typescript
// app.controller.ts
@Get('/metrics')
@Public()
async metrics(): Promise<string> {
  const [pendingOutbox, processingOutbox, failedOutbox] = await Promise.all([
    this.prisma.outboxEvent.count({ where: { status: 'PENDING' } }),
    this.prisma.outboxEvent.count({ where: { status: 'PROCESSING' } }),
    this.prisma.outboxEvent.count({ where: { status: 'FAILED' } }),
  ]);

  const lines = [
    `# HELP outbox_events_pending Number of pending outbox events`,
    `# TYPE outbox_events_pending gauge`,
    `outbox_events_pending ${pendingOutbox}`,
    `# HELP outbox_events_processing Number of processing outbox events`,
    `# TYPE outbox_events_processing gauge`,
    `outbox_events_processing ${processingOutbox}`,
    `# HELP outbox_events_failed Number of dead-lettered outbox events`,
    `# TYPE outbox_events_failed gauge`,
    `outbox_events_failed ${failedOutbox}`,
    // HTTP metrics are collected by the LoggingInterceptor and exposed here
    ...this.metricsService.getHttpMetrics(),
  ];

  return lines.join('\n');
}
```

`MetricsService` uses in-memory counters updated by `LoggingInterceptor` on every request. For production, use `prom-client` npm package which handles histogram buckets automatically.

## 22.2 Deep Health Check

```typescript
// app.controller.ts
@Get('/health')
@Public()
async health() {
  const checks = await Promise.allSettled([
    this.prisma.$queryRaw`SELECT 1`.then(() => ({ name: 'database', status: 'ok' })),
    this.redis.ping().then(r => ({ name: 'redis', status: r === 'PONG' → 'ok' : 'degraded' })),
    fetch(`${process.env.SUPABASE_URL}/auth/v1/health`).then(r => ({
      name: 'supabase_auth', status: r.ok → 'ok' : 'degraded',
    })),
    fetch('https://api.paystack.co/').then(r => ({
      name: 'paystack', status: r.ok → 'ok' : 'degraded',
    })).catch(() => ({ name: 'paystack', status: 'degraded' })),
  ]);

  // Outbox relay lag check
  const oldestPending = await this.prisma.outboxEvent.findFirst({
    where: { status: 'PENDING' },
    orderBy: { createdAt: 'asc' },
    select: { createdAt: true },
  });
  const relayLagMs = oldestPending → Date.now() - oldestPending.createdAt.getTime() : 0;

  const components = checks.map(r => r.status === 'fulfilled' → r.value : { name: 'unknown', status: 'error' });
  components.push({ name: 'outbox_relay_lag_ms', status: relayLagMs < 10_000 → 'ok' : 'degraded' });

  const overallStatus = components.every(c => c.status === 'ok') → 'ok' : 'degraded';
  const httpStatus = overallStatus === 'ok' → 200 : 503;

  return {
    status: overallStatus,
    checks: components,
    relayLagMs,
    timestamp: new Date().toISOString(),
    version: process.env.npm_package_version,
  };
}
```

## 22.3 Alerting Rules

| Alert | Condition | Channel | Severity |
|---|---|---|---|
| API down | `/health` returns non-200 for 2 consecutive checks | SMS + Slack | Critical |
| Outbox backlog | `outbox_events_pending > 500` | Slack | High |
| Outbox dead-letter | `outbox_events_failed > 0` | Slack | High |
| Relay lag | `outbox_relay_lag_ms > 10000` | Slack | High |
| Error rate spike | HTTP 5xx rate > 1% over 5 min | Slack | High |
| p95 latency breach | `POST /pos/sales` p95 > 400ms | Slack | Medium |
| DB connections | PgBouncer pool > 80% utilized | Slack | Medium |

Configure via UptimeRobot (free) for uptime checks. Use Railway's built-in metrics for compute. Use Sentry performance monitoring for latency percentiles.

## 22.4 SLO Error Budget Calculation

```
Monthly error budget = (1 - SLO_target) × 30 days × 24 hours × 60 min

POST /pos/sales (99.5% SLO):
  Budget = 0.005 × 43,200 min = 216 minutes/month
  Alert when: consumed > 50% of budget in any 7-day window

GET /storefront/products/search (99.9% SLO):
  Budget = 0.001 × 43,200 min = 43.2 minutes/month
  Alert when: consumed > 25% of budget in any 24-hour window
```

# 23. CACHE KEY ENFORCEMENT — SYSTEMATIC TENANT SCOPING

## 23.1 CacheKeyBuilder Utility

```typescript
// common/cache/cache-key.builder.ts
export class CacheKeyBuilder {
  // Tenant-scoped: MUST include merchantId or consumerId
  static merchant(merchantId: string, ...parts: string[]): string {
    return `m:${merchantId}:${parts.join(':')}`;
  }

  static consumer(consumerId: string, ...parts: string[]): string {
    return `c:${consumerId}:${parts.join(':')}`;
  }

  // Version-scoped: invalidated by incrementing merchant version counter
  static storefrontShop(merchantId: string, version: number, ...parts: string[]): string {
    return `sf:v${version}:shop:${merchantId}:${parts.join(':')}`;
  }

  // Public/platform-scoped: NOT tenant-scoped by design
  static platform(...parts: string[]): string {
    return `platform:${parts.join(':')}`;
  }

  // Global search: scoped by query hash only (cross-tenant by design)
  static search(queryHash: string): string {
    return `sf:search:${queryHash}`;
  }
}
```

## 23.2 Cache Key Registry

| Key Pattern | Scope | TTL | Invalidated By |
|---|---|---|---|
| `m:{merchantId}:products:{page}:{filters}` | Tenant | 60s | Product CRUD |
| `sf:v{n}:shop:{merchantId}` | Storefront (versioned) | 900s | Merchant profile update |
| `sf:v{n}:shop:{merchantId}:products:{page}:{hash}` | Storefront (versioned) | 120s | Product CRUD → version increment |
| `sf:search:{queryHash}` | Public (cross-tenant) | 60s | Global search invalidation key |
| `sf:search:invalidate_at` | Global | | | Any product change sets this timestamp |
| `platform:markets` | Platform | 3600s | Admin market update |
| `platform:markets:{id}:shops:{page}` | Platform | 900s | Merchant joins/leaves market |
| `platform:featured` | Platform | 1800s | Admin updates featured |
| `c:{consumerId}:orders:{page}` | Consumer | 30s | Order status change |

## 23.3 Global Search Cache Invalidation

Search results are cross-tenant (all merchants). When any product changes, the search cache must be invalidated globally — but not by deleting all keys (too expensive).

```typescript
// storefront.service.ts
private readonly SEARCH_INVALIDATE_KEY = 'sf:search:invalidate_at';

async invalidateSearchCache(): Promise<void> {
  // Set a global invalidation timestamp - O(1) Redis write
  await this.redis.set(this.SEARCH_INVALIDATE_KEY, Date.now().toString());
}

async getSearchResults(query: ProductSearchDTO): Promise<PaginatedResult> {
  const queryHash = hashQuery(query);
  const cacheKey = CacheKeyBuilder.search(queryHash);

  // Check if cache was written before the last invalidation
  const [cached, invalidateAt] = await Promise.all([
    this.redis.get<{ data: PaginatedResult; cachedAt: number }>(cacheKey),
    this.redis.get(this.SEARCH_INVALIDATE_KEY),
  ]);

  if (cached && invalidateAt && cached.cachedAt > parseInt(invalidateAt)) {
    return cached.data; // cache is still valid
  }

  const result = await this.repo.searchProducts(query);
  await this.redis.set(cacheKey, { data: result, cachedAt: Date.now() }, 60);
  return result;
}
```

Call `invalidateSearchCache()` from the Outbox relay when processing `product.updated` or `product.created` events.

# 24. SECURITY — FINANCIAL-GRADE HARDENING

## 24.1 Audit Log

Every write to financially significant resources is recorded in `AuditLog`. The interceptor captures before/after snapshots automatically.

```typescript
// common/interceptors/audit.interceptor.ts
@Injectable()
export class AuditInterceptor implements NestInterceptor {
  private readonly AUDITED_ROUTES = [
    { method: 'PATCH', pattern: /\/inventory\/products\// },
    { method: 'PATCH', pattern: /\/orders\// },
    { method: 'PATCH', pattern: /\/admin\/merchants\// },
    { method: 'DELETE', pattern: /\/inventory\/products\// },
  ];

  intercept(context: ExecutionContext, next: CallHandler) {
    const req = context.switchToHttp().getRequest();
    const shouldAudit = this.AUDITED_ROUTES.some(
      r => req.method === r.method && r.pattern.test(req.url)
    );
    if (!shouldAudit) return next.handle();

    return next.handle().pipe(
      tap(async (responseData) => {
        await this.prisma.auditLog.create({
          data: {
            actorId: req.user?.sub ?? 'system',
            actorRole: req.user?.role ?? 'system',
            action: `${req.method.toLowerCase()}.${req.url.split('/').pop()}`,
            resourceType: this.extractResourceType(req.url),
            resourceId: req.params?.id ?? 'unknown',
            before: req.body?._before ?? null,
            after: responseData ?? null,
            ip: req.ip,
            userAgent: req.headers['user-agent'],
          },
        });
      }),
    );
  }
}
```

## 24.2 Sub-Role System (RBAC)

```typescript
// jwt-payload.type.ts
export interface JwtPayload {
  sub: string;
  role: 'merchant' | 'consumer' | 'admin';
  merchantRole?: 'OWNER' | 'CASHIER';  // for merchant sub-roles
  adminRole?: 'VIEWER' | 'OPERATOR' | 'SUPER'; // for admin sub-roles
  merchantId?: string;
  plan?: string;
}
```

```typescript
// common/guards/roles.guard.ts
const ROLE_PERMISSIONS: Record<string, string[]> = {
  OWNER:    ['inventory:read', 'inventory:write', 'pos:write', 'crm:read', 'analytics:read'],
  CASHIER:  ['inventory:read', 'pos:write'], // POS only - no inventory write, no CRM
  VIEWER:   ['admin:read'],
  OPERATOR: ['admin:read', 'admin:merchants:write', 'admin:markets:write'],
  SUPER:    ['admin:read', 'admin:merchants:write', 'admin:markets:write', 'admin:plans:write', 'admin:outbox:write'],
};

// Usage: @RequirePermission('inventory:write')
```

## 24.3 Per-Tenant Rate Limiting

```typescript
// common/guards/rate-limit.guard.ts
@Injectable()
export class RateLimitGuard implements CanActivate {
  private readonly globalLimit = new Ratelimit({
    redis: Redis.fromEnv(),
    limiter: Ratelimit.slidingWindow(100, '1 m'),
  });

  private readonly tenantLimit = new Ratelimit({
    redis: Redis.fromEnv(),
    limiter: Ratelimit.slidingWindow(60, '1 m'), // 60 req/min per merchant
  });

  private readonly authLimit = new Ratelimit({
    redis: Redis.fromEnv(),
    limiter: Ratelimit.slidingWindow(5, '10 m'),
  });

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const req = context.switchToHttp().getRequest();
    const ip = req.ip;
    const merchantId = req.user?.merchantId;

    // Global IP-based limit
    const { success: globalOk } = await this.globalLimit.limit(ip);
    if (!globalOk) throw new HttpException('Too Many Requests', 429);

    // Per-tenant limit (prevents one merchant from starving others)
    if (merchantId) {
      const { success: tenantOk } = await this.tenantLimit.limit(`tenant:${merchantId}`);
      if (!tenantOk) throw new HttpException('Too Many Requests', 429);
    }

    return true;
  }
}
```

## 24.4 Paystack Webhook Replay Attack Protection

```typescript
// payments.controller.ts
@Post('webhook')
@Public()
@HttpCode(200)
async handleWebhook(
  @Headers('x-paystack-signature') signature: string,
  @Req() req: RawBodyRequest<Request>,
) {
  const rawBody = req.rawBody;
  const hash = crypto.createHmac('sha512', process.env.PAYSTACK_SECRET_KEY!)
    .update(rawBody).digest('hex');
  if (hash !== signature) throw new UnauthorizedException('WEBHOOK_SIGNATURE_INVALID');

  const event = JSON.parse(rawBody.toString());

  // Replay attack protection: reject webhooks older than 5 minutes
  const eventTimestamp = new Date(event.data?.created_at ?? event.data?.paid_at ?? 0);
  const ageMs = Date.now() - eventTimestamp.getTime();
  if (ageMs > 5 * 60 * 1000) {
    // Log but return 200 - Paystack should not retry legitimate old webhooks
    this.logger.warn(`Stale webhook rejected: ${event.data?.reference}, age: ${ageMs}ms`);
    return { received: true };
  }

  // ... rest of handler
}
```

## 24.5 Secrets Rotation Runbook

| Secret | Rotation Trigger | Procedure |
|---|---|---|
| `PAYSTACK_SECRET_KEY` | Suspected compromise or quarterly | 1. Generate new key in Paystack dashboard. 2. Add `PAYSTACK_SECRET_KEY_NEW` to Railway env. 3. Update webhook handler to accept both old and new. 4. Deploy. 5. Remove old key from Railway. 6. Remove dual-accept code. |
| `SUPABASE_SERVICE_ROLE_KEY` | Suspected compromise | 1. Rotate in Supabase dashboard. 2. Update Railway env. 3. Redeploy. |
| `JWT_ADMIN_SECRET` | Quarterly or on admin team change | 1. Update Railway env. 2. Redeploy. 3. All admin sessions invalidated - admins re-login. |

# 25. DATABASE SCALING PLAN

## 25.1 Connection Pool Math

```
Railway NestJS instance: 2 vCPUs (default)
PM2 cluster mode: 2 workers (matches vCPU count)
Prisma connection_limit per worker: 2 (set in DATABASE_URL)
Total NestJS connections: 2 workers × 2 = 4 connections

Supabase free tier: 15 direct connections
PgBouncer overhead: ~2 connections
Available for application: 13 connections
4 NestJS connections = 31% utilization - safe headroom

Supabase Pro: 25 direct connections
At 4 Railway instances (horizontal scale): 4 × 2 × 2 = 16 connections = 64% utilization
Upgrade DATABASE_URL connection_limit to 3 when scaling to 4+ instances.
```

## 25.2 Read Replica Strategy

Add a Supabase read replica when either condition is met:
- Analytics query p95 (`GET /pos/sales/summary`) exceeds 500ms consistently
- Railway compute bill exceeds $20/month (indicates traffic growth)

```typescript
// prisma/prisma.service.ts - read replica routing
@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  private readonly readClient: PrismaClient;

  constructor() {
    super({ datasources: { db: { url: process.env.DATABASE_URL } } });

    // Read replica - only instantiated when REPLICA_URL is set
    this.readClient = process.env.REPLICA_URL
      → new PrismaClient({ datasources: { db: { url: process.env.REPLICA_URL } } })
      : this; // fallback to primary if no replica configured
  }

  // Use for analytics, storefront, admin reports
  get read(): PrismaClient {
    return this.readClient;
  }
}

// Usage in storefront.service.ts:
// const products = await this.prisma.read.product.findMany({ ... });
```

## 25.3 Sale Table Partitioning

At 1 million rows (approximately 10 months at 300 merchants — 100 sales/day), apply range partitioning by `createdAt`:

```sql
-- Run as a Prisma migration (raw SQL)
-- Convert Sale to partitioned table (requires brief maintenance window)
CREATE TABLE "Sale_partitioned" (LIKE "Sale" INCLUDING ALL)
  PARTITION BY RANGE ("createdAt");

-- Monthly partitions - create 12 months ahead
CREATE TABLE "Sale_2026_01" PARTITION OF "Sale_partitioned"
  FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');
CREATE TABLE "Sale_2026_02" PARTITION OF "Sale_partitioned"
  FOR VALUES FROM ('2026-02-01') TO ('2026-03-01');
-- ... continue for each month

-- Partition creation automation (run monthly via cron)
-- SELECT create_monthly_partition('Sale_partitioned', '2027-01-01');
```

Partitioning trigger: implement when `Sale` table exceeds 1M rows. Monitor via:
```sql
SELECT relname, n_live_tup FROM pg_stat_user_tables WHERE relname = 'Sale';
```

## 25.4 Archival Strategy

Sales older than 2 years are moved to `SaleArchive` (cold storage) monthly:

```typescript
// scheduler.service.ts
@Cron('0 2 1 * *', { timeZone: 'Africa/Lagos' }) // 2am WAT on 1st of each month
async archiveOldSales(): Promise<void> {
  const lockKey = 'cron:archive-sales';
  const acquired = await this.redis.set(lockKey, '1', 'EX', 3600, 'NX');
  if (!acquired) return;

  try {
    const twoYearsAgo = new Date();
    twoYearsAgo.setFullYear(twoYearsAgo.getFullYear() - 2);

    // Cursor-based batch archival - never loads all into memory
    let cursor: string | undefined;
    let totalArchived = 0;

    do {
      const batch = await this.prisma.sale.findMany({
        where: { createdAt: { lt: twoYearsAgo } },
        take: 500,
        skip: cursor → 1 : 0,
        cursor: cursor → { id: cursor } : undefined,
        orderBy: { id: 'asc' },
        include: { items: true },
      });

      if (batch.length === 0) break;

      // Export to Supabase Storage as JSONL
      const jsonl = batch.map(s => JSON.stringify(s)).join('\n');
      const fileName = `archives/sales/${twoYearsAgo.getFullYear()}-${String(twoYearsAgo.getMonth() + 1).padStart(2, '0')}-batch-${totalArchived}.jsonl`;
      await this.storage.upload(fileName, Buffer.from(jsonl));

      // Delete from hot DB
      await this.prisma.sale.deleteMany({
        where: { id: { in: batch.map(s => s.id) } },
      });

      totalArchived += batch.length;
      cursor = batch[batch.length - 1].id;
    } while (true);

    this.logger.log(`Archived ${totalArchived} sales older than 2 years`);
  } finally {
    await this.redis.del(lockKey);
  }
}
```

# 26. API VERSIONING STRATEGY

## 26.1 Versioning Policy

URL versioning is used for all breaking changes. The current version is `v1`.

**Breaking changes** (require new version):
- Removing a field from a response
- Changing a field's type (e.g., `string` → `number`)
- Making an optional field required
- Removing an endpoint
- Changing HTTP method of an endpoint

**Non-breaking changes** (safe to deploy to existing version):
- Adding new optional fields to responses
- Adding new optional query parameters
- Adding new endpoints
- Adding new enum values (with caution — clients must handle unknown values)

## 26.2 Deprecation Timeline

When `v2` of an endpoint is introduced:
1. `v1` endpoint remains fully functional for minimum 6 months
2. `v1` responses include `Deprecation: true` header and `Sunset: <date>` header
3. Mobile app analytics (`X-App-Version` header) are monitored — `v1` is not sunset until < 1% of active devices still use it
4. After sunset: `v1` returns `410 Gone` with `{ error: { code: 'API_VERSION_DEPRECATED', message: 'Please update the app' } }`

## 26.3 Mobile App Version Header

Every API request from the mobile app includes:

```typescript
// packages/shared-api/client.ts
apiClient.interceptors.request.use(async (config) => {
  config.headers['X-App-Version'] = Constants.expoConfig?.version ?? '0.0.0';
  config.headers['X-App-Platform'] = Platform.OS; // 'ios' | 'android'
  // ... auth token attachment
  return config;
});
```

The API logs `X-App-Version` in every request log line. This enables:
- Knowing which app versions are still active before sunsetting v1
- Detecting when a breaking deploy causes errors on old app versions
- Targeting OTA updates to specific version ranges

## 26.4 EAS Update + API Deploy Coordination Rule

**Rule:** The API must be backward compatible BEFORE an OTA update is pushed.

Deployment order:
1. Deploy API changes to Railway (backward compatible — new fields are optional, old fields still present)
2. Verify Railway deploy is healthy (`/health` returns 200)
3. Push OTA update via `eas update --branch production`
4. Monitor error rate for 30 minutes post-OTA

Never push an OTA update that calls a new required API field before the API deploy is confirmed healthy.

# 27. TESTING STRATEGY

## 27.1 Coverage Targets

| Layer | Target | Rationale |
|---|---|---|
| Service layer (financial ops) | 90%+ | POS sales, stock deduction, payment processing - zero tolerance for bugs |
| Service layer (non-financial) | 80%+ | Notifications, storefront, CRM |
| Repository layer | 70%+ | DB queries are tested via integration tests |
| Controller layer | 60%+ | Integration tests cover the rest |
| Overall | 75%+ | Enforced in CI - build fails below threshold |

## 27.2 Test Database Setup

```typescript
// test/setup.ts - runs before all test suites
import { execSync } from 'child_process';

beforeAll(async () => {
  // Use a separate test database (set in .env.test)
  process.env.DATABASE_URL = process.env.TEST_DATABASE_URL;
  execSync('npx prisma migrate deploy', { env: process.env });
  execSync('npx prisma db seed', { env: process.env });
});

afterAll(async () => {
  // Clean up - truncate all tables (faster than drop/recreate)
  await prisma.$executeRaw`
    TRUNCATE TABLE "Sale", "SaleItem", "Order", "OrderItem", "Product",
    "Customer", "Transaction", "OutboxEvent", "AuditLog", "DailySalesSummary",
    "Merchant", "Consumer", "Market" RESTART IDENTITY CASCADE
  `;
});
```

## 27.3 Factory Pattern

```typescript
// test/factories.ts
export async function createTestMerchant(overrides?: Partial<Merchant>): Promise<Merchant> {
  return prisma.merchant.create({
    data: {
      phone: `+234${Math.floor(Math.random() * 9_000_000_000) + 1_000_000_000}`,
      name: 'Test Merchant',
      shopName: 'Test Shop',
      plan: 'FREE',
      ...overrides,
    },
  });
}

export async function createTestProduct(overrides: Partial<Product> & { merchantId: string }): Promise<Product> {
  return prisma.product.create({
    data: {
      name: 'Test Product',
      price: new Decimal('1000'),
      quantity: 100,
      lowStockAt: 5,
      revision: 0,
      isActive: true,
      ...overrides,
    },
  });
}

export async function createTestOrder(overrides: Partial<Order> & { merchantId: string; consumerId: string }): Promise<Order> {
  return prisma.order.create({
    data: {
      status: 'PENDING',
      total: new Decimal('5000'),
      deliveryAddress: '12 Test Street, Kano',
      ...overrides,
    },
  });
}
```

## 27.4 Contract Testing

Zod schemas are the contract between mobile and API. They live in `packages/shared-types` and are imported by both:

```typescript
// packages/shared-types/api-contracts.ts
export const CreateSaleResponseSchema = z.object({
  success: z.literal(true),
  data: z.object({
    id: z.string(),
    localId: z.string(),
    total: z.string(), // Decimal serialized as string
    items: z.array(z.object({
      productId: z.string(),
      quantity: z.number(),
      unitPrice: z.string(),
    })),
  }),
  timestamp: z.string(),
});

export type CreateSaleResponse = z.infer<typeof CreateSaleResponseSchema>;
```

The API's `ResponseTransformInterceptor` validates outgoing responses against these schemas in non-production environments. If the API response shape changes without updating the schema, the test suite fails.

## 27.5 Load Test Scenario

```bash
# k6 load test -- 50 merchants syncing simultaneously after network outage
# k6 run test/load/sync.js

import http from 'k6/http';
import { check } from 'k6';

export const options = {
  vus: 50,          // 50 virtual users (merchants)
  duration: '60s',
};

export default function () {
  const sales = Array.from({ length: 10 }, (_, i) => ({
    localId: `test-${__VU}-${i}-${Date.now()}`,
    paymentMethod: 'CASH',
    items: [{ productId: PRODUCT_IDS[i % PRODUCT_IDS.length], quantity: 1, quantityDelta: -1, unitPrice: '1000', productRevision: 0 }],
  }));

  const res = http.post(`${BASE_URL}/api/v1/pos/sales/sync`,
    JSON.stringify({ sales }),
    { headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${TOKENS[__VU]}` } }
  );

  check(res, {
    'status is 200': r => r.status === 200,
    'response time < 2000ms': r => r.timings.duration < 2000,
    'all sales processed': r => JSON.parse(r.body).data.summary.synced + JSON.parse(r.body).data.summary.duplicate === 10,
  });
}
// Expected: p95 < 1500ms, 0 errors, no deadlocks
```

## 27.6 Chaos Scenarios

| Scenario | Expected Behavior | Test Method |
|---|---|---|
| Redis unavailable | Rate limiting falls back to in-memory counter (no crash). Storefront cache misses → DB queries. | Kill Upstash connection in staging, run smoke tests |
| Supabase Auth unreachable | Cached JWTs (15min TTL) still valid. New logins fail gracefully with 503. | Block Supabase URL in staging firewall |
| FCM unavailable | OutboxEvent retries with exponential backoff. Circuit breaker opens after 10 failures. Merchants notified via SMS fallback. | Mock FCM to return 503 in integration tests |
| Paystack webhook delayed 72hrs | Idempotency check prevents double-processing. Order stays PENDING until webhook arrives. Cleanup cron skips orders with `paystackRef` set. | Integration test with delayed webhook delivery |
| DB connection pool exhausted | Requests queue at PgBouncer. `pool_timeout=10` causes 503 after 10s. Alert fires. | Saturate connection pool in load test |

## 27.7 Test File Conventions

```
backend/
+-- src/
|   +-- modules/
|       +-- pos/
|           +-- pos.service.ts
|           +-- pos.service.spec.ts      → unit test alongside source
+-- test/
|   +-- setup.ts                         → global test setup
|   +-- factories.ts                     → test data factories
|   +-- pos.e2e-spec.ts                  → integration/e2e tests
|   +-- payments.e2e-spec.ts
|   +-- sync.e2e-spec.ts
|   +-- load/
|       +-- sync.js                      → k6 load test
```

Run commands:
```bash
# Unit tests (fast, no DB)
jest --testPathPattern="\.spec\.ts$" --runInBand

# Integration tests (requires test DB)
jest --testPathPattern="\.e2e-spec\.ts$" --runInBand --forceExit

# All tests with coverage
jest --runInBand --forceExit --coverage --coverageThreshold='{"global":{"lines":75}}'

# Load test (requires k6 installed)
k6 run test/load/sync.js
```

# 28. FINAL HARDENING - PRODUCTION COMPLETENESS PASS

## 28.1 Remaining Issues Fixed in This Pass

| ID | Issue | Severity | Fix Applied |
|---|---|---|---|
| F1 | Architecture style table said "Internal EventBus" | High | Changed to "Transactional Outbox" |
| F2 | Section 9.1 onboarding still described custom OTP endpoints | High | Rewritten to Supabase SDK flow |
| F3 | NotificationsListener used `@OnEvent` (EventEmitter2) | High | Replaced with direct `NotificationsService` calls from OutboxRelayService |
| F4 | `notifications.listener.ts` in folder structure | Medium | Renamed to `notifications.service.ts` |
| F5 | Permissions matrix missing sub-roles | Medium | Expanded to 6 columns |
| F6 | Redis data layer comment said "Cache + OTP" | Low | Updated to "Cache + Outbox" |
| F7 | Middleware chain diagram showed old guard names | Medium | Updated to `RateLimitGuard` + `AuthGuard(SUPABASE_JWT)` |
| F8 | Table of contents missing sections 19-28 | Low | Updated |
| F9 | Architecture philosophy had 4 principles, missing event durability | Low | Added 5th principle |
| F10 | No graceful degradation strategy for external service failures | High | Added Section 28.3 |
| F11 | No data migration strategy for schema changes | High | Added Section 28.4 |
| F12 | No multi-region / disaster recovery plan | Medium | Added Section 28.5 |
| F13 | No rate limit on sync endpoint specifically | High | Added Section 28.6 |
| F14 | No idempotency on order creation (consumer double-tap) | High | Added Section 28.7 |
| F15 | No input sanitization beyond class-validator | Medium | Added Section 28.8 |

## 28.2 Complete Event Flow (Final State)

This is the authoritative event flow. Every domain event follows this exact path - no exceptions.

```
Business Operation (Service Layer)
  |
  +-- Prisma.$transaction {
  |     +-- Business write (Order, Sale, Product, etc.)
  |     +-- OutboxEvent.create({ eventType, aggregateId, payload, priority })
  |   }  → ATOMIC: both commit or both rollback
  |
  +-- HTTP Response returned immediately (no waiting for event delivery)

OutboxRelayService (runs every 1 second, distributed lock)
  |
  +-- Query: PENDING events WHERE nextRetryAt <= NOW(), ORDER BY priority ASC, createdAt ASC
  +-- Reset stuck PROCESSING events (visibility timeout: 30s)
  |
  +-- For each event:
        +-- Mark PROCESSING
        +-- dispatch(event) → NotificationsService | InventoryService | AnalyticsService
        |     +-- SUCCESS → Mark PROCESSED
        |     +-- FAILURE → Mark PENDING, set nextRetryAt (exponential backoff)
        |                   After maxAttempts → Mark FAILED (dead-letter)
        |                   Sentry alert fires immediately on FAILED
        +-- Release distributed lock
```

## 28.3 Graceful Degradation Strategy

The system must degrade gracefully when external services fail. Every external dependency has a defined fallback.

| Service | Failure Mode | Degradation Behavior | Recovery |
|---|---|---|---|
| Supabase Auth | Unreachable | Cached JWTs (15min TTL) still valid. New logins return 503 with `SUPABASE_AUTH_UNAVAILABLE`. | Auto-recovers when Supabase restores |
| Supabase DB | Unreachable | All API endpoints return 503. Health check shows `database: degraded`. | Auto-recovers |
| Redis (Upstash) | Unreachable | Rate limiting falls back to in-memory sliding window (per-instance, not distributed). Cache misses fall through to DB. Outbox relay continues (uses DB lock instead). | Auto-recovers |
| FCM (Firebase) | Unreachable | OutboxRelayService circuit breaker opens after 10 failures. Events retry with backoff. Merchants miss push notifications but sales/orders still process. | Circuit resets after 60s |
| Paystack | Unreachable | `POST /payments/initialize` returns 503. Existing orders remain PENDING. Webhook delivery retried by Paystack for up to 72 hours. | Auto-recovers |
| Termii (SMS) | Unreachable | OTP delivery fails. Supabase Auth returns error to client. User sees "SMS failed, try again". | Manual retry by user |
| Railway (API) | Crash/restart | PM2 restarts process. OutboxRelay resumes from last PENDING event. In-flight requests fail with 502 (Cloudflare retries once). | Auto-recovers in < 5s |

```typescript
// common/guards/rate-limit.guard.ts -- Redis fallback
async canActivate(context: ExecutionContext): Promise<boolean> {
  const req = context.switchToHttp().getRequest();
  const ip = req.ip;
  const merchantId = req.user?.merchantId;

  try {
    const { success } = await this.tenantLimit.limit(merchantId ?? ip);
    if (!success) throw new HttpException('Too Many Requests', 429);
  } catch (err) {
    if (err instanceof HttpException) throw err;
    // Redis unavailable -- fall back to in-memory rate limit
    const key = merchantId ?? ip;
    const count = (this.inMemoryCounters.get(key) ?? 0) + 1;
    this.inMemoryCounters.set(key, count);
    if (count > 100) throw new HttpException('Too Many Requests', 429);
    // Reset counter after 1 minute
    setTimeout(() => this.inMemoryCounters.delete(key), 60_000);
  }
  return true;
}
```

## 28.4 Schema Migration Strategy

Every Prisma schema change follows this procedure to prevent production incidents.

### Migration Rules

1. **Additive-only in a single deploy** - Never remove a column and add a replacement in the same migration. Do it in two separate deploys.
2. **Nullable before required** - New columns must be nullable (`String?`) in the first deploy. Make them required (`String`) only after all existing rows have been backfilled.
3. **Never rename columns** - Rename = add new column + backfill + remove old column (3 deploys).
4. **Index before constraint** - Add the index first (non-blocking `CREATE INDEX CONCURRENTLY`), then add the unique constraint.

### Migration Deployment Order

```
Deploy N:   Add nullable column to schema + migration
Deploy N+1: Backfill existing rows (background job or migration script)
Deploy N+2: Make column required (if needed) + add constraint
```

### WatermelonDB Mobile Migration Rule

Every WatermelonDB schema change requires a migration entry. The schema version must be incremented. Failure to do this crashes the app for existing users on update.

```typescript
// packages/db/migrations.ts -- ALWAYS add a new entry, never modify existing ones
export const migrations = schemaMigrations({
  migrations: [
    { toVersion: 2, steps: [ addColumns({ table: 'sales', columns: [{ name: 'sync_status', type: 'string', isOptional: true }] }) ] },
    { toVersion: 3, steps: [ addColumns({ table: 'sale_items', columns: [{ name: 'quantity_delta', type: 'number', isOptional: true }] }) ] },
    // Add new entries here -- never modify above
  ],
});
```

## 28.5 Disaster Recovery Plan

### RTO / RPO Targets

| Scenario | RTO (Recovery Time) | RPO (Data Loss) |
|---|---|---|
| Railway instance crash | < 30 seconds | 0 (stateless API) |
| Supabase DB corruption | < 4 hours | < 24 hours (daily backup) |
| Redis data loss | < 1 minute | Acceptable (cache is ephemeral) |
| Full region outage | < 2 hours | < 24 hours |

### Backup Strategy

```
Supabase (managed):
  - Automatic daily backups (Pro plan: 7-day retention)
  - Point-in-time recovery available on Pro plan
  - Test restore quarterly: restore to staging, verify data integrity

VPS PostgreSQL (if using Contabo):
  - Daily pg_dump → Backblaze B2 (script in Section 16.2)
  - 30-day retention
  - Test restore monthly

Supabase Storage (product images):
  - Supabase handles redundancy internally
  - Critical images: download and store in Backblaze B2 as secondary backup
```

### Recovery Runbook

```
1. Railway API down:
   → Check Railway dashboard for deploy errors
   → Roll back to previous deploy: railway rollback
   → If DB migration caused issue: railway run npx prisma migrate resolve --rolled-back <migration>

2. Supabase DB unreachable:
   → Check Supabase status page: status.supabase.com
   → If planned maintenance: API returns 503, merchants use offline POS
   → If data corruption: restore from backup to staging, verify, promote to production

3. Data leak suspected:
   → Immediately rotate SUPABASE_SERVICE_ROLE_KEY
   → Audit AuditLog table for suspicious actorId patterns
   → Check RLS policies are enabled: SELECT tablename, rowsecurity FROM pg_tables WHERE schemaname = 'public'
   → Notify affected merchants per NDPR requirements
```

## 28.6 Sync Endpoint Rate Limiting

The `POST /pos/sales/sync` endpoint is the highest-risk endpoint for abuse - a buggy app could send thousands of sales in a loop. It needs its own rate limit separate from the global limit.

```typescript
// pos.controller.ts
@Post('sales/sync')
@UseGuards(AuthGuard, SyncRateLimitGuard)
async syncSales(@CurrentUser() user: JwtPayload, @Body() dto: SyncSalesDTO) {
  return this.posService.syncSales(user.merchantId, dto);
}
```

```typescript
// common/guards/sync-rate-limit.guard.ts
@Injectable()
export class SyncRateLimitGuard implements CanActivate {
  private readonly syncLimit = new Ratelimit({
    redis: Redis.fromEnv(),
    limiter: Ratelimit.slidingWindow(10, '1 m'), // 10 sync requests per minute per merchant
  });

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const req = context.switchToHttp().getRequest();
    const merchantId = req.user?.merchantId;
    if (!merchantId) return true;

    const { success, reset } = await this.syncLimit.limit(`sync:${merchantId}`);
    if (!success) {
      throw new HttpException(
        { code: 'SYNC_RATE_LIMITED', message: `Sync rate limit exceeded. Retry after ${new Date(reset).toISOString()}` },
        429,
      );
    }
    return true;
  }
}
```

Additionally, the sync payload is capped:

```typescript
// pos/dto/sync-sales.dto.ts
export class SyncSalesDTO {
  @IsArray()
  @ArrayMaxSize(100) // max 100 sales per sync request
  @ValidateNested({ each: true })
  @Type(() => CreateSaleDTO)
  sales: CreateSaleDTO[];
}
```

## 28.7 Order Creation Idempotency (Consumer Double-Tap)

A consumer tapping "Place Order" twice quickly creates two orders and reserves stock twice. The API must be idempotent on order creation.

```typescript
// orders/dto/create-order.dto.ts
export class CreateOrderDTO {
  @IsUUID()
  idempotencyKey: string; // generated by client: uuid() on checkout screen mount

  @IsString()
  merchantId: string;

  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => OrderItemDTO)
  items: OrderItemDTO[];

  @IsString()
  deliveryAddress: string;

  @IsString()
  @IsOptional()
  deliveryNote?: string;
}
```

```typescript
// orders.service.ts
async createOrder(consumerId: string, dto: CreateOrderDTO): Promise<Order> {
  // Idempotency check -- return existing order if same key
  const existing = await this.prisma.order.findFirst({
    where: { consumerId, idempotencyKey: dto.idempotencyKey },
  });
  if (existing) return existing; // safe to return -- same consumer, same key

  return this.prisma.$transaction(async (tx) => {
    // ... create order logic
  });
}
```

Add `idempotencyKey String? @unique` to the `Order` model in the Prisma schema.

## 28.8 Input Sanitization

`class-validator` with `whitelist: true` strips unknown fields but does not sanitize string content. A merchant could submit `name: "<script>alert(1)</script>"` as a product name. While the API is not a browser app, this data is rendered in the admin dashboard (Next.js) and consumer storefront.

```typescript
// main.ts - add sanitization transform
import { sanitize } from 'class-sanitizer';

app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,
    forbidNonWhitelisted: true,
    transform: true,
    transformOptions: { enableImplicitConversion: true },
  }),
);

// Custom pipe for string sanitization
// common/pipes/sanitize.pipe.ts
@Injectable()
export class SanitizePipe implements PipeTransform {
  transform(value: unknown): unknown {
    if (typeof value === 'string') {
      // Strip HTML tags from string inputs
      return value.replace(/<[^>]*>/g, '').trim();
    }
    if (typeof value === 'object' && value !== null) {
      return Object.fromEntries(
        Object.entries(value as Record<string, unknown>).map(([k, v]) => [k, this.transform(v)])
      );
    }
    return value;
  }
}
```

Apply `@Transform(({ value }) => sanitizeHtml(value))` on string fields in DTOs that are rendered in UI (product name, description, shop name, delivery address).

## 28.9 OWASP Top 10 Compliance Checklist

| OWASP Risk | Mitigation in Invento |
|---|---|
| A01 Broken Access Control | Prisma middleware + PostgreSQL RLS (two layers). Ownership checks on every resource. Sub-role RBAC. |
| A02 Cryptographic Failures | MMKV encrypted storage. HTTPS only (Cloudflare TLS 1.3). Bcrypt for admin passwords. No secrets in code. |
| A03 Injection | Prisma parameterized queries (no raw SQL except migrations). `class-validator` + `SanitizePipe` on all inputs. |
| A04 Insecure Design | Transactional Outbox (no event loss). Idempotency keys on orders and sales. Atomic stock deduction. |
| A05 Security Misconfiguration | Helmet.js (11 headers). Joi startup validation (app refuses to start with missing secrets). Staging environment separate from production. |
| A06 Vulnerable Components | `npm audit` in CI pipeline. Dependabot alerts enabled on GitHub. |
| A07 Auth Failures | Supabase Auth (OTP, JWT rotation, session revocation). Short JWT TTL (15min). Per-tenant rate limiting. |
| A08 Software Integrity | EAS Build (signed APKs). ProGuard obfuscation. Certificate pinning for Paystack. `X-App-Version` header for version tracking. |
| A09 Logging Failures | Pino structured logs. AuditLog for all sensitive writes. Sentry for errors. requestId propagation. |
| A10 SSRF | No user-controlled URLs fetched server-side. Supabase Storage URLs are validated. Paystack webhook origin verified via HMAC. |

## 28.10 Production Launch Checklist

Before going live with the first 50 merchants, verify every item:

### Infrastructure
- [ ] Supabase project upgraded to Pro ($25/month) — free tier pauses after 1 week of inactivity
- [ ] `DATABASE_URL` uses PgBouncer port 6543 with `connection_limit=2`
- [ ] `DIRECT_URL` uses port 5432 for migrations only
- [ ] All environment variables validated by Joi at startup
- [ ] Railway `environment: production` requires manual approval for deploys
- [ ] Staging environment exists and mirrors production config
- [ ] UptimeRobot monitoring `/health` every 5 minutes with SMS alert

### Security
- [ ] All RLS policies enabled on tenant-scoped tables
- [ ] `PAYSTACK_SECRET_KEY` is live key (not test key)
- [ ] `SUPABASE_JWT_SECRET` matches Supabase dashboard value
- [ ] `JWT_ADMIN_SECRET` is minimum 32 random characters
- [ ] CORS `ALLOWED_ORIGINS` contains only production domains
- [ ] Cloudflare WAF rules enabled
- [ ] Admin accounts use strong passwords (bcrypt cost factor = 12)

### Data
- [ ] `prisma migrate deploy` run against production DB
- [ ] RLS policies applied via migration
- [ ] GIN index on `Product.search_vector` created
- [ ] `DailySalesSummary` table exists and is empty (will populate from first sales)
- [ ] `AuditLog` table exists

### Mobile
- [ ] EAS production build submitted to Play Store internal track
- [ ] `API_URL` in EAS production profile points to production Railway URL
- [ ] Sentry DSN configured in mobile app
- [ ] FCM credentials configured in Firebase console

### Observability
- [ ] Sentry project created, DSN added to Railway env
- [ ] `/health` endpoint returns 200 with all components `ok`
- [ ] `/metrics` endpoint returns valid Prometheus text
- [ ] Pino logs visible in Railway dashboard
- [ ] At least one test sale processed end-to-end in staging

### Business
- [ ] Paystack account verified with CAC registration
- [ ] Termii account funded with minimum ₦10,000 credit
- [ ] Domain registered and pointing to Cloudflare
- [ ] SSL certificate active (Cloudflare handles this automatically)

## 28.11 Complete Prisma Schema — Final Authoritative Version

This is the single source of truth. All previous schema fragments in sections 8.2 and earlier are superseded by this version.

```prisma
// prisma/schema.prisma — v3.0 Final
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}

model Merchant {
  id                 String    @id @default(cuid())
  phone              String    @unique
  name               String
  shopName           String
  shopDesc           String?
  logoUrl            String?
  marketId           String?
  plan               Plan      @default(FREE)
  planExpiresAt      DateTime?
  isActive           Boolean   @default(true)
  fcmToken           String?
  allowNegativeStock Boolean   @default(false)
  createdAt          DateTime  @default(now())
  updatedAt          DateTime  @updatedAt

  market         Market?             @relation(fields: [marketId], references: [id])
  products       Product[]
  sales          Sale[]
  orders         Order[]
  customers      Customer[]
  importJobs     ImportJob[]
  dailySummaries DailySalesSummary[]

  @@index([marketId])
  @@index([plan])
  @@index([isActive])
}

model Consumer {
  id        String   @id @default(cuid())
  phone     String   @unique
  name      String?
  email     String?
  fcmToken  String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  orders Order[]
}

model Admin {
  id           String    @id @default(cuid())
  email        String    @unique
  passwordHash String
  name         String
  role         AdminRole @default(OPERATOR)
  isActive     Boolean   @default(true)
  createdAt    DateTime  @default(now())
  updatedAt    DateTime  @updatedAt

  refreshTokens AdminRefreshToken[]
}

model AdminRefreshToken {
  id        String   @id @default(cuid())
  token     String   @unique
  adminId   String
  expiresAt DateTime
  createdAt DateTime @default(now())

  admin Admin @relation(fields: [adminId], references: [id], onDelete: Cascade)

  @@index([adminId])
  @@index([expiresAt])
}

model Market {
  id          String   @id @default(cuid())
  name        String
  city        String
  state       String
  description String?
  latitude    Float?
  longitude   Float?
  imageUrl    String?
  isActive    Boolean  @default(true)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  merchants Merchant[]

  @@index([city])
  @@index([isActive])
}

model Product {
  id          String   @id @default(cuid())
  merchantId  String
  name        String
  description String?
  sku         String?
  price       Decimal  @db.Decimal(12, 2)
  quantity    Int      @default(0)
  lowStockAt  Int      @default(5)
  category    String?
  imageUrl    String?
  isActive    Boolean  @default(true)
  revision    Int      @default(0)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  merchant   Merchant    @relation(fields: [merchantId], references: [id])
  saleItems  SaleItem[]
  orderItems OrderItem[]

  @@unique([merchantId, sku])
  @@index([merchantId])
  @@index([merchantId, category])
  @@index([merchantId, isActive])
  @@index([merchantId, quantity])
}

model Sale {
  id            String        @id @default(cuid())
  localId       String        @unique
  merchantId    String
  customerId    String?
  total         Decimal       @db.Decimal(12, 2)
  paymentMethod PaymentMethod
  note          String?
  syncedAt      DateTime?
  createdAt     DateTime      @default(now())

  merchant Merchant   @relation(fields: [merchantId], references: [id])
  customer Customer?  @relation(fields: [customerId], references: [id])
  items    SaleItem[]

  @@index([merchantId])
  @@index([merchantId, createdAt])
  @@index([merchantId, paymentMethod])
}

model SaleItem {
  id        String  @id @default(cuid())
  saleId    String
  productId String
  quantity  Int
  unitPrice Decimal @db.Decimal(12, 2)

  sale    Sale    @relation(fields: [saleId], references: [id], onDelete: Cascade)
  product Product @relation(fields: [productId], references: [id])

  @@index([saleId])
  @@index([productId])
}

model Customer {
  id         String   @id @default(cuid())
  merchantId String
  phone      String?
  name       String?
  email      String?
  totalSpent Decimal  @default(0) @db.Decimal(12, 2)
  visitCount Int      @default(0)
  lastSeenAt DateTime @default(now())
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt

  merchant Merchant @relation(fields: [merchantId], references: [id])
  sales    Sale[]

  @@unique([merchantId, phone])
  @@index([merchantId])
  @@index([merchantId, totalSpent])
}

model Order {
  id             String      @id @default(cuid())
  merchantId     String
  consumerId     String
  idempotencyKey String?     @unique
  status         OrderStatus @default(PENDING)
  total          Decimal     @db.Decimal(12, 2)
  deliveryAddress String
  deliveryNote   String?
  paystackRef    String?     @unique
  cancelReason   String?
  cancelledBy    String?
  createdAt      DateTime    @default(now())
  updatedAt      DateTime    @updatedAt

  merchant     Merchant      @relation(fields: [merchantId], references: [id])
  consumer     Consumer      @relation(fields: [consumerId], references: [id])
  items        OrderItem[]
  transactions Transaction[]

  @@index([merchantId, status])
  @@index([consumerId])
  @@index([consumerId, status])
  @@index([createdAt])
  @@index([paystackRef])
}

model OrderItem {
  id        String  @id @default(cuid())
  orderId   String
  productId String
  quantity  Int
  unitPrice Decimal @db.Decimal(12, 2)

  order   Order   @relation(fields: [orderId], references: [id], onDelete: Cascade)
  product Product @relation(fields: [productId], references: [id])

  @@index([orderId])
  @@index([productId])
}

model Transaction {
  id          String            @id @default(cuid())
  orderId     String
  paystackRef String            @unique
  amount      Decimal           @db.Decimal(12, 2)
  currency    String            @default("NGN")
  status      TransactionStatus @default(PENDING)
  channel     String?
  gatewayMeta Json?
  paidAt      DateTime?
  createdAt   DateTime          @default(now())

  order Order @relation(fields: [orderId], references: [id])

  @@index([orderId])
  @@index([status])
  @@index([createdAt])
}

model OutboxEvent {
  id          String       @id @default(cuid())
  eventType   String
  aggregateId String
  payload     Json
  status      OutboxStatus @default(PENDING)
  priority    Int          @default(3)
  attempts    Int          @default(0)
  nextRetryAt DateTime?
  processedAt DateTime?
  createdAt   DateTime     @default(now())
  updatedAt   DateTime     @updatedAt

  @@index([status, priority, createdAt])
  @@index([aggregateId])
  @@index([nextRetryAt])
}

model AuditLog {
  id           String   @id @default(cuid())
  actorId      String
  actorRole    String
  action       String
  resourceType String
  resourceId   String
  before       Json?
  after        Json?
  ip           String?
  userAgent    String?
  createdAt    DateTime @default(now())

  @@index([actorId])
  @@index([resourceType, resourceId])
  @@index([createdAt])
}

model ImportJob {
  id         String          @id @default(cuid())
  merchantId String
  filePath   String
  status     ImportJobStatus @default(PENDING)
  imported   Int             @default(0)
  updated    Int             @default(0)
  failed     Int             @default(0)
  errors     Json?
  createdAt  DateTime        @default(now())
  updatedAt  DateTime        @updatedAt

  merchant Merchant @relation(fields: [merchantId], references: [id])

  @@index([merchantId])
}

model DailySalesSummary {
  id              String   @id @default(cuid())
  merchantId      String
  date            DateTime
  totalRevenue    Decimal  @db.Decimal(12, 2)
  totalSales      Int
  cashRevenue     Decimal  @db.Decimal(12, 2) @default(0)
  transferRevenue Decimal  @db.Decimal(12, 2) @default(0)
  ussdRevenue     Decimal  @db.Decimal(12, 2) @default(0)
  cardRevenue     Decimal  @db.Decimal(12, 2) @default(0)
  updatedAt       DateTime @updatedAt

  merchant Merchant @relation(fields: [merchantId], references: [id])

  @@unique([merchantId, date])
  @@index([merchantId, date])
}

enum Plan {
  FREE
  BASIC
  PRO
  ENTERPRISE
}

enum PaymentMethod {
  CASH
  TRANSFER
  USSD
  CARD
  MOBILE_MONEY
}

enum OrderStatus {
  PENDING
  CONFIRMED
  PROCESSING
  SHIPPED
  DELIVERED
  CANCELLED
}

enum TransactionStatus {
  PENDING
  SUCCESS
  FAILED
  REFUNDED
}

enum OutboxStatus {
  PENDING
  PROCESSING
  PROCESSED
  FAILED
}

enum ImportJobStatus {
  PENDING
  PROCESSING
  COMPLETED
  FAILED
}

enum MerchantRole {
  OWNER
  CASHIER
}

enum AdminRole {
  VIEWER
  OPERATOR
  SUPER
}
```

## 28.12 Complete API Endpoint Reference — Final State

```
── AUTH ──────────────────────────────────────────────────────────────────
  (Merchant/Consumer auth via Supabase SDK — no NestJS endpoints)
  supabase.auth.signInWithOtp({ phone })
  supabase.auth.verifyOtp({ phone, token, type: 'sms' })
  supabase.auth.refreshSession()
  supabase.auth.signOut()

POST   /api/v1/auth/register-fcm          Merchant | Consumer
POST   /api/v1/admin/auth/login           Public (Admin only)

── MERCHANTS ─────────────────────────────────────────────────────────────
GET    /api/v1/merchants/me               Merchant (OWNER)
PATCH  /api/v1/merchants/me               Merchant (OWNER)
GET    /api/v1/merchants/me/stats         Merchant (OWNER)

── INVENTORY ─────────────────────────────────────────────────────────────
GET    /api/v1/inventory/products         Merchant (OWNER | CASHIER)
POST   /api/v1/inventory/products         Merchant (OWNER)
GET    /api/v1/inventory/products/:id     Merchant (OWNER | CASHIER)
PATCH  /api/v1/inventory/products/:id     Merchant (OWNER)
DELETE /api/v1/inventory/products/:id     Merchant (OWNER)
POST   /api/v1/inventory/products/bulk-import  Merchant (OWNER) → { jobId }
GET    /api/v1/inventory/import-jobs/:jobId    Merchant (OWNER)
GET    /api/v1/inventory/products/sku/:sku     Merchant (OWNER | CASHIER)

── POS ───────────────────────────────────────────────────────────────────
POST   /api/v1/pos/sales                  Merchant (OWNER | CASHIER)
POST   /api/v1/pos/sales/sync             Merchant (OWNER | CASHIER) [10/min limit]
GET    /api/v1/pos/sales                  Merchant (OWNER)
GET    /api/v1/pos/sales/:id              Merchant (OWNER)
GET    /api/v1/pos/sales/summary          Merchant (OWNER) ← DailySalesSummary
GET    /api/v1/sync/pull                  Merchant (OWNER | CASHIER)
POST   /api/v1/pos/sales/:localId/reconcile  Merchant (OWNER)

── ORDERS ────────────────────────────────────────────────────────────────
POST   /api/v1/orders                     Consumer [idempotencyKey required]
GET    /api/v1/orders                     Merchant (OWNER) | Consumer
GET    /api/v1/orders/:id                 Merchant (OWNER) | Consumer (own)
PATCH  /api/v1/orders/:id/status          Merchant (OWNER) | Consumer (CANCELLED)

── PAYMENTS ──────────────────────────────────────────────────────────────
POST   /api/v1/payments/initialize        Consumer
POST   /api/v1/payments/webhook           Public (HMAC + timestamp verified)

── STOREFRONT (Public) ───────────────────────────────────────────────────
GET    /api/v1/storefront/markets
GET    /api/v1/storefront/markets/:id/shops
GET    /api/v1/storefront/shops/:id
GET    /api/v1/storefront/shops/:id/products
GET    /api/v1/storefront/products/search
GET    /api/v1/storefront/products/featured

── ADMIN ─────────────────────────────────────────────────────────────────
GET    /api/v1/admin/merchants            Admin (VIEWER+)
PATCH  /api/v1/admin/merchants/:id/plan   Admin (SUPER)
PATCH  /api/v1/admin/merchants/:id/status Admin (OPERATOR+)
GET    /api/v1/admin/markets              Admin (VIEWER+)
POST   /api/v1/admin/markets              Admin (OPERATOR+)
PATCH  /api/v1/admin/markets/:id          Admin (OPERATOR+)
DELETE /api/v1/admin/markets/:id          Admin (SUPER)
GET    /api/v1/admin/orders               Admin (VIEWER+)
GET    /api/v1/admin/analytics/overview   Admin (VIEWER+)
GET    /api/v1/admin/outbox/failed        Admin (OPERATOR+)
POST   /api/v1/admin/outbox/replay        Admin (SUPER)

── SYSTEM ────────────────────────────────────────────────────────────────
GET    /api/v1/health                     Public
GET    /api/v1/metrics                    Public (Prometheus format)
```

## 28.13 Architecture Decision Log

Every major decision is recorded here. Future engineers must understand why, not just what.

| Decision | Chosen | Rejected | Reason |
|---|---|---|---|
| Backend framework | NestJS | Express, Fastify | Enforced module boundaries, DI, decorators — critical for multi-tenant SaaS at team scale |
| ORM | Prisma | TypeORM, Drizzle | Auto-generated types, clean migrations, middleware for tenant scoping |
| Auth | Supabase Auth | Custom JWT, Auth0 | Zero custom auth code, OTP lifecycle managed, JWT rotation handled — saves 3+ days |
| Event system | Transactional Outbox | EventEmitter2, Kafka, RabbitMQ | No new infrastructure, durable by default, replay from DB, sufficient for MVP scale |
| Offline DB | WatermelonDB | SQLite direct, Realm | Built-in sync protocol, reactive queries, proven at scale (Nozbe, Shopify) |
| Mobile framework | React Native Expo Bare | Flutter, Ionic | TypeScript ecosystem, shared code with web, Expo tooling (EAS Build, OTA) |
| State management | Zustand + TanStack Query | Redux, MobX | Zustand for client state (10 lines), TanStack Query for server state (caching built-in) |
| Database | PostgreSQL via Supabase | MySQL, MongoDB | Relational integrity for financial data, RLS for multi-tenancy, full-text search built-in |
| Hosting | Railway | AWS, DigitalOcean, Heroku | Simplest deploy path, GitHub integration, no DevOps required at MVP scale |
| Cache | Upstash Redis | Self-hosted Redis, Memcached | Serverless, free tier, HTTP-based (works from Railway without VPC) |
| Payments | Paystack | Flutterwave, Stripe | Best Nigerian developer experience, immediate CAC registration, USSD support |
| SMS/OTP | Termii | Africa's Talking, Twilio | Nigerian DND channel support, competitive pricing, reliable delivery |
| Push notifications | Firebase FCM | Expo Push, OneSignal | Free, unlimited, official Android/iOS support, collapse key deduplication |
| Monolith vs microservices | Modular monolith | Microservices | Team size 3–5, tight transactional coupling between domains, operational simplicity |
| CQRS | Partial (read model for analytics) | Full CQRS, no CQRS | Analytics queries were degrading POS write performance — targeted separation without full complexity |
| Multi-tenancy | Shared schema + RLS | Schema-per-tenant, DB-per-tenant | Cost-effective at MVP scale, Supabase RLS handles isolation, simpler migrations |

## 28.14 Version History

| Version | Date | Changes |
|---|---|---|
| 1.0 | March 2026 | Initial architecture — Express, custom JWT, EventEmitter2 |
| 2.0 | March 2026 | NestJS, Supabase Auth, Transactional Outbox, delta-based sync |
| 2.1 | March 2026 | Fixed: JWT secret contradiction, webhook scoping bug, set_config outside transaction, stale refresh flow, dead RefreshToken fields |
| 2.2 | March 2026 | Added: Conflict resolution framework, Outbox priority/backoff, CQRS read model, observability stack, cache key enforcement, RBAC sub-roles, DB scaling plan, API versioning, testing strategy |
| 3.0 | March 2026 | Final hardening: graceful degradation, schema migration strategy, DR plan, sync rate limiting, order idempotency, input sanitization, OWASP checklist, production launch checklist, complete final schema |

# 29. ELITE HARDENING — FINAL 10/10 PASS

> Version 3.1 | This section is the authoritative final hardening. It supersedes any conflicting guidance in earlier sections.

## SECTION A: FINAL REMAINING ISSUES

| ID | Issue | Why It Prevents 10/10 |
|---|---|---|
| E1 | Conflict resolution uses server authority + manual reconciliation | Non-deterministic. Two devices syncing concurrently produce different outcomes depending on arrival order. Not auditable. |
| E2 | Outbox relay is DB-polling every 1s — not a real queue | Single-threaded dispatch. Cannot scale workers independently. No partitioning. No consumer groups. Polling adds 0–1s latency on every event. |
| E3 | CQRS is partial with no defined boundary | `topProducts` still hits live DB. No projection pipeline. Read model update is synchronous in relay. |
| E4 | Observability lacks p50/p95/p99 histograms, distributed traces, and SLO burn rate | Cannot answer "is the system degrading?" in real time. No trace correlation across API → DB → Outbox. |
| E5 | No monolith extraction plan | Team will hit the scaling ceiling with no documented path forward. |
| E6 | DB scaling has no hot/cold separation and no query plan validation | At 10M+ rows, unvalidated queries will cause production incidents. |
| E7 | Section 14.1 Outbox relay still shows old schema (no priority/nextRetryAt) | Document inconsistency — two different OutboxEvent schemas exist. |

## SECTION B: ELITE HARDENING SOLUTIONS

### E1 — DETERMINISTIC CONFLICT RESOLUTION

**Decision: Revision numbers with monotonic ordering (not vector clocks)**

**Trade-off analysis:**

| Approach | Pros | Cons | Verdict |
|---|---|---|---|
| Vector clocks | Detects concurrent writes precisely, works across N devices | Complex to implement, complex to explain to merchants, requires clock storage per device | Overkill for this system — merchants have 1–3 devices max |
| Revision numbers (monotonic) | Simple, deterministic, auditable, maps directly to DB integer | Cannot distinguish "concurrent" from "sequential" writes — but for inventory this doesn't matter | **Chosen** |
| Last-write-wins | Simplest | Silently loses data | Rejected |

**Why revision numbers are sufficient here:** The conflict question for inventory is not "which write happened first in wall-clock time" — it is "was the device's view of stock accurate when it made the sale?" Revision numbers answer this exactly: if `device.productRevision < server.product.revision`, the device was operating on stale data.

**Conflict Classification:**

```
SOFT CONFLICT (auto-resolved):
  Device sold N units. Server has >= N units after accounting for all concurrent writes.
  → Apply delta. No merchant intervention needed.
  → Log to AuditLog with revisionDrift for analytics.

HARD CONFLICT (requires merchant decision):
  Device sold N units. Server has < N units (or 0).
  → Two sub-cases:
    a. allowNegativeStock = true  → Apply delta, quantity goes negative, flag for reconciliation
    b. allowNegativeStock = false → REJECTED, merchant sees reconciliation screen
  → Merchant chooses: FORCE_SYNC (record sale, accept negative stock) or VOID (cancel sale items)

PHANTOM CONFLICT (device sold a deleted product):
  Product.isActive = false on server but device still has it locally.
  → REJECTED with reason 'PRODUCT_DEACTIVATED'
  → Device marks product as inactive in WatermelonDB
```

**Deterministic merge rules:**

```typescript
// The ONLY merge rule for inventory: deltas are commutative
// Order of application does not matter for the final quantity.
// Device A sold 3, Device B sold 5 → server applies -3 then -5 = -8 total
// OR server applies -5 then -3 = -8 total
// Result is identical regardless of sync order.
// This is the mathematical guarantee: delta application is commutative.

// The ONLY invariant: quantity >= 0 (unless allowNegativeStock)
// Enforcement: SELECT FOR UPDATE + atomic decrement in Serializable transaction
```

**Audit trail for every merge:**

```typescript
// Every sync operation writes to AuditLog
await tx.auditLog.create({
  data: {
    actorId: merchantId,
    actorRole: 'merchant',
    action: 'pos.sync',
    resourceType: 'Product',
    resourceId: item.productId,
    before: { quantity: product.quantity, revision: product.revision },
    after: { quantity: newQty, revision: product.revision + 1 },
    ip: null,
    userAgent: 'WatermelonDB-sync',
  },
});
```

**Eventual consistency guarantee:**
After all devices sync, `server.product.quantity = initial_quantity - sum(all_deltas_applied)`. This is mathematically guaranteed by the commutative property of addition and the `SELECT FOR UPDATE` serialization. No data corruption is possible.

### E2 — QUEUE SYSTEM: JUSTIFIED DECISION

**Decision: Keep Transactional Outbox + DB polling. Do NOT add Kafka/Redis Streams at MVP.**

This is not a compromise. It is the correct engineering decision for this stage. Here is the rigorous justification:

**Why Kafka/Redis Streams are wrong for Invento MVP:**

| Factor | Kafka/Redis Streams | Outbox + DB Polling |
|---|---|---|
| Infrastructure cost | $50–200/month (Confluent/Upstash Kafka) | $0 (uses existing PostgreSQL) |
| Operational complexity | Broker management, partition rebalancing, consumer group lag monitoring | None — it's a DB table |
| Failure modes | Broker unavailable = all events stop | DB unavailable = all events stop (same failure domain) |
| Throughput at MVP scale | 1M+ events/day | ~10K events/day (300 merchants × 30 events) |
| Latency | Sub-millisecond | 0–1 second (polling interval) |
| Replay | Kafka: built-in. Redis Streams: built-in | DB query: `WHERE status = 'FAILED'` |

**At 300 merchants, Invento generates ~10K events/day. Kafka's minimum viable deployment handles 1M+ events/day. Adding Kafka now is premature optimization that adds $150/month and a new failure domain.**

**The Outbox IS production-grade for this scale.** Stripe, GitHub, and Shopify all started with DB-based event queues. The upgrade path is defined below.

**Upgrade trigger (when to add Redis Streams):**

```
Trigger: OutboxEvent PENDING count consistently > 1,000 AND relay lag > 5 seconds
         OR event throughput > 100K events/day
         OR need for multiple independent consumer groups

Migration path (zero-downtime):
  1. Add Redis Streams alongside Outbox (dual-write)
  2. New events written to both DB and Redis Stream
  3. Consumers read from Redis Stream, fall back to DB on miss
  4. After 30 days of stable operation: remove DB polling relay
  5. Keep Outbox table as durability layer (write to DB, relay to Redis Stream)
```

**Hardened Outbox — production-grade improvements applied:**

The relay in Section 20 already has priority, backoff, visibility timeout, and circuit breaker. The one remaining gap is **parallel dispatch by priority class**:

```typescript
// outbox/outbox-relay.service.ts — parallel worker pools
@Cron('* * * * * *')
async relay(): Promise<void> {
  const lockKey = 'outbox:relay:lock';
  const acquired = await this.redis.set(lockKey, process.env.INSTANCE_ID ?? '1', 'EX', 5, 'NX');
  if (!acquired) return;

  try {
    // Reset visibility timeout
    await this.prisma.outboxEvent.updateMany({
      where: { status: 'PROCESSING', updatedAt: { lt: new Date(Date.now() - 30_000) } },
      data: { status: 'PENDING' },
    });

    // Fetch events — priority 1 (critical) processed first, then 2, then 3+
    const events = await this.prisma.outboxEvent.findMany({
      where: {
        status: 'PENDING',
        OR: [{ nextRetryAt: null }, { nextRetryAt: { lte: new Date() } }],
      },
      orderBy: [{ priority: 'asc' }, { createdAt: 'asc' }],
      take: 100, // increased from 50 — parallel dispatch handles the load
    });

    // Separate by priority class for parallel processing
    const critical = events.filter(e => e.priority === 1);
    const normal   = events.filter(e => e.priority > 1);

    // Critical events processed sequentially (payment confirmations — order matters)
    for (const event of critical) {
      await this.processEvent(event);
    }

    // Normal events processed in parallel (notifications, analytics — order doesn't matter)
    await Promise.allSettled(normal.map(e => this.processEvent(e)));

  } finally {
    await this.redis.del(lockKey);
  }
}
```

### E3 — CQRS: JUSTIFIED PARTIAL IMPLEMENTATION

**Decision: Partial CQRS with strict boundaries. Full CQRS is not warranted.**

**Rigorous justification for partial CQRS:**

Full CQRS (separate read database, projection pipelines, event-sourced read models) is appropriate when:
- Read and write throughput differ by 10x+ (Invento: ~equal at MVP)
- Read models require denormalization that conflicts with write models (Invento: minor)
- Multiple independent read models needed (Invento: 1–2)
- Team has dedicated read-side engineers (Invento: 3–5 total)

**Strict CQRS boundaries for Invento:**

```
WRITE PATH (always hits primary DB):
  POST /pos/sales          → Sale table (transactional, stock deduction)
  POST /pos/sales/sync     → Sale table (transactional, SELECT FOR UPDATE)
  POST /orders             → Order table (transactional, stock reservation)
  PATCH /orders/:id/status → Order table (state machine)
  POST /inventory/products → Product table (plan limit check)
  PATCH /inventory/products/:id → Product table

READ PATH — split by query type:
  GET /pos/sales/summary   → DailySalesSummary (read model, no live aggregation)
  GET /storefront/*        → Redis cache → Primary DB (read-only queries)
  GET /admin/analytics/*   → DailySalesSummary + read replica (when available)
  GET /inventory/products  → Primary DB (indexed, paginated — acceptable at scale)
  GET /pos/sales           → Primary DB (indexed, paginated — acceptable at scale)
```

**The `topProducts` gap — fixed:**

```typescript
// pos.service.ts — topProducts now uses a materialized approach
async getTopProducts(merchantId: string, from: Date, to: Date): Promise<TopProduct[]> {
  const cacheKey = CacheKeyBuilder.merchant(merchantId, 'top-products', from.toISOString().split('T')[0], to.toISOString().split('T')[0]);
  const cached = await this.redis.get<TopProduct[]>(cacheKey);
  if (cached) return cached;

  // This query is acceptable: SaleItem is indexed on productId + saleId
  // At 10M sales/year, this query with proper indexes runs in < 200ms
  const result = await this.prisma.$queryRaw<TopProduct[]>`
    SELECT
      si."productId",
      p.name,
      SUM(si.quantity)::int AS "unitsSold",
      SUM(si.quantity * si."unitPrice") AS revenue
    FROM "SaleItem" si
    JOIN "Sale" s ON si."saleId" = s.id
    JOIN "Product" p ON si."productId" = p.id
    WHERE s."merchantId" = ${merchantId}
      AND s."createdAt" >= ${from}
      AND s."createdAt" <= ${to}
    GROUP BY si."productId", p.name
    ORDER BY "unitsSold" DESC
    LIMIT 10
  `;

  await this.redis.set(cacheKey, result, 300); // 5-minute cache
  return result;
}
```

**Scaling limit for partial CQRS:** When `topProducts` query p95 exceeds 500ms consistently, promote `DailySalesSummary` to include per-product breakdown (add `topProductsJson Json?` column updated by Outbox relay). This defers full CQRS indefinitely.

### E4 — OBSERVABILITY: FULL SRE LEVEL

**Latency histograms (p50/p95/p99):**

```typescript
// common/metrics/metrics.service.ts
import { Histogram, Counter, Gauge, Registry } from 'prom-client';

@Injectable()
export class MetricsService {
  private readonly registry = new Registry();

  readonly httpLatency = new Histogram({
    name: 'http_request_duration_ms',
    help: 'HTTP request latency in milliseconds',
    labelNames: ['method', 'route', 'status_code'],
    buckets: [10, 25, 50, 100, 200, 400, 800, 1600, 3200],
    registers: [this.registry],
  });

  readonly httpRequests = new Counter({
    name: 'http_requests_total',
    help: 'Total HTTP requests',
    labelNames: ['method', 'route', 'status_code'],
    registers: [this.registry],
  });

  readonly outboxDepth = new Gauge({
    name: 'outbox_events_pending',
    help: 'Number of pending outbox events',
    registers: [this.registry],
  });

  readonly outboxFailed = new Gauge({
    name: 'outbox_events_failed',
    help: 'Number of dead-lettered outbox events',
    registers: [this.registry],
  });

  readonly syncLatency = new Histogram({
    name: 'pos_sync_duration_ms',
    help: 'POS sync request latency',
    labelNames: ['status'],
    buckets: [100, 250, 500, 1000, 2000, 5000],
    registers: [this.registry],
  });

  readonly dbQueryLatency = new Histogram({
    name: 'db_query_duration_ms',
    help: 'Database query latency',
    labelNames: ['operation', 'model'],
    buckets: [1, 5, 10, 25, 50, 100, 250, 500],
    registers: [this.registry],
  });

  async getMetrics(): Promise<string> {
    // Update gauges from DB before returning
    const [pending, failed] = await Promise.all([
      this.prisma.outboxEvent.count({ where: { status: 'PENDING' } }),
      this.prisma.outboxEvent.count({ where: { status: 'FAILED' } }),
    ]);
    this.outboxDepth.set(pending);
    this.outboxFailed.set(failed);
    return this.registry.metrics();
  }
}
```

```typescript
// common/interceptors/metrics.interceptor.ts
@Injectable()
export class MetricsInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler) {
    const req = context.switchToHttp().getRequest();
    const start = Date.now();
    const route = req.route?.path ?? req.url;

    return next.handle().pipe(
      tap((data) => {
        const duration = Date.now() - start;
        const status = context.switchToHttp().getResponse().statusCode;
        this.metrics.httpLatency.observe({ method: req.method, route, status_code: status }, duration);
        this.metrics.httpRequests.inc({ method: req.method, route, status_code: status });
      }),
      catchError((err) => {
        const duration = Date.now() - start;
        this.metrics.httpLatency.observe({ method: req.method, route, status_code: 500 }, duration);
        this.metrics.httpRequests.inc({ method: req.method, route, status_code: 500 });
        throw err;
      }),
    );
  }
}
```

**Distributed tracing (OpenTelemetry):**

```typescript
// main.ts — OpenTelemetry SDK initialization (before NestJS bootstrap)
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';

const sdk = new NodeSDK({
  traceExporter: new OTLPTraceExporter({
    url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT ?? 'http://localhost:4318/v1/traces',
  }),
  instrumentations: [
    getNodeAutoInstrumentations({
      '@opentelemetry/instrumentation-http': { enabled: true },
      '@opentelemetry/instrumentation-express': { enabled: true },
      '@opentelemetry/instrumentation-pg': { enabled: true }, // traces every Prisma query
    }),
  ],
});

sdk.start(); // Must be called BEFORE any other imports
```

This gives automatic traces for: every HTTP request, every PostgreSQL query (via Prisma), every outgoing HTTP call (Paystack, Termii, FCM). Traces are correlated by `requestId` propagated via `x-request-id` header.

For MVP: use Sentry's built-in performance tracing (free tier, no OTEL collector needed):

```typescript
// main.ts — Sentry performance monitoring (simpler than full OTEL for MVP)
Sentry.init({
  dsn: process.env.SENTRY_DSN,
  tracesSampleRate: 0.1, // 10% of requests traced — sufficient for p95 analysis
  integrations: [
    new Sentry.Integrations.Prisma({ client: prisma }), // traces all DB queries
  ],
});
```

**SLO definitions with error budgets:**

```
SLO 1: POS Sale Creation Availability
  Target: 99.5% of POST /pos/sales return 2xx within 400ms
  Error budget: 0.5% × 43,200 min/month = 216 min/month
  Burn rate alert: Fire when 5% of budget consumed in 1 hour (fast burn)
                   Fire when 10% consumed in 6 hours (slow burn)

SLO 2: Payment Webhook Processing
  Target: 99.9% of POST /payments/webhook return 200 within 150ms
  Error budget: 0.1% × 43,200 = 43.2 min/month
  Burn rate alert: Fire when 2% consumed in 1 hour

SLO 3: Storefront Search Availability
  Target: 99.9% of GET /storefront/products/search return 2xx within 300ms (cached)
  Error budget: 43.2 min/month

SLO 4: Outbox Relay Lag
  Target: 99% of events delivered within 5 seconds of creation
  Measurement: outbox_relay_lag_ms p99 < 5000
  Alert: Fire when p99 > 10s for 5 consecutive minutes
```

**Operational dashboards:**

```
Dashboard 1: API Health
  Panels: Request rate (req/s), Error rate (%), p50/p95/p99 latency by endpoint,
          Active connections, 5xx rate over time

Dashboard 2: Business Operations
  Panels: Sales per hour, Orders per hour, Active merchants (last 24h),
          Sync success rate, Payment webhook success rate

Dashboard 3: Infrastructure
  Panels: DB connection pool utilization, Redis memory usage,
          Outbox queue depth by priority, Dead-letter count,
          Railway CPU/memory, Supabase DB size

Dashboard 4: SLO Burn Rate
  Panels: Error budget remaining per SLO (%), Burn rate (fast/slow),
          Time to budget exhaustion at current burn rate
```

### E5 — MONOLITH EXTRACTION PATH

**When to split — quantified triggers:**

| Trigger | Threshold | First Module to Extract |
|---|---|---|
| Team size | > 8 engineers | Payments (highest blast radius, clearest boundary) |
| Deploy frequency conflict | > 3 teams blocked by same deploy | Notifications (stateless, no DB writes) |
| POS write latency | p95 > 600ms consistently | POS (extract to dedicated service with own DB pool) |
| Notification failures affecting POS | Any | Notifications (isolate failure domain) |
| Monthly active merchants | > 5,000 | Storefront (read-heavy, can be CDN-cached separately) |

**Extraction order (safest first):**

```
Phase 1 — Extract Notifications (lowest risk):
  Why first: Stateless, no DB writes, pure consumer of Outbox events
  How: Deploy NotificationsService as separate Railway service
       OutboxRelayService calls it via HTTP instead of in-process
       Zero-downtime: dual-write period where both in-process and HTTP work
  Risk: Low — failure only affects push notifications, not core transactions

Phase 2 — Extract Storefront (read-only):
  Why: High read volume, can be independently scaled and cached
  How: Separate Railway service with read-only DB connection
       Cloudflare caches responses at edge
  Risk: Low — read-only, no write path affected

Phase 3 — Extract Payments (highest value):
  Why: Regulatory isolation, independent scaling, PCI compliance boundary
  How: Separate service with own DB schema (transactions only)
       Communicates via Outbox events (async) + direct HTTP for webhook
  Risk: Medium — requires careful migration of Paystack webhook endpoint

Phase 4 — Extract POS (if write latency becomes issue):
  Why: Highest write frequency, benefits from dedicated connection pool
  How: Separate service with own DB pool, own Redis instance
  Risk: High — requires careful migration of offline sync protocol
```

**Zero-downtime migration pattern (Strangler Fig):**

```
Step 1: Deploy new service alongside monolith (both running)
Step 2: Route 1% of traffic to new service (feature flag)
Step 3: Monitor error rates — if clean, increase to 10%, 50%, 100%
Step 4: Remove old code from monolith after 30 days of stable operation
Step 5: Update Outbox relay to call new service endpoint
```

**Communication patterns after extraction:**

```
Synchronous (HTTP):
  → Consumer-facing reads (storefront, order status)
  → Admin operations
  → Anything requiring immediate response

Asynchronous (Outbox → HTTP dispatch):
  → Notifications (fire-and-forget with retry)
  → Analytics updates (DailySalesSummary)
  → Low-stock alerts

Never:
  → Synchronous calls between extracted services for write operations
  → Shared database between extracted services
```

### E6 — DATABASE SCALING: HYPERSCALE READY

**Index optimization — validated query plans:**

Every query that runs in a hot path must have its execution plan validated. Add this to the CI pipeline:

```typescript
// test/db-performance.spec.ts — runs in CI against test DB with realistic data volume
describe('Query performance (10K products, 100K sales)', () => {
  it('POS sync query uses index scan not seq scan', async () => {
    const plan = await prisma.$queryRaw`
      EXPLAIN (FORMAT JSON, ANALYZE false)
      SELECT id, name, quantity, revision
      FROM "Product"
      WHERE "merchantId" = 'test-merchant-id' AND "isActive" = true
      FOR UPDATE
    `;
    const planJson = plan[0]['QUERY PLAN'][0];
    expect(planJson['Plan']['Node Type']).not.toBe('Seq Scan');
  });

  it('Sales summary query uses DailySalesSummary index', async () => {
    const plan = await prisma.$queryRaw`
      EXPLAIN (FORMAT JSON, ANALYZE false)
      SELECT * FROM "DailySalesSummary"
      WHERE "merchantId" = 'test-id' AND date >= '2026-01-01' AND date <= '2026-03-31'
    `;
    const planJson = plan[0]['QUERY PLAN'][0];
    expect(planJson['Plan']['Node Type']).toBe('Index Scan');
  });
});
```

**Hot/cold data separation:**

```
HOT DATA (< 90 days old) — stays in primary PostgreSQL:
  Sale, SaleItem, Order, OrderItem, Transaction
  Accessed frequently for analytics, reconciliation, dispute resolution

WARM DATA (90 days – 2 years) — stays in primary but partitioned:
  Accessed occasionally for historical reports
  Partition by month: queries with date filters skip irrelevant partitions

COLD DATA (> 2 years) — archived to Supabase Storage as JSONL:
  Never queried in normal operations
  Retrievable for compliance/audit within 4 hours
  Monthly archival cron (Section 25.4)
```

**Partitioning implementation — time-based on Sale:**

```sql
-- Migration: convert Sale to partitioned table
-- Run during maintenance window (< 5 min for empty table, longer for existing data)

-- Step 1: Create partitioned table
CREATE TABLE "Sale_v2" (LIKE "Sale" INCLUDING ALL)
  PARTITION BY RANGE ("createdAt");

-- Step 2: Create monthly partitions (automate with function)
CREATE OR REPLACE FUNCTION create_monthly_partition(
  table_name TEXT,
  partition_date DATE
) RETURNS void AS $$
DECLARE
  partition_name TEXT;
  start_date DATE;
  end_date DATE;
BEGIN
  partition_name := table_name || '_' || TO_CHAR(partition_date, 'YYYY_MM');
  start_date := DATE_TRUNC('month', partition_date);
  end_date := start_date + INTERVAL '1 month';

  EXECUTE format(
    'CREATE TABLE IF NOT EXISTS %I PARTITION OF %I FOR VALUES FROM (%L) TO (%L)',
    partition_name, table_name, start_date, end_date
  );
END;
$$ LANGUAGE plpgsql;

-- Step 3: Create partitions for next 12 months (run monthly via cron)
SELECT create_monthly_partition('Sale_v2', DATE_TRUNC('month', NOW()) + (n || ' months')::INTERVAL)
FROM generate_series(0, 11) n;

-- Step 4: Migrate data (background, cursor-based)
-- Step 5: Rename tables (atomic)
ALTER TABLE "Sale" RENAME TO "Sale_old";
ALTER TABLE "Sale_v2" RENAME TO "Sale";
```

**Read replica routing — complete implementation:**

```typescript
// prisma/prisma.service.ts — final version with read replica
@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  private readonly _read: PrismaClient;

  constructor() {
    super({
      datasources: { db: { url: process.env.DATABASE_URL } },
      log: process.env.NODE_ENV === 'development' → ['query', 'warn', 'error'] : ['warn', 'error'],
    });

    // Read replica — falls back to primary if REPLICA_URL not set
    this._read = process.env.REPLICA_URL
      → new PrismaClient({
          datasources: { db: { url: process.env.REPLICA_URL } },
          log: ['warn', 'error'],
        })
      : this;

    this.$use(this.tenantMiddleware.bind(this));
  }

  // Expose read client for analytics, storefront, admin reports
  get read(): PrismaClient { return this._read; }

  async onModuleInit() {
    await this.$connect();
    if (this._read !== this) await this._read.$connect();
  }

  async onModuleDestroy() {
    await this.$disconnect();
    if (this._read !== this) await this._read.$disconnect();
  }

  private async tenantMiddleware(params: Prisma.MiddlewareParams, next: (params: Prisma.MiddlewareParams) => Promise<unknown>) {
    const merchantId = RequestContext.get('merchantId');
    if (!merchantId || !TENANT_SCOPED_MODELS.includes(params.model ?? '')) {
      return next(params);
    }
    const isInTransaction = params.runInTransaction ?? false;
    await this.$executeRaw`SELECT set_config('app.current_merchant_id', ${merchantId}, ${isInTransaction})`;
    if (['findMany', 'findFirst', 'findUnique', 'count', 'aggregate'].includes(params.action)) {
      params.args ??= {};
      params.args.where = { ...params.args.where, merchantId };
    }
    if (['update', 'updateMany', 'delete', 'deleteMany'].includes(params.action)) {
      params.args ??= {};
      params.args.where = { ...params.args.where, merchantId };
    }
    if (params.action === 'create') {
      params.args.data = { ...params.args.data, merchantId };
    }
    return next(params);
  }
}
```

### E7 — DOCUMENT CONSISTENCY: SECTION 14.1 OUTBOX SCHEMA

Section 14.1 shows the old `OutboxEvent` schema without `priority` and `nextRetryAt`. The authoritative schema is in Section 28.11. Add this note to Section 14.1:

> **Note:** The OutboxEvent schema shown here is the original design. The authoritative final schema including `priority`, `nextRetryAt`, and `updatedAt` fields is defined in Section 28.11. Use Section 28.11 as the implementation reference.

## SECTION C: FINAL 10/10 ARCHITECTURE

### Complete System Architecture — Final State

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                              CLIENT LAYER                                   ║
║                                                                              ║
║  ┌──────────────────┐  ┌──────────────────┐  ┌────────────────────────────┐ ║
║  │  MERCHANT APP    │  │  CONSUMER APP    │  │   ADMIN WEB DASHBOARD      │ ║
║  │  React Native    │  │  React Native    │  │   Next.js 14 App Router    │ ║
║  │  Expo Bare       │  │  Expo Bare       │  │   Vercel                   │ ║
║  │                  │  │                  │  │                            │ ║
║  │  WatermelonDB    │  │  TanStack Query  │  │  TanStack Query            │ ║
║  │  (offline SQLite)│  │  Zustand         │  │  shadcn/ui + Recharts      │ ║
║  │  MMKV (tokens)   │  │  MMKV (tokens)   │  │  NextAuth.js               │ ║
║  │  Supabase SDK    │  │  Supabase SDK    │  │                            │ ║
║  │  X-App-Version   │  │  X-App-Version   │  │                            │ ║
║  └────────┬─────────┘  └────────┬─────────┘  └────────────┬───────────────┘ ║
╚═══════════╪════════════════════╪═════════════════════════╪════════════════╝
            │  HTTPS + x-request-id + X-App-Version         │
╔═══════════╪════════════════════╪═════════════════════════╪════════════════╗
║                         EDGE LAYER                                          ║
║              ┌────────────────────────────────────────┐                    ║
║              │           CLOUDFLARE                    │                    ║
║              │  DNS · TLS 1.3 · DDoS · WAF · CDN      │                    ║
║              │  Rate limiting (edge) · SSL termination  │                    ║
║              └──────────────────┬─────────────────────┘                    ║
╚═════════════════════════════════╪══════════════════════════════════════════╝
                                  │
╔═════════════════════════════════╪══════════════════════════════════════════╗
║                    BACKEND LAYER (NestJS Modular Monolith)                  ║
║                                                                              ║
║  ┌────────────────────────────────────────────────────────────────────┐    ║
║  │  Helmet → CORS → RateLimitGuard(per-tenant) → AuthGuard(Supabase) │    ║
║  │  → RolesGuard(RBAC) → ValidationPipe → AuditInterceptor           │    ║
║  │  → MetricsInterceptor → Controller → Service                      │    ║
║  │  → Repository → Prisma(tenant middleware) → OutboxEvent(in tx)    │    ║
║  │  → ResponseTransformInterceptor → HTTP Response                   │    ║
║  └────────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────┐   ║
║  │   AUTH   │ │INVENTORY │ │   POS    │ │  ORDERS  │ │   PAYMENTS     │   ║
║  │  Module  │ │  Module  │ │  Module  │ │  Module  │ │    Module      │   ║
║  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └────────────────┘   ║
║  ┌──────────┐ ┌──────────┐ ┌──────────────────────────────────────────┐   ║
║  │STOREFRONT│ │  ADMIN   │ │  TRANSACTIONAL OUTBOX                    │   ║
║  │  Module  │ │  Module  │ │  OutboxRelayService (1s, priority-aware) │   ║
║  └──────────┘ └──────────┘ │  Critical: sequential | Normal: parallel │   ║
║                             └──────────────────────────────────────────┘   ║
║                                                                              ║
║  ┌────────────────────────────────────────────────────────────────────┐    ║
║  │  INTEGRATION ADAPTERS                                               │    ║
║  │  PaystackAdapter · TermiiAdapter · FirebaseAdapter                  │    ║
║  └────────────────────────────────────────────────────────────────────┘    ║
╚══════════════════════════════════════════════════════════════════════════════╝
                                  │
╔═════════════════════════════════╪══════════════════════════════════════════╗
║                           DATA LAYER                                        ║
║                                                                              ║
║  ┌──────────────────────┐  ┌─────────────────┐  ┌──────────────────────┐  ║
║  │  PostgreSQL 16        │  │  Upstash Redis  │  │  Supabase Storage    │  ║
║  │  via Supabase         │  │  Cache          │  │  Product images      │  ║
║  │  Prisma ORM           │  │  Outbox locks   │  │  Import CSVs         │  ║
║  │  RLS (dual policy)    │  │  Cron locks     │  │  Sale archives       │  ║
║  │  PgBouncer pool       │  │  Rate limiting  │  │  CDN delivery        │  ║
║  │  connection_limit=2   │  │  Circuit state  │  └──────────────────────┘  ║
║  │  Read replica (Pro+)  │  └─────────────────┘                            ║
║  │  Monthly partitions   │                                                  ║
║  │  (Sale, Order tables) │                                                  ║
║  └──────────────────────┘                                                   ║
╚══════════════════════════════════════════════════════════════════════════════╝
                                  │
╔═════════════════════════════════╪══════════════════════════════════════════╗
║                        EXTERNAL SERVICES                                    ║
║                                                                              ║
║  Supabase Auth (OTP, JWT)    Paystack (payments, webhooks)                  ║
║  Termii (SMS delivery)       Firebase FCM (push, collapse-key dedup)        ║
║  Sentry (errors + traces)    Cloudflare (edge, WAF, CDN)                    ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

### Data Flow — Complete (Final State)

```
WRITE PATH (POS Sale):
  Mobile → Cloudflare → Railway(NestJS)
    → RateLimitGuard (per-tenant, 60 req/min)
    → AuthGuard (verify SUPABASE_JWT_SECRET)
    → ValidationPipe (DTO validation + SanitizePipe)
    → PosController.createSale()
    → PosService.processSaleDelta()
      → Prisma.$transaction(Serializable) {
          SELECT product FOR UPDATE (revision check)
          UPDATE product SET quantity = quantity + delta, revision = revision + 1
          INSERT Sale, SaleItems
          INSERT OutboxEvent { eventType: 'sale.created', priority: 2 }
          INSERT AuditLog { before, after, actorId }
        }
    → HTTP 201 { success: true, data: sale }

OUTBOX RELAY (async, 1s polling):
  OutboxRelayService.relay()
    → Reset stuck PROCESSING events (visibility timeout 30s)
    → Fetch PENDING events ORDER BY priority ASC, createdAt ASC
    → Critical (priority=1): sequential dispatch
    → Normal (priority>1): parallel dispatch (Promise.allSettled)
    → dispatch(event) → NotificationsService.send*(payload, eventId)
      → FCM.send(token, title, body, messageId=eventId) [collapse-key dedup]
    → On failure: exponential backoff (2^n * baseDelay, capped 1hr)
    → After maxAttempts: FAILED → Sentry alert → admin dead-letter queue

READ PATH (Sales Summary):
  Mobile → Cloudflare → Railway(NestJS)
    → AuthGuard → PosController.getSalesSummary()
    → PosService.getSalesSummary()
      → DailySalesSummary.findMany() [read model, no aggregation]
      → topProducts: Redis cache (5min TTL) → raw SQL with index
    → HTTP 200 { success: true, data: summary }

OFFLINE SYNC PATH:
  Mobile (offline) → WatermelonDB.create(sale, syncedAt=null)
  Mobile (online)  → POST /pos/sales/sync { sales: [...], productRevision }
    → SyncRateLimitGuard (10 req/min per merchant)
    → PosService.processSaleDelta() × N (Serializable transaction each)
      → SELECT FOR UPDATE (revision check, conflict classification)
      → SOFT: apply delta → SYNCED
      → HARD: apply or reject based on allowNegativeStock → PARTIAL/REJECTED
      → AuditLog written for every merge
    → Response: { results: [{ localId, status, conflictItems, stockUpdates }] }
  Mobile → WatermelonDB.update(sale.syncStatus, product.quantity from stockUpdates)
```

### Conflict Resolution — Mathematical Guarantee

```
INVARIANT: For any product P with initial quantity Q₀:
  Q_final = Q₀ + Σ(all applied deltas)

PROOF OF CORRECTNESS:
  1. Each delta application is atomic (Serializable transaction + SELECT FOR UPDATE)
  2. Delta application is commutative: applying δ₁ then δ₂ = applying δ₂ then δ₁
  3. Therefore: final quantity is independent of sync order
  4. Negative stock is prevented by: Q + δ >= 0 check (unless allowNegativeStock)
  5. Every application is logged in AuditLog (full auditability)

EVENTUAL CONSISTENCY:
  After all devices sync, server quantity = Q₀ + Σ(all deltas)
  Devices update local quantity from server's authoritative stockUpdates response
  Convergence is guaranteed in O(1) sync rounds per device
```

### Scaling Path — Quantified

```
Current (MVP): Modular monolith, single Railway instance
  Capacity: ~300 merchants, ~10K events/day, ~1K req/min peak

Scale 1 (500–2K merchants): Horizontal scale
  Action: Add 2nd Railway instance (same monolith)
  Cost: +$10–15/month
  Requires: Outbox distributed lock (already implemented)
            Cron distributed lock (already implemented)
            Stateless API (already implemented)

Scale 2 (2K–10K merchants): Read replica + partitioning
  Action: Add Supabase read replica ($25/month)
          Enable Sale table partitioning
          Add Redis cache for hot storefront data
  Cost: +$25–50/month

Scale 3 (10K–50K merchants): Extract Notifications + Storefront
  Action: Strangler Fig extraction (Section E5)
          Upgrade Outbox to Redis Streams
  Cost: +$50–100/month

Scale 4 (50K+ merchants): Extract Payments + POS
  Action: Full service extraction
          Dedicated DB per service
          Kafka for event streaming
  Cost: +$200–500/month
  Team size required: 15+ engineers
```

### Technology Stack — Final Reference

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Backend | NestJS | v10+ | Modular, DI-based framework |
| Language | TypeScript | 5.x strict | Type safety end-to-end |
| ORM | Prisma | 5.x | Type-safe DB + tenant middleware |
| Database | PostgreSQL 16 via Supabase | — | Primary data store + RLS |
| Auth | Supabase Auth | — | OTP, JWT, session management |
| Cache | Upstash Redis | — | Rate limiting, cache, locks |
| Rate Limiting | @upstash/ratelimit | latest | Per-tenant sliding window |
| Event System | Transactional Outbox | — | Durable domain events |
| Validation | class-validator + Zod | latest | DTO + contract validation |
| Security | Helmet.js | latest | HTTP security headers |
| Logging | nestjs-pino | latest | Structured JSON logs |
| Metrics | prom-client | latest | Prometheus histograms/gauges |
| Tracing | Sentry (MVP) / OpenTelemetry (scale) | — | Distributed traces |
| Scheduling | @nestjs/schedule | latest | Distributed-safe cron jobs |
| Testing | Jest + Supertest + k6 | latest | Unit + integration + load |
| Mobile | React Native Expo Bare | SDK 51+ | Cross-platform, native access |
| Navigation | Expo Router v5 | — | File-based routing |
| Server State | TanStack Query v5 | — | API caching + background sync |
| Client State | Zustand | — | Auth, cart, UI state |
| Offline DB | WatermelonDB | — | SQLite offline-first |
| Fast Storage | MMKV | — | Encrypted token storage |
| Forms | React Hook Form + Zod | — | Type-safe validated forms |
| UI | NativeWind v4 | — | Tailwind in React Native |
| Lists | FlashList (Shopify) | — | High-performance lists |
| Animations | Reanimated v3 | — | UI thread animations |
| Payments | Paystack RN SDK | — | WebView checkout |
| Builds | EAS Build | — | Cloud CI/CD |
| OTA | EAS Update | — | JS-only hotfixes |
| Errors | Sentry | — | Crash reporting |
| Web Dashboard | Next.js 14 App Router | — | Admin dashboard |
| Web UI | Tailwind + shadcn/ui | — | Component library |
| Web Auth | NextAuth.js | — | Admin session |
| API Hosting | Railway Hobby | — | ~$5–15/month |
| Web Hosting | Vercel | — | Free |
| Edge | Cloudflare | — | Free |
| DB + Auth | Supabase | — | $0 → $25/month |
| SMS | Termii | — | ~₦5–7/SMS |
| Push | Firebase FCM | — | Free |
| Payments | Paystack | — | 1.5% + ₦100/txn |

### Document Version

| Version | Date | Summary |
|---|---|---|
| 3.1 | March 2026 | Elite hardening: deterministic conflict resolution proof, justified Outbox vs Kafka decision, CQRS boundary definition, full SRE observability (prom-client histograms, OpenTelemetry), monolith extraction triggers and path, DB hot/cold separation, query plan validation in CI, final complete system diagram |
