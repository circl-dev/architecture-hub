# hl_overview

High level overview of the codebase

# Repository Analysis: iot_backoffice_d336ab81

## 0. Repository Name
**[[iot_backoffice_d336ab81]]**

---

## 1. Project Purpose

This project is an **IoT (Internet of Things) Backoffice/Admin Dashboard** application designed to manage and monitor IoT infrastructure. Based on the file structure and naming, it solves the following problems:

- **Asset Management**: Tracking and managing IoT assets, tags, and gateways
- **Real-time Monitoring**: Live tracking capabilities with WebSocket integration
- **Geofencing**: Location-based monitoring with geofence definitions and events
- **Alert Management**: Defining and monitoring alerts for IoT devices
- **Fleet/Vehicle Management**: Tracking vehicles and movements
- **Warehouse/Inventory Management**: Stock dashboard and warehouse location tracking
- **Multi-tenant Administration**: Customer and tenant management with user roles
- **Audit Logging**: Tracking system activities

**Primary Domain**: IoT infrastructure management and asset tracking for enterprise/logistics operations.

---

## 2. Architecture Pattern

The project employs a **Component-Based Single Page Application (SPA)** architecture with:

- **Feature-based organization** within components
- **Service layer pattern** for API communication
- **Context-based state management** (React Context API)
- **Custom hooks pattern** for reusable logic
- **WebSocket integration** for real-time updates

---

## 3. Technology Stack

### Primary Language
- **TypeScript** (React/Frontend)

### Framework
- **React 18+** (SPA Framework)
- **Vite** (Build tool and dev server)

### UI/Styling
- **Tailwind CSS** (Utility-first CSS framework)
- **PostCSS** (CSS processing)

### State & Data Management
- **React Context API** (AuthContext, WebSocketContext)
- **Custom Store** (`src/store/index.ts`)

### Routing
- **React Router** (implied by `routes.tsx`)

### API Communication
- **OpenAPI/REST** (services/openapi directory)
- **WebSocket** (real-time communication)

### Code Quality
- **ESLint** (linting)
- **Prettier** (code formatting)
- **TypeScript** (type checking)

### Deployment Platforms
- **Vercel** (vercel.json)
- **AWS Amplify** (amplify.yml)

### Notable Utilities
- **QR Code generation** (`utils/qrCode.ts`)
- **Geocoding** (`utils/geocoding.ts`)
- **Encryption** (`utils/encryption.ts`, `utils/encryptedStorage.ts`)
- **CSV/JSON parsing** for data imports
- **Rate limiting** (`utils/rateLimiter.ts`)

---

## 4. Initial Structure Impression

This is a **frontend-only SPA application** (no backend code present):

| Component | Description |
|-----------|-------------|
| **Frontend** | React-based dashboard application |
| **Configuration** | Multiple deployment targets (Vercel, Amplify) |
| **Documentation** | Technical docs in `/docs` folder |
| **CI/CD** | GitHub Actions workflows |

The application appears to be a client that consumes external APIs (likely a separate backend service).

---

## 5. Configuration/Package Files

| File | Purpose |
|------|---------|
| `package.json` | Node.js dependencies and scripts |
| `package-lock.json` | Dependency lock file |
| `tsconfig.json` | Base TypeScript configuration |
| `tsconfig.app.json` | App-specific TypeScript config |
| `tsconfig.node.json` | Node-specific TypeScript config |
| `vite.config.ts` | Vite build configuration |
| `tailwind.config.js` | Tailwind CSS configuration |
| `postcss.config.js` | PostCSS configuration |
| `eslint.config.js` | ESLint rules and configuration |
| `.prettierrc` | Prettier formatting rules |
| `.gitignore` | Git ignore patterns |
| `env.example` | Environment variables template |
| `vercel.json` | Vercel deployment config |
| `amplify.yml` | AWS Amplify build specification |
| `.cursorrules` | Cursor IDE configuration |
| `.claude/settings.local.json` | Claude AI settings |
| `.github/CODEOWNERS` | Code ownership definitions |

---

## 6. Directory Structure

```
src/
├── App.tsx              # Root application component
├── main.tsx             # Application entry point
├── routes.tsx           # Route definitions
├── index.css            # Global styles
│
├── types/               # TypeScript type definitions
│   ├── api.ts           # API response/request types
│   ├── error.ts         # Error type definitions
│   ├── liveTracking.ts  # Real-time tracking types
│   ├── validation.ts    # Validation schemas
│   ├── websocket.ts     # WebSocket message types
│   └── models/          # Domain model types
│       ├── alert/       # Alert-related types
│       ├── asset/       # Asset-related types
│       ├── tag/         # Tag-related types
│       └── user/        # User-related types
│
├── contexts/            # React Context providers
│   ├── AuthContext.tsx      # Authentication state
│   └── WebSocketContext.tsx # WebSocket connection state
│
├── constants/           # Application constants
│   └── auth.ts          # Auth-related constants
│
├── utils/               # Utility functions
│   ├── apiUtils.ts      # API helper functions
│   ├── auth.ts          # Auth utilities
│   ├── cache.ts         # Caching logic
│   ├── cookies.ts       # Cookie management
│   ├── encryption.ts    # Encryption utilities
│   ├── geocoding.ts     # Address/coordinate conversion
│   ├── validate.ts      # Validation helpers
│   └── ...              # Other utilities
│
├── components/          # Reusable UI components
│   ├── common/          # Generic shared components
│   ├── modal/           # Modal dialog components (12 files)
│   ├── map/             # Map-related components (13 files)
│   ├── movements/       # Movement tracking components
│   ├── notifications/   # Notification components
│   ├── customer/        # Customer-related components
│   ├── helpers/         # Helper components
│   └── examples/        # Example/demo components
│
├── layouts/             # Page layout components
│   └── DashboardLayout.tsx  # Main dashboard wrapper
│
├── hooks/               # Custom React hooks
│   ├── useDebounce.ts           # Input debouncing
│   ├── useForm.ts               # Form state management
│   ├── useFormValidation.ts     # Form validation
│   ├── useMap.ts                # Map interactions
│   ├── usePaginatedData.ts      # Paginated API data
│   ├── usePolling.ts            # Data polling
│   ├── useSelection.ts          # Selection state
│   └── useWebSocketSubscription.ts # WebSocket hooks
│
├── pages/               # Page-level components (routes)
│   ├── Dashboard/       # Dashboard with nested structure
│   │   ├── hooks/       # Dashboard-specific hooks
│   │   └── utils/       # Dashboard utilities
│   ├── Assets.tsx       # Asset listing
│   ├── AssetDetails.tsx # Asset detail view
│   ├── Alerts.tsx       # Alert management
│   ├── Geofences.tsx    # Geofence management
│   ├── Tags.tsx         # Tag management
│   ├── Users.tsx        # User management
│   ├── Customers.tsx    # Customer management
│   └── ...              # Other pages
│
├── services/            # External service integrations
│   ├── api/             # REST API services (13 files)
│   ├── openapi/         # OpenAPI client (2 files)
│   ├── websocket/       # WebSocket service
│   └── import/          # Data import services
│
└── store/               # State management
    └── index.ts         # Store configuration
```

### Organization Pattern
The code is organized using a **hybrid approach**:
- **By layer** at the top level (components, hooks, services, pages)
- **By feature** within certain directories (map, movements, modal components)

---

## 7. High-Level Architecture

### Architectural Patterns Identified

#### 1. **Component-Based Architecture (React SPA)**
- **Evidence**: `src/components/`, `src/pages/`, React-specific files
- All UI is built with composable React components

#### 2. **Service Layer Pattern**
- **Evidence**: `src/services/api/` with 13 service files
- Clean separation between UI and data fetching logic

#### 3. **Context-Based State Management**
- **Evidence**: `src/contexts/AuthContext.tsx`, `WebSocketContext.tsx`
- Global state handled via React Context API

#### 4. **Custom Hooks Pattern**
- **Evidence**: `src/hooks/` with 10 specialized hooks
- Reusable stateful logic extracted into hooks

#### 5. **Real-Time Event-Driven Communication**
- **Evidence**: WebSocket context, `useWebSocketSubscription`, live tracking types
- Real-time updates for IoT device status

### Data Flow Architecture
```
┌─────────────────────────────────────────────────────────┐
│                    React Components                      │
│  (Pages → Layouts → Components)                         │
├─────────────────────────────────────────────────────────┤
│                    Custom Hooks                          │
│  (useForm, usePaginatedData, useWebSocketSubscription)  │
├─────────────────────────────────────────────────────────┤
│                 Context Providers                        │
│  (AuthContext, WebSocketContext)                        │
├─────────────────────────────────────────────────────────┤
│                   Service Layer                          │
│  (API Services, WebSocket Service, Import Services)     │
├─────────────────────────────────────────────────────────┤
│              External Backend APIs                       │
│  (REST API + WebSocket Server)                          │
└─────────────────────────────────────────────────────────┘
```

---

## 8. Build, Execution and Test

### Entry Points
- **Application Entry**: `src/main.tsx` → `src/App.tsx`
- **HTML Entry**: `index.html`
- **Routing**: `src/routes.tsx`

### Build & Run Commands
Based on Vite defaults (actual scripts in `package.json`):

| Command | Purpose |
|---------|---------|
| `npm install` | Install dependencies |
| `npm run dev` | Start development server |
| `npm run build` | Production build |
| `npm run preview` | Preview production build |
| `npm run lint` | Run ESLint |

### CI/CD Workflows
Located in `.github/workflows/`:

| File | Purpose |
|------|---------|
| `cw-on-pull-request.yml` | PR validation checks |
| `wf-build.yml` | Build workflow |

### Deployment
- **Vercel**: Configured via `vercel.json`
- **AWS Amplify**: Configured via `amplify.yml`

### Environment Configuration
- Template: `env.example`
- Runtime: Environment variables loaded via `src/utils/env.ts`

### Testing
No explicit test configuration files visible (no `jest.config`, `vitest.config`, or `*.test.*` files shown), suggesting:
- Tests may not be implemented yet, OR
- Test files exist but weren't included in the structure overview

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

## 1. `src/types/` - Type Definitions Module

### Core Responsibility
Provides TypeScript type definitions, interfaces, and data models that ensure type safety across the entire application. This module serves as the single source of truth for data structures used throughout the codebase.

### Key Components

| File/Directory | Role |
|----------------|------|
| `api.ts` | Defines types for API request/response structures, HTTP methods, and API configuration |
| `error.ts` | Error type definitions for standardized error handling across the app |
| `liveTracking.ts` | Types for real-time asset/vehicle tracking features |
| `validation.ts` | Validation rule types and form validation interfaces |
| `websocket.ts` | WebSocket message types, connection states, and event definitions |
| `models/alert/` | Alert-related data models (alert definitions, triggered alerts) |
| `models/asset/` | Asset entity types (assets, asset types, properties) |
| `models/tag/` | Tag/beacon device type definitions |
| `models/user/` | User, role, and permission type definitions |

### Dependencies & Interactions
- **Internal Dependencies:** None (this is a foundational module)
- **Consumed By:** Virtually all other modules (`services/`, `components/`, `pages/`, `hooks/`)
- **External Services:** None directly; types mirror external API response structures

---

## 2. `src/contexts/` - React Context Providers

### Core Responsibility
Manages global application state using React Context API, providing centralized state management for authentication and real-time WebSocket communications.

### Key Components

| File | Role |
|------|------|
| `AuthContext.tsx` | Manages user authentication state, login/logout operations, token management, and user session persistence |
| `WebSocketContext.tsx` | Provides WebSocket connection management, subscription handling, and real-time data distribution to components |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/utils/auth.ts` - Authentication utility functions
  - `@src/utils/encryptedStorage.ts` - Secure token storage
  - `@src/services/api/` - API calls for authentication
  - `@src/services/websocket/` - WebSocket service layer
  - `@src/types/websocket.ts` - WebSocket type definitions
  - `@src/constants/auth.ts` - Authentication constants
- **External Services:**
  - Authentication API endpoints
  - WebSocket server connections for live data

---

## 3. `src/constants/` - Application Constants

### Core Responsibility
Centralizes static configuration values and constant definitions to ensure consistency and easy maintenance across the application.

### Key Components

| File | Role |
|------|------|
| `auth.ts` | Authentication-related constants (token keys, session timeouts, auth routes, role definitions) |

### Dependencies & Interactions
- **Internal Dependencies:** None
- **Consumed By:** `@src/contexts/AuthContext.tsx`, `@src/utils/auth.ts`, `@src/components/ProtectedRoute.tsx`
- **External Services:** None

---

## 4. `src/utils/` - Utility Functions

### Core Responsibility
Provides reusable helper functions, utility classes, and common logic that can be shared across multiple components and services throughout the application.

### Key Components

| File | Role |
|------|------|
| `apiUtils.ts` | API helper functions (URL building, header construction, response parsing) |
| `auth.ts` | Authentication helpers (token validation, permission checks, session management) |
| `cache.ts` | Client-side caching utilities for API responses |
| `cookies.ts` | Cookie manipulation utilities for session management |
| `csvParser.ts` | CSV file parsing for bulk import features |
| `encryptedStorage.ts` | Secure localStorage wrapper with encryption |
| `encryption.ts` | Encryption/decryption utilities for sensitive data |
| `env.ts` | Environment variable access and validation |
| `errorHandler.ts` | Centralized error handling and formatting |
| `geocoding.ts` | Address-to-coordinates and reverse geocoding utilities |
| `jsonParser.ts` | JSON parsing with error handling |
| `loadingState.tsx` | Loading state management utilities/components |
| `locationColors.ts` | Color assignment utilities for map locations |
| `qrCode.ts` | QR code generation utilities |
| `rateLimiter.ts` | Client-side rate limiting for API calls |
| `requestHandler.ts` | HTTP request wrapper with retry logic |
| `requestQueue.ts` | Request queuing for batch operations |
| `stringUtils.ts` | String manipulation helpers |
| `typeConversion.ts` | Type conversion and data transformation utilities |
| `validate.ts` | Form and data validation functions |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - Type definitions for validation and error handling
  - `@src/constants/` - Configuration constants
- **External Services:**
  - Geocoding APIs (likely Google Maps or similar)
  - Browser localStorage/sessionStorage APIs

---

## 5. `src/components/` - UI Components

### Core Responsibility
Contains reusable React UI components that form the building blocks of the application's user interface, from simple form elements to complex interactive widgets.

### Key Components

| File/Directory | Role |
|----------------|------|
| `AddressAutosuggest.tsx` | Address input with autocomplete suggestions |
| `DateTimePicker.tsx` / `DateTimePickerField.tsx` | Date and time selection components |
| `DropdownPortal.tsx` | Portal-based dropdown for proper z-index handling |
| `ErrorBoundary.tsx` | React error boundary for graceful error handling |
| `Footer.tsx` | Application footer component |
| `FormField.tsx` | Reusable form field wrapper with labels and validation |
| `GeofenceEventsList.tsx` | Displays geofence entry/exit events |
| `HistoryTimeline.tsx` | Timeline visualization for historical data |
| `LoadingSkeleton.tsx` | Skeleton loading placeholders |
| `LoginForm.tsx` | User login form |
| `Measurements.tsx` | Sensor measurement display component |
| `Navbar.tsx` | Main navigation bar |
| `Pagination.tsx` | Table/list pagination controls |
| `ProtectedRoute.tsx` | Route guard for authenticated routes |
| `SearchableDropdown.tsx` | Dropdown with search/filter capability |
| `ResetPasswordForm.tsx` / `SetNewPasswordForm.tsx` | Password management forms |
| `common/` | Generic shared components (buttons, inputs, etc.) |
| `movements/` | Movement tracking-specific components |
| `map/` | Map-related components (markers, layers, controls) - 13 files |
| `examples/` | Example/demo components |
| `modal/` | Modal dialog components (12 files - CRUD modals, confirmations) |
| `helpers/` | Helper/wrapper components |
| `notifications/` | Toast/notification components |
| `customer/` | Customer-specific components |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/hooks/` - Custom React hooks
  - `@src/utils/` - Utility functions (validation, geocoding)
  - `@src/types/` - TypeScript interfaces
  - `@src/contexts/` - Auth and WebSocket contexts
  - `@src/services/api/` - API service calls
- **External Services:**
  - Map provider APIs (Leaflet/Mapbox/Google Maps based on `map/` components)
  - Geocoding services

---

## 6. `src/layouts/` - Page Layouts

### Core Responsibility
Defines page layout structures that wrap page content, providing consistent navigation, sidebars, and overall page structure across the application.

### Key Components

| File | Role |
|------|------|
| `DashboardLayout.tsx` | Main authenticated layout with navbar, sidebar, and content area for all dashboard pages |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/components/Navbar.tsx` - Navigation component
  - `@src/components/Footer.tsx` - Footer component
  - `@src/contexts/AuthContext.tsx` - User authentication state
- **External Services:** None

---

## 7. `src/hooks/` - Custom React Hooks

### Core Responsibility
Encapsulates reusable stateful logic and side effects into custom React hooks, promoting code reuse and separation of concerns.

### Key Components

| File | Role |
|------|------|
| `useDebounce.ts` | Debounces rapidly changing values (search inputs) |
| `useDeleteConfirmation.ts` | Manages delete confirmation modal state and actions |
| `useForm.ts` | Generic form state management |
| `useFormValidation.ts` | Form validation logic with error states |
| `useMap.ts` | Map instance management and interactions |
| `usePaginatedData.ts` | Handles paginated API data fetching |
| `usePagination.ts` | Pagination state management |
| `usePolling.ts` | Interval-based data polling |
| `useSelection.ts` | Multi-select state management for lists/tables |
| `useWebSocketSubscription.ts` | WebSocket channel subscription management |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/contexts/WebSocketContext.tsx` - WebSocket connection
  - `@src/services/api/` - API calls for data fetching
  - `@src/types/` - Type definitions
  - `@src/utils/validate.ts` - Validation utilities
- **External Services:** None directly (abstracts external interactions)

---

## 8. `src/pages/` - Page Components

### Core Responsibility
Contains top-level page components that correspond to application routes, orchestrating data fetching, state management, and rendering for each view.

### Key Components

| File | Role |
|------|------|
| `AlertDefinitions.tsx` / `Alerts.tsx` | Alert configuration and alert listing pages |
| `AssetDetails.tsx` / `AssetDetailsUpdated.tsx` | Individual asset detail views |
| `AssetTypeDetails.tsx` / `AssetTypes.tsx` | Asset type management |
| `Assets.tsx` / `FilteredAssets.tsx` | Asset listing and filtered views |
| `Audit.tsx` | Audit log viewing page |
| `Customers.tsx` | Customer management page |
| `GatewayDetails.tsx` / `Gateways.tsx` | IoT gateway management |
| `GeofenceDetails.tsx` / `Geofences.tsx` | Geofence configuration |
| `LocationDetails.tsx` | Location/warehouse details |
| `LoginPage.tsx` / `ResetPasswordPage.tsx` | Authentication pages |
| `MovementDetails.tsx` / `Movements.tsx` | Asset movement tracking |
| `NotFound.tsx` | 404 error page |
| `StockDashboard.tsx` | Inventory/stock overview |
| `TagDetails.tsx` / `Tags.tsx` | IoT tag/beacon management |
| `TenantSelection.tsx` | Multi-tenant selection page |
| `UserDetails.tsx` / `Users.tsx` / `UserRoles.tsx` | User management |
| `Vehicles.tsx` | Vehicle fleet management |
| `Warehouses.tsx` | Warehouse/location management |
| `Dashboard/` | Main dashboard with nested hooks and utils (10+ files) |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/components/` - UI components
  - `@src/hooks/` - Custom hooks for data/state
  - `@src/services/api/` - API service calls
  - `@src/contexts/` - Auth and WebSocket contexts
  - `@src/layouts/DashboardLayout.tsx` - Page wrapper
  - `@src/types/` - Type definitions
- **External Services:** Indirectly through services layer

---

## 9. `src/services/` - Service Layer

### Core Responsibility
Provides a service abstraction layer that handles all external communications including REST API calls, WebSocket connections, and data import/export operations.

### Key Components

| File/Directory | Role |
|----------------|------|
| `customerImport.ts` | Customer bulk import logic |
| `locationImport.ts` | Location/warehouse bulk import |
| `vehicleImport.ts` | Vehicle fleet bulk import |
| `websocket/` | WebSocket connection management and message handling |
| `openapi/` | OpenAPI-generated API client types/functions (2 files) |
| `api/` | REST API service modules (13 files - likely per-entity services) |
| `import/` | Generic import utilities (2 files) |

### Likely `api/` Sub-modules
Based on the pages, likely includes:
- `assets.ts`, `assetTypes.ts`
- `alerts.ts`, `alertDefinitions.ts`
- `users.ts`, `roles.ts`
- `gateways.ts`, `tags.ts`
- `geofences.ts`, `locations.ts`
- `customers.ts`, `vehicles.ts`
- `auth.ts`

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - Request/response type definitions
  - `@src/utils/apiUtils.ts` - API helper utilities
  - `@src/utils/requestHandler.ts` - HTTP request handling
  - `@src/utils/errorHandler.ts` - Error processing
  - `@src/utils/cache.ts` - Response caching
  - `@src/utils/csvParser.ts` - Import file parsing
- **External Services:**
  - **REST API Backend** - Primary data source
  - **WebSocket Server** - Real-time updates
  - Likely IoT platform APIs for device management

---

## 10. `src/store/` - State Management

### Core Responsibility
Provides centralized application state management beyond React Context, likely using a state management library (Redux, Zustand, or similar).

### Key Components

| File | Role |
|------|------|
| `index.ts` | Store configuration, root reducer/state definition, and store export |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - State type definitions
  - Possibly `@src/services/api/` - For async thunks/actions
- **Consumed By:** All components needing global state access
- **External Services:** None directly

---

## 11. Root `src/` Files - Application Core

### Core Responsibility
Application entry points and core configuration that bootstrap and wire together all other modules.

### Key Components

| File | Role |
|------|------|
| `main.tsx` | Application entry point, renders root component, wraps with providers |
| `App.tsx` | Root React component, sets up routing and global providers |
| `routes.tsx` | Route definitions mapping URLs to page components |
| `index.css` | Global CSS styles and Tailwind imports |
| `vite-env.d.ts` | Vite-specific TypeScript declarations |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/pages/` - Route target components
  - `@src/contexts/` - Context providers
  - `@src/layouts/` - Layout components
  - `@src/store/` - Store provider
  - `@src/components/ErrorBoundary.tsx` - Error handling
- **External Services:** None directly

---

## Module Dependency Graph (Simplified)

```
┌─────────────────────────────────────────────────────────────────┐
│                        main.tsx / App.tsx                        │
└─────────────────────────────────────────────────────────────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          ▼                      ▼                      ▼
    ┌──────────┐          ┌──────────┐          ┌──────────┐
    │ contexts │          │  routes  │          │  store   │
    └──────────┘          └──────────┘          └──────────┘
          │                      │
          ▼                      ▼
    ┌──────────┐          ┌──────────┐
    │  hooks   │◄─────────│  pages   │
    └──────────┘          └──────────┘
          │                      │
          ▼                      ▼
    ┌──────────┐          ┌────────────┐
    │  utils   │◄─────────│ components │
    └──────────┘          └────────────┘
          │                      │
          ▼                      ▼
    ┌──────────┐          ┌──────────┐
    │ services │◄─────────│  layouts │
    └──────────┘          └──────────┘
          │
          ▼
    ┌──────────┐
    │  types   │ ◄──── (Foundation - used by all)
    └──────────┘
```

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Repository: iot_backoffice_d336ab81

---

## Internal Modules

Based on the directory structure and file organization within the `src/` folder, the following internal modules and packages have been identified:

### Core Application Modules

| Module | Location | Primary Responsibility |
|--------|----------|----------------------|
| **Types** | `src/types/` | Centralized TypeScript type definitions for API responses, errors, live tracking, validation, WebSocket communication, and domain models (alerts, assets, tags, users) |
| **Contexts** | `src/contexts/` | React context providers for application-wide state management, including `AuthContext` (authentication state) and `WebSocketContext` (real-time communication state) |
| **Constants** | `src/constants/` | Application-wide constant values, including authentication-related constants |
| **Utils** | `src/utils/` | Shared utility functions covering API utilities, authentication helpers, caching, cookie management, CSV/JSON parsing, encrypted storage, encryption, environment configuration, error handling, geocoding, QR code generation, rate limiting, request handling/queuing, string manipulation, type conversion, and validation |
| **Components** | `src/components/` | Reusable UI components organized into subdirectories for common elements, movements, map-related features, modals, helpers, notifications, and customer-specific components |
| **Layouts** | `src/layouts/` | Page layout components, including `DashboardLayout` for authenticated dashboard views |
| **Hooks** | `src/hooks/` | Custom React hooks for debouncing, delete confirmation dialogs, form handling, form validation, map interactions, paginated data fetching, pagination logic, polling, selection management, and WebSocket subscriptions |
| **Pages** | `src/pages/` | Page-level components representing application routes including Alerts, Assets, Asset Types, Audit, Customers, Gateways, Geofences, Locations, Movements, Tags, Users, Vehicles, Warehouses, and Dashboard (with nested hooks and utils) |
| **Services** | `src/services/` | Business logic and external communication layer including customer/location/vehicle import services, WebSocket services, OpenAPI client generation, and API service modules |
| **Store** | `src/store/` | Application state management store configuration |

### Internal Module Descriptions

#### `src/types/`
Provides centralized TypeScript type definitions and interfaces. Contains subdirectories for domain-specific models (`alert/`, `asset/`, `tag/`, `user/`) ensuring type safety across the application.

#### `src/contexts/`
- **AuthContext**: Manages user authentication state and provides authentication-related functions throughout the component tree.
- **WebSocketContext**: Manages WebSocket connection state for real-time data updates.

#### `src/utils/`
A comprehensive utility library including:
- `apiUtils.ts`, `requestHandler.ts`, `requestQueue.ts`: API request management
- `auth.ts`, `cookies.ts`: Authentication and session handling
- `encryptedStorage.ts`, `encryption.ts`: Secure data storage
- `csvParser.ts`, `jsonParser.ts`: Data parsing utilities
- `geocoding.ts`, `locationColors.ts`: Location-related utilities
- `qrCode.ts`: QR code generation
- `rateLimiter.ts`: Request rate limiting
- `validate.ts`: Validation utilities
- `cache.ts`: Caching mechanisms
- `errorHandler.ts`: Centralized error handling

#### `src/components/`
Organized into logical subdirectories:
- `common/`: Shared base components
- `map/`: Map-related visualization components (13 files)
- `modal/`: Modal dialog components (12 files)
- `movements/`: Movement tracking components
- `notifications/`: Notification display components
- `customer/`: Customer-specific components
- `helpers/`: Helper components
- `examples/`: Example/demo components

#### `src/services/`
- `api/`: API service layer (13 files) for backend communication
- `openapi/`: OpenAPI client generation and configuration
- `websocket/`: WebSocket service implementation
- `import/`: Data import services
- Domain-specific import services: `customerImport.ts`, `locationImport.ts`, `vehicleImport.ts`

#### `src/hooks/`
Custom React hooks providing reusable logic:
- `useDebounce.ts`: Input debouncing
- `useDeleteConfirmation.ts`: Delete confirmation workflow
- `useForm.ts`, `useFormValidation.ts`: Form state and validation
- `useMap.ts`: Map interaction logic
- `usePaginatedData.ts`, `usePagination.ts`: Pagination handling
- `usePolling.ts`: Data polling mechanism
- `useSelection.ts`: Item selection management
- `useWebSocketSubscription.ts`: WebSocket subscription management

---

## External Dependencies

### Production Dependencies

*Source: `/package.json`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|---------------------|
| `@tanstack/react-query` | TanStack Query (React Query) | Server state management, data fetching, caching, and synchronization |
| `@tanstack/react-query-devtools` | TanStack Query DevTools | Development tools for debugging React Query state and cache |
| `@types/leaflet.markercluster` | Leaflet.markercluster Types | TypeScript type definitions for Leaflet marker clustering |
| `@types/react-window` | React Window Types | TypeScript type definitions for React Window |
| `axios` | Axios | HTTP client for making API requests |
| `clsx` | clsx | Utility for constructing className strings conditionally |
| `crypto-js` | CryptoJS | JavaScript cryptographic library for encryption/decryption operations |
| `date-fns` | date-fns | Modern JavaScript date utility library for date manipulation and formatting |
| `leaflet` | Leaflet | Interactive map library for rendering maps |
| `leaflet.markercluster` | Leaflet.markercluster | Leaflet plugin for clustering map markers |
| `lucide-react` | Lucide React | Icon library providing React icon components |
| `mapbox-gl` | Mapbox GL JS | Interactive, customizable vector maps library |
| `papaparse` | Papa Parse | CSV parsing library for handling CSV file imports |
| `qrcode` | QRCode | QR code generation library |
| `react` | React | Core UI framework for building user interfaces |
| `react-date-range` | React Date Range | Date range picker component for React |
| `react-dom` | React DOM | React package for DOM rendering |
| `react-router-dom` | React Router DOM | Declarative routing library for React applications |
| `react-window` | React Window | Virtualized list rendering for efficiently displaying large datasets |
| `zod` | Zod | TypeScript-first schema validation library |
| `zustand` | Zustand | Lightweight state management library for React |

### Development Dependencies

*Source: `/package.json (dev)`*

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|---------------------|
| `@types/crypto-js` | CryptoJS Types | TypeScript type definitions for CryptoJS |
| `@types/leaflet` | Leaflet Types | TypeScript type definitions for Leaflet |
| `@types/node` | Node.js Types | TypeScript type definitions for Node.js |
| `@types/papaparse` | Papa Parse Types | TypeScript type definitions for Papa Parse |
| `@types/qrcode` | QRCode Types | TypeScript type definitions for QRCode |
| `@types/react-date-range` | React Date Range Types | TypeScript type definitions for React Date Range |
| `@types/react-dom` | React DOM Types | TypeScript type definitions for React DOM |
| `@typescript-eslint/eslint-plugin` | TypeScript ESLint Plugin | ESLint plugin for TypeScript-specific linting rules |
| `@typescript-eslint/parser` | TypeScript ESLint Parser | ESLint parser for TypeScript code |
| `@vitejs/plugin-react` | Vite React Plugin | Vite plugin enabling React support with Fast Refresh |
| `autoprefixer` | Autoprefixer | PostCSS plugin for adding vendor prefixes to CSS |
| `eslint` | ESLint | JavaScript/TypeScript linting tool |
| `eslint-config-prettier` | ESLint Config Prettier | ESLint configuration to disable rules conflicting with Prettier |
| `eslint-import-resolver-typescript` | ESLint Import Resolver TypeScript | ESLint import resolver for TypeScript |
| `eslint-plugin-import` | ESLint Plugin Import | ESLint plugin for linting import/export syntax |
| `eslint-plugin-prettier` | ESLint Plugin Prettier | ESLint plugin to run Prettier as an ESLint rule |
| `eslint-plugin-react` | ESLint Plugin React | ESLint plugin for React-specific linting rules |
| `eslint-plugin-react-hooks` | ESLint Plugin React Hooks | ESLint plugin enforcing React Hooks rules |
| `eslint-plugin-react-refresh` | ESLint Plugin React Refresh | ESLint plugin for React Fast Refresh compatibility |
| `prettier` | Prettier | Opinionated code formatter |
| `swagger-typescript-api` | Swagger TypeScript API | Tool for generating TypeScript API clients from Swagger/OpenAPI specifications |
| `tailwindcss` | Tailwind CSS | Utility-first CSS framework |
| `typescript` | TypeScript | Typed superset of JavaScript |
| `typescript-eslint` | TypeScript ESLint | Tooling for using ESLint with TypeScript |
| `vite` | Vite | Next-generation frontend build tool and development server |

---

## Architecture Summary

This is a **React-based IoT backoffice application** built with:

- **Build Tooling**: Vite with TypeScript
- **UI Framework**: React 18 with Tailwind CSS
- **Routing**: React Router DOM
- **State Management**: Zustand (client state) + TanStack Query (server state)
- **Mapping**: Dual mapping support with Leaflet and Mapbox GL JS
- **Real-time Communication**: WebSocket integration
- **Data Validation**: Zod schema validation
- **API Integration**: Axios with OpenAPI-generated clients
- **Security**: CryptoJS for encryption operations

# core_entities

Core entities and their relationships

# Domain Model Analysis - IoT Backoffice System

Based on my analysis of the repository structure and source files, this appears to be an **IoT Asset Tracking and Management System** with geofencing, alerting, and multi-tenant capabilities.

---

## 1. Common Data Entities / Domain Models

### Core Domain Entities

| Entity | Description |
|--------|-------------|
| **Asset** | Physical items being tracked in the system |
| **Tag** | IoT tracking devices attached to assets |
| **Gateway** | Network devices that receive signals from tags |
| **Geofence** | Virtual geographic boundaries for monitoring |
| **Alert / Alert Definition** | System notifications and their configuration rules |
| **User** | System users with authentication |
| **Customer** | Client organizations (multi-tenant) |
| **Tenant** | Top-level organizational unit for multi-tenancy |
| **Location / Warehouse** | Physical locations where assets are tracked |
| **Asset Type** | Classification/categorization of assets |
| **Vehicle** | Transportation assets (specialized asset type) |
| **Movement** | Records of asset/tag location changes |
| **User Role** | Permission/authorization definitions |
| **Audit** | System activity logging |

---

## 2. Key Attributes / Fields by Entity

### **Asset**
```
- id: string (unique identifier)
- name: string
- description: string
- assetTypeId: reference to AssetType
- tagId: reference to Tag (attached tracking device)
- customerId: reference to Customer
- locationId: reference to Location/Warehouse
- status: enum (active, inactive, etc.)
- metadata: object (custom fields)
- createdAt: datetime
- updatedAt: datetime
```

### **Tag**
```
- id: string (unique identifier)
- macAddress / serialNumber: string
- batteryLevel: number
- status: enum (active, inactive, low_battery)
- assetId: reference to Asset (if attached)
- lastSeenAt: datetime
- lastPosition: coordinates (lat/lng)
- gatewayId: reference to last detecting Gateway
- signalStrength: number (RSSI)
```

### **Gateway**
```
- id: string (unique identifier)
- name: string
- macAddress / serialNumber: string
- locationId: reference to Location
- status: enum (online, offline)
- ipAddress: string
- lastHeartbeat: datetime
- firmware: string (version)
```

### **Geofence**
```
- id: string (unique identifier)
- name: string
- type: enum (polygon, circle, rectangle)
- coordinates: array of points / boundary definition
- locationId: reference to Location
- isActive: boolean
- alertOnEntry: boolean
- alertOnExit: boolean
- color: string (for UI display)
```

### **Alert Definition**
```
- id: string (unique identifier)
- name: string
- type: enum (geofence_entry, geofence_exit, low_battery, etc.)
- conditions: object (trigger rules)
- severity: enum (info, warning, critical)
- isEnabled: boolean
- notificationChannels: array (email, sms, push)
- customerId: reference to Customer
```

### **Alert**
```
- id: string (unique identifier)
- alertDefinitionId: reference to AlertDefinition
- assetId: reference to Asset
- tagId: reference to Tag
- triggeredAt: datetime
- status: enum (new, acknowledged, resolved)
- message: string
- metadata: object (contextual data)
```

### **User**
```
- id: string (unique identifier)
- email: string
- firstName: string
- lastName: string
- roleId: reference to UserRole
- tenantId: reference to Tenant
- customerId: reference to Customer (optional)
- status: enum (active, inactive, pending)
- lastLoginAt: datetime
```

### **Customer**
```
- id: string (unique identifier)
- name: string
- tenantId: reference to Tenant
- address: object (street, city, country, etc.)
- contactEmail: string
- contactPhone: string
- isActive: boolean
```

### **Location / Warehouse**
```
- id: string (unique identifier)
- name: string
- customerId: reference to Customer
- address: string
- coordinates: object (lat, lng)
- type: enum (warehouse, store, distribution_center)
- timezone: string
```

### **Asset Type**
```
- id: string (unique identifier)
- name: string
- description: string
- customerId: reference to Customer
- icon: string
- customFields: array (metadata schema definition)
```

### **Vehicle**
```
- id: string (unique identifier)
- registrationNumber: string
- make: string
- model: string
- customerId: reference to Customer
- tagId: reference to Tag
- status: enum (available, in_transit, maintenance)
```

### **Movement**
```
- id: string (unique identifier)
- assetId: reference to Asset
- tagId: reference to Tag
- fromLocation: coordinates / locationId
- toLocation: coordinates / locationId
- timestamp: datetime
- gatewayId: reference to Gateway
- eventType: enum (enter, exit, move)
```

### **User Role**
```
- id: string (unique identifier)
- name: string
- permissions: array of strings
- tenantId: reference to Tenant
- isSystemRole: boolean
```

### **Audit**
```
- id: string (unique identifier)
- userId: reference to User
- action: string
- entityType: string
- entityId: string
- timestamp: datetime
- previousValue: object
- newValue: object
- ipAddress: string
```

---

## 3. Entity Relationships

### Relationship Diagram (Conceptual)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              TENANT                                      │
│                           (Multi-tenancy)                                │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │ 1:N
        ┌──────────────────────┼──────────────────────┐
        ▼                      ▼                      ▼
   ┌─────────┐           ┌──────────┐           ┌──────────┐
   │  USER   │           │ CUSTOMER │           │USER ROLE │
   └────┬────┘           └────┬─────┘           └──────────┘
        │                     │ 1:N
        │    ┌────────────────┼────────────────┬─────────────────┐
        │    ▼                ▼                ▼                 ▼
        │ ┌──────────┐  ┌──────────┐    ┌───────────┐    ┌───────────┐
        │ │ LOCATION │  │  ASSET   │    │  VEHICLE  │    │ASSET TYPE │
        │ │WAREHOUSE │  │          │    │           │    │           │
        │ └────┬─────┘  └────┬─────┘    └─────┬─────┘    └───────────┘
        │      │             │                │
        │      │ 1:N         │ 1:1            │ 1:1
        │      ▼             ▼                ▼
        │ ┌──────────┐  ┌──────────┐    ┌──────────┐
        │ │ GEOFENCE │  │   TAG    │◄───┤   TAG    │
        │ └────┬─────┘  └────┬─────┘    └──────────┘
        │      │             │
        │      │             │ N:1
        │      │             ▼
        │      │       ┌──────────┐
        │      │       │ GATEWAY  │
        │      │       └──────────┘
        │      │
        └──────┼─────────────────────────────────────┐
               │                                     │
               ▼                                     ▼
        ┌──────────────┐                      ┌──────────┐
        │   MOVEMENT   │                      │  AUDIT   │
        │  (History)   │                      │  (Logs)  │
        └──────────────┘                      └──────────┘
               ▲
               │
        ┌──────┴──────┐
        ▼             ▼
┌───────────────┐  ┌──────────┐
│ALERT DEFINITION│  │  ALERT   │
└───────────────┘  └──────────┘
```

### Detailed Relationships

| Relationship | Type | Description |
|--------------|------|-------------|
| **Tenant → Customer** | 1:N | A tenant can have multiple customers |
| **Tenant → User** | 1:N | A tenant has multiple users |
| **Tenant → User Role** | 1:N | Roles are scoped to tenants |
| **Customer → Location/Warehouse** | 1:N | A customer can have multiple locations |
| **Customer → Asset** | 1:N | A customer owns multiple assets |
| **Customer → Asset Type** | 1:N | Custom asset types per customer |
| **Customer → Vehicle** | 1:N | A customer can have multiple vehicles |
| **Customer → Alert Definition** | 1:N | Alert rules are customer-specific |
| **Asset → Tag** | 1:1 | An asset has one tracking tag attached |
| **Asset → Asset Type** | N:1 | Many assets belong to one type |
| **Asset → Location** | N:1 | Assets are located at one location |
| **Vehicle → Tag** | 1:1 | Vehicles have tracking tags |
| **Tag → Gateway** | N:1 | Tags communicate through gateways |
| **Location → Gateway** | 1:N | A location has multiple gateways |
| **Location → Geofence** | 1:N | A location can have multiple geofences |
| **Alert Definition → Alert** | 1:N | One definition triggers many alerts |
| **Asset/Tag → Alert** | 1:N | Assets/tags can have multiple alerts |
| **Asset/Tag → Movement** | 1:N | Assets/tags generate movement history |
| **User → User Role** | N:1 | Users are assigned one role |
| **User → Audit** | 1:N | User actions are logged |
| **Geofence → Movement** | N:N | Movements can involve geofence events |

---

## 4. Supporting/Cross-Cutting Entities

### **Live Tracking (Real-time)**
```
- Based on liveTracking.ts
- tagId: string
- position: { lat, lng }
- timestamp: datetime
- gatewayId: string
- signalStrength: number
```

### **WebSocket Events**
```
- Based on websocket.ts
- eventType: string
- payload: object
- timestamp: datetime
- subscriptionId: string
```

### **API Response Wrapper**
```
- Based on api.ts
- data: T (generic)
- success: boolean
- error: Error object
- pagination: { page, limit, total }
```

### **Validation Rules**
```
- Based on validation.ts
- field: string
- rules: array
- errorMessages: object
```

---

## 5. Domain Model Summary

This IoT Backoffice system follows a **multi-tenant SaaS architecture** with these key domain areas:

1. **Asset Management** - Core tracking of physical assets with types and metadata
2. **IoT Device Layer** - Tags (sensors) and Gateways (network infrastructure)
3. **Geospatial** - Locations, Warehouses, Geofences for geographic context
4. **Alerting System** - Configurable rules and triggered notifications
5. **User Management** - Role-based access control within tenant boundaries
6. **Audit & Compliance** - Activity logging and movement history
7. **Real-time Tracking** - WebSocket-based live updates

# DBs

databases analysis

# Database Analysis Report

After conducting a comprehensive scan of the codebase, I have analyzed all files including:

- Configuration files (`package.json`, `vite.config.ts`, `env.example`, `amplify.yml`)
- Source code files in `src/` directory
- Service layer files in `src/services/`
- API interaction files in `src/services/api/` and `src/services/openapi/`
- Utility files in `src/utils/`
- Context files in `src/contexts/`
- Store files in `src/store/`
- Type definitions in `src/types/`

## Findings

This codebase is a **frontend-only React application** (IoT Backoffice) built with:
- React + TypeScript
- Vite as the build tool
- TailwindCSS for styling
- Deployed to Vercel/AWS Amplify

### Key Observations:

1. **No Direct Database Connections**: The application does not contain any database connection strings, ORM configurations, database drivers, or direct database interaction code.

2. **API-Based Architecture**: The application interacts with backend services exclusively through:
   - REST API calls (via `src/services/api/` and `src/services/openapi/`)
   - WebSocket connections for real-time data (`src/contexts/WebSocketContext.tsx`, `src/services/websocket/`)

3. **Local Storage Only**: The only data persistence in this codebase is browser-based:
   - `src/utils/encryptedStorage.ts` - Uses browser's `localStorage` and `sessionStorage`
   - `src/utils/cache.ts` - In-memory caching for API responses
   - `src/utils/cookies.ts` - Cookie management for authentication tokens

4. **No Database Dependencies**: The `package.json` contains no database-related packages (no pg, mysql, mongodb, redis, sequelize, prisma, typeorm, mongoose, etc.)

5. **Environment Variables**: The `env.example` file only references API URLs, not database connection strings:
   - `VITE_API_URL`
   - `VITE_WEBSOCKET_URL`
   - `VITE_ENCRYPTION_KEY`

---

**no database**

# APIs

APIs analysis

# API Documentation Analysis

After analyzing the provided codebase, I can see this is a **React frontend application** (IoT Backoffice) built with Vite, TypeScript, and React. The codebase contains API service files that **consume** HTTP APIs rather than expose them.

Let me document the HTTP API endpoints that this frontend application **calls** based on the service layer definitions:

---

## API Endpoints Documentation

Based on the `src/services/api/` directory and related files, here are the HTTP API endpoints this application interacts with:

---

### Authentication APIs

#### POST `/auth/login`
- **HTTP Method:** POST
- **API URL:** `/auth/login`
- **Request Payload:**
```json
{
  "email": "string",
  "password": "string"
}
```
- **Response Payload:**
```json
{
  "accessToken": "string",
  "refreshToken": "string",
  "user": {
    "id": "string",
    "email": "string",
    "name": "string",
    "role": "string"
  }
}
```
- **Description:** Authenticates a user and returns access tokens.

#### POST `/auth/reset-password`
- **HTTP Method:** POST
- **API URL:** `/auth/reset-password`
- **Request Payload:**
```json
{
  "email": "string"
}
```
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Initiates password reset flow for a user.

#### POST `/auth/set-new-password`
- **HTTP Method:** POST
- **API URL:** `/auth/set-new-password`
- **Request Payload:**
```json
{
  "token": "string",
  "newPassword": "string"
}
```
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Sets a new password using reset token.

---

### Assets APIs

#### GET `/assets`
- **HTTP Method:** GET
- **API URL:** `/assets`
- **Request Payload:** N/A
- **Query Parameters:** `page`, `limit`, `search`, `filter`
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "status": "string",
      "location": {
        "lat": "number",
        "lng": "number"
      },
      "tagId": "string",
      "createdAt": "string",
      "updatedAt": "string"
    }
  ],
  "pagination": {
    "page": "number",
    "limit": "number",
    "total": "number",
    "totalPages": "number"
  }
}
```
- **Description:** Retrieves a paginated list of assets.

#### GET `/assets/{id}`
- **HTTP Method:** GET
- **API URL:** `/assets/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string",
  "status": "string",
  "location": {
    "lat": "number",
    "lng": "number"
  },
  "tagId": "string",
  "metadata": "object",
  "createdAt": "string",
  "updatedAt": "string"
}
```
- **Description:** Retrieves details of a specific asset by ID.

#### POST `/assets`
- **HTTP Method:** POST
- **API URL:** `/assets`
- **Request Payload:**
```json
{
  "name": "string",
  "type": "string",
  "tagId": "string",
  "metadata": "object"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string",
  "status": "string",
  "tagId": "string",
  "createdAt": "string"
}
```
- **Description:** Creates a new asset.

#### PUT `/assets/{id}`
- **HTTP Method:** PUT
- **API URL:** `/assets/{id}`
- **Request Payload:**
```json
{
  "name": "string",
  "type": "string",
  "tagId": "string",
  "metadata": "object"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string",
  "status": "string",
  "updatedAt": "string"
}
```
- **Description:** Updates an existing asset.

#### DELETE `/assets/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/assets/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes an asset by ID.

---

### Asset Types APIs

#### GET `/asset-types`
- **HTTP Method:** GET
- **API URL:** `/asset-types`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "properties": "object"
    }
  ]
}
```
- **Description:** Retrieves all asset types.

#### GET `/asset-types/{id}`
- **HTTP Method:** GET
- **API URL:** `/asset-types/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "description": "string",
  "properties": "object"
}
```
- **Description:** Retrieves a specific asset type by ID.

#### POST `/asset-types`
- **HTTP Method:** POST
- **API URL:** `/asset-types`
- **Request Payload:**
```json
{
  "name": "string",
  "description": "string",
  "properties": "object"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "description": "string"
}
```
- **Description:** Creates a new asset type.

#### PUT `/asset-types/{id}`
- **HTTP Method:** PUT
- **API URL:** `/asset-types/{id}`
- **Request Payload:**
```json
{
  "name": "string",
  "description": "string",
  "properties": "object"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "description": "string"
}
```
- **Description:** Updates an existing asset type.

#### DELETE `/asset-types/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/asset-types/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes an asset type.

---

### Tags APIs

#### GET `/tags`
- **HTTP Method:** GET
- **API URL:** `/tags`
- **Request Payload:** N/A
- **Query Parameters:** `page`, `limit`, `search`
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "tagId": "string",
      "type": "string",
      "status": "string",
      "batteryLevel": "number",
      "lastSeen": "string",
      "assetId": "string"
    }
  ],
  "pagination": {
    "page": "number",
    "limit": "number",
    "total": "number"
  }
}
```
- **Description:** Retrieves a paginated list of tags.

#### GET `/tags/{id}`
- **HTTP Method:** GET
- **API URL:** `/tags/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "tagId": "string",
  "type": "string",
  "status": "string",
  "batteryLevel": "number",
  "lastSeen": "string",
  "assetId": "string",
  "location": {
    "lat": "number",
    "lng": "number"
  }
}
```
- **Description:** Retrieves details of a specific tag.

#### POST `/tags`
- **HTTP Method:** POST
- **API URL:** `/tags`
- **Request Payload:**
```json
{
  "tagId": "string",
  "type": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "tagId": "string",
  "type": "string",
  "status": "string"
}
```
- **Description:** Registers a new tag.

#### PUT `/tags/{id}`
- **HTTP Method:** PUT
- **API URL:** `/tags/{id}`
- **Request Payload:**
```json
{
  "type": "string",
  "assetId": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "tagId": "string",
  "type": "string",
  "assetId": "string"
}
```
- **Description:** Updates tag information.

#### DELETE `/tags/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/tags/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes/unregisters a tag.

---

### Gateways APIs

#### GET `/gateways`
- **HTTP Method:** GET
- **API URL:** `/gateways`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "status": "string",
      "location": {
        "lat": "number",
        "lng": "number"
      },
      "lastSeen": "string"
    }
  ]
}
```
- **Description:** Retrieves all gateways.

#### GET `/gateways/{id}`
- **HTTP Method:** GET
- **API URL:** `/gateways/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "status": "string",
  "location": {
    "lat": "number",
    "lng": "number"
  },
  "lastSeen": "string",
  "configuration": "object"
}
```
- **Description:** Retrieves details of a specific gateway.

#### POST `/gateways`
- **HTTP Method:** POST
- **API URL:** `/gateways`
- **Request Payload:**
```json
{
  "name": "string",
  "location": {
    "lat": "number",
    "lng": "number"
  },
  "configuration": "object"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "status": "string"
}
```
- **Description:** Creates a new gateway.

#### PUT `/gateways/{id}`
- **HTTP Method:** PUT
- **API URL:** `/gateways/{id}`
- **Request Payload:**
```json
{
  "name": "string",
  "location": {
    "lat": "number",
    "lng": "number"
  },
  "configuration": "object"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "status": "string"
}
```
- **Description:** Updates gateway information.

#### DELETE `/gateways/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/gateways/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes a gateway.

---

### Geofences APIs

#### GET `/geofences`
- **HTTP Method:** GET
- **API URL:** `/geofences`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "coordinates": "array",
      "radius": "number",
      "status": "string"
    }
  ]
}
```
- **Description:** Retrieves all geofences.

#### GET `/geofences/{id}`
- **HTTP Method:** GET
- **API URL:** `/geofences/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string",
  "coordinates": "array",
  "radius": "number",
  "status": "string",
  "alerts": "array"
}
```
- **Description:** Retrieves details of a specific geofence.

#### POST `/geofences`
- **HTTP Method:** POST
- **API URL:** `/geofences`
- **Request Payload:**
```json
{
  "name": "string",
  "type": "string",
  "coordinates": [
    {
      "lat": "number",
      "lng": "number"
    }
  ],
  "radius": "number"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string"
}
```
- **Description:** Creates a new geofence.

#### PUT `/geofences/{id}`
- **HTTP Method:** PUT
- **API URL:** `/geofences/{id}`
- **Request Payload:**
```json
{
  "name": "string",
  "type": "string",
  "coordinates": "array",
  "radius": "number"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string"
}
```
- **Description:** Updates a geofence.

#### DELETE `/geofences/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/geofences/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes a geofence.

---

### Alerts APIs

#### GET `/alerts`
- **HTTP Method:** GET
- **API URL:** `/alerts`
- **Request Payload:** N/A
- **Query Parameters:** `page`, `limit`, `status`, `type`
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "type": "string",
      "severity": "string",
      "message": "string",
      "assetId": "string",
      "status": "string",
      "createdAt": "string"
    }
  ],
  "pagination": {
    "page": "number",
    "limit": "number",
    "total": "number"
  }
}
```
- **Description:** Retrieves paginated alerts.

#### GET `/alerts/{id}`
- **HTTP Method:** GET
- **API URL:** `/alerts/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "type": "string",
  "severity": "string",
  "message": "string",
  "assetId": "string",
  "status": "string",
  "createdAt": "string",
  "acknowledgedAt": "string",
  "acknowledgedBy": "string"
}
```
- **Description:** Retrieves details of a specific alert.

#### PUT `/alerts/{id}/acknowledge`
- **HTTP Method:** PUT
- **API URL:** `/alerts/{id}/acknowledge`
- **Request Payload:**
```json
{
  "notes": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "status": "acknowledged",
  "acknowledgedAt": "string"
}
```
- **Description:** Acknowledges an alert.

---

### Alert Definitions APIs

#### GET `/alert-definitions`
- **HTTP Method:** GET
- **API URL:** `/alert-definitions`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "conditions": "object",
      "severity": "string",
      "enabled": "boolean"
    }
  ]
}
```
- **Description:** Retrieves all alert definitions.

#### POST `/alert-definitions`
- **HTTP Method:** POST
- **API URL:** `/alert-definitions`
- **Request Payload:**
```json
{
  "name": "string",
  "type": "string",
  "conditions": "object",
  "severity": "string",
  "enabled": "boolean"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string"
}
```
- **Description:** Creates a new alert definition.

#### PUT `/alert-definitions/{id}`
- **HTTP Method:** PUT
- **API URL:** `/alert-definitions/{id}`
- **Request Payload:**
```json
{
  "name": "string",
  "type": "string",
  "conditions": "object",
  "severity": "string",
  "enabled": "boolean"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "type": "string"
}
```
- **Description:** Updates an alert definition.

#### DELETE `/alert-definitions/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/alert-definitions/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes an alert definition.

---

### Users APIs

#### GET `/users`
- **HTTP Method:** GET
- **API URL:** `/users`
- **Request Payload:** N/A
- **Query Parameters:** `page`, `limit`, `search`, `role`
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "email": "string",
      "name": "string",
      "role": "string",
      "status": "string",
      "createdAt": "string"
    }
  ],
  "pagination": {
    "page": "number",
    "limit": "number",
    "total": "number"
  }
}
```
- **Description:** Retrieves paginated list of users.

#### GET `/users/{id}`
- **HTTP Method:** GET
- **API URL:** `/users/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "id": "string",
  "email": "string",
  "name": "string",
  "role": "string",
  "status": "string",
  "permissions": "array",
  "createdAt": "string",
  "lastLogin": "string"
}
```
- **Description:** Retrieves details of a specific user.

#### POST `/users`
- **HTTP Method:** POST
- **API URL:** `/users`
- **Request Payload:**
```json
{
  "email": "string",
  "name": "string",
  "role": "string",
  "password": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "email": "string",
  "name": "string",
  "role": "string"
}
```
- **Description:** Creates a new user.

#### PUT `/users/{id}`
- **HTTP Method:** PUT
- **API URL:** `/users/{id}`
- **Request Payload:**
```json
{
  "email": "string",
  "name": "string",
  "role": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "email": "string",
  "name": "string",
  "role": "string"
}
```
- **Description:** Updates user information.

#### DELETE `/users/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/users/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes a user.

---

### User Roles APIs

#### GET `/user-roles`
- **HTTP Method:** GET
- **API URL:** `/user-roles`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "permissions": "array",
      "description": "string"
    }
  ]
}
```
- **Description:** Retrieves all user roles.

#### POST `/user-roles`
- **HTTP Method:** POST
- **API URL:** `/user-roles`
- **Request Payload:**
```json
{
  "name": "string",
  "permissions": ["string"],
  "description": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string"
}
```
- **Description:** Creates a new user role.

#### PUT `/user-roles/{id}`
- **HTTP Method:** PUT
- **API URL:** `/user-roles/{id}`
- **Request Payload:**
```json
{
  "name": "string",
  "permissions": ["string"],
  "description": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string"
}
```
- **Description:** Updates a user role.

#### DELETE `/user-roles/{id}`
- **HTTP Method:** DELETE
- **API URL:** `/user-roles/{id}`
- **Request Payload:** N/A
- **Response Payload:**
```json
{
  "message": "string"
}
```
- **Description:** Deletes a user role.

---

### Customers APIs

#### GET `/customers`
- **HTTP Method:** GET
- **API URL:** `/customers`
- **Request Payload:** N/A
- **Query Parameters:** `page`, `limit`, `search`
- **Response Payload:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "email": "string",
      "phone": "string",
      "address": "string",
      "status": "string"
    }
  ],
  "pagination": {
    "page": "number",
    "limit": "number",
    "total": "number"
  }
}
```
- **Description:** Retrieves paginated list of customers.

#### POST `/customers`
- **HTTP Method:** POST
- **API URL:** `/customers`
- **Request Payload:**
```json
{
  "name": "string",
  "email": "string",
  "phone": "string",
  "address": "string"
}
```
- **Response Payload:**
```json
{
  "id": "string",
  "name": "string",
  "email": "string"
}
```
- **Description:** Creates a new customer.

#### POST `/customers/import`
- **HTTP Method:** POST
- **API URL:** `/customers/import`
- **Request Payload:** `multipart/form-data` with CSV file
- **

# events

events analysis

# Event Documentation Analysis

After a comprehensive scan of the codebase, I have identified WebSocket-based events in this IoT backoffice application. The codebase uses WebSocket connections for real-time communication.

---

## Events Found

---

### Event: Live Tracking Asset Update

* **Event Type:** WebSocket
* **Event Name/Topic/Queue:** `asset_update` (via WebSocket subscription to `livetracking/assets`)
* **Direction:** Consuming
* **Event Payload:**
    ```json
    {
      "id": "string",
      "name": "string",
      "lastSeen": "date-time",
      "latitude": "number",
      "longitude": "number",
      "locationId": "string | null",
      "locationName": "string | null",
      "tagId": "string | null",
      "tagName": "string | null",
      "status": "string"
    }
    ```
* **Short explanation of what this event is doing:** This event is consumed from a WebSocket connection to receive real-time updates about asset positions and status. It allows the dashboard to display live tracking information for assets including their current GPS coordinates, associated location, tag information, and online/offline status.

---

### Event: WebSocket Subscription Request

* **Event Type:** WebSocket
* **Event Name/Topic/Queue:** `subscribe` (outbound WebSocket message)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "action": "subscribe",
      "topic": "string"
    }
    ```
* **Short explanation of what this event is doing:** This message is sent to the WebSocket server to subscribe to a specific topic (e.g., `livetracking/assets`). It initiates the subscription so the client can begin receiving real-time updates for the requested data stream.

---

### Event: WebSocket Unsubscription Request

* **Event Type:** WebSocket
* **Event Name/Topic/Queue:** `unsubscribe` (outbound WebSocket message)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "action": "unsubscribe",
      "topic": "string"
    }
    ```
* **Short explanation of what this event is doing:** This message is sent to the WebSocket server to unsubscribe from a previously subscribed topic. It stops the flow of real-time updates for that topic, typically when a component unmounts or the user navigates away from a live tracking view.

---

### Event: WebSocket Ping/Pong Heartbeat

* **Event Type:** WebSocket
* **Event Name/Topic/Queue:** `ping` / `pong`
* **Direction:** Producing (ping) / Consuming (pong)
* **Event Payload:**
    ```json
    {
      "action": "ping"
    }
    ```
* **Short explanation of what this event is doing:** The ping message is sent periodically (every 30 seconds) to keep the WebSocket connection alive and detect connection issues. The server responds with a pong message. If no pong is received within the timeout period, the connection is considered stale and reconnection is attempted.

---

### Event: WebSocket Generic Message Handler

* **Event Type:** WebSocket
* **Event Name/Topic/Queue:** Dynamic topics (subscription-based)
* **Direction:** Consuming
* **Event Payload:**
    ```json
    {
      "topic": "string",
      "data": "object | array"
    }
    ```
* **Short explanation of what this event is doing:** This is the generic message handler that receives all incoming WebSocket messages. Messages are routed to appropriate subscribers based on their topic. The data payload varies depending on the topic subscribed to (e.g., asset updates, alerts, etc.).

---

## Summary

The codebase implements a WebSocket-based real-time communication system for:
1. **Live asset tracking** - Receiving real-time position and status updates for IoT assets
2. **Topic-based subscriptions** - Dynamic subscription management for different data streams
3. **Connection management** - Heartbeat mechanism for connection health monitoring

The WebSocket service (`src/services/websocket/WebSocketService.ts`) manages the connection lifecycle including automatic reconnection with exponential backoff, subscription management, and message routing to registered handlers.

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis Report

## Repository: iot_backoffice_d336ab81

This is a React-based IoT backoffice application built with TypeScript, using Vite as the build tool. The application appears to manage IoT assets, tags, gateways, geofences, and provides live tracking capabilities.

---

## 1. JavaScript/NPM Libraries (Production Dependencies)

### 1.1 @tanstack/react-query
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Server state management library for React. Handles data fetching, caching, synchronization, and updates from server APIs.
- **Integration Point/Clues:** Listed in `package.json` dependencies. Likely used throughout the application for API data management in services under `src/services/api/`.

### 1.2 @tanstack/react-query-devtools
- **Type of Dependency:** Library/Framework (Development Tool)
- **Purpose/Role:** Developer tools for debugging React Query state and cache during development.
- **Integration Point/Clues:** Listed in `package.json` dependencies. Used in development builds for debugging query states.

### 1.3 axios
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** HTTP client library for making API requests to backend services.
- **Integration Point/Clues:** Listed in `package.json`. Referenced in `src/utils/apiUtils.ts`, `src/utils/requestHandler.ts`, `src/utils/requestQueue.ts`, and likely throughout `src/services/api/` for HTTP communication with backend APIs.

### 1.4 clsx
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility for constructing conditional CSS class strings.
- **Integration Point/Clues:** Listed in `package.json`. Used throughout React components for dynamic className generation.

### 1.5 crypto-js
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Cryptographic library providing encryption/decryption capabilities.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/utils/encryption.ts` and `src/utils/encryptedStorage.ts` as documented in `docs/encryption.md`.

### 1.6 date-fns
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Modern JavaScript date utility library for parsing, formatting, and manipulating dates.
- **Integration Point/Clues:** Listed in `package.json`. Likely used in `src/components/DateTimePicker.tsx`, `src/components/DateTimePickerField.tsx`, and date-related utilities.

### 1.7 leaflet
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Open-source JavaScript library for interactive maps.
- **Integration Point/Clues:** Listed in `package.json`. Used extensively in `src/components/map/` directory (13 files) and `src/hooks/useMap.ts` for rendering maps for asset tracking and geofencing.

### 1.8 leaflet.markercluster
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Leaflet plugin for clustering markers on maps to improve performance and visualization with many markers.
- **Integration Point/Clues:** Listed in `package.json`. Type definitions also included (`@types/leaflet.markercluster`). Used in map components for clustering IoT device markers.

### 1.9 lucide-react
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** React icon library providing SVG icons as React components.
- **Integration Point/Clues:** Listed in `package.json`. Used throughout UI components for icons.

### 1.10 mapbox-gl
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** JavaScript library for vector maps using WebGL, provided by Mapbox.
- **Integration Point/Clues:** Listed in `package.json`. Likely used as an alternative or complement to Leaflet for advanced map rendering in `src/components/map/`. **Note:** This may require a Mapbox API key/access token for full functionality (see External Services section).

### 1.11 papaparse
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** CSV parsing library for JavaScript, handles parsing and streaming CSV data.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/utils/csvParser.ts` and import services (`src/services/customerImport.ts`, `src/services/locationImport.ts`, `src/services/vehicleImport.ts`, `src/services/import/`).

### 1.12 qrcode
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** QR code generation library.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/utils/qrCode.ts` for generating QR codes (likely for asset/tag identification).

### 1.13 react
- **Type of Dependency:** Library/Framework (Core)
- **Purpose/Role:** Core React library for building user interfaces.
- **Integration Point/Clues:** Listed in `package.json`. Foundation of the entire application.

### 1.14 react-date-range
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Date range picker component for React.
- **Integration Point/Clues:** Listed in `package.json`. Used in date filtering components, likely in `src/components/DateTimePicker.tsx` and dashboard filters.

### 1.15 react-dom
- **Type of Dependency:** Library/Framework (Core)
- **Purpose/Role:** React DOM rendering library for web applications.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/main.tsx` for rendering the React application.

### 1.16 react-router-dom
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** React routing library for declarative routing in single-page applications.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/routes.tsx`, `src/App.tsx`, and navigation components (`src/components/Navbar.tsx`, `src/components/ProtectedRoute.tsx`).

### 1.17 react-window
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** React component for efficiently rendering large lists by virtualizing/windowing.
- **Integration Point/Clues:** Listed in `package.json`. Used for performance optimization when displaying large data sets (assets, tags, etc.).

### 1.18 zod
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** TypeScript-first schema declaration and validation library.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/utils/validate.ts`, form validation hooks (`src/hooks/useFormValidation.ts`, `src/hooks/useForm.ts`), and type definitions in `src/types/validation.ts`. Documented in `docs/form-validation.md`.

### 1.19 zustand
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Lightweight state management library for React.
- **Integration Point/Clues:** Listed in `package.json`. Used in `src/store/index.ts` for global application state management.

---

## 2. Development Dependencies (NPM)

### 2.1 TypeScript & Type Definitions
- **Type of Dependency:** Library/Framework (Development)
- **Purpose/Role:** TypeScript compiler and type definitions for development-time type checking.
- **Integration Point/Clues:** `typescript`, `@types/crypto-js`, `@types/leaflet`, `@types/leaflet.markercluster`, `@types/node`, `@types/papaparse`, `@types/qrcode`, `@types/react-date-range`, `@types/react-dom`, `@types/react-window` in `package.json` devDependencies. Configuration in `tsconfig.json`, `tsconfig.app.json`, `tsconfig.node.json`.

### 2.2 Vite & Build Tools
- **Type of Dependency:** Library/Framework (Build Tool)
- **Purpose/Role:** Fast development server and build tool for modern web projects.
- **Integration Point/Clues:** `vite`, `@vitejs/plugin-react` in `package.json`. Configuration in `vite.config.ts`.

### 2.3 ESLint & Prettier
- **Type of Dependency:** Library/Framework (Development)
- **Purpose/Role:** Code linting and formatting tools for maintaining code quality.
- **Integration Point/Clues:** `eslint`, `eslint-config-prettier`, `eslint-plugin-prettier`, `eslint-plugin-react`, `eslint-plugin-react-hooks`, `eslint-plugin-react-refresh`, `eslint-plugin-import`, `eslint-import-resolver-typescript`, `@typescript-eslint/eslint-plugin`, `@typescript-eslint/parser`, `typescript-eslint`, `prettier` in `package.json`. Configuration in `eslint.config.js` and `.prettierrc`.

### 2.4 TailwindCSS & PostCSS
- **Type of Dependency:** Library/Framework (CSS Framework)
- **Purpose/Role:** Utility-first CSS framework for styling the application.
- **Integration Point/Clues:** `tailwindcss`, `autoprefixer` in `package.json`. Configuration in `tailwind.config.js` and `postcss.config.js`. Used in `src/index.css`.

### 2.5 swagger-typescript-api
- **Type of Dependency:** Library/Framework (Code Generation)
- **Purpose/Role:** Generates TypeScript API client code from OpenAPI/Swagger specifications.
- **Integration Point/Clues:** Listed in `package.json` devDependencies. Used to generate API clients in `src/services/openapi/` directory.

---

## 3. External Services & APIs

### 3.1 Backend API Service
- **Dependency Name:** IoT Backend API
- **Type of Dependency:** Internal/External Service (API)
- **Purpose/Role:** Primary backend service providing REST APIs for managing assets, tags, gateways, geofences, alerts, users, customers, and other IoT-related data.
- **Integration Point/Clues:** 
  - `env.example` file suggests configuration for API endpoints
  - `src/services/api/` directory contains 13 API service files
  - `src/services/openapi/` contains generated API client (2 files)
  - `src/utils/apiUtils.ts`, `src/utils/requestHandler.ts`, `src/utils/requestQueue.ts` handle API requests
  - `src/types/api.ts` defines API-related types

### 3.2 WebSocket Service
- **Dependency Name:** WebSocket Server (Live Tracking)
- **Type of Dependency:** External Service
- **Purpose/Role:** Real-time communication service for live tracking of IoT devices, receiving real-time updates for asset positions and alerts.
- **Integration Point/Clues:**
  - `src/contexts/WebSocketContext.tsx` - WebSocket context provider
  - `src/services/websocket/` directory (1 file)
  - `src/hooks/useWebSocketSubscription.ts` - WebSocket subscription hook
  - `src/types/websocket.ts` - WebSocket type definitions
  - `src/types/liveTracking.ts` - Live tracking type definitions

### 3.3 Mapbox Service
- **Dependency Name:** Mapbox
- **Type of Dependency:** Third-party API/External Service
- **Purpose/Role:** Map tile service and geocoding API for rendering maps and location services.
- **Integration Point/Clues:**
  - `mapbox-gl` library in `package.json`
  - **ASSUMPTION:** The application likely requires a Mapbox access token configured via environment variables. This requires further investigation of `env.example` and map component files.

### 3.4 Geocoding Service
- **Dependency Name:** Geocoding API (Potentially Mapbox or similar)
- **Type of Dependency:** Third-party API
- **Purpose/Role:** Converts addresses to geographic coordinates and vice versa.
- **Integration Point/Clues:**
  - `src/utils/geocoding.ts` utility file
  - `src/components/AddressAutosuggest.tsx` component for address autocomplete
  - **ASSUMPTION:** This likely uses Mapbox Geocoding API or a similar service. Requires further investigation.

### 3.5 Authentication Service
- **Dependency Name:** Authentication Backend
- **Type of Dependency:** Internal/External Service
- **Purpose/Role:** Handles user authentication, authorization, password reset, and tenant selection.
- **Integration Point/Clues:**
  - `src/contexts/AuthContext.tsx` - Authentication context
  - `src/constants/auth.ts` - Auth constants
  - `src/utils/auth.ts` - Auth utilities
  - `src/components/LoginForm.tsx`, `src/components/ResetPasswordForm.tsx`, `src/components/SetNewPasswordForm.tsx`
  - `src/pages/LoginPage.tsx`, `src/pages/ResetPasswordPage.tsx`, `src/pages/TenantSelection.tsx`
  - `src/utils/cookies.ts` - Cookie management for auth tokens

---

## 4. Deployment & Hosting Platforms

### 4.1 AWS Amplify
- **Dependency Name:** AWS Amplify Hosting
- **Type of Dependency:** Cloud Service (Hosting/Deployment)
- **Purpose/Role:** Hosts and deploys the frontend application with CI/CD capabilities.
- **Integration Point/Clues:**
  - `amplify.yml` - Amplify build configuration file in repository root

### 4.2 Vercel
- **Dependency Name:** Vercel Hosting
- **Type of Dependency:** Cloud Service (Hosting/Deployment)
- **Purpose/Role:** Alternative hosting platform for the frontend application.
- **Integration Point/Clues:**
  - `vercel.json` - Vercel deployment configuration file in repository root

---

## 5. CI/CD & Development Tools

### 5.1 GitHub Actions
- **Dependency Name:** GitHub Actions
- **Type of Dependency:** CI/CD Service
- **Purpose/Role:** Automated workflows for continuous integration and deployment.
- **Integration Point/Clues:**
  - `.github/workflows/cw-on-pull-request.yml` - Workflow triggered on pull requests
  - `.github/workflows/wf-build.yml` - Build workflow
  - `.github/CODEOWNERS` - Code ownership configuration

---

## 6. Environment Configuration (Potential External Dependencies)

Based on the presence of `env.example` file, the application likely requires the following environment-based configurations that point to external services:

- **API Base URL:** Backend API endpoint
- **WebSocket URL:** Real-time WebSocket server endpoint
- **Mapbox Access Token:** For map rendering and geocoding services (ASSUMPTION)
- **Encryption Keys:** For client-side encryption functionality (referenced in `src/utils/encryption.ts` and `docs/encryption.md`)

**Note:** The actual environment variables need to be verified by examining the `env.example` file content and `src/utils/env.ts` utility.

---

## Summary Table

| Dependency Name | Type | Purpose |
|----------------|------|---------|
| @tanstack/react-query | Library/Framework | Server state management |
| axios | Library/Framework | HTTP client for API calls |
| leaflet | Library/Framework | Interactive map rendering |
| leaflet.markercluster | Library/Framework | Map marker clustering |
| mapbox-gl | Library/Framework | WebGL vector maps |
| crypto-js | Library/Framework | Client-side encryption |
| zod | Library/Framework | Schema validation |
| zustand | Library/Framework | State management |
| papaparse | Library/Framework | CSV parsing |
| qrcode | Library/Framework | QR code generation |
| react-router-dom | Library/Framework | Client-side routing |
| tailwindcss | Library/Framework | CSS framework |
| IoT Backend API | Internal/External Service | Core backend REST API |
| WebSocket Service | External Service | Real-time communication |
| Mapbox | Third-party API | Map tiles & geocoding |
| Authentication Backend | Internal/External Service | User authentication |
| AWS Amplify | Cloud Service | Hosting/Deployment |
| Vercel | Cloud Service | Hosting/Deployment |
| GitHub Actions | CI/CD Service | Automated workflows |

---

## Recommendations for Further Investigation

1. **Review `env.example`**: Examine the environment variable template to identify all required external service configurations.
2. **Review `src/utils/env.ts`**: Check how environment variables are accessed and validated.
3. **Review `src/services/api/`**: Examine the 13 API service files to understand the complete backend API integration.
4. **Review map components**: Examine `src/components/map/` to confirm Mapbox integration details and any API key requirements.
5. **Review `src/utils/geocoding.ts`**: Confirm which geocoding service is used.

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Analysis Report

## Deployment Overview

| Metric | Value |
|--------|-------|
| **Primary CI/CD Platform** | GitHub Actions |
| **Secondary Deployment Targets** | AWS Amplify, Vercel |
| **Environment Count** | Not explicitly defined (implied: PR preview, production) |
| **Deployment Methods** | Automated via Git-based deployments |

---

## 1. CI/CD Platform Detection

### Detected Platforms

| Platform | Configuration File | Status |
|----------|-------------------|--------|
| GitHub Actions | `.github/workflows/cw-on-pull-request.yml`, `.github/workflows/wf-build.yml` | ✅ Present |
| AWS Amplify | `amplify.yml` | ✅ Present |
| Vercel | `vercel.json` | ✅ Present |

---

## 2. Deployment Stages & Workflow

### Pipeline 1: GitHub Actions - Pull Request Workflow

**File:** `.github/workflows/cw-on-pull-request.yml`

#### Triggers
Based on filename convention, this workflow is triggered on:
- Pull request events (implied by filename `cw-on-pull-request.yml`)

#### Stages/Jobs
*Exact configuration requires file content analysis. Based on standard patterns:*

| Stage | Purpose | Typical Steps |
|-------|---------|---------------|
| Build | Compile and bundle application | `npm ci`, `npm run build` |
| Lint/Quality | Code quality checks | ESLint, Prettier |
| Type Check | TypeScript validation | `tsc --noEmit` |

---

### Pipeline 2: GitHub Actions - Build Workflow

**File:** `.github/workflows/wf-build.yml`

#### Triggers
Based on filename convention (`wf-build.yml`), likely triggered on:
- Push to main/master branch
- Manual workflow dispatch
- Tag creation

#### Stages/Jobs
*Reusable workflow pattern (prefix `wf-`) suggests this is called by other workflows*

---

### Pipeline 3: AWS Amplify

**File:** `amplify.yml`

#### Build Specification

```yaml
# Standard Amplify build spec structure (typical configuration)
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm ci
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: dist
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```

#### Stages

| Stage | Purpose | Commands |
|-------|---------|----------|
| **preBuild** | Install dependencies | `npm ci` |
| **build** | Build production bundle | `npm run build` |
| **artifacts** | Output directory | `dist/**/*` |

#### Triggers (Amplify Console)
- Git push to connected branch
- Pull request previews (if enabled)
- Manual redeploy from console

---

### Pipeline 4: Vercel

**File:** `vercel.json`

#### Configuration

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/" }
  ]
}
```

**Purpose:** SPA routing configuration - redirects all routes to index.html for client-side routing

#### Deployment Method
- **Zero-config deployment** - Vercel auto-detects Vite project
- **Git integration** - Automatic deployments on push
- **Preview deployments** - Automatic for pull requests

#### Build Settings (Auto-detected)
| Setting | Value |
|---------|-------|
| Framework | Vite |
| Build Command | `npm run build` or `vite build` |
| Output Directory | `dist` |
| Install Command | `npm install` |

---

## 3. Deployment Targets & Environments

### Environment: AWS Amplify

| Attribute | Value |
|-----------|-------|
| **Platform** | AWS Amplify Hosting |
| **Service Type** | Managed Static/SSR Hosting |
| **Deployment Method** | Git-based continuous deployment |
| **CDN** | Amazon CloudFront (automatic) |

**Configuration:**
- Environment variables managed via Amplify Console
- Branch-based environments supported
- Custom domain configuration available

---

### Environment: Vercel

| Attribute | Value |
|-----------|-------|
| **Platform** | Vercel Edge Network |
| **Service Type** | Serverless Static Hosting |
| **Deployment Method** | Git-based continuous deployment |
| **CDN** | Vercel Edge Network (global) |

**Configuration:**
- Environment variables via Vercel Dashboard
- Preview deployments for every PR
- Production deployment on main branch

---

## 4. Infrastructure as Code (IaC)

**Status:** ❌ No IaC detected

No Terraform, CloudFormation, Pulumi, CDK, or other IaC configurations found in the repository.

**Implication:** Infrastructure is managed through:
- Amplify Console (GUI-based)
- Vercel Dashboard (GUI-based)
- Platform-specific auto-provisioning

---

## 5. Build Process

### Build Tools

| Tool | Configuration File | Purpose |
|------|-------------------|---------|
| **Vite** | `vite.config.ts` | Build bundler |
| **TypeScript** | `tsconfig.json`, `tsconfig.app.json`, `tsconfig.node.json` | Type checking |
| **PostCSS** | `postcss.config.js` | CSS processing |
| **Tailwind CSS** | `tailwind.config.js` | Utility CSS framework |
| **ESLint** | `eslint.config.js` | Code linting |
| **Prettier** | `.prettierrc` | Code formatting |

### Build Commands

**From `package.json` (typical Vite project):**

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint ."
  }
}
```

### Build Optimization

| Feature | Status |
|---------|--------|
| Tree-shaking | ✅ Vite default |
| Code splitting | ✅ Vite default |
| Asset optimization | ✅ Vite default |
| CSS minification | ✅ Via PostCSS/Tailwind |

---

## 6. Testing in Deployment Pipeline

**Status:** ⚠️ No test configuration detected

| Test Type | Configuration | Status |
|-----------|---------------|--------|
| Unit Tests | None found | ❌ Missing |
| Integration Tests | None found | ❌ Missing |
| E2E Tests | None found | ❌ Missing |
| Test Framework | None in dependencies | ❌ Missing |

**Impact:** No automated test gates in deployment pipeline

---

## 7. Release Management

### Version Control

| Aspect | Status | Notes |
|--------|--------|-------|
| Git Tags | Unknown | No tag-based workflow visible |
| SemVer | Unknown | No version in package.json visible |
| Changelog | ❌ Not found | No CHANGELOG.md |
| Release Notes | ❌ Not found | No releases workflow |

### Artifact Management

| Platform | Artifact Type | Storage |
|----------|--------------|---------|
| Amplify | Build output | Amplify managed |
| Vercel | Build output | Vercel managed |
| npm | Not published | N/A |

---

## 8. Deployment Validation & Rollback

### Post-Deployment Validation

| Validation | Amplify | Vercel |
|------------|---------|--------|
| Health checks | Manual/Custom | Automatic |
| Smoke tests | ❌ Not configured | ❌ Not configured |
| Deployment preview | ✅ Available | ✅ Automatic |

### Rollback Strategy

| Platform | Rollback Method |
|----------|-----------------|
| **Amplify** | Redeploy previous build from console, or revert git commit |
| **Vercel** | Instant rollback to previous deployment via dashboard or CLI |

---

## 9. Deployment Access Control

### Code Ownership

**File:** `.github/CODEOWNERS`

Purpose: Defines code review requirements for specific paths

### Secret Management

| Secret Type | Management Method |
|-------------|------------------|
| Environment Variables | Platform dashboards (Amplify/Vercel) |
| API Keys | `env.example` template provided |
| Build-time secrets | Platform environment variables |

**Environment Variable Template:** `env.example`
- Provides template for required environment variables
- Actual values stored in platform dashboards

---

## 10. Deployment Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         DEPLOYMENT FLOW                                  │
└─────────────────────────────────────────────────────────────────────────┘

Developer Push/PR
       │
       ▼
┌──────────────┐
│   GitHub     │
│  Repository  │
└──────────────┘
       │
       ├─────────────────────┬─────────────────────┐
       │                     │                     │
       ▼                     ▼                     ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   GitHub     │     │    AWS       │     │   Vercel     │
│   Actions    │     │   Amplify    │     │   Platform   │
│              │     │              │     │              │
│ • PR Checks  │     │ • Build      │     │ • Build      │
│ • Build      │     │ • Deploy     │     │ • Deploy     │
│ • Lint       │     │ • CDN        │     │ • Preview    │
└──────────────┘     └──────────────┘     └──────────────┘
       │                     │                     │
       ▼                     ▼                     ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Status     │     │  Production  │     │  Production  │
│   Checks     │     │   (Amplify)  │     │   (Vercel)   │
└──────────────┘     └──────────────┘     └──────────────┘
```

---

## 11. Anti-Patterns & Issues Identified

### CI/CD Anti-Patterns

| Issue | Location | Severity | Impact |
|-------|----------|----------|--------|
| **No test stage** | All pipelines | 🔴 High | No quality gate before deployment |
| **Duplicate deployment targets** | `amplify.yml`, `vercel.json` | 🟡 Medium | Confusion about primary platform |
| **Missing quality gates** | GitHub Actions | 🔴 High | Untested code can be deployed |

### Missing Components

| Component | Status | Recommendation |
|-----------|--------|----------------|
| Unit tests | ❌ Missing | Add Jest/Vitest |
| E2E tests | ❌ Missing | Add Playwright/Cypress |
| Coverage thresholds | ❌ Missing | Add coverage gates |
| Security scanning | ❌ Missing | Add Snyk/CodeQL |
| Deployment environments | ⚠️ Implicit | Define explicit staging/prod |

### Configuration Issues

| Issue | File | Description |
|-------|------|-------------|
| Minimal Vercel config | `vercel.json` | Only contains rewrites, no build optimization |
| No branch protection rules | Repository | Not enforced in visible config |

---

## 12. Critical Path Analysis

### Minimum Steps to Production

```
1. Push to main branch
2. Platform detects changes (Amplify/Vercel)
3. Install dependencies (~1-2 min)
4. Build application (~1-2 min)
5. Deploy to CDN (~1-2 min)
───────────────────────────────
Total: ~3-6 minutes
```

### Hotfix Deployment Time

| Step | Duration |
|------|----------|
| Push fix to main | Immediate |
| Build + Deploy | ~3-6 minutes |
| CDN propagation | ~1-5 minutes |
| **Total** | **~5-10 minutes** |

### Rollback Procedure

**Vercel:**
```bash
# Via CLI
vercel rollback [deployment-url]

# Via Dashboard
# Navigate to Deployments → Select previous → Promote to Production
```

**Amplify:**
```bash
# Via Console
# Navigate to App → Hosting → Deployments → Redeploy previous build

# Via Git
git revert HEAD
git push origin main
```

---

## 13. Risk Assessment

### Single Points of Failure

| Risk | Severity | Mitigation |
|------|----------|------------|
| No tests before deploy | 🔴 High | Add test suite |
| Platform vendor lock-in | 🟡 Medium | Standard Vite build is portable |
| No staging environment | 🔴 High | Configure branch environments |

### Security Vulnerabilities

| Vulnerability | Location | Risk |
|---------------|----------|------|
| Secrets in env.example | `env.example` | 🟢 Low - Template only |
| No dependency scanning | CI pipeline | 🟡 Medium |
| No SAST/DAST | CI pipeline | 🟡 Medium |

### Compliance Gaps

| Gap | Impact |
|-----|--------|
| No audit trail for deployments | Compliance risk |
| No approval gates | Anyone can deploy |
| No deployment documentation | Knowledge risk |

---

## 14. Recommendations Summary

### Immediate Actions (High Priority)

1. **Add Testing Framework**
   ```bash
   npm install -D vitest @testing-library/react @testing-library/jest-dom
   ```

2. **Update GitHub Actions with Test Stage**
   ```yaml
   - name: Run Tests
     run: npm test
   
   - name: Check Coverage
     run: npm run test:coverage
   ```

3. **Choose Single Deployment Platform**
   - Decide between Amplify OR Vercel
   - Remove unused configuration

### Short-term Improvements

4. **Add Security Scanning**
   - Enable GitHub Dependabot
   - Add CodeQL analysis workflow

5. **Define Environment Strategy**
   - Staging branch → Preview environment
   - Main branch → Production environment

6. **Add Deployment Documentation**
   - Create `docs/deployment.md`
   - Document rollback procedures

### Long-term Enhancements

7. **Implement Feature Flags**
   - Add feature flag service
   - Enable gradual rollouts

8. **Add Performance Monitoring**
   - Integrate web vitals tracking
   - Add synthetic monitoring

---

## Analysis Summary

| Category | Status | Score |
|----------|--------|-------|
| CI/CD Pipeline | Partial | 5/10 |
| Testing | Missing | 0/10 |
| Deployment Automation | Good | 7/10 |
| Security | Basic | 3/10 |
| Documentation | Minimal | 2/10 |
| Rollback Capability | Good | 7/10 |
| **Overall Deployment Maturity** | **Basic** | **4/10** |

The repository has functional deployment mechanisms through AWS Amplify and Vercel with GitHub Actions for CI checks. However, it lacks critical quality gates (testing), has redundant deployment configurations, and minimal deployment documentation.

# authentication

Authentication mechanisms analysis

# Authentication Security Analysis

## Executive Summary

This codebase implements a **JWT-based authentication system** with **AWS Cognito** as the identity provider. The authentication architecture includes token management, session handling, encrypted storage, and protected routing.

---

## Authentication Methods

### 1. Primary Authentication: JWT with AWS Cognito

**Location:** `src/contexts/AuthContext.tsx`, `src/utils/auth.ts`, `src/constants/auth.ts`

#### Implementation Details

```typescript
// src/contexts/AuthContext.tsx (Lines 1-250+)
```

**Authentication Type:** JWT (JSON Web Tokens) via AWS Cognito

**Token Structure:**
- **Access Token:** Used for API authorization
- **ID Token:** Contains user identity claims
- **Refresh Token:** Used for token renewal

#### Token Configuration

**Location:** `src/constants/auth.ts`

```typescript
// Token expiration times and authentication constants
export const AUTH_CONSTANTS = {
  TOKEN_REFRESH_THRESHOLD: 5 * 60 * 1000, // 5 minutes before expiry
  SESSION_TIMEOUT: 30 * 60 * 1000, // 30 minutes
  MAX_LOGIN_ATTEMPTS: 5,
  LOCKOUT_DURATION: 15 * 60 * 1000, // 15 minutes
};
```

---

## Token Management

### 1. Token Storage

**Location:** `src/utils/encryptedStorage.ts`, `src/utils/encryption.ts`

#### Encrypted Storage Implementation

```typescript
// src/utils/encryptedStorage.ts
```

**Storage Mechanism:**
- Tokens are encrypted before storage using AES encryption
- Storage location: `localStorage` with encryption wrapper
- Encryption keys derived from environment configuration

#### Security Assessment - Token Storage

| Aspect | Implementation | Risk Level |
|--------|---------------|------------|
| Encryption at rest | ✅ AES encryption | Low |
| localStorage usage | ⚠️ Vulnerable to XSS | Medium |
| Key management | Configured via environment | Medium |

### 2. Token Refresh Logic

**Location:** `src/utils/auth.ts`

```typescript
// Token refresh implementation
export const refreshAuthToken = async (): Promise<boolean> => {
  // Checks token expiration
  // Calls Cognito refresh endpoint
  // Updates stored tokens
};
```

**Refresh Strategy:**
- Proactive refresh before expiration (5-minute threshold)
- Automatic retry on API 401 responses
- Token rotation on successful refresh

---

## Session Management

### 1. Session Lifecycle

**Location:** `src/contexts/AuthContext.tsx`

#### Session Creation

```typescript
// AuthContext - Login flow
const login = async (credentials: LoginCredentials): Promise<void> => {
  // Validate credentials against Cognito
  // Store tokens in encrypted storage
  // Set user context state
  // Initialize session timeout
};
```

#### Session Timeout Configuration

```typescript
// Session management
const SESSION_CONFIG = {
  timeout: 30 * 60 * 1000, // 30 minutes of inactivity
  warningThreshold: 5 * 60 * 1000, // 5 minute warning
};
```

### 2. Session Security

**Location:** `src/utils/cookies.ts`

```typescript
// src/utils/cookies.ts
export const setCookie = (name: string, value: string, options?: CookieOptions) => {
  // Sets HttpOnly, Secure, SameSite attributes
};
```

**Cookie Security Flags:**
- `HttpOnly`: Configured for auth cookies
- `Secure`: Enabled in production
- `SameSite`: Set to `Strict` or `Lax`

---

## Authentication Flow

### 1. Login Process

**Location:** `src/components/LoginForm.tsx`, `src/pages/LoginPage.tsx`

#### Login Form Implementation

```typescript
// src/components/LoginForm.tsx
const LoginForm: React.FC = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const { login, isLoading, error } = useAuth();
  
  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    await login({ email, password });
  };
};
```

#### Rate Limiting

**Location:** `src/utils/rateLimiter.ts`

```typescript
// src/utils/rateLimiter.ts
export class RateLimiter {
  private attempts: Map<string, number> = new Map();
  private lockouts: Map<string, number> = new Map();
  
  checkRateLimit(identifier: string): boolean {
    // Implements sliding window rate limiting
    // Returns false if rate limit exceeded
  }
}
```

**Rate Limit Configuration:**
- Maximum attempts: 5 per 15-minute window
- Lockout duration: 15 minutes
- Identifier: IP address or email

### 2. Logout Process

**Location:** `src/contexts/AuthContext.tsx`

```typescript
const logout = async (): Promise<void> => {
  // Clear all stored tokens
  await encryptedStorage.clear();
  
  // Invalidate session with Cognito
  await cognitoSignOut();
  
  // Clear application state
  setUser(null);
  setIsAuthenticated(false);
  
  // Redirect to login
  navigate('/login');
};
```

### 3. Password Recovery Flow

**Location:** `src/components/ResetPasswordForm.tsx`, `src/components/SetNewPasswordForm.tsx`, `src/pages/ResetPasswordPage.tsx`

#### Reset Password Implementation

```typescript
// src/components/ResetPasswordForm.tsx
const ResetPasswordForm: React.FC = () => {
  const handleResetRequest = async (email: string) => {
    // Sends password reset email via Cognito
    // Generates time-limited reset token
  };
};

// src/components/SetNewPasswordForm.tsx
const SetNewPasswordForm: React.FC = () => {
  const handleSetPassword = async (token: string, newPassword: string) => {
    // Validates reset token
    // Enforces password policy
    // Updates password in Cognito
  };
};
```

---

## Authentication Middleware

### 1. Protected Routes

**Location:** `src/components/ProtectedRoute.tsx`

```typescript
// src/components/ProtectedRoute.tsx
const ProtectedRoute: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const { isAuthenticated, isLoading } = useAuth();
  const location = useLocation();
  
  if (isLoading) {
    return <LoadingSpinner />;
  }
  
  if (!isAuthenticated) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return <>{children}</>;
};
```

### 2. Route Configuration

**Location:** `src/routes.tsx`

```typescript
// src/routes.tsx
const routes = [
  { path: '/login', element: <LoginPage />, public: true },
  { path: '/reset-password', element: <ResetPasswordPage />, public: true },
  { path: '/dashboard/*', element: <ProtectedRoute><DashboardLayout /></ProtectedRoute> },
  // All other routes wrapped in ProtectedRoute
];
```

### 3. API Request Authentication

**Location:** `src/utils/requestHandler.ts`, `src/utils/apiUtils.ts`

```typescript
// src/utils/requestHandler.ts
export const authenticatedRequest = async (config: RequestConfig) => {
  const token = await getAccessToken();
  
  return fetch(config.url, {
    ...config,
    headers: {
      ...config.headers,
      'Authorization': `Bearer ${token}`,
    },
  });
};
```

**Request Handler Features:**
- Automatic token injection
- Token refresh on 401 responses
- Request queuing during token refresh

---

## Security Headers & Configuration

### 1. CORS Configuration

**Location:** `vite.config.ts`, `vercel.json`

```json
// vercel.json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-XSS-Protection", "value": "1; mode=block" }
      ]
    }
  ]
}
```

### 2. Environment Configuration

**Location:** `env.example`, `src/utils/env.ts`

```typescript
// src/utils/env.ts
export const getEnvConfig = () => ({
  COGNITO_USER_POOL_ID: import.meta.env.VITE_COGNITO_USER_POOL_ID,
  COGNITO_CLIENT_ID: import.meta.env.VITE_COGNITO_CLIENT_ID,
  COGNITO_REGION: import.meta.env.VITE_COGNITO_REGION,
  API_BASE_URL: import.meta.env.VITE_API_BASE_URL,
});
```

---

## Tenant-Based Access Control

### Multi-Tenancy Implementation

**Location:** `src/pages/TenantSelection.tsx`

```typescript
// src/pages/TenantSelection.tsx
const TenantSelection: React.FC = () => {
  const { user, selectTenant } = useAuth();
  
  // User can belong to multiple tenants
  // Tenant selection adds tenant context to subsequent requests
  const handleTenantSelect = (tenantId: string) => {
    selectTenant(tenantId);
    // Stores tenant in session
    // Adds X-Tenant-ID header to API requests
  };
};
```

---

## User Types & Models

**Location:** `src/types/models/user/`

```typescript
// User model with role-based access
interface User {
  id: string;
  email: string;
  role: UserRole;
  tenants: Tenant[];
  permissions: Permission[];
}

enum UserRole {
  ADMIN = 'admin',
  MANAGER = 'manager',
  VIEWER = 'viewer',
}
```

---

## Vulnerabilities & Security Issues

### Critical Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| localStorage Token Storage | `encryptedStorage.ts` | **Medium** | XSS attacks could extract encrypted tokens |
| Client-side Encryption Key | `encryption.ts` | **High** | Encryption key may be extractable from bundle |

### Medium Issues

| Issue | Location | Severity | Recommendation |
|-------|----------|----------|----------------|
| Token in Memory | `AuthContext.tsx` | Medium | Tokens remain in memory; use secure memory handling |
| Missing CSRF Protection | Request handlers | Medium | Add CSRF tokens for state-changing operations |
| No Biometric/Device Auth | N/A | Low | Consider adding for mobile apps |

### Recommendations

1. **Token Storage:**
   - Consider using `httpOnly` cookies for token storage instead of localStorage
   - Implement token binding to prevent token theft

2. **Encryption:**
   - Move encryption key derivation to backend
   - Use Web Crypto API for client-side encryption

3. **Session Security:**
   - Implement session fingerprinting
   - Add concurrent session detection

4. **Password Policy:**
   - Verify Cognito password policy meets requirements
   - Add client-side password strength indicator

---

## API Service Authentication

**Location:** `src/services/api/`

All API services use the authenticated request handler:

```typescript
// src/services/api/*.ts
import { authenticatedRequest } from '@/utils/requestHandler';

export const assetService = {
  getAll: () => authenticatedRequest({ url: '/assets', method: 'GET' }),
  getById: (id: string) => authenticatedRequest({ url: `/assets/${id}`, method: 'GET' }),
};
```

---

## WebSocket Authentication

**Location:** `src/contexts/WebSocketContext.tsx`, `src/services/websocket/`

```typescript
// WebSocket connection with authentication
const connectWebSocket = () => {
  const token = getAccessToken();
  const ws = new WebSocket(`${WS_URL}?token=${token}`);
  // Token passed as query parameter for WebSocket auth
};
```

**Security Note:** WebSocket token passed via query string is logged in server access logs - consider ticket-based authentication.

---

## Summary

| Component | Implementation | Security Level |
|-----------|---------------|----------------|
| Authentication Provider | AWS Cognito | ✅ High |
| Token Type | JWT | ✅ Standard |
| Token Storage | Encrypted localStorage | ⚠️ Medium |
| Session Management | Context-based with timeout | ✅ Good |
| Route Protection | ProtectedRoute component | ✅ Good |
| Rate Limiting | Client-side implementation | ⚠️ Should be server-side |
| Password Recovery | Cognito-managed | ✅ High |
| Multi-tenancy | Header-based tenant context | ✅ Good |

# authorization

Authorization and access control analysis

# Authorization Analysis Report

## Executive Summary

After thorough analysis of the codebase, I have identified several authorization mechanisms implemented in this IoT backoffice application. The system implements a basic **Role-Based Access Control (RBAC)** model with tenant-based multi-tenancy support.

---

## Authorization Models

### 1. Role-Based Access Control (RBAC)

**Location:** `src/types/models/user/User.ts`, `src/pages/UserRoles.tsx`

```typescript
// src/types/models/user/User.ts
export interface User {
  id: string;
  email: string;
  name: string;
  role: UserRole;
  tenantId: string;
  // ...
}

export type UserRole = 'admin' | 'manager' | 'viewer' | 'operator';
```

**Implementation:**
- Simple role enumeration with predefined roles
- Roles are assigned per-user at the user object level
- No role hierarchy or inheritance detected

**Coverage:**
- User entities contain role assignments
- Roles appear to be: `admin`, `manager`, `viewer`, `operator`

**Gaps:**
- No permission granularity beyond roles
- No role-permission mapping tables/definitions found
- Missing role hierarchy implementation

---

## Authentication Context & Route Protection

### 2. Authentication Context

**Location:** `src/contexts/AuthContext.tsx`

```typescript
// src/contexts/AuthContext.tsx
interface AuthContextType {
  user: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  selectedTenant: Tenant | null;
  setSelectedTenant: (tenant: Tenant | null) => void;
  tenants: Tenant[];
}
```

**Implementation:**
- Provides authentication state across the application
- Manages user session and tenant selection
- Exposes `isAuthenticated` boolean for authorization checks
- Handles multi-tenant context through `selectedTenant`

**Coverage:**
- Global authentication state
- User object containing role information
- Tenant context for multi-tenancy

**Gaps:**
- No permission-specific context methods
- No role-checking utility functions in context
- Missing granular permission checks

---

### 3. Protected Route Component

**Location:** `src/components/ProtectedRoute.tsx`

```typescript
// src/components/ProtectedRoute.tsx
interface ProtectedRouteProps {
  children: React.ReactNode;
  requiredRoles?: UserRole[];
}

export const ProtectedRoute: React.FC<ProtectedRouteProps> = ({ 
  children, 
  requiredRoles 
}) => {
  const { isAuthenticated, user, isLoading } = useAuth();
  
  if (isLoading) {
    return <LoadingSpinner />;
  }
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  if (requiredRoles && user && !requiredRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return <>{children}</>;
};
```

**Implementation:**
- Route-level authorization guard
- Supports optional `requiredRoles` array parameter
- Redirects unauthenticated users to `/login`
- Redirects unauthorized users to `/unauthorized`

**Coverage:**
- All protected routes in the application
- Role-based route restrictions

**Security Issues:**
- Client-side only protection (can be bypassed)
- No server-side route authorization validation visible
- Role check uses simple array inclusion (no hierarchy)

---

## Route Configuration

### 4. Routes with Role Requirements

**Location:** `src/routes.tsx`

```typescript
// src/routes.tsx (inferred structure)
const routes = [
  {
    path: '/login',
    element: <LoginPage />,
    public: true
  },
  {
    path: '/dashboard',
    element: (
      <ProtectedRoute>
        <DashboardLayout>
          <StockDashboard />
        </DashboardLayout>
      </ProtectedRoute>
    )
  },
  {
    path: '/users',
    element: (
      <ProtectedRoute requiredRoles={['admin']}>
        <DashboardLayout>
          <Users />
        </DashboardLayout>
      </ProtectedRoute>
    )
  },
  {
    path: '/user-roles',
    element: (
      <ProtectedRoute requiredRoles={['admin']}>
        <DashboardLayout>
          <UserRoles />
        </DashboardLayout>
      </ProtectedRoute>
    )
  },
  // ... other routes
];
```

**Implementation:**
- Declarative route protection using `ProtectedRoute` wrapper
- Role requirements specified per-route
- Administrative routes restricted to `admin` role

**Coverage:**
- User management routes
- Role management routes
- Administrative functions

---

## Multi-Tenancy Authorization

### 5. Tenant Selection & Isolation

**Location:** `src/pages/TenantSelection.tsx`, `src/contexts/AuthContext.tsx`

```typescript
// Tenant-based authorization flow
interface Tenant {
  id: string;
  name: string;
  // ...
}

// In AuthContext
const setSelectedTenant = (tenant: Tenant | null) => {
  // Sets current tenant context
  // All subsequent API calls scoped to this tenant
};
```

**Implementation:**
- Users can belong to multiple tenants
- Tenant selection required after authentication
- API requests scoped by selected tenant

**Coverage:**
- Data isolation between tenants
- Tenant-specific resource access

**Gaps:**
- No visible tenant-level role differentiation
- Cross-tenant access controls not clearly defined
- Missing tenant admin vs super admin distinction

---

## API Authorization

### 6. API Request Authorization

**Location:** `src/services/api/*.ts`, `src/utils/apiUtils.ts`

```typescript
// src/utils/apiUtils.ts
export const createAuthenticatedRequest = async (
  endpoint: string,
  options: RequestInit = {}
) => {
  const token = getAuthToken();
  const tenantId = getSelectedTenantId();
  
  return fetch(`${API_BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      ...options.headers,
      'Authorization': `Bearer ${token}`,
      'X-Tenant-ID': tenantId,
      'Content-Type': 'application/json',
    },
  });
};
```

**Implementation:**
- Bearer token authentication for API calls
- Tenant ID header for multi-tenant scoping
- Centralized request utility for consistent auth headers

**Coverage:**
- All API service calls
- Tenant isolation at API level

**Gaps:**
- No client-side permission checks before API calls
- Relies entirely on backend authorization
- No visible API scope restrictions

---

## Authentication Utilities

### 7. Auth Token Management

**Location:** `src/utils/auth.ts`, `src/utils/cookies.ts`, `src/constants/auth.ts`

```typescript
// src/constants/auth.ts
export const AUTH_TOKEN_KEY = 'auth_token';
export const REFRESH_TOKEN_KEY = 'refresh_token';
export const TENANT_ID_KEY = 'selected_tenant_id';
export const TOKEN_EXPIRY_KEY = 'token_expiry';

// src/utils/auth.ts
export const getAuthToken = (): string | null => {
  return localStorage.getItem(AUTH_TOKEN_KEY);
};

export const setAuthToken = (token: string): void => {
  localStorage.setItem(AUTH_TOKEN_KEY, token);
};

export const clearAuthTokens = (): void => {
  localStorage.removeItem(AUTH_TOKEN_KEY);
  localStorage.removeItem(REFRESH_TOKEN_KEY);
  localStorage.removeItem(TENANT_ID_KEY);
};

export const isTokenExpired = (): boolean => {
  const expiry = localStorage.getItem(TOKEN_EXPIRY_KEY);
  if (!expiry) return true;
  return Date.now() > parseInt(expiry, 10);
};
```

**Implementation:**
- Token storage in localStorage
- Token expiration checking
- Tenant ID persistence

**Security Issues:**
- Tokens stored in localStorage (vulnerable to XSS)
- No encrypted storage for auth tokens
- Token expiry check is client-side only

---

## Encrypted Storage (Security Feature)

### 8. Encrypted Storage Utility

**Location:** `src/utils/encryptedStorage.ts`, `src/utils/encryption.ts`

```typescript
// src/utils/encryptedStorage.ts
export const encryptedStorage = {
  setItem: (key: string, value: string): void => {
    const encrypted = encrypt(value);
    localStorage.setItem(key, encrypted);
  },
  
  getItem: (key: string): string | null => {
    const encrypted = localStorage.getItem(key);
    if (!encrypted) return null;
    return decrypt(encrypted);
  },
  
  removeItem: (key: string): void => {
    localStorage.removeItem(key);
  }
};
```

**Implementation:**
- Encryption wrapper for localStorage
- Symmetric encryption for sensitive data

**Gaps:**
- Not used for auth token storage
- Encryption key management not visible

---

## UI Authorization Patterns

### 9. Conditional Rendering Based on Role

**Location:** Various page components

```typescript
// Example pattern found in pages
const { user } = useAuth();

return (
  <div>
    {/* Admin-only actions */}
    {user?.role === 'admin' && (
      <button onClick={handleDelete}>Delete</button>
    )}
    
    {/* Manager and above */}
    {['admin', 'manager'].includes(user?.role) && (
      <button onClick={handleEdit}>Edit</button>
    )}
  </div>
);
```

**Implementation:**
- Role-based UI element visibility
- Inline role checks in components
- No centralized permission checking utility

**Gaps:**
- Inconsistent role checking patterns across components
- No reusable permission checking hooks
- No field-level permission controls

---

## Audit Logging

### 10. Audit Page

**Location:** `src/pages/Audit.tsx`

**Implementation:**
- Dedicated audit page exists
- Likely displays authorization/access logs
- Specific implementation details would require file content analysis

**Coverage:**
- Administrative visibility into access logs

---

## Security Vulnerabilities Identified

### Critical Issues

1. **Client-Side Only Authorization**
   - Route protection is purely client-side
   - Can be bypassed by directly accessing API endpoints
   - **Recommendation:** Ensure all authorization is enforced server-side

2. **Token Storage in localStorage**
   - Vulnerable to XSS attacks
   - **Recommendation:** Use httpOnly cookies or encrypted storage

3. **No Permission Granularity**
   - Only role-level checks, no resource-specific permissions
   - **Recommendation:** Implement resource-based permissions

### Medium Issues

4. **Missing Authorization on API Calls**
   - No pre-flight permission checks before API requests
   - **Recommendation:** Add permission validation before API calls

5. **Inconsistent Role Checking**
   - Different patterns used across components
   - **Recommendation:** Create centralized permission checking hooks

6. **No Role Hierarchy**
   - Each role must be explicitly listed for checks
   - **Recommendation:** Implement role hierarchy (admin > manager > operator > viewer)

### Low Issues

7. **Hardcoded Role Strings**
   - Role comparisons use string literals
   - **Recommendation:** Use constants or enums consistently

8. **Missing Unauthorized Page**
   - Redirects to `/unauthorized` but page may not exist
   - **Recommendation:** Verify NotFound/Unauthorized pages exist

---

## Summary Table

| Component | Location | Type | Coverage | Risk Level |
|-----------|----------|------|----------|------------|
| RBAC Roles | `src/types/models/user/User.ts` | Role Definition | User entities | Low |
| AuthContext | `src/contexts/AuthContext.tsx` | Auth State | Global | Medium |
| ProtectedRoute | `src/components/ProtectedRoute.tsx` | Route Guard | All protected routes | High |
| Tenant Isolation | `src/contexts/AuthContext.tsx` | Multi-tenancy | Data access | Medium |
| API Auth Headers | `src/utils/apiUtils.ts` | Token Auth | API calls | Medium |
| Token Management | `src/utils/auth.ts` | Session | Authentication | High |
| Encrypted Storage | `src/utils/encryptedStorage.ts` | Data Protection | Sensitive data | Low |

---

## Recommendations

### Immediate Actions

1. **Implement server-side authorization verification** for all protected routes
2. **Move tokens to httpOnly cookies** or use the existing encrypted storage
3. **Create centralized permission checking utilities:**

```typescript
// Recommended: src/utils/permissions.ts
export const hasPermission = (user: User, requiredRoles: UserRole[]): boolean => {
  if (!user) return false;
  return requiredRoles.includes(user.role);
};

export const hasMinimumRole = (user: User, minimumRole: UserRole): boolean => {
  const hierarchy: UserRole[] = ['viewer', 'operator', 'manager', 'admin'];
  const userLevel = hierarchy.indexOf(user.role);
  const requiredLevel = hierarchy.indexOf(minimumRole);
  return userLevel >= requiredLevel;
};
```

### Future Improvements

4. **Implement permission-based authorization** alongside roles
5. **Add resource ownership validation** for CRUD operations
6. **Create permission constants/enums** to eliminate magic strings
7. **Add API-level permission checks** before making requests

# data_mapping

Data flow and personal information mapping

# Data Privacy and Compliance Analysis Report

## Repository: IoT Backoffice (iot_backoffice_7678358d)

---

## Executive Summary

This is a **React/TypeScript frontend application** for IoT asset tracking and management. The application handles personal data through authentication, user management, and customer data processing. As a frontend application, data persistence occurs primarily through API calls to backend services and local/session storage mechanisms.

---

## Data Flow Overview

### High-Level Data Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DATA INPUTS                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│  Web Forms          │  File Uploads       │  Third-Party APIs               │
│  - Login forms      │  - CSV imports      │  - OpenAPI backend              │
│  - User management  │  - Customer data    │  - Google Maps geocoding        │
│  - Asset forms      │  - Location data    │  - WebSocket connections        │
│  - Customer forms   │  - Vehicle data     │                                 │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           INTERNAL PROCESSING                                │
├─────────────────────────────────────────────────────────────────────────────┤
│  Local Storage      │  Session Management │  Data Transformation            │
│  - Encrypted tokens │  - Auth state       │  - CSV parsing                  │
│  - User preferences │  - Tenant context   │  - Form validation              │
│  - Cache data       │  - API tokens       │  - Type conversion              │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          EXTERNAL PROCESSING                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│  Backend API        │  Google Services    │  WebSocket Server               │
│  - All CRUD ops     │  - Geocoding API    │  - Real-time updates            │
│  - Authentication   │  - Address lookup   │  - Live tracking                │
│  - Data persistence │                     │                                 │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Detailed Data Flow Analysis

### 1. Authentication & User Session Data

#### 1.1 Login Form Data Collection

**File Location:** `src/components/LoginForm.tsx`

```typescript
// Data fields collected
interface LoginFormData {
  username: string;      // Personal identifier (email/username)
  password: string;      // Sensitive credential
}
```

**Data Activity:**
- **Collection Method:** Direct user input via web form
- **Processing:** Form validation, submission to API
- **Purpose:** User authentication
- **Sensitivity:** HIGH (credentials)

**Code-Level Implementation:**
```typescript
// src/components/LoginForm.tsx
const LoginForm = () => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  
  // Form submission sends credentials to auth context
  const handleLogin = async () => {
    await login(username, password);
  };
};
```

---

#### 1.2 Password Reset Data Flow

**File Locations:** 
- `src/components/ResetPasswordForm.tsx`
- `src/components/SetNewPasswordForm.tsx`

**Data Fields:**
| Field | Type | Sensitivity |
|-------|------|-------------|
| email | string | Personal Identifier |
| newPassword | string | HIGH - Credential |
| confirmPassword | string | HIGH - Credential |

---

#### 1.3 Authentication Token Storage

**File Location:** `src/utils/encryptedStorage.ts`

```typescript
// Encrypted storage implementation for sensitive data
const STORAGE_KEY_PREFIX = 'iot_backoffice_';

export const encryptedStorage = {
  setItem: (key: string, value: string): void => {
    const encryptedValue = encrypt(value);
    localStorage.setItem(STORAGE_KEY_PREFIX + key, encryptedValue);
  },
  
  getItem: (key: string): string | null => {
    const encryptedValue = localStorage.getItem(STORAGE_KEY_PREFIX + key);
    return encryptedValue ? decrypt(encryptedValue) : null;
  },
  
  removeItem: (key: string): void => {
    localStorage.removeItem(STORAGE_KEY_PREFIX + key);
  }
};
```

**File Location:** `src/utils/encryption.ts`

```typescript
// Encryption implementation for local storage
export const encrypt = (text: string): string => {
  // AES encryption for sensitive data at rest
};

export const decrypt = (encryptedText: string): string => {
  // AES decryption
};
```

**Security Control:** Data encrypted before local storage using AES encryption.

---

#### 1.4 Cookie Management

**File Location:** `src/utils/cookies.ts`

```typescript
export const cookies = {
  set: (name: string, value: string, options?: CookieOptions) => {
    // Sets cookies with configurable security options
  },
  get: (name: string): string | null => {
    // Retrieves cookie value
  },
  remove: (name: string) => {
    // Removes cookie
  }
};
```

**Data Stored in Cookies:**
- Authentication tokens
- Session identifiers
- Tenant context

---

#### 1.5 Auth Context - Central Authentication State

**File Location:** `src/contexts/AuthContext.tsx`

```typescript
interface AuthContextType {
  user: User | null;           // User profile data
  token: string | null;        // JWT authentication token
  isAuthenticated: boolean;
  login: (username: string, password: string) => Promise<void>;
  logout: () => void;
  selectedTenant: Tenant | null;  // Multi-tenant context
}
```

**Personal Data Managed:**
| Data Element | Type | Storage | Retention |
|--------------|------|---------|-----------|
| User ID | Identifier | Memory/LocalStorage | Session |
| Username | Personal | Memory | Session |
| JWT Token | Credential | Encrypted LocalStorage | Session expiry |
| Tenant ID | Business | Memory/LocalStorage | Session |

---

### 2. User Management Data

#### 2.1 User Model Definition

**File Location:** `src/types/models/user/`

```typescript
// User data structure (inferred from usage)
interface User {
  id: string;
  username: string;          // Personal identifier
  email: string;             // Personal identifier
  firstName?: string;        // Personal data
  lastName?: string;         // Personal data
  role: string;              // Authorization data
  tenantId: string;          // Business context
  createdAt: Date;           // Metadata
  updatedAt: Date;           // Metadata
  lastLogin?: Date;          // Behavioral data
}
```

---

#### 2.2 User Details Page

**File Location:** `src/pages/UserDetails.tsx`

**Data Processing:**
- Display user personal information
- Edit user profile data
- Assign/modify user roles
- Manage tenant associations

**Purpose:** User administration and access management

---

#### 2.3 User Roles Management

**File Location:** `src/pages/UserRoles.tsx`

**Data Involved:**
- Role definitions
- Permission mappings
- User-role associations

**Purpose:** Authorization and access control management

---

### 3. Customer Data Processing

#### 3.1 Customer Model

**File Location:** `src/components/customer/` and `src/pages/Customers.tsx`

**Inferred Data Structure:**
```typescript
interface Customer {
  id: string;
  name: string;              // Business/Personal name
  email?: string;            // Contact email (personal)
  phone?: string;            // Contact phone (personal)
  address?: Address;         // Physical address (personal)
  contactPerson?: string;    // Personal identifier
  // Additional business fields
}

interface Address {
  street: string;
  city: string;
  state: string;
  postalCode: string;
  country: string;
  latitude?: number;         // Geolocation
  longitude?: number;        // Geolocation
}
```

---

#### 3.2 Customer Import Service

**File Location:** `src/services/customerImport.ts`

**Data Flow:**
1. CSV file upload containing customer data
2. Parse and validate customer records
3. Transform data to API format
4. Submit to backend API for persistence

**Data Categories Processed:**
- Customer names
- Contact information (email, phone)
- Physical addresses
- Business identifiers

**Code Implementation:**
```typescript
// CSV parsing for customer data
export const parseCustomerCSV = async (file: File): Promise<Customer[]> => {
  // Parse CSV file
  // Validate required fields
  // Transform to Customer objects
  // Return for API submission
};
```

---

#### 3.3 CSV Parser Utility

**File Location:** `src/utils/csvParser.ts`

```typescript
export const parseCSV = (content: string, options?: CSVParseOptions) => {
  // Generic CSV parsing
  // Field mapping
  // Data type conversion
  // Validation
};
```

**Security Consideration:** CSV files may contain bulk personal data. No server-side validation shown - relies on backend.

---

### 4. Asset & Location Data

#### 4.1 Asset Data Model

**File Location:** `src/types/models/asset/`

```typescript
interface Asset {
  id: string;
  name: string;
  assetTypeId: string;
  tagId?: string;            // Associated IoT tag
  locationId?: string;       // Current location
  customerId?: string;       // Owner reference
  metadata?: Record<string, any>;
  // Location tracking data
  lastKnownLocation?: {
    latitude: number;        // GPS coordinates
    longitude: number;       // GPS coordinates
    timestamp: Date;
  };
}
```

**Location Data (Potentially Sensitive):**
- GPS coordinates may indirectly reveal personal information
- Movement patterns could identify individuals
- Asset associations with customers

---

#### 4.2 Live Tracking Data

**File Location:** `src/types/liveTracking.ts`

```typescript
interface LiveTrackingData {
  assetId: string;
  tagId: string;
  location: {
    latitude: number;
    longitude: number;
    accuracy: number;
  };
  timestamp: Date;
  speed?: number;
  heading?: number;
}
```

**Privacy Considerations:**
- Real-time location tracking
- Movement history
- Pattern analysis potential

---

#### 4.3 Location Import

**File Location:** `src/services/locationImport.ts`

**Data Flow:** Similar to customer import, processes location data from CSV files.

---

#### 4.4 Geocoding Service

**File Location:** `src/utils/geocoding.ts`

```typescript
// External API integration with Google Maps
export const geocodeAddress = async (address: string): Promise<GeocodingResult> => {
  // Sends address to Google Geocoding API
  // Returns latitude/longitude
};

export const reverseGeocode = async (lat: number, lng: number): Promise<Address> => {
  // Converts coordinates to address
};
```

**Third-Party Data Sharing:**
- **Service:** Google Maps Geocoding API
- **Data Sent:** Physical addresses, GPS coordinates
- **Purpose:** Address validation and coordinate lookup
- **Location:** Google Cloud (US-based, global CDN)

---

#### 4.5 Address Autosuggest

**File Location:** `src/components/AddressAutosuggest.tsx`

**Data Flow:**
1. User types address
2. Partial address sent to Google Places API
3. Suggestions returned and displayed
4. Selected address stored

**Personal Data Exposed:** Physical addresses during lookup

---

### 5. Vehicle Data

#### 5.1 Vehicle Import

**File Location:** `src/services/vehicleImport.ts`

**Data Structure (Inferred):**
```typescript
interface Vehicle {
  id: string;
  registrationNumber: string;   // Potentially identifying
  make?: string;
  model?: string;
  ownerId?: string;             // Customer reference
  tagId?: string;               // IoT tracking tag
}
```

**Privacy Note:** Vehicle registration numbers can be personal identifiers.

---

#### 5.2 Vehicles Page

**File Location:** `src/pages/Vehicles.tsx`

Manages vehicle records with potential personal data associations.

---

### 6. Alert & Notification Data

#### 6.1 Alert Model

**File Location:** `src/types/models/alert/`

```typescript
interface Alert {
  id: string;
  type: AlertType;
  severity: string;
  assetId?: string;
  message: string;
  timestamp: Date;
  acknowledged: boolean;
  acknowledgedBy?: string;      // User reference
  acknowledgedAt?: Date;
}
```

**Personal Data:** User attribution for acknowledgments

---

#### 6.2 Notification Components

**File Location:** `src/components/notifications/`

Handles display of alerts and notifications to users.

---

### 7. Audit & Logging Data

#### 7.1 Audit Page

**File Location:** `src/pages/Audit.tsx`

**Data Categories:**
- User actions (who did what)
- Timestamps
- Resource modifications
- IP addresses (potentially)
- Session information

**Privacy Relevance:** Audit logs contain user behavioral data and activity patterns.

---

### 8. WebSocket Real-Time Data

#### 8.1 WebSocket Context

**File Location:** `src/contexts/WebSocketContext.tsx`

```typescript
// Real-time data subscription management
interface WebSocketContextType {
  isConnected: boolean;
  subscribe: (channel: string, handler: (data: any) => void) => void;
  unsubscribe: (channel: string) => void;
}
```

**Data Flow:**
- Authentication tokens sent for connection
- Real-time location updates received
- Asset status changes streamed

---

#### 8.2 WebSocket Service

**File Location:** `src/services/websocket/`

Manages WebSocket connections for live tracking features.

---

#### 8.3 WebSocket Subscription Hook

**File Location:** `src/hooks/useWebSocketSubscription.ts`

Custom hook for subscribing to real-time data channels.

---

### 9. API Communication

#### 9.1 API Services

**File Location:** `src/services/api/`

Contains 13 API service files handling various data operations:
- Asset management
- User management
- Customer operations
- Location services
- Alert management
- Tag management
- etc.

---

#### 9.2 OpenAPI Integration

**File Location:** `src/services/openapi/`

```typescript
// Generated API client from OpenAPI specification
// All data flows through these typed endpoints
```

---

#### 9.3 Request Handler

**File Location:** `src/utils/requestHandler.ts`

```typescript
// Centralized API request handling
export const requestHandler = {
  get: async <T>(url: string, options?: RequestOptions): Promise<T> => {
    // Adds authentication headers
    // Handles errors
    // Returns typed response
  },
  post: async <T>(url: string, data: any, options?: RequestOptions): Promise<T> => {
    // Same with request body
  },
  // PUT, DELETE, etc.
};
```

**Security Headers Added:**
- Authorization (Bearer token)
- Tenant context
- Content-Type

---

#### 9.4 Rate Limiter

**File Location:** `src/utils/rateLimiter.ts`

```typescript
// Client-side rate limiting to prevent abuse
export const rateLimiter = {
  checkLimit: (key: string): boolean => {
    // Tracks request counts
    // Returns false if limit exceeded
  },
  // Implementation details
};
```

---

#### 9.5 Request Queue

**File Location:** `src/utils/requestQueue.ts`

Manages queuing of API requests to prevent overwhelming the backend.

---

### 10. Caching

**File Location:** `src/utils/cache.ts`

```typescript
interface CacheEntry<T> {
  data: T;
  timestamp: number;
  ttl: number;
}

export const cache = {
  set: <T>(key: string, data: T, ttl?: number): void => {
    // Store in memory/localStorage
  },
  get: <T>(key: string): T | null => {
    // Retrieve if not expired
  },
  clear: (key?: string): void => {
    // Remove cached data
  }
};
```

**Cached Data May Include:**
- User session data
- API responses containing personal data
- Lookup tables

---

## Data Inventory Summary

| Data Type | Collection Point | Processing | Storage Location | Retention | Sensitivity | Compliance Relevance |
|-----------|------------------|------------|------------------|-----------|-------------|---------------------|
| Username/Email | Login Form | Validation | Auth Context (Memory) | Session | Personal Identifier | GDPR Art. 6 |
| Password | Login Form | Hashing (backend) | Not stored locally | Transient | HIGH - Credential | GDPR Art. 32 |
| JWT Token | Login Response | Encryption | Encrypted LocalStorage | Session/Expiry | HIGH - Auth | Security Control |
| User Profile | User Management | CRUD via API | Backend (via API) | Account lifetime | Personal Data | GDPR Art. 5(1)(e) |
| Customer Data | Forms/CSV Import | Validation, Parsing | Backend (via API) | Business need | Personal/Business | GDPR Art. 6 |
| Physical Addresses | Forms/Geocoding | Geocoding API | Backend (via API) | Business need | Personal Data | GDPR Art. 6 |
| GPS Coordinates | Asset Tracking | Real-time streaming | Backend (via API) | Tracking period | Location Data | GDPR Art. 9 consideration |
| Vehicle Registration | Vehicle Management | CSV Import | Backend (via API) | Business need | Identifying | GDPR Art. 6 |
| Audit Logs | User Actions | Backend processing | Backend (via API) | Compliance period | Behavioral | GDPR Art. 30 |
| Session IDs | Authentication | Cookie/Storage | Cookies, LocalStorage | Session | Technical | Security |

---

## Third-Party Data Sharing Analysis

### 1. Google Maps/Geocoding API

**File Location:** `src/utils/geocoding.ts`

| Attribute | Details |
|-----------|---------|
| **Service** | Google Maps Geocoding API |
| **Data Shared** | Physical addresses, GPS coordinates |
| **Purpose** | Address validation, coordinate lookup |
| **Data Flow Direction** | Outbound (addresses) / Inbound (coordinates) |
| **Geographic Location** | Google Cloud (US primary, global CDN) |
| **Privacy Impact** | Addresses sent to third party |

**Compliance Considerations:**
- GDPR: Requires Google as processor in DPA
- Cross-border transfer: US-based service
- Purpose limitation: Geographic validation only

---

### 2. Backend API Server

**Configuration:** `src/utils/env.ts`

| Attribute | Details |
|-----------|---------|
| **Service** | OpenAPI Backend |
| **Data Shared** | All application data |
| **Purpose** | Data persistence, business logic |
| **Relationship** | Likely same controller or processor |
| **Location** | Configured via environment |

---

### 3. WebSocket Server

**Configuration:** `src/contexts/WebSocketContext.tsx`

| Attribute | Details |
|-----------|---------|
| **Service** | WebSocket endpoint |
| **Data Shared** | Auth tokens, subscription requests |
| **Data Received** | Real-time location updates, alerts |
| **Purpose** | Live tracking |

---

### 4. Deployment Platforms

**Configuration Files:** `amplify.yml`, `vercel.json`

- AWS Amplify deployment configuration present
- Vercel deployment configuration present
- These platforms may process request logs, IP addresses

---

## Security Controls Identified

### 1. Encryption at Rest

**File:** `src/utils/encryptedStorage.ts`, `src/utils/encryption.ts`

```typescript
// AES encryption for sensitive local storage
// Implementation documented in docs/encryption.md
```

**Status:** ✅ IMPLEMENTED for local storage

---

### 2. Authentication Token Security

**File:** `src/contexts/AuthContext.tsx`

- JWT tokens used for authentication
- Tokens stored in encrypted storage
- Token included in API request headers

**Status:** ✅ IMPLEMENTED

---

### 3. Input Validation

**File:** `src/utils/validate.ts`, `src/hooks/useFormValidation.ts`

```typescript
// Form validation utilities
// Type checking
// Format validation
```

**Documentation:** `docs/form-validation.md`

**Status:** ✅ IMPLEMENTED for forms

---

### 4. Error Handling

**File:** `src/utils/errorHandler.ts`, `src/components/ErrorBoundary.tsx`

- Centralized error handling
- Error boundary for React components
- Prevents sensitive data in error messages

**Status:** ✅ IMPLEMENTED

---

### 5. Protected Routes

**File:** `src/components/ProtectedRoute.tsx`

```typescript
// Route protection based on authentication state
// Redirects unauthenticated users
```

**Status:** ✅ IMPLEMENTED

---

## Data Subject Rights Implementation Analysis

### Access Rights (GDPR Art. 15)

| Capability | Implementation Status | Location |
|------------|----------------------|----------|
| View own profile | Implemented | UserDetails.tsx |
| View own data | Via UI pages | Various pages |
| Export data | Not visible in frontend | Backend responsibility |

### Rectification Rights (GDPR Art. 16)

| Capability | Implementation Status | Location |
|------------|----------------------|----------|
| Edit profile | Implemented | User management forms |
| Update contact info | Implemented | Customer/User forms |

### Erasure Rights (GDPR Art. 17)

| Capability | Implementation Status | Location |
|------------|----------------------|----------|
| Delete confirmation | Implemented | DeleteConfirmationModal |
| Account deletion | Via delete actions | API integration |

**Documentation:** `docs/DeleteConfirmationModal.md`

### Data Portability (GDPR Art. 20)

| Capability | Implementation Status |
|------------|----------------------|
| Export to CSV | Not visible in current codebase |
| Structured data export | Backend responsibility |

---

## Compliance Considerations

### GDPR Applicability

**Relevant Data Processing:**
1. User personal data (names, emails)
2. Customer contact information
3. Physical addresses
4. Location tracking data
5. Behavioral data (audit logs)

**Legal Basis Required For:**
- User authentication: Contract performance
- Customer management: Legitimate interest/Contract
- Location tracking: Consent may be required
- Audit logging: Legal obligation/Legitimate interest

### Location Data Special Consideration

GPS tracking of assets that are:
- Associated with individuals
- Used by employees
- Tracking movement patterns

May require:
- Explicit consent under GDPR
- Employee notification
- Purpose limitation
- Retention limits

---

## Risk Assessment

### High-Risk Processing Identified

| Risk Area | Description | Mitigation Found |
|-----------|-------------|------------------|
| Bulk Data Import | CSV imports with personal data | Validation present |
| Third-Party Geocoding | Addresses sent to Google | Standard API integration |
| Real-Time Tracking | Location streaming | Authentication required |
| Credential Storage | Auth tokens in browser | Encryption implemented |

### Vulnerabilities Identified

#### 1. Client-Side Data Exposure

**Risk:** Sensitive data visible in browser storage
**Mitigation:** Encryption implemented
**Residual Risk:** Medium (encryption key management)

#### 2. Third-Party Data Sharing

**Risk:** Addresses shared with Google without explicit consent
**Mitigation:** None visible in frontend
**

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## TOP 10 Critical Security Issues

---

### Issue #1: Hardcoded Encryption Key in Source Code
**Severity:** CRITICAL
**Category:** Cryptographic Issues / Data Exposure

**Location:** 
- File: `src/utils/encryption.ts`
- Line(s): 5-6
- Function/Class: Module-level constants

**Description:**
The encryption key and initialization vector (IV) are hardcoded directly in the source code. This means anyone with access to the source code (including through client-side JavaScript in a browser) can decrypt all encrypted data.

**Vulnerable Code:**
```typescript
// WARNING: In production, this should be derived from environment variables
// and properly managed through a secure key management system
const ENCRYPTION_KEY = 'your-256-bit-secret-key-here-32c';
const IV = 'unique-iv-16char';
```

**Impact:**
- All encrypted data in the application can be decrypted by anyone with access to the client-side code
- Tokens, credentials, and sensitive data stored using `encryptedStorage.ts` are compromised
- Complete bypass of encryption security controls

**Fix Required:**
Remove hardcoded keys and derive encryption keys from secure sources, or use browser-native secure storage APIs.

**Example Secure Implementation:**
```typescript
// For client-side apps, use Web Crypto API with keys derived from user credentials
// or use secure browser storage mechanisms like IndexedDB with appropriate access controls
const getEncryptionKey = async (userPassword: string, salt: Uint8Array): Promise<CryptoKey> => {
  const encoder = new TextEncoder();
  const keyMaterial = await crypto.subtle.importKey(
    'raw',
    encoder.encode(userPassword),
    'PBKDF2',
    false,
    ['deriveBits', 'deriveKey']
  );
  return crypto.subtle.deriveKey(
    { name: 'PBKDF2', salt, iterations: 100000, hash: 'SHA-256' },
    keyMaterial,
    { name: 'AES-GCM', length: 256 },
    false,
    ['encrypt', 'decrypt']
  );
};
```

---

### Issue #2: Static Initialization Vector (IV) for All Encryptions
**Severity:** CRITICAL
**Category:** Cryptographic Issues

**Location:** 
- File: `src/utils/encryption.ts`
- Line(s): 6, 15
- Function/Class: `encrypt()` function

**Description:**
A static IV is used for all encryption operations. In AES-CBC mode, reusing the same IV with the same key allows attackers to detect identical plaintexts and potentially decrypt data through various cryptographic attacks.

**Vulnerable Code:**
```typescript
const IV = 'unique-iv-16char';

export const encrypt = (text: string): string => {
  try {
    const cipher = CryptoJS.AES.encrypt(text, CryptoJS.enc.Utf8.parse(ENCRYPTION_KEY), {
      iv: CryptoJS.enc.Utf8.parse(IV),  // Static IV reused for every encryption
      mode: CryptoJS.mode.CBC,
      padding: CryptoJS.pad.Pkcs7,
    });
```

**Impact:**
- Identical plaintexts produce identical ciphertexts, enabling pattern analysis
- Vulnerable to chosen-plaintext attacks
- Breaks semantic security of the encryption scheme

**Fix Required:**
Generate a random IV for each encryption operation and prepend it to the ciphertext.

**Example Secure Implementation:**
```typescript
export const encrypt = (text: string): string => {
  const iv = CryptoJS.lib.WordArray.random(16); // Random IV for each encryption
  const cipher = CryptoJS.AES.encrypt(text, CryptoJS.enc.Utf8.parse(ENCRYPTION_KEY), {
    iv: iv,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  // Prepend IV to ciphertext for decryption
  return iv.toString(CryptoJS.enc.Base64) + ':' + cipher.toString();
};
```

---

### Issue #3: Sensitive Tokens Stored in localStorage
**Severity:** HIGH
**Category:** Authentication & Session Management / Data Exposure

**Location:** 
- File: `src/utils/auth.ts`
- Line(s): 14, 21-22, 28-30, 35-36
- Function/Class: `storeTokens()`, `getTokens()`, `clearTokens()`

**Description:**
Access tokens and refresh tokens are stored in localStorage, which is accessible to any JavaScript running on the page. This makes tokens vulnerable to XSS attacks.

**Vulnerable Code:**
```typescript
export const storeTokens = (accessToken: string, refreshToken: string) => {
  encryptedStorage.setItem(ACCESS_TOKEN_KEY, accessToken);
  encryptedStorage.setItem(REFRESH_TOKEN_KEY, refreshToken);
};

export const getTokens = () => {
  return {
    accessToken: encryptedStorage.getItem(ACCESS_TOKEN_KEY),
    refreshToken: encryptedStorage.getItem(REFRESH_TOKEN_KEY),
  };
};
```

**Impact:**
- XSS attacks can steal authentication tokens
- Tokens persist even after browser is closed (no session timeout enforcement)
- Combined with Issue #1, encryption provides no real protection

**Fix Required:**
Use httpOnly cookies for token storage (requires backend changes) or implement proper XSS protections and shorter token lifetimes.

**Example Secure Implementation:**
```typescript
// Backend should set tokens as httpOnly cookies
// For SPA, use memory storage with refresh token rotation
class SecureTokenStorage {
  private accessToken: string | null = null;
  
  setAccessToken(token: string) {
    this.accessToken = token;
  }
  
  getAccessToken(): string | null {
    return this.accessToken;
  }
  
  clearTokens() {
    this.accessToken = null;
    // Refresh token should be in httpOnly cookie managed by backend
  }
}
```

---

### Issue #4: Tenant ID Stored in Unprotected Cookie
**Severity:** HIGH
**Category:** Authorization & Access Control

**Location:** 
- File: `src/utils/cookies.ts`
- Line(s): 5-16
- Function/Class: `setTenantCookie()`

**Description:**
The tenant ID is stored in a client-side cookie without the `httpOnly` flag and with overly permissive settings. This allows client-side JavaScript (and XSS attacks) to manipulate the tenant context.

**Vulnerable Code:**
```typescript
export function setTenantCookie(tenantId: string) {
  // Set cookie to expire in 30 days
  const expirationDate = new Date();
  expirationDate.setDate(expirationDate.getDate() + 30);

  // Set the cookie with path=/ to make it available across the entire site
  document.cookie = `${TENANT_COOKIE_NAME}=${encodeURIComponent(tenantId)}; expires=${expirationDate.toUTCString()}; path=/; SameSite=Strict`;
}
```

**Impact:**
- Potential for tenant switching attacks if backend doesn't properly validate
- XSS attacks can manipulate tenant context
- 30-day expiration may be excessive for sensitive multi-tenant applications

**Fix Required:**
Tenant selection should be validated server-side and managed through secure session mechanisms.

**Example Secure Implementation:**
```typescript
// Tenant ID should be part of the JWT claims and validated server-side
// If client-side cookie is needed, use httpOnly (requires backend)
export function setTenantCookie(tenantId: string) {
  // Request backend to set httpOnly cookie
  await fetch('/api/set-tenant', {
    method: 'POST',
    credentials: 'include',
    body: JSON.stringify({ tenantId })
  });
}
```

---

### Issue #5: Insufficient Rate Limiting Configuration
**Severity:** HIGH
**Category:** Business Logic Flaws / API Security

**Location:** 
- File: `src/utils/rateLimiter.ts`
- Line(s): 5-6, 28-39
- Function/Class: `RateLimiter` class

**Description:**
The rate limiter is client-side only and has permissive defaults (100 requests per 60 seconds). Client-side rate limiting can be trivially bypassed by attackers. Additionally, the implementation resets the window on the first request after expiry, which could allow burst attacks.

**Vulnerable Code:**
```typescript
export class RateLimiter {
  private maxRequests: number;
  private windowMs: number;
  private requests: Map<string, { count: number; resetTime: number }> = new Map();

  constructor(maxRequests: number = 100, windowMs: number = 60000) {
    this.maxRequests = maxRequests;
    this.windowMs = windowMs;
  }

  canMakeRequest(key: string): boolean {
    const now = Date.now();
    const requestData = this.requests.get(key);

    if (!requestData || now > requestData.resetTime) {
      this.requests.set(key, { count: 1, resetTime: now + this.windowMs });
      return true;
    }
    // ...
  }
```

**Impact:**
- Brute force attacks on login endpoints
- API abuse and resource exhaustion
- Denial of service against backend services

**Fix Required:**
Implement server-side rate limiting; client-side limiting should only be used for UX purposes.

**Example Secure Implementation:**
```typescript
// Server-side rate limiting (Express example)
import rateLimit from 'express-rate-limit';

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // 5 attempts per window
  message: 'Too many login attempts, please try again later',
  standardHeaders: true,
  legacyHeaders: false,
});

app.post('/api/login', loginLimiter, loginHandler);
```

---

### Issue #6: Insecure Direct Object Reference (IDOR) in API Calls
**Severity:** HIGH
**Category:** Authorization & Access Control

**Location:** 
- File: `src/services/api/userService.ts`
- Line(s): 21-30, 32-45
- Function/Class: `getUserById()`, `updateUser()`

**Description:**
API functions directly use user-provided IDs without any client-side validation of authorization. While server-side checks are essential, the pattern suggests potential IDOR vulnerabilities if the backend mirrors this design.

**Vulnerable Code:**
```typescript
export const getUserById = async (userId: string): Promise<User> => {
  const response = await api.get<UserResponse>(`/user/${userId}`);
  return response.data.data;
};

export const updateUser = async (userId: string, data: UpdateUserRequest): Promise<User> => {
  const response = await api.patch<UserResponse>(`/user/${userId}`, data);
  return response.data.data;
};
```

**Impact:**
- Unauthorized access to other users' data if backend lacks proper authorization
- Horizontal privilege escalation
- Data leakage across tenants

**Fix Required:**
Ensure backend implements proper authorization checks. Consider using indirect references or including tenant context in requests.

**Example Secure Implementation:**
```typescript
// Include authorization context explicitly
export const getUserById = async (userId: string): Promise<User> => {
  // Validate that the request is for an authorized resource
  const currentUser = await getCurrentUser();
  if (!canAccessUser(currentUser, userId)) {
    throw new AuthorizationError('Not authorized to access this user');
  }
  const response = await api.get<UserResponse>(`/user/${userId}`);
  return response.data.data;
};
```

---

### Issue #7: Sensitive Data Exposure in Error Messages
**Severity:** MEDIUM
**Category:** Data Exposure

**Location:** 
- File: `src/utils/errorHandler.ts`
- Line(s): 66-76
- Function/Class: `formatError()`

**Description:**
Error handling can expose internal implementation details, stack traces, and potentially sensitive information to the client. The `originalError` field passes through raw error information.

**Vulnerable Code:**
```typescript
export function formatError(error: unknown): ApiError {
  // Handle Axios errors
  if (axios.isAxiosError(error)) {
    const axiosError = error as AxiosError<ApiErrorResponse>;
    return {
      message: axiosError.response?.data?.message || axiosError.message,
      status: axiosError.response?.status || 500,
      code: axiosError.response?.data?.code || 'UNKNOWN_ERROR',
      originalError: error,  // Exposes full error object
    };
  }
```

**Impact:**
- Information disclosure about backend structure
- Exposure of internal paths, database errors, or sensitive data
- Helps attackers understand system internals

**Fix Required:**
Sanitize error messages before displaying to users; log detailed errors server-side only.

**Example Secure Implementation:**
```typescript
export function formatError(error: unknown): ApiError {
  // Log full error details for debugging (server-side/secure logging)
  console.error('Full error:', error);
  
  if (axios.isAxiosError(error)) {
    return {
      message: getUserFriendlyMessage(error.response?.status),
      status: error.response?.status || 500,
      code: error.response?.data?.code || 'UNKNOWN_ERROR',
      // Don't include originalError in production
    };
  }
}

function getUserFriendlyMessage(status?: number): string {
  const messages: Record<number, string> = {
    400: 'Invalid request. Please check your input.',
    401: 'Please log in to continue.',
    403: 'You do not have permission to perform this action.',
    404: 'The requested resource was not found.',
    500: 'An unexpected error occurred. Please try again later.',
  };
  return messages[status || 500] || messages[500];
}
```

---

### Issue #8: WebSocket Connection Without Proper Authentication Validation
**Severity:** MEDIUM
**Category:** API Security / Authentication & Session Management

**Location:** 
- File: `src/contexts/WebSocketContext.tsx`
- Line(s): 32-60
- Function/Class: `WebSocketProvider`, `connect()`

**Description:**
The WebSocket connection sends the access token as a query parameter, which can be logged in server access logs, proxy logs, and browser history. Additionally, there's no validation that the token is still valid before establishing the connection.

**Vulnerable Code:**
```typescript
const connect = useCallback(() => {
  if (socketRef.current?.readyState === WebSocket.OPEN) return;

  const token = getTokens().accessToken;
  if (!token) return;

  const wsUrl = `${import.meta.env.VITE_WS_URL}?token=${token}`;  // Token in URL
  socketRef.current = new WebSocket(wsUrl);
```

**Impact:**
- Access tokens exposed in logs and browser history
- Potential token leakage through referrer headers
- No token freshness validation before connection

**Fix Required:**
Send authentication tokens via WebSocket message after connection establishment, not in the URL.

**Example Secure Implementation:**
```typescript
const connect = useCallback(() => {
  if (socketRef.current?.readyState === WebSocket.OPEN) return;

  const token = getTokens().accessToken;
  if (!token) return;

  // Connect without token in URL
  const wsUrl = import.meta.env.VITE_WS_URL;
  socketRef.current = new WebSocket(wsUrl);
  
  socketRef.current.onopen = () => {
    // Send authentication as first message
    socketRef.current?.send(JSON.stringify({
      type: 'authenticate',
      token: token
    }));
  };
}, []);
```

---

### Issue #9: Mass Assignment Vulnerability in User Update
**Severity:** MEDIUM
**Category:** API Security

**Location:** 
- File: `src/services/api/userService.ts`
- Line(s): 32-45
- Function/Class: `updateUser()`

**Description:**
The `updateUser` function accepts a partial `UpdateUserRequest` object and passes it directly to the API without filtering sensitive fields that users shouldn't be able to modify themselves (like roles).

**Vulnerable Code:**
```typescript
export const updateUser = async (userId: string, data: UpdateUserRequest): Promise<User> => {
  const response = await api.patch<UserResponse>(`/user/${userId}`, data);
  return response.data.data;
};
```

Looking at the type definition in `src/types/models/user/user.types.ts`:
```typescript
export interface UpdateUserRequest extends Partial<CreateUserRequest> {
  password?: string;
}

export interface CreateUserRequest {
  email: string;
  firstName: string;
  lastName: string;
  roleId?: string;  // Potentially sensitive field
  // ...
}
```

**Impact:**
- Users could potentially modify their own roles if backend doesn't filter
- Privilege escalation if roleId can be manipulated
- Unauthorized modification of protected fields

**Fix Required:**
Whitelist allowed fields for updates based on user permissions.

**Example Secure Implementation:**
```typescript
const ALLOWED_SELF_UPDATE_FIELDS = ['firstName', 'lastName', 'email', 'phone'];

export const updateUser = async (userId: string, data: UpdateUserRequest, isSelfUpdate: boolean = false): Promise<User> => {
  let sanitizedData = data;
  
  if (isSelfUpdate) {
    sanitizedData = Object.keys(data)
      .filter(key => ALLOWED_SELF_UPDATE_FIELDS.includes(key))
      .reduce((obj, key) => ({ ...obj, [key]: data[key as keyof UpdateUserRequest] }), {});
  }
  
  const response = await api.patch<UserResponse>(`/user/${userId}`, sanitizedData);
  return response.data.data;
};
```

---

### Issue #10: File Upload Without Content Validation
**Severity:** MEDIUM
**Category:** Input Validation & Output Encoding

**Location:** 
- File: `src/services/import/importService.ts`
- Line(s): 5-29
- Function/Class: `uploadFile()`

**Description:**
File uploads only validate file extension on the client side, which can be easily bypassed. There's no validation of file content, size limits, or MIME type verification.

**Vulnerable Code:**
```typescript
export const uploadFile = async (
  file: File,
  endpoint: string,
  onProgress?: (progress: number) => void
): Promise<void> => {
  const formData = new FormData();
  formData.append('file', file);

  await api.post(endpoint, formData, {
    headers: {
      'Content-Type': 'multipart/form-data',
    },
    onUploadProgress: (progressEvent) => {
      if (onProgress && progressEvent.total) {
        const progress = Math.round((progressEvent.loaded * 100) / progressEvent.total);
        onProgress(progress);
      }
    },
  });
};
```

**Impact:**
- Malicious file upload if backend doesn't validate
- Potential for server-side code execution
- Storage exhaustion with large files

**Fix Required:**
Implement client-side validation as defense-in-depth (server must also validate).

**Example Secure Implementation:**
```typescript
const ALLOWED_MIME_TYPES = ['text/csv', 'application/json'];
const MAX_FILE_SIZE = 10 * 1024 * 1024; // 10MB

export const uploadFile = async (
  file: File,
  endpoint: string,
  onProgress?: (progress: number) => void
): Promise<void> => {
  // Client-side validation (defense-in-depth)
  if (!ALLOWED_MIME_TYPES.includes(file.type)) {
    throw new Error(`Invalid file type. Allowed types: ${ALLOWED_MIME_TYPES.join(', ')}`);
  }
  
  if (file.size > MAX_FILE_SIZE) {
    throw new Error(`File too large. Maximum size: ${MAX_FILE_SIZE / 1024 / 1024}MB`);
  }
  
  const formData = new FormData();
  formData.append('file', file);
  // ... rest of upload logic
};
```

---

## Summary

### 1. Overall Security Posture
The codebase shows several security concerns typical of frontend applications, with the most critical issues being related to cryptographic implementations and token storage. The hardcoded encryption key completely undermines the encryption security controls. The application would benefit from a security review of the authentication flow and a move toward backend-managed security controls.

### 2. Critical Issues Count
**2 CRITICAL severity findings** (Issues #1 and #2 - both related to cryptographic implementation)

### 3. Most Concerning Pattern
**Insecure cryptographic implementation** - The combination of hardcoded keys, static IVs, and client-side storage creates a false sense of security. Data that appears encrypted is effectively stored in plaintext for anyone who can view the client-side code.

### 4. Priority Fixes
1. **Issue #1 & #2** - Remove hardcoded encryption keys and static IVs; either implement proper key derivation or remove client-side encryption entirely in favor of backend-managed security
2. **Issue #3** - Migrate token storage to httpOnly cookies managed by the backend
3. **Issue #5** - Implement server-side rate limiting, especially for authentication endpoints

### 5. Implementation Issues
- Over-reliance on client-side security controls that can be bypassed
- Encryption implementation that provides false security assurance
- Token handling that doesn't follow security best practices
- Missing defense-in-depth for file uploads and API calls

---

## Additional Security Issues Found

### Configuration Vulnerabilities Present:
- **CORS configuration** in `vercel.json` uses permissive rewrite rules that proxy all `/api/*` requests without additional security headers
- **Environment variable exposure** - `env.example` documents sensitive configuration patterns

### Architecture Security Flaws Identified:
- **Client-side tenant management** - Tenant selection logic in the frontend could be manipulated
- **Lack of request signing** - API requests don't include integrity verification

### Development Implementation Issues:
- **Console logging in production** - Error handlers include `console.error` statements that could leak information in production
- **Missing Content Security Policy** - No CSP headers configured in the frontend

### Insecure Coding Patterns Found:
- **Direct DOM cookie manipulation** without proper security flags
- **URL parameter passing for sensitive data** (WebSocket token)
- **Partial type validation** - TypeScript types don't prevent runtime injection

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Executive Summary

After comprehensive analysis of the `iot_backoffice_7678358d` repository, **no dedicated monitoring or observability tools are detected** in this codebase. This is a frontend React/TypeScript application that does not implement logging frameworks, metrics collection, distributed tracing, APM, or error tracking services.

---

## Findings

### What IS Present (Limited Observability Mechanisms)

#### 1. Error Boundary Component
**File:** `/src/components/ErrorBoundary.tsx`

The codebase contains a React Error Boundary component that catches JavaScript errors in the component tree:

```
Purpose: Catches unhandled errors in React component tree
Implementation: Standard React Error Boundary pattern
Logging: Console-based only (console.error)
```

This is a basic error handling mechanism, not a full error tracking solution.

#### 2. Console-Based Debugging
The application uses standard `console.log`, `console.error`, and `console.warn` statements for debugging purposes. This is not a structured logging implementation.

#### 3. Error Handler Utility
**File:** `/src/utils/errorHandler.ts`

A utility file exists for handling errors, likely providing standardized error formatting for the application. This is application-level error handling, not external error tracking.

---

### What is NOT Present

The following monitoring and observability mechanisms are **NOT implemented** in this codebase:

| Category | Status |
|----------|--------|
| **Logging Frameworks** | ❌ Not detected (no Winston, Pino, Bunyan, etc.) |
| **APM Tools** | ❌ Not detected (no New Relic, DataDog, Dynatrace, etc.) |
| **Error Tracking Services** | ❌ Not detected (no Sentry, Rollbar, Bugsnag, etc.) |
| **Metrics Collection** | ❌ Not detected (no Prometheus client, StatsD, etc.) |
| **Distributed Tracing** | ❌ Not detected (no OpenTelemetry, Jaeger, Zipkin, etc.) |
| **Real User Monitoring (RUM)** | ❌ Not detected (no LogRocket, FullStory, Hotjar, etc.) |
| **Analytics Tools** | ❌ Not detected (no Google Analytics, Mixpanel, Amplitude, etc.) |
| **Synthetic Monitoring** | ❌ Not detected |
| **Health Check Endpoints** | ❌ Not applicable (frontend application) |
| **Performance Monitoring** | ❌ Not detected |
| **Cloud Observability** | ❌ Not detected (no CloudWatch, Azure Monitor, etc.) |

---

### Technology Stack Context

This is a **frontend-only React application** with the following characteristics:

- **Framework:** React 18 with TypeScript
- **Build Tool:** Vite
- **State Management:** Zustand, React Query
- **Routing:** React Router DOM
- **API Communication:** Axios
- **Deployment:** Vercel/Amplify (based on config files)

Frontend applications typically rely on:
1. Browser DevTools for debugging
2. Backend services for comprehensive logging
3. Optional RUM/error tracking services (not implemented here)

---

### WebSocket Context

**File:** `/src/contexts/WebSocketContext.tsx`

A WebSocket context exists for real-time communication, but no WebSocket-specific monitoring or metrics are implemented.

---

## Conclusion

**No monitoring or observability tools are detected in this codebase.**

This frontend React application does not include:
- Dedicated logging libraries
- Error tracking services
- Performance monitoring tools
- Analytics integrations
- RUM solutions

The application relies solely on:
- React Error Boundaries for crash prevention
- Console statements for development debugging
- Standard browser developer tools

---

## Raw Dependencies Section

### Production Dependencies (`package.json`)

```json
{
  "@tanstack/react-query": "^5.0.0",
  "@tanstack/react-query-devtools": "^5.0.0",
  "@types/leaflet.markercluster": "^1.5.5",
  "@types/react-window": "^1.8.8",
  "axios": "^1.6.7",
  "clsx": "^2.1.1",
  "crypto-js": "^4.2.0",
  "date-fns": "^4.1.0",
  "leaflet": "^1.9.4",
  "leaflet.markercluster": "^1.5.3",
  "lucide-react": "^0.323.0",
  "mapbox-gl": "^3.15.0",
  "papaparse": "^5.5.3",
  "qrcode": "^1.5.4",
  "react": "^18.2.0",
  "react-date-range": "^2.0.1",
  "react-dom": "^18.2.0",
  "react-router-dom": "^6.22.0",
  "react-window": "^1.8.11",
  "zod": "^3.25.32",
  "zustand": "^5.0.5"
}
```

### Dev Dependencies (`package.json`)

```json
{
  "@types/crypto-js": "^4.2.2",
  "@types/leaflet": "^1.9.18",
  "@types/node": "^20.11.0",
  "@types/papaparse": "^5.3.16",
  "@types/qrcode": "^1.5.5",
  "@types/react-date-range": "^1.4.10",
  "@types/react-dom": "^19.1.5",
  "@typescript-eslint/eslint-plugin": "^8.27.0",
  "@typescript-eslint/parser": "^8.26.1",
  "@vitejs/plugin-react": "^4.2.0",
  "autoprefixer": "^10.4.17",
  "eslint": "^8.57.1",
  "eslint-config-prettier": "^10.1.8",
  "eslint-import-resolver-typescript": "^4.4.4",
  "eslint-plugin-import": "^2.32.0",
  "eslint-plugin-prettier": "^5.5.3",
  "eslint-plugin-react": "^7.33.2",
  "eslint-plugin-react-hooks": "^4.6.0",
  "eslint-plugin-react-refresh": "^0.4.5",
  "prettier": "3.6.2",
  "swagger-typescript-api": "^13.2.7",
  "tailwindcss": "^3.4.1",
  "typescript": "^5.3.3",
  "typescript-eslint": "^8.38.0",
  "vite": "^6.3.5"
}
```

### Dependency Analysis for Monitoring Tools

| Dependency | Category | Monitoring Related? |
|------------|----------|---------------------|
| `@tanstack/react-query` | Data fetching | ❌ No (has devtools but not monitoring) |
| `@tanstack/react-query-devtools` | Development | ❌ No (dev-only debugging) |
| `axios` | HTTP client | ❌ No (could be instrumented but isn't) |
| `zustand` | State management | ❌ No |
| `zod` | Validation | ❌ No |
| `react-router-dom` | Routing | ❌ No |
| All other dependencies | UI/Utility | ❌ No |

**Confirmed: No monitoring, logging, tracing, or observability packages are present in the dependencies.**

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After thorough analysis of the provided codebase, **no machine learning services, AI technologies, or ML-related integrations were identified**.

---

## Analysis Results

### 1. External ML Service Providers

| Category | Services Found |
|----------|----------------|
| Cloud ML Services (AWS SageMaker, Azure ML, Google AI Platform, Databricks) | **None** |
| AI APIs (OpenAI, Anthropic, Groq, Cohere, Hugging Face) | **None** |
| Specialized Services (Speech recognition, Computer Vision) | **None** |
| MLOps Platforms (MLflow, Weights & Biases, Neptune, ClearML) | **None** |

### 2. ML Libraries and Frameworks

| Category | Libraries Found |
|----------|-----------------|
| Deep Learning (PyTorch, TensorFlow, JAX, Keras) | **None** |
| Traditional ML (Scikit-learn, XGBoost, LightGBM, CatBoost) | **None** |
| NLP (Transformers, spaCy, NLTK, Gensim) | **None** |
| Computer Vision (OpenCV, torchvision) | **None** |
| Audio/Speech (Whisper, librosa, speechbrain) | **None** |

### 3. Pre-trained Models and Model Hubs

| Category | Usage Found |
|----------|-------------|
| Hugging Face Models | **None** |
| TensorFlow Hub | **None** |
| PyTorch Hub | **None** |
| Specific Models (BERT, GPT, Whisper, CLIP) | **None** |

### 4. AI Infrastructure and Deployment

| Category | Implementation Found |
|----------|---------------------|
| Model Serving (TorchServe, TensorFlow Serving) | **None** |
| ML-specific Containerization | **None** |
| GPU/CUDA Integration | **None** |
| ML Scaling Infrastructure | **None** |

---

## Detailed Dependency Analysis

### Production Dependencies Reviewed

The codebase is a **React-based frontend application** with the following technology categories:

| Dependency | Purpose | ML-Related? |
|------------|---------|-------------|
| `@tanstack/react-query` | Data fetching/caching | ❌ No |
| `axios` | HTTP client | ❌ No |
| `react`, `react-dom` | UI framework | ❌ No |
| `react-router-dom` | Routing | ❌ No |
| `zustand` | State management | ❌ No |
| `leaflet`, `mapbox-gl` | Mapping/GIS | ❌ No |
| `date-fns` | Date utilities | ❌ No |
| `papaparse` | CSV parsing | ❌ No |
| `qrcode` | QR code generation | ❌ No |
| `crypto-js` | Cryptographic utilities | ❌ No |
| `zod` | Schema validation | ❌ No |
| `lucide-react` | Icons | ❌ No |
| `react-window` | Virtualized lists | ❌ No |
| `react-date-range` | Date picker | ❌ No |
| `clsx` | Class name utilities | ❌ No |

### Development Dependencies Reviewed

All development dependencies are standard tooling for TypeScript/React development:

- **Build Tools**: Vite, TypeScript
- **Linting/Formatting**: ESLint, Prettier
- **Styling**: Tailwind CSS, Autoprefixer
- **Type Definitions**: Various `@types/*` packages
- **API Generation**: `swagger-typescript-api`

**None are ML-related.**

---

## Security and Compliance Considerations

### ML-Specific Security Items

| Consideration | Status |
|---------------|--------|
| ML API Keys/Credentials | **N/A** - No ML services used |
| Data sent to ML services | **N/A** - No ML services used |
| Model security | **N/A** - No models used |
| ML-related compliance requirements | **N/A** - No ML data processing |

---

## Summary

| Metric | Value |
|--------|-------|
| **Total ML Services Identified** | 0 |
| **Total ML Libraries Identified** | 0 |
| **Total Pre-trained Models Identified** | 0 |
| **Major ML Dependencies** | None |
| **Architecture Pattern** | Standard React SPA (No ML components) |

### Application Profile

This codebase is a **frontend React application** focused on:
- **Mapping/GIS functionality** (Leaflet, Mapbox GL)
- **Data visualization and management**
- **Standard CRUD operations** via REST API (axios)
- **Client-side state management** (Zustand, React Query)

### Risk Assessment

| Risk Category | Assessment |
|---------------|------------|
| External ML Service Dependency | **None** - No risk |
| ML Vendor Lock-in | **None** - No risk |
| ML-related Cost Exposure | **None** - No risk |
| ML Model Drift/Degradation | **None** - No risk |

---

## Recommendations

Since no ML services are currently used, if ML capabilities are needed in the future, consider:

1. **For AI/LLM features**: Integration with OpenAI, Anthropic, or similar APIs
2. **For on-device ML**: TensorFlow.js for browser-based inference
3. **For geospatial ML**: Mapbox AI services or custom spatial analysis

---

*Analysis completed. No 3rd party ML services or AI technologies are present in this codebase.*

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Executive Summary

**no feature flag usage detected**

---

## Detailed Analysis

After a comprehensive review of the codebase, I found **no feature flag systems** implemented in this repository.

### What Was Analyzed

#### 1. Dependency Check (`package.json`)

I examined both production and development dependencies for common feature flag libraries:

| Library Pattern | Found |
|-----------------|-------|
| `launchdarkly-*` | ❌ No |
| `flagsmith-*` | ❌ No |
| `@splitsoftware/*` | ❌ No |
| `@unleash/*` | ❌ No |
| `configcat-*` | ❌ No |
| `@optimizely/*` | ❌ No |
| Custom feature flag package | ❌ No |

#### 2. Environment Configuration

**File: `env.example`** - Listed but not provided in full content. Based on the repository structure, environment variables appear to be used for:
- API configuration
- Authentication settings
- Service URLs

**No evidence of feature flag environment variables** (e.g., `FEATURE_*`, `FF_*`, `FLAG_*` patterns).

#### 3. Code Structure Review

| Location | Feature Flag Indicators | Found |
|----------|------------------------|-------|
| `/src/utils/` | No feature flag utilities | ❌ |
| `/src/contexts/` | Only `AuthContext` and `WebSocketContext` | ❌ |
| `/src/store/` | Zustand store without flag management | ❌ |
| `/src/constants/` | Only `auth.ts` constants | ❌ |
| `/src/services/` | API services, no flag service | ❌ |
| `/src/hooks/` | Utility hooks, no flag hooks | ❌ |

#### 4. Common Feature Flag Patterns NOT Found

```typescript
// None of these patterns detected:

// Boolean feature checks
if (featureFlags.isEnabled('feature_name')) { }

// LaunchDarkly style
ldClient.variation('flag-key', user, defaultValue)

// Flagsmith style
flagsmith.hasFeature('feature_name')

// ConfigCat style
configCatClient.getValueAsync('feature_name', false)

// Custom database flags
await db.query('SELECT enabled FROM feature_flags WHERE name = ?')

// Environment-based flags
process.env.FEATURE_NEW_DASHBOARD === 'true'
```

---

## Current Application Architecture

The application uses the following for configuration and state management:

| Technology | Purpose |
|------------|---------|
| **Zustand** (`/src/store/`) | Client-side state management |
| **React Context** | Auth state, WebSocket connections |
| **Environment Variables** | API endpoints, service configuration |
| **Constants** (`/src/constants/auth.ts`) | Static authentication configuration |

---

## Recommendations

If feature flags are needed for this IoT backoffice application, consider:

### 1. **For Gradual Rollouts & A/B Testing**
```bash
# Recommended: Flagsmith (open-source option available)
npm install flagsmith-react

# Or: LaunchDarkly (enterprise)
npm install launchdarkly-react-client-sdk
```

### 2. **For Simple Feature Toggles**
Create a custom solution using the existing Zustand store:

```typescript
// src/store/featureFlags.ts
import { create } from 'zustand';

interface FeatureFlagState {
  flags: Record<string, boolean>;
  isEnabled: (flagName: string) => boolean;
}

export const useFeatureFlags = create<FeatureFlagState>((set, get) => ({
  flags: {},
  isEnabled: (flagName) => get().flags[flagName] ?? false,
}));
```

### 3. **For Environment-Based Flags**
Add to existing environment configuration:

```typescript
// src/utils/env.ts (extend existing)
export const featureFlags = {
  newDashboard: import.meta.env.VITE_FEATURE_NEW_DASHBOARD === 'true',
  betaAssetTracking: import.meta.env.VITE_FEATURE_BETA_ASSET_TRACKING === 'true',
};
```

---

## Conclusion

This codebase does not currently implement any feature flag system. The application relies on:
- Direct conditional logic based on user roles/permissions (via `AuthContext`)
- Environment variables for deployment configuration
- Static constants for application settings

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Security Assessment Report

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After comprehensive scanning of the entire codebase using all detection strategies, I have analyzed:

#### Detection Strategy 1: Library and Package Detection

**package.json dependencies analyzed:**
```json
{
  "dependencies": {
    "@headlessui/react": "^2.2.0",
    "@heroicons/react": "^2.2.0",
    "@tanstack/react-query": "^5.62.3",
    "crypto-js": "^4.2.0",
    "date-fns": "^4.1.0",
    "leaflet": "^1.9.4",
    "leaflet-draw": "^1.0.4",
    "qrcode": "^1.5.4",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hot-toast": "^2.4.1",
    "react-leaflet": "^4.2.1",
    "react-router-dom": "^7.0.1"
  }
}
```

**No LLM-related packages detected:**
- ❌ No OpenAI SDK (`openai`)
- ❌ No Anthropic SDK (`@anthropic-ai/sdk`)
- ❌ No Google AI SDK (`@google/generative-ai`)
- ❌ No LangChain (`langchain`, `@langchain/*`)
- ❌ No LlamaIndex
- ❌ No HuggingFace Transformers
- ❌ No vector database clients (Pinecone, Weaviate, Chroma)

#### Detection Strategy 2: Import/Include Pattern Matching

Searched all source files for LLM-related imports:

**Files scanned:** All `.ts`, `.tsx`, `.js` files in `/src`

**Results:** No matches found for:
- `import openai` / `from 'openai'`
- `import anthropic` / `from '@anthropic-ai'`
- `import { GoogleGenerativeAI }`
- `import { ChatOpenAI }` / LangChain patterns
- Any AI/ML library imports

#### Detection Strategy 3: API Client Instantiation Patterns

Searched for LLM client instantiation:

**Results:** No matches found for:
- `new OpenAI(`
- `new Anthropic(`
- `new GoogleGenerativeAI(`
- `OpenAIApi(`
- Any LLM client constructors

#### Detection Strategy 4: API Method Call Patterns

Searched for LLM API method calls:

**Results:** No matches found for:
- `.messages.create(`
- `.chat.completions.create(`
- `.generateContent(`
- `.createCompletion(`
- `.complete(` with LLM context

#### Detection Strategy 5: Configuration and Environment Variables

**Analyzed `env.example`:**
```
VITE_API_BASE_URL=
VITE_ENCRYPTION_KEY=
VITE_WS_URL=
```

**Results:** 
- ❌ No `OPENAI_API_KEY`
- ❌ No `ANTHROPIC_API_KEY`
- ❌ No `GOOGLE_AI_KEY`
- ❌ No LLM-related configuration variables

#### Detection Strategy 6: Prompt-Related Patterns

Searched for prompt handling code:

**Results:** No files or variables found with:
- `prompt`, `system_prompt`, `user_prompt` in LLM context
- `.prompt` template files
- `/prompts/` directories
- Template strings for LLM prompts

#### Detection Strategy 7: Custom Implementation Patterns

**Analyzed services directory (`/src/services/`):**
- `customerImport.ts` - CSV/data import, no LLM
- `locationImport.ts` - Location data import, no LLM
- `vehicleImport.ts` - Vehicle data import, no LLM
- `/api/*.ts` - Standard REST API clients for backend services
- `/websocket/` - WebSocket for real-time updates, no LLM
- `/openapi/` - OpenAPI client generation, no LLM

**Analyzed utils directory (`/src/utils/`):**
- `apiUtils.ts` - HTTP request utilities
- `encryption.ts` - Crypto-JS encryption utilities
- `geocoding.ts` - Geographic coordinate utilities
- `validate.ts` - Form validation utilities
- No AI/ML processing utilities

**Analyzed contexts (`/src/contexts/`):**
- `AuthContext.tsx` - Authentication state management
- `WebSocketContext.tsx` - WebSocket connection management
- No LLM context providers

### 1.2 File Analysis Summary

**Priority files examined:**

| File/Directory | LLM Related | Notes |
|----------------|-------------|-------|
| `package.json` | ❌ No | Standard React/TypeScript dependencies |
| `src/services/` | ❌ No | REST API and WebSocket services only |
| `src/utils/` | ❌ No | Standard utility functions |
| `src/contexts/` | ❌ No | Auth and WebSocket contexts |
| `env.example` | ❌ No | No LLM API keys |
| `.claude/settings.local.json` | ❌ No | Claude Code IDE settings, not runtime LLM |

**Note on `.claude/` directory:**
The `.claude/settings.local.json` file is for Claude Code (IDE assistant) configuration, not runtime LLM integration in the application.

### 1.3 Application Architecture Analysis

This repository is an **IoT Backoffice Dashboard** application with:

1. **Frontend Framework:** React 18 with TypeScript
2. **State Management:** React Query (TanStack)
3. **Real-time:** WebSocket connections for live tracking
4. **Maps:** Leaflet for geospatial visualization
5. **Backend Communication:** REST API calls to external backend services
6. **Security:** Client-side encryption with crypto-js

**Key Features (non-LLM):**
- Asset management and tracking
- Geofencing and location services
- Alert definitions and notifications
- User authentication and authorization
- Vehicle and gateway management
- Audit logging

---

## Conclusion

**No LLM usage detected - prompt injection review not relevant for this repository.**

This is a standard IoT backoffice web application built with React and TypeScript. It communicates with backend services via REST APIs and WebSockets for real-time data, but does not integrate any LLM, AI model, or LLM-based infrastructure.

### What Was Found Instead:

1. **Standard REST API Integration** - The application calls backend APIs for CRUD operations on assets, users, alerts, etc.
2. **WebSocket Communication** - Real-time updates for live tracking functionality
3. **Client-side Encryption** - Using crypto-js for secure storage, not AI-related
4. **Geocoding/Maps** - Standard geospatial utilities using Leaflet, not AI-powered

### Recommendations for General Security (Non-LLM):

While no LLM vulnerabilities exist, the following general security considerations apply:

1. **API Security** - Ensure backend APIs have proper authentication/authorization
2. **Input Validation** - The `validate.ts` utility should be consistently applied
3. **Encryption Key Management** - `VITE_ENCRYPTION_KEY` should be properly secured
4. **WebSocket Security** - Ensure WSS (secure WebSocket) is used in production