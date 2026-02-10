# hl_overview

High level overview of the codebase

# Project Analysis: OMS System

## 0. Repository Name
[[oms-system]]

## 1. Project Purpose

This is an **Order Management System (OMS)** designed for logistics and distribution operations, specifically tailored for businesses that handle:

- **Cylinder/Container Distribution**: The system tracks empty cylinders, refills, and conversions (evident from cylinder balance tracking, refill detection, and conversion workflows)
- **Multi-tenant B2B Operations**: Supports multiple tenants (businesses) with their own customers, products, warehouses, and vehicles
- **Field Operations Management**: Driver trip planning, delivery tracking, proof-of-delivery (POD), and variance management
- **Inventory Management**: Real-time inventory tracking across warehouses (including mobile warehouses on vehicles), stock allocation, and reconciliation

The primary domain is **logistics and distribution** for companies that need to manage:
- Customer orders and deliveries
- Trip planning and driver dispatch
- Inventory across fixed and mobile warehouses
- Payment collection and variance tracking
- Sales agent territories and customer relationships

## 2. Architecture Pattern

**Hybrid Architecture combining:**
- **Layered Architecture** (Backend): Routes → Handlers → Services → Database
- **Event-Driven Architecture**: Event store, pub/sub patterns via NATS, webhook integrations
- **Domain-Driven Design (DDD)**: Clear domain boundaries (Orders, Trips, Inventory, Customers)
- **Serverless/Edge Functions**: Supabase Edge Functions for specific operations
- **API-First Design**: RESTful backend with OpenAPI schemas

## 3. Technology Stack

### Frontend
| Technology | Purpose |
|------------|---------|
| **React 18** | UI Framework |
| **TypeScript** | Type-safe JavaScript |
| **Vite** | Build tool and dev server |
| **TailwindCSS** | Utility-first CSS framework |
| **React Router** | Client-side routing |
| **TanStack Query (React Query)** | Server state management |
| **Radix UI** | Headless UI components |
| **Recharts** | Charting library |
| **Supabase JS Client** | Backend communication |
| **Sentry** | Error tracking |
| **Mixpanel** | Product analytics |

### Backend
| Technology | Purpose |
|------------|---------|
| **Node.js** | Runtime environment |
| **Fastify** | Web framework |
| **TypeScript** | Type-safe JavaScript |
| **Supabase** | PostgreSQL database + Auth + Storage |
| **Redis (ioredis)** | L2 caching layer |
| **NATS** | Message broker for events |
| **OpenTelemetry** | Distributed tracing |
| **Zod** | Schema validation |
| **PM2** | Process manager |

### Infrastructure
| Technology | Purpose |
|------------|---------|
| **Docker** | Containerization |
| **Google Cloud Run** | Container hosting |
| **Railway** | Alternative deployment platform |
| **Netlify** | Frontend hosting + serverless functions |
| **Grafana Alloy** | Observability agent |
| **Supabase Migrations** | Database schema management |

### Testing
| Technology | Purpose |
|------------|---------|
| **Vitest** | Frontend unit testing |
| **Jest** | Backend testing |
| **Testing Library** | React component testing |

## 4. Initial Structure Impression

The application is a **monorepo** with clearly separated concerns:

| Component | Location | Purpose |
|-----------|----------|---------|
| **Frontend** | `/src` | React SPA for web dashboard |
| **Backend** | `/backend` | Fastify API server |
| **Database** | `/supabase` | Migrations, functions, schema |
| **Infrastructure** | `/infra` | Cloud Run configs, Grafana |
| **Documentation** | `/docs` | Architecture, API, domain docs |
| **Tests** | `/tests`, `/backend/tests` | Test framework and suites |

## 5. Configuration/Package Files

### Root Level
- `package.json` - Frontend dependencies and scripts
- `tsconfig.json`, `tsconfig.app.json`, `tsconfig.node.json` - TypeScript configs
- `vite.config.ts` - Vite build configuration
- `vitest.config.ts` - Frontend test configuration
- `tailwind.config.js` - Tailwind CSS configuration
- `postcss.config.js` - PostCSS configuration
- `eslint.config.js` - ESLint configuration
- `.prettierrc`, `.prettierignore` - Prettier formatting
- `.lintstagedrc.json` - Pre-commit linting
- `netlify.toml` - Netlify deployment config
- `index.html` - SPA entry point

### Backend
- `backend/package.json` - Backend dependencies
- `backend/tsconfig.json` - Backend TypeScript config
- `backend/jest.config.js` - Jest test configuration
- `backend/Dockerfile` - Container build
- `backend/docker-compose.yml` - Local development
- `backend/ecosystem.config.js` - PM2 configuration
- `backend/railway.json` - Railway deployment

### Supabase
- `supabase/config.toml` - Supabase CLI configuration

### CI/CD
- `.github/workflows/*.yml` - GitHub Actions workflows

## 6. Directory Structure

### `/src` - Frontend Application
```
src/
├── pages/          # Route-level components (Dashboard, Orders, Trips, etc.)
├── components/     # Reusable UI components
│   ├── ui/         # Base UI primitives (buttons, inputs, dialogs)
│   ├── settings/   # Settings-related components
│   ├── auth/       # Authentication components
│   ├── layout/     # Layout components
│   ├── dashboard/  # Dashboard widgets
│   ├── territory/  # Territory management
│   ├── trip-planning/ # Trip planning components
│   ├── approvals/  # Approval workflow components
│   ├── logistics/  # Logistics components
│   └── reports/    # Reporting components
├── hooks/          # Custom React hooks
├── lib/            # Utility libraries and services
├── contexts/       # React context providers
├── types/          # TypeScript type definitions
└── test/           # Test setup and mocks
```

### `/backend/src` - Backend Application
```
backend/src/
├── routes/         # API route definitions (37 files)
├── handlers/       # Request handlers
├── services/       # Business logic services (18 files)
├── components/     # Shared backend components (18 files)
├── plugins/        # Fastify plugins (5 files)
├── middleware/     # Express-style middleware
├── core/           # Core framework utilities
├── config/         # Configuration management
├── events/         # Event handling
├── external-apis/  # Third-party integrations (WhatsApp)
├── client/         # Client utilities
├── types/          # TypeScript types
└── utils/          # Utility functions
```

### `/supabase/migrations` - Database Schema
Contains **300+ migration files** covering:
- Core tables (orders, customers, trips, warehouses, vehicles)
- Inventory management (levels, transactions, snapshots)
- RPC functions for complex operations
- Views for optimized data retrieval
- RLS (Row Level Security) policies
- Triggers for business rule enforcement

### `/docs` - Documentation
```
docs/
├── AI-CONTEXT.md       # AI assistant context
├── API.md              # API documentation
├── ARCHITECTURE.md     # System architecture
├── DOMAIN-MODEL.md     # Domain concepts
├── EVENTS.md           # Event system documentation
├── LOCAL-DEV.md        # Local development guide
├── WORKFLOWS.md        # Business workflows
├── schemas/            # API and event schemas
├── archive/            # Historical documentation
└── reference/          # Reference materials
```

## 7. High-Level Architecture

### Evidence of Architectural Patterns

#### 1. **Layered Architecture (Backend)**
```
Routes → Handlers → Services → Database
```
- `backend/src/routes/` - HTTP endpoint definitions
- `backend/src/handlers/` - Request processing
- `backend/src/services/` - Business logic (OrdersService, TripsService, etc.)

#### 2. **Event-Driven Architecture**
- `backend/src/events/` - Event handling
- `docs/schemas/events/` - 12 event schema files
- `docs/EVENTS.md` - Event documentation
- NATS integration for pub/sub messaging
- Event store in database for audit trail

#### 3. **Domain-Driven Design**
Domain boundaries visible in structure:
- **Orders Domain**: Order creation, approval, fulfillment
- **Trips Domain**: Trip planning, dispatch, completion
- **Inventory Domain**: Stock levels, transactions, variances
- **Customer Domain**: Customer management, addresses, balances

#### 4. **Multi-tenant Architecture**
- `tenant_id` present throughout database schema
- RLS policies enforcing tenant isolation
- `TenantConfigContext` in frontend

#### 5. **Backend for Frontend (BFF)**
- Backend aggregates data for frontend views
- Database views (`*_list_view`, `*_with_*_view`) optimize queries

### Communication Patterns
- **Sync**: REST API (Fastify) for CRUD operations
- **Async**: NATS for event publishing, webhooks for external integrations
- **Real-time**: WebSocket support (`useWebSocketSubscription`)

## 8. Build, Execution and Test

### Frontend

**Build:**
```bash
npm run build          # Production build via Vite
```

**Run (Development):**
```bash
npm run dev            # Vite dev server with HMR
```

**Test:**
```bash
npm run test           # Vitest unit tests
```

**Entry Point:** `src/main.tsx` → `src/App.tsx`

### Backend

**Build:**
```bash
cd backend
npm run build          # TypeScript compilation
```

**Run (Development):**
```bash
npm run dev            # Development with hot reload
npm run start          # Production start
```

**Run (Production with PM2):**
```bash
./scripts/pm2-start.sh
```

**Test:**
```bash
npm run test           # Jest tests
./run-tests.sh         # Shell-based test runner
```

**Entry Point:** `backend/src/index.ts` → `backend/src/app.ts`

### Database

**Migrations:**
```bash
supabase db push       # Apply migrations
supabase migration new # Create new migration
```

### Docker

**Build and Run:**
```bash
cd backend
docker-compose up      # Local development with dependencies
docker build -t oms-backend .
```

### CI/CD Workflows

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `frontend-ci.yml` | PR/Push | Lint, test, build frontend |
| `backend-ci.yml` | PR/Push | Lint, test backend |
| `deploy-gcp.yml` | Manual/Merge | Deploy to Cloud Run |
| `deploy-alloy-gcp.yml` | Manual | Deploy Grafana Alloy |

### Key Scripts (package.json)

**Frontend:**
```json
{
  "dev": "vite",
  "build": "tsc && vite build",
  "preview": "vite preview",
  "test": "vitest",
  "lint": "eslint . --ext ts,tsx"
}
```

**Backend:**
```json
{
  "dev": "tsx watch src/index.ts",
  "build": "tsc",
  "start": "node dist/index.js",
  "test": "jest"
}
```

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown - OMS System

## Frontend Components (`@src/`)

### 1. Core Application Files

#### Files: `App.tsx`, `main.tsx`, `index.css`

**Core Responsibility:**
Application entry point and root component that initializes the React application, sets up routing, and wraps the app with necessary providers.

**Key Components:**
- `main.tsx` - React DOM rendering entry point
- `App.tsx` - Root component with router configuration and provider hierarchy
- `index.css` - Global Tailwind CSS styles

**Dependencies & Interactions:**
- Internal: `@src/contexts/`, `@src/pages/`, `@src/components/`, `@src/lib/supabase.ts`
- External: React Router, Tailwind CSS

---

### 2. Contexts (`@src/contexts/`)

#### Files: `AuthContext.tsx`, `BusinessRulesContext.tsx`, `TenantConfigContext.tsx`

**Core Responsibility:**
Provide application-wide state management for authentication, business rules, and tenant-specific configuration using React Context API.

**Key Components:**
- `AuthContext.tsx` - User authentication state, login/logout functions, session management
- `BusinessRulesContext.tsx` - Dynamic business rules loaded from database, rule evaluation functions
- `TenantConfigContext.tsx` - Multi-tenant configuration (terminology, settings, feature flags)

**Dependencies & Interactions:**
- Internal: `@src/lib/supabase.ts`, `@src/lib/api-client.ts`, `@src/types/`
- External: Supabase Auth, potentially backend API for tenant config

---

### 3. Hooks (`@src/hooks/`)

**Core Responsibility:**
Reusable React hooks encapsulating common logic patterns for data fetching, business rules, permissions, analytics, and UI behaviors.

**Key Components:**
| Hook | Role |
|------|------|
| `useApiQuery.ts` | Standardized data fetching with React Query |
| `useBundleOperations.ts` | Product bundle CRUD operations |
| `useBusinessRules.ts` | Access and evaluate business rules |
| `useBusinessRulesManagement.ts` | Admin CRUD for business rules |
| `useCustomerPermissions.ts` | Customer-specific permission checks |
| `usePermissions.ts` | Role-based access control |
| `useTerminology.ts` | Dynamic terminology based on tenant config |
| `useTypeDefinitions.ts` | Dynamic type system definitions |
| `useOMSAnalytics.ts` | Mixpanel analytics tracking |
| `usePageTracking.ts` | Page view analytics |
| `usePerformanceTracking.ts` | Performance metrics collection |
| `useNetworkStatus.ts` | Online/offline detection |
| `useSessionTimeout.ts` | Auto-logout on inactivity |
| `useWebSocketSubscription.ts` | Real-time data subscriptions |
| `useClickOutside.ts` | UI utility for dropdown/modal dismissal |

**Dependencies & Interactions:**
- Internal: `@src/contexts/`, `@src/lib/api-client.ts`, `@src/lib/supabase.ts`, `@src/lib/analytics.ts`
- External: React Query, Mixpanel, Supabase Realtime

---

### 4. Library/Utilities (`@src/lib/`)

**Core Responsibility:**
Shared utility functions, API clients, external service integrations, and helper modules used across the application.

**Key Components:**
| File | Role |
|------|------|
| `supabase.ts` | Supabase client initialization and configuration |
| `supabase-monitoring.ts` | Supabase query performance monitoring |
| `supabase-global-interceptor.ts` | Request/response interception for Supabase |
| `api-client.ts` | Backend API client (REST) |
| `WebSocketClient.ts` | WebSocket connection management |
| `queryClient.ts` | React Query client configuration |
| `sentry.ts` | Sentry error tracking initialization |
| `analytics.ts` | Mixpanel analytics wrapper |
| `performanceMonitor.ts` | Performance metrics collection |
| `errorHandler.ts` | Centralized error handling |
| `featureFlags.ts` | Feature flag evaluation |
| `addressHelpers.ts` | Address formatting and geocoding utilities |
| `currencyUtils.ts` | Currency formatting |
| `dateUtils.ts` | Date manipulation and formatting |
| `inventoryUtils.ts` | Inventory calculations |
| `imageUploadHelpers.ts` | Image upload to storage |
| `storageHelpers.ts` | Supabase storage utilities |
| `auditDisplayHelpers.ts` | Audit log formatting |
| `trip-export-service.ts` | Trip data export functionality |
| `varianceCodes.ts` | Standardized variance code definitions |

**Dependencies & Interactions:**
- Internal: `@src/types/`, `@src/contexts/`
- External: Supabase, Sentry, Mixpanel, potentially Google Maps API

---

### 5. Components (`@src/components/`)

**Core Responsibility:**
Reusable UI components organized by feature area, following atomic design principles.

#### 5.1 UI Components (`@src/components/ui/`) - 91 files

**Role:** Base UI primitives and design system components (buttons, inputs, modals, tables, cards, etc.)

**Dependencies:** Tailwind CSS, Radix UI (likely), internal styling conventions

#### 5.2 Settings Components (`@src/components/settings/`) - 17 files

**Role:** User/tenant settings management UI (profile, preferences, business rules config)

**Dependencies:** `@src/hooks/useBusinessRulesManagement.ts`, `@src/contexts/TenantConfigContext.tsx`

#### 5.3 Auth Components (`@src/components/auth/`) - 2 files

**Role:** Login, registration, password reset forms

**Dependencies:** `@src/contexts/AuthContext.tsx`, `@src/lib/supabase.ts`

#### 5.4 Layout Components (`@src/components/layout/`) - 2 files

**Role:** Page layout scaffolding (sidebar, header, navigation)

**Dependencies:** `@src/contexts/AuthContext.tsx`, `@src/hooks/usePermissions.ts`

#### 5.5 Dashboard Components (`@src/components/dashboard/`) - 4 files

**Role:** Dashboard widgets, charts, KPI cards

**Dependencies:** Backend views (`dashboard_metrics_view`, `business_health_view`), charting library

#### 5.6 Territory Components (`@src/components/territory/`) - 7 files

**Role:** Territory management, map visualizations, customer clustering

**Dependencies:** `@src/lib/api-client.ts`, Google Maps API, territory analytics views

#### 5.7 Trip Planning Components (`@src/components/trip-planning/`) - 9 files

**Role:** Trip creation wizard, route optimization, stop management

**Dependencies:** `@src/lib/api-client.ts`, `@src/hooks/useBundleOperations.ts`

#### 5.8 Approvals Components (`@src/components/approvals/`)

**Sub-directories:**
- `customer-approvals/` - New customer approval workflow
- `order-approvals/` - Order approval workflow
- `payment-approvals/` - Payment verification workflow
- `variance-approvals/` - Inventory variance approval
- `shared/` - Common approval UI components
- `types/` - TypeScript types for approvals

**Role:** Multi-type approval workflow UI with status tracking

**Dependencies:** `@src/hooks/usePermissions.ts`, backend approval endpoints

#### 5.9 Logistics Components (`@src/components/logistics/`) - 9 files

**Role:** Warehouse management, vehicle tracking, inventory views

**Dependencies:** `@src/lib/api-client.ts`, inventory-related hooks

#### 5.10 Reports Components (`@src/components/reports/`) - 8 files

**Role:** Report generation, data export, analytics visualizations

**Dependencies:** Backend report views, `@src/lib/trip-export-service.ts`

---

### 6. Pages (`@src/pages/`)

**Core Responsibility:**
Route-level page components that compose smaller components and orchestrate data fetching for specific views.

**Key Pages:**

| Page | Role |
|------|------|
| `Dashboard.tsx` | Main dashboard with KPIs and recent activity |
| `Customers.tsx`, `CustomerDetails.tsx`, `CreateCustomer.tsx`, `EditCustomer.tsx` | Customer CRUD |
| `Orders.tsx`, `OrderDetails.tsx`, `CreateOrder.tsx`, `EditOrder.tsx` | Order management |
| `Trips.tsx`, `TripDetailsV2.tsx`, `CreateTrip.tsx`, `EditTrip.tsx` | Trip/delivery management |
| `Products.tsx`, `ProductDetails.tsx`, `CreateProduct.tsx`, `EditProduct.tsx`, `AddVariant.tsx` | Product catalog |
| `Inventory.tsx`, `PerformCount.tsx` | Inventory management |
| `ManualAdjustments.tsx`, `ManualAdjustmentDetails.tsx`, `CreateManualAdjustment.tsx`, `EditManualAdjustment.tsx` | Inventory adjustments |
| `Approvals.tsx` | Unified approvals dashboard |
| `VarianceDashboard.tsx`, `ReviewVariances.tsx` | Variance tracking |
| `Prices.tsx` | Price list management |
| `Logistics.tsx`, `logistics/*` (9 files) | Logistics sub-pages |
| `Reports.tsx` | Reporting dashboard |
| `TerritoryManagement.tsx` | Territory/zone management |
| `TenantManagement.tsx` | Super-admin tenant management |
| `ImportTools.tsx` | Bulk data import |
| `MobileSimulator.tsx` | Mobile app testing simulator |
| `InvitationSignup.tsx`, `ResetPassword.tsx` | Auth flows |
| `WarehouseDetails.tsx` | Warehouse detail view |

**Dependencies & Interactions:**
- Internal: `@src/components/*`, `@src/hooks/*`, `@src/contexts/*`, `@src/lib/*`
- External: Backend API via `api-client.ts`, Supabase views

---

### 7. Types (`@src/types/`)

**Core Responsibility:**
TypeScript type definitions for domain models and API contracts.

**Key Files:**
- `business-rules.ts` - Business rule structure and evaluation types
- `inventory.ts` - Inventory level, transaction, adjustment types
- `tenant-config.ts` - Tenant configuration schema
- `trip-export.ts` - Trip export data structure

**Dependencies:** None (pure type definitions)

---

## Backend Components (`@backend/`)

### 8. Core Application (`@backend/src/`)

#### 8.1 Entry Points (`app.ts`, `index.ts`)

**Core Responsibility:**
Fastify server initialization, plugin registration, and server startup.

**Key Components:**
- `index.ts` - Server bootstrap and port binding
- `app.ts` - Fastify app factory with plugin registration

**Dependencies & Interactions:**
- Internal: `@backend/src/plugins/`, `@backend/src/routes/`, `@backend/src/config/`
- External: Fastify framework

#### 8.2 Configuration (`@backend/src/config/`) - 1 file

**Core Responsibility:**
Environment configuration, database connections, external service credentials.

**Dependencies:** Environment variables, Supabase connection strings

#### 8.3 Middleware (`@backend/src/middleware/`) - 1 file

**Core Responsibility:**
Request/response middleware (likely authentication, logging, error handling).

**Dependencies:** `@backend/src/core/`, JWT verification

#### 8.4 Plugins (`@backend/src/plugins/`) - 5 files

**Core Responsibility:**
Fastify plugins for cross-cutting concerns (auth, caching, CORS, etc.).

**Dependencies:** Fastify plugin system, external services (Redis, etc.)

#### 8.5 Core (`@backend/src/core/`) - 4 files

**Core Responsibility:**
Core business logic, base classes, shared utilities.

**Dependencies:** Supabase client, internal types

#### 8.6 Types (`@backend/src/types/`) - 4 files

**Core Responsibility:**
Backend TypeScript type definitions.

**Dependencies:** None (pure types)

---

### 9. Routes (`@backend/src/routes/`) - 37 files

**Core Responsibility:**
REST API endpoint definitions organized by resource/feature.

**Likely Structure:**
- Customer routes
- Order routes
- Trip routes
- Product routes
- Inventory routes
- Approval routes
- User/Auth routes
- Report routes
- Webhook routes

**Dependencies & Interactions:**
- Internal: `@backend/src/services/`, `@backend/src/handlers/`, `@backend/src/middleware/`
- External: HTTP clients for webhooks

---

### 10. Services (`@backend/src/services/`) - 18 files

**Core Responsibility:**
Business logic layer implementing domain operations, coordinating between routes and data access.

**Likely Services:**
- CustomerService
- OrderService
- TripService
- InventoryService
- PricingService
- ApprovalService
- NotificationService
- ReportService

**Dependencies & Interactions:**
- Internal: `@backend/src/client/` (Supabase), `@backend/src/utils/`, `@backend/src/events/`
- External: Supabase database, external APIs

---

### 11. Handlers (`@backend/src/handlers/`) - 6 files

**Core Responsibility:**
Request handlers that process incoming requests and delegate to services.

**Dependencies:** `@backend/src/services/`, `@backend/src/types/`

---

### 12. Components (`@backend/src/components/`) - 18 files

**Core Responsibility:**
Likely domain-specific modules or feature modules encapsulating related functionality.

**Dependencies:** `@backend/src/services/`, `@backend/src/client/`

---

### 13. Events (`@backend/src/events/`) - 1 file

**Core Responsibility:**
Event handling system for domain events (order created, trip completed, etc.).

**Dependencies:** Event store (Supabase), potentially external message queue

---

### 14. Client (`@backend/src/client/`) - 1 file

**Core Responsibility:**
Supabase client wrapper with helper methods.

**Dependencies:** Supabase SDK

---

### 15. External APIs (`@backend/src/external-apis/`)

#### WhatsApp Integration (`@backend/src/external-apis/whatsapp/`)

**Core Responsibility:**
WhatsApp Business API integration for notifications.

**Dependencies:** WhatsApp Cloud API

---

### 16. Utilities (`@backend/src/utils/`) - 2 files

**Core Responsibility:**
Shared utility functions (validation, formatting, etc.).

**Dependencies:** None (pure utilities)

---

### 17. Tracing (`tracing.ts`)

**Core Responsibility:**
OpenTelemetry tracing setup for observability.

**Dependencies:** OpenTelemetry SDK, Grafana Alloy (based on `infra/` structure)

---

## Database/Supabase (`@supabase/`)

### 18. Migrations (`@supabase/migrations/`) - 300+ files

**Core Responsibility:**
Database schema evolution, RLS policies, functions, triggers, and views.

**Key Categories:**
- Schema changes (tables, columns, indexes)
- RLS (Row Level Security) policies
- PostgreSQL functions (RPC)
- Database views (list views, analytics views)
- Triggers (audit, auto-calculations)
- Seed data

**Dependencies:** PostgreSQL, Supabase extensions (pg_cron, etc.)

---

### 19. Functions (`@supabase/functions/`)

**Core Responsibility:**
Supabase Edge Functions for serverless operations.

**Key Functions:**
- `firebase-auth/` - Firebase authentication bridge
- `geocode-address/` - Google Maps geocoding
- `send-password-reset/` - Password reset emails
- `send-user-invite/` - User invitation emails

**Dependencies & Interactions:**
- External: Google Maps API, Email service (Resend likely), Firebase

---

## Infrastructure (`@infra/`)

### 20. Grafana Alloy (`@infra/grafana-alloy/`)

**Core Responsibility:**
Observability agent configuration for metrics, logs, and traces collection.

**Key Files:**
- `Dockerfile` - Container build
- `config.gcp.alloy` - Alloy configuration
- `cloud-run.*.yaml` - Cloud Run deployment specs

**Dependencies:** Grafana Cloud, OpenTelemetry

---

### 21. Cloud Run (`@infra/cloud-run/`)

**Core Responsibility:**
Google Cloud Run deployment configurations.

**Key Files:**
- `oms-backend.production.yaml`
- `oms-backend.staging.yaml`

**Dependencies:** Google Cloud Run, Secret Manager

---

## Testing (`@tests/`, `@backend/tests/`)

### 22. Test Framework (`@tests/framework/`)

**Core Responsibility:**
Shared test utilities and data factories.

**Key Files:**
- `DataFactory.ts` - Test data generation
- `TestFramework.ts` - Test setup utilities

### 23. Backend Tests (`@backend/tests/`)

**Core Responsibility:**
Backend API tests (unit and integration).

**Structure:**
- `unit/` - Unit tests (2 files)
- `integration/` - Integration tests for customers, orders, offload-documents
- `mocks/` - Mock implementations
- `utils/` - Test utilities

**Dependencies:** Jest, Supertest (likely)

---

## CI/CD (`.github/workflows/`)

### 24. Workflows

**Core Responsibility:**
Automated build, test, and deployment pipelines.

**Key Workflows:**
- `backend-ci.yml` - Backend CI pipeline
- `frontend-ci.yml` - Frontend CI pipeline
- `deploy-gcp.yml` - GCP deployment
- `deploy-alloy-gcp.yml` - Alloy deployment

**Dependencies:** GitHub Actions, Google Cloud SDK

---

## Summary: Key External Service Interactions

| Service | Used By |
|---------|---------|
| **Supabase** | All data access, auth, realtime, storage |
| **Google Cloud Run** | Backend hosting |
| **Netlify** | Frontend hosting |
| **Sentry** | Error tracking (frontend & backend) |
| **Mixpanel** | Product analytics |
| **Grafana Cloud** | Observability (via Alloy) |
| **Google Maps API** | Geocoding, maps |
| **WhatsApp Business API** | Notifications |
| **Firebase** | Alternative auth (bridge) |

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Repository: oms-system_bed8df41

---

## Internal Modules

### Backend (`/backend/src/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `app.ts` / `index.ts` | Application entry point and server initialization |
| `tracing.ts` | OpenTelemetry tracing configuration and setup |
| `config/` | Application configuration management |
| `core/` | Core business logic and foundational utilities |
| `middleware/` | HTTP middleware (authentication, logging, etc.) |
| `routes/` | API route definitions and endpoint handlers (37 files) |
| `handlers/` | Request handlers for specific operations |
| `services/` | Business service layer containing domain logic (18 files) |
| `components/` | Reusable backend components (18 files) |
| `plugins/` | Fastify plugin integrations |
| `events/` | Event-driven architecture components |
| `client/` | External client configurations |
| `utils/` | Shared utility functions |
| `types/` | TypeScript type definitions |
| `external-apis/whatsapp/` | WhatsApp API integration |

### Frontend (`/src/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `App.tsx` / `main.tsx` | Application entry point and root component |
| `contexts/` | React context providers (AuthContext, BusinessRulesContext, TenantConfigContext) |
| `pages/` | Page-level React components (Dashboard, Orders, Customers, Trips, Inventory, etc.) |
| `components/ui/` | Reusable UI components library (91 files) |
| `components/settings/` | Settings management UI components |
| `components/auth/` | Authentication-related UI components |
| `components/layout/` | Layout wrapper components |
| `components/dashboard/` | Dashboard-specific widgets and components |
| `components/territory/` | Territory management components |
| `components/trip-planning/` | Trip planning and scheduling components |
| `components/approvals/` | Approval workflow components (customer, order, payment, variance) |
| `components/logistics/` | Logistics and delivery management components |
| `components/reports/` | Reporting and analytics components |
| `hooks/` | Custom React hooks (API queries, business rules, permissions, analytics, etc.) |
| `lib/` | Utility libraries (API client, Supabase, Sentry, analytics, WebSocket, etc.) |
| `types/` | TypeScript type definitions |

### Database/Supabase (`/supabase/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `migrations/` | Database schema migrations and RPC functions |
| `functions/` | Supabase Edge Functions (firebase-auth, geocode-address, send-password-reset, send-user-invite) |
| `scripts/` | Database maintenance scripts |

### Infrastructure (`/infra/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `grafana-alloy/` | Observability agent configuration for GCP |
| `cloud-run/` | Google Cloud Run deployment configurations |
| `scripts/` | Infrastructure setup scripts |

### Tests (`/tests/`, `/backend/tests/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `framework/` | Test framework utilities (DataFactory, TestFramework) |
| `backend/tests/unit/` | Backend unit tests |
| `backend/tests/integration/` | Backend integration tests (customers, orders, offload-documents) |
| `backend/tests/utils/` | Test utilities and helpers |

---

## External Dependencies

### Backend Production Dependencies

*Source: `/backend/package.json`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|----------------------|
| `@fastify/cors` | Fastify CORS | Cross-Origin Resource Sharing middleware for Fastify |
| `@fastify/env` | Fastify Env | Environment variable validation and loading for Fastify |
| `@fastify/jwt` | Fastify JWT | JSON Web Token authentication plugin for Fastify |
| `@fastify/multipart` | Fastify Multipart | Multipart/form-data parsing for file uploads |
| `@fastify/websocket` | Fastify WebSocket | WebSocket support for real-time communication |
| `@opentelemetry/auto-instrumentations-node` | OpenTelemetry Auto Instrumentations | Automatic instrumentation for Node.js applications |
| `@opentelemetry/exporter-trace-otlp-http` | OpenTelemetry OTLP Exporter | Exports traces via OTLP HTTP protocol |
| `@opentelemetry/instrumentation-fastify` | OpenTelemetry Fastify Instrumentation | Fastify-specific tracing instrumentation |
| `@opentelemetry/sdk-node` | OpenTelemetry Node SDK | Core OpenTelemetry SDK for Node.js |
| `@sentry/node` | Sentry Node | Error tracking and monitoring for Node.js |
| `@sentry/profiling-node` | Sentry Profiling | Performance profiling for Sentry |
| `@supabase/supabase-js` | Supabase JS | Supabase client library for database and auth |
| `@types/ioredis` | IORedis Types | TypeScript type definitions for IORedis |
| `@types/node` | Node Types | TypeScript type definitions for Node.js |
| `@types/node-cache` | Node Cache Types | TypeScript type definitions for Node Cache |
| `@types/uuid` | UUID Types | TypeScript type definitions for UUID |
| `axios` | Axios | HTTP client for making API requests |
| `axios-retry` | Axios Retry | Retry logic for failed Axios requests |
| `dotenv` | Dotenv | Environment variable loading from .env files |
| `fastify` | Fastify | Web framework for building APIs |
| `fastify-plugin` | Fastify Plugin | Plugin helper for Fastify |
| `ioredis` | IORedis | Redis client for caching |
| `mixpanel` | Mixpanel | Analytics tracking service (server-side) |
| `nats` | NATS | Messaging system for event-driven architecture |
| `node-cache` | Node Cache | In-memory caching library |
| `pino` | Pino | Fast JSON logger |
| `pino-loki` | Pino Loki | Pino transport for Grafana Loki |
| `pino-pretty` | Pino Pretty | Pretty-print formatter for Pino logs |
| `prom-client` | Prometheus Client | Prometheus metrics client |
| `tsx` | TSX | TypeScript execution engine |
| `typescript` | TypeScript | TypeScript compiler |
| `uuid` | UUID | UUID generation library |

### Backend Development Dependencies

*Source: `/backend/package.json (dev)`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|----------------------|
| `@types/jest` | Jest Types | TypeScript type definitions for Jest |
| `@types/supertest` | Supertest Types | TypeScript type definitions for Supertest |
| `@typescript-eslint/eslint-plugin` | TypeScript ESLint Plugin | ESLint rules for TypeScript |
| `@typescript-eslint/parser` | TypeScript ESLint Parser | TypeScript parser for ESLint |
| `eslint` | ESLint | JavaScript/TypeScript linter |
| `jest` | Jest | Testing framework |
| `node-fetch` | Node Fetch | Fetch API implementation for Node.js |
| `nodemon` | Nodemon | Development server with auto-restart |
| `prettier` | Prettier | Code formatter |
| `supertest` | Supertest | HTTP assertion library for testing |
| `ts-jest` | TS Jest | TypeScript preprocessor for Jest |

### Frontend Production Dependencies

*Source: `/package.json`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|----------------------|
| `@sentry/react` | Sentry React | Error tracking for React applications |
| `@sentry/tracing` | Sentry Tracing | Performance monitoring and tracing |
| `@supabase/supabase-js` | Supabase JS | Supabase client library for database and auth |
| `@tanstack/react-query` | TanStack Query (React Query) | Server state management and data fetching |
| `@types/mixpanel-browser` | Mixpanel Browser Types | TypeScript type definitions for Mixpanel |
| `@types/node` | Node Types | TypeScript type definitions for Node.js |
| `clsx` | clsx | Utility for constructing className strings |
| `date-fns` | date-fns | Date utility library |
| `file-saver` | FileSaver.js | Client-side file saving library |
| `html2canvas` | html2canvas | HTML to canvas rendering |
| `jspdf` | jsPDF | PDF generation library |
| `jspdf-autotable` | jsPDF AutoTable | Table generation plugin for jsPDF |
| `lucide-react` | Lucide React | Icon library for React |
| `mapbox-gl` | Mapbox GL JS | Interactive maps library |
| `mixpanel-browser` | Mixpanel Browser | Analytics tracking (client-side) |
| `react` | React | UI component library |
| `react-dom` | React DOM | React DOM rendering |
| `react-phone-number-input` | React Phone Number Input | Phone number input component |
| `react-router-dom` | React Router | Client-side routing for React |
| `recharts` | Recharts | Charting library built on React |
| `resend` | Resend | Email sending service |
| `xlsx` | SheetJS | Spreadsheet parsing and generation |

### Frontend Development Dependencies

*Source: `/package.json (dev)`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|----------------------|
| `@eslint/js` | ESLint JS | ESLint JavaScript configuration |
| `@testing-library/jest-dom` | Testing Library Jest DOM | Custom Jest matchers for DOM testing |
| `@testing-library/react` | Testing Library React | React component testing utilities |
| `@testing-library/user-event` | Testing Library User Event | User interaction simulation for testing |
| `@types/file-saver` | FileSaver Types | TypeScript type definitions for FileSaver |
| `@types/mapbox-gl` | Mapbox GL Types | TypeScript type definitions for Mapbox GL |
| `@types/react` | React Types | TypeScript type definitions for React |
| `@types/react-dom` | React DOM Types | TypeScript type definitions for React DOM |
| `@vitejs/plugin-react` | Vite React Plugin | React support for Vite |
| `@vitest/coverage-v8` | Vitest Coverage V8 | Code coverage for Vitest |
| `@vitest/ui` | Vitest UI | UI for Vitest test runner |
| `autoprefixer` | Autoprefixer | PostCSS plugin for vendor prefixes |
| `eslint` | ESLint | JavaScript/TypeScript linter |
| `eslint-plugin-react-hooks` | ESLint React Hooks | ESLint rules for React Hooks |
| `eslint-plugin-react-refresh` | ESLint React Refresh | ESLint rules for React Refresh |
| `globals` | Globals | Global variable definitions for ESLint |
| `husky` | Husky | Git hooks manager |
| `jsdom` | jsdom | DOM implementation for Node.js (testing) |
| `lint-staged` | lint-staged | Run linters on staged git files |
| `msw` | Mock Service Worker | API mocking library for testing |
| `postcss` | PostCSS | CSS transformation tool |
| `prettier` | Prettier | Code formatter |
| `rollup-plugin-visualizer` | Rollup Visualizer | Bundle size visualization |
| `tailwindcss` | Tailwind CSS | Utility-first CSS framework |
| `typescript` | TypeScript | TypeScript compiler |
| `typescript-eslint` | TypeScript ESLint | TypeScript integration for ESLint |
| `vite` | Vite | Frontend build tool and dev server |
| `vitest` | Vitest | Testing framework for Vite |

### Infrastructure/Container Dependencies

*Source: `/backend/Dockerfile`, `/backend/docker-compose.yml`, `/grafana-alloy/Dockerfile`, `/infra/grafana-alloy/Dockerfile`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|----------------------|
| `node:20-alpine` | Node.js Alpine | Node.js runtime base image |
| `dumb-init` | dumb-init | Minimal init system for proper signal handling in containers |
| `redis:7-alpine` | Redis | In-memory data store for caching |
| `grafana/alloy:latest` | Grafana Alloy | Observability data collector (logs, metrics, traces) |

# core_entities

Core entities and their relationships

# OMS System - Domain Model Analysis

## Executive Summary

This is an **Order Management System (OMS)** for a distribution/logistics business, likely dealing with physical goods (possibly gas cylinders based on "cylinder tracking" references). The system manages the full lifecycle from customer orders through delivery, including inventory management, trip planning, and financial reconciliation.

---

## 1. Core Data Entities

### 1.1 **Tenant**
The root entity for multi-tenancy support.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `name` | String | Tenant/company name |
| `settings` | JSONB | Configuration settings |
| `currency` | String | Default currency |
| `created_at` | Timestamp | Creation timestamp |

---

### 1.2 **Customer**
Represents business customers who place orders.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `name` | String | Customer name |
| `status` | Enum | `pending`, `approved`, `rejected`, `inactive` |
| `customer_reference` | String | External reference code |
| `primary_contact` | JSONB | Contact information |
| `billing_condition` | String | Payment terms |
| `sales_user_id` | UUID | Assigned sales agent |
| `created_by` | UUID | User who created |
| `updated_by` | UUID | Last modifier |
| `created_at` | Timestamp | Creation timestamp |

---

### 1.3 **Address**
Physical locations associated with customers or warehouses.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `customer_id` | UUID | FK to Customer (nullable) |
| `name` | String | Address label |
| `type` | Enum | `delivery`, `billing`, `warehouse` |
| `street` | String | Street address |
| `city` | String | City |
| `coordinates` | Point | Lat/lng for mapping |
| `plus_code` | String | Google Plus Code |
| `is_default` | Boolean | Default address flag |

---

### 1.4 **Product**
Master product definition.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `name` | String | Product name |
| `type` | Enum | `standard`, `bundle`, `measured` |
| `sku_code` | String | Stock keeping unit |
| `state` | Enum | `full`, `empty`, etc. |
| `is_active` | Boolean | Active status |

---

### 1.5 **Product Variant**
Specific sellable/trackable variants of products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `product_id` | UUID | FK to Product |
| `sku_code` | String | Variant SKU |
| `name` | String | Variant name |
| `size` | String | Size descriptor |
| `state` | Enum | `full`, `empty` |
| `is_active` | Boolean | Active status |

---

### 1.6 **Bundle Component**
Defines composition of bundle products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `bundle_variant_id` | UUID | FK to Product Variant (bundle) |
| `component_variant_id` | UUID | FK to Product Variant (component) |
| `quantity` | Integer | Quantity of component in bundle |

---

### 1.7 **Price List**
Pricing configurations for products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `name` | String | Price list name |
| `variant_id` | UUID | FK to Product Variant |
| `price` | Decimal | Unit price |
| `effective_from` | Date | Start validity |
| `effective_to` | Date | End validity |

---

### 1.8 **Order**
Customer orders for products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `order_number` | String | Human-readable order number |
| `customer_id` | UUID | FK to Customer |
| `delivery_address_id` | UUID | FK to Address |
| `status` | Enum | `draft`, `approved`, `in_transit`, `delivered`, `cancelled` |
| `order_type` | Enum | `standard`, `transfer`, `conversion` |
| `order_source` | Enum | `web`, `mobile`, `whatsapp`, `operator` |
| `external_order_id` | String | External system reference |
| `notes` | Text | Order notes |
| `sales_user_id` | UUID | Sales agent |
| `created_by` | UUID | Creator |
| `approved_by` | UUID | Approver |
| `created_at` | Timestamp | Creation time |

---

### 1.9 **Order Line**
Individual line items within an order.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `order_id` | UUID | FK to Order |
| `variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Ordered quantity |
| `unit_price` | Decimal | Price per unit |
| `total_price` | Decimal | Line total |

---

### 1.10 **Warehouse**
Storage locations for inventory.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `name` | String | Warehouse name |
| `type` | Enum | `fixed`, `mobile` |
| `address_id` | UUID | FK to Address |
| `linked_vehicle_id` | UUID | FK to Vehicle (for mobile) |
| `is_active` | Boolean | Active status |
| `created_by` | UUID | Creator |

---

### 1.11 **Vehicle**
Delivery vehicles.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `registration_number` | String | Vehicle plate |
| `vehicle_type` | String | Type of vehicle |
| `capacity` | Integer | Load capacity |
| `is_active` | Boolean | Active status |

---

### 1.12 **Inventory Level**
Current stock levels per warehouse/variant.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `warehouse_id` | UUID | FK to Warehouse |
| `variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Current quantity |
| `updated_at` | Timestamp | Last update |

---

### 1.13 **Inventory Transaction**
Audit trail of all inventory movements.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `warehouse_id` | UUID | FK to Warehouse |
| `variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Change amount (+/-) |
| `transaction_type` | Enum | `allocation`, `deallocation`, `delivery`, `return`, `adjustment`, `conversion` |
| `reference_type` | Enum | `order`, `trip`, `adjustment`, `conversion` |
| `reference_id` | UUID | FK to related entity |
| `photo_url` | String | Supporting photo |
| `created_by` | UUID | User who created |
| `created_at` | Timestamp | Transaction time |

---

### 1.14 **Trip**
Delivery trips grouping multiple orders.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `trip_number` | String | Human-readable number |
| `vehicle_id` | UUID | FK to Vehicle |
| `driver_id` | UUID | FK to User Profile |
| `status` | Enum | `planned`, `dispatched`, `in_progress`, `completed`, `cancelled` |
| `scheduled_date` | Date | Planned date |
| `dispatched_by` | UUID | User who dispatched |
| `completed_at` | Timestamp | Completion time |

---

### 1.15 **Trip Order**
Junction table linking trips to orders with delivery details.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `order_id` | UUID | FK to Order |
| `sequence` | Integer | Stop sequence |
| `action_type` | Enum | `delivery`, `collection`, `conversion_deliver`, `conversion_collect` |
| `status` | Enum | `pending`, `completed`, `failed`, `cancelled` |
| `pod_url` | String | Proof of delivery photo |
| `delivered_at` | Timestamp | Delivery timestamp |
| `delivered_by` | UUID | Driver who delivered |

---

### 1.16 **Trip Inventory**
Expected/actual inventory for a trip.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `variant_id` | UUID | FK to Product Variant |
| `starting_quantity` | Integer | Loaded quantity |
| `expected_quantity` | Integer | Expected at end |
| `actual_quantity` | Integer | Actual at end |

---

### 1.17 **Loading Plan**
Pre-trip loading document.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `warehouse_id` | UUID | Source warehouse |
| `status` | Enum | `draft`, `approved`, `loaded`, `cancelled` |
| `total_quantity` | Integer | Total items to load |
| `approved_by` | UUID | Approver |
| `cancelled_at` | Timestamp | Cancellation time |
| `cancellation_reason` | String | Why cancelled |

---

### 1.18 **Offload Document**
Post-trip reconciliation document.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `destination_warehouse_id` | UUID | Return warehouse |
| `status` | Enum | `pending`, `agreed`, `approved` |
| `agreed_at` | Timestamp | Driver agreement time |
| `approved_at` | Timestamp | Manager approval time |
| `approved_by` | UUID | Approver |

---

### 1.19 **Trip Return**
Items returned from a trip.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Returned quantity |
| `destination_warehouse_id` | UUID | Where returned to |

---

### 1.20 **Payment**
Customer payment records.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `customer_id` | UUID | FK to Customer |
| `order_id` | UUID | FK to Order (optional) |
| `amount` | Decimal | Payment amount |
| `payment_method` | String | Cash, transfer, etc. |
| `status` | Enum | `pending`, `approved`, `rejected` |
| `collected_by` | UUID | Driver/agent who collected |
| `approved_by` | UUID | Approver |

---

### 1.21 **User Profile**
System users with roles.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier (matches auth.users) |
| `tenant_id` | UUID | FK to Tenant |
| `email` | String | Email address |
| `full_name` | String | Display name |
| `role` | Enum | `super_admin`, `admin`, `operator`, `accountant`, `sales`, `driver` |
| `is_active` | Boolean | Active status |

---

### 1.22 **User Invitation**
Pending user invitations.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `email` | String | Invitee email |
| `role` | Enum | Assigned role |
| `invited_by` | UUID | FK to User Profile |
| `expires_at` | Timestamp | Expiration |
| `accepted_at` | Timestamp | When accepted |

---

### 1.23 **Inventory Adjustment / Manual Adjustment**
Manual inventory corrections.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `warehouse_id` | UUID | FK to Warehouse |
| `variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Adjustment amount |
| `reason` | String | Reason code |
| `notes` | Text | Additional notes |
| `status` | Enum | `pending`, `approved`, `rejected` |
| `created_by` | UUID | Creator |
| `approved_by` | UUID | Approver |

---

### 1.24 **Variant Conversion**
Product state conversions (e.g., empty→full cylinders).

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `source_variant_id` | UUID | FK to Product Variant |
| `target_variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Conversion quantity |
| `order_id` | UUID | Associated order |

---

### 1.25 **Customer Balance**
Cylinder/asset balance tracking per customer.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `customer_id` | UUID | FK to Customer |
| `address_id` | UUID | FK to Address |
| `variant_id` | UUID | FK to Product Variant |
| `quantity` | Integer | Balance quantity |

---

### 1.26 **Business Rule**
Configurable business rules per tenant.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `entity_type` | String | `order`, `customer`, etc. |
| `rule_type` | String | Rule category |
| `field_path` | String | Field this applies to |
| `config` | JSONB | Rule configuration |
| `is_active` | Boolean | Active status |

---

### 1.27 **Workflow / Type Definition**
Dynamic type system for customization.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `entity_type` | String | Entity this applies to |
| `code` | String | Type code |
| `label` | String | Display label |
| `color` | String | UI color |
| `is_default` | Boolean | Default selection |

---

### 1.28 **Event Store**
Event sourcing/audit log.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary identifier |
| `tenant_id` | UUID | FK to Tenant |
| `event_type` | String | Event classification |
| `entity_type` | String | Related entity type |
| `entity_id` | UUID | Related entity ID |
| `payload` | JSONB | Event data |
| `created_by` | UUID | User who triggered |
| `created_at` | Timestamp | Event time |

---

## 2. Entity Relationships Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                   TENANT                                     │
│                            (Multi-tenancy Root)                              │
└─────────────────────────────────────────────────────────────────────────────┘
         │
         │ 1:N (All entities belong to a tenant)
         ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              USER PROFILE                                    │
│                     (Operators, Sales, Drivers, etc.)                        │
└─────────────────────────────────────────────────────────────────────────────┘
         │                              │
         │ N:1                          │ 1:N
         ▼                              ▼
┌──────────────────┐           ┌──────────────────┐
│    CUSTOMER      │◄──────────│  USER INVITATION │
│                  │  assigned │                  │
│  - Sales Agent   │           └──────────────────┘
└──────────────────┘
         │
         │ 1:N
         ▼
┌──────────────────┐           ┌──────────────────┐
│    ADDRESS       │           │ CUSTOMER BALANCE │
│                  │◄──────────│   (per address)  │
│ delivery/billing │   1:N     └──────────────────┘
└──────────────────┘
         │
         │ N:1 (delivery_address)
         ▼
┌──────────────────┐           ┌──────────────────┐
│     ORDER        │───────────│   ORDER LINE     │
│                  │    1:N    │                  │
│ - status         │           │ - variant_id     │
│ - order_type     │           │ - quantity       │
│ - order_source   │           │ - unit_price     │
└──────────────────┘           └──────────────────┘
         │                              │
         │ N:M (via trip_orders)        │ N:1
         ▼                              ▼
┌──────────────────┐           ┌──────────────────┐
│      TRIP        │           │ PRODUCT VARIANT  │◄─────┐
│                  │           │                  │      │
│ - vehicle_id     │           │ - sku_code       │      │
│ - driver_id      │           │ - state          │      │
│ - status         │           └──────────────────┘      │
└──────────────────┘                    │                │
    │       │                           │ N:1            │
    │       │                           ▼                │
    │       │                  ┌──────────────────┐      │
    │       │                  │    PRODUCT       │      │
    │       │                  │                  │      │
    │       │                  │ - type (bundle)  │      │
    │       │                  └──────────────────┘      │
    │       │                                            │
    │       │                  ┌──────────────────┐      │
    │       │                  │ BUNDLE COMPONENT │──────┘
    │       │                  │                  │
    │       │                  │ bundle → component│
    │       │                  └──────────────────┘
    │       │
    │       │ 1:N
    │       ▼
    │  ┌──────────────────┐
    │  │   TRIP ORDER     │  (Junction with delivery status)
    │  │                  │
    │  │ - sequence       │
    │  │ - action_type    │
    │  │ - pod_url        │
    │  └──────────────────┘
    │
    │ 1:N
    ▼
┌──────────────────┐           ┌──────────────────┐
│  TRIP INVENTORY  │           │   TRIP RETURN    │
│                  │           │                  │
│ - starting_qty   │           │ - variant_id     │
│ - actual_qty     │           │ - quantity       │
└──────────────────┘           └──────────────────┘
    │
    │ Related
    ▼
┌──────────────────┐           ┌──────────────────┐
│  LOADING PLAN    │           │ OFFLOAD DOCUMENT │
│                  │           │                  │
│ Pre-trip loading │           │ Post-trip recon  │
└──────────────────┘           └──────────────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│                           INVENTORY DOMAIN                                   │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────┐           ┌──────────────────┐
│   WAREHOUSE      │───────────│  INVENTORY LEVEL │
│                  │    1:N    │                  │
│ - type (mobile/  │           │ per variant      │
│   fixed)         │           └──────────────────┘
│ - linked_vehicle │                   │
└──────────────────┘                   │
         │                             │
         │ 1:N                         │ Audited by
         ▼                             ▼
┌──────────────────┐           ┌──────────────────┐
│    VEHICLE       │           │ INVENTORY        │
│                  │           │ TRANSACTION      │
│ - registration   │           │                  │
│ - capacity       │           │ - type           │
└──────────────────┘           │ - reference_type │
                               │ - reference_id   │
                               └──────────────────┘

┌──────────────────┐           ┌──────────────────┐
│ MANUAL ADJUSTMENT│           │VARIANT CONVERSION│
│                  │           │                  │
│ Corrections      │           │ State changes    │
└──────────────────┘           └──────────────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│                           PRICING & PAYMENTS                                 │
└─

# state_machines

Entity lifecycle state machines and transitions

I'll analyze the codebase to identify entities with state machines and status fields. Let me scan through the relevant files.

Based on my comprehensive analysis of the codebase, I've identified several entities with state machines. Let me document them:

---

### Entity: Order

**States**: DRAFT, PENDING_APPROVAL, APPROVED, ALLOCATED, DISPATCHED, IN_TRANSIT, DELIVERED, CANCELLED, FAILED, REJECTED

**State Machine Diagram**:
```
                                    ┌──────────────────────────────────────────────┐
                                    │                                              │
                                    ▼                                              │
DRAFT ──► PENDING_APPROVAL ──► APPROVED ──► ALLOCATED ──► DISPATCHED ──► IN_TRANSIT
              │                    │                          │              │
              │                    │                          │              ▼
              ▼                    ▼                          │          DELIVERED
          REJECTED             CANCELLED                      │              │
                                                              │              ▼
                                                              └─────────► FAILED
                                                                             │
                                                                             ▼
                                                                         CANCELLED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| DRAFT | PENDING_APPROVAL | submitOrder | Order has lines, customer assigned | order.submitted |
| PENDING_APPROVAL | APPROVED | approveOrder | User has approver role, business rules pass | order.approved |
| PENDING_APPROVAL | REJECTED | rejectOrder | Rejection reason provided | order.rejected |
| APPROVED | ALLOCATED | allocateInventory | Sufficient inventory available | order.allocated |
| ALLOCATED | DISPATCHED | dispatchTrip | Trip assigned and dispatched | order.dispatched |
| DISPATCHED | IN_TRANSIT | startTrip | Driver started trip | order.in_transit |
| IN_TRANSIT | DELIVERED | completeDelivery | POD captured, quantities confirmed | order.delivered |
| IN_TRANSIT | FAILED | failDelivery | Failure reason provided | order.failed |
| * (non-terminal) | CANCELLED | cancelOrder | Not in terminal state | order.cancelled |

---

### Entity: Trip

**States**: PLANNED, LOADING, DISPATCHED, IN_PROGRESS, COMPLETED, CANCELLED

**State Machine Diagram**:
```
PLANNED ──► LOADING ──► DISPATCHED ──► IN_PROGRESS ──► COMPLETED
    │           │            │              │
    │           │            │              │
    ▼           ▼            ▼              ▼
CANCELLED   CANCELLED    CANCELLED      CANCELLED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PLANNED | LOADING | startLoading | Loading plan created | trip.loading_started |
| LOADING | DISPATCHED | dispatchTrip | Inventory loaded, driver assigned | trip.dispatched |
| DISPATCHED | IN_PROGRESS | startTrip | Driver confirms start | trip.started |
| IN_PROGRESS | COMPLETED | completeTrip | All stops processed, variance approved | trip.completed |
| PLANNED | CANCELLED | cancelTrip | Not yet dispatched | trip.cancelled |
| LOADING | CANCELLED | cancelTrip | Not yet dispatched | trip.cancelled |
| DISPATCHED | CANCELLED | cancelTrip | Admin override | trip.cancelled |
| IN_PROGRESS | CANCELLED | cancelTrip | Admin override | trip.cancelled |

---

### Entity: Customer

**States**: PENDING_APPROVAL, APPROVED, REJECTED, INACTIVE

**State Machine Diagram**:
```
PENDING_APPROVAL ──► APPROVED ◄──► INACTIVE
        │
        ▼
    REJECTED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING_APPROVAL | APPROVED | approveCustomer | All required fields set, approver role | customer.approved |
| PENDING_APPROVAL | REJECTED | rejectCustomer | Rejection reason provided | customer.rejected |
| APPROVED | INACTIVE | toggleCustomerStatus | Admin/Operator role | customer.deactivated |
| INACTIVE | APPROVED | toggleCustomerStatus | Admin/Operator role | customer.reactivated |

---

### Entity: LoadingPlan

**States**: DRAFT, CONFIRMED, CANCELLED

**State Machine Diagram**:
```
DRAFT ──► CONFIRMED
  │           │
  ▼           ▼
CANCELLED  CANCELLED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| DRAFT | CONFIRMED | confirmLoadingPlan | All items verified | loading_plan.confirmed |
| DRAFT | CANCELLED | cancelLoadingPlan | Not yet confirmed | loading_plan.cancelled |
| CONFIRMED | CANCELLED | cancelLoadingPlan | Admin override, reason provided | loading_plan.cancelled |

---

### Entity: OffloadDocument

**States**: PENDING, SUBMITTED, APPROVED, REJECTED

**State Machine Diagram**:
```
PENDING ──► SUBMITTED ──► APPROVED
                │
                ▼
            REJECTED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | SUBMITTED | submitOffload | All variances documented | offload.submitted |
| SUBMITTED | APPROVED | approveVariance | Operator/Admin approval | offload.approved |
| SUBMITTED | REJECTED | rejectVariance | Rejection reason provided | offload.rejected |

---

### Entity: TripOrder (Trip Stop)

**States**: PENDING, IN_PROGRESS, COMPLETED, FAILED, CANCELLED

**State Machine Diagram**:
```
PENDING ──► IN_PROGRESS ──► COMPLETED
    │            │
    │            ▼
    │         FAILED
    │
    ▼
CANCELLED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | IN_PROGRESS | startDelivery | Driver at location | trip_order.started |
| IN_PROGRESS | COMPLETED | completeDelivery | POD captured, quantities confirmed | trip_order.completed |
| IN_PROGRESS | FAILED | failDelivery | Failure reason provided | trip_order.failed |
| PENDING | CANCELLED | cancelStop | Admin/Operator role | trip_order.cancelled |

---

### Entity: UserInvitation

**States**: PENDING, ACCEPTED, EXPIRED

**State Machine Diagram**:
```
PENDING ──► ACCEPTED
    │
    ▼
EXPIRED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | ACCEPTED | acceptInvitation | Valid token, within expiry | invitation.accepted |
| PENDING | EXPIRED | expireInvitation | Past expiry date (automatic) | invitation.expired |

---

### Entity: Payment

**States**: PENDING, APPROVED, REJECTED

**State Machine Diagram**:
```
PENDING ──► APPROVED
    │
    ▼
REJECTED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | APPROVED | approvePayment | Accountant/Admin role | payment.approved |
| PENDING | REJECTED | rejectPayment | Rejection reason provided | payment.rejected |

---

## JSON Output Block

```json
{
  "state_machines": [
    {
      "entity": "Order",
      "status_field": "status",
      "states": ["DRAFT", "PENDING_APPROVAL", "APPROVED", "ALLOCATED", "DISPATCHED", "IN_TRANSIT", "DELIVERED", "CANCELLED", "FAILED", "REJECTED"],
      "initial_state": "DRAFT",
      "terminal_states": ["DELIVERED", "CANCELLED", "FAILED", "REJECTED"],
      "transitions": [
        {
          "from": "DRAFT",
          "to": "PENDING_APPROVAL",
          "trigger": "submitOrder",
          "preconditions": ["Order has lines", "Customer assigned"],
          "events": ["order.submitted"]
        },
        {
          "from": "PENDING_APPROVAL",
          "to": "APPROVED",
          "trigger": "approveOrder",
          "preconditions": ["User has approver role", "Business rules pass"],
          "events": ["order.approved"]
        },
        {
          "from": "PENDING_APPROVAL",
          "to": "REJECTED",
          "trigger": "rejectOrder",
          "preconditions": ["Rejection reason provided"],
          "events": ["order.rejected"]
        },
        {
          "from": "APPROVED",
          "to": "ALLOCATED",
          "trigger": "allocateInventory",
          "preconditions": ["Sufficient inventory available"],
          "events": ["order.allocated"]
        },
        {
          "from": "ALLOCATED",
          "to": "DISPATCHED",
          "trigger": "dispatchTrip",
          "preconditions": ["Trip assigned and dispatched"],
          "events": ["order.dispatched"]
        },
        {
          "from": "DISPATCHED",
          "to": "IN_TRANSIT",
          "trigger": "startTrip",
          "preconditions": ["Driver started trip"],
          "events": ["order.in_transit"]
        },
        {
          "from": "IN_TRANSIT",
          "to": "DELIVERED",
          "trigger": "completeDelivery",
          "preconditions": ["POD captured", "Quantities confirmed"],
          "events": ["order.delivered"]
        },
        {
          "from": "IN_TRANSIT",
          "to": "FAILED",
          "trigger": "failDelivery",
          "preconditions": ["Failure reason provided"],
          "events": ["order.failed"]
        },
        {
          "from": "*",
          "to": "CANCELLED",
          "trigger": "cancelOrder",
          "preconditions": ["Not in terminal state"],
          "events": ["order.cancelled"]
        }
      ]
    },
    {
      "entity": "Trip",
      "status_field": "status",
      "states": ["PLANNED", "LOADING", "DISPATCHED", "IN_PROGRESS", "COMPLETED", "CANCELLED"],
      "initial_state": "PLANNED",
      "terminal_states": ["COMPLETED", "CANCELLED"],
      "transitions": [
        {
          "from": "PLANNED",
          "to": "LOADING",
          "trigger": "startLoading",
          "preconditions": ["Loading plan created"],
          "events": ["trip.loading_started"]
        },
        {
          "from": "LOADING",
          "to": "DISPATCHED",
          "trigger": "dispatchTrip",
          "preconditions": ["Inventory loaded", "Driver assigned"],
          "events": ["trip.dispatched"]
        },
        {
          "from": "DISPATCHED",
          "to": "IN_PROGRESS",
          "trigger": "startTrip",
          "preconditions": ["Driver confirms start"],
          "events": ["trip.started"]
        },
        {
          "from": "IN_PROGRESS",
          "to": "COMPLETED",
          "trigger": "completeTrip",
          "preconditions": ["All stops processed", "Variance approved"],
          "events": ["trip.completed"]
        },
        {
          "from": "PLANNED",
          "to": "CANCELLED",
          "trigger": "cancelTrip",
          "preconditions": ["Not yet dispatched"],
          "events": ["trip.cancelled"]
        },
        {
          "from": "LOADING",
          "to": "CANCELLED",
          "trigger": "cancelTrip",
          "preconditions": ["Not yet dispatched"],
          "events": ["trip.cancelled"]
        },
        {
          "from": "DISPATCHED",
          "to": "CANCELLED",
          "trigger": "cancelTrip",
          "preconditions": ["Admin override"],
          "events": ["trip.cancelled"]
        },
        {
          "from": "IN_PROGRESS",
          "to": "CANCELLED",
          "trigger": "cancelTrip",
          "preconditions": ["Admin override"],
          "events": ["trip.cancelled"]
        }
      ]
    },
    {
      "entity": "Customer",
      "status_field": "status",
      "states": ["PENDING_APPROVAL", "APPROVED", "REJECTED", "INACTIVE"],
      "initial_state": "PENDING_APPROVAL",
      "terminal_states": ["REJECTED"],
      "transitions": [
        {
          "from": "PENDING_APPROVAL",
          "to": "APPROVED",
          "trigger": "approveCustomer",
          "preconditions": ["All required fields set", "User has approver role"],
          "events": ["customer.approved"]
        },
        {
          "from": "PENDING_APPROVAL",
          "to": "REJECTED",
          "trigger": "rejectCustomer",
          "preconditions": ["Rejection reason provided"],
          "events": ["customer.rejected"]
        },
        {
          "from": "APPROVED",
          "to": "INACTIVE",
          "trigger": "toggleCustomerStatus",
          "preconditions": ["Admin or Operator role"],
          "events": ["customer.deactivated"]
        },
        {
          "from": "INACTIVE",
          "to": "APPROVED",
          "trigger": "toggleCustomerStatus",
          "preconditions": ["Admin or Operator role"],
          "events": ["customer.reactivated"]
        }
      ]
    },
    {
      "entity": "LoadingPlan",
      "status_field": "status",
      "states": ["DRAFT", "CONFIRMED", "CANCELLED"],
      "initial_state": "DRAFT",
      "terminal_states": ["CONFIRMED", "CANCELLED"],
      "transitions": [
        {
          "from": "DRAFT",
          "to": "CONFIRMED",
          "trigger": "confirmLoadingPlan",
          "preconditions": ["All items verified"],
          "events": ["loading_plan.confirmed"]
        },
        {
          "from": "DRAFT",
          "to": "CANCELLED",
          "trigger": "cancelLoadingPlan",
          "preconditions": ["Not yet confirmed"],
          "events": ["loading_plan.cancelled"]
        },
        {
          "from": "CONFIRMED",
          "to": "CANCELLED",
          "trigger": "cancelLoadingPlan",
          "preconditions": ["Admin override", "Cancellation reason provided"],
          "events": ["loading_plan.cancelled"]
        }
      ]
    },
    {
      "entity": "OffloadDocument",
      "status_field": "status",
      "states": ["PENDING", "SUBMITTED", "APPROVED", "REJECTED"],
      "initial_state": "PENDING",
      "terminal_states": ["APPROVED", "REJECTED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "SUBMITTED",
          "trigger": "submitOffload",
          "preconditions": ["All variances documented"],
          "events": ["offload.submitted"]
        },
        {
          "from": "SUBMITTED",
          "to": "APPROVED",
          "trigger": "approveVariance",
          "preconditions": ["Operator or Admin approval"],
          "events": ["offload.approved"]
        },
        {
          "from": "SUBMITTED",
          "to": "REJECTED",
          "trigger": "rejectVariance",
          "preconditions": ["Rejection reason provided"],
          "events": ["offload.rejected"]
        }
      ]
    },
    {
      "entity": "TripOrder",
      "status_field": "status",
      "states": ["PENDING", "IN_PROGRESS", "COMPLETED", "FAILED", "CANCELLED"],
      "initial_state": "PENDING",
      "terminal_states": ["COMPLETED", "FAILED", "CANCELLED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "IN_PROGRESS",
          "trigger": "startDelivery",
          "preconditions": ["Driver at location"],
          "events": ["trip_order.started"]
        },
        {
          "from": "IN_PROGRESS",
          "to": "COMPLETED",
          "trigger": "completeDelivery",
          "preconditions": ["POD captured", "Quantities confirmed"],
          "events": ["trip_order.completed"]
        },
        {
          "from": "IN_PROGRESS",
          "to": "FAILED",
          "trigger": "failDelivery",
          "preconditions": ["Failure reason provided"],
          "events": ["trip_order.failed"]
        },
        {
          "from": "PENDING",
          "to": "CANCELLED",
          "trigger": "cancelStop",
          "preconditions": ["Admin or Operator role"],
          "events": ["trip_order.cancelled"]
        }
      ]
    },
    {
      "entity": "UserInvitation",
      "status_field": "status",
      "states": ["PENDING", "ACCEPTED", "EXPIRED"],
      "initial_state": "PENDING",
      "terminal_states": ["ACCEPTED", "EXPIRED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "ACCEPTED",
          "trigger": "acceptInvitation",
          "preconditions": ["Valid token", "Within expiry period"],
          "events": ["invitation.accepted"]
        },
        {
          "from": "PENDING",
          "to": "EXPIRED",
          "trigger": "expireInvitation",
          "preconditions": ["Past expiry date"],
          "events": ["invitation.expired"]
        }
      ]
    },
    {
      "entity": "Payment",
      "status_field": "status",
      "states": ["PENDING", "APPROVED", "REJECTED"],
      "initial_state": "PENDING",
      "terminal_states": ["APPROVED", "REJECTED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "APPROVED",
          "trigger": "approvePayment",
          "preconditions": ["Accountant or Admin role"],
          "events": ["payment.approved"]
        },
        {
          "from": "PENDING",
          "to": "REJECTED",
          "trigger": "rejectPayment",
          "preconditions": ["Rejection reason provided"],
          "events": ["payment.rejected"]
        }
      ]
    }
  ]
}
```

# DBs

databases analysis

# Database Analysis for OMS System

Based on my comprehensive analysis of the codebase, I have identified **two databases** used in this system:

---

## Database: PostgreSQL (via Supabase)

* **Database Name/Type:** PostgreSQL (SQL) - hosted on Supabase
* **Purpose/Role:** Primary transactional database for the Order Management System (OMS). Stores all core business data including customers, orders, trips, products, inventory, warehouses, vehicles, user profiles, payments, and audit trails. Handles multi-tenant data isolation through Row Level Security (RLS) policies.
* **Key Technologies/Access Methods:** 
  - TypeScript/JavaScript
  - Supabase JS Client (`@supabase/supabase-js`) for frontend and backend
  - PostgreSQL RPC functions for complex business logic
  - Row Level Security (RLS) for tenant isolation
  - Database triggers and functions for automation
  - Views for optimized data retrieval

* **Key Files/Configuration:**
  - `supabase/config.toml` - Supabase project configuration
  - `supabase/migrations/` - 300+ migration files defining schema evolution
  - `supabase/dev_schema.sql`, `supabase/prod_schema.sql` - Schema snapshots
  - `src/lib/supabase.ts` - Frontend Supabase client initialization
  - `backend/src/client/supabase.ts` - Backend Supabase client
  - `backend/src/config/index.ts` - Database connection configuration
  - Environment variables: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`

* **Schema/Table Structure:**

  **Core Business Tables:**
  - `tenants`: `id` (PK), `name`, `settings` (JSONB), `created_at`
  - `user_profiles`: `id` (PK), `tenant_id` (FK), `role`, `full_name`, `email`, `phone`
  - `customers`: `id` (PK), `tenant_id` (FK), `name`, `type`, `status`, `sales_user_id` (FK), `credit_limit`
  - `addresses`: `id` (PK), `tenant_id` (FK), `customer_id` (FK), `type`, `coordinates`, `plus_code`, `is_default`
  - `orders`: `id` (PK), `tenant_id` (FK), `customer_id` (FK), `order_number`, `status`, `order_type`, `delivery_address_id`, `total_amount`, `source`
  - `order_lines`: `id` (PK), `order_id` (FK), `variant_id` (FK), `quantity`, `delivered_quantity`, `unit_price`
  
  **Product & Inventory Tables:**
  - `products`: `id` (PK), `tenant_id` (FK), `name`, `sku_code`, `type` (single/bundle/measured), `state_attr`
  - `product_variants`: `id` (PK), `product_id` (FK), `tenant_id` (FK), `name`, `sku_code`, `size`, `price`
  - `bundle_components`: `id` (PK), `bundle_variant_id` (FK), `component_variant_id` (FK), `quantity`
  - `warehouses`: `id` (PK), `tenant_id` (FK), `name`, `type` (static/mobile), `linked_vehicle_id`
  - `inventory_levels`: `id` (PK), `warehouse_id` (FK), `variant_id` (FK), `quantity`
  - `inventory_transactions`: `id` (PK), `tenant_id` (FK), `warehouse_id` (FK), `variant_id` (FK), `quantity`, `transaction_type`, `reference_type`, `reference_id`
  - `inventory_adjustments`: `id` (PK), `tenant_id` (FK), `warehouse_id` (FK), `adjustment_number`, `status`, `reason`
  
  **Logistics Tables:**
  - `vehicles`: `id` (PK), `tenant_id` (FK), `registration_number`, `driver_id` (FK), `type`
  - `trips`: `id` (PK), `tenant_id` (FK), `trip_number`, `vehicle_id` (FK), `driver_id` (FK), `status`, `dispatch_date`
  - `trip_orders`: `id` (PK), `trip_id` (FK), `order_id` (FK), `sequence`, `action_type`, `delivery_status`
  - `loading_plans`: `id` (PK), `tenant_id` (FK), `trip_id` (FK), `status`, `loaded_at`, `loaded_by`
  - `offload_documents`: `id` (PK), `tenant_id` (FK), `trip_id` (FK), `status`, `destination_warehouse_id`
  
  **Financial Tables:**
  - `price_lists`: `id` (PK), `tenant_id` (FK), `name`, `is_default`, `valid_from`, `valid_to`
  - `price_list_items`: `id` (PK), `price_list_id` (FK), `variant_id` (FK), `price`
  - `payments`: `id` (PK), `tenant_id` (FK), `customer_id` (FK), `order_id` (FK), `amount`, `payment_method`, `status`
  - `customer_balances`: `id` (PK), `customer_id` (FK), `variant_id` (FK), `quantity` (for cylinder tracking)
  - `customer_empty_balances`: `id` (PK), `customer_id` (FK), `address_id` (FK), `variant_id` (FK), `quantity`
  
  **Configuration Tables:**
  - `business_rules`: `id` (PK), `tenant_id` (FK), `rule_type`, `entity_type`, `rule_key`, `config` (JSONB)
  - `workflows`: `id` (PK), `tenant_id` (FK), `entity_type`, `status_key`, `config` (JSONB)
  - `type_definitions`: `id` (PK), `tenant_id` (FK), `category`, `code`, `label`, `config` (JSONB)
  - `tenant_external_api_credentials`: `id` (PK), `tenant_id` (FK), `provider`, `credentials` (JSONB)
  
  **Audit Tables:**
  - `order_audit`: `id` (PK), `order_id` (FK), `action`, `changes` (JSONB), `performed_by`, `created_at`
  - `customer_profile_history`: `id` (PK), `customer_id` (FK), `changes` (JSONB), `changed_by`
  - `event_store`: `id` (PK), `tenant_id` (FK), `event_type`, `aggregate_type`, `aggregate_id`, `payload` (JSONB)

  **Key Views (Optimized Queries):**
  - `orders_list_view` - Enriched orders with customer/address/line details
  - `customers_list_view` - Customers with default addresses and sales agent info
  - `trips_list_view` - Trips with driver/vehicle/stop counts
  - `inventory_levels_view` - Inventory with warehouse and variant details
  - `dashboard_metrics_view` - Pre-aggregated KPIs
  - `trip_orders_with_addresses_view` - Trip stops with full delivery info
  - `product_variants_with_pricing_view` - Variants with current pricing

* **Key Entities and Relationships:**
  - **Tenant** (1) → **User Profiles** (M), **Customers** (M), **Orders** (M), **Products** (M), etc.
  - **Customer** (1) → **Orders** (M), **Addresses** (M), **Payments** (M), **Customer Balances** (M)
  - **Order** (1) → **Order Lines** (M), **Trip Orders** (M), **Payments** (M)
  - **Product** (1) → **Product Variants** (M)
  - **Product Variant** (1) → **Bundle Components** (M as bundle), **Order Lines** (M), **Inventory Levels** (M)
  - **Warehouse** (1) → **Inventory Levels** (M), **Inventory Transactions** (M)
  - **Vehicle** (1) → **Trips** (M), **Warehouse** (1 - mobile warehouse link)
  - **Trip** (1) → **Trip Orders** (M), **Loading Plans** (M), **Offload Documents** (M)
  - **User Profile** → **Customer** (as sales_user), **Orders** (as created_by), **Trips** (as driver)

* **Interacting Components:**
  - **Frontend Application** (React/TypeScript via `src/lib/supabase.ts`)
  - **Backend API Service** (Fastify via `backend/src/client/supabase.ts`)
  - **Authentication Service** (Supabase Auth integration)
  - **Order Management Module** (`backend/src/routes/orders.ts`, `src/pages/Orders.tsx`)
  - **Customer Management Module** (`backend/src/routes/customers.ts`, `src/pages/Customers.tsx`)
  - **Inventory Management Module** (`backend/src/routes/inventory*.ts`, `src/pages/Inventory.tsx`)
  - **Trip/Logistics Module** (`backend/src/routes/trips.ts`, `src/pages/Trips.tsx`)
  - **Approval Workflows** (`backend/src/routes/approval*.ts`, `src/components/approvals/`)
  - **Reporting Module** (`src/pages/Reports.tsx`, various analytics views)
  - **Mobile API** (PostgreSQL RPC functions like `get_driver_trip_mobile`, `complete_delivery`)

---

## Database: Redis

* **Database Name/Type:** Redis (NoSQL - Key-Value Store / In-Memory Cache)
* **Purpose/Role:** Used as a Layer 2 (L2) caching system to improve performance by caching frequently accessed data from the primary PostgreSQL database. Reduces database load and improves response times for read-heavy operations.
* **Key Technologies/Access Methods:**
  - Node.js
  - `ioredis` client library
  - Custom caching service with TTL management
  - Cache invalidation via application events

* **Key Files/Configuration:**
  - `backend/src/services/cache.ts` - Main cache service implementation
  - `backend/src/services/CacheService.ts` - Cache service class
  - `backend/src/config/index.ts` - Redis connection configuration
  - `backend/test-redis-connection.cjs` - Redis connection test script
  - `backend/test-l2-cache.cjs` - L2 cache testing
  - Environment variables: `REDIS_URL`, `REDIS_HOST`, `REDIS_PORT`, `REDIS_PASSWORD`

* **Schema/Collection Structure (Cache Keys):**
  Based on the caching service patterns observed:
  - `{tenant_id}:orders:{order_id}` - Cached order details
  - `{tenant_id}:orders:list:{filters_hash}` - Cached paginated order lists
  - `{tenant_id}:customers:{customer_id}` - Cached customer details
  - `{tenant_id}:customers:list:{filters_hash}` - Cached customer lists
  - `{tenant_id}:products:{product_id}` - Cached product details
  - `{tenant_id}:variants:{variant_id}` - Cached variant details
  - `{tenant_id}:inventory:{warehouse_id}` - Cached inventory levels
  - `{tenant_id}:trips:{trip_id}` - Cached trip details
  - `{tenant_id}:price_lists:{price_list_id}` - Cached price list data
  - `{tenant_id}:dashboard:metrics` - Cached dashboard KPIs

* **Key Entities and Relationships:**
  - **Cached Order** - JSON representation of order with lines
  - **Cached Customer** - JSON representation of customer with addresses
  - **Cached Product/Variant** - JSON representation with pricing info
  - **Cached Inventory** - JSON representation of warehouse inventory levels
  - **Relationships:** Cache keys are namespaced by tenant_id for multi-tenant isolation; relationships between cached entities are managed at the application layer during cache population/invalidation

* **Interacting Components:**
  - **Cache Service** (`backend/src/services/cache.ts`) - Core caching logic
  - **Order Service** - Caches order queries and invalidates on mutations
  - **Customer Service** - Caches customer data
  - **Inventory Service** - Caches inventory levels
  - **Trip Service** - Caches trip data
  - **API Route Handlers** - Use cache service for read operations
  - **Event Handlers** - Trigger cache invalidation on data changes

---

## Summary

| Database | Type | Primary Purpose |
|----------|------|-----------------|
| PostgreSQL (Supabase) | SQL | Primary transactional database - stores all business data with multi-tenant isolation via RLS |
| Redis | NoSQL (Key-Value) | L2 caching layer - improves read performance and reduces database load |

# APIs

APIs analysis

# HTTP API Documentation - OMS System Backend

## Overview

This documentation covers all HTTP API endpoints exposed by the OMS (Order Management System) backend, built with Fastify framework.

---

## Authentication

All endpoints require authentication via Bearer token in the `Authorization` header unless otherwise noted.

```
Authorization: Bearer <supabase_jwt_token>
```

---

## Base URL

- **Development**: `http://localhost:3000`
- **Staging**: Configured via environment
- **Production**: Configured via environment

---

## API Endpoints

### Health & System

#### Health Check

- **HTTP Method**: `GET`
- **API URL**: `/health`
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "status": "healthy",
  "version": "string",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "services": {
    "database": "connected",
    "redis": "connected"
  }
}
```
- **Description**: Returns the health status of the backend service and its dependencies.

---

### Authentication Routes

#### Verify Token

- **HTTP Method**: `GET`
- **API URL**: `/auth/verify`
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "valid": true,
  "user": {
    "id": "uuid",
    "email": "string",
    "role": "string",
    "tenant_id": "uuid"
  }
}
```
- **Description**: Verifies the current authentication token and returns user information.

#### Get Current User

- **HTTP Method**: `GET`
- **API URL**: `/auth/me`
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "id": "uuid",
  "email": "string",
  "full_name": "string",
  "role": "string",
  "tenant_id": "uuid",
  "permissions": ["string"]
}
```
- **Description**: Returns the current authenticated user's profile and permissions.

---

### Customers

#### List Customers

- **HTTP Method**: `GET`
- **API URL**: `/customers`
- **Query Parameters**:
  - `page` (number, optional): Page number for pagination
  - `limit` (number, optional): Number of items per page
  - `search` (string, optional): Search term for filtering
  - `status` (string, optional): Filter by customer status
  - `sales_agent_id` (uuid, optional): Filter by sales agent
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "string",
      "code": "string",
      "status": "active|pending|inactive",
      "phone": "string",
      "email": "string",
      "sales_agent_id": "uuid",
      "created_at": "timestamp",
      "updated_at": "timestamp"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```
- **Description**: Retrieves a paginated list of customers for the current tenant.

#### Get Customer by ID

- **HTTP Method**: `GET`
- **API URL**: `/customers/{id}`
- **Path Parameters**:
  - `id` (uuid): Customer ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "code": "string",
  "status": "string",
  "phone": "string",
  "email": "string",
  "billing_address": "string",
  "sales_agent_id": "uuid",
  "price_list_id": "uuid",
  "addresses": [
    {
      "id": "uuid",
      "name": "string",
      "address_line_1": "string",
      "city": "string",
      "coordinates": { "lat": 0, "lng": 0 }
    }
  ],
  "created_at": "timestamp",
  "updated_at": "timestamp"
}
```
- **Description**: Retrieves detailed information about a specific customer including addresses.

#### Create Customer

- **HTTP Method**: `POST`
- **API URL**: `/customers`
- **Request Payload**:
```json
{
  "name": "string (required)",
  "code": "string (optional)",
  "phone": "string (optional)",
  "email": "string (optional)",
  "billing_address": "string (optional)",
  "sales_agent_id": "uuid (optional)",
  "price_list_id": "uuid (optional)",
  "addresses": [
    {
      "name": "string",
      "address_line_1": "string",
      "city": "string",
      "coordinates": { "lat": 0, "lng": 0 }
    }
  ]
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "code": "string",
  "status": "pending",
  "created_at": "timestamp"
}
```
- **Description**: Creates a new customer record. Customer starts in "pending" status awaiting approval.

#### Update Customer

- **HTTP Method**: `PUT`
- **API URL**: `/customers/{id}`
- **Path Parameters**:
  - `id` (uuid): Customer ID
- **Request Payload**:
```json
{
  "name": "string (optional)",
  "phone": "string (optional)",
  "email": "string (optional)",
  "billing_address": "string (optional)",
  "sales_agent_id": "uuid (optional)",
  "price_list_id": "uuid (optional)"
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "updated_at": "timestamp"
}
```
- **Description**: Updates an existing customer's information.

#### Delete Customer

- **HTTP Method**: `DELETE`
- **API URL**: `/customers/{id}`
- **Path Parameters**:
  - `id` (uuid): Customer ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "success": true,
  "message": "Customer deleted successfully"
}
```
- **Description**: Soft deletes a customer record.

#### Get Customer Addresses

- **HTTP Method**: `GET`
- **API URL**: `/customers/{id}/addresses`
- **Path Parameters**:
  - `id` (uuid): Customer ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "string",
      "address_line_1": "string",
      "address_line_2": "string",
      "city": "string",
      "state": "string",
      "postal_code": "string",
      "coordinates": { "lat": 0, "lng": 0 },
      "is_default": true
    }
  ]
}
```
- **Description**: Retrieves all delivery addresses for a customer.

#### Add Customer Address

- **HTTP Method**: `POST`
- **API URL**: `/customers/{id}/addresses`
- **Path Parameters**:
  - `id` (uuid): Customer ID
- **Request Payload**:
```json
{
  "name": "string (required)",
  "address_line_1": "string (required)",
  "address_line_2": "string (optional)",
  "city": "string (required)",
  "state": "string (optional)",
  "postal_code": "string (optional)",
  "coordinates": { "lat": 0, "lng": 0 },
  "is_default": false
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "created_at": "timestamp"
}
```
- **Description**: Adds a new delivery address for a customer.

---

### Orders

#### List Orders

- **HTTP Method**: `GET`
- **API URL**: `/orders`
- **Query Parameters**:
  - `page` (number, optional): Page number
  - `limit` (number, optional): Items per page
  - `status` (string, optional): Filter by status (pending, approved, in_transit, delivered, cancelled)
  - `customer_id` (uuid, optional): Filter by customer
  - `date_from` (date, optional): Start date filter
  - `date_to` (date, optional): End date filter
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "data": [
    {
      "id": "uuid",
      "order_number": "string",
      "customer_id": "uuid",
      "customer_name": "string",
      "status": "string",
      "order_type": "string",
      "total_amount": 0,
      "delivery_date": "date",
      "created_at": "timestamp"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100
  }
}
```
- **Description**: Retrieves a paginated list of orders with optional filtering.

#### Get Order by ID

- **HTTP Method**: `GET`
- **API URL**: `/orders/{id}`
- **Path Parameters**:
  - `id` (uuid): Order ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "id": "uuid",
  "order_number": "string",
  "customer_id": "uuid",
  "customer": {
    "id": "uuid",
    "name": "string"
  },
  "status": "string",
  "order_type": "string",
  "delivery_address_id": "uuid",
  "delivery_address": {},
  "order_lines": [
    {
      "id": "uuid",
      "product_variant_id": "uuid",
      "quantity": 0,
      "unit_price": 0,
      "line_total": 0
    }
  ],
  "total_amount": 0,
  "notes": "string",
  "created_at": "timestamp",
  "updated_at": "timestamp"
}
```
- **Description**: Retrieves detailed order information including line items.

#### Create Order

- **HTTP Method**: `POST`
- **API URL**: `/orders`
- **Request Payload**:
```json
{
  "customer_id": "uuid (required)",
  "delivery_address_id": "uuid (required)",
  "order_type": "string (optional, default: standard)",
  "delivery_date": "date (optional)",
  "notes": "string (optional)",
  "order_lines": [
    {
      "product_variant_id": "uuid (required)",
      "quantity": "number (required)",
      "unit_price": "number (optional)"
    }
  ]
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "order_number": "string",
  "status": "pending",
  "created_at": "timestamp"
}
```
- **Description**: Creates a new order with line items. Order enters pending status for approval workflow.

#### Update Order

- **HTTP Method**: `PUT`
- **API URL**: `/orders/{id}`
- **Path Parameters**:
  - `id` (uuid): Order ID
- **Request Payload**:
```json
{
  "delivery_address_id": "uuid (optional)",
  "delivery_date": "date (optional)",
  "notes": "string (optional)",
  "order_lines": [
    {
      "id": "uuid (optional, for updates)",
      "product_variant_id": "uuid",
      "quantity": "number",
      "unit_price": "number"
    }
  ]
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "updated_at": "timestamp"
}
```
- **Description**: Updates an existing order. Only allowed for orders in certain statuses.

#### Delete Order

- **HTTP Method**: `DELETE`
- **API URL**: `/orders/{id}`
- **Path Parameters**:
  - `id` (uuid): Order ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "success": true,
  "message": "Order deleted successfully"
}
```
- **Description**: Deletes an order. Only allowed for orders in pending status.

#### Approve Order

- **HTTP Method**: `POST`
- **API URL**: `/orders/{id}/approve`
- **Path Parameters**:
  - `id` (uuid): Order ID
- **Request Payload**:
```json
{
  "notes": "string (optional)"
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "status": "approved",
  "approved_at": "timestamp",
  "approved_by": "uuid"
}
```
- **Description**: Approves a pending order, making it ready for trip assignment.

#### Reject Order

- **HTTP Method**: `POST`
- **API URL**: `/orders/{id}/reject`
- **Path Parameters**:
  - `id` (uuid): Order ID
- **Request Payload**:
```json
{
  "reason": "string (required)"
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "status": "rejected",
  "rejection_reason": "string"
}
```
- **Description**: Rejects a pending order with a reason.

---

### Products

#### List Products

- **HTTP Method**: `GET`
- **API URL**: `/products`
- **Query Parameters**:
  - `page` (number, optional): Page number
  - `limit` (number, optional): Items per page
  - `search` (string, optional): Search by name or SKU
  - `category` (string, optional): Filter by category
  - `active` (boolean, optional): Filter by active status
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "string",
      "sku": "string",
      "category": "string",
      "product_type": "standard|bundle|measured",
      "is_active": true,
      "variants": [
        {
          "id": "uuid",
          "name": "string",
          "sku_code": "string",
          "size": "string"
        }
      ]
    }
  ],
  "pagination": {}
}
```
- **Description**: Retrieves list of products with their variants.

#### Get Product by ID

- **HTTP Method**: `GET`
- **API URL**: `/products/{id}`
- **Path Parameters**:
  - `id` (uuid): Product ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "description": "string",
  "sku": "string",
  "category": "string",
  "product_type": "string",
  "is_active": true,
  "variants": [
    {
      "id": "uuid",
      "name": "string",
      "sku_code": "string",
      "size": "string",
      "weight": 0,
      "is_active": true
    }
  ],
  "bundle_components": [
    {
      "component_variant_id": "uuid",
      "quantity": 0
    }
  ]
}
```
- **Description**: Retrieves detailed product information including variants and bundle components.

#### Create Product

- **HTTP Method**: `POST`
- **API URL**: `/products`
- **Request Payload**:
```json
{
  "name": "string (required)",
  "description": "string (optional)",
  "sku": "string (required)",
  "category": "string (optional)",
  "product_type": "standard|bundle|measured (default: standard)",
  "is_active": true,
  "variants": [
    {
      "name": "string",
      "sku_code": "string",
      "size": "string",
      "weight": 0
    }
  ]
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "created_at": "timestamp"
}
```
- **Description**: Creates a new product with optional variants.

#### Update Product

- **HTTP Method**: `PUT`
- **API URL**: `/products/{id}`
- **Path Parameters**:
  - `id` (uuid): Product ID
- **Request Payload**:
```json
{
  "name": "string (optional)",
  "description": "string (optional)",
  "category": "string (optional)",
  "is_active": "boolean (optional)"
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "updated_at": "timestamp"
}
```
- **Description**: Updates product information.

#### Delete Product

- **HTTP Method**: `DELETE`
- **API URL**: `/products/{id}`
- **Path Parameters**:
  - `id` (uuid): Product ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "success": true
}
```
- **Description**: Deactivates a product (soft delete).

---

### Product Variants

#### List Product Variants

- **HTTP Method**: `GET`
- **API URL**: `/product-variants`
- **Query Parameters**:
  - `product_id` (uuid, optional): Filter by product
  - `active` (boolean, optional): Filter by active status
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "data": [
    {
      "id": "uuid",
      "product_id": "uuid",
      "name": "string",
      "sku_code": "string",
      "size": "string",
      "weight": 0,
      "is_active": true
    }
  ]
}
```
- **Description**: Retrieves list of product variants.

#### Get Product Variant by ID

- **HTTP Method**: `GET`
- **API URL**: `/product-variants/{id}`
- **Path Parameters**:
  - `id` (uuid): Variant ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "id": "uuid",
  "product_id": "uuid",
  "name": "string",
  "sku_code": "string",
  "size": "string",
  "weight": 0,
  "is_active": true,
  "product": {
    "id": "uuid",
    "name": "string"
  }
}
```
- **Description**: Retrieves detailed variant information.

#### Create Product Variant

- **HTTP Method**: `POST`
- **API URL**: `/product-variants`
- **Request Payload**:
```json
{
  "product_id": "uuid (required)",
  "name": "string (required)",
  "sku_code": "string (required)",
  "size": "string (optional)",
  "weight": "number (optional)",
  "is_active": true
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "name": "string",
  "created_at": "timestamp"
}
```
- **Description**: Creates a new variant for a product.

#### Update Product Variant

- **HTTP Method**: `PUT`
- **API URL**: `/product-variants/{id}`
- **Path Parameters**:
  - `id` (uuid): Variant ID
- **Request Payload**:
```json
{
  "name": "string (optional)",
  "size": "string (optional)",
  "weight": "number (optional)",
  "is_active": "boolean (optional)"
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "updated_at": "timestamp"
}
```
- **Description**: Updates variant information.

#### Delete Product Variant

- **HTTP Method**: `DELETE`
- **API URL**: `/product-variants/{id}`
- **Path Parameters**:
  - `id` (uuid): Variant ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "success": true
}
```
- **Description**: Deactivates a product variant.

---

### Trips

#### List Trips

- **HTTP Method**: `GET`
- **API URL**: `/trips`
- **Query Parameters**:
  - `page` (number, optional): Page number
  - `limit` (number, optional): Items per page
  - `status` (string, optional): Filter by status
  - `driver_id` (uuid, optional): Filter by driver
  - `vehicle_id` (uuid, optional): Filter by vehicle
  - `date` (date, optional): Filter by trip date
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "data": [
    {
      "id": "uuid",
      "trip_number": "string",
      "status": "planned|dispatched|in_progress|completed|cancelled",
      "driver_id": "uuid",
      "driver_name": "string",
      "vehicle_id": "uuid",
      "vehicle_number": "string",
      "scheduled_date": "date",
      "stops_count": 0,
      "completed_stops": 0
    }
  ],
  "pagination": {}
}
```
- **Description**: Retrieves list of trips with summary information.

#### Get Trip by ID

- **HTTP Method**: `GET`
- **API URL**: `/trips/{id}`
- **Path Parameters**:
  - `id` (uuid): Trip ID
- **Request Payload**: N/A
- **Response Payload**:
```json
{
  "id": "uuid",
  "trip_number": "string",
  "status": "string",
  "driver_id": "uuid",
  "driver": {
    "id": "uuid",
    "full_name": "string"
  },
  "vehicle_id": "uuid",
  "vehicle": {
    "id": "uuid",
    "registration_number": "string"
  },
  "scheduled_date": "date",
  "started_at": "timestamp",
  "completed_at": "timestamp",
  "trip_orders": [
    {
      "id": "uuid",
      "order_id": "uuid",
      "sequence": 0,
      "status": "string",
      "order": {}
    }
  ],
  "trip_inventory": [
    {
      "product_variant_id": "uuid",
      "loaded_quantity": 0,
      "delivered_quantity": 0,
      "returned_quantity": 0
    }
  ]
}
```
- **Description**: Retrieves detailed trip information with orders and inventory.

#### Create Trip

- **HTTP Method**: `POST`
- **API URL**: `/trips`
- **Request Payload**:
```json
{
  "driver_id": "uuid (required)",
  "vehicle_id": "uuid (required)",
  "scheduled_date": "date (required)",
  "order_ids": ["uuid"],
  "notes": "string (optional)"
}
```
- **Response Payload**:
```json
{
  "id": "uuid",
  "trip_number": "string",
  "status": "planned",
  "created_at": "timestamp"
}
```
- **Description**: Creates a new trip with assigned orders.

#### Update Trip

- **HTTP Method**: `PUT`
- **API URL**: `/trips/{id}`
- **Path Parameters**:
  - `id` (uuid): Trip

# events

events analysis

# Event Documentation Analysis

After a comprehensive scan of the codebase, I have identified the following events being consumed and produced by the system.

---

## Events Produced

---

### Event: Order Created

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `order.created`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "orderId": "string (UUID)",
      "tenantId": "string (UUID)",
      "customerId": "string (UUID)",
      "orderNumber": "string",
      "status": "string",
      "totalAmount": "number",
      "createdBy": "string (UUID)",
      "createdAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted whenever a new order is successfully created in the system. It can be used by other services or components to trigger downstream actions such as notifications, analytics tracking, or inventory checks.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/orderService.ts`

---

### Event: Order Updated

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `order.updated`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "orderId": "string (UUID)",
      "tenantId": "string (UUID)",
      "previousStatus": "string",
      "newStatus": "string",
      "updatedBy": "string (UUID)",
      "updatedAt": "string (ISO 8601 datetime)",
      "changes": "object (key-value pairs of changed fields)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when an existing order is updated, including status changes. It enables reactive workflows like sending status update notifications or triggering approval flows.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/orderService.ts`

---

### Event: Order Deleted

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `order.deleted`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "orderId": "string (UUID)",
      "tenantId": "string (UUID)",
      "deletedBy": "string (UUID)",
      "deletedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when an order is deleted from the system. It can trigger cleanup operations or audit logging.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/orderService.ts`

---

### Event: Customer Created

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `customer.created`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "customerId": "string (UUID)",
      "tenantId": "string (UUID)",
      "name": "string",
      "status": "string",
      "createdBy": "string (UUID)",
      "createdAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when a new customer is created, allowing other services to react (e.g., welcome notifications, CRM integrations, analytics).

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/customerService.ts`

---

### Event: Customer Updated

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `customer.updated`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "customerId": "string (UUID)",
      "tenantId": "string (UUID)",
      "updatedBy": "string (UUID)",
      "updatedAt": "string (ISO 8601 datetime)",
      "changes": "object (key-value pairs of changed fields)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when customer details are modified, enabling reactive updates across the system.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/customerService.ts`

---

### Event: Trip Created

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `trip.created`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "tripId": "string (UUID)",
      "tenantId": "string (UUID)",
      "tripNumber": "string",
      "driverId": "string (UUID)",
      "vehicleId": "string (UUID)",
      "status": "string",
      "scheduledDate": "string (ISO 8601 date)",
      "createdBy": "string (UUID)",
      "createdAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when a new delivery trip is created, allowing logistics components to prepare inventory allocation and driver notifications.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/tripService.ts`

---

### Event: Trip Updated

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `trip.updated`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "tripId": "string (UUID)",
      "tenantId": "string (UUID)",
      "previousStatus": "string",
      "newStatus": "string",
      "updatedBy": "string (UUID)",
      "updatedAt": "string (ISO 8601 datetime)",
      "changes": "object"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when a trip is modified or its status changes, enabling real-time tracking updates and inventory reconciliation workflows.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/tripService.ts`

---

### Event: Inventory Adjusted

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `inventory.adjusted`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "adjustmentId": "string (UUID)",
      "tenantId": "string (UUID)",
      "warehouseId": "string (UUID)",
      "variantId": "string (UUID)",
      "previousQuantity": "number",
      "newQuantity": "number",
      "adjustmentType": "string",
      "reason": "string",
      "adjustedBy": "string (UUID)",
      "adjustedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when inventory levels are manually adjusted, allowing for audit trails and triggering reorder workflows if thresholds are crossed.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/inventoryService.ts`

---

### Event: Price List Updated

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `pricelist.updated`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "priceListId": "string (UUID)",
      "tenantId": "string (UUID)",
      "name": "string",
      "updatedBy": "string (UUID)",
      "updatedAt": "string (ISO 8601 datetime)",
      "affectedVariants": "array of string (variant IDs)"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted when a price list is updated, enabling cache invalidation and notifying dependent systems about pricing changes.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/priceListService.ts`

---

### Event: Cache Invalidation

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `cache.invalidate`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "entityType": "string (e.g., 'customer', 'order', 'product', 'inventory')",
      "entityId": "string (UUID) | null",
      "tenantId": "string (UUID)",
      "pattern": "string (cache key pattern to invalidate)",
      "reason": "string"
    }
    ```
* **Short explanation of what this event is doing:** This event is emitted to signal that cached data should be invalidated. It's consumed by the caching layer to ensure data consistency across the system.

**Source:** `backend/src/events/eventBus.ts`, `backend/src/services/cacheService.ts`

---

## Events Consumed

---

### Event: Cache Invalidation Handler

* **Event Type:** Internal Event Bus (Custom EventBus using Node.js EventEmitter)
* **Event Name/Topic/Queue:** `cache.invalidate`
* **Direction:** Consuming
* **Event Payload:**
    ```json
    {
      "entityType": "string",
      "entityId": "string (UUID) | null",
      "tenantId": "string (UUID)",
      "pattern": "string",
      "reason": "string"
    }
    ```
* **Short explanation of what this event is doing:** The cache service listens to this event and invalidates the appropriate Redis cache entries based on the entity type and pattern provided. This ensures stale data is not served after mutations.

**Source:** `backend/src/services/cacheService.ts`

---

## WebSocket Events (Real-time Updates)

The system also uses WebSocket connections for real-time updates to the frontend:

---

### Event: Entity Update Notification

* **Event Type:** WebSocket (via Supabase Realtime)
* **Event Name/Topic/Queue:** `realtime:{table_name}`
* **Direction:** Producing (to connected clients)
* **Event Payload:**
    ```json
    {
      "type": "string ('INSERT' | 'UPDATE' | 'DELETE')",
      "table": "string",
      "schema": "string",
      "record": "object (the affected record)",
      "old_record": "object | null (previous state for updates/deletes)"
    }
    ```
* **Short explanation of what this event is doing:** Real-time database changes are broadcast to subscribed frontend clients via Supabase Realtime, enabling live updates to dashboards, order lists, and trip tracking without polling.

**Source:** `src/lib/WebSocketClient.ts`, `src/hooks/useWebSocketSubscription.ts`

---

## WhatsApp Integration Events

---

### Event: WhatsApp Message Send

* **Event Type:** External HTTP API (WhatsApp Business API)
* **Event Name/Topic/Queue:** WhatsApp Business API endpoint
* **Direction:** Producing (outbound API call)
* **Event Payload:**
    ```json
    {
      "messaging_product": "whatsapp",
      "to": "string (phone number)",
      "type": "string ('template' | 'text')",
      "template": {
        "name": "string",
        "language": {
          "code": "string"
        },
        "components": "array"
      }
    }
    ```
* **Short explanation of what this event is doing:** The system sends WhatsApp messages to customers for order notifications, delivery updates, and other communications through the WhatsApp Business API.

**Source:** `backend/src/external-apis/whatsapp/`

---

## Summary

| Event Name | Event Type | Direction | Purpose |
|------------|------------|-----------|---------|
| `order.created` | Internal EventBus | Producing | Notify system of new orders |
| `order.updated` | Internal EventBus | Producing | Notify system of order changes |
| `order.deleted` | Internal EventBus | Producing | Notify system of order deletions |
| `customer.created` | Internal EventBus | Producing | Notify system of new customers |
| `customer.updated` | Internal EventBus | Producing | Notify system of customer changes |
| `trip.created` | Internal EventBus | Producing | Notify system of new trips |
| `trip.updated` | Internal EventBus | Producing | Notify system of trip changes |
| `inventory.adjusted` | Internal EventBus | Producing | Track inventory adjustments |
| `pricelist.updated` | Internal EventBus | Producing | Notify of pricing changes |
| `cache.invalidate` | Internal EventBus | Producing/Consuming | Cache invalidation coordination |
| `realtime:{table}` | WebSocket (Supabase) | Producing | Real-time UI updates |
| WhatsApp Messages | External HTTP API | Producing | Customer notifications |

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis Report

## Repository: oms-system_bed8df41

This analysis identifies all external dependencies in the OMS (Order Management System) codebase, including services, libraries, and resources required for runtime operation.

---

## Table of Contents

1. [Cloud Services & Infrastructure](#cloud-services--infrastructure)
2. [Database Services](#database-services)
3. [Caching Services](#caching-services)
4. [Monitoring & Observability](#monitoring--observability)
5. [Analytics Services](#analytics-services)
6. [Messaging & Event Streaming](#messaging--event-streaming)
7. [External APIs](#external-apis)
8. [Authentication Services](#authentication-services)
9. [Frontend Libraries](#frontend-libraries)
10. [Backend Libraries](#backend-libraries)
11. [Development & Testing Libraries](#development--testing-libraries)
12. [Deployment Platforms](#deployment-platforms)

---

## Cloud Services & Infrastructure

### 1. Supabase (Backend-as-a-Service)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Supabase |
| **Type of Dependency** | External Service / Database Platform |
| **Purpose/Role** | Primary database platform providing PostgreSQL database, authentication, real-time subscriptions, storage, and serverless functions. Core data persistence layer for the entire OMS. |
| **Integration Point/Clues** | - Library: `@supabase/supabase-js` in both frontend (`package.json`) and backend (`backend/package.json`)<br>- Configuration: Environment variables `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` in `backend/docker-compose.yml`<br>- Migration files in `supabase/migrations/` (300+ migration files)<br>- Client initialization in `src/lib/supabase.ts`<br>- Backend client in `backend/src/client/`<br>- Edge functions in `supabase/functions/` |

### 2. Google Cloud Platform (GCP)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google Cloud Platform |
| **Type of Dependency** | Cloud Infrastructure Provider |
| **Purpose/Role** | Hosting platform for backend services using Cloud Run for containerized deployments. |
| **Integration Point/Clues** | - Cloud Run configurations in `infra/cloud-run/oms-backend.production.yaml` and `infra/cloud-run/oms-backend.staging.yaml`<br>- Grafana Alloy GCP configs in `infra/grafana-alloy/cloud-run.production.yaml` and `infra/grafana-alloy/cloud-run.staging.yaml`<br>- Deployment workflow in `.github/workflows/deploy-gcp.yml`<br>- Documentation in `docs/deploy/GCP-CLOUD-RUN-SETUP.md` |

### 3. Netlify

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Netlify |
| **Type of Dependency** | Frontend Hosting / Serverless Functions |
| **Purpose/Role** | Hosts the frontend React application and provides serverless function endpoints for Google Maps configuration and user invitations. |
| **Integration Point/Clues** | - Configuration file: `netlify.toml`<br>- Serverless functions in `netlify/functions/google-maps-config.js` and `netlify/functions/send-user-invite.js`<br>- Frontend CI workflow in `.github/workflows/frontend-ci.yml` |

### 4. Railway

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Railway |
| **Type of Dependency** | Cloud Deployment Platform |
| **Purpose/Role** | Alternative deployment platform for the backend service (likely staging environment). |
| **Integration Point/Clues** | - Configuration files: `.railwayproject`, `backend/railway.json`<br>- Ignore file: `backend/.railwayignore`<br>- Verification script: `backend/verify-railway-deployment.js`<br>- Staging deployment script: `backend/deploy-staging.sh` |

---

## Database Services

### 5. PostgreSQL (via Supabase)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | PostgreSQL Database |
| **Type of Dependency** | External Database |
| **Purpose/Role** | Primary relational database for storing all application data including orders, customers, inventory, trips, and audit logs. |
| **Integration Point/Clues** | - Extensive SQL migrations in `supabase/migrations/`<br>- Schema files: `supabase/dev_schema.sql`, `supabase/prod_schema.sql`<br>- RPC functions and views defined in migrations<br>- Configuration in `supabase/config.toml` |

### 6. Redis

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Redis |
| **Type of Dependency** | Caching Service |
| **Purpose/Role** | L2 caching layer for improving performance by caching frequently accessed data and reducing database load. |
| **Integration Point/Clues** | - Library: `ioredis` in `backend/package.json`<br>- Docker service defined in `backend/docker-compose.yml` (redis:7-alpine image)<br>- Environment variables: `ENABLE_CACHE`, `REDIS_ENABLED`<br>- Test files: `backend/test-redis-connection.cjs`, `backend/test-l2-cache.cjs`<br>- Documentation in `docs/archive/REDIS_L2_CACHE_FIX_PLAN.md` |

---

## Monitoring & Observability

### 7. Sentry

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Sentry |
| **Type of Dependency** | Error Monitoring / APM Service |
| **Purpose/Role** | Application performance monitoring, error tracking, and profiling for both frontend and backend. |
| **Integration Point/Clues** | - Frontend libraries: `@sentry/react`, `@sentry/tracing` in `package.json`<br>- Backend libraries: `@sentry/node`, `@sentry/profiling-node` in `backend/package.json`<br>- Initialization file: `src/lib/sentry.ts`<br>- Documentation: `docs/archive/SENTRY_INTEGRATION.md` |

### 8. Grafana Cloud (via Alloy)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Grafana Cloud |
| **Type of Dependency** | Monitoring / Observability Platform |
| **Purpose/Role** | Centralized logging, metrics, and distributed tracing using Grafana Alloy as the collector agent. |
| **Integration Point/Clues** | - Grafana Alloy configurations in `grafana-alloy/` and `infra/grafana-alloy/`<br>- Dockerfiles for Alloy: `grafana-alloy/Dockerfile`, `infra/grafana-alloy/Dockerfile`<br>- Alloy config files: `grafana-alloy/config.alloy`, `infra/grafana-alloy/config.gcp.alloy`<br>- Dashboard definitions in `grafana/` directory<br>- Deployment workflow: `.github/workflows/deploy-alloy-gcp.yml` |

### 9. Grafana Loki (Log Aggregation)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Grafana Loki |
| **Type of Dependency** | Log Aggregation Service |
| **Purpose/Role** | Centralized log aggregation and querying, integrated with Grafana for visualization. |
| **Integration Point/Clues** | - Library: `pino-loki` in `backend/package.json`<br>- Configuration likely in Alloy config files |

### 10. OpenTelemetry (OTLP)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | OpenTelemetry |
| **Type of Dependency** | Observability Framework / Distributed Tracing |
| **Purpose/Role** | Provides distributed tracing capabilities, sending trace data to observability backends. |
| **Integration Point/Clues** | - Libraries in `backend/package.json`: `@opentelemetry/auto-instrumentations-node`, `@opentelemetry/exporter-trace-otlp-http`, `@opentelemetry/instrumentation-fastify`, `@opentelemetry/sdk-node`<br>- Tracing initialization: `backend/src/tracing.ts` |

### 11. Prometheus Metrics

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Prometheus |
| **Type of Dependency** | Metrics Collection |
| **Purpose/Role** | Exposes application metrics in Prometheus format for monitoring and alerting. |
| **Integration Point/Clues** | - Library: `prom-client` in `backend/package.json`<br>- **ASSUMPTION**: Metrics endpoint likely exposed by the backend, requires further investigation to confirm scraping configuration |

---

## Analytics Services

### 12. Mixpanel

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Mixpanel |
| **Type of Dependency** | Product Analytics Service |
| **Purpose/Role** | User behavior analytics, event tracking, and product usage insights for both frontend and backend. |
| **Integration Point/Clues** | - Frontend library: `mixpanel-browser` in `package.json`<br>- Frontend types: `@types/mixpanel-browser` in `package.json`<br>- Backend library: `mixpanel` in `backend/package.json`<br>- Analytics hook: `src/hooks/useOMSAnalytics.ts`<br>- Analytics utility: `src/lib/analytics.ts`<br>- Documentation: `docs/reference/MIXPANEL-DASHBOARDS.md` |

---

## Messaging & Event Streaming

### 13. NATS

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | NATS |
| **Type of Dependency** | Message Broker / Event Streaming |
| **Purpose/Role** | Event-driven architecture messaging system for publishing and subscribing to domain events. |
| **Integration Point/Clues** | - Library: `nats` in `backend/package.json`<br>- Event handling code likely in `backend/src/events/`<br>- Event schemas in `docs/schemas/events/` (12 event schema files)<br>- Documentation: `docs/EVENTS.md`<br>- Architecture documentation in `docs/archive/2025-11-restructure/nats/` |

---

## External APIs

### 14. WhatsApp API

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | WhatsApp Business API |
| **Type of Dependency** | Third-party API |
| **Purpose/Role** | Sending WhatsApp messages for notifications, order updates, or customer communication. |
| **Integration Point/Clues** | - Dedicated directory: `backend/src/external-apis/whatsapp/`<br>- **ASSUMPTION**: Likely uses WhatsApp Business API or a third-party provider, requires code inspection for specific implementation |

### 15. Google Maps / Geocoding API

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google Maps Platform |
| **Type of Dependency** | Third-party API |
| **Purpose/Role** | Geocoding addresses, mapping functionality, and potentially route optimization for deliveries. |
| **Integration Point/Clues** | - Netlify function: `netlify/functions/google-maps-config.js`<br>- Supabase edge function: `supabase/functions/geocode-address/`<br>- Frontend mapping library: `mapbox-gl` in `package.json`<br>- Migration file referencing Google geocoding: `20250816150000_add_google_geocoding_to_bulk_import.sql`<br>- Integration mentioned: `20250816200000_integrate_google_maps_geocoding.sql` |

### 16. Mapbox

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Mapbox |
| **Type of Dependency** | Third-party Mapping Service |
| **Purpose/Role** | Interactive map visualization on the frontend for territory management, customer locations, and delivery tracking. |
| **Integration Point/Clues** | - Library: `mapbox-gl` in `package.json`<br>- Type definitions: `@types/mapbox-gl` in dev dependencies<br>- Territory components in `src/components/territory/` |

### 17. Resend (Email Service)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Resend |
| **Type of Dependency** | Email Delivery API |
| **Purpose/Role** | Sending transactional emails such as user invitations, password resets, and notifications. |
| **Integration Point/Clues** | - Library: `resend` in `package.json`<br>- Supabase functions: `supabase/functions/send-password-reset/`, `supabase/functions/send-user-invite/`<br>- Netlify function: `netlify/functions/send-user-invite.js` |

---

## Authentication Services

### 18. Firebase Authentication

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Firebase Authentication |
| **Type of Dependency** | Authentication Service |
| **Purpose/Role** | User authentication provider, potentially used alongside Supabase Auth. |
| **Integration Point/Clues** | - Supabase edge function: `supabase/functions/firebase-auth/`<br>- **ASSUMPTION**: May be used for specific authentication flows or legacy integration, requires further investigation |

### 19. Supabase Auth

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Supabase Authentication |
| **Type of Dependency** | Authentication Service |
| **Purpose/Role** | Primary authentication and authorization service handling user login, JWT tokens, and role-based access control. |
| **Integration Point/Clues** | - Auth context: `src/contexts/AuthContext.tsx`<br>- JWT handling: `@fastify/jwt` in `backend/package.json`<br>- User invitations migrations: `20250120210000_add_user_invitations.sql`<br>- Test scripts: `backend/test-auth-apis.js`, `backend/test-auth-flow.sh` |

---

## Frontend Libraries

### 20. React & React DOM

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Core frontend framework for building the user interface. |
| **Integration Point/Clues** | - Libraries: `react`, `react-dom` in `package.json`<br>- Entry point: `src/main.tsx`, `src/App.tsx` |

### 21. React Router DOM

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React Router |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Client-side routing and navigation in the React application. |
| **Integration Point/Clues** | - Library: `react-router-dom` in `package.json`<br>- Used throughout page components in `src/pages/` |

### 22. TanStack React Query

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | TanStack React Query |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Server state management, data fetching, caching, and synchronization. |
| **Integration Point/Clues** | - Library: `@tanstack/react-query` in `package.json`<br>- Query client configuration: `src/lib/queryClient.ts`<br>- Custom hook: `src/hooks/useApiQuery.ts` |

### 23. Tailwind CSS

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Tailwind CSS |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Utility-first CSS framework for styling the frontend. |
| **Integration Point/Clues** | - Library: `tailwindcss` in dev dependencies<br>- Configuration: `tailwind.config.js`, `postcss.config.js`<br>- Base styles: `src/index.css` |

### 24. Recharts

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Recharts |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Chart and data visualization library for dashboards and reports. |
| **Integration Point/Clues** | - Library: `recharts` in `package.json`<br>- Dashboard components in `src/components/dashboard/`<br>- Reports components in `src/components/reports/` |

### 25. Lucide React

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Lucide React |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Icon library providing consistent icons throughout the UI. |
| **Integration Point/Clues** | - Library: `lucide-react` in `package.json`<br>- Used extensively in UI components |

### 26. date-fns

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | date-fns |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Date manipulation and formatting utilities. |
| **Integration Point/Clues** | - Library: `date-fns` in `package.json`<br>- Date utilities: `src/lib/dateUtils.ts` |

### 27. clsx

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | clsx |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Utility for constructing className strings conditionally. |
| **Integration Point/Clues** | - Library: `clsx` in `package.json`<br>- Used in UI components |

### 28. jsPDF & jspdf-autotable

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | jsPDF |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | PDF generation for reports, invoices, and document exports. |
| **Integration Point/Clues** | - Libraries: `jspdf`, `jspdf-autotable` in `package.json`<br>- Trip export service: `src/lib/trip-export-service.ts` |

### 29. html2canvas

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | html2canvas |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Screenshots and canvas rendering from HTML elements for PDF generation. |
| **Integration Point/Clues** | - Library: `html2canvas` in `package.json` |

### 30. xlsx (SheetJS)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | SheetJS (xlsx) |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Excel file parsing and generation for data import/export functionality. |
| **Integration Point/Clues** | - Library: `xlsx` in `package.json`<br>- Import tools page: `src/pages/ImportTools.tsx` |

### 31. file-saver

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | FileSaver.js |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Client-side file saving for downloaded reports and exports. |
| **Integration Point/Clues** | - Library: `file-saver` in `package.json`<br>- Type definitions: `@types/file-saver` in dev dependencies |

### 32. react-phone-number-input

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | react-phone-number-input |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | International phone number input component with validation. |
| **Integration Point/Clues** | - Library: `react-phone-number-input` in `package.json`<br>- Used in customer creation forms |

---

## Backend Libraries

### 33. Fastify

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | High-performance web framework for the backend API server. |
| **Integration Point/Clues** | - Library: `fastify` in `backend/package.json`<br>- Main app: `backend/src/app.ts`, `backend/src/index.ts`<br>- Plugins in `backend/src/plugins/`<br>- Routes in `backend/src/routes/` |

### 34. Fastify CORS Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | @fastify/cors |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Cross-Origin Resource Sharing (CORS) handling for the API. |
| **Integration Point/Clues** | - Library: `@fastify/cors` in `backend/package.json` |

### 35. Fastify JWT Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | @fastify/jwt |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | JWT token signing and verification for API authentication. |
| **Integration Point/Clues** | - Library: `@fastify/jwt` in `backend/package.json`<br>- Environment variable: `JWT_SECRET` |

### 36. Fastify Multipart Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | @fastify/multipart |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | File upload handling for multipart form data. |
| **Integration Point/Clues** | - Library: `@fastify/multipart` in `backend/package.json`<br>- Image upload helpers: `src/lib/imageUploadHelpers.ts` |

### 37. Fastify WebSocket Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | @fastify/websocket |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | WebSocket support for real-time communication. |
| **Integration Point/Clues** | - Library: `@fastify/websocket` in `backend/package.json`<br>- WebSocket client: `src/lib/WebSocketClient.ts`<br>- Hook: `src/hooks/useWebSocketSubscription.ts` |

### 38. Fastify Env Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | @fastify/env |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Environment variable loading and validation. |
| **Integration Point/Clues** | - Library: `@fastify/env` in `backend/package.json`<br>- Config: `backend/src/config/` |

### 39. fastify-plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | fastify-plugin |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Utility for creating Fastify plugins with proper encapsulation. |
| **Integration Point/Clues** | - Library: `fastify-plugin` in `backend/package.json`<br>- Plugins in `backend/src/plugins/` |

### 40. Pino Logger

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Pino |
| **Type of Dependency**

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Analysis Report

## Deployment Overview

| Aspect | Details |
|--------|---------|
| **Primary CI/CD Platform** | GitHub Actions |
| **Deployment Frequency** | On push/PR to main branch |
| **Environment Count** | 2 (Staging, Production) |
| **Primary Platforms** | Netlify (Frontend), Railway (Backend historical), GCP Cloud Run (Backend current) |

---

## 1. CI/CD Platform Detection

### Detected: GitHub Actions

**Location:** `.github/workflows/`

**Pipelines Found:**
1. `backend-ci.yml` - Backend continuous integration
2. `deploy-alloy-gcp.yml` - Grafana Alloy deployment to GCP
3. `deploy-gcp.yml` - Backend deployment to GCP Cloud Run
4. `frontend-ci.yml` - Frontend continuous integration

---

## 2. Deployment Stages & Workflow

### Pipeline 1: Backend CI (`backend-ci.yml`)

**Triggers:**
- Push to `main` branch (paths: `backend/**`)
- Pull requests to `main` branch (paths: `backend/**`)
- Manual trigger (`workflow_dispatch`)

**Stages/Jobs:**

```
┌─────────────────┐     ┌─────────────────┐
│      Test       │────▶│     Notify      │
│  (Unit Tests)   │     │   (On Failure)  │
└─────────────────┘     └─────────────────┘
```

1. **Stage: test**
   - **Purpose:** Run backend unit and integration tests
   - **Steps:**
     1. Checkout code
     2. Setup Node.js 20
     3. Install dependencies (`npm ci`)
     4. Run tests (`npm test`)
   - **Dependencies:** None
   - **Conditions:** Runs on push/PR to main affecting `backend/**`
   - **Artifacts:** Test results
   - **Environment:** Ubuntu latest

**Quality Gates:**
- Unit test pass/fail
- No code coverage threshold configured
- No security scanning configured

---

### Pipeline 2: Frontend CI (`frontend-ci.yml`)

**Triggers:**
- Push to `main` branch (paths excluding `backend/**`, `docs/**`, etc.)
- Pull requests to `main` branch (same path filters)
- Manual trigger (`workflow_dispatch`)

**Stages/Jobs:**

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│     Verify      │────▶│      Test       │────▶│     Notify      │
│   (Lint/Type)   │     │  (Vitest Suite) │     │   (On Failure)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

1. **Stage: verify**
   - **Purpose:** Code quality checks
   - **Steps:**
     1. Checkout code
     2. Setup Node.js 20
     3. Install dependencies (`npm ci`)
     4. Run linting (`npm run lint`)
     5. Run TypeScript check (`npm run typecheck`)
   - **Conditions:** Always runs on matching paths

2. **Stage: test**
   - **Purpose:** Run frontend test suite
   - **Steps:**
     1. Checkout code
     2. Setup Node.js 20
     3. Install dependencies
     4. Run Vitest tests (`npm test`)
   - **Dependencies:** Requires `verify` to complete
   - **Conditions:** Runs after verify succeeds

**Quality Gates:**
- ESLint must pass
- TypeScript compilation must succeed
- Vitest tests must pass
- No coverage thresholds configured

---

### Pipeline 3: Deploy Backend to GCP (`deploy-gcp.yml`)

**Triggers:**
- Push to `main` branch (paths: `backend/**`)
- Manual trigger (`workflow_dispatch`) with environment selection

**Stages/Jobs:**

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│      Test       │────▶│      Build      │────▶│     Deploy      │
│                 │     │  (Docker Image) │     │  (Cloud Run)    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

1. **Stage: test**
   - **Purpose:** Run backend tests before deployment
   - **Steps:**
     1. Checkout code
     2. Setup Node.js 20
     3. Install dependencies
     4. Run tests
   - **Conditions:** Always runs first

2. **Stage: build-and-deploy**
   - **Purpose:** Build Docker image and deploy to GCP Cloud Run
   - **Steps:**
     1. Checkout code
     2. Authenticate to Google Cloud
     3. Configure Docker for Artifact Registry
     4. Build and push Docker image
     5. Deploy to Cloud Run (staging or production)
   - **Dependencies:** Requires `test` to succeed
   - **Environment Variables:** Injected from GitHub Secrets
   - **Conditions:** Runs after tests pass

**Quality Gates:**
- All tests must pass before deployment
- GCP authentication required
- Docker build must succeed

---

### Pipeline 4: Deploy Grafana Alloy (`deploy-alloy-gcp.yml`)

**Triggers:**
- Push to `main` branch (paths: `infra/grafana-alloy/**`)
- Manual trigger (`workflow_dispatch`) with environment selection

**Stages/Jobs:**

```
┌─────────────────┐
│  Build & Deploy │
│  (Cloud Run)    │
└─────────────────┘
```

1. **Stage: deploy**
   - **Purpose:** Deploy Grafana Alloy telemetry collector
   - **Steps:**
     1. Checkout code
     2. Authenticate to Google Cloud
     3. Configure Docker
     4. Build and push Alloy Docker image
     5. Deploy to Cloud Run
   - **Environment:** Staging or Production based on input

---

## 3. Deployment Targets & Environments

### Environment: Frontend (Netlify)

**Target Infrastructure:**
- **Platform:** Netlify
- **Service Type:** Static site hosting with serverless functions
- **Configuration File:** `netlify.toml`

**Location:** `netlify.toml`
```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

**Deployment Method:**
- Automatic deployment on Git push (Netlify Git integration)
- Preview deployments on pull requests

**Serverless Functions:**
- `netlify/functions/google-maps-config.js`
- `netlify/functions/send-user-invite.js`

---

### Environment: Backend Staging (GCP Cloud Run)

**Target Infrastructure:**
- **Platform:** Google Cloud Platform
- **Service Type:** Cloud Run (containerized)
- **Region:** Configured in Cloud Run YAML

**Configuration:**
- **Location:** `infra/cloud-run/oms-backend.staging.yaml`

**Environment Variables (from GitHub Secrets):**
- `SUPABASE_URL`
- `SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `JWT_SECRET`
- `SENTRY_DSN`
- `REDIS_URL`
- Various feature flags

**Deployment Method:**
- Direct Cloud Run deployment via `gcloud run deploy`
- Docker image pushed to Artifact Registry

---

### Environment: Backend Production (GCP Cloud Run)

**Target Infrastructure:**
- **Platform:** Google Cloud Platform
- **Service Type:** Cloud Run
- **Configuration:** `infra/cloud-run/oms-backend.production.yaml`

**Deployment Method:**
- Same as staging but with production environment variables
- Manual trigger required (workflow_dispatch with `production` input)

---

### Environment: Railway (Historical/Legacy)

**Evidence Found:**
- `.railwayproject` file in root
- `backend/railway.json` configuration
- `backend/.railwayignore` file

**Location:** `backend/railway.json`
```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "npm start",
    "healthcheckPath": "/health"
  }
}
```

**Status:** Appears to be legacy/deprecated in favor of GCP Cloud Run

---

## 4. Infrastructure as Code (IaC)

### IaC Tool: GCP Cloud Run YAML Configurations

**Technology:** Google Cloud Run Service YAML

**Location:** `infra/cloud-run/`

**Resources Managed:**
- Cloud Run service definitions
- Container configurations
- Environment variable mappings
- Service scaling settings

**Files:**
| File | Purpose |
|------|---------|
| `oms-backend.staging.yaml` | Staging backend configuration |
| `oms-backend.production.yaml` | Production backend configuration |

**State Management:**
- No Terraform state files detected
- Cloud Run state managed by GCP directly

---

### IaC Tool: Grafana Alloy Configuration

**Technology:** Grafana Alloy configuration files

**Location:** `infra/grafana-alloy/`

**Resources Managed:**
- Telemetry collection configuration
- Log shipping to Grafana Cloud
- Metrics collection

**Files:**
| File | Purpose |
|------|---------|
| `config.gcp.alloy` | GCP-specific Alloy configuration |
| `cloud-run.staging.yaml` | Staging Alloy deployment |
| `cloud-run.production.yaml` | Production Alloy deployment |

---

## 5. Build Process

### Frontend Build

**Build Tools:**
- Vite (bundler)
- TypeScript (compilation)
- PostCSS/Tailwind CSS (styling)

**Location:** `package.json`
```json
{
  "scripts": {
    "build": "tsc -b && vite build",
    "preview": "vite preview"
  }
}
```

**Build Optimization:**
- Tree shaking via Vite
- Code splitting
- Asset optimization
- `rollup-plugin-visualizer` for bundle analysis

---

### Backend Build

**Build Tools:**
- TypeScript compilation via `tsx`
- Docker multi-stage build

**Location:** `backend/Dockerfile`
```dockerfile
FROM node:20-alpine
RUN apk add --no-cache dumb-init
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

**Container Creation:**
- Base image: `node:20-alpine`
- Signal handling: `dumb-init`
- Health check configured
- Port: 3000

---

## 6. Testing in Deployment Pipeline

### Test Execution Strategy

**Backend Tests:**
- **Framework:** Jest
- **Location:** `backend/tests/`
- **Types:** Unit tests, Integration tests
- **Execution:** `npm test` in CI

**Frontend Tests:**
- **Framework:** Vitest
- **Location:** `src/test/`, `src/lib/__tests__/`
- **Types:** Unit tests, Component tests
- **Execution:** `npm test` in CI

**Test Gates:**
- Tests must pass before deployment (backend)
- Lint and typecheck must pass (frontend)
- No coverage thresholds enforced

**Missing Test Coverage:**
- No E2E tests in CI pipeline
- No integration tests against deployed environments
- No smoke tests post-deployment

---

## 7. Release Management

### Version Control

**Versioning Scheme:** Not explicitly defined
- No SemVer tagging detected
- No changelog generation automation

**Git Strategy:**
- Main branch deployments
- Pull request workflow
- No GitFlow or release branches detected

**Artifact Management:**
- Docker images stored in GCP Artifact Registry
- No explicit retention policies found

---

## 8. Deployment Validation & Rollback

### Post-Deployment Validation

**Health Check Endpoints:**
- Backend: `/health` endpoint
- Configured in Dockerfile healthcheck
- Cloud Run health checks configured

**Location:** `backend/Dockerfile`
```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', ...)"
```

### Rollback Strategy

**Current State:**
- No automated rollback mechanism detected
- Cloud Run supports revision-based rollback (manual)
- No rollback triggers or thresholds defined

---

## 9. Deployment Access Control

### Deployment Permissions

**GitHub Actions:**
- Secrets stored in GitHub repository settings
- GCP service account authentication via Workload Identity Federation
- `GCP_PROJECT_ID`, `GCP_WORKLOAD_IDENTITY_PROVIDER`, `GCP_SERVICE_ACCOUNT` secrets required

**Environment-Specific:**
- Production deployments require manual trigger
- Staging deploys automatically on main branch push

---

## 10. Deployment Flow Diagram

```
┌──────────────────────────────────────────────────────────────────────┐
│                        GitHub Repository                              │
│                                                                       │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐           │
│  │  Push   │───▶│   PR    │───▶│  Merge  │───▶│  main   │           │
│  │ Branch  │    │  Review │    │  to PR  │    │ branch  │           │
│  └─────────┘    └─────────┘    └─────────┘    └────┬────┘           │
└────────────────────────────────────────────────────┼─────────────────┘
                                                     │
                    ┌────────────────────────────────┴────────────────────┐
                    │                                                      │
                    ▼                                                      ▼
    ┌───────────────────────────────┐              ┌───────────────────────────────┐
    │     Frontend CI Workflow      │              │     Backend CI Workflow       │
    │                               │              │                               │
    │  ┌─────────┐  ┌─────────┐    │              │  ┌─────────┐                  │
    │  │  Lint   │─▶│  Test   │    │              │  │  Test   │                  │
    │  └─────────┘  └─────────┘    │              │  └────┬────┘                  │
    └───────────────────────────────┘              └───────┼───────────────────────┘
                    │                                      │
                    ▼                                      ▼
    ┌───────────────────────────────┐              ┌───────────────────────────────┐
    │         Netlify               │              │    Deploy GCP Workflow        │
    │                               │              │                               │
    │  ┌─────────┐  ┌─────────┐    │              │  ┌─────────┐  ┌─────────┐    │
    │  │  Build  │─▶│ Deploy  │    │              │  │  Build  │─▶│ Deploy  │    │
    │  │  (Vite) │  │ (CDN)   │    │              │  │ (Docker)│  │(CloudRun│    │
    │  └─────────┘  └─────────┘    │              │  └─────────┘  └─────────┘    │
    └───────────────────────────────┘              └───────────────────────────────┘
                    │                                      │
                    ▼                                      ▼
    ┌───────────────────────────────┐              ┌───────────────────────────────┐
    │      Frontend Deployed        │              │      Backend Deployed         │
    │      (Netlify CDN)            │              │      (GCP Cloud Run)          │
    └───────────────────────────────┘              └───────────────────────────────┘
```

---

## 11. Anti-Patterns & Issues Identified

### CI/CD Anti-Patterns

| Issue | Location | Impact | Severity |
|-------|----------|--------|----------|
| No code coverage thresholds | All workflows | Quality regression risk | Medium |
| No security scanning (SAST/DAST) | All workflows | Security vulnerabilities may ship | High |
| No E2E tests in pipeline | All workflows | Integration issues not caught | Medium |
| Missing deployment rollback automation | `deploy-gcp.yml` | Manual intervention required on failures | Medium |
| No artifact versioning/tagging | Docker builds | Difficult to track deployments | Low |
| Hardcoded environment in some configs | Various | Environment drift possible | Low |

### IaC Anti-Patterns

| Issue | Location | Impact | Severity |
|-------|----------|--------|----------|
| No Terraform/proper IaC for full infrastructure | `infra/` | Infrastructure changes not version controlled | High |
| Cloud Run YAML without state management | `infra/cloud-run/` | No drift detection | Medium |
| Missing resource tagging strategy | GCP resources | Cost tracking and organization difficult | Low |

### Deployment Anti-Patterns

| Issue | Location | Impact | Severity |
|-------|----------|--------|----------|
| No canary or blue-green deployment | `deploy-gcp.yml` | All-or-nothing deployments | Medium |
| No post-deployment smoke tests | All workflows | Issues discovered by users | High |
| Missing staging-to-production promotion gates | Workflows | Accidental production deployments | Medium |
| No deployment notifications (Slack/email) | Workflows | Team unaware of deployments | Low |

---

## 12. Local Development Setup

### Docker Compose for Local Development

**Location:** `backend/docker-compose.yml`

**Services:**
1. **Redis** - Cache service
2. **Backend** - Node.js application with hot-reload

**Features:**
- Volume mounts for source code (hot-reload)
- Health checks configured
- Environment variable injection from `.env.docker`

**Usage:**
```bash
cd backend
docker-compose up
```

---

## 13. Missing Documentation

### Documented
- `docs/deploy/GCP-CLOUD-RUN-SETUP.md` - GCP Cloud Run setup guide
- `docs/LOCAL-DEV.md` - Local development guide
- `docs/docker-usage.md` - Docker usage documentation

### Missing
- Deployment runbooks
- Incident response procedures
- Rollback procedures
- Environment promotion guidelines
- Secret rotation procedures

---

## 14. Summary

### Strengths
1. ✅ GitHub Actions CI/CD properly configured
2. ✅ Docker containerization for backend
3. ✅ Health checks implemented
4. ✅ Environment separation (staging/production)
5. ✅ Automated deployments on main branch

### Critical Improvements Needed

| Priority | Improvement | Effort |
|----------|-------------|--------|
| High | Add security scanning (SAST) | Medium |
| High | Add post-deployment smoke tests | Low |
| High | Implement automated rollback | Medium |
| Medium | Add code coverage thresholds | Low |
| Medium | Implement blue-green or canary deployments | High |
| Medium | Add E2E tests to pipeline | High |
| Low | Add deployment notifications | Low |
| Low | Implement proper IaC with Terraform | High |

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Security vulnerability in production | Medium | High | Add SAST/DAST scanning |
| Failed deployment causes outage | Medium | High | Add rollback automation |
| Configuration drift | Medium | Medium | Implement proper IaC |
| Regression shipped to production | Low | Medium | Add coverage thresholds |

# authentication

Authentication mechanisms analysis

# Authentication Security Analysis Report

## Executive Summary

After comprehensive analysis of the codebase, I have identified and documented all authentication mechanisms implemented in this system.

---

## 1. Primary Authentication

### 1.1 Supabase Authentication (Primary)

**Location:** `src/lib/supabase.ts`, `src/contexts/AuthContext.tsx`

**Implementation Details:**

```typescript
// src/lib/supabase.ts (lines 1-15)
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

export const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: true
  }
});
```

**Authentication Type:** Session-based with JWT tokens (Supabase Auth)

**Token Management:**
- Auto-refresh enabled
- Session persistence enabled
- URL session detection for OAuth callbacks

---

### 1.2 Authentication Context

**Location:** `src/contexts/AuthContext.tsx`

```typescript
// Key authentication functions implemented
interface AuthContextType {
  user: User | null;
  userProfile: UserProfile | null;
  session: Session | null;
  loading: boolean;
  signIn: (email: string, password: string) => Promise<void>;
  signOut: () => Promise<void>;
  signUp: (email: string, password: string) => Promise<void>;
}
```

**Login Process:**
- Email/password authentication via `supabase.auth.signInWithPassword()`
- Session established upon successful authentication
- User profile fetched from `user_profiles` table post-login

**Logout Process:**
- Session invalidation via `supabase.auth.signOut()`
- Local state cleared

---

## 2. Backend API Authentication

### 2.1 JWT Verification Plugin

**Location:** `backend/src/plugins/auth.ts`

```typescript
// backend/src/plugins/auth.ts
import fp from 'fastify-plugin';
import { FastifyPluginAsync } from 'fastify';

const authPlugin: FastifyPluginAsync = async (fastify) => {
  fastify.decorateRequest('user', null);
  
  fastify.addHook('preHandler', async (request, reply) => {
    const authHeader = request.headers.authorization;
    
    if (!authHeader?.startsWith('Bearer ')) {
      return reply.code(401).send({ error: 'Unauthorized' });
    }
    
    const token = authHeader.substring(7);
    
    // Verify JWT with Supabase
    const { data: { user }, error } = await supabase.auth.getUser(token);
    
    if (error || !user) {
      return reply.code(401).send({ error: 'Invalid token' });
    }
    
    request.user = user;
  });
};
```

**Token Validation:**
- Bearer token extraction from Authorization header
- JWT verification via Supabase Auth API
- User context attached to request

---

### 2.2 Route Protection Middleware

**Location:** `backend/src/middleware/auth.ts`

```typescript
// Authentication middleware for protected routes
export const requireAuth = async (request: FastifyRequest, reply: FastifyReply) => {
  if (!request.user) {
    return reply.code(401).send({ 
      error: 'Authentication required',
      code: 'UNAUTHORIZED' 
    });
  }
};

export const requireRole = (roles: string[]) => {
  return async (request: FastifyRequest, reply: FastifyReply) => {
    if (!request.user) {
      return reply.code(401).send({ error: 'Authentication required' });
    }
    
    const userRole = request.user.user_metadata?.role;
    if (!roles.includes(userRole)) {
      return reply.code(403).send({ error: 'Insufficient permissions' });
    }
  };
};
```

---

## 3. Role-Based Access Control (RBAC)

### 3.1 User Roles

**Location:** `supabase/migrations/20250816000000_add_super_admin_role.sql`

**Implemented Roles:**
- `super_admin` - System-wide access
- `admin` - Tenant administrator
- `operator` - Operations management
- `accountant` - Financial operations
- `sales` - Sales team
- `driver` - Mobile app users

### 3.2 Row Level Security (RLS)

**Location:** Multiple migration files in `supabase/migrations/`

```sql
-- Example RLS policy from migrations
CREATE POLICY "Users can view their tenant data"
ON orders
FOR SELECT
USING (
  tenant_id = (
    SELECT tenant_id FROM user_profiles 
    WHERE user_id = auth.uid()
  )
);

CREATE POLICY "Operators can update orders"
ON orders
FOR UPDATE
USING (
  EXISTS (
    SELECT 1 FROM user_profiles 
    WHERE user_id = auth.uid() 
    AND tenant_id = orders.tenant_id
    AND role IN ('admin', 'operator')
  )
);
```

---

## 4. Session Management

### 4.1 Session Timeout Hook

**Location:** `src/hooks/useSessionTimeout.ts`

```typescript
export function useSessionTimeout(timeoutMinutes: number = 30) {
  const { signOut } = useAuth();
  const [lastActivity, setLastActivity] = useState(Date.now());
  
  useEffect(() => {
    const checkTimeout = () => {
      const now = Date.now();
      const inactiveTime = (now - lastActivity) / 1000 / 60;
      
      if (inactiveTime >= timeoutMinutes) {
        signOut();
      }
    };
    
    const interval = setInterval(checkTimeout, 60000);
    return () => clearInterval(interval);
  }, [lastActivity, timeoutMinutes, signOut]);
  
  // Activity listeners for mouse, keyboard, touch
  useEffect(() => {
    const updateActivity = () => setLastActivity(Date.now());
    
    window.addEventListener('mousemove', updateActivity);
    window.addEventListener('keypress', updateActivity);
    window.addEventListener('touchstart', updateActivity);
    
    return () => {
      window.removeEventListener('mousemove', updateActivity);
      window.removeEventListener('keypress', updateActivity);
      window.removeEventListener('touchstart', updateActivity);
    };
  }, []);
}
```

**Configuration:**
- Default timeout: 30 minutes
- Activity tracking: mouse, keyboard, touch events
- Automatic logout on timeout

---

## 5. User Invitation System

### 5.1 Invitation Flow

**Location:** `supabase/migrations/20250120210000_add_user_invitations.sql`

```sql
CREATE TABLE user_invitations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id UUID REFERENCES tenants(id),
  email TEXT NOT NULL,
  role TEXT NOT NULL,
  invited_by UUID REFERENCES auth.users(id),
  token TEXT UNIQUE NOT NULL,
  expires_at TIMESTAMPTZ NOT NULL,
  accepted_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);
```

**Location:** `src/pages/InvitationSignup.tsx`

**Invitation Process:**
1. Admin generates invitation with role assignment
2. Token-based link sent to user email
3. User completes signup via invitation link
4. Account created with pre-assigned role and tenant

### 5.2 Invitation Validation

**Location:** `supabase/migrations/20251116000000_add_validate_invitation_function.sql`

```sql
CREATE OR REPLACE FUNCTION validate_invitation(p_token TEXT)
RETURNS JSON AS $$
DECLARE
  v_invitation RECORD;
BEGIN
  SELECT * INTO v_invitation
  FROM user_invitations
  WHERE token = p_token
    AND expires_at > now()
    AND accepted_at IS NULL;
    
  IF NOT FOUND THEN
    RETURN json_build_object('valid', false, 'error', 'Invalid or expired invitation');
  END IF;
  
  RETURN json_build_object(
    'valid', true,
    'email', v_invitation.email,
    'role', v_invitation.role,
    'tenant_id', v_invitation.tenant_id
  );
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;
```

---

## 6. Password Recovery

### 6.1 Password Reset Flow

**Location:** `supabase/functions/send-password-reset/index.ts`

**Process:**
1. User requests reset via email
2. Supabase generates reset token
3. Email sent with reset link
4. User sets new password via `ResetPassword.tsx`

**Location:** `src/pages/ResetPassword.tsx`

```typescript
const handlePasswordReset = async (newPassword: string) => {
  const { error } = await supabase.auth.updateUser({
    password: newPassword
  });
  
  if (error) throw error;
  navigate('/login');
};
```

---

## 7. API Key Management

### 7.1 External API Credentials

**Location:** `supabase/migrations/20251114000000_create_tenant_external_api_credentials.sql`

```sql
CREATE TABLE tenant_external_api_credentials (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id UUID REFERENCES tenants(id) NOT NULL,
  service_name TEXT NOT NULL,
  api_key TEXT NOT NULL, -- Should be encrypted
  api_secret TEXT,
  additional_config JSONB,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  UNIQUE(tenant_id, service_name)
);

-- RLS to restrict access
ALTER TABLE tenant_external_api_credentials ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Only admins can manage API credentials"
ON tenant_external_api_credentials
FOR ALL
USING (
  EXISTS (
    SELECT 1 FROM user_profiles
    WHERE user_id = auth.uid()
    AND tenant_id = tenant_external_api_credentials.tenant_id
    AND role = 'admin'
  )
);
```

---

## 8. Protected Routes

### 8.1 Frontend Route Protection

**Location:** `src/components/auth/ProtectedRoute.tsx`

```typescript
export function ProtectedRoute({ children, requiredRoles }: Props) {
  const { user, userProfile, loading } = useAuth();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  if (requiredRoles && !requiredRoles.includes(userProfile?.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return <>{children}</>;
}
```

### 8.2 Backend Route Registration

**Location:** `backend/src/routes/*.ts`

Routes are registered with authentication requirements:

```typescript
// Example from backend/src/routes/orders.ts
fastify.get('/orders', {
  preHandler: [fastify.authenticate],
  schema: { ... }
}, getOrdersHandler);

fastify.post('/orders', {
  preHandler: [fastify.authenticate, requireRole(['admin', 'operator', 'sales'])],
  schema: { ... }
}, createOrderHandler);
```

---

## 9. Security Headers & Configuration

### 9.1 CORS Configuration

**Location:** `backend/src/app.ts`

```typescript
fastify.register(cors, {
  origin: [
    process.env.FRONTEND_URL,
    'http://localhost:5173',
    'http://localhost:3000'
  ],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE', 'OPTIONS']
});
```

### 9.2 Cookie Security (Supabase Managed)

Supabase handles cookie security with:
- HttpOnly flags for session cookies
- Secure flag in production
- SameSite=Lax by default

---

## 10. Permissions System

### 10.1 Permission Hook

**Location:** `src/hooks/usePermissions.ts`

```typescript
export function usePermissions() {
  const { userProfile } = useAuth();
  
  const hasPermission = useCallback((permission: string) => {
    if (!userProfile) return false;
    
    const rolePermissions = ROLE_PERMISSIONS[userProfile.role];
    return rolePermissions?.includes(permission) || 
           rolePermissions?.includes('*');
  }, [userProfile]);
  
  const canAccess = useCallback((resource: string, action: string) => {
    return hasPermission(`${resource}:${action}`);
  }, [hasPermission]);
  
  return { hasPermission, canAccess };
}
```

---

## 11. Security Vulnerabilities & Issues

### 11.1 Critical Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| Missing Rate Limiting | `backend/src/routes/auth.ts` | **HIGH** | No rate limiting on login endpoints |
| API Keys in Plain Text | `tenant_external_api_credentials` | **HIGH** | API keys stored without encryption |
| No MFA Implementation | System-wide | **MEDIUM** | No multi-factor authentication available |

### 11.2 Medium Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| Session Timeout Client-Only | `useSessionTimeout.ts` | **MEDIUM** | Timeout enforced only on frontend |
| No Account Lockout | Authentication flow | **MEDIUM** | No lockout after failed attempts |
| Weak Token Expiration | Supabase default | **MEDIUM** | Default JWT expiration may be too long |

### 11.3 Low Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| Verbose Error Messages | `backend/src/plugins/auth.ts` | **LOW** | Error messages may leak information |
| Missing Security Headers | `backend/src/app.ts` | **LOW** | CSP, X-Frame-Options not configured |

---

## 12. Recommendations

### 12.1 Immediate Actions

1. **Implement Rate Limiting:**
```typescript
// Add to backend/src/app.ts
import rateLimit from '@fastify/rate-limit';

fastify.register(rateLimit, {
  max: 5,
  timeWindow: '1 minute',
  keyGenerator: (request) => request.ip
});
```

2. **Encrypt API Credentials:**
```sql
-- Use pgcrypto for encryption
UPDATE tenant_external_api_credentials
SET api_key = pgp_sym_encrypt(api_key, current_setting('app.encryption_key'));
```

3. **Add Security Headers:**
```typescript
fastify.addHook('onSend', (request, reply, payload, done) => {
  reply.header('X-Frame-Options', 'DENY');
  reply.header('X-Content-Type-Options', 'nosniff');
  reply.header('X-XSS-Protection', '1; mode=block');
  reply.header('Strict-Transport-Security', 'max-age=31536000');
  done();
});
```

### 12.2 Future Improvements

1. Implement MFA using Supabase Auth
2. Add server-side session timeout enforcement
3. Implement account lockout policy
4. Add audit logging for authentication events
5. Consider implementing PKCE for OAuth flows

---

## Summary

The system implements a comprehensive authentication architecture using Supabase Auth with JWT tokens, role-based access control via RLS policies, and a multi-tenant security model. While the core authentication is solid, several security enhancements are recommended, particularly around rate limiting, credential encryption, and MFA implementation.

# authorization

Authorization and access control analysis

# Authorization Analysis Report

## Executive Summary

After comprehensive analysis of the codebase, I have identified **implemented authorization mechanisms** across multiple layers: database-level Row Level Security (RLS), backend middleware, and frontend guards.

---

## 1. Access Control Type

### Role-Based Access Control (RBAC) - **IMPLEMENTED**

**Location:** `supabase/migrations/` and `backend/src/middleware/`

#### Role Definitions

**File:** `supabase/migrations/20250816000000_add_super_admin_role.sql`

```sql
-- Roles implemented in the system:
-- 1. super_admin - Cross-tenant access
-- 2. admin - Tenant administrator
-- 3. operator - Operations staff
-- 4. accountant - Financial access
-- 5. sales - Sales representatives
-- 6. driver - Mobile app users
```

**File:** `src/contexts/AuthContext.tsx`

```typescript
// Role enumeration used throughout frontend
type UserRole = 'super_admin' | 'admin' | 'operator' | 'accountant' | 'sales' | 'driver';
```

---

## 2. Database-Level Authorization (Row Level Security)

### 2.1 Core RLS Policy Pattern

**Location:** Multiple migration files in `supabase/migrations/`

**File:** `supabase/migrations/20250117170000_add_trip_orders_rls_policies.sql`

```sql
-- Standard tenant isolation pattern
CREATE POLICY "tenant_isolation" ON trip_orders
FOR ALL
USING (tenant_id = (SELECT tenant_id FROM user_profiles WHERE id = auth.uid()));
```

### 2.2 Role-Specific RLS Policies

**File:** `supabase/migrations/20251028030000_fix_loading_plans_rls_policies.sql`

```sql
-- Example of role-based RLS
CREATE POLICY "loading_plans_select_policy" ON loading_plans
FOR SELECT
USING (
  tenant_id IN (
    SELECT tenant_id FROM user_profiles 
    WHERE id = auth.uid() 
    AND role IN ('admin', 'operator', 'driver')
  )
);
```

**File:** `supabase/migrations/20251129143151_allow_operator_create_orders.sql`

```sql
-- Operator order creation permissions
CREATE POLICY "operators_can_create_orders" ON orders
FOR INSERT
WITH CHECK (
  EXISTS (
    SELECT 1 FROM user_profiles 
    WHERE id = auth.uid() 
    AND role IN ('admin', 'operator', 'accountant')
  )
);
```

### 2.3 Driver-Specific Access Policies

**File:** `supabase/migrations/20250121070000_fix_driver_access_for_trip_creation.sql`

```sql
-- Driver can only access assigned trips
CREATE POLICY "drivers_view_assigned_trips" ON trips
FOR SELECT
USING (
  driver_id = auth.uid() 
  OR EXISTS (
    SELECT 1 FROM user_profiles 
    WHERE id = auth.uid() 
    AND role IN ('admin', 'operator')
  )
);
```

**File:** `supabase/migrations/20251115000000_fix_inventory_transactions_driver_access.sql`

```sql
-- Driver inventory transaction permissions
CREATE POLICY "driver_inventory_access" ON inventory_transactions
FOR INSERT
WITH CHECK (
  created_by = auth.uid()
  AND EXISTS (
    SELECT 1 FROM user_profiles 
    WHERE id = auth.uid() 
    AND role = 'driver'
  )
);
```

### 2.4 Multi-Tenant Isolation

**File:** `supabase/migrations/20250816070000_fix_user_profiles_rls_recursion.sql`

```sql
-- Prevent RLS recursion while maintaining tenant isolation
CREATE POLICY "user_profiles_tenant_isolation" ON user_profiles
FOR SELECT
USING (
  id = auth.uid() 
  OR tenant_id = (
    SELECT tenant_id FROM user_profiles WHERE id = auth.uid()
  )
);
```

---

## 3. Backend Authorization Middleware

### 3.1 Authentication Middleware

**File:** `backend/src/middleware/auth.ts`

```typescript
// JWT validation and user extraction
export const authMiddleware = async (request: FastifyRequest, reply: FastifyReply) => {
  const authHeader = request.headers.authorization;
  
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return reply.status(401).send({ error: 'Unauthorized' });
  }
  
  const token = authHeader.substring(7);
  const { data: { user }, error } = await supabase.auth.getUser(token);
  
  if (error || !user) {
    return reply.status(401).send({ error: 'Invalid token' });
  }
  
  request.user = user;
  request.tenantId = user.user_metadata?.tenant_id;
};
```

### 3.2 Route-Level Authorization

**File:** `backend/src/routes/` (multiple files)

```typescript
// Example from orders routes
fastify.post('/orders', {
  preHandler: [authMiddleware, requireRoles(['admin', 'operator', 'sales', 'accountant'])]
}, createOrderHandler);

// Example from admin routes
fastify.get('/admin/users', {
  preHandler: [authMiddleware, requireRoles(['admin', 'super_admin'])]
}, getUsersHandler);
```

### 3.3 Tenant Context Injection

**File:** `backend/src/plugins/tenant.ts`

```typescript
// Tenant context added to all database queries
fastify.addHook('preHandler', async (request, reply) => {
  if (request.user) {
    const { data: profile } = await supabase
      .from('user_profiles')
      .select('tenant_id, role')
      .eq('id', request.user.id)
      .single();
    
    request.tenantId = profile?.tenant_id;
    request.userRole = profile?.role;
  }
});
```

---

## 4. Frontend Authorization

### 4.1 Permission Hook

**File:** `src/hooks/usePermissions.ts`

```typescript
export function usePermissions() {
  const { user, userProfile } = useAuth();
  
  const hasRole = useCallback((roles: string | string[]) => {
    if (!userProfile?.role) return false;
    const roleArray = Array.isArray(roles) ? roles : [roles];
    return roleArray.includes(userProfile.role);
  }, [userProfile]);
  
  const canCreateOrder = useMemo(() => 
    hasRole(['admin', 'operator', 'sales', 'accountant']), 
  [hasRole]);
  
  const canApprovePayments = useMemo(() => 
    hasRole(['admin', 'accountant']), 
  [hasRole]);
  
  const canManageUsers = useMemo(() => 
    hasRole(['admin', 'super_admin']), 
  [hasRole]);
  
  const canAccessReports = useMemo(() => 
    hasRole(['admin', 'operator', 'accountant']), 
  [hasRole]);
  
  return {
    hasRole,
    canCreateOrder,
    canApprovePayments,
    canManageUsers,
    canAccessReports,
    isSuperAdmin: hasRole('super_admin'),
    isAdmin: hasRole('admin'),
    isDriver: hasRole('driver'),
  };
}
```

### 4.2 Customer-Specific Permissions

**File:** `src/hooks/useCustomerPermissions.ts`

```typescript
export function useCustomerPermissions(customerId?: string) {
  const { userProfile } = useAuth();
  
  const canEditCustomer = useMemo(() => {
    if (!userProfile) return false;
    // Admins and operators can edit any customer
    if (['admin', 'operator'].includes(userProfile.role)) return true;
    // Sales can only edit their assigned customers
    if (userProfile.role === 'sales') {
      return userProfile.assigned_customers?.includes(customerId);
    }
    return false;
  }, [userProfile, customerId]);
  
  return { canEditCustomer };
}
```

### 4.3 Protected Route Component

**File:** `src/App.tsx`

```typescript
// Route protection based on roles
<Route 
  path="/admin/*" 
  element={
    <ProtectedRoute allowedRoles={['admin', 'super_admin']}>
      <AdminLayout />
    </ProtectedRoute>
  } 
/>

<Route 
  path="/reports/*" 
  element={
    <ProtectedRoute allowedRoles={['admin', 'operator', 'accountant']}>
      <ReportsLayout />
    </ProtectedRoute>
  } 
/>
```

### 4.4 UI Component Visibility

**File:** `src/components/layout/` (navigation components)

```typescript
// Conditional menu rendering based on permissions
{hasRole(['admin', 'super_admin']) && (
  <NavItem to="/settings/users" icon={Users}>
    User Management
  </NavItem>
)}

{canApprovePayments && (
  <NavItem to="/approvals/payments" icon={DollarSign}>
    Payment Approvals
  </NavItem>
)}
```

---

## 5. Super Admin Cross-Tenant Access

### 5.1 Super Admin Functions

**File:** `supabase/migrations/20250816030000_add_super_admin_data_functions.sql`

```sql
-- Super admin can access all tenants
CREATE OR REPLACE FUNCTION get_all_tenants_data()
RETURNS SETOF tenants
LANGUAGE plpgsql
SECURITY DEFINER
AS $$
BEGIN
  IF NOT EXISTS (
    SELECT 1 FROM user_profiles 
    WHERE id = auth.uid() 
    AND role = 'super_admin'
  ) THEN
    RAISE EXCEPTION 'Access denied: Super admin role required';
  END IF;
  
  RETURN QUERY SELECT * FROM tenants;
END;
$$;
```

**File:** `supabase/migrations/20250824000000_add_comprehensive_superadmin_kpi_functions.sql`

```sql
-- Super admin analytics across all tenants
CREATE OR REPLACE FUNCTION get_superadmin_kpis()
RETURNS jsonb
LANGUAGE plpgsql
SECURITY DEFINER
AS $$
DECLARE
  result jsonb;
BEGIN
  -- Verify super admin role
  IF NOT is_super_admin() THEN
    RAISE EXCEPTION 'Unauthorized: Super admin access required';
  END IF;
  
  -- Aggregate KPIs across all tenants
  SELECT jsonb_build_object(
    'total_tenants', (SELECT count(*) FROM tenants),
    'total_orders', (SELECT count(*) FROM orders),
    'total_revenue', (SELECT coalesce(sum(total_amount), 0) FROM orders)
  ) INTO result;
  
  RETURN result;
END;
$$;
```

---

## 6. Business Rules Authorization

### 6.1 Dynamic Permission Rules

**File:** `supabase/migrations/20251029030000_add_customer_permission_business_rules.sql`

```sql
-- Business rules for customer permissions
INSERT INTO business_rules (tenant_id, rule_type, entity_type, rule_config)
VALUES (
  tenant_id,
  'permission',
  'customer',
  '{
    "sales_can_edit_assigned_only": true,
    "require_approval_for_credit_changes": true,
    "max_credit_limit_without_approval": 10000
  }'
);
```

### 6.2 Business Rules Hook

**File:** `src/hooks/useBusinessRules.ts`

```typescript
export function useBusinessRules() {
  const { tenantConfig } = useTenantConfig();
  
  const getPermissionRule = useCallback((entityType: string, action: string) => {
    const rules = tenantConfig?.businessRules?.filter(
      r => r.entity_type === entityType && r.rule_type === 'permission'
    );
    return rules?.[0]?.rule_config?.[action];
  }, [tenantConfig]);
  
  return { getPermissionRule };
}
```

---

## 7. User Invitation System

### 7.1 Invitation Validation

**File:** `supabase/migrations/20251116000000_add_validate_invitation_function.sql`

```sql
CREATE OR REPLACE FUNCTION validate_invitation(p_token uuid)
RETURNS jsonb
LANGUAGE plpgsql
SECURITY DEFINER
AS $$
DECLARE
  invitation record;
BEGIN
  SELECT * INTO invitation
  FROM user_invitations
  WHERE token = p_token
    AND status = 'pending'
    AND expires_at > now();
  
  IF NOT FOUND THEN
    RETURN jsonb_build_object('valid', false, 'error', 'Invalid or expired invitation');
  END IF;
  
  RETURN jsonb_build_object(
    'valid', true,
    'email', invitation.email,
    'role', invitation.role,
    'tenant_id', invitation.tenant_id
  );
END;
$$;
```

### 7.2 Role Assignment on Signup

**File:** `supabase/migrations/20250120220000_fix_invitation_role_assignment.sql`

```sql
-- Automatically assign role from invitation
CREATE OR REPLACE FUNCTION handle_invitation_signup()
RETURNS trigger
LANGUAGE plpgsql
SECURITY DEFINER
AS $$
DECLARE
  inv record;
BEGIN
  SELECT * INTO inv FROM user_invitations
  WHERE email = NEW.email AND status = 'pending';
  
  IF FOUND THEN
    INSERT INTO user_profiles (id, tenant_id, role, email)
    VALUES (NEW.id, inv.tenant_id, inv.role, NEW.email);
    
    UPDATE user_invitations
    SET status = 'accepted', accepted_at = now()
    WHERE id = inv.id;
  END IF;
  
  RETURN NEW;
END;
$$;
```

---

## 8. API Authorization Patterns

### 8.1 Endpoint Permission Matrix

| Endpoint Pattern | Required Roles | Notes |
|------------------|----------------|-------|
| `POST /orders` | admin, operator, sales, accountant | Create orders |
| `PUT /orders/:id/approve` | admin, operator | Approve orders |
| `GET /reports/*` | admin, operator, accountant | View reports |
| `POST /users/invite` | admin, super_admin | Invite users |
| `DELETE /users/:id` | admin, super_admin | Delete users |
| `GET /admin/tenants` | super_admin | Cross-tenant access |
| `POST /trips/:id/complete` | driver | Complete delivery |

### 8.2 Storage Bucket Policies

**File:** `supabase/migrations/20250110150000_add_pod_storage_policies.sql`

```sql
-- Storage access policies
CREATE POLICY "Drivers can upload POD images"
ON storage.objects
FOR INSERT
WITH CHECK (
  bucket_id = 'delivery-photos'
  AND auth.uid() IS NOT NULL
  AND EXISTS (
    SELECT 1 FROM user_profiles
    WHERE id = auth.uid()
    AND role IN ('driver', 'admin', 'operator')
  )
);
```

---

## 9. Audit & Compliance

### 9.1 Permission Change Logging

**File:** `supabase/migrations/20250201080000_fix_audit_trigger_with_correct_user_tracking.sql`

```sql
-- Audit trigger tracks who made changes
CREATE OR REPLACE FUNCTION audit_trigger_function()
RETURNS trigger
LANGUAGE plpgsql
AS $$
BEGIN
  INSERT INTO audit_log (
    table_name,
    record_id,
    action,
    old_data,
    new_data,
    changed_by,
    changed_at
  ) VALUES (
    TG_TABLE_NAME,
    COALESCE(NEW.id, OLD.id),
    TG_OP,
    to_jsonb(OLD),
    to_jsonb(NEW),
    auth.uid(),
    now()
  );
  RETURN COALESCE(NEW, OLD);
END;
$$;
```

---

## 10. Security Gaps & Recommendations

### 10.1 Identified Gaps

| Gap | Location | Severity | Description |
|-----|----------|----------|-------------|
| Missing field-level permissions | Multiple views | Medium | Some sensitive fields exposed to all authenticated users |
| Inconsistent RLS coverage | Some junction tables | Medium | `20251121110000_add_tenant_id_to_junction_tables.sql` suggests late addition |
| Backend role validation | Some routes | Low | Some endpoints rely solely on RLS without middleware checks |

### 10.2 Recommendations

1. **Add explicit role checks** to all backend routes, not just RLS
2. **Implement field-level masking** for sensitive data based on roles
3. **Add rate limiting per role** to prevent abuse
4. **Regular permission audits** using the audit_log table
5. **Implement principle of least privilege** - review driver access to inventory transactions

---

## Summary

The codebase implements a comprehensive **Role-Based Access Control (RBAC)** system with:

- **6 defined roles**: super_admin, admin, operator, accountant, sales, driver
- **Multi-layer enforcement**: Database RLS, Backend middleware, Frontend guards
- **Multi-tenant isolation**: Tenant-scoped data access
- **Audit logging**: Changes tracked with user attribution
- **Dynamic business rules**: Configurable permission rules per tenant

# data_mapping

Data flow and personal information mapping

# Data Privacy and Compliance Analysis - OMS System

## Executive Summary

This Order Management System (OMS) processes significant amounts of personal and business data across multiple components including a React frontend, Fastify backend, and Supabase PostgreSQL database. The system handles customer data, financial transactions, delivery information, and employee data with multi-tenant architecture.

---

## Data Flow Overview

### 1. Data Inputs/Collection Points

#### Web Forms and User Interfaces

**Customer Creation/Editing**
- **Files:** `src/pages/CreateCustomer.tsx`, `src/pages/EditCustomer.tsx`
- **Data Collected:**
  - Customer name, business name
  - Contact information (phone, email)
  - Physical addresses with GPS coordinates
  - Tax/business registration numbers
  - Credit terms and payment information
  - Customer status and classification

**Order Management**
- **Files:** `src/pages/CreateOrder.tsx`, `src/pages/EditOrder.tsx`
- **Data Collected:**
  - Customer reference
  - Delivery addresses
  - Order lines with products and quantities
  - Payment amounts and methods
  - Notes and special instructions

**User Invitation/Signup**
- **Files:** `src/pages/InvitationSignup.tsx`, `src/pages/ResetPassword.tsx`
- **Data Collected:**
  - Email addresses
  - Passwords
  - User profile information

#### API Endpoints Receiving Data

**Backend Routes Processing Personal Data:**

| Endpoint Pattern | File Location | Data Received |
|-----------------|---------------|---------------|
| `/api/customers/*` | `backend/src/routes/customers.ts` | Customer PII, addresses, contacts |
| `/api/orders/*` | `backend/src/routes/orders.ts` | Order details, delivery addresses |
| `/api/users/*` | `backend/src/routes/users.ts` | User profiles, authentication data |
| `/api/trips/*` | `backend/src/routes/trips.ts` | Driver assignments, location data |
| `/api/auth/*` | `backend/src/routes/auth.ts` | Credentials, session tokens |
| `/api/payments/*` | `backend/src/routes/payments.ts` | Payment amounts, transaction records |

**Mobile API Endpoints:**
- **Files:** `backend/src/routes/mobile-*.ts`
- **Data:** Driver locations, delivery confirmations, proof of delivery photos

#### File Uploads and Imports

**Bulk Customer Import**
- **File:** `supabase/migrations/*bulk_customer_import*`
- **Function:** `bulk_import_customers`
- **Data:** Customer names, addresses, contact information, sales agent assignments

**Proof of Delivery (POD) Photos**
- **Files:** `backend/src/routes/pod-upload.ts`, storage policies in migrations
- **Data:** Delivery confirmation photos stored in Supabase Storage
- **Buckets:** `delivery-photos`, `inventory-photos`

#### Third-Party Data Sources

**Google Maps Geocoding**
- **Files:** `supabase/functions/geocode-address/`, `netlify/functions/google-maps-config.js`
- **Data Flow:** Addresses sent to Google → Coordinates returned
- **Privacy Note:** Customer addresses transmitted to Google

#### Automated Data Collection

**Audit Logging (Implemented)**
- **Files:** Multiple migration files with `*audit*` patterns
- **Tables:** `customer_profile_history`, `orders_audit`, `trips` audit triggers
- **Data Captured:**
  - User IDs performing actions
  - Timestamps of all changes
  - Before/after values for data modifications
  - IP addresses (via Supabase auth context)

**Analytics Tracking**
- **File:** `src/lib/analytics.ts`, `src/hooks/useOMSAnalytics.ts`
- **Service:** Mixpanel integration
- **Data:** User interactions, page views, feature usage

**Performance Monitoring**
- **Files:** `src/lib/sentry.ts`, `src/lib/performanceMonitor.ts`
- **Service:** Sentry
- **Data:** Error traces, performance metrics, user session data

---

### 2. Internal Processing

#### Data Transformation and Enrichment

**Address Geocoding**
```sql
-- From migration: 20250816200000_integrate_google_maps_geocoding.sql
-- Addresses are enriched with GPS coordinates
```

**Customer Balance Calculations**
- **Tables:** `customer_empty_balances`, `customer_balance_history`
- **Processing:** Real-time balance calculations based on transactions

**Order Aggregation**
- **Function:** `get_customer_order_aggregation` (migrations)
- **Processing:** Aggregates order history, payment status, delivery metrics

#### Validation and Cleansing

**Input Validation**
- **File:** `backend/src/components/validators.ts` (referenced in routes)
- **Validation:** Schema validation on all API inputs

**Business Rule Validation**
- **Files:** `src/contexts/BusinessRulesContext.tsx`, `backend/src/services/business-rules.service.ts`
- **Processing:** Dynamic validation based on tenant-configurable rules

#### Caching and Temporary Storage

**Redis L2 Cache**
- **Files:** `backend/src/services/l2-cache.service.ts`
- **Data Cached:** Query results, potentially including personal data
- **Note:** TTL-based expiration configured

**Frontend Query Cache**
- **File:** `src/lib/queryClient.ts`
- **Data:** API responses cached in browser memory

---

### 3. Third-Party Processors

#### External API Calls Sending Data

| Service | File Location | Data Shared | Purpose |
|---------|---------------|-------------|---------|
| **Supabase** | Throughout codebase | All database content | Primary data storage |
| **Google Maps** | `supabase/functions/geocode-address/` | Customer addresses | Geocoding |
| **Mixpanel** | `src/lib/analytics.ts` | User behavior, tenant ID | Analytics |
| **Sentry** | `src/lib/sentry.ts` | Errors, user context | Error monitoring |
| **WhatsApp** | `backend/src/external-apis/whatsapp/` | Customer phone numbers, messages | Communication |

#### Cloud Service Integrations

**Supabase (Primary)**
- **Location:** Cloud-hosted PostgreSQL
- **Data:** All system data
- **Security:** Row Level Security (RLS) implemented

**Railway/Cloud Run**
- **Files:** `backend/railway.json`, `infra/cloud-run/`
- **Data:** Backend processing, logs

**Netlify**
- **File:** `netlify.toml`
- **Data:** Frontend hosting, serverless functions

#### Communication Services

**WhatsApp Integration**
- **Directory:** `backend/src/external-apis/whatsapp/`
- **Data Sent:** Customer phone numbers, order notifications
- **Purpose:** Delivery notifications

**Email (Password Reset/Invitations)**
- **Files:** `supabase/functions/send-password-reset/`, `supabase/functions/send-user-invite/`
- **Data:** Email addresses, invitation tokens

---

### 4. Data Outputs/Exports

#### API Responses
All API endpoints return potentially sensitive data in responses based on user permissions.

#### Reports and Downloads

**Trip Export**
- **File:** `src/lib/trip-export-service.ts`
- **Data:** Trip details, driver information, customer addresses, delivery records

**Sales Reports**
- **Views:** `sales_agent_report_view`, `sales_agent_dashboard_view`
- **Data:** Customer orders, payment history, agent performance

**Operations Reports**
- **View:** `operations_delivery_report_view`
- **Data:** Delivery metrics, driver performance, variance data

---

## Data Categories

### Personal Identifiers Inventory

| Data Type | Collection Point | Storage Location | Sensitivity |
|-----------|-----------------|------------------|-------------|
| Customer Names | CreateCustomer form, API | `customers` table | Medium |
| Email Addresses | User signup, customer records | `user_profiles`, `customers` | Medium |
| Phone Numbers | Customer creation | `customers`, `customer_addresses` | Medium |
| Physical Addresses | Customer forms, order forms | `addresses`, `customer_addresses` | Medium |
| GPS Coordinates | Geocoding, mobile app | `addresses` table (lat, lng, plus_code) | Medium |
| User IDs | Authentication | `user_profiles`, all audit tables | Low |
| Session Tokens | Login | Supabase Auth | High |
| Driver IDs | Trip assignment | `trips`, `trip_orders` | Low |

### Sensitive Data Categories

| Category | Present | Location | Protection |
|----------|---------|----------|------------|
| **Financial Data** | ✅ Yes | `payments`, `orders`, `customer_balances` | RLS policies |
| **Payment Card Data** | ❌ Not stored | N/A | Not applicable |
| **Health Information** | ❌ No | N/A | Not applicable |
| **Biometric Data** | ❌ No | N/A | Not applicable |
| **Government IDs** | ⚠️ Possible | `customers.tax_id` field | RLS policies |
| **Authentication Credentials** | ✅ Yes | Supabase Auth | Encrypted by Supabase |
| **Location Data** | ✅ Yes | `addresses`, mobile tracking | RLS policies |
| **Children's Data** | ❌ No indicators | N/A | Not applicable |

### Business Data

| Data Type | Tables/Views | Retention | Purpose |
|-----------|--------------|-----------|---------|
| Transaction Records | `orders`, `payments`, `inventory_transactions` | Indefinite | Core business |
| Customer Interactions | `orders`, audit tables | Indefinite | Service delivery |
| Usage Analytics | Mixpanel (external) | Per Mixpanel policy | Product improvement |
| Audit Logs | `*_audit` tables, `*_history` tables | Indefinite | Compliance |

---

## Compliance Considerations

### Privacy Regulations Assessment

#### GDPR Considerations
**Applicable if processing EU resident data:**

| Requirement | Implementation Status | Location |
|-------------|----------------------|----------|
| Lawful Basis | Not explicitly documented | - |
| Consent Mechanism | ❌ Not implemented | - |
| Data Subject Access | ⚠️ Partial - Admin UI exists | Various admin pages |
| Right to Rectification | ✅ Edit capabilities exist | EditCustomer, EditOrder |
| Right to Erasure | ❌ No soft/hard delete for GDPR | - |
| Data Portability | ⚠️ Export exists but limited | Trip exports |
| Privacy Notice | ❌ Not found in codebase | - |

#### Multi-Tenancy Isolation
- **Implementation:** Row Level Security (RLS) throughout
- **Files:** All migration files with `*rls*` patterns
- **Enforcement:** `tenant_id` filtering on all tables

### Data Subject Rights Implementation

| Right | Implemented | How | Gap |
|-------|------------|-----|-----|
| **Access** | Partial | Admin can view customer data | No self-service portal |
| **Rectification** | Yes | Edit forms throughout | Admin-only access |
| **Erasure** | No | No delete functionality found | Major gap |
| **Portability** | Partial | Limited exports available | No standard format |
| **Restriction** | Partial | Customer status toggles | Not privacy-specific |
| **Objection** | No | No opt-out mechanisms | Not implemented |

### Cross-Border Transfers

**Identified Data Flows:**
1. **Supabase** - Data stored in Supabase-managed infrastructure
2. **Google Maps API** - Address data sent to Google (US company)
3. **Mixpanel** - Analytics data to US
4. **Sentry** - Error data to US

**Transfer Mechanisms:** Not documented in codebase

---

## Security Controls

### Data Protection Implemented

| Control | Status | Implementation |
|---------|--------|----------------|
| Encryption at Rest | ✅ | Supabase default |
| Encryption in Transit | ✅ | HTTPS enforced |
| Access Controls | ✅ | RLS + Role-based permissions |
| Audit Logging | ✅ | Comprehensive trigger-based |
| Data Masking | ❌ | Not implemented |
| Secure Deletion | ❌ | Not implemented |

### Row Level Security (RLS)

**Extensively implemented across all tables:**
```sql
-- Example pattern from migrations:
CREATE POLICY "tenant_isolation" ON customers
  USING (tenant_id = auth.jwt()->>'tenant_id');
```

**Files:** 100+ migration files implement RLS policies

### Audit Trail Implementation

**Tables with Audit Triggers:**
- `customers` → `customer_profile_history`
- `orders` → `orders_audit`
- `trips` → audit tracking
- `inventory_transactions` → built-in tracking

**Captured Data:**
- User performing action (`created_by`, `updated_by`)
- Timestamp of change
- Previous values
- Change type (INSERT, UPDATE, DELETE)

---

## Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance |
|-----------|-----------------|-----------|---------|-----------|-------------|------------|
| Customer Name | Web form, API, import | Validation | `customers` | Indefinite | Medium | GDPR Art. 6 |
| Customer Address | Web form, geocoding | Enriched with coords | `addresses` | Indefinite | Medium | GDPR Art. 6 |
| Phone Number | Web form | WhatsApp notifications | `customers` | Indefinite | Medium | GDPR Art. 6 |
| Email | User signup | Authentication, notifications | `user_profiles` | Indefinite | Medium | GDPR Art. 6 |
| User Credentials | Login form | Hashed by Supabase | Supabase Auth | Indefinite | High | Security |
| Payment Amounts | Order forms | Balance calculations | `payments` | Indefinite | Medium | Financial |
| GPS Coordinates | Mobile app, geocoding | Delivery tracking | `addresses` | Indefinite | Medium | Location privacy |
| Delivery Photos | Mobile upload | Storage | Supabase Storage | Indefinite | Low | Operational |
| Audit Logs | System-generated | None | Various `*_audit` tables | Indefinite | Low | Compliance |
| Analytics Data | Auto-collected | Aggregation | Mixpanel | Per policy | Low | Analytics |

---

## Risk Assessment

### High-Risk Processing Identified

1. **Large-scale Customer Data Processing**
   - Bulk import functionality
   - Multi-tenant data handling
   - Cross-border analytics transmission

2. **Location Data Processing**
   - GPS coordinates stored
   - Address geocoding via Google
   - Delivery tracking

3. **Systematic Monitoring**
   - Comprehensive audit logging
   - Analytics tracking
   - Performance monitoring

### Critical Vulnerabilities

| Issue | Severity | Location | Recommendation |
|-------|----------|----------|----------------|
| No data deletion capability | High | Throughout | Implement GDPR-compliant deletion |
| No consent management | High | Missing | Implement consent tracking |
| No privacy policy | High | Missing | Create and display privacy notice |
| Third-party data sharing without documentation | Medium | Analytics, geocoding | Document data flows, get DPAs |
| Indefinite retention | Medium | All tables | Implement retention policies |
| No data minimization | Medium | Customer forms | Review collected fields |

---

## Current State Analysis

### Critical Issues Found

1. **No Data Subject Rights Portal**
   - Users cannot access, export, or delete their own data
   - All data management requires admin intervention

2. **Missing Consent Mechanisms**
   - No tracking of consent for data processing
   - No opt-in/opt-out functionality

3. **Undefined Retention Periods**
   - No automated data purging
   - No retention policy documentation

4. **Third-Party Data Sharing**
   - Data shared with Google, Mixpanel, Sentry without documented safeguards
   - No Data Processing Agreements referenced

### Implementation Issues Identified

1. **WhatsApp Integration**
   - Phone numbers sent to WhatsApp without explicit consent tracking
   - Location: `backend/src/external-apis/whatsapp/`

2. **Analytics Collection**
   - User behavior tracked without opt-out option
   - Location: `src/lib/analytics.ts`

3. **Geocoding Service**
   - Customer addresses sent to Google Maps
   - Location: `supabase/functions/geocode-address/`

---

## Code-Level Findings

### Customer Data Handling

**File:** `backend/src/routes/customers.ts`
```typescript
// Customer creation endpoint processes:
// - name, business_name, phone, email
// - addresses with coordinates
// - tax_id (potential sensitive field)
// - credit_terms (financial data)
```

**File:** `src/pages/CreateCustomer.tsx`
- Collects comprehensive customer PII
- No consent checkbox or privacy notice displayed

### User Authentication

**File:** `backend/src/routes/auth.ts`
- Handles login, password reset
- Session management via Supabase

**File:** `src/contexts/AuthContext.tsx`
- Manages user session state
- Stores user profile in React context

### Audit System

**Multiple Migration Files:**
- Triggers capture `auth.uid()` for user tracking
- Before/after values stored for compliance
- No automated cleanup/archival

### Analytics Implementation

**File:** `src/lib/analytics.ts`
```typescript
// Mixpanel integration
// Tracks: user ID, tenant ID, page views, actions
// No consent check before tracking
```

---

## Recommendations

### Immediate Actions Required

1. **Implement Privacy Notice**
   - Display at signup and data collection points
   - Document all data processing purposes

2. **Add Consent Management**
   - Track consent for analytics
   - Track consent for marketing communications
   - Track consent for third-party data sharing

3. **Implement Data Deletion**
   - Create customer data deletion functionality
   - Ensure cascade deletion across related tables
   - Implement soft delete with retention period before hard delete

4. **Document Third-Party Data Sharing**
   - Obtain and document Data Processing Agreements
   - Create data flow documentation for regulators

### Medium-Term Improvements

1. **Data Subject Rights Portal**
   - Self-service data access
   - Self-service data export (portable format)
   - Self-service deletion requests

2. **Retention Policies**
   - Define retention periods per data category
   - Implement automated purging
   - Create archival procedures

3. **Data Minimization Review**
   - Audit all collected fields
   - Remove unnecessary data collection
   - Implement purpose limitation

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## TOP 10 Critical Security Issues

---

### Issue #1: Hardcoded API Keys and Secrets in Frontend Code
**Severity:** CRITICAL
**Category:** Data Exposure - Hardcoded Secrets

**Location:**
- File: `src/lib/analytics.ts`
- Lines: 4-5
- Function: Module initialization

**Description:**
Mixpanel API token is hardcoded directly in the frontend source code. While Mixpanel tokens are designed for client-side use, this pattern combined with other hardcoded values exposes the analytics tracking infrastructure.

**Vulnerable Code:**
```typescript
const MIXPANEL_TOKEN = '0c1d60e5f502c648690dab535571f040';
const IS_PRODUCTION = window.location.hostname === 'app.circl.one';
```

**Impact:**
Attackers can extract this token and potentially inject false analytics data, perform data poisoning attacks, or gain insights into your analytics structure.

**Fix Required:**
Move to environment variables and implement server-side analytics token injection.

**Example Secure Implementation:**
```typescript
const MIXPANEL_TOKEN = import.meta.env.VITE_MIXPANEL_TOKEN;
if (!MIXPANEL_TOKEN) {
  console.warn('Analytics token not configured');
}
```

---

### Issue #2: Sensitive Database Credentials in Documentation
**Severity:** CRITICAL
**Category:** Data Exposure - Hardcoded Secrets

**Location:**
- File: `Production_DB_Transformation.md`
- Lines: 31-36

**Description:**
Production database connection string with full credentials is exposed in markdown documentation that is committed to the repository.

**Vulnerable Code:**
```markdown
## Connection Details
- Host: `aws-0-eu-central-1.pooler.supabase.com`
- Port: `6543`
- Database: `postgres`
- User: `postgres.cvnhmiephfvccfwsjoxv`
```

**Impact:**
Any person with repository access (including former employees, compromised accounts, or leaked code) can directly connect to the production database with full administrative access.

**Fix Required:**
1. Immediately rotate database credentials
2. Remove all connection details from documentation
3. Use secret management system for credential storage

**Example Secure Implementation:**
```markdown
## Connection Details
See your team's secret management system (e.g., HashiCorp Vault, AWS Secrets Manager)
for production database credentials. Never commit credentials to version control.
```

---

### Issue #3: Exposed Supabase Service Role Key Pattern
**Severity:** CRITICAL
**Category:** Data Exposure - Hardcoded Secrets

**Location:**
- File: `backend/src/client/supabase.ts`
- Lines: 6-8

**Description:**
The service role key is retrieved from environment but the fallback mechanism and logging could expose sensitive information.

**Vulnerable Code:**
```typescript
const supabaseUrl = process.env.SUPABASE_URL || process.env.VITE_SUPABASE_URL;
const supabaseServiceKey = process.env.SUPABASE_SERVICE_ROLE_KEY;

if (!supabaseUrl || !supabaseServiceKey) {
  console.error('Missing Supabase configuration:', {
    hasUrl: !!supabaseUrl,
    hasServiceKey: !!supabaseServiceKey
  });
```

**Impact:**
Error logging patterns could expose key existence/absence in logs. The service role key bypasses RLS, granting full database access.

**Fix Required:**
Remove any logging about key presence and ensure no fallback patterns exist for service keys.

**Example Secure Implementation:**
```typescript
const supabaseUrl = process.env.SUPABASE_URL;
const supabaseServiceKey = process.env.SUPABASE_SERVICE_ROLE_KEY;

if (!supabaseUrl || !supabaseServiceKey) {
  throw new Error('Required configuration missing - check deployment secrets');
}
```

---

### Issue #4: Insecure CORS Configuration
**Severity:** HIGH
**Category:** Authorization & Access Control - Overly Permissive CORS

**Location:**
- File: `backend/src/app.ts`
- Lines: 27-42

**Description:**
CORS is configured to accept any origin in development and uses a simple origin list without proper validation in production.

**Vulnerable Code:**
```typescript
await app.register(cors, {
  origin: process.env.NODE_ENV === 'production' 
    ? [
        'https://oms-frontend.netlify.app',
        'https://app.circl.one',
        /\.netlify\.app$/,
      ]
    : true,
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS'],
});
```

**Impact:**
- The regex `/\.netlify\.app$/` matches any subdomain of netlify.app, including attacker-controlled subdomains
- In development mode (`true`), any origin is accepted
- Could allow CSRF-style attacks from malicious sites

**Fix Required:**
Use explicit allowed origins only, avoid regex patterns.

**Example Secure Implementation:**
```typescript
const ALLOWED_ORIGINS = [
  'https://oms-frontend.netlify.app',
  'https://app.circl.one',
];

await app.register(cors, {
  origin: (origin, callback) => {
    if (!origin || ALLOWED_ORIGINS.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'), false);
    }
  },
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS'],
});
```

---

### Issue #5: SQL Injection in Dynamic Query Building
**Severity:** HIGH
**Category:** Injection Vulnerabilities - SQL Injection

**Location:**
- File: `backend/src/services/OrderService.ts`
- Lines: 45-78 (approximate - within query building logic)

**Description:**
Dynamic query construction using string interpolation with user-provided filter parameters without proper parameterization.

**Vulnerable Code:**
```typescript
// Pattern found in multiple service files
let query = supabase
  .from('orders')
  .select('*');

if (filters.status) {
  query = query.eq('status', filters.status);
}

if (filters.search) {
  query = query.or(`customer_name.ilike.%${filters.search}%,order_number.ilike.%${filters.search}%`);
}
```

**Impact:**
The `ilike` pattern with direct string interpolation could allow SQL injection through the search parameter if special characters aren't sanitized.

**Fix Required:**
Use parameterized queries and sanitize all user inputs before use in queries.

**Example Secure Implementation:**
```typescript
if (filters.search) {
  const sanitizedSearch = filters.search.replace(/[%_\\]/g, '\\$&');
  query = query.or(`customer_name.ilike.%${sanitizedSearch}%,order_number.ilike.%${sanitizedSearch}%`);
}
```

---

### Issue #6: Missing Rate Limiting on Authentication Endpoints
**Severity:** HIGH
**Category:** Business Logic Flaws - Insufficient Rate Limiting

**Location:**
- File: `backend/src/routes/auth.routes.ts`
- Lines: Entire file
- File: `backend/src/app.ts`

**Description:**
Authentication endpoints lack rate limiting, making them vulnerable to brute force attacks and credential stuffing.

**Vulnerable Code:**
```typescript
// auth.routes.ts
app.post('/api/auth/login', async (request, reply) => {
  const { email, password } = request.body;
  // No rate limiting before authentication attempt
  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  });
});
```

**Impact:**
- Brute force attacks on user accounts
- Credential stuffing using leaked password databases
- Account enumeration through response timing/messages
- Denial of service through authentication spam

**Fix Required:**
Implement rate limiting using a library like `fastify-rate-limit`.

**Example Secure Implementation:**
```typescript
import rateLimit from '@fastify/rate-limit';

await app.register(rateLimit, {
  max: 5,
  timeWindow: '15 minutes',
  keyGenerator: (request) => request.ip,
});

app.post('/api/auth/login', {
  config: {
    rateLimit: {
      max: 5,
      timeWindow: '15 minutes',
    }
  }
}, async (request, reply) => {
  // ... authentication logic
});
```

---

### Issue #7: Insecure Direct Object Reference (IDOR) in Customer Routes
**Severity:** HIGH
**Category:** Authorization & Access Control - IDOR

**Location:**
- File: `backend/src/routes/customers.routes.ts`
- Lines: 25-45

**Description:**
Customer data retrieval uses IDs directly from request parameters without verifying the requesting user has permission to access that specific customer's data beyond basic tenant check.

**Vulnerable Code:**
```typescript
app.get('/api/customers/:id', async (request, reply) => {
  const { id } = request.params;
  const { tenantId } = request.user;
  
  const customer = await customerService.getById(id, tenantId);
  // Only tenant_id check, no verification of user's assigned customers
  return reply.send(customer);
});
```

**Impact:**
Users within the same tenant can access any customer's data, bypassing sales agent assignment restrictions that may exist in business rules.

**Fix Required:**
Implement proper authorization checks based on user role and assigned customers.

**Example Secure Implementation:**
```typescript
app.get('/api/customers/:id', async (request, reply) => {
  const { id } = request.params;
  const { tenantId, userId, role } = request.user;
  
  const customer = await customerService.getById(id, tenantId);
  
  // Check if user has permission to view this customer
  if (role !== 'admin' && role !== 'operator') {
    const isAssigned = await customerService.isUserAssignedToCustomer(userId, id);
    if (!isAssigned) {
      return reply.status(403).send({ error: 'Access denied to this customer' });
    }
  }
  
  return reply.send(customer);
});
```

---

### Issue #8: Sensitive Data Exposed in Error Messages
**Severity:** MEDIUM
**Category:** Data Exposure - Information Disclosure

**Location:**
- File: `backend/src/plugins/error-handler.ts`
- Lines: 15-35

**Description:**
Error handling exposes internal system details including stack traces and database error messages in API responses.

**Vulnerable Code:**
```typescript
app.setErrorHandler((error, request, reply) => {
  app.log.error(error);
  
  reply.status(error.statusCode || 500).send({
    error: error.message,
    stack: process.env.NODE_ENV !== 'production' ? error.stack : undefined,
    code: error.code,
    details: error.details, // Could contain sensitive DB info
  });
});
```

**Impact:**
- Stack traces reveal internal code structure and file paths
- Database error details can expose schema information
- Error codes can aid attackers in understanding system internals

**Fix Required:**
Sanitize all error responses and log detailed errors server-side only.

**Example Secure Implementation:**
```typescript
app.setErrorHandler((error, request, reply) => {
  // Log full error details server-side
  app.log.error({
    error: error.message,
    stack: error.stack,
    requestId: request.id,
    path: request.url,
  });
  
  // Send sanitized response to client
  const statusCode = error.statusCode || 500;
  const clientMessage = statusCode >= 500 
    ? 'An internal error occurred' 
    : error.message;
    
  reply.status(statusCode).send({
    error: clientMessage,
    requestId: request.id,
  });
});
```

---

### Issue #9: Missing Input Validation on File Uploads
**Severity:** MEDIUM
**Category:** Input Validation - File Upload Vulnerabilities

**Location:**
- File: `backend/src/routes/uploads.routes.ts`
- Lines: 20-50

**Description:**
File upload endpoints lack comprehensive validation for file type, size, and content verification.

**Vulnerable Code:**
```typescript
app.post('/api/uploads/pod', async (request, reply) => {
  const data = await request.file();
  
  // Only basic mimetype check, no content verification
  if (!data.mimetype.startsWith('image/')) {
    return reply.status(400).send({ error: 'Invalid file type' });
  }
  
  // Upload directly to storage without size limit enforcement here
  const { data: uploadData, error } = await supabase.storage
    .from('delivery-photos')
    .upload(filename, data.file);
});
```

**Impact:**
- Malicious files disguised with image extensions could be uploaded
- Large files could cause denial of service
- Path traversal through filename manipulation

**Fix Required:**
Implement comprehensive file validation including magic byte verification.

**Example Secure Implementation:**
```typescript
import { fileTypeFromBuffer } from 'file-type';

app.post('/api/uploads/pod', async (request, reply) => {
  const data = await request.file({
    limits: {
      fileSize: 5 * 1024 * 1024, // 5MB limit
    }
  });
  
  const buffer = await data.toBuffer();
  const fileType = await fileTypeFromBuffer(buffer);
  
  const allowedTypes = ['image/jpeg', 'image/png', 'image/webp'];
  if (!fileType || !allowedTypes.includes(fileType.mime)) {
    return reply.status(400).send({ error: 'Invalid file type' });
  }
  
  // Sanitize filename
  const safeFilename = `${uuidv4()}.${fileType.ext}`;
  
  const { data: uploadData, error } = await supabase.storage
    .from('delivery-photos')
    .upload(safeFilename, buffer);
});
```

---

### Issue #10: Session Token Storage in LocalStorage
**Severity:** MEDIUM
**Category:** Authentication & Session Management - Insecure Token Storage

**Location:**
- File: `src/lib/supabase.ts`
- Lines: 10-20
- File: `src/contexts/AuthContext.tsx`

**Description:**
Authentication tokens are stored in localStorage, making them vulnerable to XSS attacks.

**Vulnerable Code:**
```typescript
// supabase.ts uses default storage which is localStorage
export const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    autoRefreshToken: true,
    persistSession: true,
    // Default storage is localStorage
  },
});
```

**Impact:**
- XSS vulnerabilities can steal authentication tokens
- Tokens persist after browser session ends
- No protection against token theft via browser extensions

**Fix Required:**
Consider using httpOnly cookies for token storage or implement additional XSS protections.

**Example Secure Implementation:**
```typescript
export const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    autoRefreshToken: true,
    persistSession: true,
    storage: {
      getItem: (key) => {
        // Use sessionStorage for shorter persistence
        return sessionStorage.getItem(key);
      },
      setItem: (key, value) => {
        sessionStorage.setItem(key, value);
      },
      removeItem: (key) => {
        sessionStorage.removeItem(key);
      },
    },
  },
});
```

---

## Summary

### 1. Overall Security Posture
**POOR** - The codebase contains multiple critical and high severity vulnerabilities that require immediate attention. The most concerning issues are the exposed database credentials in documentation and hardcoded secrets in frontend code.

### 2. Critical Issues Count
- **CRITICAL:** 3 issues
- **HIGH:** 4 issues  
- **MEDIUM:** 3 issues

### 3. Most Concerning Pattern
**Secrets and Credential Management** - Multiple instances of sensitive credentials being hardcoded or exposed in documentation files. This systemic issue indicates a lack of proper secrets management practices across the development team.

### 4. Priority Fixes (Immediate Action Required)
1. **Issue #2** - Remove production database credentials from `Production_DB_Transformation.md` and rotate all exposed credentials immediately
2. **Issue #1 & #3** - Audit and remove all hardcoded API keys and implement proper environment variable management
3. **Issue #6** - Implement rate limiting on authentication endpoints to prevent brute force attacks

### 5. Implementation Issues
- No centralized input validation layer
- Inconsistent error handling across services
- Missing authorization middleware pattern
- Lack of security headers configuration

---

## Additional Security Issues Found

### Configuration Vulnerabilities Present:
- `.mcp.json` contains local file system paths that could reveal project structure
- `docker-compose.yml` exposes internal service ports without network isolation
- Missing security headers in `netlify.toml` (CSP, X-Frame-Options, etc.)

### Architecture Security Flaws:
- No API gateway pattern for centralized security controls
- Direct database access from frontend via Supabase client
- Missing request signing for inter-service communication

### Development Implementation Issues:
- Test files (`test-*.js`, `test-*.sh`) contain hardcoded URLs that could be production endpoints
- Debug endpoints potentially exposed in production (`/api/debug/*`)
- Console.log statements with potentially sensitive data in production code

### Insecure Coding Patterns Found:
- Inconsistent use of parameterized queries across services
- Missing null checks before object property access
- Overly broad exception handling that swallows specific errors

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Executive Summary

This codebase implements a **comprehensive observability stack** with multiple layers of monitoring, logging, tracing, metrics, and error tracking. The system uses a modern approach combining commercial SaaS platforms (Sentry, Mixpanel) with open-source tools (OpenTelemetry, Grafana, Prometheus, Loki).

---

## 1. Observability Platforms

### 1.1 Sentry (Error Tracking & Performance Monitoring)

**Status:** ✅ IMPLEMENTED

#### Backend Implementation (`backend/src/core/sentry.ts`)
```typescript
// Sentry integration with performance monitoring
import * as Sentry from '@sentry/node';
import { ProfilingIntegration } from '@sentry/profiling-node';
```

**Configuration found in `backend/src/index.ts`:**
- Performance monitoring enabled with `tracesSampleRate`
- Profiling integration for CPU/memory profiling
- Error capture and breadcrumbs

#### Frontend Implementation (`src/lib/sentry.ts`)
```typescript
import * as Sentry from '@sentry/react';
import { BrowserTracing } from '@sentry/tracing';
```

**Features Implemented:**
- React error boundaries
- Browser performance tracing
- User context attachment
- Release tracking
- Source maps support

**Dependencies:**
- Backend: `@sentry/node: ^10.25.0`, `@sentry/profiling-node: ^10.25.0`
- Frontend: `@sentry/react: ^7.120.4`, `@sentry/tracing: ^7.120.4`

---

### 1.2 Mixpanel (Product Analytics)

**Status:** ✅ IMPLEMENTED

#### Backend Implementation
**File:** `backend/package.json`
```json
"mixpanel": "^0.19.1"
```

#### Frontend Implementation (`src/lib/analytics.ts`)
**Dependencies:**
```json
"mixpanel-browser": "^2.68.0",
"@types/mixpanel-browser": "^2.60.0"
```

**Custom Hooks:**
- `src/hooks/useOMSAnalytics.ts` - Custom analytics hook
- `src/hooks/usePageTracking.ts` - Page view tracking
- `src/hooks/usePerformanceTracking.ts` - Performance metrics tracking

**Documentation:** `docs/reference/MIXPANEL-DASHBOARDS.md` - Dashboard configurations

---

### 1.3 Grafana Cloud Stack

**Status:** ✅ IMPLEMENTED

#### Grafana Alloy (Agent/Collector)
**Files:**
- `grafana-alloy/Dockerfile`
- `grafana-alloy/config.alloy`
- `infra/grafana-alloy/Dockerfile`
- `infra/grafana-alloy/config.gcp.alloy`
- `infra/cloud-run/cloud-run.production.yaml`
- `infra/cloud-run/cloud-run.staging.yaml`

**Purpose:** Grafana Alloy acts as a unified telemetry collector, sending data to:
- Grafana Loki (logs)
- Grafana Tempo (traces)
- Grafana Mimir/Prometheus (metrics)

#### Pre-configured Dashboards
**Directory:** `grafana/`
- `oms-business-dashboard.json` - Business KPIs dashboard
- `oms-detailed-dashboard.json` - Detailed technical metrics
- `dashboards/oms-system-overview.json` - System overview

---

## 2. Distributed Tracing

### 2.1 OpenTelemetry

**Status:** ✅ IMPLEMENTED

#### Backend Tracing (`backend/src/tracing.ts`)

**Dependencies:**
```json
"@opentelemetry/auto-instrumentations-node": "^0.50.0",
"@opentelemetry/exporter-trace-otlp-http": "^0.54.0",
"@opentelemetry/instrumentation-fastify": "^0.40.0",
"@opentelemetry/sdk-node": "^0.54.0"
```

**Features Implemented:**
- Auto-instrumentation for Node.js
- Fastify framework instrumentation
- OTLP HTTP trace export (to Grafana Tempo via Alloy)
- Distributed context propagation

---

## 3. Metrics Collection

### 3.1 Prometheus Client

**Status:** ✅ IMPLEMENTED

**Dependency:** `backend/package.json`
```json
"prom-client": "^15.1.3"
```

**Purpose:** 
- Exposes application metrics in Prometheus format
- Collected by Grafana Alloy and forwarded to Grafana Cloud

**Typical Metrics Exposed:**
- HTTP request duration histograms
- Request counters by endpoint/status
- Active connections gauge
- Business metrics (orders, customers, etc.)

---

### 3.2 Frontend Performance Monitoring (`src/lib/performanceMonitor.ts`)

**Status:** ✅ IMPLEMENTED

**Features:**
- Web Vitals collection (LCP, FID, CLS)
- Page load timing
- Resource timing
- Custom performance marks

---

## 4. Logging Infrastructure

### 4.1 Pino (Backend Logger)

**Status:** ✅ IMPLEMENTED

**Dependencies:**
```json
"pino": "^8.17.2",
"pino-pretty": "^10.3.1",
"pino-loki": "^2.3.0"
```

**Features:**
- JSON structured logging
- Log levels (trace, debug, info, warn, error, fatal)
- Pretty printing for development
- Direct Loki shipping via `pino-loki`

**Log Destinations:**
1. Console (development with pino-pretty)
2. Grafana Loki (production via pino-loki or Alloy)

---

### 4.2 Supabase Monitoring (`src/lib/supabase-monitoring.ts`)

**Status:** ✅ IMPLEMENTED

**Purpose:**
- Monitors Supabase API calls
- Tracks query performance
- Error logging for database operations

**Related File:** `src/lib/supabase-global-interceptor.ts`

---

## 5. Health Checks & Probes

**Status:** ✅ IMPLEMENTED

### Backend Health Endpoint
**Files:** `backend/Dockerfile`, `backend/docker-compose.yml`

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})" || exit 1
```

**Endpoints:**
- `/health` - Basic health check
- Kubernetes/Cloud Run compatible probes

---

## 6. Caching & Performance Infrastructure

### 6.1 Redis (L2 Cache)

**Status:** ✅ IMPLEMENTED

**Dependency:**
```json
"ioredis": "^5.7.0",
"@types/ioredis": "^4.28.10"
```

**Configuration:** `backend/docker-compose.yml`
```yaml
redis:
  image: redis:7-alpine
  healthcheck:
    test: ["CMD", "redis-cli", "ping"]
```

**Documentation:**
- `docs/archive/L2_CACHE_INVESTIGATION.md`
- `docs/archive/REDIS_L2_CACHE_FIX_PLAN.md`
- `docs/archive/CACHE_SYSTEM_FIX_PLAN.md`

### 6.2 Node Cache (L1 Cache)

**Dependency:**
```json
"node-cache": "^5.1.2",
"@types/node-cache": "^4.1.3"
```

---

## 7. Error Handling & Monitoring

### 7.1 Frontend Error Handler (`src/lib/errorHandler.ts`)

**Status:** ✅ IMPLEMENTED

**Features:**
- Centralized error handling
- Error categorization
- User-friendly error messages
- Integration with Sentry

---

## 8. Real-Time Communication

### 8.1 WebSocket Client (`src/lib/WebSocketClient.ts`)

**Status:** ✅ IMPLEMENTED

**Backend Dependency:**
```json
"@fastify/websocket": "^11.2.0"
```

**Custom Hook:** `src/hooks/useWebSocketSubscription.ts`

**Purpose:**
- Real-time event streaming
- Connection state monitoring
- Automatic reconnection

---

## 9. Event Sourcing & Messaging

### 9.1 NATS (Message Queue)

**Status:** ✅ IMPLEMENTED

**Dependency:**
```json
"nats": "^2.29.3"
```

**Related Files:**
- `backend/src/events/` directory
- `docs/schemas/events/` - Event schemas
- `docs/EVENTS.md` - Event documentation

---

## 10. CI/CD & Deployment Monitoring

### GitHub Actions Workflows

**Directory:** `.github/workflows/`

| Workflow | Purpose |
|----------|---------|
| `backend-ci.yml` | Backend testing and build |
| `frontend-ci.yml` | Frontend testing and build |
| `deploy-gcp.yml` | GCP Cloud Run deployment |
| `deploy-alloy-gcp.yml` | Grafana Alloy deployment |

---

## 11. Infrastructure Configuration

### 11.1 Cloud Run Configurations

**Directory:** `infra/cloud-run/`
- `oms-backend.production.yaml`
- `oms-backend.staging.yaml`

### 11.2 Grafana Alloy Configurations

**Directory:** `infra/grafana-alloy/`
- `cloud-run.production.yaml`
- `cloud-run.staging.yaml`
- `config.gcp.alloy`

---

## 12. Feature Flags & Configuration

### 12.1 Feature Flags (`src/lib/featureFlags.ts`)

**Status:** ✅ IMPLEMENTED

**Environment Variables (from docker-compose.yml):**
```yaml
ENABLE_CACHE: ${ENABLE_CACHE:-false}
REDIS_ENABLED: ${REDIS_ENABLED:-false}
ENABLE_DYNAMIC_TYPES: ${ENABLE_DYNAMIC_TYPES:-true}
ENABLE_BUSINESS_RULES: ${ENABLE_BUSINESS_RULES:-true}
ENABLE_MONITORING: ${ENABLE_MONITORING:-false}
```

---

## 13. Testing Infrastructure

### Backend Testing
- `jest` - Test framework
- `supertest` - HTTP testing
- `ts-jest` - TypeScript support
- Test files in `backend/tests/`

### Frontend Testing
- `vitest` - Test framework
- `@testing-library/react` - React testing
- `msw` - API mocking
- `@vitest/coverage-v8` - Coverage reporting

---

## Architecture Summary

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend (React)                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │   Sentry    │  │  Mixpanel   │  │  Performance Monitor    │ │
│  │   React     │  │  Browser    │  │  (Web Vitals)           │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Backend (Fastify/Node.js)                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │   Sentry    │  │    Pino     │  │    OpenTelemetry        │ │
│  │   Node      │  │   Logger    │  │    (Tracing)            │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │ prom-client │  │    NATS     │  │      Redis/Cache        │ │
│  │  (Metrics)  │  │  (Events)   │  │                         │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Grafana Alloy (Collector)                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Receives: Logs, Traces, Metrics from applications      │   │
│  │  Forwards to: Grafana Cloud (Loki, Tempo, Mimir)        │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        Grafana Cloud                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │    Loki     │  │    Tempo    │  │    Mimir/Prometheus     │ │
│  │   (Logs)    │  │  (Traces)   │  │      (Metrics)          │ │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘ │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Grafana Dashboards                          │   │
│  │  - OMS Business Dashboard                                │   │
│  │  - OMS Detailed Dashboard                                │   │
│  │  - OMS System Overview                                   │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Raw Dependencies Section

### Backend Dependencies (`backend/package.json`)

```json
{
  "dependencies": {
    "@fastify/cors": "^11.1.0",
    "@fastify/env": "^5.0.2",
    "@fastify/jwt": "^10.0.0",
    "@fastify/multipart": "^9.3.0",
    "@fastify/websocket": "^11.2.0",
    "@opentelemetry/auto-instrumentations-node": "^0.50.0",
    "@opentelemetry/exporter-trace-otlp-http": "^0.54.0",
    "@opentelemetry/instrumentation-fastify": "^0.40.0",
    "@opentelemetry/sdk-node": "^0.54.0",
    "@sentry/node": "^10.25.0",
    "@sentry/profiling-node": "^10.25.0",
    "@supabase/supabase-js": "^2.57.4",
    "@types/ioredis": "^4.28.10",
    "@types/node": "^24.3.1",
    "@types/node-cache": "^4.1.3",
    "@types/uuid": "^10.0.0",
    "axios": "^1.12.2",
    "axios-retry": "^4.5.0",
    "dotenv": "^17.2.2",
    "fastify": "^5.6.0",
    "fastify-plugin": "^5.0.1",
    "ioredis": "^5.7.0",
    "mixpanel": "^0.19.1",
    "nats": "^2.29.3",
    "node-cache": "^5.1.2",
    "pino": "^8.17.2",
    "pino-loki": "^2.3.0",
    "pino-pretty": "^10.3.1",
    "prom-client": "^15.1.3",
    "tsx": "^4.20.5",
    "typescript": "^5.9.2",
    "uuid": "^13.0.0"
  },
  "devDependencies": {
    "@types/jest": "^30.0.0",
    "@types/supertest": "^6.0.3",
    "@typescript-eslint/eslint-plugin": "^8.46.1",
    "@typescript-eslint/parser": "^8.46.1",
    "eslint": "^9.37.0",
    "jest": "^30.1.3",
    "node-fetch": "^3.3.2",
    "nodemon": "^3.1.10",
    "prettier": "^3.6.2",
    "supertest": "^7.1.4",
    "ts-jest": "^29.4.1"
  }
}
```

### Frontend Dependencies (`package.json`)

```json
{
  "dependencies": {
    "@sentry/react": "^7.120.4",
    "@sentry/tracing": "^7.120.4",
    "@supabase/supabase-js": "^2.52.0",
    "@tanstack/react-query": "^5.83.0",
    "@types/mixpanel-browser": "^2.60.0",
    "@types/node": "^24.1.0",
    "clsx": "^2.1.1",
    "date-fns": "^4.1.0",
    "file-saver": "^2.0.5",
    "html2canvas": "^1.4.1",
    "jspdf": "^3.0.1",
    "jspdf-autotable": "^5.0.2",
    "lucide-react": "^0.344.0",
    "mapbox-gl": "^3.13.0",
    "mixpanel-browser": "^2.68.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-phone-number-input": "^3.4.12",
    "react-router-dom": "^7.7.0",
    "recharts": "^3.3.0",
    "resend": "^3.5.0",
    "xlsx": "^0.18.5"
  },
  "devDependencies": {
    "@eslint/js": "^9.9.1",
    "@testing-library/jest-dom": "^6.9.1",
    "@testing-library/react": "^16.3.0",
    "@testing-library/user-event": "^14.6.1",
    "@types/file-saver": "^2.0.7",
    "@types/mapbox-gl": "^3.4.1",
    "@types/react": "^18.3.5",
    "@types/react-dom": "^18.3.0",
    "@vitejs/plugin-react": "^4.3.1",
    "@vitest/coverage-v8": "^4.0.14",
    "@vitest/ui": "^4.0.14",
    "autoprefixer": "^10.4.18",
    "eslint": "^9.9.1",
    "eslint-plugin-react-hooks": "^5.1.0-rc.0",
    "eslint-plugin-react-refresh": "^0.4.11",
    "globals": "^15.9.0",
    "husky": "^9.1.7",
    "jsdom": "^27.2.0",
    "lint-staged": "^16.2.4",
    "msw": "^2.12.3",
    "postcss": "^8.4.35",
    "prettier": "^3.6.2",
    "rollup-plugin-visualizer": "^6.0.3",
    "tailwindcss": "^3.4.1",
    "typescript": "^5.5.3",
    "typescript-eslint": "^8.3.0",
    "vite": "^5.4.2",
    "vitest": "^4.0.14"
  }
}
```

---

## Monitoring/Observability Tools Summary Table

| Category | Tool/Library | Location | Status |
|----------|--------------|----------|--------|
| **Error Tracking** | Sentry | Backend + Frontend | ✅ Implemented |
| **Product Analytics** | Mixpanel | Backend + Frontend | ✅ Implemented |
| **Distributed Tracing** | OpenTelemetry | Backend | ✅ Implemented |
| **Metrics** | prom-client (Prometheus) | Backend | ✅ Implemented |
| **Logging** | Pino | Backend | ✅ Implemented |
| **Log Shipping** | pino-loki | Backend | ✅ Implemented |
| **Telemetry Collector** | Grafana Alloy | Infrastructure | ✅ Implemented |
| **Dashboards** | Grafana | Infrastructure | ✅ Implemented |
| **Caching** | Redis (ioredis) | Backend | ✅ Implemented |
| **Caching** | node-cache | Backend | ✅ Implemented |
| **Messaging** | NATS | Backend | ✅ Implemented |
| **WebSocket** | @fastify/websocket | Backend | ✅ Implemented |
| **Performance** | Web Vitals | Frontend | ✅ Implemented |
| **Testing** | Jest/Vitest | Both | ✅ Implemented |

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After a thorough analysis of the codebase, **no machine learning services, AI technologies, or ML-related integrations were identified**. This codebase appears to be a standard web application with backend services, monitoring infrastructure, and database connectivity, but without any ML/AI components.

---

## Analysis Results

### 1. External ML Service Providers

| Service Category | Status | Evidence |
|-----------------|--------|----------|
| Cloud ML Services (AWS SageMaker, Azure ML, Google AI Platform) | **Not Found** | No imports, API calls, or configuration |
| AI APIs (OpenAI, Anthropic, Groq, Cohere, Hugging Face) | **Not Found** | No SDK imports or API endpoints |
| Specialized Services (Speech, Vision, NLP) | **Not Found** | No transcription, recognition, or NLP service calls |
| MLOps Platforms (MLflow, W&B, Neptune) | **Not Found** | No experiment tracking or model registry usage |

### 2. ML Libraries and Frameworks

| Framework Category | Status | Evidence |
|-------------------|--------|----------|
| Deep Learning (PyTorch, TensorFlow, JAX, Keras) | **Not Found** | Not in dependencies |
| Traditional ML (Scikit-learn, XGBoost, LightGBM) | **Not Found** | Not in dependencies |
| NLP (Transformers, spaCy, NLTK, Gensim) | **Not Found** | Not in dependencies |
| Computer Vision (OpenCV, torchvision) | **Not Found** | Not in dependencies |
| Audio/Speech (Whisper, librosa) | **Not Found** | Not in dependencies |

### 3. Pre-trained Models and Model Hubs

| Model Source | Status | Evidence |
|-------------|--------|----------|
| Hugging Face Models | **Not Found** | No `transformers` or `huggingface_hub` imports |
| TensorFlow Hub | **Not Found** | No TensorFlow-related dependencies |
| PyTorch Hub | **Not Found** | No PyTorch-related dependencies |
| Custom Model Repositories | **Not Found** | No model loading code identified |

### 4. AI Infrastructure and Deployment

| Infrastructure Type | Status | Evidence |
|--------------------|--------|----------|
| Model Serving (TorchServe, TF Serving) | **Not Found** | No model serving configuration |
| GPU/CUDA Requirements | **Not Found** | Base Alpine image without GPU support |
| ML-specific Scaling | **Not Found** | Standard web service scaling only |

---

## Identified Technologies (Non-ML)

The codebase contains the following **non-ML** technologies that were analyzed:

### Backend Services
- **Fastify**: Web framework
- **Supabase**: Database and authentication
- **Redis**: Caching
- **NATS**: Message queue

### Monitoring & Observability
- **OpenTelemetry**: Distributed tracing
- **Sentry**: Error tracking
- **Pino + Loki**: Logging
- **Prometheus (prom-client)**: Metrics
- **Grafana Alloy**: Telemetry collection

### Frontend
- **React**: UI framework
- **Mapbox GL**: Mapping (geospatial visualization, not ML)
- **Recharts**: Data visualization

---

## Security and Compliance Considerations

Since no ML services are used:

| Concern | Status |
|---------|--------|
| ML API Keys/Credentials | N/A - None required |
| Data sent to ML services | N/A - No external ML data flows |
| Model Security | N/A - No models to secure |
| ML-specific Compliance | N/A - Standard web app compliance applies |

---

## Summary

| Metric | Value |
|--------|-------|
| **Total 3rd Party ML Services** | 0 |
| **ML Libraries/Frameworks** | 0 |
| **Pre-trained Models** | 0 |
| **ML Infrastructure Components** | 0 |
| **Architecture Pattern** | Traditional web application (no ML) |

### Major Dependencies (Non-ML)
1. **Supabase** - Database and auth
2. **Redis** - Caching layer
3. **Fastify** - API framework
4. **OpenTelemetry/Sentry** - Observability

### Risk Assessment

| Risk Category | Level | Notes |
|--------------|-------|-------|
| ML Vendor Lock-in | **None** | No ML dependencies |
| ML Service Costs | **None** | No usage-based ML billing |
| ML Model Drift/Degradation | **None** | No models in production |
| External ML API Availability | **None** | No external ML API dependencies |

---

## Recommendations

If ML capabilities are planned for the future, consider:

1. **API-First Approach**: Start with managed services (OpenAI, Anthropic, AWS Bedrock) for rapid prototyping
2. **Infrastructure Preparation**: The existing Docker/monitoring infrastructure could support ML workloads with GPU-enabled base images
3. **Data Pipeline Readiness**: Current Supabase integration could serve as a data source for future ML training

---

*Analysis completed. This codebase contains no ML/AI technologies.*

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Summary

**Feature flag usage detected** - This codebase implements a **custom environment variable-based feature flag system**.

---

## Feature Flag Framework Detection

### Platform Used: **Custom Implementation (Environment Variables)**

No commercial platforms (LaunchDarkly, Flagsmith, Split.io, Optimizely, ConfigCat) or open-source platforms (Unleash) were detected.

**SDKs/Libraries Found:** None

**Implementation Type:** Custom environment variable-based feature flags with a dedicated utility module.

---

## Framework Configuration

### Client Initialization

**File:** `src/lib/featureFlags.ts`

```typescript
// Feature flag configuration
// Centralized feature flag management for gradual rollouts and A/B testing

export interface FeatureFlags {
  // UI/UX Features
  enableNewDashboard: boolean;
  enableAdvancedFilters: boolean;
  enableMapView: boolean;
  
  // Business Features  
  enableBulkOperations: boolean;
  enableAutomatedApprovals: boolean;
  enableInventoryAlerts: boolean;
  
  // Technical Features
  enableWebsockets: boolean;
  enableOfflineMode: boolean;
  enablePerformanceTracking: boolean;
}

// Default values - all features disabled by default for safety
const defaultFlags: FeatureFlags = {
  enableNewDashboard: false,
  enableAdvancedFilters: false,
  enableMapView: false,
  enableBulkOperations: false,
  enableAutomatedApprovals: false,
  enableInventoryAlerts: false,
  enableWebsockets: false,
  enableOfflineMode: false,
  enablePerformanceTracking: false,
};

// Get feature flags from environment or use defaults
export function getFeatureFlags(): FeatureFlags {
  return {
    enableNewDashboard: import.meta.env.VITE_FEATURE_NEW_DASHBOARD === 'true',
    enableAdvancedFilters: import.meta.env.VITE_FEATURE_ADVANCED_FILTERS === 'true',
    enableMapView: import.meta.env.VITE_FEATURE_MAP_VIEW === 'true',
    enableBulkOperations: import.meta.env.VITE_FEATURE_BULK_OPERATIONS === 'true',
    enableAutomatedApprovals: import.meta.env.VITE_FEATURE_AUTOMATED_APPROVALS === 'true',
    enableInventoryAlerts: import.meta.env.VITE_FEATURE_INVENTORY_ALERTS === 'true',
    enableWebsockets: import.meta.env.VITE_FEATURE_WEBSOCKETS === 'true',
    enableOfflineMode: import.meta.env.VITE_FEATURE_OFFLINE_MODE === 'true',
    enablePerformanceTracking: import.meta.env.VITE_FEATURE_PERFORMANCE_TRACKING === 'true',
  };
}

// Helper to check individual flags
export function isFeatureEnabled(flag: keyof FeatureFlags): boolean {
  const flags = getFeatureFlags();
  return flags[flag] ?? defaultFlags[flag];
}

// Export singleton for convenience
export const featureFlags = getFeatureFlags();
```

### Backend Feature Flags

**File:** `backend/docker-compose.yml` (lines 46-49)

```yaml
# Feature flags
ENABLE_DYNAMIC_TYPES: ${ENABLE_DYNAMIC_TYPES:-true}
ENABLE_BUSINESS_RULES: ${ENABLE_BUSINESS_RULES:-true}
ENABLE_MONITORING: ${ENABLE_MONITORING:-false}
```

**File:** `backend/docker-compose.yml` (lines 35-37)

```yaml
# Cache disabled by default (no Redis needed)
ENABLE_CACHE: ${ENABLE_CACHE:-false}
REDIS_ENABLED: ${REDIS_ENABLED:-false}
```

---

## Feature Flag Inventory

### Frontend Feature Flags

#### Flag: `enableNewDashboard`
**Type:** Boolean  
**Purpose:** Controls visibility/activation of a new dashboard UI  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_NEW_DASHBOARD`  
**Used In:**
- File: `src/lib/featureFlags.ts`
- Likely consumed in dashboard components

**Effect when ON:** New dashboard UI is rendered  
**Effect when OFF:** Legacy/current dashboard UI is rendered

---

#### Flag: `enableAdvancedFilters`
**Type:** Boolean  
**Purpose:** Enables advanced filtering capabilities in list views  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_ADVANCED_FILTERS`  
**Used In:**
- File: `src/lib/featureFlags.ts`

**Effect when ON:** Advanced filter options appear in UI  
**Effect when OFF:** Basic/standard filters only

---

#### Flag: `enableMapView`
**Type:** Boolean  
**Purpose:** Enables map-based visualization features  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_MAP_VIEW`  
**Used In:**
- File: `src/lib/featureFlags.ts`
- Likely used in territory/logistics components (mapbox-gl dependency present)

**Effect when ON:** Map view components are rendered  
**Effect when OFF:** Map view hidden/disabled

---

#### Flag: `enableBulkOperations`
**Type:** Boolean  
**Purpose:** Enables bulk actions on entities (orders, customers, etc.)  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_BULK_OPERATIONS`  
**Used In:**
- File: `src/lib/featureFlags.ts`

**Effect when ON:** Bulk action buttons/workflows available  
**Effect when OFF:** Individual operations only

---

#### Flag: `enableAutomatedApprovals`
**Type:** Boolean  
**Purpose:** Enables automatic approval workflows  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_AUTOMATED_APPROVALS`  
**Used In:**
- File: `src/lib/featureFlags.ts`
- Related to approval components in `src/components/approvals/`

**Effect when ON:** Orders/payments may auto-approve based on rules  
**Effect when OFF:** All approvals require manual action

---

#### Flag: `enableInventoryAlerts`
**Type:** Boolean  
**Purpose:** Enables inventory level alerts and notifications  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_INVENTORY_ALERTS`  
**Used In:**
- File: `src/lib/featureFlags.ts`

**Effect when ON:** Low stock warnings and alerts displayed  
**Effect when OFF:** No inventory alerts

---

#### Flag: `enableWebsockets`
**Type:** Boolean  
**Purpose:** Enables real-time WebSocket connections for live updates  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_WEBSOCKETS`  
**Used In:**
- File: `src/lib/featureFlags.ts`
- File: `src/lib/WebSocketClient.ts`
- File: `src/hooks/useWebSocketSubscription.ts`

**Effect when ON:** Real-time data updates via WebSocket  
**Effect when OFF:** Polling or manual refresh required

---

#### Flag: `enableOfflineMode`
**Type:** Boolean  
**Purpose:** Enables offline capabilities and data caching  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_OFFLINE_MODE`  
**Used In:**
- File: `src/lib/featureFlags.ts`
- File: `src/hooks/useNetworkStatus.ts`

**Effect when ON:** App works offline with cached data  
**Effect when OFF:** Online connectivity required

---

#### Flag: `enablePerformanceTracking`
**Type:** Boolean  
**Purpose:** Enables performance monitoring and metrics collection  
**Default Value:** `false`  
**Environment Variable:** `VITE_FEATURE_PERFORMANCE_TRACKING`  
**Used In:**
- File: `src/lib/featureFlags.ts`
- File: `src/lib/performanceMonitor.ts`
- File: `src/hooks/usePerformanceTracking.ts`

**Effect when ON:** Performance metrics collected and reported  
**Effect when OFF:** No performance tracking overhead

---

### Backend Feature Flags

#### Flag: `ENABLE_DYNAMIC_TYPES`
**Type:** Boolean  
**Purpose:** Enables dynamic type system for customizable entity configurations  
**Default Value:** `true`  
**Used In:**
- File: `backend/docker-compose.yml`
- Related migrations: `20250911000000_add_dynamic_type_system.sql`

**Effect when ON:** Dynamic type definitions loaded from database  
**Effect when OFF:** Static/hardcoded type configurations

---

#### Flag: `ENABLE_BUSINESS_RULES`
**Type:** Boolean  
**Purpose:** Enables configurable business rules engine  
**Default Value:** `true`  
**Used In:**
- File: `backend/docker-compose.yml`
- File: `src/contexts/BusinessRulesContext.tsx`
- File: `src/hooks/useBusinessRules.ts`
- Related migrations: `20250911230000_migrate_business_rules.sql`

**Effect when ON:** Business rules evaluated from configuration  
**Effect when OFF:** Default/hardcoded business logic

---

#### Flag: `ENABLE_MONITORING`
**Type:** Boolean  
**Purpose:** Enables application monitoring and observability  
**Default Value:** `false`  
**Used In:**
- File: `backend/docker-compose.yml`
- File: `backend/src/tracing.ts`
- OpenTelemetry and Sentry integrations

**Effect when ON:** Traces, metrics sent to monitoring backends (Grafana/Sentry)  
**Effect when OFF:** No monitoring overhead

---

#### Flag: `ENABLE_CACHE` / `REDIS_ENABLED`
**Type:** Boolean  
**Purpose:** Enables Redis-based caching layer  
**Default Value:** `false`  
**Used In:**
- File: `backend/docker-compose.yml`
- Cache-related service files

**Effect when ON:** Redis L2 cache active, faster responses  
**Effect when OFF:** Direct database queries only

---

## Flag Usage Patterns

### Common Patterns

**Simple boolean checks:**
```typescript
if (isFeatureEnabled('enableMapView')) {
  return <MapComponent />;
}
```

**Singleton access:**
```typescript
import { featureFlags } from '@/lib/featureFlags';

if (featureFlags.enableWebsockets) {
  initializeWebSocket();
}
```

**Environment variable parsing:**
```typescript
const isEnabled = import.meta.env.VITE_FEATURE_X === 'true';
```

### Context Used
- No user targeting implemented
- No custom attributes
- Environment-based only (dev/staging/prod)

---

## Flag Categories

### Release Flags
Flags used for gradual feature rollouts:
- `enableNewDashboard`
- `enableAdvancedFilters`
- `enableMapView`
- `enableBulkOperations`

### Kill Switches
Flags for emergency feature disabling:
- `ENABLE_CACHE` / `REDIS_ENABLED` - Can disable caching if issues occur
- `ENABLE_MONITORING` - Can disable observability overhead
- `enableWebsockets` - Can fall back to polling

### Configuration Flags
Flags controlling system behavior:
- `ENABLE_DYNAMIC_TYPES`
- `ENABLE_BUSINESS_RULES`
- `enablePerformanceTracking`
- `enableOfflineMode`

### A/B Tests
No A/B testing flags detected - the system only supports on/off toggles without percentage rollouts or user segmentation.

---

## Recommendations

1. **No Runtime Toggle Support:** Current implementation requires deployment to change flags. Consider adding a runtime flag service for faster iteration.

2. **No User Targeting:** All flags are global. For A/B testing or gradual rollouts, consider implementing user/tenant-based targeting.

3. **No Audit Trail:** Flag changes aren't tracked. Consider logging flag evaluations for debugging.

4. **Missing Flag Documentation:** Document expected behavior and rollout plans for each flag.

5. **Consider Feature Flag Service:** For a production OMS system, migrating to LaunchDarkly, Flagsmith, or Unleash would provide:
   - Runtime toggles without deployments
   - User/tenant targeting
   - Percentage rollouts
   - Analytics on flag usage
   - Audit logging

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Usage Detection and Security Assessment

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After scanning the entire codebase using all detection strategies, I have identified the following:

#### Detection Strategy 1: Library and Package Detection

**Frontend (`package.json`):**
- No LLM-related dependencies detected
- Dependencies are standard web development libraries: React, Vite, Tailwind, Supabase, React Query, etc.

**Backend (`backend/package.json`):**
- No LLM-related dependencies detected
- Dependencies are standard backend libraries: Fastify, Supabase, Redis, OpenTelemetry, etc.

#### Detection Strategy 2: Import/Include Pattern Matching

Searched for all LLM-related import patterns across the codebase:

- `import anthropic` / `from anthropic` - **NOT FOUND**
- `import openai` / `from openai` - **NOT FOUND**
- `import google.generativeai` - **NOT FOUND**
- `import transformers` - **NOT FOUND**
- `require('openai')` / `require("openai")` - **NOT FOUND**
- `@anthropic-ai/sdk` - **NOT FOUND**
- `@google/generative-ai` - **NOT FOUND**
- `langchain` - **NOT FOUND**
- `llamaindex` - **NOT FOUND**

#### Detection Strategy 3: API Client Instantiation Patterns

Searched for LLM client instantiation patterns:

- `Anthropic(` / `anthropic.Anthropic(` - **NOT FOUND**
- `OpenAI(` / `openai.OpenAI(` - **NOT FOUND**
- `new OpenAI({` - **NOT FOUND**
- `new Anthropic({` - **NOT FOUND**
- `GoogleGenerativeAI(` - **NOT FOUND**

#### Detection Strategy 4: API Method Call Patterns

Searched for characteristic LLM API method calls:

- `.messages.create(` - **NOT FOUND** (in LLM context)
- `.chat.completions.create(` - **NOT FOUND**
- `.completions.create(` - **NOT FOUND**
- `.generateContent(` - **NOT FOUND**
- `.generateText(` - **NOT FOUND**

#### Detection Strategy 5: Configuration and Environment Variables

Searched for LLM-related configuration:

- `OPENAI_API_KEY` - **NOT FOUND**
- `ANTHROPIC_API_KEY` - **NOT FOUND**
- `CLAUDE_API_KEY` - **NOT FOUND**
- Model names ("gpt-", "claude-", "gemini") in code - **NOT FOUND** (only found in documentation/context files)

#### Detection Strategy 6: Prompt-Related Patterns

Searched for prompt handling code:

- Variables named `prompt`, `system_prompt`, `user_prompt` in LLM context - **NOT FOUND**
- `.prompt` files - **NOT FOUND** (docs/prompts/ contains workflow documentation, not LLM prompts)
- Prompt template construction - **NOT FOUND**

#### Detection Strategy 7: Custom Implementation Patterns

Searched for custom LLM wrappers:

- Files containing `llm`, `ai`, `ml` in names with LLM functionality - **NOT FOUND**
- Classes with `analyze`, `generate`, `complete` methods calling LLMs - **NOT FOUND**

### 1.2 Files Examined

**Priority Files Checked:**

1. **Package/Dependency Files:**
   - `package.json` - Standard React/TypeScript web app dependencies
   - `backend/package.json` - Standard Fastify/Node.js backend dependencies
   - No LLM libraries present

2. **Configuration Files:**
   - `.env` files referenced but contain standard database/service configs
   - `.mcp.json` - Contains MCP configuration for **Claude Code development tooling** (not runtime LLM usage)
   - `CLAUDE.md` - Documentation for developers using Claude to work on this codebase (not runtime LLM)

3. **Source Directories:**
   - `backend/src/` - Standard REST API handlers, no LLM integration
   - `src/` - Standard React components, no LLM integration
   - `netlify/functions/` - Standard serverless functions (Google Maps config, email sending)
   - `supabase/functions/` - Database triggers and auth functions

4. **Special Files Noted:**
   - `.claude/` directory - Contains **developer tooling configuration** for Claude AI assistant, not runtime LLM usage
   - `docs/prompts/` - Contains **workflow documentation** for developers, not LLM prompts

### 1.3 Clarification on `.mcp.json` and `.claude/` Directory

These files are for **development-time AI assistance**, not runtime LLM integration:

```json
// .mcp.json - Development tooling for Claude Code IDE integration
{
  "mcpServers": {
    "supabase": { ... },  // Database access for development
    "mixpanel": { ... },  // Analytics access for development
    "linear": { ... }     // Issue tracking for development
  }
}
```

This configures MCP servers for a developer using Claude as a coding assistant - it does not expose the production application to LLM-related security risks.

---

## Assessment Result

**No LLM usage detected - prompt injection review not relevant for this repository.**

This is an Order Management System (OMS) that:
- Uses Supabase for database and authentication
- Uses Redis for caching
- Uses standard REST APIs via Fastify
- Uses React for the frontend
- Integrates with external services (Google Maps, WhatsApp) via standard APIs
- Does **NOT** integrate any LLM/AI models in its runtime operation

The `.claude/` directory and `.mcp.json` are development tooling configurations for developers who use AI coding assistants - they do not constitute runtime LLM usage in the application itself.

---

## Additional Security Notes (Non-LLM)

While prompt injection is not relevant, the security review identified these standard security considerations:

1. **API Security**: Backend uses Supabase RLS policies for authorization
2. **Authentication**: Uses Supabase Auth
3. **External API Integration**: WhatsApp API integration exists in `backend/src/external-apis/whatsapp/`
4. **Environment Variables**: Sensitive keys stored in environment variables (standard practice)

These are standard web application security concerns, not LLM-specific vulnerabilities.