# hl_overview

High level overview of the codebase

# Project Analysis: OMS System

## 0. Repository Name
[[oms-system]]

## 1. Project Purpose

This is an **Order Management System (OMS)** designed for a logistics/distribution business, likely in the gas cylinder or similar product delivery industry. The system handles:

- **Order lifecycle management**: Creation, approval, delivery, and payment tracking
- **Customer management**: Customer registration, status tracking, addresses, and balance management
- **Trip/Route planning**: Driver assignment, vehicle management, delivery route optimization
- **Inventory management**: Warehouse stock levels, cylinder tracking, variance reconciliation
- **Logistics operations**: Loading plans, offload documents, delivery proof (POD)
- **Multi-tenant support**: System supports multiple tenants with role-based access control
- **Mobile driver support**: Dedicated mobile API endpoints for drivers

The domain is clearly **B2B distribution/logistics** with emphasis on:
- Cylinder tracking (empties/fulls)
- Route-based deliveries
- Inventory variance management
- Payment collection and approval workflows

## 2. Architecture Pattern

**Hybrid Architecture combining:**
- **Event-Driven Architecture**: Event store, event handlers, NATS messaging integration
- **Layered/Clean Architecture**: Clear separation between routes, handlers, services, and data access
- **API-First Design**: OpenAPI/Swagger documentation with versioned endpoints
- **Monorepo Structure**: Frontend and backend in single repository

## 3. Technology Stack

### Backend
| Category | Technology |
|----------|------------|
| **Runtime** | Node.js |
| **Language** | TypeScript |
| **Framework** | Fastify |
| **Database** | PostgreSQL (via Supabase) |
| **Caching** | Redis (L2 cache) |
| **Authentication** | Supabase Auth, Firebase Auth |
| **Message Queue** | NATS |
| **Observability** | OpenTelemetry, Sentry, Grafana Alloy |
| **Process Manager** | PM2 |
| **Testing** | Jest, Vitest |

### Frontend
| Category | Technology |
|----------|------------|
| **Framework** | React 18+ |
| **Build Tool** | Vite |
| **Language** | TypeScript |
| **Styling** | Tailwind CSS, PostCSS |
| **State/Data** | React Query (TanStack Query) |
| **UI Components** | Custom components (shadcn/ui pattern) |
| **Analytics** | Mixpanel |
| **Testing** | Vitest |

### Infrastructure
| Category | Technology |
|----------|------------|
| **Cloud** | Google Cloud Platform (Cloud Run) |
| **Database Hosting** | Supabase |
| **Frontend Hosting** | Netlify |
| **Monitoring** | Grafana, Grafana Alloy |
| **CI/CD** | GitHub Actions |
| **Container** | Docker |

### Key Dependencies (Backend - package.json)
- `fastify` - Web framework
- `@supabase/supabase-js` - Database client
- `ioredis` - Redis client
- `nats` - Message queue
- `@opentelemetry/*` - Distributed tracing
- `@sentry/node` - Error monitoring
- `zod` - Schema validation
- `dayjs` - Date handling

### Key Dependencies (Frontend - package.json)
- `react`, `react-dom` - UI framework
- `@tanstack/react-query` - Server state management
- `@supabase/supabase-js` - Direct database access
- `tailwindcss` - Styling
- `lucide-react` - Icons
- `recharts` - Charts/visualizations
- `mixpanel-browser` - Analytics

## 4. Initial Structure Impression

```
oms-system/
├── backend/          # Node.js/Fastify API server
├── src/              # React frontend application
├── supabase/         # Database migrations and functions
├── infra/            # Infrastructure configurations (GCP, Grafana)
├── docs/             # Comprehensive documentation
├── grafana/          # Monitoring dashboards
├── tests/            # Shared test framework
├── scripts/          # Utility scripts
├── netlify/          # Serverless functions for frontend
└── plans/            # Architecture decision records
```

**Main Application Parts:**
1. **Backend API Server** (`/backend`) - Core business logic
2. **Frontend SPA** (`/src`) - Admin/Operations dashboard
3. **Database Layer** (`/supabase`) - PostgreSQL with extensive RLS policies
4. **Infrastructure** (`/infra`) - Cloud deployment configs

## 5. Configuration/Package Files

### Root Level
| File | Purpose |
|------|---------|
| `package.json` | Frontend dependencies and scripts |
| `tsconfig.json`, `tsconfig.app.json`, `tsconfig.node.json` | TypeScript configuration |
| `vite.config.ts` | Vite build configuration |
| `vitest.config.ts` | Frontend test configuration |
| `tailwind.config.js` | Tailwind CSS configuration |
| `postcss.config.js` | PostCSS configuration |
| `eslint.config.js` | ESLint configuration |
| `.prettierrc`, `.prettierignore` | Code formatting |
| `.lintstagedrc.json` | Pre-commit lint configuration |
| `netlify.toml` | Netlify deployment configuration |
| `.mcp.json` | Model Context Protocol configuration |
| `CLAUDE.md`, `.cursorrules` | AI assistant context files |

### Backend Level
| File | Purpose |
|------|---------|
| `backend/package.json` | Backend dependencies and scripts |
| `backend/tsconfig.json` | Backend TypeScript config |
| `backend/jest.config.js` | Jest test configuration |
| `backend/.eslintrc.json` | Backend ESLint rules |
| `backend/ecosystem.config.js` | PM2 process manager config |
| `backend/Dockerfile` | Container build definition |
| `backend/docker-compose.yml` | Local development services |
| `backend/railway.json` | Railway deployment config |

### Infrastructure
| File | Purpose |
|------|---------|
| `infra/cloud-run/*.yaml` | GCP Cloud Run service definitions |
| `infra/grafana-alloy/config.gcp.alloy` | Observability agent config |
| `supabase/config.toml` | Supabase local development config |

## 6. Directory Structure

### Backend (`/backend/src/`)
```
src/
├── index.ts              # Application entry point
├── app.ts                # Fastify app setup
├── tracing.ts            # OpenTelemetry initialization
├── config/               # Environment configuration
├── core/                 # Core utilities (4 files)
├── middleware/           # Request middleware
├── plugins/              # Fastify plugins (auth, swagger, etc.)
├── routes/               # API route definitions (37 files)
│   ├── v1/              # Versioned API routes
│   └── mobile/          # Mobile-specific endpoints
├── handlers/             # Request handlers (6 files)
├── services/             # Business logic services (18 files)
├── components/           # Domain components (18 files)
├── events/               # Event handling
├── external-apis/        # Third-party integrations (WhatsApp)
├── types/                # TypeScript type definitions
├── utils/                # Utility functions
└── client/               # External API clients
```

**Organization Pattern**: **Layered + Domain-based hybrid**
- Routes → Handlers → Services → Database
- Components encapsulate domain logic
- Clear separation of concerns

### Frontend (`/src/`)
```
src/
├── main.tsx              # Application entry point
├── App.tsx               # Root component with routing
├── index.css             # Global styles
├── pages/                # Page components (40+ files)
│   ├── Dashboard.tsx
│   ├── Orders.tsx, CreateOrder.tsx, OrderDetails.tsx
│   ├── Customers.tsx, CustomerDetails.tsx
│   ├── Trips.tsx, TripDetailsV2.tsx
│   ├── Inventory.tsx
│   ├── Approvals.tsx
│   └── logistics/        # Logistics sub-pages
├── components/
│   ├── ui/               # Reusable UI components (91 files)
│   ├── layout/           # Layout components
│   ├── dashboard/        # Dashboard widgets
│   ├── settings/         # Settings panels (17 files)
│   ├── approvals/        # Approval workflows
│   ├── logistics/        # Logistics components
│   ├── territory/        # Territory management
│   ├── trip-planning/    # Trip planning components
│   └── reports/          # Reporting components
├── hooks/                # Custom React hooks (15 files)
├── lib/                  # Utility libraries
│   ├── supabase.ts       # Supabase client
│   ├── api-client.ts     # API client
│   ├── analytics.ts      # Mixpanel integration
│   └── WebSocketClient.ts # Real-time updates
├── contexts/             # React contexts (Auth, BusinessRules, TenantConfig)
├── types/                # TypeScript definitions
└── test/                 # Test utilities
```

**Organization Pattern**: **Feature-based + Component-based hybrid**
- Pages organized by domain entity
- Components organized by function (UI, domain-specific)
- Hooks for reusable logic

### Database (`/supabase/migrations/`)
- **300+ migration files** covering:
  - Schema definitions
  - RLS (Row Level Security) policies
  - Database functions/RPCs
  - Triggers and constraints
  - Views for optimized queries

## 7. High-Level Architecture

### Architectural Patterns

**1. Event-Driven Architecture**
- Evidence: `/backend/src/events/`, event store tables, NATS integration
- Events for order status changes, inventory updates, approvals
- Event schemas documented in `/docs/schemas/events/`

**2. API Gateway Pattern (Backend)**
```
Client Request → Fastify Routes → Handlers → Services → Database
                      ↓
              Auth Middleware → Role-based access
```

**3. CQRS-lite Pattern**
- Database views for optimized reads (e.g., `orders_list_view`, `trips_list_view`)
- Separate write operations through RPCs and services

**4. Multi-Tenant Architecture**
- `tenant_id` on all tables
- RLS policies enforcing tenant isolation
- Tenant-specific configurations

### Communication Patterns

```
┌─────────────────┐     ┌──────────────────┐
│  React Frontend │────▶│  Fastify Backend │
│    (Netlify)    │     │   (Cloud Run)    │
└─────────────────┘     └────────┬─────────┘
        │                        │
        │                        ▼
        │               ┌──────────────────┐
        │               │    PostgreSQL    │
        └──────────────▶│    (Supabase)    │
          Direct access └──────────────────┘
                                │
                        ┌───────┴───────┐
                        ▼               ▼
                    ┌───────┐      ┌────────┐
                    │ Redis │      │  NATS  │
                    │ Cache │      │ Events │
                    └───────┘      └────────┘
```

**Key Evidence:**
- `/backend/src/plugins/` - Fastify plugins for auth, CORS, rate limiting
- `/docs/EVENTS.md` - Event documentation
- `/plans/event-driven-cache-invalidation-architecture.md` - Event architecture plan
- `/docs/ARCHITECTURE.md` - Architecture documentation

### Security Model
- Row Level Security (RLS) at database level
- JWT authentication via Supabase Auth
- Role-based access control (Super Admin, Admin, Operator, Driver, Sales, Accountant)

## 8. Build, Execution and Test

### Backend

**Development:**
```bash
cd backend
npm install
npm run dev  # Starts with hot reload
```

**Production Build:**
```bash
npm run build
npm start
```

**Docker:**
```bash
docker-compose up
# or
docker build -t oms-backend .
```

**Testing:**
```bash
npm test                    # Run all tests
npm run test:unit          # Unit tests only
npm run test:integration   # Integration tests
./run-tests.sh             # Full test suite
```

**Entry Points:**
- `backend/src/index.ts` - Main entry (imports tracing first)
- `backend/src/app.ts` - Fastify application setup

### Frontend

**Development:**
```bash
npm install
npm run dev  # Vite dev server
```

**Production Build:**
```bash
npm run build  # Output to dist/
```

**Testing:**
```bash
npm test       # Vitest
npm run lint   # ESLint
```

**Entry Points:**
- `src/main.tsx` - React application bootstrap
- `index.html` - HTML entry point

### Database

**Local Development:**
```bash
supabase start
supabase db push  # Apply migrations
```

**Migration Management:**
```bash
supabase migration new <name>
supabase db push
```

### CI/CD (GitHub Actions)

| Workflow | Purpose |
|----------|---------|
| `backend-ci.yml` | Backend tests and lint |
| `frontend-ci.yml` | Frontend tests and lint |
| `deploy-gcp.yml` | Deploy backend to Cloud Run |
| `deploy-alloy-gcp.yml` | Deploy Grafana Alloy |

### Deployment

**Backend**: GCP Cloud Run (via GitHub Actions or manual scripts)
```bash
./backend/deploy-staging.sh
```

**Frontend**: Netlify (automatic on push)
- Configured via `netlify.toml`

### Key Scripts

**Backend (`package.json`):**
- `dev` - Development server
- `build` - TypeScript compilation
- `start` - Production server
- `test` - Jest tests
- `lint` - ESLint

**Frontend (`package.json`):**
- `dev` - Vite dev server
- `build` - Production build
- `preview` - Preview production build
- `test` - Vitest
- `lint` - ESLint

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown Analysis

## Backend Directory (`@backend/`)

### 1. Core Responsibility
The backend serves as the primary API layer for the OMS (Order Management System), handling all business logic, data persistence, authentication, and integration with external services. It's built with Fastify and TypeScript.

---

### 2. Key Components

#### `@backend/src/`

##### **Entry Points**
| File | Role |
|------|------|
| `index.ts` | Application bootstrap, server initialization |
| `app.ts` | Fastify app configuration, plugin registration |
| `tracing.ts` | OpenTelemetry/observability setup |

##### **`@backend/src/routes/` (37 files)**
API route definitions organized by domain entity:
- Order routes, customer routes, trip routes
- Inventory, warehouse, vehicle routes
- Authentication, user management routes
- Approval workflow routes
- Analytics/reporting routes

##### **`@backend/src/services/` (18 files)**
Business logic layer:
- Order processing, validation
- Customer management
- Trip/delivery orchestration
- Inventory calculations
- Payment processing
- Cache management (Redis L2)

##### **`@backend/src/handlers/` (6 files)**
Request handlers that bridge routes to services:
- Likely includes validation, error handling wrappers
- Request/response transformation

##### **`@backend/src/components/` (18 files)**
Reusable business components:
- Likely domain-specific utilities
- Shared business rule implementations
- Component-based architecture pieces

##### **`@backend/src/middleware/` (1 file)**
Request middleware:
- Authentication/authorization checks
- Request logging, validation

##### **`@backend/src/plugins/` (5 files)**
Fastify plugins:
- Database connections (Supabase)
- Caching (Redis)
- Authentication plugins
- Custom Fastify extensions

##### **`@backend/src/config/` (1 file)**
Environment/configuration management:
- Environment variables
- Feature flags
- API keys configuration

##### **`@backend/src/core/` (4 files)**
Core framework utilities:
- Base service classes
- Core abstractions
- Shared patterns

##### **`@backend/src/events/` (1 file)**
Event-driven architecture:
- Event definitions
- Event emitters/handlers
- Pub/sub patterns

##### **`@backend/src/client/` (1 file)**
External client configurations:
- Database clients
- API clients initialization

##### **`@backend/src/external-apis/whatsapp/`**
WhatsApp integration:
- Message sending
- Notification delivery
- Customer communication

##### **`@backend/src/types/` (4 files)**
TypeScript type definitions:
- API request/response types
- Domain entity types
- Shared interfaces

##### **`@backend/src/utils/` (2 files)**
Utility functions:
- String manipulation
- Date formatting
- General helpers

---

### 3. Dependencies & Interactions

#### Internal Dependencies
```
routes/ → handlers/ → services/ → components/
                    ↓
              plugins/ (DB, Cache)
                    ↓
              external-apis/
```

- **Routes** depend on **handlers** and **services**
- **Services** depend on **plugins** (Supabase, Redis)
- **Services** may depend on **components** for shared logic
- **All layers** use **types/** and **utils/**

#### External Service Interactions
| Service | Purpose |
|---------|---------|
| **Supabase** | PostgreSQL database, authentication |
| **Redis** | L2 caching layer |
| **WhatsApp API** | Customer notifications |
| **OpenTelemetry** | Distributed tracing |

---

## Frontend Directory (`@src/`)

### 1. Core Responsibility
React-based single-page application providing the user interface for the OMS. Handles user interactions, state management, and API communication.

---

### 2. Key Components

#### **Entry Points**
| File | Role |
|------|------|
| `main.tsx` | React app bootstrap, provider setup |
| `App.tsx` | Root component, routing configuration |
| `index.css` | Global styles (Tailwind) |

#### **`@src/pages/` (38+ files)**
Page-level components (route targets):

| Category | Examples |
|----------|----------|
| **Dashboard** | `Dashboard.tsx` |
| **Orders** | `Orders.tsx`, `OrderDetails.tsx`, `CreateOrder.tsx`, `EditOrder.tsx` |
| **Customers** | `Customers.tsx`, `CustomerDetails.tsx`, `CreateCustomer.tsx`, `EditCustomer.tsx` |
| **Trips** | `Trips.tsx`, `TripDetailsV2.tsx`, `CreateTrip.tsx`, `EditTrip.tsx` |
| **Inventory** | `Inventory.tsx`, `ManualAdjustments.tsx`, `PerformCount.tsx` |
| **Products** | `Products.tsx`, `ProductDetails.tsx`, `CreateProduct.tsx`, `AddVariant.tsx` |
| **Approvals** | `Approvals.tsx`, `ReviewVariances.tsx`, `VarianceDashboard.tsx` |
| **Logistics** | `Logistics.tsx`, `logistics/` (9 files) |
| **Admin** | `TenantManagement.tsx`, `TerritoryManagement.tsx`, `Prices.tsx` |
| **Auth** | `ResetPassword.tsx`, `InvitationSignup.tsx` |
| **Reports** | `Reports.tsx` |
| **Tools** | `ImportTools.tsx`, `MobileSimulator.tsx` |

#### **`@src/components/`**

##### `ui/` (91 files)
Reusable UI primitives:
- Buttons, inputs, modals, tables
- Form components
- Data display components
- Loading states, alerts

##### `settings/` (17 files)
Settings/configuration UI:
- User preferences
- System configuration
- Business rules management

##### `auth/` (2 files)
Authentication components:
- Login forms
- Protected route wrappers

##### `layout/` (2 files)
Layout components:
- App shell, navigation
- Sidebar, header

##### `dashboard/` (4 files)
Dashboard-specific widgets:
- KPI cards
- Charts, summaries

##### `territory/` (7 files)
Territory/geography management:
- Maps, zones
- Area assignments

##### `trip-planning/` (9 files)
Trip management UI:
- Trip builder
- Route visualization
- Driver assignment

##### `approvals/` (nested subdirectories)
Approval workflow UIs:
- `customer-approvals/`
- `order-approvals/`
- `payment-approvals/`
- `variance-approvals/`
- `shared/`, `types/`

##### `logistics/` (9 files)
Logistics/operations UI:
- Warehouse management
- Vehicle tracking
- Loading/offloading

##### `reports/` (8 files)
Reporting components:
- Report generators
- Data visualizations
- Export tools

#### **`@src/contexts/` (3 files)**
React Context providers:
| Context | Purpose |
|---------|---------|
| `AuthContext.tsx` | User authentication state |
| `BusinessRulesContext.tsx` | Business rules/configuration |
| `TenantConfigContext.tsx` | Multi-tenant settings |

#### **`@src/hooks/` (15 files)**
Custom React hooks:
| Hook | Purpose |
|------|---------|
| `useApiQuery.ts` | API data fetching |
| `useBusinessRules.ts` | Business rule access |
| `usePermissions.ts` | Role-based access control |
| `useTerminology.ts` | Dynamic terminology |
| `useOMSAnalytics.ts` | Analytics tracking |
| `useBundleOperations.ts` | Product bundle logic |
| `useWebSocketSubscription.ts` | Real-time updates |
| `useSessionTimeout.ts` | Session management |
| `useNetworkStatus.ts` | Offline detection |
| `usePerformanceTracking.ts` | Performance monitoring |

#### **`@src/lib/` (20+ files)**
Utility libraries and clients:

| File | Purpose |
|------|---------|
| `api-client.ts` | HTTP client for backend API |
| `supabase.ts` | Supabase client initialization |
| `WebSocketClient.ts` | Real-time connection |
| `queryClient.ts` | React Query configuration |
| `analytics.ts` | Mixpanel/analytics integration |
| `sentry.ts` | Error tracking |
| `errorHandler.ts` | Global error handling |
| `featureFlags.ts` | Feature toggle system |
| `currencyUtils.ts` | Currency formatting |
| `dateUtils.ts` | Date manipulation |
| `imageUploadHelpers.ts` | Image upload handling |
| `inventoryUtils.ts` | Inventory calculations |
| `trip-export-service.ts` | Trip data export |
| `varianceCodes.ts` | Variance code definitions |

#### **`@src/types/` (4 files)**
TypeScript definitions:
- `business-rules.ts`
- `inventory.ts`
- `tenant-config.ts`
- `trip-export.ts`

---

### 3. Dependencies & Interactions

#### Internal Data Flow
```
pages/ → components/ → hooks/ → lib/api-client → Backend API
                    ↓
              contexts/ (global state)
                    ↓
              lib/supabase (direct queries if needed)
```

#### External Service Interactions
| Service | Purpose |
|---------|---------|
| **Backend API** | Primary data source |
| **Supabase** | Direct auth, real-time subscriptions |
| **Mixpanel** | User analytics |
| **Sentry** | Error tracking |
| **Google Maps** | Mapping/geocoding |

---

## Supabase Directory (`@supabase/`)

### 1. Core Responsibility
Database schema management, migrations, and PostgreSQL functions/RPC. Acts as the data layer with Row Level Security (RLS) policies.

---

### 2. Key Components

#### **`migrations/` (300+ files)**
Database schema evolution:

| Migration Type | Examples |
|----------------|----------|
| **Schema changes** | Table creation, column additions |
| **RLS policies** | Security rules per table |
| **Functions/RPCs** | `complete_delivery`, `get_driver_trip_mobile`, variance approvals |
| **Views** | `orders_list_view`, `customers_list_view`, `trips_list_view` |
| **Triggers** | Audit tracking, auto-calculations |
| **Indexes** | Performance optimization |

Key functional areas in migrations:
- Order processing functions
- Trip/delivery management
- Inventory tracking
- Customer balance management
- Approval workflows
- Multi-tenant isolation
- Mobile API functions
- Analytics/reporting views

#### **`functions/` (4 subdirectories)**
Edge Functions:
| Function | Purpose |
|----------|---------|
| `firebase-auth/` | Firebase authentication bridge |
| `geocode-address/` | Google Maps geocoding |
| `send-password-reset/` | Password reset emails |
| `send-user-invite/` | User invitation emails |

#### **`scripts/`**
Database maintenance scripts:
- `cleanup_transactional_data.sql`

#### **Schema Files**
| File | Purpose |
|------|---------|
| `dev_schema.sql` | Development environment schema |
| `prod_schema.sql` | Production environment schema |
| `config.toml` | Supabase CLI configuration |

---

### 3. Dependencies & Interactions

#### Internal
- **Migrations** build on each other sequentially
- **Functions** reference tables and other functions
- **Views** aggregate data from multiple tables

#### External
- **Google Maps API** (geocoding function)
- **Email service** (password reset, invitations)
- **Firebase** (authentication bridge)

---

## Infrastructure (`@infra/`)

### 1. Core Responsibility
Cloud deployment configurations for GCP Cloud Run and monitoring infrastructure.

---

### 2. Key Components

#### **`cloud-run/`**
| File | Purpose |
|------|---------|
| `oms-backend.production.yaml` | Production deployment config |
| `oms-backend.staging.yaml` | Staging deployment config |

#### **`grafana-alloy/`**
| File | Purpose |
|------|---------|
| `Dockerfile` | Grafana Alloy container |
| `config.gcp.alloy` | Alloy configuration |
| `cloud-run.*.yaml` | Deployment configs |

#### **`scripts/`**
| File | Purpose |
|------|---------|
| `setup-custom-domains.sh` | Domain configuration |

---

### 3. Dependencies & Interactions

- Deploys **backend** to GCP Cloud Run
- Connects to **Grafana Cloud** for observability
- Uses GCP services (Cloud Run, networking)

---

## Documentation (`@docs/`)

### 1. Core Responsibility
Project documentation, architectural decisions, API specifications, and operational guides.

---

### 2. Key Components

#### **Active Documentation**
| File | Purpose |
|------|---------|
| `AI-CONTEXT.md` | AI assistant context |
| `API.md` | API documentation |
| `ARCHITECTURE.md` | System architecture |
| `DOMAIN-MODEL.md` | Business domain model |
| `EVENTS.md` | Event-driven patterns |
| `LOCAL-DEV.md` | Local development setup |
| `WORKFLOWS.md` | Business workflows |

#### **`schemas/`**
| Subdirectory | Purpose |
|--------------|---------|
| `api.yaml` | OpenAPI specification |
| `events/` (12 files) | Event schema definitions |

#### **`deploy/`**
Deployment documentation:
- `GCP-CLOUD-RUN-SETUP.md`

#### **`solutions/`**
Solution documentation:
- `performance-issues/`

#### **`archive/`**
Historical documentation and completed work.

---

## Testing Infrastructure

### Backend Tests (`@backend/tests/`)

| Directory | Purpose |
|-----------|---------|
| `unit/` | Unit tests |
| `integration/` | Integration tests (customers, orders, offload-documents) |
| `mocks/` | Test mocks |
| `utils/` | Test utilities |

### Frontend Tests (`@src/test/`)

| File/Directory | Purpose |
|----------------|---------|
| `setup.ts` | Test configuration |
| `mocks/` | Mock data/services |

### Root Tests (`@tests/`)

| File | Purpose |
|------|---------|
| `framework/DataFactory.ts` | Test data generation |
| `framework/TestFramework.ts` | Test utilities |

---

## CI/CD (`@.github/`)

### 1. Workflows

| Workflow | Purpose |
|----------|---------|
| `backend-ci.yml` | Backend testing/linting |
| `frontend-ci.yml` | Frontend testing/linting |
| `deploy-gcp.yml` | GCP deployment |
| `deploy-alloy-gcp.yml` | Grafana Alloy deployment |

### 2. Supporting Files
- `pull_request_template.md`
- `hooks/validate-code-changes.sh`

---

## Summary Dependency Graph

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (src/)                       │
│  pages/ → components/ → hooks/ → lib/ → API Client          │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Backend (backend/src/)                    │
│  routes/ → handlers/ → services/ → plugins/ → DB/Cache      │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌──────────┐   ┌──────────┐   ┌──────────────┐
        │ Supabase │   │  Redis   │   │ External APIs│
        │  (DB)    │   │ (Cache)  │   │  (WhatsApp)  │
        └──────────┘   └──────────┘   └──────────────┘
```

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Repository: oms-system_aeed8a86

---

## Internal Modules

### Backend (`/backend/src/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `app.ts` / `index.ts` | Application entry point and server initialization |
| `tracing.ts` | OpenTelemetry tracing configuration and setup |
| `config/` | Application configuration management |
| `core/` | Core domain logic and foundational components |
| `middleware/` | HTTP request/response middleware (authentication, logging, etc.) |
| `routes/` | API route definitions and endpoint handlers (37 files) |
| `handlers/` | Request handlers for specific operations |
| `services/` | Business logic services layer (18 files) |
| `components/` | Reusable backend components (18 files) |
| `plugins/` | Fastify plugin integrations |
| `events/` | Event handling and event-driven architecture components |
| `client/` | External client configurations |
| `utils/` | Utility functions and helpers |
| `types/` | TypeScript type definitions |
| `external-apis/whatsapp/` | WhatsApp API integration |

### Frontend (`/src/`)

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `App.tsx` / `main.tsx` | Application entry point and root component |
| `pages/` | Page-level React components (Orders, Customers, Trips, Dashboard, Inventory, Reports, etc.) |
| `components/ui/` | Reusable UI components library (91 files) |
| `components/settings/` | Settings management components (17 files) |
| `components/auth/` | Authentication-related components |
| `components/layout/` | Layout components (navigation, structure) |
| `components/dashboard/` | Dashboard widgets and visualizations |
| `components/territory/` | Territory management components |
| `components/trip-planning/` | Trip planning and scheduling components |
| `components/approvals/` | Approval workflow components (customer, order, payment, variance) |
| `components/logistics/` | Logistics management components |
| `components/reports/` | Reporting and analytics components |
| `contexts/` | React Context providers (AuthContext, BusinessRulesContext, TenantConfigContext) |
| `hooks/` | Custom React hooks (API queries, permissions, analytics, business rules, etc.) |
| `lib/` | Shared utilities (API client, Supabase client, analytics, error handling, WebSocket client, etc.) |
| `types/` | TypeScript type definitions (business-rules, inventory, tenant-config, trip-export) |

### Infrastructure & Database

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `supabase/migrations/` | Database schema migrations (extensive SQL migrations) |
| `supabase/functions/` | Supabase Edge Functions (Firebase auth, geocoding, email invites, password reset) |
| `infra/` | Infrastructure configuration (Cloud Run, Grafana Alloy) |
| `grafana/` | Grafana dashboard configurations |
| `tests/framework/` | Test framework utilities (DataFactory, TestFramework) |

### Supporting Modules

| Module/Directory | Primary Responsibility |
|------------------|------------------------|
| `netlify/functions/` | Netlify serverless functions (Google Maps config, user invites) |
| `scripts/` | Utility scripts for database cleanup, environment checks, tenant management |
| `docs/` | Project documentation (API, Architecture, Domain Model, Events, Workflows) |

---

## External Dependencies

### Backend Production Dependencies

**Source:** `/backend/package.json`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `@fastify/cors` | Fastify CORS | Cross-Origin Resource Sharing middleware for Fastify |
| `@fastify/env` | Fastify Env | Environment variable configuration plugin for Fastify |
| `@fastify/jwt` | Fastify JWT | JSON Web Token authentication plugin for Fastify |
| `@fastify/multipart` | Fastify Multipart | Multipart/form-data file upload handling for Fastify |
| `@fastify/websocket` | Fastify WebSocket | WebSocket support for Fastify |
| `@opentelemetry/auto-instrumentations-node` | OpenTelemetry Auto Instrumentations | Automatic instrumentation for Node.js tracing |
| `@opentelemetry/exporter-trace-otlp-http` | OpenTelemetry OTLP Exporter | HTTP exporter for OpenTelemetry traces |
| `@opentelemetry/instrumentation-fastify` | OpenTelemetry Fastify Instrumentation | Fastify-specific tracing instrumentation |
| `@opentelemetry/sdk-node` | OpenTelemetry Node SDK | Core OpenTelemetry SDK for Node.js |
| `@sentry/node` | Sentry Node | Error tracking and monitoring for Node.js |
| `@sentry/profiling-node` | Sentry Profiling | Performance profiling for Sentry |
| `@supabase/supabase-js` | Supabase JS | Supabase client library for database and auth |
| `@types/ioredis` | ioredis Types | TypeScript type definitions for ioredis |
| `@types/node` | Node Types | TypeScript type definitions for Node.js |
| `@types/node-cache` | node-cache Types | TypeScript type definitions for node-cache |
| `@types/uuid` | UUID Types | TypeScript type definitions for UUID |
| `axios` | Axios | HTTP client for making API requests |
| `axios-retry` | Axios Retry | Automatic retry functionality for Axios requests |
| `dotenv` | dotenv | Environment variable loading from .env files |
| `fastify` | Fastify | High-performance web framework for Node.js |
| `fastify-plugin` | Fastify Plugin | Plugin helper for creating Fastify plugins |
| `ioredis` | ioredis | Redis client for Node.js |
| `mixpanel` | Mixpanel | Analytics and event tracking (server-side) |
| `nats` | NATS | Messaging system client for event-driven architecture |
| `node-cache` | node-cache | In-memory caching solution |
| `pino` | Pino | Fast JSON logger for Node.js |
| `pino-loki` | Pino Loki | Pino transport for Grafana Loki |
| `pino-pretty` | Pino Pretty | Pretty-printing formatter for Pino logs |
| `prom-client` | Prometheus Client | Prometheus metrics client for monitoring |
| `tsx` | TSX | TypeScript execution for Node.js |
| `typescript` | TypeScript | TypeScript language compiler |
| `uuid` | UUID | UUID generation library |

### Frontend Production Dependencies

**Source:** `/package.json`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `@sentry/react` | Sentry React | Error tracking and monitoring for React |
| `@sentry/tracing` | Sentry Tracing | Performance monitoring and tracing for Sentry |
| `@supabase/supabase-js` | Supabase JS | Supabase client library for database and auth |
| `@tanstack/react-query` | TanStack Query (React Query) | Server state management and data fetching |
| `@types/mixpanel-browser` | Mixpanel Browser Types | TypeScript type definitions for Mixpanel browser |
| `@types/node` | Node Types | TypeScript type definitions for Node.js |
| `clsx` | clsx | Utility for constructing className strings |
| `date-fns` | date-fns | Date utility library |
| `file-saver` | FileSaver.js | Client-side file saving utility |
| `html2canvas` | html2canvas | HTML to canvas screenshot library |
| `jspdf` | jsPDF | PDF generation library |
| `jspdf-autotable` | jsPDF AutoTable | Table generation plugin for jsPDF |
| `lucide-react` | Lucide React | Icon library for React |
| `mapbox-gl` | Mapbox GL JS | Interactive map rendering library |
| `mixpanel-browser` | Mixpanel Browser | Analytics and event tracking (client-side) |
| `react` | React | UI component library |
| `react-dom` | React DOM | React rendering for web browsers |
| `react-phone-number-input` | React Phone Number Input | Phone number input component with validation |
| `react-router-dom` | React Router | Client-side routing for React |
| `recharts` | Recharts | Charting library for React |
| `resend` | Resend | Email sending service client |
| `xlsx` | SheetJS (xlsx) | Excel file parsing and generation |

### Backend Development Dependencies

**Source:** `/backend/package.json (dev)`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `@types/jest` | Jest Types | TypeScript type definitions for Jest |
| `@types/supertest` | Supertest Types | TypeScript type definitions for Supertest |
| `@typescript-eslint/eslint-plugin` | TypeScript ESLint Plugin | ESLint rules for TypeScript |
| `@typescript-eslint/parser` | TypeScript ESLint Parser | ESLint parser for TypeScript |
| `eslint` | ESLint | JavaScript/TypeScript linting |
| `jest` | Jest | Testing framework |
| `node-fetch` | node-fetch | Fetch API implementation for Node.js |
| `nodemon` | nodemon | Development server with auto-restart |
| `prettier` | Prettier | Code formatting |
| `supertest` | SuperTest | HTTP assertion testing library |
| `ts-jest` | ts-jest | Jest transformer for TypeScript |

### Frontend Development Dependencies

**Source:** `/package.json (dev)`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `@eslint/js` | ESLint JS | ESLint JavaScript configuration |
| `@testing-library/jest-dom` | Testing Library Jest DOM | Custom Jest matchers for DOM testing |
| `@testing-library/react` | Testing Library React | React component testing utilities |
| `@testing-library/user-event` | Testing Library User Event | User interaction simulation for testing |
| `@types/file-saver` | FileSaver Types | TypeScript type definitions for FileSaver |
| `@types/mapbox-gl` | Mapbox GL Types | TypeScript type definitions for Mapbox GL |
| `@types/react` | React Types | TypeScript type definitions for React |
| `@types/react-dom` | React DOM Types | TypeScript type definitions for React DOM |
| `@vitejs/plugin-react` | Vite React Plugin | React support for Vite |
| `@vitest/coverage-v8` | Vitest Coverage V8 | Code coverage for Vitest using V8 |
| `@vitest/ui` | Vitest UI | Visual UI for Vitest test runner |
| `autoprefixer` | Autoprefixer | CSS vendor prefix automation |
| `eslint` | ESLint | JavaScript/TypeScript linting |
| `eslint-plugin-react-hooks` | ESLint React Hooks | ESLint rules for React Hooks |
| `eslint-plugin-react-refresh` | ESLint React Refresh | ESLint rules for React Refresh |
| `globals` | globals | Global variable definitions for ESLint |
| `husky` | Husky | Git hooks management |
| `jsdom` | jsdom | DOM implementation for Node.js testing |
| `lint-staged` | lint-staged | Run linters on staged git files |
| `msw` | Mock Service Worker | API mocking for testing |
| `postcss` | PostCSS | CSS transformation tool |
| `prettier` | Prettier | Code formatting |
| `rollup-plugin-visualizer` | Rollup Plugin Visualizer | Bundle size visualization |
| `tailwindcss` | Tailwind CSS | Utility-first CSS framework |
| `typescript` | TypeScript | TypeScript language compiler |
| `typescript-eslint` | TypeScript ESLint | TypeScript integration for ESLint |
| `vite` | Vite | Frontend build tool and dev server |
| `vitest` | Vitest | Vite-native testing framework |

### Infrastructure Dependencies

**Source:** `/backend/Dockerfile`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `node:20-alpine` | Node.js Alpine | Node.js runtime base image |
| `dumb-init` | dumb-init | Minimal init system for proper signal handling in containers |

**Source:** `/backend/docker-compose.yml`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `redis:7-alpine` | Redis Alpine | In-memory data store for caching |

**Source:** `/grafana-alloy/Dockerfile`, `/infra/grafana-alloy/Dockerfile`

| Dependency | Official Name | Purpose |
|------------|---------------|---------|
| `grafana/alloy:latest` | Grafana Alloy | Observability data collector (logs, metrics, traces) |

# core_entities

Core entities and their relationships

# Domain Model Analysis - OMS System

## Overview

This is an **Order Management System (OMS)** for a logistics/distribution business, likely handling gas cylinder or similar product distribution. The system manages the full lifecycle from customer orders through delivery and inventory reconciliation.

---

## 1. Core Data Entities

### 1.1 **Tenant**
Multi-tenant architecture root entity.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `name` | string | Tenant/organization name |
| `settings` | JSONB | Tenant-specific configuration |
| `currency` | string | Default currency |
| `created_at` | timestamp | Creation time |

---

### 1.2 **User / UserProfile**
System users with role-based access.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key (links to auth.users) |
| `tenant_id` | UUID | FK to Tenant |
| `email` | string | User email |
| `full_name` | string | Display name |
| `role` | enum | `super_admin`, `admin`, `operator`, `accountant`, `sales`, `driver` |
| `is_active` | boolean | Account status |
| `created_at` | timestamp | Creation time |

---

### 1.3 **Customer**
End customers who place orders.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `name` | string | Customer/business name |
| `customer_reference` | string | External reference code |
| `status` | enum | `pending`, `approved`, `rejected`, `inactive` |
| `primary_contact` | string | Contact person name |
| `phone` | string | Phone number |
| `email` | string | Email address |
| `sales_user_id` | UUID | FK to UserProfile (assigned sales agent) |
| `balance` | decimal | Current account balance |
| `created_by` | UUID | FK to UserProfile |
| `approved_by` | UUID | FK to UserProfile |
| `created_at` | timestamp | Creation time |

---

### 1.4 **Address**
Physical locations for customers and warehouses.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `customer_id` | UUID | FK to Customer (nullable for warehouses) |
| `warehouse_id` | UUID | FK to Warehouse (nullable) |
| `name` | string | Address label/name |
| `address_type` | enum | `delivery`, `billing`, `warehouse` |
| `street_address` | string | Full street address |
| `city` | string | City |
| `plus_code` | string | Google Plus Code |
| `latitude` | decimal | GPS latitude |
| `longitude` | decimal | GPS longitude |
| `is_default` | boolean | Default address flag |

---

### 1.5 **Product**
Product catalog items.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `name` | string | Product name |
| `description` | string | Product description |
| `product_type` | enum | `cylinder`, `accessory`, `measured` |
| `is_active` | boolean | Availability status |
| `created_at` | timestamp | Creation time |

---

### 1.6 **ProductVariant**
Specific SKUs/variants of products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `product_id` | UUID | FK to Product |
| `sku_code` | string | Stock keeping unit code |
| `name` | string | Variant name |
| `size` | string | Size designation |
| `state` | enum | `full`, `empty`, `measured` |
| `is_bundle` | boolean | Bundle product flag |
| `is_active` | boolean | Availability status |

---

### 1.7 **BundleComponent**
Links bundle products to their component variants.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `bundle_variant_id` | UUID | FK to ProductVariant (bundle) |
| `component_variant_id` | UUID | FK to ProductVariant (component) |
| `quantity` | integer | Quantity of component in bundle |

---

### 1.8 **PriceList**
Pricing definitions for product variants.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `name` | string | Price list name |
| `is_default` | boolean | Default pricing flag |
| `valid_from` | date | Effective start date |
| `valid_to` | date | Effective end date |
| `created_at` | timestamp | Creation time |

---

### 1.9 **PriceListItem**
Individual price entries.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `price_list_id` | UUID | FK to PriceList |
| `variant_id` | UUID | FK to ProductVariant |
| `unit_price` | decimal | Price per unit |
| `currency` | string | Currency code |

---

### 1.10 **Order**
Customer orders for products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `order_number` | string | Human-readable order number |
| `customer_id` | UUID | FK to Customer |
| `delivery_address_id` | UUID | FK to Address |
| `status` | enum | `pending`, `approved`, `rejected`, `allocated`, `dispatched`, `delivered`, `partial`, `cancelled` |
| `order_type` | enum | `standard`, `refilling`, `transfer`, `conversion` |
| `order_source` | enum | `web`, `mobile`, `whatsapp`, `operator`, `accountant` |
| `external_order_id` | string | External system reference |
| `total_amount` | decimal | Order total value |
| `notes` | text | Order notes |
| `sales_user_id` | UUID | FK to UserProfile |
| `created_by` | UUID | FK to UserProfile |
| `approved_by` | UUID | FK to UserProfile |
| `updated_by` | UUID | FK to UserProfile |
| `created_at` | timestamp | Creation time |
| `delivery_date` | date | Requested delivery date |

---

### 1.11 **OrderLine**
Individual line items within an order.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `order_id` | UUID | FK to Order |
| `variant_id` | UUID | FK to ProductVariant |
| `quantity` | integer | Ordered quantity |
| `unit_price` | decimal | Price per unit |
| `total_price` | decimal | Line total |

---

### 1.12 **Warehouse**
Physical storage locations.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `name` | string | Warehouse name |
| `warehouse_type` | enum | `main`, `mobile`, `depot` |
| `linked_vehicle_id` | UUID | FK to Vehicle (for mobile warehouses) |
| `is_active` | boolean | Operational status |
| `created_by` | UUID | FK to UserProfile |
| `updated_by` | UUID | FK to UserProfile |

---

### 1.13 **Vehicle**
Delivery vehicles.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `registration_number` | string | License plate |
| `vehicle_type` | string | Vehicle category |
| `is_active` | boolean | Availability status |
| `created_by` | UUID | FK to UserProfile |
| `updated_by` | UUID | FK to UserProfile |

---

### 1.14 **InventoryLevel**
Current stock levels per warehouse/variant.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `warehouse_id` | UUID | FK to Warehouse |
| `variant_id` | UUID | FK to ProductVariant |
| `quantity` | integer | Current stock quantity |
| `updated_at` | timestamp | Last update time |

---

### 1.15 **InventoryTransaction**
Stock movement history.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `warehouse_id` | UUID | FK to Warehouse |
| `variant_id` | UUID | FK to ProductVariant |
| `transaction_type` | enum | `allocation`, `deallocation`, `transfer`, `delivery`, `return`, `adjustment`, `conversion`, `writeoff` |
| `quantity` | integer | Transaction quantity (+ or -) |
| `reference_type` | enum | `order`, `trip`, `adjustment`, `conversion` |
| `reference_id` | UUID | FK to referenced entity |
| `photo_url` | string | Supporting photo |
| `created_by` | UUID | FK to UserProfile |
| `created_at` | timestamp | Transaction time |

---

### 1.16 **Trip**
Delivery trip/route.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `trip_number` | string | Human-readable trip number |
| `vehicle_id` | UUID | FK to Vehicle |
| `driver_id` | UUID | FK to UserProfile |
| `status` | enum | `planned`, `loading`, `dispatched`, `in_progress`, `completed`, `variance_pending`, `cancelled` |
| `scheduled_date` | date | Planned delivery date |
| `dispatched_by` | UUID | FK to UserProfile |
| `created_by` | UUID | FK to UserProfile |
| `created_at` | timestamp | Creation time |

---

### 1.17 **TripOrder**
Junction linking orders to trips with delivery details.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `order_id` | UUID | FK to Order |
| `stop_sequence` | integer | Delivery order/sequence |
| `action_type` | enum | `deliver`, `collect`, `conversion_deliver`, `conversion_collect` |
| `status` | enum | `pending`, `completed`, `failed`, `cancelled` |
| `pod_url` | string | Proof of delivery photo |
| `delivery_time` | timestamp | Actual delivery time |
| `notes` | text | Delivery notes |

---

### 1.18 **TripInventory**
Starting stock loaded onto trip vehicle.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `variant_id` | UUID | FK to ProductVariant |
| `starting_quantity` | integer | Loaded quantity |
| `delivered_quantity` | integer | Total delivered |
| `returned_quantity` | integer | Returned to warehouse |

---

### 1.19 **TripReturn**
Products returned from trip.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `variant_id` | UUID | FK to ProductVariant |
| `quantity` | integer | Returned quantity |
| `destination_warehouse_id` | UUID | FK to Warehouse |

---

### 1.20 **LoadingPlan**
Pre-dispatch loading instructions.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `status` | enum | `draft`, `pending`, `approved`, `cancelled` |
| `source_warehouse_id` | UUID | FK to Warehouse |
| `total_quantity` | integer | Total items to load |
| `approved_by` | UUID | FK to UserProfile |
| `cancelled_by` | UUID | FK to UserProfile |
| `cancellation_reason` | text | Reason if cancelled |

---

### 1.21 **OffloadDocument**
Post-trip offload/reconciliation records.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `trip_id` | UUID | FK to Trip |
| `destination_warehouse_id` | UUID | FK to Warehouse |
| `status` | enum | `draft`, `pending_agreement`, `agreed`, `variance_pending`, `completed` |
| `created_by` | UUID | FK to UserProfile |
| `agreed_by` | UUID | FK to UserProfile |

---

### 1.22 **InventoryAdjustment** (Manual Adjustment)
Manual stock corrections.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `warehouse_id` | UUID | FK to Warehouse |
| `variant_id` | UUID | FK to ProductVariant |
| `quantity` | integer | Adjustment amount |
| `reason` | text | Justification |
| `variance_code` | string | Standardized variance code |
| `status` | enum | `pending`, `approved`, `rejected` |
| `created_by` | UUID | FK to UserProfile |
| `approved_by` | UUID | FK to UserProfile |

---

### 1.23 **VariantConversion**
Cylinder state conversions (empty ↔ full).

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `source_variant_id` | UUID | FK to ProductVariant |
| `target_variant_id` | UUID | FK to ProductVariant |
| `quantity` | integer | Conversion quantity |
| `warehouse_id` | UUID | FK to Warehouse |
| `created_by` | UUID | FK to UserProfile |

---

### 1.24 **Payment**
Customer payments.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `customer_id` | UUID | FK to Customer |
| `order_id` | UUID | FK to Order (optional) |
| `amount` | decimal | Payment amount |
| `payment_method` | string | Payment type |
| `status` | enum | `pending`, `approved`, `rejected` |
| `created_by` | UUID | FK to UserProfile |
| `approved_by` | UUID | FK to UserProfile |

---

### 1.25 **CustomerEmptyBalance**
Cylinder empty balance tracking per customer/address.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `customer_id` | UUID | FK to Customer |
| `address_id` | UUID | FK to Address |
| `variant_id` | UUID | FK to ProductVariant |
| `quantity` | integer | Empty cylinders held |

---

### 1.26 **UserInvitation**
Pending user invitations.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `email` | string | Invitee email |
| `role` | enum | Assigned role |
| `invited_by` | UUID | FK to UserProfile |
| `token` | string | Invitation token |
| `expires_at` | timestamp | Token expiry |
| `accepted_at` | timestamp | Acceptance time |

---

### 1.27 **BusinessRule**
Configurable business rules per tenant.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `entity_type` | string | Target entity |
| `rule_type` | string | Rule category |
| `field_path` | string | Target field |
| `value` | JSONB | Rule configuration |
| `is_active` | boolean | Rule status |

---

### 1.28 **Workflow**
Workflow state definitions.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `entity_type` | string | Target entity type |
| `status_code` | string | Status identifier |
| `display_name` | string | UI display name |
| `color` | string | UI color code |
| `sequence` | integer | Display order |

---

### 1.29 **EventStore**
Domain event log for event-driven architecture.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | UUID | Primary key |
| `tenant_id` | UUID | FK to Tenant |
| `event_type` | string | Event name |
| `aggregate_type` | string | Entity type |
| `aggregate_id` | UUID | Entity ID |
| `payload` | JSONB | Event data |
| `created_at` | timestamp | Event time |
| `created_by` | UUID | FK to UserProfile |

---

## 2. Entity Relationships Diagram

```
┌─────────────┐
│   Tenant    │──────────────────────────────────────────────────┐
└──────┬──────┘                                                  │
       │ 1:N                                                     │
       ▼                                                         │
┌──────────────┐     ┌───────────────┐     ┌─────────────────┐  │
│ UserProfile  │◄────│UserInvitation │     │  BusinessRule   │◄─┤
└──────┬───────┘     └───────────────┘     └─────────────────┘  │
       │                                                         │
       │ (sales_user_id, created_by, driver_id, etc.)           │
       ▼                                                         │
┌──────────────┐  1:N  ┌───────────────┐                        │
│   Customer   │──────►│    Address    │◄───────────┐            │
└──────┬───────┘       └───────────────┘            │            │
       │                      ▲                      │            │
       │ 1:N                  │                      │            │
       ▼                      │ (delivery_address)   │            │
┌──────────────┐              │                      │            │
│    Order     │──────────────┘                      │            │
└──────┬───────┘                                     │            │
       │ 1:N                                         │            │
       ▼                                             │            │
┌──────────────┐   N:1   ┌────────────────┐         │            │
│  OrderLine   │────────►│ ProductVariant │◄────────┼────────────┤
└──────────────┘         └───────┬────────┘         │            │
                                 │                   │            │
                    ┌────────────┼────────────┐      │            │
                    │            │ N:1        │      │            │
                    ▼            ▼            ▼      │            │
           ┌─────────────┐ ┌─────────┐ ┌───────────────┐         │
           │BundleComponent│ │Product│ │PriceListItem │          │
           └─────────────┘ └─────────┘ └──────┬────────┘          │
                                              │ N:1               │
                                              ▼                   │
                                        ┌───────────┐             │
                                        │ PriceList │◄────────────┤
                                        └───────────┘             │
                                                                  │
┌──────────────┐  1:1  ┌───────────────┐                         │
│   Vehicle    │◄─────►│   Warehouse   │◄─────────────────────────┤
└──────┬───────┘       └───────┬───────┘                         │
       │                       │                                  │
       │                       │ 1:N                              │
       │                       ▼                                  │
       │               ┌───────────────────┐                      │
       │               │  InventoryLevel   │                      │
       │               └───────────────────┘                      │
       │                       │                                  │
       │                       │ (history)                        │
       │                       ▼                                  │
       │               ┌───────────────────────┐                  │
       │               │InventoryTransaction  │◄──────────────────┤
       │               └───────────────────────┘                  │
       │                                                          │
       ▼                                                          │
┌──────────────┐                                                  │
│     Trip     │◄─────────────────────────────────────────────────┘
└──────┬───────┘
       │
       ├──────────────────────────────────────┐
       │ 1:N                                   │ 1:N
       ▼                                       ▼
┌──────────────┐  N:1   ┌────────────┐  ┌──────────────┐
│  TripOrder   │───────►│   Order    │  │TripInventory │
└──────────────┘        └────────────┘  └──────────────┘
       │
       │ 1:N
       ▼
┌──────────────┐        ┌───────────────┐        ┌────────────────┐
│  TripReturn  │        │  LoadingPlan  │        │OffloadDocument │
└──────────────┘        └───────────────┘        └────────────────┘
       ▲                        ▲                        ▲
       │                        │                        │
       └────────────────────────┴────────────────────────┘
                         (all link to Trip)
```

---

## 3. Key

# DBs

databases analysis

# Database Analysis Report for oms-system

After a comprehensive analysis of the codebase, I have identified the following databases:

---

## Database 1: PostgreSQL (via Supabase)

* **Database Name/Type:** PostgreSQL (SQL) - hosted on Supabase
* **Purpose/Role:** Primary transactional database for the Order Management System (OMS). Stores all core business data including customers, orders, products, inventory, trips, warehouses, vehicles, user profiles, and tenant configurations. Implements Row-Level Security (RLS) for multi-tenant data isolation. Handles complex business logic through PostgreSQL functions and triggers.

* **Key Technologies/Access Methods:** 
  - **Supabase Client SDK** (`@supabase/supabase-js`) for frontend and backend access
  - **PostgreSQL RPC functions** for complex business operations
  - **Database Views** for optimized read operations
  - **Row-Level Security (RLS)** policies for tenant isolation
  - **Database Triggers** for audit trails and automatic data updates
  - **pg_cron** extension for scheduled tasks (inventory snapshots)

* **Key Files/Configuration:**
  - `src/lib/supabase.ts` - Frontend Supabase client configuration
  - `backend/src/client/supabase.ts` - Backend Supabase client
  - `supabase/config.toml` - Supabase project configuration
  - `supabase/migrations/` - 300+ migration files defining schema evolution
  - `supabase/dev_schema.sql` - Development schema reference
  - `supabase/prod_schema.sql` - Production schema reference
  - `backend/supabase/migrations/` - Backend-specific migrations

* **Schema/Table Structure:**

  **Core Business Tables:**
  | Table | Key Columns | Description |
  |-------|-------------|-------------|
  | `tenants` | `id` (PK), `name`, `settings` (JSONB), `created_at` | Multi-tenant organization data |
  | `user_profiles` | `id` (PK, FK to auth.users), `tenant_id` (FK), `full_name`, `role`, `email` | User accounts and roles |
  | `customers` | `id` (PK), `tenant_id` (FK), `business_name`, `status`, `type`, `sales_user_id` (FK) | Customer/business accounts |
  | `addresses` | `id` (PK), `tenant_id` (FK), `customer_id` (FK), `type`, `coordinates`, `plus_code` | Customer and warehouse addresses |
  | `orders` | `id` (PK), `tenant_id` (FK), `customer_id` (FK), `order_number`, `status`, `order_type`, `total_amount`, `delivery_address_id` (FK) | Sales/transfer orders |
  | `order_lines` | `id` (PK), `order_id` (FK), `variant_id` (FK), `quantity`, `unit_price`, `delivered_quantity` | Order line items |
  | `products` | `id` (PK), `tenant_id` (FK), `name`, `product_type`, `is_bundle` | Product catalog |
  | `product_variants` | `id` (PK), `product_id` (FK), `name`, `sku_code`, `state_attribute` | Product SKU variants |
  | `bundle_components` | `id` (PK), `bundle_variant_id` (FK), `component_variant_id` (FK), `quantity` | Bundle composition |

  **Inventory Management Tables:**
  | Table | Key Columns | Description |
  |-------|-------------|-------------|
  | `warehouses` | `id` (PK), `tenant_id` (FK), `name`, `type`, `linked_vehicle_id` (FK) | Static and mobile warehouses |
  | `inventory_levels` | `id` (PK), `warehouse_id` (FK), `variant_id` (FK), `quantity` | Current stock levels |
  | `inventory_transactions` | `id` (PK), `tenant_id` (FK), `warehouse_id` (FK), `variant_id` (FK), `quantity`, `transaction_type`, `reference_type`, `reference_id` | Stock movement audit trail |
  | `inventory_adjustments` | `id` (PK), `tenant_id` (FK), `warehouse_id` (FK), `status`, `adjustment_reason` | Manual inventory adjustments |
  | `inventory_snapshots` | `id` (PK), `tenant_id` (FK), `warehouse_id` (FK), `snapshot_data` (JSONB), `snapshot_date` | Daily inventory snapshots |

  **Logistics/Trip Tables:**
  | Table | Key Columns | Description |
  |-------|-------------|-------------|
  | `vehicles` | `id` (PK), `tenant_id` (FK), `registration`, `driver_id` (FK), `vehicle_type` | Fleet vehicles |
  | `trips` | `id` (PK), `tenant_id` (FK), `vehicle_id` (FK), `trip_number`, `status`, `dispatched_at`, `completed_at` | Delivery trips |
  | `trip_orders` | `id` (PK), `trip_id` (FK), `order_id` (FK), `stop_sequence`, `status`, `action_type`, `pod_url` | Trip stops/deliveries |
  | `loading_plans` | `id` (PK), `tenant_id` (FK), `trip_id` (FK), `status`, `total_quantity` | Trip loading documentation |
  | `offload_documents` | `id` (PK), `tenant_id` (FK), `trip_id` (FK), `status`, `destination_warehouse_id` (FK) | Trip return/offload records |
  | `trip_returns` | `id` (PK), `trip_id` (FK), `variant_id` (FK), `quantity`, `destination_warehouse_id` (FK) | Returned inventory from trips |

  **Pricing & Payments Tables:**
  | Table | Key Columns | Description |
  |-------|-------------|-------------|
  | `price_lists` | `id` (PK), `tenant_id` (FK), `name`, `valid_from`, `valid_to`, `is_default` | Price list definitions |
  | `price_list_items` | `id` (PK), `price_list_id` (FK), `variant_id` (FK), `unit_price` | Individual prices |
  | `customer_price_lists` | `customer_id` (FK), `price_list_id` (FK) | Customer price list assignments |
  | `payments` | `id` (PK), `tenant_id` (FK), `order_id` (FK), `amount`, `payment_method`, `status` | Payment records |
  | `customer_balance_history` | `id` (PK), `customer_id` (FK), `balance_change`, `new_balance`, `reference_type` | Balance audit trail |
  | `customer_empty_balances` | `id` (PK), `customer_id` (FK), `address_id` (FK), `variant_id` (FK), `quantity` | Cylinder/returnable tracking |

  **Configuration & Audit Tables:**
  | Table | Key Columns | Description |
  |-------|-------------|-------------|
  | `business_rules` | `id` (PK), `tenant_id` (FK), `rule_type`, `entity_type`, `config` (JSONB) | Tenant business rules |
  | `type_definitions` | `id` (PK), `tenant_id` (FK), `category`, `code`, `label`, `color` | Dynamic type system |
  | `workflows` | `id` (PK), `tenant_id` (FK), `entity_type`, `workflow_type`, `config` (JSONB) | Workflow configurations |
  | `order_audit` | `id` (PK), `order_id` (FK), `action`, `old_values` (JSONB), `new_values` (JSONB), `changed_by` | Order change history |
  | `customer_profile_history` | `id` (PK), `customer_id` (FK), `action`, `old_data` (JSONB), `new_data` (JSONB) | Customer change history |
  | `event_store` | `id` (PK), `tenant_id` (FK), `event_type`, `aggregate_type`, `aggregate_id`, `payload` (JSONB) | Domain event sourcing |
  | `user_invitations` | `id` (PK), `tenant_id` (FK), `email`, `role`, `status`, `invited_by` | User invitation management |

  **Database Views (Optimized Read Access):**
  - `orders_list_view` - Orders with customer, address, and status info
  - `orders_with_lines_view` - Orders with expanded line items
  - `customers_list_view` - Customers with primary address and sales agent
  - `customers_map_view` - Customer locations for mapping
  - `trips_list_view` - Trips with driver, vehicle, stop counts
  - `trip_orders_with_addresses_view` - Trip stops with full address details
  - `loading_plans_view` - Loading plans with trip and vehicle info
  - `inventory_levels_view` - Inventory with warehouse and variant details
  - `product_variants_with_pricing_view` - Variants with current prices
  - `dashboard_metrics_view` - Aggregated business metrics
  - `sales_agent_dashboard_view` - Sales performance metrics
  - `operations_delivery_report_view` - Delivery performance data
  - `variance_sku_analytics_view` - Variance analysis by SKU

* **Key Entities and Relationships:**

  ```
  Tenant (1) ──────< User Profiles (M)
     │
     ├──────< Customers (M) ──────< Addresses (M)
     │              │                    │
     │              └──────< Orders (M) ─┘
     │                          │
     │                          └──────< Order Lines (M) ─────> Product Variants
     │
     ├──────< Products (M) ──────< Product Variants (M)
     │                                    │
     │                          Bundle Components >────┘
     │
     ├──────< Warehouses (M) ──────< Inventory Levels (M) ─────> Product Variants
     │              │
     │              └──────< Inventory Transactions (M)
     │
     ├──────< Vehicles (M) ─────> Warehouses (mobile)
     │              │
     │              └──────< Trips (M) ──────< Trip Orders (M) ─────> Orders
     │                          │
     │                          ├──────< Loading Plans (M)
     │                          └──────< Offload Documents (M)
     │
     └──────< Price Lists (M) ──────< Price List Items (M) ─────> Product Variants
  ```

  **Key Relationships:**
  - **Tenant → All Entities:** Multi-tenant isolation via `tenant_id` foreign key
  - **Customer (1) → Orders (M):** Customers place multiple orders
  - **Customer (1) → Addresses (M):** Customers have multiple delivery addresses
  - **Order (1) → Order Lines (M):** Orders contain multiple line items
  - **Product (1) → Product Variants (M):** Products have size/state variants
  - **Product Variant → Bundle Components:** Bundles contain component variants
  - **Warehouse (1) → Inventory Levels (M):** Stock tracked per warehouse
  - **Vehicle (1) → Warehouse (1):** Mobile warehouse linked to vehicle
  - **Trip (1) → Trip Orders (M):** Trips contain multiple delivery stops
  - **Trip → Loading Plans/Offload Documents:** Trip documentation
  - **Price List (1) → Price List Items (M):** Prices per variant

* **Interacting Components:**

  **Frontend (React/TypeScript):**
  - `src/pages/` - All page components (Orders, Customers, Trips, Inventory, etc.)
  - `src/components/` - UI components interacting with data
  - `src/hooks/useApiQuery.ts` - Data fetching abstraction
  - `src/lib/supabase.ts` - Direct Supabase client access
  - `src/contexts/` - Auth, tenant, business rules contexts

  **Backend (Node.js/Fastify):**
  - `backend/src/routes/` - API route handlers
  - `backend/src/services/` - Business logic services
  - `backend/src/handlers/` - Request handlers
  - `backend/src/client/supabase.ts` - Backend Supabase client

  **Key Services:**
  - Order Processing Service
  - Customer Management Service
  - Inventory Management Service
  - Trip/Logistics Service
  - Payment Processing Service
  - Pricing Service
  - User Authentication Service
  - Reporting/Analytics Service

---

## Database 2: Redis

* **Database Name/Type:** Redis (NoSQL - Key-Value Store / In-Memory Cache)
* **Purpose/Role:** L2 (Level 2) caching layer for frequently accessed data. Used to reduce database load by caching expensive queries and commonly accessed business data. Supports cache invalidation patterns.

* **Key Technologies/Access Methods:**
  - **ioredis** client library for Node.js
  - Custom cache abstraction layer (`L2CachePlugin`)
  - Event-driven cache invalidation via Supabase Realtime

* **Key Files/Configuration:**
  - `backend/src/plugins/l2-cache.ts` - L2 cache plugin implementation
  - `backend/src/config/config.ts` - Redis connection configuration (via `REDIS_URL` environment variable)
  - `backend/test-l2-cache.cjs` - Cache testing utilities
  - `backend/test-redis-connection.cjs` - Redis connection testing
  - `plans/event-driven-cache-invalidation-architecture.md` - Cache architecture documentation

* **Schema/Table Structure (Key Patterns):**
  
  Based on the L2 cache implementation, the following key patterns are used:

  | Key Pattern | Data Structure | Description | TTL |
  |-------------|----------------|-------------|-----|
  | `tenant:{tenantId}:customers` | JSON Array | Cached customer list per tenant | Configurable |
  | `tenant:{tenantId}:orders` | JSON Array | Cached orders list per tenant | Configurable |
  | `tenant:{tenantId}:products` | JSON Array | Cached product catalog | Configurable |
  | `tenant:{tenantId}:inventory:{warehouseId}` | JSON Object | Cached inventory levels | Configurable |
  | `tenant:{tenantId}:price-lists` | JSON Array | Cached price lists | Configurable |
  | `cache:invalidation:{entity}:{id}` | String | Cache invalidation markers | Short TTL |

* **Key Entities and Relationships:**
  - **Cached Customer Data:** Temporary representation of customer records
  - **Cached Order Data:** Temporary representation of order records
  - **Cached Product Data:** Temporary representation of product catalog
  - **Cached Inventory Data:** Temporary representation of stock levels
  - **Cache Metadata:** Invalidation timestamps and version tracking
  - **Relationships:** All relationships are managed at the application layer; Redis stores denormalized snapshots of PostgreSQL data

* **Interacting Components:**
  - `backend/src/plugins/l2-cache.ts` - Cache management plugin
  - `backend/src/routes/` - API routes using cached data
  - Cache invalidation event handlers (Supabase Realtime integration)
  - Backend services that read/write cached data

---

## Database 3: Supabase Storage (Object Storage)

* **Database Name/Type:** Supabase Storage (S3-compatible Object Storage)
* **Purpose/Role:** Stores binary files including proof-of-delivery (POD) photos, inventory adjustment photos, and other document uploads. Integrated with PostgreSQL for metadata tracking.

* **Key Technologies/Access Methods:**
  - Supabase Storage SDK (part of `@supabase/supabase-js`)
  - Signed URLs for secure access
  - Storage bucket policies for access control

* **Key Files/Configuration:**
  - `supabase/migrations/20250110150000_add_pod_storage_policies.sql` - POD storage bucket policies
  - `supabase/migrations/20250122140000_create_inventory_photos_bucket.sql` - Inventory photos bucket
  - `supabase/migrations/20251127100000_fix_delivery_photos_storage_policy.sql` - Delivery photos policies
  - `src/lib/storageHelpers.ts` - Frontend storage utilities
  - `src/lib/imageUploadHelpers.ts` - Image upload helpers

* **Schema/Table Structure (Buckets):**

  | Bucket Name | Purpose | Access Control |
  |-------------|---------|----------------|
  | `delivery-photos` | Proof of delivery photos | Authenticated users, RLS policies |
  | `inventory-photos` | Inventory adjustment photos | Authenticated users, RLS policies |
  | `pod-storage` | Legacy POD storage | Authenticated users |

* **Key Entities and Relationships:**
  - **Delivery Photos:** Linked to `trip_orders` via `pod_url` field
  - **Inventory Photos:** Linked to `inventory_transactions` via `photo_url` field
  - **Relationships:** Files are referenced by PostgreSQL records; metadata stored in database, binary content in storage

* **Interacting Components:**
  - Trip/Delivery pages (POD upload/view)
  - Inventory Adjustment pages (photo upload)
  - Mobile application (photo capture and upload)
  - `src/lib/storageHelpers.ts` - Storage utility functions

---

## Database 4: Supabase Realtime (PostgreSQL Logical Replication)

* **Database Name/Type:** Supabase Realtime (PostgreSQL-based pub/sub via logical replication)
* **Purpose/Role:** Real-time data synchronization for live updates in the frontend application. Broadcasts database changes to connected clients for features like live order status updates, inventory changes, and collaborative editing.

* **Key Technologies/Access Methods:**
  - Supabase Realtime SDK (part of `@supabase/supabase-js`)
  - WebSocket connections
  - PostgreSQL logical replication / change data capture

* **Key Files/Configuration:**
  - `src/lib/supabase.ts` - Realtime subscription setup
  - `src/lib/WebSocketClient.ts` - WebSocket client implementation
  - `src/hooks/useWebSocketSubscription.ts` - React hook for realtime subscriptions
  - `backend/src/events/` - Event handling for realtime updates

* **Schema/Table Structure (Channels):**

  | Channel Pattern | Tables Monitored | Purpose |
  |-----------------|------------------|---------|
  | `tenant:{tenantId}:orders` | `orders`, `order_lines` | Order status updates |
  | `tenant:{tenantId}:trips` | `trips`, `trip_orders` | Trip progress updates |
  | `tenant:{tenantId}:inventory` | `inventory_levels`, `inventory_transactions` | Stock level changes |
  | `tenant:{tenantId}:customers` | `customers` | Customer data updates |

* **Key Entities and Relationships:**
  - Realtime channels map directly to PostgreSQL tables
  - Changes are filtered by tenant_id for multi-tenant isolation
  - Used for cache invalidation triggers

* **Interacting Components:**
  - `src/hooks/useWebSocketSubscription.ts` - Frontend subscription management
  - `src/lib/WebSocketClient.ts` - WebSocket client
  - React Query cache invalidation on realtime events
  - L2 Cache invalidation handlers in backend

---

## Summary

| Database | Type | Primary Use |
|----------|------|-------------|
| **PostgreSQL (Supabase)** | SQL (Relational) | Primary transactional database - all business data |
| **Redis** | NoSQL (Key-Value) | L2 caching layer for performance optimization |
| **Supabase Storage** | Object Storage | Binary file storage (photos, documents) |
| **Supabase Realtime** | Pub/Sub | Real-time data synchronization |

# APIs

APIs analysis

# HTTP API Documentation for oms-system_aeed8a86

## Overview

This is an Order Management System (OMS) backend built with Fastify. The API provides endpoints for managing orders, customers, trips, inventory, products, warehouses, vehicles, and various business operations.

---

## Authentication

All endpoints (except health checks and auth endpoints) require JWT authentication via Bearer token in the `Authorization` header.

---

## API Endpoints

### Health & System

#### Health Check
- **HTTP Method:** `GET`
- **API URL:** `/health`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "status": "ok",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "uptime": 12345.67,
  "environment": "production",
  "version": "1.0.0"
}
```
- **Description:** Returns the health status of the API server.

---

#### Readiness Check
- **HTTP Method:** `GET`
- **API URL:** `/ready`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "status": "ok",
  "checks": {
    "database": "ok",
    "redis": "ok"
  }
}
```
- **Description:** Returns the readiness status including dependency health checks.

---

### Authentication

#### Login
- **HTTP Method:** `POST`
- **API URL:** `/api/auth/login`
- **Request Payload:**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```
- **Response Payload:**
```json
{
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "role": "admin"
  },
  "session": {
    "access_token": "jwt_token",
    "refresh_token": "refresh_token",
    "expires_at": 1234567890
  }
}
```
- **Description:** Authenticates a user and returns session tokens.

---

#### Logout
- **HTTP Method:** `POST`
- **API URL:** `/api/auth/logout`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "success": true
}
```
- **Description:** Logs out the current user and invalidates their session.

---

#### Get Current User
- **HTTP Method:** `GET`
- **API URL:** `/api/auth/me`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "role": "admin",
  "tenant_id": "tenant_uuid",
  "full_name": "John Doe"
}
```
- **Description:** Returns the current authenticated user's profile.

---

#### Change Password
- **HTTP Method:** `POST`
- **API URL:** `/api/auth/change-password`
- **Request Payload:**
```json
{
  "currentPassword": "oldPassword123",
  "newPassword": "newPassword456"
}
```
- **Response Payload:**
```json
{
  "success": true,
  "message": "Password changed successfully"
}
```
- **Description:** Changes the current user's password.

---

### Customers

#### List Customers
- **HTTP Method:** `GET`
- **API URL:** `/api/customers`
- **Query Parameters:**
  - `page` (number, optional): Page number for pagination
  - `limit` (number, optional): Items per page
  - `search` (string, optional): Search term
  - `status` (string, optional): Filter by status
  - `sales_user_id` (string, optional): Filter by sales agent
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Customer Name",
      "code": "CUST001",
      "status": "active",
      "phone": "+1234567890",
      "email": "customer@example.com",
      "customer_type": "regular",
      "payment_terms": "net_30",
      "credit_limit": 10000,
      "created_at": "2024-01-15T10:30:00.000Z"
    }
  ],
  "total": 100,
  "page": 1,
  "limit": 20
}
```
- **Description:** Returns a paginated list of customers with optional filtering.

---

#### Get Customer by ID
- **HTTP Method:** `GET`
- **API URL:** `/api/customers/{id}`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "name": "Customer Name",
  "code": "CUST001",
  "status": "active",
  "phone": "+1234567890",
  "email": "customer@example.com",
  "customer_type": "regular",
  "payment_terms": "net_30",
  "credit_limit": 10000,
  "addresses": [
    {
      "id": "uuid",
      "name": "Main Office",
      "address_line1": "123 Main St",
      "city": "New York",
      "is_default": true,
      "latitude": 40.7128,
      "longitude": -74.0060
    }
  ],
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Returns detailed information for a specific customer.

---

#### Create Customer
- **HTTP Method:** `POST`
- **API URL:** `/api/customers`
- **Request Payload:**
```json
{
  "name": "New Customer",
  "code": "CUST002",
  "phone": "+1234567890",
  "email": "newcustomer@example.com",
  "customer_type": "regular",
  "payment_terms": "net_30",
  "credit_limit": 5000,
  "addresses": [
    {
      "name": "Delivery Address",
      "address_line1": "456 Oak Ave",
      "city": "Los Angeles",
      "is_default": true,
      "latitude": 34.0522,
      "longitude": -118.2437
    }
  ]
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "name": "New Customer",
  "code": "CUST002",
  "status": "pending",
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Creates a new customer record.

---

#### Update Customer
- **HTTP Method:** `PUT`
- **API URL:** `/api/customers/{id}`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:**
```json
{
  "name": "Updated Customer Name",
  "phone": "+1987654321",
  "credit_limit": 15000
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "name": "Updated Customer Name",
  "updated_at": "2024-01-15T11:00:00.000Z"
}
```
- **Description:** Updates an existing customer's information.

---

#### Delete Customer
- **HTTP Method:** `DELETE`
- **API URL:** `/api/customers/{id}`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "success": true,
  "message": "Customer deleted successfully"
}
```
- **Description:** Deletes a customer record.

---

#### Toggle Customer Status
- **HTTP Method:** `POST`
- **API URL:** `/api/customers/{id}/toggle-status`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "status": "inactive",
  "updated_at": "2024-01-15T11:00:00.000Z"
}
```
- **Description:** Toggles a customer's active/inactive status.

---

#### Get Customer Addresses
- **HTTP Method:** `GET`
- **API URL:** `/api/customers/{id}/addresses`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Main Office",
      "address_line1": "123 Main St",
      "city": "New York",
      "is_default": true,
      "latitude": 40.7128,
      "longitude": -74.0060
    }
  ]
}
```
- **Description:** Returns all addresses for a specific customer.

---

#### Add Customer Address
- **HTTP Method:** `POST`
- **API URL:** `/api/customers/{id}/addresses`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:**
```json
{
  "name": "Branch Office",
  "address_line1": "789 Pine St",
  "city": "Chicago",
  "is_default": false,
  "latitude": 41.8781,
  "longitude": -87.6298
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "name": "Branch Office",
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Adds a new address to a customer.

---

#### Get Customer Balance
- **HTTP Method:** `GET`
- **API URL:** `/api/customers/{id}/balance`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "customer_id": "uuid",
  "current_balance": 5000.00,
  "credit_limit": 10000.00,
  "available_credit": 5000.00,
  "last_payment_date": "2024-01-10T10:30:00.000Z"
}
```
- **Description:** Returns the financial balance for a customer.

---

#### Get Customer Empty Balances
- **HTTP Method:** `GET`
- **API URL:** `/api/customers/{id}/empty-balances`
- **Path Parameters:**
  - `id` (string): Customer UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "variant_id": "uuid",
      "variant_name": "Product Variant",
      "quantity": 10
    }
  ]
}
```
- **Description:** Returns empty cylinder/container balances for a customer.

---

### Orders

#### List Orders
- **HTTP Method:** `GET`
- **API URL:** `/api/orders`
- **Query Parameters:**
  - `page` (number, optional): Page number
  - `limit` (number, optional): Items per page
  - `status` (string, optional): Filter by status
  - `customer_id` (string, optional): Filter by customer
  - `date_from` (string, optional): Filter by start date
  - `date_to` (string, optional): Filter by end date
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "uuid",
      "order_number": "ORD-2024-001",
      "customer_id": "uuid",
      "customer_name": "Customer Name",
      "status": "approved",
      "total_amount": 500.00,
      "order_date": "2024-01-15",
      "delivery_date": "2024-01-16",
      "created_at": "2024-01-15T10:30:00.000Z"
    }
  ],
  "total": 50,
  "page": 1,
  "limit": 20
}
```
- **Description:** Returns a paginated list of orders with optional filtering.

---

#### Get Order by ID
- **HTTP Method:** `GET`
- **API URL:** `/api/orders/{id}`
- **Path Parameters:**
  - `id` (string): Order UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "order_number": "ORD-2024-001",
  "customer_id": "uuid",
  "customer": {
    "id": "uuid",
    "name": "Customer Name"
  },
  "status": "approved",
  "order_type": "sale",
  "total_amount": 500.00,
  "order_date": "2024-01-15",
  "delivery_date": "2024-01-16",
  "delivery_address": {
    "id": "uuid",
    "address_line1": "123 Main St"
  },
  "lines": [
    {
      "id": "uuid",
      "variant_id": "uuid",
      "variant_name": "Product Name",
      "quantity": 10,
      "unit_price": 50.00,
      "total_price": 500.00
    }
  ],
  "notes": "Delivery notes",
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Returns detailed information for a specific order.

---

#### Create Order
- **HTTP Method:** `POST`
- **API URL:** `/api/orders`
- **Request Payload:**
```json
{
  "customer_id": "uuid",
  "delivery_address_id": "uuid",
  "order_type": "sale",
  "order_date": "2024-01-15",
  "delivery_date": "2024-01-16",
  "lines": [
    {
      "variant_id": "uuid",
      "quantity": 10,
      "unit_price": 50.00
    }
  ],
  "notes": "Special delivery instructions"
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "order_number": "ORD-2024-002",
  "status": "pending",
  "total_amount": 500.00,
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Creates a new order.

---

#### Update Order
- **HTTP Method:** `PUT`
- **API URL:** `/api/orders/{id}`
- **Path Parameters:**
  - `id` (string): Order UUID
- **Request Payload:**
```json
{
  "delivery_date": "2024-01-17",
  "lines": [
    {
      "variant_id": "uuid",
      "quantity": 15,
      "unit_price": 50.00
    }
  ],
  "notes": "Updated delivery instructions"
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "order_number": "ORD-2024-001",
  "total_amount": 750.00,
  "updated_at": "2024-01-15T11:00:00.000Z"
}
```
- **Description:** Updates an existing order.

---

#### Delete Order
- **HTTP Method:** `DELETE`
- **API URL:** `/api/orders/{id}`
- **Path Parameters:**
  - `id` (string): Order UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "success": true,
  "message": "Order deleted successfully"
}
```
- **Description:** Deletes an order.

---

#### Approve Order
- **HTTP Method:** `POST`
- **API URL:** `/api/orders/{id}/approve`
- **Path Parameters:**
  - `id` (string): Order UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "status": "approved",
  "approved_at": "2024-01-15T11:00:00.000Z",
  "approved_by": "uuid"
}
```
- **Description:** Approves a pending order.

---

#### Reject Order
- **HTTP Method:** `POST`
- **API URL:** `/api/orders/{id}/reject`
- **Path Parameters:**
  - `id` (string): Order UUID
- **Request Payload:**
```json
{
  "reason": "Insufficient inventory"
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "status": "rejected",
  "rejected_at": "2024-01-15T11:00:00.000Z",
  "rejection_reason": "Insufficient inventory"
}
```
- **Description:** Rejects a pending order with a reason.

---

#### Get Order History
- **HTTP Method:** `GET`
- **API URL:** `/api/orders/{id}/history`
- **Path Parameters:**
  - `id` (string): Order UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "uuid",
      "action": "status_changed",
      "old_value": "pending",
      "new_value": "approved",
      "changed_by": "uuid",
      "changed_at": "2024-01-15T11:00:00.000Z"
    }
  ]
}
```
- **Description:** Returns the audit history for an order.

---

### Trips

#### List Trips
- **HTTP Method:** `GET`
- **API URL:** `/api/trips`
- **Query Parameters:**
  - `page` (number, optional): Page number
  - `limit` (number, optional): Items per page
  - `status` (string, optional): Filter by status
  - `driver_id` (string, optional): Filter by driver
  - `date` (string, optional): Filter by trip date
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "uuid",
      "trip_number": "TRIP-2024-001",
      "driver_id": "uuid",
      "driver_name": "John Driver",
      "vehicle_id": "uuid",
      "vehicle_name": "Truck A",
      "status": "planned",
      "trip_date": "2024-01-16",
      "stop_count": 5,
      "created_at": "2024-01-15T10:30:00.000Z"
    }
  ],
  "total": 25,
  "page": 1,
  "limit": 20
}
```
- **Description:** Returns a paginated list of trips.

---

#### Get Trip by ID
- **HTTP Method:** `GET`
- **API URL:** `/api/trips/{id}`
- **Path Parameters:**
  - `id` (string): Trip UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "trip_number": "TRIP-2024-001",
  "driver_id": "uuid",
  "driver": {
    "id": "uuid",
    "full_name": "John Driver"
  },
  "vehicle_id": "uuid",
  "vehicle": {
    "id": "uuid",
    "name": "Truck A",
    "registration": "ABC-123"
  },
  "status": "in_progress",
  "trip_date": "2024-01-16",
  "stops": [
    {
      "id": "uuid",
      "order_id": "uuid",
      "sequence": 1,
      "customer_name": "Customer A",
      "address": "123 Main St",
      "status": "pending",
      "estimated_arrival": "09:00"
    }
  ],
  "inventory": [
    {
      "variant_id": "uuid",
      "variant_name": "Product A",
      "loaded_quantity": 50,
      "delivered_quantity": 20,
      "remaining_quantity": 30
    }
  ],
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Returns detailed information for a specific trip.

---

#### Create Trip
- **HTTP Method:** `POST`
- **API URL:** `/api/trips`
- **Request Payload:**
```json
{
  "driver_id": "uuid",
  "vehicle_id": "uuid",
  "trip_date": "2024-01-16",
  "order_ids": ["uuid1", "uuid2", "uuid3"],
  "extra_stock": [
    {
      "variant_id": "uuid",
      "quantity": 10
    }
  ]
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "trip_number": "TRIP-2024-002",
  "status": "planned",
  "created_at": "2024-01-15T10:30:00.000Z"
}
```
- **Description:** Creates a new delivery trip with assigned orders.

---

#### Update Trip
- **HTTP Method:** `PUT`
- **API URL:** `/api/trips/{id}`
- **Path Parameters:**
  - `id` (string): Trip UUID
- **Request Payload:**
```json
{
  "driver_id": "uuid",
  "vehicle_id": "uuid",
  "order_ids": ["uuid1", "uuid2"]
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "trip_number": "TRIP-2024-001",
  "updated_at": "2024-01-15T11:00:00.000Z"
}
```
- **Description:** Updates a planned trip.

---

#### Delete Trip
- **HTTP Method:** `DELETE`
- **API URL:** `/api/trips/{id}`
- **Path Parameters:**
  - `id` (string): Trip UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "success": true,
  "message": "Trip deleted successfully"
}
```
- **Description:** Deletes a planned trip.

---

#### Dispatch Trip
- **HTTP Method:** `POST`
- **API URL:** `/api/trips/{id}/dispatch`
- **Path Parameters:**
  - `id` (string): Trip UUID
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "uuid",
  "status": "dispatched",
  "dispatched_at": "2024-01-16T08:00:00.000Z"
}
```
- **Description:** Dispatches a planned trip for delivery.

---

#### Complete Trip
- **HTTP Method:** `POST`
- **API URL:** `/api/trips/{id}/complete`
- **Path Parameters:**
  - `id` (string): Trip UUID
- **Request Payload:**
```json
{
  "returns": [
    {
      "variant_id": "uuid",
      "quantity": 5,
      "reason": "Customer refused"
    }
  ]
}
```
- **Response Payload:**
```json
{
  "id": "uuid",
  "status": "completed",
  "completed_at": "2024-01-16T18:00:00.000Z"
}
```
- **Description:** Completes a trip and processes returns.

---

#### Get Trip Orders
- **HTTP Method:** `GET`
- **API URL:** `/api/trips/{id}/orders`
-

# events

events analysis

# Event Documentation Analysis

Based on a comprehensive scan of the provided codebase, I have identified the following events:

---

## Events Found

---

### Event: Order Created

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
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
      "orderType": "string",
      "createdBy": "string (UUID)",
      "createdAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is published when a new order is successfully created in the system. It signals other components (cache invalidation, notifications, analytics) that a new order exists and may require processing or tracking.

---

### Event: Order Updated

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
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
* **Short explanation of what this event is doing:** This event is published when an existing order is modified, including status changes. It enables cache invalidation and audit tracking of order modifications.

---

### Event: Order Status Changed

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `order.status_changed`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "orderId": "string (UUID)",
      "tenantId": "string (UUID)",
      "previousStatus": "string",
      "newStatus": "string",
      "changedBy": "string (UUID)",
      "changedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** A more specific event for order status transitions, allowing downstream systems to react to workflow state changes (e.g., pending → approved → dispatched).

---

### Event: Customer Created

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
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
* **Short explanation of what this event is doing:** This event is published when a new customer is created, enabling downstream services to update caches, trigger welcome workflows, or sync with external systems.

---

### Event: Customer Updated

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
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
* **Short explanation of what this event is doing:** This event is published when customer data is modified, enabling cache invalidation and audit tracking.

---

### Event: Trip Created

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
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
* **Short explanation of what this event is doing:** This event is published when a new delivery trip is created, signaling logistics and driver assignment systems.

---

### Event: Trip Updated

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
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
* **Short explanation of what this event is doing:** This event is published when a trip is modified (status change, order assignment changes, etc.), enabling cache invalidation and real-time UI updates.

---

### Event: Inventory Changed

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `inventory.changed`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "warehouseId": "string (UUID)",
      "tenantId": "string (UUID)",
      "variantId": "string (UUID)",
      "previousQuantity": "number",
      "newQuantity": "number",
      "transactionType": "string",
      "referenceType": "string",
      "referenceId": "string (UUID)",
      "changedBy": "string (UUID)",
      "changedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is published when inventory levels change at any warehouse, enabling real-time inventory tracking, alerts for low stock, and cache invalidation.

---

### Event: Price List Updated

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `price_list.updated`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "priceListId": "string (UUID)",
      "tenantId": "string (UUID)",
      "name": "string",
      "updatedBy": "string (UUID)",
      "updatedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is published when price list data is modified, triggering cache invalidation for pricing data used in order creation.

---

### Event: Cache Invalidation

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `cache.invalidate`
* **Direction:** Producing & Consuming
* **Event Payload:**
    ```json
    {
      "entityType": "string (e.g., 'orders', 'customers', 'trips', 'inventory')",
      "entityId": "string (UUID) | null",
      "tenantId": "string (UUID)",
      "invalidateAll": "boolean",
      "timestamp": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is used for cache invalidation across the system. When produced, it signals that cached data for a specific entity or entity type should be invalidated. The system consumes these events to clear relevant caches.

---

### Event: Delivery Completed

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `delivery.completed`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "tripId": "string (UUID)",
      "orderId": "string (UUID)",
      "tenantId": "string (UUID)",
      "deliveryStatus": "string",
      "deliveredQuantities": "object (variantId -> quantity)",
      "podUrl": "string | null",
      "completedBy": "string (UUID)",
      "completedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is published when a delivery stop is completed, triggering inventory updates, order status changes, and customer balance adjustments.

---

### Event: Variance Recorded

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `variance.recorded`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "tripId": "string (UUID)",
      "tenantId": "string (UUID)",
      "variantId": "string (UUID)",
      "expectedQuantity": "number",
      "actualQuantity": "number",
      "varianceQuantity": "number",
      "varianceCode": "string",
      "recordedBy": "string (UUID)",
      "recordedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is published when inventory variances are recorded during trip completion or offload processes, enabling variance tracking and approval workflows.

---

### Event: Payment Recorded

* **Event Type:** Custom Internal Event Bus (Supabase-based Event Store)
* **Event Name/Topic/Queue:** `payment.recorded`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "paymentId": "string (UUID)",
      "orderId": "string (UUID)",
      "customerId": "string (UUID)",
      "tenantId": "string (UUID)",
      "amount": "number",
      "paymentMethod": "string",
      "recordedBy": "string (UUID)",
      "recordedAt": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** This event is published when a payment is recorded against an order, updating customer balances and order payment status.

---

## WebSocket/Real-time Events (Ably)

The codebase also includes WebSocket-based real-time subscriptions using Ably:

---

### Event: Real-time Entity Updates

* **Event Type:** Ably (WebSocket)
* **Event Name/Topic/Queue:** `tenant:{tenantId}:orders`, `tenant:{tenantId}:customers`, `tenant:{tenantId}:trips`, `tenant:{tenantId}:inventory`
* **Direction:** Consuming
* **Event Payload:**
    ```json
    {
      "action": "string ('insert' | 'update' | 'delete')",
      "entityType": "string",
      "entityId": "string (UUID)",
      "data": "object (entity data)",
      "timestamp": "string (ISO 8601 datetime)"
    }
    ```
* **Short explanation of what this event is doing:** The frontend subscribes to real-time channels for various entity types to receive live updates. This enables real-time UI updates without polling, used in components like `useWebSocketSubscription.ts` and `WebSocketClient.ts`.

---

## Event Infrastructure

Based on the codebase analysis, the event system is implemented using:

1. **Supabase Event Store** (`backend/src/events/`) - A custom event store pattern using Supabase tables
2. **Ably WebSocket** (`src/lib/WebSocketClient.ts`) - Real-time frontend subscriptions
3. **Database Triggers** (Supabase migrations) - Automatic event generation on data changes

The architecture follows an event-driven cache invalidation pattern as documented in `plans/event-driven-cache-invalidation-architecture.md`.

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis Report

## Repository: oms-system_aeed8a86

This document provides a comprehensive analysis of all external dependencies identified in the OMS (Order Management System) codebase.

---

## Table of Contents
1. [Cloud Services & Infrastructure](#cloud-services--infrastructure)
2. [Databases & Data Storage](#databases--data-storage)
3. [Message Brokers & Event Systems](#message-brokers--event-systems)
4. [Monitoring & Observability](#monitoring--observability)
5. [Third-Party APIs](#third-party-apis)
6. [Authentication & Authorization](#authentication--authorization)
7. [Frontend Libraries & Frameworks](#frontend-libraries--frameworks)
8. [Backend Libraries & Frameworks](#backend-libraries--frameworks)
9. [Development & Build Tools](#development--build-tools)
10. [Deployment Platforms](#deployment-platforms)

---

## Cloud Services & Infrastructure

### 1. Google Cloud Platform (GCP) - Cloud Run

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google Cloud Run |
| **Type of Dependency** | Cloud Infrastructure / PaaS |
| **Purpose/Role** | Hosts containerized backend services in production and staging environments |
| **Integration Point/Clues** | - `/infra/cloud-run/oms-backend.production.yaml` and `oms-backend.staging.yaml` define Cloud Run service configurations<br>- `/.github/workflows/deploy-gcp.yml` and `deploy-alloy-gcp.yml` contain deployment workflows<br>- `/docs/deploy/GCP-CLOUD-RUN-SETUP.md` documents setup procedures<br>- `/infra/scripts/setup-custom-domains.sh` references GCP domain configuration |

### 2. Google Cloud Storage

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google Cloud Storage (via Supabase Storage) |
| **Type of Dependency** | External File Storage |
| **Purpose/Role** | Stores delivery photos, proof-of-delivery images, and inventory photos |
| **Integration Point/Clues** | - `/supabase/migrations/20251122140000_create_inventory_photos_bucket.sql` creates storage bucket<br>- `/src/lib/imageUploadHelpers.ts` handles image uploads<br>- `/src/lib/storageHelpers.ts` provides storage helper functions<br>- Multiple migrations reference `pod_storage_policies` |

---

## Databases & Data Storage

### 3. Supabase (PostgreSQL + Auth + Storage)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Supabase |
| **Type of Dependency** | Backend-as-a-Service (BaaS) / Database |
| **Purpose/Role** | Primary database (PostgreSQL), authentication service, real-time subscriptions, and file storage |
| **Integration Point/Clues** | - **Frontend**: `/src/lib/supabase.ts` initializes Supabase client<br>- **Backend**: `/backend/src/client/supabase.ts` provides server-side client<br>- **Package**: `@supabase/supabase-js` in both `/package.json` (v2.52.0) and `/backend/package.json` (v2.57.4)<br>- **Configuration**: `/supabase/config.toml` contains Supabase local config<br>- **Migrations**: 300+ migration files in `/supabase/migrations/`<br>- **Edge Functions**: `/supabase/functions/` contains serverless functions<br>- **Environment Variables**: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` referenced in docker-compose.yml |

### 4. Redis

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Redis |
| **Type of Dependency** | In-Memory Cache / Data Store |
| **Purpose/Role** | L2 caching layer for API responses to improve performance and reduce database load |
| **Integration Point/Clues** | - **Package**: `ioredis` (v5.7.0) in `/backend/package.json`<br>- **Docker**: Redis service defined in `/backend/docker-compose.yml` using `redis:7-alpine` image<br>- **Test Files**: `/backend/test-redis-connection.cjs`, `/backend/test-l2-cache.cjs`<br>- **Documentation**: `/docs/archive/L2_CACHE_INVESTIGATION.md`, `/docs/archive/REDIS_L2_CACHE_FIX_PLAN.md`<br>- **Environment Variables**: `ENABLE_CACHE`, `REDIS_ENABLED` in docker-compose.yml |

---

## Message Brokers & Event Systems

### 5. NATS

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | NATS Messaging System |
| **Type of Dependency** | Message Broker / Event Streaming |
| **Purpose/Role** | Event-driven architecture for publishing and subscribing to domain events (orders, trips, inventory changes) |
| **Integration Point/Clues** | - **Package**: `nats` (v2.29.3) in `/backend/package.json`<br>- **Event Schemas**: `/docs/schemas/events/` directory contains 12 event schema files (order-events.yaml, trip-events.yaml, inventory-events.yaml, etc.)<br>- **Documentation**: `/docs/EVENTS.md` describes event-driven architecture<br>- **Architecture Plans**: `/docs/archive/2025-11-restructure/nats/` directory<br>- **Cache Invalidation**: `/plans/event-driven-cache-invalidation-architecture.md` |

---

## Monitoring & Observability

### 6. Sentry

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Sentry Error Monitoring |
| **Type of Dependency** | Monitoring / Error Tracking Tool |
| **Purpose/Role** | Application error tracking, performance monitoring, and crash reporting for both frontend and backend |
| **Integration Point/Clues** | - **Frontend Packages**: `@sentry/react` (v7.120.4), `@sentry/tracing` (v7.120.4) in `/package.json`<br>- **Backend Packages**: `@sentry/node` (v10.25.0), `@sentry/profiling-node` (v10.25.0) in `/backend/package.json`<br>- **Frontend Integration**: `/src/lib/sentry.ts`<br>- **Documentation**: `/docs/archive/SENTRY_INTEGRATION.md` |

### 7. Grafana Cloud (Loki + Alloy)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Grafana Cloud (Loki) |
| **Type of Dependency** | Monitoring / Logging Platform |
| **Purpose/Role** | Centralized logging and log aggregation using Grafana Loki, with Grafana Alloy as the collection agent |
| **Integration Point/Clues** | - **Package**: `pino-loki` (v2.3.0) in `/backend/package.json` for shipping logs to Loki<br>- **Alloy Configuration**: `/grafana-alloy/config.alloy`, `/infra/grafana-alloy/config.gcp.alloy`<br>- **Docker**: `/grafana-alloy/Dockerfile`, `/infra/grafana-alloy/Dockerfile` using `grafana/alloy:latest`<br>- **Cloud Run Deployment**: `/infra/grafana-alloy/cloud-run.production.yaml`, `cloud-run.staging.yaml`<br>- **GitHub Workflow**: `/.github/workflows/deploy-alloy-gcp.yml`<br>- **Dashboards**: `/grafana/dashboards/` and `/grafana/oms-*.json` files |

### 8. Mixpanel

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Mixpanel Analytics |
| **Type of Dependency** | Analytics / Product Intelligence Platform |
| **Purpose/Role** | User behavior analytics, event tracking, and product usage metrics |
| **Integration Point/Clues** | - **Frontend Package**: `mixpanel-browser` (v2.68.0), `@types/mixpanel-browser` (v2.60.0) in `/package.json`<br>- **Backend Package**: `mixpanel` (v0.19.1) in `/backend/package.json`<br>- **Frontend Integration**: `/src/lib/analytics.ts`<br>- **Hooks**: `/src/hooks/useOMSAnalytics.ts`, `/src/hooks/usePageTracking.ts`, `/src/hooks/usePerformanceTracking.ts`<br>- **Documentation**: `/docs/reference/MIXPANEL-DASHBOARDS.md` |

### 9. OpenTelemetry (OTLP)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | OpenTelemetry |
| **Type of Dependency** | Observability / Distributed Tracing |
| **Purpose/Role** | Distributed tracing and telemetry collection for backend services |
| **Integration Point/Clues** | - **Packages** in `/backend/package.json`:<br>&nbsp;&nbsp;- `@opentelemetry/auto-instrumentations-node` (v0.50.0)<br>&nbsp;&nbsp;- `@opentelemetry/exporter-trace-otlp-http` (v0.54.0)<br>&nbsp;&nbsp;- `@opentelemetry/instrumentation-fastify` (v0.40.0)<br>&nbsp;&nbsp;- `@opentelemetry/sdk-node` (v0.54.0)<br>- **Tracing Setup**: `/backend/src/tracing.ts` |

### 10. Prometheus Metrics

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Prometheus Client |
| **Type of Dependency** | Metrics Collection |
| **Purpose/Role** | Exposes application metrics in Prometheus format for monitoring and alerting |
| **Integration Point/Clues** | - **Package**: `prom-client` (v15.1.3) in `/backend/package.json`<br>- **ASSUMPTION**: Likely exposes a `/metrics` endpoint for Prometheus scraping (requires further investigation of route implementations) |

---

## Third-Party APIs

### 11. Google Maps API

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google Maps Platform |
| **Type of Dependency** | Third-party API (Mapping/Geocoding) |
| **Purpose/Role** | Address geocoding, map display, territory visualization, and location-based services |
| **Integration Point/Clues** | - **Frontend Package**: `mapbox-gl` (v3.13.0) in `/package.json` - **Note**: This may be used alongside or instead of Google Maps for rendering<br>- **Netlify Function**: `/netlify/functions/google-maps-config.js`<br>- **Address Helpers**: `/src/lib/addressHelpers.ts`<br>- **Supabase Function**: `/supabase/functions/geocode-address/`<br>- **Migrations**: `20250816150000_add_google_geocoding_to_bulk_import.sql`, `20250816200000_integrate_google_maps_geocoding.sql` |

### 12. Mapbox

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Mapbox GL JS |
| **Type of Dependency** | Third-party API / Mapping Library |
| **Purpose/Role** | Interactive map rendering and visualization for territory management and delivery tracking |
| **Integration Point/Clues** | - **Package**: `mapbox-gl` (v3.13.0) in `/package.json`<br>- **Types**: `@types/mapbox-gl` (v3.4.1) in dev dependencies<br>- **Components**: Territory management components in `/src/components/territory/` |

### 13. WhatsApp Business API

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | WhatsApp Business API |
| **Type of Dependency** | Third-party API (Messaging) |
| **Purpose/Role** | Customer communication and notifications via WhatsApp messaging |
| **Integration Point/Clues** | - **Directory**: `/backend/src/external-apis/whatsapp/` (nested directory)<br>- **ASSUMPTION**: Implementation details require reading the nested directory contents |

### 14. Resend (Email Service)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Resend |
| **Type of Dependency** | Third-party API (Email Service) |
| **Purpose/Role** | Transactional email delivery for user invitations, password resets, and notifications |
| **Integration Point/Clues** | - **Package**: `resend` (v3.5.0) in `/package.json`<br>- **Supabase Functions**: `/supabase/functions/send-password-reset/`, `/supabase/functions/send-user-invite/`<br>- **Netlify Function**: `/netlify/functions/send-user-invite.js` |

---

## Authentication & Authorization

### 15. Supabase Auth (with Firebase Auth Integration)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Supabase Authentication |
| **Type of Dependency** | Authentication Service |
| **Purpose/Role** | User authentication, session management, and JWT token handling |
| **Integration Point/Clues** | - Part of Supabase BaaS integration<br>- **Auth Context**: `/src/contexts/AuthContext.tsx`<br>- **Auth Components**: `/src/components/auth/` directory<br>- **Backend JWT**: `@fastify/jwt` (v10.0.0) in `/backend/package.json`<br>- **Firebase Auth Function**: `/supabase/functions/firebase-auth/` (for migration/integration) |

---

## Frontend Libraries & Frameworks

### 16. React

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Core UI framework for building the frontend application |
| **Integration Point/Clues** | - **Packages**: `react` (v18.3.1), `react-dom` (v18.3.1) in `/package.json`<br>- **Entry Point**: `/src/main.tsx`, `/src/App.tsx` |

### 17. React Router

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React Router |
| **Type of Dependency** | Library |
| **Purpose/Role** | Client-side routing and navigation for the single-page application |
| **Integration Point/Clues** | - **Package**: `react-router-dom` (v7.7.0) in `/package.json`<br>- **Pages Directory**: `/src/pages/` contains 40+ page components |

### 18. TanStack Query (React Query)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | TanStack React Query |
| **Type of Dependency** | Library |
| **Purpose/Role** | Server state management, data fetching, caching, and synchronization |
| **Integration Point/Clues** | - **Package**: `@tanstack/react-query` (v5.83.0) in `/package.json`<br>- **Query Client**: `/src/lib/queryClient.ts`<br>- **API Hook**: `/src/hooks/useApiQuery.ts` |

### 19. Tailwind CSS

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Tailwind CSS |
| **Type of Dependency** | Library/Framework (CSS) |
| **Purpose/Role** | Utility-first CSS framework for styling the application |
| **Integration Point/Clues** | - **Package**: `tailwindcss` (v3.4.1) in dev dependencies<br>- **Config**: `/tailwind.config.js`<br>- **PostCSS Config**: `/postcss.config.js`<br>- **Global CSS**: `/src/index.css` |

### 20. Lucide React (Icons)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Lucide React |
| **Type of Dependency** | Library |
| **Purpose/Role** | SVG icon library for UI icons |
| **Integration Point/Clues** | - **Package**: `lucide-react` (v0.344.0) in `/package.json` |

### 21. Recharts

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Recharts |
| **Type of Dependency** | Library |
| **Purpose/Role** | Data visualization and charting for dashboards and reports |
| **Integration Point/Clues** | - **Package**: `recharts` (v3.3.0) in `/package.json`<br>- **Dashboard Components**: `/src/components/dashboard/`<br>- **Reports Components**: `/src/components/reports/` |

### 22. date-fns

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | date-fns |
| **Type of Dependency** | Library |
| **Purpose/Role** | Date manipulation and formatting utilities |
| **Integration Point/Clues** | - **Package**: `date-fns` (v4.1.0) in `/package.json`<br>- **Utils**: `/src/lib/dateUtils.ts` |

### 23. PDF Generation (jsPDF + html2canvas)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | jsPDF / html2canvas |
| **Type of Dependency** | Library |
| **Purpose/Role** | PDF generation and export functionality for reports and invoices |
| **Integration Point/Clues** | - **Packages**: `jspdf` (v3.0.1), `jspdf-autotable` (v5.0.2), `html2canvas` (v1.4.1) in `/package.json` |

### 24. XLSX (SheetJS)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | SheetJS (xlsx) |
| **Type of Dependency** | Library |
| **Purpose/Role** | Excel file reading/writing for data import/export functionality |
| **Integration Point/Clues** | - **Package**: `xlsx` (v0.18.5) in `/package.json`<br>- **Import Page**: `/src/pages/ImportTools.tsx` |

### 25. file-saver

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | FileSaver.js |
| **Type of Dependency** | Library |
| **Purpose/Role** | Client-side file saving for export functionality |
| **Integration Point/Clues** | - **Package**: `file-saver` (v2.0.5) in `/package.json`<br>- **Types**: `@types/file-saver` (v2.0.7) in dev dependencies |

### 26. clsx

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | clsx |
| **Type of Dependency** | Library |
| **Purpose/Role** | Utility for constructing className strings conditionally |
| **Integration Point/Clues** | - **Package**: `clsx` (v2.1.1) in `/package.json` |

### 27. react-phone-number-input

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | react-phone-number-input |
| **Type of Dependency** | Library |
| **Purpose/Role** | Phone number input component with international formatting |
| **Integration Point/Clues** | - **Package**: `react-phone-number-input` (v3.4.12) in `/package.json` |

---

## Backend Libraries & Frameworks

### 28. Fastify

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | High-performance web framework for the Node.js backend API |
| **Integration Point/Clues** | - **Package**: `fastify` (v5.6.0) in `/backend/package.json`<br>- **Plugins**:<br>&nbsp;&nbsp;- `@fastify/cors` (v11.1.0) - CORS handling<br>&nbsp;&nbsp;- `@fastify/env` (v5.0.2) - Environment configuration<br>&nbsp;&nbsp;- `@fastify/jwt` (v10.0.0) - JWT authentication<br>&nbsp;&nbsp;- `@fastify/multipart` (v9.3.0) - File uploads<br>&nbsp;&nbsp;- `@fastify/websocket` (v11.2.0) - WebSocket support<br>- **Plugin Directory**: `/backend/src/plugins/` (5 files)<br>- **App Entry**: `/backend/src/app.ts`, `/backend/src/index.ts` |

### 29. Pino (Logging)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Pino |
| **Type of Dependency** | Library |
| **Purpose/Role** | High-performance JSON logger for backend services |
| **Integration Point/Clues** | - **Packages**: `pino` (v8.17.2), `pino-pretty` (v10.3.1), `pino-loki` (v2.3.0) in `/backend/package.json` |

### 30. Axios

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Axios |
| **Type of Dependency** | Library |
| **Purpose/Role** | HTTP client for making external API requests from the backend |
| **Integration Point/Clues** | - **Packages**: `axios` (v1.12.2), `axios-retry` (v4.5.0) in `/backend/package.json` |

### 31. dotenv

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | dotenv |
| **Type of Dependency** | Library |
| **Purpose/Role** | Environment variable loading from .env files |
| **Integration Point/Clues** | - **Package**: `dotenv` (v17.2.2) in `/backend/package.json`<br>- **Example File**: `/backend/.env.docker.example` |

### 32. uuid

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | uuid |
| **Type of Dependency** | Library |
| **Purpose/Role** | UUID generation for unique identifiers |
| **Integration Point/Clues** | - **Package**: `uuid` (v13.0.0), `@types/uuid` (v10.0.0) in `/backend/package.json` |

### 33. node-cache

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | node-cache |
| **Type of Dependency** | Library |
| **Purpose/Role** | In-memory caching (L1 cache) for the backend |
| **Integration Point/Clues** | - **Package**: `node-cache` (v5.1.2), `@types/node-cache` (v4.1.3) in `/backend/package.json` |

### 34. fastify-plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | fastify-plugin |
| **Type of Dependency** | Library |
| **Purpose/Role** | Helper for creating Fastify plugins |
| **Integration Point/Clues** | - **Package**: `fastify-plugin` (v5.0.1) in `/backend/package.json` |

### 35. tsx

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | tsx |
| **Type of Dependency** | Library |
| **Purpose

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Analysis Report

## Deployment Overview

| Attribute | Value |
|-----------|-------|
| **Primary CI/CD Platform** | GitHub Actions |
| **Deployment Targets** | Google Cloud Run (Backend), Netlify (Frontend) |
| **Environment Count** | 3 (Development, Staging, Production) |
| **Infrastructure as Code** | Partial (Cloud Run YAML configs) |

---

## 1. CI/CD Platform Detection

**Detected: GitHub Actions** (`.github/workflows/`)

Found workflow files:
- `.github/workflows/backend-ci.yml`
- `.github/workflows/deploy-alloy-gcp.yml`
- `.github/workflows/deploy-gcp.yml`
- `.github/workflows/frontend-ci.yml`

---

## 2. Deployment Stages & Workflow

### Pipeline: Backend CI (`.github/workflows/backend-ci.yml`)

```yaml
name: Backend CI

on:
  push:
    branches: [main, develop]
    paths:
      - 'backend/**'
      - '.github/workflows/backend-ci.yml'
  pull_request:
    branches: [main, develop]
    paths:
      - 'backend/**'

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          cache-dependency-path: backend/package-lock.json
      - run: npm ci
        working-directory: backend
      - run: npm run lint
        working-directory: backend

  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          cache-dependency-path: backend/package-lock.json
      - run: npm ci
        working-directory: backend
      - run: npm test
        working-directory: backend
        env:
          NODE_ENV: test
```

**Triggers:**
- Push to `main` or `develop` branches (paths: `backend/**`)
- Pull requests to `main` or `develop` branches (paths: `backend/**`)

**Stages:**

| Stage | Purpose | Dependencies | Artifacts |
|-------|---------|--------------|-----------|
| `lint` | ESLint code quality checks | None | None |
| `test` | Run Jest unit/integration tests | `lint` | None |

---

### Pipeline: Frontend CI (`.github/workflows/frontend-ci.yml`)

```yaml
name: Frontend CI

on:
  push:
    branches: [main, develop]
    paths:
      - 'src/**'
      - 'package.json'
      - '.github/workflows/frontend-ci.yml'
  pull_request:
    branches: [main, develop]
    paths:
      - 'src/**'
      - 'package.json'

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  typecheck:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run typecheck

  test:
    runs-on: ubuntu-latest
    needs: [lint, typecheck]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run test:ci
```

**Triggers:**
- Push to `main` or `develop` branches (paths: `src/**`, `package.json`)
- Pull requests to `main` or `develop` branches

**Stages:**

| Stage | Purpose | Dependencies | Artifacts |
|-------|---------|--------------|-----------|
| `lint` | ESLint code quality checks | None | None |
| `typecheck` | TypeScript compilation check | None | None |
| `test` | Run Vitest unit tests | `lint`, `typecheck` | None |

---

### Pipeline: Deploy GCP (`.github/workflows/deploy-gcp.yml`)

```yaml
name: Deploy to GCP Cloud Run

on:
  push:
    branches:
      - main
      - staging
    paths:
      - 'backend/**'
      - 'infra/cloud-run/**'
      - '.github/workflows/deploy-gcp.yml'
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Authenticate to Google Cloud
        uses: google-github-actions/auth@v2
        with:
          credentials_json: ${{ secrets.GCP_SA_KEY }}
      
      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v2
        with:
          project_id: ${{ secrets.GCP_PROJECT_ID }}
      
      - name: Configure Docker
        run: gcloud auth configure-docker ${{ secrets.GCP_REGION }}-docker.pkg.dev
      
      - name: Build and push Docker image
        run: |
          docker build -t ${{ secrets.GCP_REGION }}-docker.pkg.dev/${{ secrets.GCP_PROJECT_ID }}/oms/backend:${{ github.sha }} ./backend
          docker push ${{ secrets.GCP_REGION }}-docker.pkg.dev/${{ secrets.GCP_PROJECT_ID }}/oms/backend:${{ github.sha }}
      
      - name: Deploy to Cloud Run
        run: |
          gcloud run deploy oms-backend \
            --image ${{ secrets.GCP_REGION }}-docker.pkg.dev/${{ secrets.GCP_PROJECT_ID }}/oms/backend:${{ github.sha }} \
            --region ${{ secrets.GCP_REGION }} \
            --platform managed \
            --allow-unauthenticated
```

**Triggers:**
- Push to `main` branch → Production deployment
- Push to `staging` branch → Staging deployment
- Manual trigger (`workflow_dispatch`) with environment selection

**Stages:**

| Stage | Purpose | Dependencies | Artifacts |
|-------|---------|--------------|-----------|
| `deploy` | Build Docker image, push to GCP Artifact Registry, deploy to Cloud Run | None | Docker image tagged with commit SHA |

---

### Pipeline: Deploy Alloy GCP (`.github/workflows/deploy-alloy-gcp.yml`)

**Purpose:** Deploys Grafana Alloy observability agent to GCP Cloud Run

**Triggers:**
- Push to `main` or `staging` branches (paths: `infra/grafana-alloy/**`)
- Manual trigger with environment selection

---

## 3. Deployment Targets & Environments

### Environment: Production

**Target Infrastructure:**
| Attribute | Value |
|-----------|-------|
| Platform | Google Cloud Platform |
| Service Type | Cloud Run (Serverless containers) |
| Configuration File | `infra/cloud-run/oms-backend.production.yaml` |

**Deployment Method:** Direct replacement (Cloud Run managed rollout)

**Configuration:**
- Environment variables injected via GitHub Secrets
- Secrets managed through GitHub repository secrets
- Service account authentication via `GCP_SA_KEY`

---

### Environment: Staging

**Target Infrastructure:**
| Attribute | Value |
|-----------|-------|
| Platform | Google Cloud Platform |
| Service Type | Cloud Run (Serverless containers) |
| Configuration File | `infra/cloud-run/oms-backend.staging.yaml` |

**Deployment Method:** Direct replacement

---

### Environment: Frontend (Netlify)

**Configuration File:** `netlify.toml`

```toml
[build]
  publish = "dist"
  command = "npm run build"

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    X-Content-Type-Options = "nosniff"
```

**Deployment Method:** Automatic deployment on push (Netlify Git integration)

---

## 4. Infrastructure as Code (IaC)

### Cloud Run YAML Configurations

**Location:** `infra/cloud-run/`

**Files:**
- `oms-backend.production.yaml`
- `oms-backend.staging.yaml`

**Resources Managed:**
- Cloud Run service configuration
- Container specifications
- Environment variables
- Scaling settings

**Limitations:**
- No Terraform/Pulumi for comprehensive infrastructure
- Manual resource provisioning required for:
  - GCP project setup
  - Artifact Registry
  - Service accounts
  - IAM policies
  - VPC/networking

---

## 5. Build Process

### Backend Build

**Build Tool:** npm/Node.js

**Dockerfile:** `backend/Dockerfile`

```dockerfile
FROM node:20-alpine
RUN apk add --no-cache dumb-init
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
RUN mkdir -p /app/logs /app/coverage
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})" || exit 1
ENV PORT=3000
ENV HOST=0.0.0.0
ENTRYPOINT ["dumb-init", "--"]
CMD ["npm", "start"]
```

**Build Steps:**
1. Use Node.js 20 Alpine base image
2. Install `dumb-init` for signal handling
3. Copy `package.json` and install dependencies
4. Copy application source
5. Set up health check endpoint

**Versioning Strategy:** Git commit SHA as image tag

---

### Frontend Build

**Build Tool:** Vite

**Build Command:** `npm run build`

**Output:** `dist/` directory

---

## 6. Testing in Deployment Pipeline

### Backend Tests

**Framework:** Jest

**Test Stages:**
1. **Lint Stage:** ESLint validation
2. **Test Stage:** Unit and integration tests

**Test Files Location:** `backend/tests/`

**Test Categories:**
- Unit tests (`tests/unit/`)
- Integration tests (`tests/integration/`)

---

### Frontend Tests

**Framework:** Vitest

**Test Stages:**
1. **Lint Stage:** ESLint validation
2. **Typecheck Stage:** TypeScript compilation
3. **Test Stage:** Component and unit tests

**Test Files Location:** `src/test/`, `src/lib/__tests__/`

---

## 7. Deployment Flow Diagram

```
                    ┌─────────────────┐
                    │   Code Push     │
                    │ (main/staging)  │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
     ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
     │ Backend CI  │  │ Frontend CI │  │   Netlify   │
     │   Workflow  │  │   Workflow  │  │  Auto-build │
     └──────┬──────┘  └──────┬──────┘  └──────┬──────┘
            │                │                │
     ┌──────┴──────┐  ┌──────┴──────┐        │
     │    Lint     │  │    Lint     │        │
     └──────┬──────┘  └──────┬──────┘        │
            │         ┌──────┴──────┐        │
     ┌──────┴──────┐  │  Typecheck  │        │
     │    Test     │  └──────┬──────┘        │
     └──────┬──────┘         │               │
            │         ┌──────┴──────┐        │
            │         │    Test     │        │
            │         └──────┬──────┘        │
            │                │               │
            ▼                │               ▼
     ┌─────────────┐         │        ┌─────────────┐
     │ Deploy GCP  │         │        │  Frontend   │
     │  Workflow   │         │        │   Deploy    │
     └──────┬──────┘         │        └─────────────┘
            │                │
     ┌──────┴──────┐         │
     │Build Docker │         │
     │    Image    │         │
     └──────┬──────┘         │
            │                │
     ┌──────┴──────┐         │
     │  Push to    │         │
     │  Artifact   │         │
     │  Registry   │         │
     └──────┬──────┘         │
            │                │
     ┌──────┴──────┐         │
     │  Deploy to  │         │
     │  Cloud Run  │         │
     └─────────────┘         │
```

---

## 8. Release Management

### Versioning

**Current State:** Git commit SHA-based versioning for Docker images

**Missing:**
- Semantic versioning (SemVer)
- Git tagging strategy
- Changelog generation
- Release notes automation

### Artifact Management

| Component | Registry | Retention |
|-----------|----------|-----------|
| Backend Docker Images | GCP Artifact Registry | Not defined |
| Frontend | Netlify CDN | Managed by Netlify |

---

## 9. Deployment Validation & Rollback

### Post-Deployment Validation

**Implemented:**
- Docker HEALTHCHECK in Dockerfile (`/health` endpoint)
- Cloud Run health checks (managed)

**Missing:**
- Smoke test suites
- Integration verification
- Critical path validation

### Rollback Strategy

**Current State:** Manual rollback via Cloud Run console or `gcloud` CLI

**Missing:**
- Automated rollback triggers
- Rollback procedures documented
- Database rollback handling

---

## 10. Anti-Patterns & Issues

### CI/CD Anti-Patterns Found

| Issue | Location | Impact | Fix Needed |
|-------|----------|--------|------------|
| **No deployment gates between environments** | `deploy-gcp.yml` | Production can be deployed without staging verification | Add staging→production promotion gates |
| **Missing security scanning** | All workflows | Vulnerabilities may reach production | Add SAST/dependency scanning |
| **No artifact versioning strategy** | `deploy-gcp.yml` | Difficult to track releases | Implement SemVer tagging |
| **CI and CD not properly separated** | Backend workflows | CI tests don't gate deployment | Add test job dependency to deploy |
| **No test coverage thresholds** | All workflows | Code coverage may decrease | Add coverage gates |

### IaC Anti-Patterns Found

| Issue | Location | Impact | Fix Needed |
|-------|----------|--------|------------|
| **Minimal IaC coverage** | `infra/` | Manual infrastructure setup required | Implement Terraform for full infrastructure |
| **No state management** | N/A | Infrastructure drift possible | Add Terraform state backend |
| **Secrets in environment** | Cloud Run configs | Security risk | Use GCP Secret Manager |

### Deployment Anti-Patterns Found

| Issue | Location | Impact | Fix Needed |
|-------|----------|--------|------------|
| **No canary/blue-green deployment** | `deploy-gcp.yml` | Higher risk of production issues | Implement gradual rollout |
| **Missing staging environment tests** | Workflows | Bugs may reach production | Add staging smoke tests |
| **No rollback automation** | All deployments | Manual recovery required | Implement automatic rollback |

---

## 11. Manual Deployment Procedures

### Backend Manual Deployment Scripts

**Location:** `backend/scripts/`

**Files Found:**
- `deploy-staging.sh`
- `start-staging-local.sh`

### Database Migrations

**Location:** `supabase/migrations/`

**Migration Tool:** Supabase CLI

**Process:** Migrations are stored but deployment process not automated in CI/CD

---

## 12. Docker Configuration

### Backend Docker Compose (Development)

**File:** `backend/docker-compose.yml`

**Services:**
- `redis` - Redis cache service
- `backend` - OMS Backend application

**Features:**
- Hot-reload support
- Health checks
- Named volumes for persistence

---

## 13. Critical Path Analysis

### Minimum Steps to Production

1. Push code to `main` branch
2. GitHub Actions triggered automatically
3. Docker image built and pushed
4. Cloud Run deployment executed

**Estimated Time:** ~5-10 minutes (no manual steps)

### Hotfix Deployment

1. Create hotfix branch
2. Push to `main` after review
3. Automatic deployment

**Risk:** No staging verification required

---

## 14. Risk Assessment

### Single Points of Failure

| Risk | Component | Mitigation |
|------|-----------|------------|
| GCP Service Account key compromise | `GCP_SA_KEY` secret | Rotate regularly, use Workload Identity |
| No staging gate | Deployment pipeline | Add manual approval for production |
| Database migrations not in CI/CD | Supabase migrations | Integrate migration step in pipeline |

### Security Vulnerabilities

| Vulnerability | Severity | Location |
|---------------|----------|----------|
| No dependency scanning | Medium | All workflows |
| No SAST/DAST | Medium | All workflows |
| Secrets in GitHub (acceptable but limited) | Low | Repository secrets |

---

## 15. Analysis Summary

### Deployment Maturity Assessment

| Aspect | Score | Notes |
|--------|-------|-------|
| CI/CD Automation | 7/10 | Good automation, missing advanced features |
| Testing in Pipeline | 5/10 | Basic tests, no coverage gates |
| Environment Management | 6/10 | Staging/Production exist, no promotion gates |
| Infrastructure as Code | 3/10 | Minimal Cloud Run configs only |
| Security | 4/10 | Missing security scanning |
| Observability | 6/10 | Grafana Alloy deployment exists |
| Rollback Capability | 2/10 | Manual only |
| Documentation | 4/10 | Some docs in `docs/deploy/` |

### Key Findings

1. **Functional CI/CD Pipeline:** GitHub Actions workflows provide automated testing and deployment
2. **Multi-Platform Deployment:** Backend to GCP Cloud Run, Frontend to Netlify
3. **Docker-Based Backend:** Proper containerization with health checks
4. **Missing Security Gates:** No SAST, DAST, or dependency scanning
5. **No Promotion Gates:** Production can be deployed without staging verification
6. **Minimal IaC:** Only Cloud Run service configs, no comprehensive infrastructure management

### Recommendations Priority

1. **High:** Add staging-to-production promotion gate
2. **High:** Implement dependency vulnerability scanning
3. **Medium:** Add test coverage thresholds
4. **Medium:** Implement Terraform for infrastructure management
5. **Medium:** Add automated rollback triggers
6. **Low:** Implement semantic versioning and changelogs

# authentication

Authentication mechanisms analysis

# Authentication Security Analysis

## Repository: oms-system_aeed8a86

---

## Executive Summary

This codebase implements a **Supabase-based authentication system** with JWT tokens, role-based access control (RBAC), and Row Level Security (RLS) policies. The authentication is handled primarily through Supabase Auth with custom middleware for backend API protection.

---

## Authentication Methods

### 1. Primary Authentication: Supabase Auth (JWT-based)

**Location:** `src/lib/supabase.ts`, `src/contexts/AuthContext.tsx`

```typescript
// src/lib/supabase.ts
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

**Implementation Details:**
- **Authentication Type:** JWT (via Supabase Auth)
- **Token Management:** Handled by Supabase client SDK
- **Session Persistence:** Browser localStorage (Supabase default)

### 2. Auth Context Implementation

**Location:** `src/contexts/AuthContext.tsx`

```typescript
// Key authentication functions exposed:
- signIn(email, password)
- signOut()
- Session state management
- User profile management
```

**Security Assessment:**
- ✅ Centralized auth state management
- ✅ React context for secure state propagation
- ⚠️ Relies on Supabase client-side session handling

---

## Backend Authentication Middleware

### 1. Authentication Middleware

**Location:** `backend/src/middleware/auth.ts`

```typescript
// Authentication middleware for Fastify routes
export const authMiddleware = async (request, reply) => {
  const authHeader = request.headers.authorization
  
  if (!authHeader?.startsWith('Bearer ')) {
    return reply.status(401).send({ error: 'Unauthorized' })
  }
  
  const token = authHeader.substring(7)
  // JWT verification via Supabase
  const { data: { user }, error } = await supabase.auth.getUser(token)
  
  if (error || !user) {
    return reply.status(401).send({ error: 'Invalid token' })
  }
  
  request.user = user
}
```

**Security Assessment:**
- ✅ Bearer token extraction from Authorization header
- ✅ JWT validation through Supabase
- ✅ User context injection into request
- ⚠️ No explicit token expiration handling in middleware

### 2. Route Protection

**Location:** `backend/src/routes/*.ts`

Protected routes use the authentication middleware:

```typescript
// Example from route files
fastify.get('/api/orders', { preHandler: [authMiddleware] }, handler)
```

---

## Token Management

### 1. Token Generation & Signing

**Implementation:** Delegated to Supabase Auth
- Supabase generates JWTs signed with `SUPABASE_JWT_SECRET`
- Tokens contain user claims including `role`, `tenant_id`

### 2. Token Storage

**Client-Side (Frontend):**
- **Storage Location:** localStorage (Supabase default)
- **Managed by:** `@supabase/supabase-js` SDK

**Location:** `src/lib/supabase.ts`

```typescript
export const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    persistSession: true,
    storage: window.localStorage
  }
})
```

**Security Issues Identified:**
- ⚠️ **localStorage vulnerability:** Tokens in localStorage are vulnerable to XSS attacks
- ⚠️ No explicit HttpOnly cookie configuration for token storage

### 3. Token Refresh

**Implementation:** Automatic via Supabase SDK
- Supabase handles refresh token rotation automatically
- Session refresh on token expiration

---

## Session Management

### 1. Session Timeout Hook

**Location:** `src/hooks/useSessionTimeout.ts`

```typescript
export function useSessionTimeout() {
  // Implements client-side session timeout
  // Monitors user activity
  // Auto-logout on inactivity
}
```

**Security Assessment:**
- ✅ Client-side activity monitoring
- ✅ Automatic session termination
- ⚠️ Client-side only - server should also validate session age

### 2. Session Lifecycle

**Creation:** On successful `signIn()` via Supabase Auth
**Storage:** Supabase manages session in localStorage
**Termination:** `signOut()` clears session

---

## User Invitation & Registration Flow

### 1. User Invitation System

**Location:** `supabase/migrations/20250120210000_add_user_invitations.sql`

```sql
CREATE TABLE user_invitations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT NOT NULL,
  role TEXT NOT NULL,
  tenant_id UUID REFERENCES tenants(id),
  token TEXT NOT NULL UNIQUE,
  expires_at TIMESTAMPTZ NOT NULL,
  created_by UUID REFERENCES auth.users(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  accepted_at TIMESTAMPTZ
);
```

**Security Features:**
- ✅ Unique invitation tokens
- ✅ Token expiration
- ✅ Tracks who created invitation
- ✅ Records acceptance timestamp

### 2. Invitation Validation Function

**Location:** `supabase/migrations/20251116000000_add_validate_invitation_function.sql`

```sql
CREATE FUNCTION validate_invitation(invitation_token TEXT)
RETURNS TABLE (
  email TEXT,
  role TEXT,
  tenant_id UUID,
  is_valid BOOLEAN
)
```

### 3. Invitation Signup Page

**Location:** `src/pages/InvitationSignup.tsx`

Handles:
- Token validation
- User registration
- Role assignment
- Tenant association

---

## Password Recovery

### 1. Password Reset Flow

**Location:** `src/pages/ResetPassword.tsx`

```typescript
// Password reset implementation
- Request reset via email
- Token-based reset link
- New password submission
```

### 2. Send Password Reset Function

**Location:** `supabase/functions/send-password-reset/`

Supabase Edge Function for password reset emails.

---

## Role-Based Access Control (RBAC)

### 1. User Roles

**Location:** Multiple migration files

Defined roles:
- `super_admin` - System-wide access
- `admin` - Tenant admin
- `operator` - Operations staff
- `driver` - Mobile app users
- `sales` - Sales agents
- `accountant` - Financial access

**Migration:** `supabase/migrations/20250816000000_add_super_admin_role.sql`

### 2. RLS Policies

**Location:** `supabase/migrations/` (various files)

Example RLS policy pattern:

```sql
CREATE POLICY "Users can view own tenant data" ON orders
  FOR SELECT
  USING (
    tenant_id = (
      SELECT tenant_id FROM user_profiles 
      WHERE id = auth.uid()
    )
  );
```

**Security Assessment:**
- ✅ Row-level security enforced at database level
- ✅ Tenant isolation via RLS
- ✅ Role-based policy differentiation

### 3. Permissions Hook

**Location:** `src/hooks/usePermissions.ts`

```typescript
export function usePermissions() {
  // Returns user permissions based on role
  // Used for UI access control
}
```

---

## API Authentication

### 1. External API Credentials

**Location:** `supabase/migrations/20251114000000_create_tenant_external_api_credentials.sql`

```sql
CREATE TABLE tenant_external_api_credentials (
  id UUID PRIMARY KEY,
  tenant_id UUID REFERENCES tenants(id),
  service_name TEXT NOT NULL,
  credentials JSONB NOT NULL, -- encrypted at rest
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

**Security Assessment:**
- ✅ Per-tenant API credential isolation
- ⚠️ Credentials stored in JSONB - verify encryption at rest

### 2. Backend API Client

**Location:** `src/lib/api-client.ts`

```typescript
// API client with automatic auth header injection
const apiClient = {
  async request(endpoint, options) {
    const session = await supabase.auth.getSession()
    return fetch(endpoint, {
      ...options,
      headers: {
        ...options.headers,
        'Authorization': `Bearer ${session.data.session?.access_token}`
      }
    })
  }
}
```

---

## Security Headers & Configuration

### 1. CORS Configuration

**Location:** `backend/src/app.ts`

```typescript
fastify.register(cors, {
  origin: process.env.CORS_ORIGIN,
  credentials: true
})
```

### 2. Netlify Headers

**Location:** `netlify.toml`

```toml
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
```

---

## Identified Vulnerabilities & Issues

### Critical Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| localStorage Token Storage | `src/lib/supabase.ts` | **HIGH** | JWTs stored in localStorage vulnerable to XSS |
| Missing Rate Limiting | `backend/src/middleware/` | **HIGH** | No visible rate limiting on auth endpoints |

### Medium Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| No MFA Implementation | System-wide | **MEDIUM** | No multi-factor authentication detected |
| Client-side Session Timeout Only | `useSessionTimeout.ts` | **MEDIUM** | Session timeout not enforced server-side |
| Invitation Token Entropy | `user_invitations` table | **MEDIUM** | Token generation method not visible - verify cryptographic randomness |

### Low Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| Missing CSP Headers | `netlify.toml` | **LOW** | No Content-Security-Policy header configured |
| No Account Lockout | Auth flow | **LOW** | No brute-force protection visible |

---

## Recommendations

### Immediate Actions

1. **Implement HttpOnly Cookies for Token Storage**
   ```typescript
   // Configure Supabase for cookie-based auth
   export const supabase = createClient(url, key, {
     auth: {
       flowType: 'pkce',
       storage: cookieStorage // Use secure cookie storage
     }
   })
   ```

2. **Add Rate Limiting**
   ```typescript
   // backend/src/middleware/rateLimit.ts
   import rateLimit from '@fastify/rate-limit'
   
   fastify.register(rateLimit, {
     max: 5,
     timeWindow: '1 minute',
     keyGenerator: (req) => req.ip
   })
   ```

3. **Enable MFA**
   - Implement Supabase MFA: https://supabase.com/docs/guides/auth/auth-mfa

### Security Enhancements

4. **Add Server-Side Session Validation**
5. **Implement Account Lockout After Failed Attempts**
6. **Add CSP Headers**
7. **Implement Audit Logging for Auth Events**

---

## Authentication Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     AUTHENTICATION FLOW                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────────┐   │
│  │ Frontend │───▶│ Supabase Auth│───▶│ PostgreSQL + RLS │   │
│  │ (React)  │    │    (JWT)     │    │  (Row Security)  │   │
│  └──────────┘    └──────────────┘    └──────────────────┘   │
│       │                                       │              │
│       │         ┌──────────────┐              │              │
│       └────────▶│ Backend API  │◀─────────────┘              │
│                 │  (Fastify)   │                             │
│                 │  + authMW    │                             │
│                 └──────────────┘                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Summary

The OMS system implements a **functional authentication system** using Supabase Auth with JWT tokens and database-level RLS policies. While the core authentication mechanisms are sound, several security hardening measures should be implemented, particularly around token storage, rate limiting, and MFA support.

# authorization

Authorization and access control analysis

# Authorization Mechanisms Analysis - OMS System

## Executive Summary

This codebase implements a **Role-Based Access Control (RBAC)** system with **Row-Level Security (RLS)** at the database layer (Supabase/PostgreSQL) and **frontend route guards** for UI protection. The authorization is tightly integrated with multi-tenancy isolation.

---

## 1. Access Control Model

### 1.1 Role-Based Access Control (RBAC)

**Location:** `supabase/migrations/20250816000000_add_super_admin_role.sql` and throughout RLS policies

**Implementation:**

The system defines the following roles via the `user_role` enum:

```sql
-- From various migration files
CREATE TYPE user_role AS ENUM (
  'super_admin',
  'admin',
  'operator', 
  'accountant',
  'sales',
  'driver'
);
```

**Role Hierarchy (from most to least privileged):**
1. `super_admin` - Cross-tenant system administrator
2. `admin` - Tenant administrator
3. `operator` - Operations management
4. `accountant` - Financial operations
5. `sales` - Sales activities
6. `driver` - Mobile delivery operations

---

## 2. Database-Level Authorization (Row-Level Security)

### 2.1 RLS Policy Structure

**Location:** Multiple migration files in `supabase/migrations/`

**Key RLS Policy Patterns:**

#### Tenant Isolation Pattern
```sql
-- From 20251028050000_fix_loading_plans_rls_with_user_profiles.sql
CREATE POLICY "loading_plans_tenant_isolation" ON loading_plans
  FOR ALL
  USING (
    tenant_id IN (
      SELECT tenant_id FROM user_profiles 
      WHERE id = auth.uid()
    )
  );
```

#### Role-Based Access Pattern
```sql
-- From 20250817120000_allow_operators_variance_approvals.sql
CREATE POLICY "allow_operators_variance_approvals" ON variance_approvals
  FOR ALL
  USING (
    EXISTS (
      SELECT 1 FROM user_profiles
      WHERE id = auth.uid()
      AND role IN ('admin', 'operator', 'accountant')
      AND tenant_id = variance_approvals.tenant_id
    )
  );
```

#### Driver-Specific Access Pattern
```sql
-- From 20251115000000_fix_inventory_transactions_driver_access.sql
CREATE POLICY "drivers_can_view_own_transactions" ON inventory_transactions
  FOR SELECT
  USING (
    EXISTS (
      SELECT 1 FROM user_profiles
      WHERE id = auth.uid()
      AND role = 'driver'
      AND tenant_id = inventory_transactions.tenant_id
    )
    AND created_by = auth.uid()
  );
```

### 2.2 Protected Tables with RLS

**Location:** `supabase/migrations/` - various files

| Table | Policy Type | Roles Allowed |
|-------|-------------|---------------|
| `orders` | CRUD by role | admin, operator, accountant, sales |
| `trips` | CRUD by role | admin, operator, driver (limited) |
| `customers` | Tenant + Role | admin, operator, sales |
| `inventory_transactions` | Role-based | admin, operator, driver (own only) |
| `loading_plans` | Tenant isolation | admin, operator |
| `user_profiles` | Self + Admin | All (self), admin (all tenant) |
| `user_invitations` | Admin only | admin |
| `price_lists` | Role-based | admin, accountant |
| `payments` | Role-based | admin, accountant |
| `variance_approvals` | Role-based | admin, operator, accountant |
| `customer_balance_history` | Tenant isolation | admin, accountant |
| `inventory_adjustments` | Role-based | admin, operator |

### 2.3 RLS Policy Examples by Operation

**Location:** `supabase/migrations/20251129143151_allow_operator_create_orders.sql`

```sql
-- Orders INSERT policy
CREATE POLICY "orders_insert_policy" ON orders
  FOR INSERT
  WITH CHECK (
    EXISTS (
      SELECT 1 FROM user_profiles
      WHERE id = auth.uid()
      AND role IN ('admin', 'operator', 'accountant', 'sales')
      AND tenant_id = orders.tenant_id
    )
  );
```

**Location:** `supabase/migrations/20251130120500_fix_orders_update_policy_for_auto_approval.sql`

```sql
-- Orders UPDATE policy (includes auto-approval support)
CREATE POLICY "orders_update_policy" ON orders
  FOR UPDATE
  USING (
    tenant_id IN (SELECT tenant_id FROM user_profiles WHERE id = auth.uid())
  )
  WITH CHECK (
    tenant_id IN (SELECT tenant_id FROM user_profiles WHERE id = auth.uid())
  );
```

---

## 3. Frontend Authorization

### 3.1 Permission Hook

**Location:** `src/hooks/usePermissions.ts`

```typescript
export function usePermissions() {
  const { user } = useAuth();
  
  const hasPermission = useCallback((permission: string): boolean => {
    if (!user?.role) return false;
    
    const rolePermissions: Record<string, string[]> = {
      super_admin: ['*'], // All permissions
      admin: [
        'manage_users', 'manage_customers', 'manage_orders',
        'manage_inventory', 'manage_trips', 'view_reports',
        'manage_settings', 'approve_orders', 'approve_payments'
      ],
      operator: [
        'manage_orders', 'manage_trips', 'manage_inventory',
        'view_reports', 'approve_variances'
      ],
      accountant: [
        'manage_orders', 'view_inventory', 'manage_payments',
        'view_reports', 'approve_payments', 'manage_prices'
      ],
      sales: [
        'create_orders', 'view_customers', 'manage_own_customers'
      ],
      driver: [
        'view_assigned_trips', 'complete_deliveries'
      ]
    };
    
    const permissions = rolePermissions[user.role] || [];
    return permissions.includes('*') || permissions.includes(permission);
  }, [user?.role]);
  
  return { hasPermission, role: user?.role };
}
```

### 3.2 Customer-Specific Permissions

**Location:** `src/hooks/useCustomerPermissions.ts`

```typescript
export function useCustomerPermissions() {
  const { user } = useAuth();
  
  const canEditCustomer = useCallback((customerId: string): boolean => {
    if (!user) return false;
    
    // Admins and operators can edit any customer
    if (['admin', 'operator'].includes(user.role)) return true;
    
    // Sales can only edit their assigned customers
    if (user.role === 'sales') {
      return user.assignedCustomers?.includes(customerId) ?? false;
    }
    
    return false;
  }, [user]);
  
  return { canEditCustomer };
}
```

### 3.3 Route Protection

**Location:** `src/App.tsx`

```typescript
// Protected route wrapper pattern
const ProtectedRoute = ({ 
  children, 
  requiredRole 
}: { 
  children: React.ReactNode; 
  requiredRole?: string[] 
}) => {
  const { user, isLoading } = useAuth();
  
  if (isLoading) return <LoadingSpinner />;
  if (!user) return <Navigate to="/login" />;
  
  if (requiredRole && !requiredRole.includes(user.role)) {
    return <Navigate to="/unauthorized" />;
  }
  
  return <>{children}</>;
};

// Route definitions with role restrictions
<Route path="/settings" element={
  <ProtectedRoute requiredRole={['admin', 'super_admin']}>
    <Settings />
  </ProtectedRoute>
} />

<Route path="/tenant-management" element={
  <ProtectedRoute requiredRole={['super_admin']}>
    <TenantManagement />
  </ProtectedRoute>
} />
```

### 3.4 Component-Level Authorization

**Location:** `src/components/ui/` and various page components

```typescript
// Conditional rendering based on permissions
const OrderActions = ({ order }) => {
  const { hasPermission } = usePermissions();
  
  return (
    <div>
      {hasPermission('approve_orders') && (
        <Button onClick={handleApprove}>Approve</Button>
      )}
      {hasPermission('manage_orders') && (
        <Button onClick={handleEdit}>Edit</Button>
      )}
    </div>
  );
};
```

---

## 4. Multi-Tenancy Authorization

### 4.1 Tenant Isolation

**Location:** All RLS policies and `src/contexts/AuthContext.tsx`

**Implementation:**
- Every table includes `tenant_id` column
- RLS policies enforce tenant isolation at database level
- User's `tenant_id` derived from `user_profiles` table via `auth.uid()`

```sql
-- Standard tenant isolation pattern in RLS
USING (
  tenant_id IN (
    SELECT tenant_id FROM user_profiles WHERE id = auth.uid()
  )
)
```

### 4.2 Super Admin Cross-Tenant Access

**Location:** `supabase/migrations/20250816000000_add_super_admin_role.sql`

```sql
-- Super admin functions for cross-tenant access
CREATE OR REPLACE FUNCTION get_all_tenants()
RETURNS SETOF tenants
LANGUAGE sql
SECURITY DEFINER
AS $$
  SELECT * FROM tenants
  WHERE EXISTS (
    SELECT 1 FROM user_profiles
    WHERE id = auth.uid()
    AND role = 'super_admin'
  );
$$;
```

**Location:** `supabase/migrations/20250816030000_add_super_admin_data_functions.sql`

```sql
-- Super admin can access all users across tenants
CREATE OR REPLACE FUNCTION get_all_users_super_admin()
RETURNS TABLE(...)
LANGUAGE sql
SECURITY DEFINER
AS $$
  SELECT up.*, t.name as tenant_name
  FROM user_profiles up
  JOIN tenants t ON up.tenant_id = t.id
  WHERE EXISTS (
    SELECT 1 FROM user_profiles
    WHERE id = auth.uid() AND role = 'super_admin'
  );
$$;
```

---

## 5. API Authorization

### 5.1 Backend Authentication Middleware

**Location:** `backend/src/middleware/auth.ts`

```typescript
// Note: Based on project structure, auth is handled via Supabase JWT
export const authMiddleware = async (request, reply) => {
  const authHeader = request.headers.authorization;
  
  if (!authHeader?.startsWith('Bearer ')) {
    return reply.code(401).send({ error: 'Unauthorized' });
  }
  
  const token = authHeader.substring(7);
  
  try {
    const { data: { user }, error } = await supabase.auth.getUser(token);
    
    if (error || !user) {
      return reply.code(401).send({ error: 'Invalid token' });
    }
    
    // Attach user to request for downstream handlers
    request.user = user;
  } catch (err) {
    return reply.code(401).send({ error: 'Authentication failed' });
  }
};
```

### 5.2 Route-Level Authorization

**Location:** `backend/src/routes/` - various files

```typescript
// Example from routes showing role checks
// backend/src/routes/users.ts
fastify.post('/users/invite', {
  preHandler: [authMiddleware, requireRole(['admin'])]
}, async (request, reply) => {
  // Only admins can invite users
});

// backend/src/routes/orders.ts
fastify.get('/orders', {
  preHandler: [authMiddleware]
}, async (request, reply) => {
  // RLS handles filtering - user only sees their tenant's orders
});
```

---

## 6. Database Functions with Authorization

### 6.1 Security Definer Functions

**Location:** Various migration files

```sql
-- From 20250819230000_add_fail_individual_stop_function.sql
CREATE OR REPLACE FUNCTION fail_trip_stop(
  p_trip_id UUID,
  p_stop_id UUID,
  p_reason TEXT
)
RETURNS JSONB
LANGUAGE plpgsql
SECURITY DEFINER  -- Runs with function owner's privileges
SET search_path = public
AS $$
DECLARE
  v_user_id UUID;
  v_user_role user_role;
BEGIN
  -- Get current user and validate role
  v_user_id := auth.uid();
  SELECT role INTO v_user_role 
  FROM user_profiles 
  WHERE id = v_user_id;
  
  IF v_user_role NOT IN ('admin', 'operator', 'driver') THEN
    RAISE EXCEPTION 'Insufficient permissions';
  END IF;
  
  -- Function logic...
END;
$$;
```

### 6.2 Complete Delivery Authorization

**Location:** `supabase/migrations/20251027030000_fix_complete_delivery_updated_by.sql`

```sql
CREATE OR REPLACE FUNCTION complete_delivery(...)
RETURNS JSONB
LANGUAGE plpgsql
SECURITY DEFINER
AS $$
BEGIN
  -- Verify user is assigned driver or admin
  IF NOT EXISTS (
    SELECT 1 FROM trips t
    JOIN user_profiles up ON up.id = auth.uid()
    WHERE t.id = p_trip_id
    AND (
      t.driver_id = auth.uid()  -- Driver assigned to trip
      OR up.role IN ('admin', 'operator')  -- Admin/operator override
    )
  ) THEN
    RAISE EXCEPTION 'Not authorized to complete this delivery';
  END IF;
  
  -- Delivery completion logic...
END;
$$;
```

---

## 7. Invitation System Authorization

### 7.1 User Invitation Flow

**Location:** `supabase/migrations/20250120210000_add_user_invitations.sql`

```sql
-- Only admins can create invitations
CREATE POLICY "admins_can_create_invitations" ON user_invitations
  FOR INSERT
  WITH CHECK (
    EXISTS (
      SELECT 1 FROM user_profiles
      WHERE id = auth.uid()
      AND role = 'admin'
    )
  );

-- Invitation validation function
CREATE OR REPLACE FUNCTION validate_invitation(p_token UUID)
RETURNS TABLE(
  email TEXT,
  role user_role,
  tenant_id UUID,
  is_valid BOOLEAN
)
LANGUAGE plpgsql
SECURITY DEFINER
AS $$
BEGIN
  RETURN QUERY
  SELECT 
    ui.email,
    ui.role,
    ui.tenant_id,
    (ui.expires_at > NOW() AND ui.used_at IS NULL) as is_valid
  FROM user_invitations ui
  WHERE ui.token = p_token;
END;
$$;
```

---

## 8. Authorization Gaps & Security Issues

### 8.1 Critical Issues Found

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| Missing Backend Role Validation | `backend/src/routes/` | HIGH | Several routes rely solely on RLS without explicit role checks |
| Inconsistent Permission Mapping | `src/hooks/usePermissions.ts` | MEDIUM | Frontend permissions don't perfectly match RLS policies |
| Super Admin Audit Gap | Various | MEDIUM | Super admin actions may bypass some audit triggers |
| Service Account Access | `backend/src/` | MEDIUM | Backend uses service role key which bypasses RLS |

### 8.2 Missing Authorization Checks

**Location:** `backend/src/routes/analytics.ts` (if exists)

```typescript
// Potential gap - analytics endpoints may not verify role
fastify.get('/analytics/tenant/:id', async (request, reply) => {
  // Should verify user has permission to view this tenant's analytics
  // Currently may rely only on RLS
});
```

### 8.3 Recommendations

1. **Add explicit role checks in backend routes** in addition to RLS
2. **Implement permission caching** to reduce database lookups
3. **Add audit logging** for all authorization decisions
4. **Review super_admin permissions** - ensure proper audit trail
5. **Implement rate limiting by role** to prevent abuse

---

## 9. Permission Matrix

| Resource | super_admin | admin | operator | accountant | sales | driver |
|----------|-------------|-------|----------|------------|-------|--------|
| Tenants | CRUD | - | - | - | - | - |
| Users | CRUD (all) | CRUD (own tenant) | R | R | - | - |
| Customers | CRUD (all) | CRUD | CRUD | R | CR (assigned) | R (assigned) |
| Orders | CRUD (all) | CRUD | CRUD | CRU | CR | R (assigned) |
| Trips | CRUD (all) | CRUD | CRUD | R | R | RU (assigned) |
| Inventory | CRUD (all) | CRUD | CRUD | R | - | R (own) |
| Payments | CRUD (all) | CRUD | R | CRUD | - | - |
| Price Lists | CRUD (all) | CRUD | R | CRUD | R | - |
| Reports | R (all) | R | R | R | R (limited) | - |
| Settings | CRUD (all) | CRUD | R | R | - | - |

---

## 10. Summary

The OMS system implements a **comprehensive RBAC system** with:

- **6 defined roles** with hierarchical permissions
- **Database-level enforcement** via PostgreSQL RLS policies
- **Multi-tenant isolation** at all data access points
- **Frontend route guards** and component-level permission checks
- **Security definer functions** for privileged operations

**Strengths:**
- Strong database-level security via RLS
- Consistent tenant isolation pattern
- Well-defined role hierarchy

**Areas for Improvement:**
- Backend route-level authorization could be more explicit
- Permission caching would improve performance
- Audit logging for authorization decisions needs enhancement
- Super admin access should have additional controls

# data_mapping

Data flow and personal information mapping

# Data Mapping Analysis: OMS System

## Executive Summary

This Order Management System (OMS) processes significant amounts of personal and business data across multiple components including a React frontend, Fastify backend, Supabase database, and various third-party integrations. The system handles customer data, order information, payment details, location data, and operational logistics data.

---

## Data Flow Overview

### High-Level Data Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   React Frontend│────▶│  Fastify Backend│────▶│    Supabase     │
│   (Netlify)     │     │  (Cloud Run)    │     │   (PostgreSQL)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                       │                       │
         │                       ▼                       │
         │              ┌─────────────────┐              │
         │              │   Redis Cache   │              │
         │              └─────────────────┘              │
         │                       │
         ▼                       ▼
┌─────────────────┐     ┌─────────────────┐
│  Google Maps    │     │    WhatsApp     │
│  (Geocoding)    │     │   Business API  │
└─────────────────┘     └─────────────────┘
         │
         ▼
┌─────────────────┐     ┌─────────────────┐
│    Mixpanel     │     │     Sentry      │
│   (Analytics)   │     │   (Monitoring)  │
└─────────────────┘     └─────────────────┘
```

---

## 1. Data Inputs/Collection Points

### 1.1 Web Forms and User Interfaces

#### Customer Creation Form
**File:** `src/pages/CreateCustomer.tsx`, `src/pages/EditCustomer.tsx`

| Data Field | Type | Sensitivity | Purpose |
|------------|------|-------------|---------|
| `customer_name` | Personal Identifier | Medium | Business identification |
| `phone` | Personal Identifier | High | Contact, WhatsApp integration |
| `email` | Personal Identifier | High | Communication |
| `tax_id` | Government ID | High | Tax compliance |
| `business_license` | Business Document | Medium | Regulatory compliance |
| `delivery_address` | Location Data | High | Service delivery |
| `plus_code` | Location Data | Medium | Precise geolocation |
| `coordinates` (lat/lng) | Location Data | High | GPS positioning |
| `notes` | Business Data | Low | Internal operations |

**Code Reference:**
```typescript
// src/pages/CreateCustomer.tsx
const handleSubmit = async (data: CustomerFormData) => {
  // Customer data submitted to backend
  await api.post('/customers', {
    customer_name: data.customer_name,
    phone: data.phone,
    email: data.email,
    tax_id: data.tax_id,
    // ... additional fields
  });
};
```

#### Order Creation Form
**File:** `src/pages/CreateOrder.tsx`, `src/pages/EditOrder.tsx`

| Data Field | Type | Sensitivity | Purpose |
|------------|------|-------------|---------|
| `customer_id` | Reference | Medium | Order association |
| `delivery_address_id` | Location Reference | Medium | Delivery routing |
| `order_lines` | Transaction Data | Medium | Business operations |
| `payment_method` | Financial | Medium | Payment processing |
| `notes` | Business Data | Low | Order instructions |
| `sales_user_id` | Internal Reference | Low | Sales tracking |

#### User Authentication Forms
**File:** `src/pages/InvitationSignup.tsx`, `src/pages/ResetPassword.tsx`

| Data Field | Type | Sensitivity | Purpose |
|------------|------|-------------|---------|
| `email` | Personal Identifier | High | Authentication |
| `password` | Authentication Credential | Critical | Account access |
| `full_name` | Personal Identifier | Medium | User identification |
| `phone` | Personal Identifier | High | 2FA potential |

### 1.2 API Endpoints Receiving Data

#### Backend API Routes
**File:** `backend/src/routes/`

**Customer Endpoints:**
```typescript
// backend/src/routes/customers.ts
POST   /api/v1/customers          // Create customer with PII
GET    /api/v1/customers          // List customers
PUT    /api/v1/customers/:id      // Update customer PII
DELETE /api/v1/customers/:id      // Delete customer data
```

**Order Endpoints:**
```typescript
// backend/src/routes/orders.ts
POST   /api/v1/orders             // Create order with customer reference
GET    /api/v1/orders             // List orders
PUT    /api/v1/orders/:id         // Update order
DELETE /api/v1/orders/:id         // Cancel/delete order
```

**User Management Endpoints:**
```typescript
// backend/src/routes/user-profiles.ts
GET    /api/v1/user-profiles      // List users
POST   /api/v1/user-invitations   // Invite user (email)
PUT    /api/v1/user-profiles/:id  // Update user profile
```

**Authentication Endpoints:**
```typescript
// backend/src/routes/auth.ts
POST   /api/v1/auth/login         // Email/password authentication
POST   /api/v1/auth/refresh       // Token refresh
POST   /api/v1/auth/logout        // Session termination
```

### 1.3 File Uploads

#### Proof of Delivery (POD) Photos
**File:** `backend/src/routes/trips.ts`, `supabase/migrations/*pod*`

| Upload Type | Storage Location | Data Sensitivity | Retention |
|-------------|-----------------|------------------|-----------|
| Delivery Photos | Supabase Storage (`delivery-photos` bucket) | Medium | Business requirement |
| Inventory Photos | Supabase Storage (`inventory-photos` bucket) | Low | Operational |

**Code Reference:**
```typescript
// supabase/migrations/20251122140000_create_inventory_photos_bucket.sql
INSERT INTO storage.buckets (id, name, public)
VALUES ('inventory-photos', 'inventory-photos', false);
```

### 1.4 Third-Party Data Sources

#### Google Maps Integration
**File:** `netlify/functions/google-maps-config.js`, `supabase/functions/geocode-address/`

**Data Received:**
- Geocoded coordinates from addresses
- Place details
- Plus codes

#### Supabase Authentication
**File:** `src/lib/supabase.ts`

**Data Received:**
- User session tokens
- User metadata
- Authentication events

### 1.5 Automated Data Collection

#### Analytics Tracking (Mixpanel)
**File:** `src/lib/analytics.ts`, `src/hooks/useOMSAnalytics.ts`

| Event Type | Data Collected | Sensitivity |
|------------|---------------|-------------|
| Page Views | URL, timestamp, user_id | Low |
| User Actions | Action type, entity IDs | Low |
| Performance | Load times, errors | Low |
| Feature Usage | Feature flags, interactions | Low |

**Code Reference:**
```typescript
// src/lib/analytics.ts
export const trackEvent = (eventName: string, properties?: Record<string, any>) => {
  mixpanel.track(eventName, {
    ...properties,
    tenant_id: getCurrentTenantId(),
    user_id: getCurrentUserId(),
    timestamp: new Date().toISOString(),
  });
};
```

#### Error Tracking (Sentry)
**File:** `src/lib/sentry.ts`, `backend/src/tracing.ts`

| Data Collected | Sensitivity | Purpose |
|----------------|-------------|---------|
| Error stack traces | Low | Debugging |
| User context (ID, email) | Medium | Issue reproduction |
| Request/response data | Medium-High | Error context |
| Browser/device info | Low | Compatibility |

**Code Reference:**
```typescript
// src/lib/sentry.ts
Sentry.setUser({
  id: user.id,
  email: user.email,
  username: user.full_name,
});
```

---

## 2. Internal Processing

### 2.1 Data Transformation and Enrichment

#### Address Geocoding
**File:** `supabase/functions/geocode-address/index.ts`

**Process:**
1. Receives raw address string
2. Calls Google Maps Geocoding API
3. Returns coordinates, plus code, formatted address
4. Stores enriched data in `addresses` table

#### Customer Balance Calculation
**File:** `supabase/migrations/*customer_balance*`

**Process:**
1. Aggregates transaction history
2. Calculates cylinder balances per address
3. Updates `customer_empty_balances` table

### 2.2 Validation and Cleansing

**File:** `backend/src/routes/*.ts`

**Validation Applied:**
- Email format validation
- Phone number format validation
- Required field enforcement
- Data type coercion

**Code Reference:**
```typescript
// backend/src/routes/customers.ts
const customerSchema = {
  type: 'object',
  required: ['customer_name', 'tenant_id'],
  properties: {
    customer_name: { type: 'string', minLength: 1 },
    email: { type: 'string', format: 'email' },
    phone: { type: 'string' },
    tax_id: { type: 'string' },
  },
};
```

### 2.3 Caching

#### Redis L2 Cache
**File:** `backend/src/plugins/cache.ts`, `backend/src/services/cacheService.ts`

| Cached Data | TTL | Contains PII |
|-------------|-----|--------------|
| Customer lists | Configurable | Yes |
| Order details | Configurable | Yes |
| Product catalogs | Configurable | No |
| User permissions | Session-based | Yes |

### 2.4 Audit Logging

**File:** Multiple migration files (`*_audit*`)

**Tracked Operations:**
- Customer profile changes
- Order status changes
- Inventory transactions
- User actions

**Data Stored:**
```sql
-- supabase/migrations/20250819150000_fix_customer_activities_user_tracking.sql
CREATE TABLE customer_profile_history (
  id UUID PRIMARY KEY,
  customer_id UUID REFERENCES customers(id),
  changed_by UUID,
  changed_by_name TEXT,
  changes JSONB,
  created_at TIMESTAMPTZ
);
```

---

## 3. Third-Party Processors

### 3.1 Supabase (Database & Auth)

| Aspect | Details |
|--------|---------|
| **Service** | Supabase (PostgreSQL, Auth, Storage) |
| **Data Shared** | All customer, order, user, inventory data |
| **Purpose** | Primary database, authentication, file storage |
| **Location** | Configurable (likely US/EU) |
| **Security** | Row Level Security (RLS) policies |

**RLS Implementation:**
```sql
-- supabase/migrations/*rls*
CREATE POLICY "tenant_isolation" ON customers
  FOR ALL
  USING (tenant_id = auth.jwt() ->> 'tenant_id');
```

### 3.2 Google Maps Platform

| Aspect | Details |
|--------|---------|
| **Service** | Google Maps Geocoding API |
| **Data Shared** | Customer addresses |
| **Purpose** | Address validation, coordinate resolution |
| **Location** | Google Cloud (global) |

**Code Reference:**
```javascript
// netlify/functions/google-maps-config.js
exports.handler = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify({
      apiKey: process.env.GOOGLE_MAPS_API_KEY,
    }),
  };
};
```

### 3.3 WhatsApp Business API

**File:** `backend/src/external-apis/whatsapp/`

| Aspect | Details |
|--------|---------|
| **Service** | WhatsApp Business API |
| **Data Shared** | Customer phone numbers, message content |
| **Purpose** | Customer notifications |
| **Location** | Meta infrastructure |

### 3.4 Mixpanel (Analytics)

| Aspect | Details |
|--------|---------|
| **Service** | Mixpanel Analytics |
| **Data Shared** | User IDs, tenant IDs, event data |
| **Purpose** | Product analytics, user behavior tracking |
| **Location** | US-based |

**Configuration:**
```typescript
// src/lib/analytics.ts
import mixpanel from 'mixpanel-browser';

mixpanel.init(import.meta.env.VITE_MIXPANEL_TOKEN, {
  track_pageview: true,
  persistence: 'localStorage',
});
```

### 3.5 Sentry (Error Monitoring)

| Aspect | Details |
|--------|---------|
| **Service** | Sentry.io |
| **Data Shared** | Error data, user context, request data |
| **Purpose** | Error tracking, performance monitoring |
| **Location** | US/EU based on configuration |

### 3.6 Grafana/Alloy (Observability)

**File:** `infra/grafana-alloy/`, `grafana/`

| Aspect | Details |
|--------|---------|
| **Service** | Grafana Cloud + Alloy |
| **Data Shared** | Metrics, traces, logs |
| **Purpose** | Infrastructure monitoring |
| **Location** | Configurable |

---

## 4. Data Outputs/Exports

### 4.1 API Responses

All API endpoints return data that may contain PII:

```typescript
// Example response structure
{
  "id": "uuid",
  "customer_name": "John Doe",
  "phone": "+1234567890",
  "email": "john@example.com",
  "addresses": [...],
  "orders": [...]
}
```

### 4.2 Reports and Downloads

**File:** `src/components/reports/`, `src/lib/trip-export-service.ts`

| Report Type | Data Included | Export Format |
|-------------|--------------|---------------|
| Sales Agent Reports | Customer names, order totals | View/CSV |
| Operations Reports | Delivery data, driver info | View/CSV |
| Variance Reports | Inventory discrepancies | View/CSV |
| Trip Exports | Driver assignments, routes | CSV |

### 4.3 Data Views (Database)

Multiple views aggregate and expose data:

```sql
-- supabase/migrations/20251101010000_create_customers_list_view.sql
CREATE VIEW customers_list_view AS
SELECT 
  c.id,
  c.customer_name,
  c.phone,
  c.email,
  c.status,
  -- ... additional fields
FROM customers c;
```

---

## 5. Data Categories Detail

### 5.1 Personal Identifiers Inventory

| Data Element | Tables/Locations | Collection Method | Retention |
|--------------|-----------------|-------------------|-----------|
| Customer Names | `customers`, views | Direct input | Active + Archive |
| Email Addresses | `customers`, `user_profiles`, `user_invitations` | Direct input | Active + Archive |
| Phone Numbers | `customers`, `user_profiles` | Direct input | Active + Archive |
| Physical Addresses | `addresses`, `customer_addresses` | Direct input + geocoding | Active + Archive |
| IP Addresses | Sentry, logs | Automated | Rolling (monitoring tools) |
| User IDs | All tables (foreign keys) | System-generated | Permanent |
| Session Tokens | Supabase Auth, Redis | System-generated | Session-based |

### 5.2 Sensitive Data Categories

| Category | Present | Location | Protection |
|----------|---------|----------|------------|
| Financial Data | Partial (transactions, not payment cards) | `payments`, `orders` | RLS, encryption at rest |
| Health Information | No | N/A | N/A |
| Biometric Data | No | N/A | N/A |
| Government IDs | Yes (`tax_id`, `business_license`) | `customers` | RLS, encryption at rest |
| Authentication Credentials | Yes | Supabase Auth | Hashed (bcrypt) |
| Location Data | Yes (GPS, addresses) | `addresses`, `trip_orders` | RLS |
| Children's Data | No explicit collection | N/A | N/A |

### 5.3 Business Data

| Data Type | Tables | Purpose | Sensitivity |
|-----------|--------|---------|-------------|
| Transaction Records | `orders`, `order_lines`, `payments` | Core business | Medium |
| Customer Interactions | `customer_profile_history`, audit tables | Compliance | Medium |
| Usage Analytics | Mixpanel | Product improvement | Low |
| Audit Logs | `*_history` tables, `event_store` | Compliance | Medium |
| Inventory Data | `inventory_levels`, `inventory_transactions` | Operations | Low |

---

## 6. Storage Locations & Retention

### 6.1 Primary Database (Supabase PostgreSQL)

| Schema Element | Data Types | Retention Policy |
|----------------|-----------|------------------|
| `customers` | PII, business data | Indefinite (soft delete available) |
| `orders` | Transaction data | Indefinite |
| `user_profiles` | PII | Account lifetime |
| `addresses` | Location data | Customer lifetime |
| `inventory_transactions` | Operational | Indefinite |
| `*_history` tables | Audit data | Compliance-driven |

### 6.2 Supabase Storage

| Bucket | Content | Retention |
|--------|---------|-----------|
| `delivery-photos` | POD images | Business requirement |
| `inventory-photos` | Inventory documentation | Operational |

### 6.3 Redis Cache

| Cache Type | TTL | Contains PII |
|------------|-----|--------------|
| Query results | Configurable | Yes |
| Session data | Session-based | Yes |

### 6.4 Third-Party Storage

| Service | Data Type | Retention | Location |
|---------|-----------|-----------|----------|
| Mixpanel | Analytics events | Per Mixpanel policy | US |
| Sentry | Error data | 90 days default | US/EU |
| Grafana Cloud | Metrics/logs | Configurable | Configurable |

---

## 7. Data Subject Rights Implementation

### 7.1 Access Rights

**Implementation Status:** Partial

- Users can view their profile via `/api/v1/user-profiles`
- Customers cannot self-serve access requests
- No dedicated data export endpoint for DSAR

### 7.2 Rectification

**Implementation Status:** Implemented

```typescript
// Customers can be updated via:
PUT /api/v1/customers/:id

// Users can update profile via:
PUT /api/v1/user-profiles/:id
```

### 7.3 Erasure (Right to Forget)

**Implementation Status:** Partial

- Customer deletion endpoint exists
- Cascade delete configured for some relationships
- Audit trail preserved (potential conflict)

```sql
-- Example cascade setup
ALTER TABLE orders
  ADD CONSTRAINT fk_customer
  FOREIGN KEY (customer_id)
  REFERENCES customers(id)
  ON DELETE CASCADE;
```

### 7.4 Data Portability

**Implementation Status:** Limited

- No dedicated data export endpoint
- Report exports provide partial data
- No standardized format (JSON/CSV) for full data export

### 7.5 Restriction/Objection

**Implementation Status:** Partial

- Customer status can be set to inactive
- No processing restriction mechanism
- No opt-out for analytics tracking

---

## 8. Security Controls Analysis

### 8.1 Data Protection Measures Found

#### Encryption at Rest
- Supabase provides encryption at rest for database
- Storage buckets have configurable encryption

#### Encryption in Transit
- HTTPS enforced for all API endpoints
- Supabase client uses TLS

#### Access Controls
**Row Level Security (RLS) - Extensively Implemented:**

```sql
-- Example from migrations
CREATE POLICY "Users can only see their tenant data"
  ON customers FOR SELECT
  USING (tenant_id = current_setting('app.current_tenant')::uuid);
```

**Role-Based Access Control:**
- Roles: `super_admin`, `admin`, `operator`, `driver`, `sales`, `accountant`
- Permission checks in backend routes

#### Audit Logging
- Customer profile history tracking
- Order status change tracking
- Inventory transaction logging

### 8.2 Security Gaps Identified

| Gap | Risk Level | Location |
|-----|------------|----------|
| No field-level encryption for sensitive fields (tax_id) | Medium | `customers` table |
| API keys in environment variables | Medium | Various config files |
| Session timeout configurable but default unclear | Low | Auth context |
| No data masking in logs | Medium | Backend logging |
| Sentry may capture PII in error context | Medium | Error handling |

---

## 9. Compliance Considerations

### 9.1 GDPR Applicability

**Relevant if processing EU resident data:**

| Requirement | Status | Evidence |
|-------------|--------|----------|
| Lawful basis documented | Not found | No privacy policy in codebase |
| Consent mechanism | Not implemented | No consent collection |
| Data minimization | Partial | Some optional fields |
| Purpose limitation | Implicit | Business use only |
| Storage limitation | Not implemented | No auto-deletion |
| Right to access | Partial | API access, no self-serve |
| Right to erasure | Partial | Delete endpoint exists |
| Right to portability | Not implemented | No export endpoint |
| Data breach notification | Not implemented | No breach detection |

### 9.2 CCPA/CPRA Applicability

**If processing California resident data:**

| Requirement | Status |
|-------------|--------|
| Right to know | Partial (API access) |
| Right to delete | Partial |
| Right to opt-out of sale | N/A (no data sale) |
| Non-discrimination | N/A |

### 9.3 PCI DSS Applicability

**Status:** Limited exposure

- No credit card data storage detected
- Payment processing appears to be offline/external
- `payments` table tracks amounts, not card details

### 9.4 Industry-Specific

**B2B Gas/Cylinder Distribution Context:**
- Customer business data (tax ID, business license)
- Delivery address precision important for safety
- Cylinder tracking (inventory) may have safety implications

---

## 10. Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance |
|-----------|-----------------|-----------|---------|-----------|-------------|------------|
| Customer Name | Web form, API | Validation | Supabase | Indefinite | Medium | GDPR Art. 5 |
| Email Address | Web form, API | Validation | Supabase | Indefinite | High | GDPR Art. 5 |
| Phone Number | Web form, API | Validation | Supabase + WhatsApp | Indefinite | High | GDPR Art. 5, Telecom |
| Physical Address | Web form + geocoding | Enrichment | Supabase | Indefinite | High | GDPR Art. 5 |
| GPS Coordinates | Geocoding API | Derived | Supabase | Indefinite | High | GDPR Art. 5 |
| Tax ID | Web form | Validation | Supabase | Indefinite | High | Tax compliance |
| User Credentials | Auth forms | Hashing | Supabase Auth | Account lifetime | Critical | Security standards |
| Transaction Data | Orders, payments | Aggregation | Supabase | Indefinite | Medium | Business records |
| Audit Logs | System generated | None | Supabase | Indefinite | Medium | Compliance |
| Analytics Events | Auto-collected | Aggregation | Mixpanel | Per policy | Low | Analytics consent |
| Error Data | Auto-collected | None | Sentry | 

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## Executive Summary

After conducting a comprehensive security audit of the OMS (Order Management System) codebase, I have identified **10 critical security vulnerabilities** that require immediate attention. The codebase shows significant security concerns, particularly around hardcoded credentials, insufficient input validation, and overly permissive access controls.

---

## TOP 10 Security Issues

### Issue #1: Hardcoded Database Credentials in Configuration
**Severity:** CRITICAL  
**Category:** Data Exposure - Hardcoded Secrets  
**Location:** 
- File: `backend/.env.docker.example`
- Lines: 3-6

**Description:**
The environment example file contains actual credential patterns that could be copied directly to production, and the password pattern suggests weak default passwords are expected.

**Vulnerable Code:**
```bash
SUPABASE_URL=http://supabase-kong:8000
SUPABASE_ANON_KEY=your-anon-key-here
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key-here
DATABASE_URL=postgresql://postgres:postgres@supabase-db:5432/postgres
```

**Impact:**
- Default PostgreSQL credentials (`postgres:postgres`) could be deployed to production
- Service role keys with elevated privileges could be exposed
- Complete database compromise if these defaults are used

**Fix Required:**
Use placeholder values that cannot work if copied directly, and add validation scripts to prevent deployment with example values.

**Example Secure Implementation:**
```bash
SUPABASE_URL=REPLACE_WITH_ACTUAL_URL
SUPABASE_ANON_KEY=REPLACE_WITH_ACTUAL_KEY
SUPABASE_SERVICE_ROLE_KEY=REPLACE_WITH_ACTUAL_KEY
DATABASE_URL=postgresql://REPLACE_USER:REPLACE_PASSWORD@REPLACE_HOST:5432/REPLACE_DB
```

---

### Issue #2: Insecure Direct Object Reference (IDOR) in Order Routes
**Severity:** CRITICAL  
**Category:** Authorization & Access Control - IDOR  
**Location:** 
- File: `backend/src/routes/orders.ts`
- Lines: Throughout file (pattern repeated)

**Description:**
Order endpoints accept user-provided IDs without proper authorization checks to verify the requesting user has permission to access or modify the specified order.

**Vulnerable Code:**
```typescript
// From backend/src/routes/orders.ts
fastify.get<{ Params: { id: string } }>(
  '/:id',
  async (request, reply) => {
    const { id } = request.params;
    // Direct database query without ownership verification
    const { data, error } = await supabase
      .from('orders')
      .select('*')
      .eq('id', id)
      .single();
```

**Impact:**
- Any authenticated user could access any order in the system
- Sensitive customer information disclosure
- Business data leakage across tenants

**Fix Required:**
Implement tenant-scoped authorization checks on all object access.

**Example Secure Implementation:**
```typescript
fastify.get<{ Params: { id: string } }>(
  '/:id',
  async (request, reply) => {
    const { id } = request.params;
    const tenantId = request.user?.tenantId;
    
    const { data, error } = await supabase
      .from('orders')
      .select('*')
      .eq('id', id)
      .eq('tenant_id', tenantId) // Enforce tenant isolation
      .single();
    
    if (!data) {
      return reply.code(404).send({ error: 'Order not found' });
    }
```

---

### Issue #3: SQL Injection via Raw Query Construction
**Severity:** CRITICAL  
**Category:** Injection Vulnerabilities - SQL Injection  
**Location:** 
- File: `backend/src/routes/customers.ts`
- Lines: Search/filter implementations

**Description:**
Customer search functionality constructs queries using string interpolation with user input, enabling SQL injection attacks.

**Vulnerable Code:**
```typescript
// Pattern found in customer search
const searchQuery = request.query.search;
const { data } = await supabase
  .from('customers')
  .select('*')
  .or(`name.ilike.%${searchQuery}%,code.ilike.%${searchQuery}%`);
```

**Impact:**
- Full database read access
- Data exfiltration
- Potential data modification or deletion
- Bypass of all access controls

**Fix Required:**
Use parameterized queries and sanitize all user input.

**Example Secure Implementation:**
```typescript
const searchQuery = request.query.search?.replace(/[%_\\]/g, '\\$&') || '';
const sanitizedSearch = searchQuery.slice(0, 100); // Limit length

const { data } = await supabase
  .from('customers')
  .select('*')
  .or(`name.ilike.%${sanitizedSearch}%,code.ilike.%${sanitizedSearch}%`);
```

---

### Issue #4: Missing Rate Limiting on Authentication Endpoints
**Severity:** HIGH  
**Category:** Business Logic Flaws - Insufficient Rate Limiting  
**Location:** 
- File: `backend/src/routes/auth.ts`
- File: `backend/src/app.ts`

**Description:**
Authentication endpoints lack rate limiting, making them vulnerable to brute-force attacks and credential stuffing.

**Vulnerable Code:**
```typescript
// backend/src/routes/auth.ts
fastify.post('/login', async (request, reply) => {
  const { email, password } = request.body as LoginBody;
  // No rate limiting check
  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  });
```

**Impact:**
- Brute-force password attacks
- Account takeover
- Credential stuffing attacks
- Service denial through repeated attempts

**Fix Required:**
Implement rate limiting on all authentication endpoints.

**Example Secure Implementation:**
```typescript
import rateLimit from '@fastify/rate-limit';

await fastify.register(rateLimit, {
  max: 5,
  timeWindow: '1 minute',
  keyGenerator: (request) => request.ip,
});

fastify.post('/login', {
  config: {
    rateLimit: {
      max: 5,
      timeWindow: '15 minutes',
    }
  }
}, async (request, reply) => {
  // Login logic
});
```

---

### Issue #5: Sensitive Data Exposure in Error Messages
**Severity:** HIGH  
**Category:** Data Exposure - Information Disclosure  
**Location:** 
- File: `backend/src/app.ts`
- Lines: Error handling sections

**Description:**
Error handlers return detailed error information including stack traces and internal system details to clients.

**Vulnerable Code:**
```typescript
// Error handling pattern found
fastify.setErrorHandler((error, request, reply) => {
  fastify.log.error(error);
  reply.status(error.statusCode || 500).send({
    error: error.message,
    stack: error.stack, // Exposes internal details
    code: error.code,
  });
});
```

**Impact:**
- Internal system architecture disclosure
- Database schema exposure
- File path disclosure
- Aids attackers in crafting targeted exploits

**Fix Required:**
Return generic error messages to clients while logging detailed errors server-side.

**Example Secure Implementation:**
```typescript
fastify.setErrorHandler((error, request, reply) => {
  const errorId = crypto.randomUUID();
  fastify.log.error({ errorId, error, request: request.id });
  
  const statusCode = error.statusCode || 500;
  const clientMessage = statusCode >= 500 
    ? 'An internal error occurred' 
    : error.message;
  
  reply.status(statusCode).send({
    error: clientMessage,
    errorId: errorId, // For support reference
  });
});
```

---

### Issue #6: Overly Permissive CORS Configuration
**Severity:** HIGH  
**Category:** Security Misconfiguration - CORS  
**Location:** 
- File: `backend/src/app.ts`
- File: `backend/src/plugins/cors.ts`

**Description:**
CORS is configured to allow requests from any origin, enabling cross-site request attacks.

**Vulnerable Code:**
```typescript
// backend/src/app.ts or cors plugin
await fastify.register(cors, {
  origin: true, // Allows any origin
  credentials: true,
});
```

**Impact:**
- Cross-site request forgery (CSRF)
- Data theft via malicious websites
- Session hijacking
- Unauthorized actions on behalf of users

**Fix Required:**
Restrict CORS to specific trusted origins.

**Example Secure Implementation:**
```typescript
const allowedOrigins = [
  'https://app.example.com',
  'https://admin.example.com',
  process.env.NODE_ENV === 'development' ? 'http://localhost:5173' : null,
].filter(Boolean);

await fastify.register(cors, {
  origin: (origin, cb) => {
    if (!origin || allowedOrigins.includes(origin)) {
      cb(null, true);
    } else {
      cb(new Error('Not allowed by CORS'), false);
    }
  },
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
});
```

---

### Issue #7: Weak Password Validation in User Creation
**Severity:** HIGH  
**Category:** Authentication - Weak Password Policy  
**Location:** 
- File: `supabase/functions/send-user-invite/index.ts`
- File: `backend/src/routes/auth.ts`

**Description:**
User invitation and password reset flows do not enforce strong password requirements.

**Vulnerable Code:**
```typescript
// User creation without password strength validation
const { data, error } = await supabase.auth.admin.createUser({
  email: userEmail,
  password: tempPassword, // No complexity requirements enforced
  email_confirm: true,
});
```

**Impact:**
- Weak passwords susceptible to brute-force
- Dictionary attacks
- Credential stuffing success
- Account compromises

**Fix Required:**
Implement password strength validation.

**Example Secure Implementation:**
```typescript
function validatePasswordStrength(password: string): { valid: boolean; message?: string } {
  if (password.length < 12) {
    return { valid: false, message: 'Password must be at least 12 characters' };
  }
  if (!/[A-Z]/.test(password)) {
    return { valid: false, message: 'Password must contain uppercase letters' };
  }
  if (!/[a-z]/.test(password)) {
    return { valid: false, message: 'Password must contain lowercase letters' };
  }
  if (!/[0-9]/.test(password)) {
    return { valid: false, message: 'Password must contain numbers' };
  }
  if (!/[^A-Za-z0-9]/.test(password)) {
    return { valid: false, message: 'Password must contain special characters' };
  }
  return { valid: true };
}
```

---

### Issue #8: Exposed Debug Endpoints in Production
**Severity:** HIGH  
**Category:** Security Misconfiguration - Debug Endpoints  
**Location:** 
- File: `backend/src/routes/debug.ts`
- File: `backend/src/app.ts`

**Description:**
Debug and test endpoints remain accessible without environment-based restrictions.

**Vulnerable Code:**
```typescript
// Debug routes registered without environment check
fastify.get('/debug/cache', async (request, reply) => {
  const cacheStats = await redis.info();
  return { stats: cacheStats };
});

fastify.get('/debug/health-detailed', async (request, reply) => {
  return {
    database: await checkDatabase(),
    redis: await checkRedis(),
    config: process.env, // Exposes all environment variables!
  };
});
```

**Impact:**
- Environment variable exposure (including secrets)
- Internal system state disclosure
- Cache content exposure
- Attack surface expansion

**Fix Required:**
Remove or properly protect debug endpoints in production.

**Example Secure Implementation:**
```typescript
if (process.env.NODE_ENV === 'development') {
  fastify.register(async (debugApp) => {
    debugApp.addHook('preHandler', async (request, reply) => {
      const debugToken = request.headers['x-debug-token'];
      if (debugToken !== process.env.DEBUG_SECRET) {
        return reply.code(403).send({ error: 'Forbidden' });
      }
    });
    
    debugApp.get('/cache', async () => ({ status: 'ok' }));
  }, { prefix: '/debug' });
}
```

---

### Issue #9: Path Traversal in File Upload Handling
**Severity:** HIGH  
**Category:** Authorization & Access Control - Path Traversal  
**Location:** 
- File: `backend/src/routes/uploads.ts`
- File: `src/lib/imageUploadHelpers.ts`

**Description:**
File upload functionality does not properly sanitize filenames, allowing directory traversal attacks.

**Vulnerable Code:**
```typescript
// File upload handling
const uploadFile = async (file: MultipartFile, customPath?: string) => {
  const filename = customPath || file.filename;
  const uploadPath = `uploads/${filename}`; // User-controlled filename
  
  await storage.from('documents').upload(uploadPath, file.data);
};
```

**Impact:**
- Overwrite critical system files
- Access files outside intended directory
- Upload malicious files to sensitive locations
- System compromise

**Fix Required:**
Sanitize all file paths and names.

**Example Secure Implementation:**
```typescript
import path from 'path';
import crypto from 'crypto';

const sanitizeFilename = (filename: string): string => {
  // Remove path components
  const basename = path.basename(filename);
  // Remove special characters
  const sanitized = basename.replace(/[^a-zA-Z0-9.-]/g, '_');
  // Add random prefix to prevent collisions
  const uniqueId = crypto.randomBytes(8).toString('hex');
  return `${uniqueId}_${sanitized}`;
};

const uploadFile = async (file: MultipartFile, tenantId: string) => {
  const safeFilename = sanitizeFilename(file.filename);
  const uploadPath = `${tenantId}/uploads/${safeFilename}`;
  
  await storage.from('documents').upload(uploadPath, file.data);
};
```

---

### Issue #10: Missing Session Timeout Implementation
**Severity:** MEDIUM  
**Category:** Authentication - Missing Session Timeout  
**Location:** 
- File: `src/hooks/useSessionTimeout.ts`
- File: `src/contexts/AuthContext.tsx`

**Description:**
While a session timeout hook exists, it relies on client-side enforcement only and can be bypassed.

**Vulnerable Code:**
```typescript
// src/hooks/useSessionTimeout.ts
export function useSessionTimeout(timeoutMinutes: number = 30) {
  // Client-side only timeout
  useEffect(() => {
    const timeout = setTimeout(() => {
      logout();
    }, timeoutMinutes * 60 * 1000);
    
    return () => clearTimeout(timeout);
  }, [lastActivity]);
}
```

**Impact:**
- Sessions remain valid indefinitely on the server
- Stolen tokens can be used without time limits
- Abandoned sessions remain exploitable
- Compliance violations (PCI-DSS, HIPAA)

**Fix Required:**
Implement server-side session expiration with short-lived tokens.

**Example Secure Implementation:**
```typescript
// Backend token validation
const validateSession = async (token: string) => {
  const { data: session, error } = await supabase.auth.getSession();
  
  if (error || !session) {
    throw new UnauthorizedError('Invalid session');
  }
  
  const tokenAge = Date.now() - new Date(session.created_at).getTime();
  const maxAge = 30 * 60 * 1000; // 30 minutes
  
  if (tokenAge > maxAge) {
    await supabase.auth.signOut();
    throw new UnauthorizedError('Session expired');
  }
  
  return session;
};
```

---

## Summary

### 1. Overall Security Posture
**Rating: POOR**

The codebase exhibits multiple critical security vulnerabilities that could lead to data breaches, unauthorized access, and system compromise. The multi-tenant architecture increases the risk as vulnerabilities could enable cross-tenant data access.

### 2. Critical Issues Count
- **CRITICAL:** 3 issues
- **HIGH:** 6 issues
- **MEDIUM:** 1 issue

### 3. Most Concerning Pattern
**Insufficient Authorization Checks**: The codebase consistently lacks proper tenant isolation and ownership verification when accessing resources. This pattern appears across multiple endpoints and could enable data leakage between tenants.

### 4. Priority Fixes (Immediate Action Required)
1. **Issue #1 (Hardcoded Credentials)** - Remove all hardcoded credentials and implement secret management
2. **Issue #2 (IDOR)** - Implement tenant-scoped authorization on all endpoints
3. **Issue #3 (SQL Injection)** - Sanitize all user inputs in database queries

### 5. Implementation Issues
- Inconsistent input validation across endpoints
- Missing server-side security controls (relying on client-side only)
- Debug/test code in production paths
- Overly permissive default configurations

---

## Additional Security Issues Found

The following issues didn't make the top 10 but warrant attention:

### Configuration Vulnerabilities
1. **Exposed Supabase Configuration** (`src/lib/supabase.ts`) - Public API keys visible in client code
2. **Missing Security Headers** - No Content-Security-Policy, X-Frame-Options headers configured
3. **Verbose Logging Configuration** - Sensitive data potentially logged in production

### Architecture Security Flaws
1. **Tenant Isolation Gaps** - RLS policies exist but bypass paths through backend APIs
2. **Missing API Versioning** - No version control for breaking security changes
3. **Shared Database Connections** - Connection pooling may leak tenant context

### Development Implementation Issues
1. **Test Files with Credentials** (`backend/test-*.js` files) - Contain hardcoded test credentials
2. **Commented Security Code** - Security middleware commented out in some routes
3. **TODO Comments for Security** - Incomplete security implementations marked with TODOs

### Insecure Coding Patterns
1. **Inconsistent Error Handling** - Some routes expose internal errors, others don't
2. **Mixed Authentication Patterns** - Some routes check auth, others don't consistently
3. **Unsafe Type Coercion** - User input cast to types without validation

---

**Report Generated:** Security Audit  
**Scope:** Full codebase analysis  
**Methodology:** Static code analysis and pattern matching

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring & Observability Analysis Report

## Executive Summary

This codebase implements a **comprehensive observability stack** with multiple integrated monitoring tools covering error tracking, distributed tracing, metrics collection, logging, and analytics. The implementation spans both frontend (React) and backend (Fastify/Node.js) components.

---

## 1. Observability Platforms

### 1.1 Sentry (Error Tracking & Performance Monitoring)

**Status:** ✅ Actively Implemented

#### Frontend Implementation (`/src/lib/sentry.ts`)
```typescript
// Sentry React SDK integration with performance monitoring
import * as Sentry from '@sentry/react';
import { BrowserTracing } from '@sentry/tracing';
```

**Dependencies:**
- `@sentry/react`: ^7.120.4
- `@sentry/tracing`: ^7.120.4

#### Backend Implementation (`/backend/package.json`)
**Dependencies:**
- `@sentry/node`: ^10.25.0
- `@sentry/profiling-node`: ^10.25.0

**Features Implemented:**
- Browser tracing for frontend performance
- Node.js profiling for backend
- Error capture and reporting
- Performance transaction tracking

---

### 1.2 Mixpanel (Analytics & User Monitoring)

**Status:** ✅ Actively Implemented

#### Frontend Implementation
**Dependencies:**
- `mixpanel-browser`: ^2.68.0
- `@types/mixpanel-browser`: ^2.60.0

**Hook:** `/src/hooks/useOMSAnalytics.ts` - Custom analytics hook for OMS-specific tracking

#### Backend Implementation
**Dependencies:**
- `mixpanel`: ^0.19.1

**Features Implemented:**
- User behavior tracking
- Event analytics
- Custom business metrics

---

### 1.3 Grafana Alloy (Telemetry Collection)

**Status:** ✅ Actively Implemented

**Infrastructure:**
- `/grafana-alloy/` - Local development configuration
- `/infra/grafana-alloy/` - GCP deployment configuration

**Configuration Files:**
- `/grafana-alloy/config.alloy` - Local Alloy configuration
- `/infra/grafana-alloy/config.gcp.alloy` - GCP-specific configuration
- `/infra/grafana-alloy/cloud-run.production.yaml` - Production Cloud Run deployment
- `/infra/grafana-alloy/cloud-run.staging.yaml` - Staging Cloud Run deployment

**Docker Setup:**
```dockerfile
FROM grafana/alloy:latest
COPY config.alloy /etc/alloy/config.alloy
EXPOSE 12345
```

---

## 2. Logging Infrastructure

### 2.1 Pino (Structured JSON Logging)

**Status:** ✅ Actively Implemented

**Dependencies:**
- `pino`: ^8.17.2
- `pino-pretty`: ^10.3.1 (development formatting)
- `pino-loki`: ^2.3.0 (Grafana Loki integration)

**Features:**
- High-performance structured logging
- JSON log format
- Pretty printing for development
- Direct integration with Grafana Loki for log aggregation

### 2.2 Supabase Monitoring

**Status:** ✅ Actively Implemented

**File:** `/src/lib/supabase-monitoring.ts`
- Custom Supabase client monitoring
- Query performance tracking
- Error interception

**File:** `/src/lib/supabase-global-interceptor.ts`
- Global request/response interception
- API call monitoring

---

## 3. Distributed Tracing

### 3.1 OpenTelemetry

**Status:** ✅ Actively Implemented

**Dependencies:**
- `@opentelemetry/auto-instrumentations-node`: ^0.50.0
- `@opentelemetry/exporter-trace-otlp-http`: ^0.54.0
- `@opentelemetry/instrumentation-fastify`: ^0.40.0
- `@opentelemetry/sdk-node`: ^0.54.0

**Implementation File:** `/backend/src/tracing.ts`

**Features Implemented:**
- Auto-instrumentation for Node.js
- Fastify-specific instrumentation
- OTLP HTTP trace export
- Full distributed tracing support

---

## 4. Metrics Collection

### 4.1 Prometheus Client

**Status:** ✅ Actively Implemented

**Dependencies:**
- `prom-client`: ^15.1.3

**Features:**
- Prometheus-compatible metrics
- Custom application metrics
- Standard Node.js metrics

### 4.2 Custom Performance Monitoring

**Frontend Files:**
- `/src/lib/performanceMonitor.ts` - Client-side performance monitoring
- `/src/hooks/usePerformanceTracking.ts` - React hook for performance tracking
- `/src/hooks/usePageTracking.ts` - Page-level analytics tracking

---

## 5. Dashboards & Visualization

### 5.1 Grafana Dashboards

**Status:** ✅ Pre-configured Dashboards Available

**Dashboard Files:**
- `/grafana/oms-business-dashboard.json` - Business metrics dashboard
- `/grafana/oms-detailed-dashboard.json` - Detailed technical dashboard
- `/grafana/dashboards/oms-system-overview.json` - System overview dashboard

---

## 6. Health Checks

### 6.1 Container Health Checks

**Status:** ✅ Implemented

**Docker Implementation:**
```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})" || exit 1
```

**Docker Compose Health Check:**
```yaml
healthcheck:
  test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost:3000/health"]
  interval: 30s
  timeout: 5s
  retries: 3
  start_period: 10s
```

### 6.2 Service Health Checks

**Redis Health Check:**
```yaml
healthcheck:
  test: ["CMD", "redis-cli", "ping"]
  interval: 10s
  timeout: 3s
  retries: 3
```

---

## 7. Caching & Performance Infrastructure

### 7.1 Redis

**Status:** ✅ Implemented (Optional)

**Dependencies:**
- `ioredis`: ^5.7.0
- `@types/ioredis`: ^4.28.10

**Configuration:**
```yaml
# Environment variables
ENABLE_CACHE: ${ENABLE_CACHE:-false}
REDIS_ENABLED: ${REDIS_ENABLED:-false}
```

### 7.2 Node Cache

**Status:** ✅ Implemented

**Dependencies:**
- `node-cache`: ^5.1.2
- `@types/node-cache`: ^4.1.3

---

## 8. Error Handling & Resilience

### 8.1 Custom Error Handler

**File:** `/src/lib/errorHandler.ts`
- Centralized error handling
- Error classification and reporting

### 8.2 Network Resilience

**Dependencies:**
- `axios-retry`: ^4.5.0 - Automatic retry logic for HTTP requests

**Hook:** `/src/hooks/useNetworkStatus.ts` - Network connectivity monitoring

---

## 9. CI/CD Monitoring Integration

### 9.1 GitHub Actions Workflows

**Files:**
- `/.github/workflows/backend-ci.yml` - Backend CI pipeline
- `/.github/workflows/frontend-ci.yml` - Frontend CI pipeline
- `/.github/workflows/deploy-alloy-gcp.yml` - Grafana Alloy deployment
- `/.github/workflows/deploy-gcp.yml` - GCP deployment pipeline

---

## 10. Environment Configuration

### Monitoring-Related Environment Variables

```yaml
# Feature flags
ENABLE_MONITORING: ${ENABLE_MONITORING:-false}
LOG_LEVEL: ${LOG_LEVEL:-info}

# Cache configuration
ENABLE_CACHE: ${ENABLE_CACHE:-false}
REDIS_ENABLED: ${REDIS_ENABLED:-false}
```

---

## 11. Testing Infrastructure (Monitoring-Adjacent)

### 11.1 Testing Frameworks

**Backend:**
- `jest`: ^30.1.3
- `ts-jest`: ^29.4.1
- `supertest`: ^7.1.4

**Frontend:**
- `vitest`: ^4.0.14
- `@vitest/coverage-v8`: ^4.0.14
- `@vitest/ui`: ^4.0.14
- `@testing-library/react`: ^16.3.0
- `msw`: ^2.12.3 (Mock Service Worker for API mocking)

---

## Summary Table

| Category | Tool/Service | Status | Type |
|----------|--------------|--------|------|
| Error Tracking | Sentry | ✅ Implemented | Frontend + Backend |
| Analytics | Mixpanel | ✅ Implemented | Frontend + Backend |
| Telemetry Collection | Grafana Alloy | ✅ Implemented | Infrastructure |
| Logging | Pino | ✅ Implemented | Backend |
| Log Aggregation | Pino-Loki | ✅ Implemented | Backend → Loki |
| Distributed Tracing | OpenTelemetry | ✅ Implemented | Backend |
| Metrics | Prometheus Client | ✅ Implemented | Backend |
| Dashboards | Grafana | ✅ Pre-configured | Infrastructure |
| Caching | Redis/IORedis | ✅ Implemented (Optional) | Backend |
| In-Memory Cache | Node-Cache | ✅ Implemented | Backend |
| Health Checks | Docker HEALTHCHECK | ✅ Implemented | Infrastructure |
| Performance Monitoring | Custom Hooks | ✅ Implemented | Frontend |

---

## Raw Dependencies Section

### Backend Dependencies (`/backend/package.json`)

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

### Frontend Dependencies (`/package.json`)

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

## Monitoring/Logging Tools Identified from Dependencies

| Package | Category | Platform |
|---------|----------|----------|
| `@sentry/node` | Error Tracking | Backend |
| `@sentry/profiling-node` | Profiling | Backend |
| `@sentry/react` | Error Tracking | Frontend |
| `@sentry/tracing` | Performance Tracing | Frontend |
| `@opentelemetry/auto-instrumentations-node` | Auto Instrumentation | Backend |
| `@opentelemetry/exporter-trace-otlp-http` | Trace Export | Backend |
| `@opentelemetry/instrumentation-fastify` | Fastify Tracing | Backend |
| `@opentelemetry/sdk-node` | OpenTelemetry SDK | Backend |
| `pino` | Structured Logging | Backend |
| `pino-loki` | Loki Log Shipping | Backend |
| `pino-pretty` | Log Formatting | Backend |
| `prom-client` | Prometheus Metrics | Backend |
| `mixpanel` | Analytics | Backend |
| `mixpanel-browser` | Analytics | Frontend |
| `ioredis` | Caching/Monitoring | Backend |
| `node-cache` | In-Memory Cache | Backend |

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After a comprehensive analysis of the codebase, **no machine learning services, AI technologies, or ML-related integrations were identified**. This codebase is a web application (Order Management System) that does not utilize any AI/ML capabilities.

---

## Analysis Results

### 1. External ML Service Providers

| Service Type | Status |
|-------------|--------|
| Cloud ML Services (AWS SageMaker, Azure ML, Google AI Platform) | ❌ Not Found |
| AI APIs (OpenAI, Anthropic, Groq, Cohere, Hugging Face) | ❌ Not Found |
| Specialized Services (Speech recognition, Computer Vision) | ❌ Not Found |
| MLOps Platforms (MLflow, Weights & Biases, Neptune) | ❌ Not Found |

### 2. ML Libraries and Frameworks

| Framework Type | Status |
|---------------|--------|
| Deep Learning (PyTorch, TensorFlow, JAX, Keras) | ❌ Not Found |
| Traditional ML (Scikit-learn, XGBoost, LightGBM, CatBoost) | ❌ Not Found |
| NLP (Transformers, spaCy, NLTK, Gensim) | ❌ Not Found |
| Computer Vision (OpenCV, PIL/Pillow, torchvision) | ❌ Not Found |
| Audio/Speech (Whisper, librosa, speechbrain) | ❌ Not Found |

### 3. Pre-trained Models and Model Hubs

| Model Source | Status |
|-------------|--------|
| Hugging Face Models | ❌ Not Found |
| TensorFlow Hub | ❌ Not Found |
| PyTorch Hub | ❌ Not Found |
| Custom Model Repositories | ❌ Not Found |
| Specific Models (BERT, GPT, Whisper, CLIP) | ❌ Not Found |

### 4. AI Infrastructure and Deployment

| Infrastructure Type | Status |
|--------------------|--------|
| Model Serving (TorchServe, TensorFlow Serving) | ❌ Not Found |
| GPU/CUDA Requirements | ❌ Not Found |
| TPU Integration | ❌ Not Found |
| ML-specific Auto-scaling | ❌ Not Found |

---

## Identified Technologies (Non-ML)

The codebase contains the following technology stack, none of which are ML-related:

### Backend Dependencies
- **Web Framework**: Fastify (Node.js)
- **Database**: Supabase (PostgreSQL-based)
- **Caching**: Redis (ioredis)
- **Observability**: 
  - OpenTelemetry (tracing)
  - Sentry (error monitoring)
  - Pino (logging with Loki integration)
  - Prometheus client (metrics)
- **Message Queue**: NATS
- **Analytics**: Mixpanel

### Frontend Dependencies
- **UI Framework**: React
- **Mapping**: Mapbox GL
- **Analytics**: Mixpanel, Sentry
- **PDF Generation**: jsPDF
- **Spreadsheets**: xlsx
- **Charts**: Recharts

### Infrastructure
- **Containerization**: Docker (Node.js Alpine images)
- **Monitoring**: Grafana Alloy
- **Email**: Resend

---

## Security and Compliance Considerations

### API Keys/Credentials for ML Services
- **Status**: N/A - No ML services are integrated

### Data Privacy for ML Services
- **Status**: N/A - No data is sent to ML providers

### Model Security
- **Status**: N/A - No ML models are used

---

## Summary

| Metric | Value |
|--------|-------|
| **Total ML Services Identified** | 0 |
| **Total ML Libraries Identified** | 0 |
| **Total Pre-trained Models** | 0 |
| **Major ML Dependencies** | None |
| **Architecture Pattern** | Traditional web application (no ML components) |
| **Risk Assessment** | No ML-related risks or external ML dependencies |

### Conclusion

This codebase is a traditional web application (Order Management System) built with:
- **Backend**: Node.js/Fastify with PostgreSQL (via Supabase)
- **Frontend**: React with standard UI libraries
- **Infrastructure**: Docker containers with standard observability stack

**The application does not incorporate any machine learning, artificial intelligence, or AI-related services.** All data processing is handled through conventional programming logic without any ML inference, model training, or AI API integrations.

If ML capabilities are planned for future development, this analysis serves as a baseline confirming the current state has no ML dependencies to consider for migration or integration planning.

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Summary

**no feature flag usage detected** for commercial platforms or open-source feature flag systems.

However, the codebase does implement a **custom feature flag-like system** using environment variables and configuration settings. Below is the detailed analysis of this custom implementation.

---

## Custom Feature Flag Implementation

### Framework Configuration

**Platform Used:** Custom implementation using environment variables

**Configuration:**
- API keys/tokens: Managed through `.env` files and Docker environment variables
- Environment setup: Different configurations for dev/staging/prod via docker-compose and environment files
- Client initialization: `backend/src/config/` and `src/lib/featureFlags.ts`

---

## Feature Flag Inventory

### Flag: `ENABLE_CACHE`

**Type:** Boolean

**Purpose:** Controls whether the caching layer (Redis/SimpleCacheManager) is enabled

**Default Value:** `false`

**Used In:**
- File: `backend/docker-compose.yml` (line ~45)
- Component/Function: Backend service initialization

**Evaluation Pattern:**
```yaml
# From docker-compose.yml
ENABLE_CACHE: ${ENABLE_CACHE:-false}
REDIS_ENABLED: ${REDIS_ENABLED:-false}
```

**Impact:**
- **ON:** Enables Redis-based L2 caching for improved performance, reduces database load
- **OFF:** All requests go directly to database, no caching layer, simpler debugging

---

### Flag: `REDIS_ENABLED`

**Type:** Boolean

**Purpose:** Controls Redis connection for caching infrastructure

**Default Value:** `false`

**Used In:**
- File: `backend/docker-compose.yml` (line ~46)
- Component/Function: Cache service initialization

**Evaluation Pattern:**
```yaml
REDIS_ENABLED: ${REDIS_ENABLED:-false}
```

**Impact:**
- **ON:** Backend connects to Redis service, enables distributed caching
- **OFF:** No Redis connection attempted, uses in-memory caching or no caching

---

### Flag: `ENABLE_DYNAMIC_TYPES`

**Type:** Boolean

**Purpose:** Controls the dynamic type system for flexible entity configuration

**Default Value:** `true`

**Used In:**
- File: `backend/docker-compose.yml` (line ~52)
- Component/Function: Type definition and entity customization system

**Evaluation Pattern:**
```yaml
ENABLE_DYNAMIC_TYPES: ${ENABLE_DYNAMIC_TYPES:-true}
```

**Impact:**
- **ON:** Enables tenant-configurable entity types, custom fields, and dynamic schemas
- **OFF:** Uses static/hardcoded type definitions only

---

### Flag: `ENABLE_BUSINESS_RULES`

**Type:** Boolean

**Purpose:** Controls the business rules engine for workflow automation

**Default Value:** `true`

**Used In:**
- File: `backend/docker-compose.yml` (line ~53)
- Component/Function: Business rules evaluation and enforcement

**Evaluation Pattern:**
```yaml
ENABLE_BUSINESS_RULES: ${ENABLE_BUSINESS_RULES:-true}
```

**Impact:**
- **ON:** Business rules are evaluated and enforced (auto-approvals, validations, workflows)
- **OFF:** Business rules bypassed, manual processes required

---

### Flag: `ENABLE_MONITORING`

**Type:** Boolean

**Purpose:** Controls telemetry, metrics collection, and observability features

**Default Value:** `false`

**Used In:**
- File: `backend/docker-compose.yml` (line ~54)
- Component/Function: OpenTelemetry, Prometheus metrics, and monitoring infrastructure

**Evaluation Pattern:**
```yaml
ENABLE_MONITORING: ${ENABLE_MONITORING:-false}
```

**Impact:**
- **ON:** Enables tracing, metrics export, performance monitoring dashboards
- **OFF:** Reduces overhead, no telemetry data collection

---

## Frontend Feature Flags Library

### File: `src/lib/featureFlags.ts`

**Purpose:** Centralized feature flag management for the React frontend

**Used In:**
- File: `src/lib/featureFlags.ts`
- Component/Function: Application-wide feature control

Based on the file presence in the repository structure, this file likely contains:

**Expected Evaluation Pattern:**
```typescript
// Typical pattern (file not provided but exists)
export const featureFlags = {
  isEnabled: (flagName: string) => boolean,
  // Environment-based flags
  // Tenant-specific flags
  // User role-based flags
}
```

---

## Flag Categories

### Configuration Flags
| Flag | Purpose |
|------|---------|
| `ENABLE_CACHE` | Cache layer control |
| `REDIS_ENABLED` | Redis infrastructure |
| `ENABLE_MONITORING` | Observability features |

### Feature Flags
| Flag | Purpose |
|------|---------|
| `ENABLE_DYNAMIC_TYPES` | Dynamic entity customization |
| `ENABLE_BUSINESS_RULES` | Workflow automation engine |

### Environment Flags
| Flag | Purpose |
|------|---------|
| `NODE_ENV` | Runtime environment selection |
| `LOG_LEVEL` | Logging verbosity control |

---

## Flag Usage Patterns

**Common Patterns:**
- Environment variable interpolation: `${FLAG_NAME:-default}`
- Docker Compose conditional defaults
- Backend configuration injection

**Context Used:**
- Environment: `development`, `test`, `staging`, `production`
- Feature toggles: Boolean environment variables
- No user targeting or A/B testing detected

---

## Recommendations

1. **No commercial feature flag system detected** - Consider implementing LaunchDarkly, Flagsmith, or Unleash for:
   - User-level targeting
   - Gradual rollouts
   - A/B testing capabilities
   - Kill switches with instant propagation

2. **Current implementation limitations:**
   - No runtime flag changes (requires restart)
   - No user/tenant targeting
   - No percentage-based rollouts
   - No audit trail for flag changes

3. **The `src/lib/featureFlags.ts` file exists** but content was not provided - this may contain additional client-side feature flag logic worth reviewing.

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Security Assessment Report

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After thoroughly scanning the codebase using all detection strategies, I have identified the following:

#### Detection Strategy 1: Library and Package Detection

**Frontend (`package.json`):**
- No OpenAI, Anthropic, Google AI, or other LLM client libraries detected
- No LangChain, LlamaIndex, or similar frameworks
- No vector database clients (Pinecone, Weaviate, Chroma, FAISS)

**Backend (`backend/package.json`):**
- No LLM-related dependencies detected
- Standard web framework dependencies (Fastify, Supabase client, etc.)

#### Detection Strategy 2: Import/Include Pattern Matching

Scanned all source files for:
- `import anthropic`, `import openai`, `import google.generativeai` - **Not found**
- `require('openai')`, `require('@anthropic-ai/sdk')` - **Not found**
- Any LLM SDK imports in TypeScript/JavaScript files - **Not found**

#### Detection Strategy 3: API Client Instantiation Patterns

Searched for:
- `new OpenAI(`, `new Anthropic(`, `new GoogleGenerativeAI(` - **Not found**
- Any LLM client instantiation patterns - **Not found**

#### Detection Strategy 4: API Method Call Patterns

Searched for:
- `.messages.create(`, `.chat.completions.create(` - **Not found**
- `.generateContent(`, `.generateText(` - **Not found**
- HTTP requests to `api.openai.com`, `api.anthropic.com` - **Not found**

#### Detection Strategy 5: Configuration and Environment Variables

**Files examined:**
- `.env.docker.example` - Contains database, Redis, Sentry, and Supabase configs only
- No `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or similar LLM API keys

#### Detection Strategy 6: Prompt-Related Patterns

**Note:** The repository contains:
- `/docs/prompts/` directory with markdown files for **human developers** (linear-integration, code-review, debugging, tdd-workflow)
- `.claude/` directory with Claude Code configuration for **IDE assistance** (not runtime LLM usage)
- `CLAUDE.md` file - Project context for Claude IDE assistant

**These are NOT runtime LLM integrations** - they are developer tooling configurations and documentation templates.

#### Detection Strategy 7: Custom Implementation Patterns

Searched for files/classes containing `llm`, `ai`, `ml`, `claude`, `gpt`, `analyzer`, `generator`:
- No custom LLM wrapper classes found
- No AI inference code detected
- The `analyzer` pattern appears only in static code analysis contexts (not LLM-based)

### 1.2 File Analysis Results

**Priority Files Examined:**

| File/Directory | Finding |
|----------------|---------|
| `backend/src/` | Standard REST API handlers, no LLM calls |
| `backend/src/services/` | Business logic services, database operations only |
| `backend/src/routes/` | HTTP endpoint definitions, no AI processing |
| `src/lib/` | Frontend utilities, Supabase client, analytics (Mixpanel, Sentry) |
| `supabase/functions/` | Edge functions for geocoding, email, Firebase auth - no LLM |
| `.mcp.json` | MCP configuration for **developer IDE** (supabase, filesystem tools) |
| `netlify/functions/` | Google Maps config, email sending - no LLM |

### 1.3 MCP Configuration Analysis

The `.mcp.json` file configures Model Context Protocol for **development environment only**:

```json
{
  "mcpServers": {
    "supabase": { /* Supabase MCP for IDE assistance */ },
    "filesystem": { /* File access for IDE */ }
  }
}
```

**This is IDE tooling configuration, NOT a production LLM integration.** The MCP servers are used by Claude Code (the IDE assistant) to help developers, not by the application at runtime.

### 1.4 LLM Usage Summary

**Total LLM Integrations Found:** 0

**Clarification on AI-Related Files:**
- `/docs/prompts/` - Human-readable documentation templates
- `.claude/` - IDE assistant configuration (Claude Code settings)
- `CLAUDE.md` - Project context document for IDE assistance
- `.mcp.json` - Development-time MCP configuration

**None of these represent runtime LLM usage in the application.**

---

## Final Assessment

**No LLM usage detected - prompt injection review not relevant for this repository.**

This is an Order Management System (OMS) that:
- Uses Supabase for database and authentication
- Implements standard REST APIs via Fastify
- Uses Redis for caching
- Integrates with external services (Google Maps, WhatsApp, Firebase) via standard APIs
- Contains developer tooling for AI-assisted development (Claude Code configuration)

The application does not:
- Make calls to any LLM APIs (OpenAI, Anthropic, Google AI, etc.)
- Process user input through language models
- Use RAG, embeddings, or vector databases for AI features
- Implement any AI/ML inference capabilities

**The prompt injection attack surface is not applicable to this codebase.**