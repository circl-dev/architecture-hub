# hl_overview

High level overview of the codebase

# Project Analysis: WhatsApp Booking Engine Backoffice

## 0. Repository Name
[[WhatsApp-booking-engine-backoffice_3f88ae53]]

---

## 1. Project Purpose

This is a **backoffice/admin dashboard** for a WhatsApp-based booking and communication engine. The primary domain appears to be **customer engagement and automated communication management**, specifically:

- **Multi-tenant SaaS platform** for managing WhatsApp conversations and voice calls
- **AI-powered agent/chatbot management** with LLM integrations
- **Survey and review management** (batch surveys, review agents)
- **Voice testing and call monitoring** capabilities
- **Customer relationship management** (CRM-like features: customer forms, bulk imports, pricing)
- **Order Management System (OMS)** integration
- **Prompt engineering and analytics** for AI conversations
- **Message delivery tracking** and failed message handling

The system appears designed for businesses to manage automated WhatsApp conversations with customers, likely for bookings, surveys, and customer service.

---

## 2. Architecture Pattern

**Single Page Application (SPA)** with:
- **Component-based architecture** (React)
- **Feature-based organization** with some layer separation
- **Context-based state management** (AuthContext)
- **API client abstraction pattern**

---

## 3. Technology Stack

### Primary Language
- **TypeScript** (React frontend)

### Framework
- **React 18+** (SPA framework)
- **Vite** (Build tool and dev server)

### Styling
- **Tailwind CSS** (Utility-first CSS framework)
- **PostCSS** (CSS processing)

### Major Libraries/Dependencies (inferred from structure)
| Category | Technology |
|----------|------------|
| Routing | React Router (implied by page structure) |
| State Management | React Context API |
| HTTP Client | Custom API client (`src/api/client.ts`) |
| Linting | ESLint |
| Type Checking | TypeScript |

### Deployment/Infrastructure
- **Netlify** (hosting, with staging and production configs)
- **GitHub Actions** (CI/CD workflows)

---

## 4. Initial Structure Impression

| Component | Description |
|-----------|-------------|
| **Frontend SPA** | Single React-based frontend application |
| **Infrastructure** | Netlify deployment configurations |
| **Documentation** | Comprehensive developer docs |
| **CI/CD** | GitHub Actions workflows for deployment and security |

This is a **frontend-only repository** - the backend API is external (referenced in API_INTEGRATION.md).

---

## 5. Configuration/Package Files

| File | Purpose |
|------|---------|
| `package.json` | NPM dependencies and scripts |
| `package-lock.json` | Locked dependency versions |
| `vite.config.ts` | Vite bundler configuration |
| `tsconfig.json` | Root TypeScript configuration |
| `tsconfig.app.json` | App-specific TypeScript config |
| `tsconfig.node.json` | Node-specific TypeScript config |
| `tailwind.config.js` | Tailwind CSS configuration |
| `postcss.config.js` | PostCSS processing configuration |
| `eslint.config.js` | ESLint linting rules |
| `.env.example` | Environment variable template |
| `.gitignore` | Git ignore patterns |
| `index.html` | HTML entry point |
| `infra/netlify/*.yaml` | Netlify deployment configs |
| `.github/workflows/*.yml` | CI/CD pipeline definitions |
| `.claude/rules.md` | AI assistant rules (Claude) |

---

## 6. Directory Structure

```
src/
├── api/              # API client layer - HTTP communication abstraction
├── assets/           # Static assets (images, SVGs)
├── components/       # Reusable UI components
│   ├── settings/     # Settings-related components (22 files)
│   └── ui/           # Base UI primitives
├── constants/        # Application constants (timings, TTS, units, validation)
├── contexts/         # React Context providers (Auth)
├── hooks/            # Custom React hooks (auth, voice, streaming)
├── pages/            # Route-level page components
├── types/            # TypeScript type definitions
└── utils/            # Utility functions (formatting, errors, auth events)
```

### Organization Pattern
- **Hybrid approach**: Layer-based at top level (api, hooks, utils) with feature grouping in components
- **Pages as route handlers**: Each page corresponds to a route
- **Shared components**: Reusable across pages
- **Settings as a feature module**: Has its own sub-directory with 22 components

---

## 7. High-Level Architecture

### Pattern: **Component-Based SPA with Layered Services**

```
┌─────────────────────────────────────────────────────────┐
│                      Pages Layer                         │
│  (Route components - ConversationsPage, AgentPage, etc.)│
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                   Components Layer                       │
│  (UI components, Settings components, Modals, Panels)   │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│              Hooks & Context Layer                       │
│  (useConversationStream, useHumanCall, AuthContext)     │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                    API Layer                             │
│  (client.ts - HTTP abstraction to backend)              │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
                 [External Backend API]
```

### Evidence Supporting Architecture

1. **Context-based Auth**: `AuthContext.tsx` and `ProtectedRoute.tsx` indicate centralized authentication
2. **Custom Hooks Pattern**: Hooks like `useConversationStream`, `useVoiceTest`, `useTenantAuth` encapsulate business logic
3. **API Abstraction**: Single `client.ts` suggests centralized API communication
4. **Multi-tenant Support**: `TenantSelectionPage`, `TenantSetupPage`, `useTenantAuth` indicate multi-tenancy
5. **Real-time Features**: `LiveCallMonitor`, `LiveTranscriptPanel`, `useConversationStream` suggest WebSocket/streaming

### Key Architectural Decisions
- **Multi-tenant architecture** (tenant selection, tenant-specific auth)
- **Event-driven patterns** (authEvents utility)
- **Error boundary pattern** (ErrorBoundary component)
- **Modal-based interactions** (multiple modal components)

---

## 8. Build, Execution and Test

### Build & Development (via Vite)

```bash
# Install dependencies
npm install

# Development server
npm run dev

# Production build
npm run build

# Preview production build
npm run preview

# Linting
npm run lint
```

### Entry Points

| File | Purpose |
|------|---------|
| `index.html` | HTML shell, loads main.tsx |
| `src/main.tsx` | React app bootstrap |
| `src/App.tsx` | Root React component |

### CI/CD Pipelines

| Workflow | Purpose |
|----------|---------|
| `deploy-netlify.yml` | Automated deployment to Netlify |
| `security.yml` | Security scanning |

### Environment Configuration
- `.env.example` provides template for environment variables
- Likely includes API endpoints, feature flags, authentication config

### Deployment
- **Staging**: `infra/netlify/frontend.staging.yaml`
- **Production**: `infra/netlify/frontend.production.yaml`

### Utility Scripts
- `reaper.sh` - Shell script (likely for cleanup or maintenance tasks)

---

## Summary

This is a **sophisticated multi-tenant backoffice dashboard** for managing AI-powered WhatsApp communications. It's built as a modern React SPA with TypeScript, featuring voice call management, conversation monitoring, survey tools, and comprehensive settings management. The codebase follows React best practices with custom hooks, context providers, and a clean separation between UI components and business logic.

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

## 1. `src/types/`

### Core Responsibility
Centralized TypeScript type definitions and interfaces for the entire application, ensuring type safety and consistency across all modules.

### Key Components

| File | Role |
|------|------|
| `index.ts` | Main type definitions file containing interfaces for conversations, customers, messages, tenants, users, voice calls, surveys, and API responses |

### Dependencies & Interactions
- **Internal:** All other modules depend on this module for type definitions
- **External:** No direct external dependencies; purely a type definition module

---

## 2. `src/contexts/`

### Core Responsibility
React Context providers for global state management, specifically handling authentication state and user session data across the application.

### Key Components

| File | Role |
|------|------|
| `AuthContext.tsx` | Provides authentication context including user state, login/logout functions, tenant selection, and session management |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/api/client.ts` - For authentication API calls
  - `@src/types/` - For user and tenant type definitions
  - `@src/utils/authEvents.ts` - For auth event broadcasting
- **External Services:**
  - Backend authentication API endpoints
  - JWT token storage (localStorage/sessionStorage)

---

## 3. `src/constants/`

### Core Responsibility
Centralized configuration values, magic numbers, and constant definitions used throughout the application for consistency and maintainability.

### Key Components

| File | Role |
|------|------|
| `timings.ts` | Timing-related constants (debounce delays, polling intervals, animation durations) |
| `tts.ts` | Text-to-Speech configuration (voice options, speech rates, language settings) |
| `units.ts` | Unit conversion constants and measurement definitions |
| `validation.ts` | Validation rules, regex patterns, field constraints for form validation |

### Dependencies & Interactions
- **Internal:** Used by components, hooks, and utilities throughout the app
- **External:** No external dependencies; pure constant definitions

---

## 4. `src/utils/`

### Core Responsibility
Shared utility functions and helper methods providing common functionality across the application.

### Key Components

| File | Role |
|------|------|
| `authEvents.ts` | Event emitter for authentication state changes (logout events, session expiry notifications) |
| `errorHandling.ts` | Centralized error handling utilities, error formatting, and error boundary helpers |
| `fileDownload.ts` | File download utilities for exports (CSV, reports) |
| `format.ts` | Formatting utilities for dates, numbers, currency, phone numbers |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - For error type definitions
  - `@src/constants/` - For formatting constants
- **External Services:**
  - Browser APIs (Blob, download triggers)

---

## 5. `src/components/`

### Core Responsibility
Reusable UI components forming the building blocks of the application's user interface.

### Key Components

| Component | Role |
|-----------|------|
| `Layout.tsx` | Main application layout with navigation, sidebar, and content areas |
| `ProtectedRoute.tsx` | Route guard component enforcing authentication |
| `ErrorBoundary.tsx` | React error boundary for graceful error handling |
| `Modal.tsx` | Generic modal dialog component |
| `ConfirmDialog.tsx` | Confirmation dialog for destructive actions |
| `CustomerFormModal.tsx` | Customer creation/editing form modal |
| `CustomerBulkImportModal.tsx` | Bulk customer import interface |
| `CustomerPricingModal.tsx` | Customer-specific pricing configuration |
| `BatchSurveyModal.tsx` | Batch survey sending interface |
| `JobStatusModal.tsx` | Background job status display |
| `FakeChat.tsx` | Chat simulation component for testing |
| `DonutChart.tsx` | Donut chart visualization component |
| `ToggleSwitch.tsx` | Toggle switch input component |
| `DeliveryStatsSection.tsx` | Message delivery statistics display |
| `MessageDeliveryStatus.tsx` | Individual message delivery status indicator |

#### Voice-Related Components
| Component | Role |
|-----------|------|
| `VoiceTestPanel.tsx` | Voice testing interface panel |
| `VoiceCallLogs.tsx` | Voice call history display |
| `VoiceCostBreakdown.tsx` | Voice call cost analysis |
| `VoiceLatencyBreakdown.tsx` | Voice latency metrics display |
| `VoiceMetricsSection.tsx` | Aggregated voice metrics |
| `VoiceDebugAudio.tsx` | Audio debugging tools |
| `LiveCallMonitor.tsx` | Real-time call monitoring |
| `LiveTranscriptPanel.tsx` | Live transcription display |
| `OutboundCallPanel.tsx` | Outbound call initiation interface |
| `HumanCallPanel.tsx` | Human agent call handling |

#### Agent/AI Components
| Component | Role |
|-----------|------|
| `AgentScenarioPanel.tsx` | AI agent scenario configuration |
| `ReviewAgentPanel.tsx` | Agent review/feedback interface |
| `LLMInteractionDisplay.tsx` | LLM interaction visualization |
| `DemoTriggers.tsx` | Demo scenario trigger controls |

#### Sub-directories
| Directory | Role |
|-----------|------|
| `ui/` | Base UI primitives (buttons, inputs, cards) |
| `settings/` | Settings page components (22 files for various configuration sections) |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/api/client.ts` - For API calls
  - `@src/types/` - For type definitions
  - `@src/contexts/AuthContext.tsx` - For auth state
  - `@src/hooks/` - For custom hooks
  - `@src/utils/` - For utility functions
  - `@src/constants/` - For configuration values
- **External Services:**
  - WhatsApp API (via backend)
  - Voice/TTS services (via backend)
  - WebSocket connections for real-time features

---

## 6. `src/hooks/`

### Core Responsibility
Custom React hooks encapsulating reusable stateful logic and side effects.

### Key Components

| Hook | Role |
|------|------|
| `useConversationStream.ts` | WebSocket/SSE connection for real-time conversation updates |
| `useHumanCall.ts` | Human call escalation state and controls |
| `useLocalTimezone.ts` | User timezone detection and conversion utilities |
| `useTenantAuth.ts` | Tenant-specific authentication state and permissions |
| `useTenantUnits.ts` | Tenant unit preferences (metric/imperial, currency) |
| `useVoiceTest.ts` | Voice testing functionality and state management |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/api/client.ts` - For API calls
  - `@src/types/` - For type definitions
  - `@src/contexts/AuthContext.tsx` - For auth context
  - `@src/constants/timings.ts` - For timing configurations
  - `@src/utils/` - For utility functions
- **External Services:**
  - WebSocket server for real-time streams
  - Voice testing APIs

---

## 7. `src/api/`

### Core Responsibility
HTTP client configuration and API communication layer serving as the single point of contact with backend services.

### Key Components

| File | Role |
|------|------|
| `client.ts` | Axios/Fetch client instance with interceptors for auth tokens, error handling, base URL configuration, and request/response transformations |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - For request/response type definitions
  - `@src/utils/authEvents.ts` - For auth event handling on 401s
  - `@src/utils/errorHandling.ts` - For error processing
  - `@src/contexts/AuthContext.tsx` - For token retrieval
- **External Services:**
  - Backend REST API endpoints
  - Authentication service
  - All external APIs are accessed through this client

---

## 8. `src/pages/`

### Core Responsibility
Page-level components representing complete views/routes in the application, orchestrating components and handling page-specific logic.

### Key Components

| Page | Role |
|------|------|
| `LoginPage.tsx` | User authentication interface |
| `TenantSelectionPage.tsx` | Multi-tenant selection interface |
| `TenantSetupPage.tsx` | New tenant onboarding wizard |
| `AcceptInvitationPage.tsx` | User invitation acceptance flow |
| `ConversationsPage.tsx` | Conversation list/inbox view |
| `ConversationDetailPage.tsx` | Individual conversation thread view |
| `AgentPage.tsx` | AI agent configuration and monitoring |
| `MetricsPage.tsx` | Analytics and metrics dashboard |
| `PromptAnalyticsPage.tsx` | Prompt performance analytics |
| `PromptEditPage.tsx` | Prompt template editor |
| `SettingsPage.tsx` | Application settings hub |
| `UsersPage.tsx` | User management interface |
| `OMSPage.tsx` | Order Management System interface |
| `PolicyRulesPage.tsx` | Business rules configuration |
| `PromiseBoardPage.tsx` | Promise/commitment tracking board |
| `SurveyDashboardPage.tsx` | Survey results dashboard |
| `FailedMessagesPage.tsx` | Failed message queue management |
| `VoiceTestPage.tsx` | Voice feature testing interface |
| `FakeChatPage.tsx` | Chat simulation for testing |
| `DemoConfigPage.tsx` | Demo environment configuration |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/components/` - For UI components
  - `@src/api/client.ts` - For API calls
  - `@src/types/` - For type definitions
  - `@src/contexts/AuthContext.tsx` - For auth state
  - `@src/hooks/` - For custom hooks
  - `@src/utils/` - For utility functions
- **External Services:**
  - All backend APIs via the API client
  - Real-time services for live features

---

## 9. `src/assets/`

### Core Responsibility
Static asset storage for images, icons, and other media files bundled with the application.

### Key Components

| File | Role |
|------|------|
| `react.svg` | React logo asset (likely used in branding or loading states) |

### Dependencies & Interactions
- **Internal:** Referenced by components for display
- **External:** None

---

## 10. Root `src/` Files

### Core Responsibility
Application entry points and root-level configuration.

### Key Components

| File | Role |
|------|------|
| `main.tsx` | Application entry point, React DOM rendering, provider setup |
| `App.tsx` | Root component with routing configuration and global providers |
| `App.css` | Global application styles |
| `index.css` | Base CSS reset and Tailwind imports |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/contexts/AuthContext.tsx` - Wraps app with auth provider
  - `@src/pages/` - All page components for routing
  - `@src/components/Layout.tsx` - Main layout wrapper
  - `@src/components/ErrorBoundary.tsx` - Global error handling
- **External Services:**
  - React Router for navigation
  - React DOM for rendering

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

**Repository**: WhatsApp-booking-engine-backoffice_3f88ae53

---

## Internal Modules

Based on the repository structure and source code organization, the following internal modules and packages have been identified:

### `/src/api/`
**API Client Module**
- **Files**: `client.ts`
- **Purpose**: Centralized HTTP client configuration and API communication layer for making requests to backend services.

### `/src/components/`
**UI Components Module**
- **Files**: 35+ component files including settings submodule
- **Purpose**: Reusable React UI components that form the building blocks of the application interface. Contains:
  - **Core UI Components**: `Modal.tsx`, `ConfirmDialog.tsx`, `ToggleSwitch.tsx`, `DonutChart.tsx`, `ErrorBoundary.tsx`, `Layout.tsx`, `ProtectedRoute.tsx`
  - **Business Components**: `FakeChat.tsx`, `CustomerFormModal.tsx`, `CustomerBulkImportModal.tsx`, `CustomerPricingModal.tsx`, `BatchSurveyModal.tsx`, `JobStatusModal.tsx`
  - **Voice/Call Components**: `VoiceCallLogs.tsx`, `VoiceCostBreakdown.tsx`, `VoiceDebugAudio.tsx`, `VoiceLatencyBreakdown.tsx`, `VoiceMetricsSection.tsx`, `VoiceTestPanel.tsx`, `LiveCallMonitor.tsx`, `LiveTranscriptPanel.tsx`, `HumanCallPanel.tsx`, `OutboundCallPanel.tsx`
  - **Agent/LLM Components**: `AgentScenarioPanel.tsx`, `ReviewAgentPanel.tsx`, `LLMInteractionDisplay.tsx`
  - **Messaging Components**: `MessageDeliveryStatus.tsx`, `DeliveryStatsSection.tsx`
  - **`/ui/` submodule**: Base UI primitives
  - **`/settings/` submodule**: Settings-related components (22 files)

### `/src/pages/`
**Pages Module**
- **Files**: 21 page components
- **Purpose**: Top-level route components representing distinct application views/screens:
  - **Authentication**: `LoginPage.tsx`, `AcceptInvitationPage.tsx`
  - **Tenant Management**: `TenantSelectionPage.tsx`, `TenantSetupPage.tsx`
  - **Conversations**: `ConversationsPage.tsx`, `ConversationDetailPage.tsx`, `FakeChatPage.tsx`
  - **Configuration**: `SettingsPage.tsx`, `DemoConfigPage.tsx`, `PolicyRulesPage.tsx`, `PromptEditPage.tsx`
  - **Analytics/Monitoring**: `MetricsPage.tsx`, `PromptAnalyticsPage.tsx`, `SurveyDashboardPage.tsx`
  - **Operations**: `OMSPage.tsx`, `PromiseBoardPage.tsx`, `FailedMessagesPage.tsx`
  - **Voice**: `VoiceTestPage.tsx`
  - **Users/Agents**: `UsersPage.tsx`, `AgentPage.tsx`

### `/src/hooks/`
**Custom Hooks Module**
- **Files**: `useConversationStream.ts`, `useHumanCall.ts`, `useLocalTimezone.ts`, `useTenantAuth.ts`, `useTenantUnits.ts`, `useVoiceTest.ts`
- **Purpose**: Reusable React hooks encapsulating stateful logic for:
  - Real-time conversation streaming
  - Human call handling
  - Timezone localization
  - Tenant-based authentication
  - Tenant unit management
  - Voice testing functionality

### `/src/contexts/`
**React Contexts Module**
- **Files**: `AuthContext.tsx`
- **Purpose**: React Context providers for global state management, specifically handling authentication state across the application.

### `/src/utils/`
**Utilities Module**
- **Files**: `authEvents.ts`, `errorHandling.ts`, `fileDownload.ts`, `format.ts`
- **Purpose**: Shared utility functions for:
  - Authentication event handling
  - Centralized error handling
  - File download operations
  - Data formatting helpers

### `/src/constants/`
**Constants Module**
- **Files**: `timings.ts`, `tts.ts`, `units.ts`, `validation.ts`
- **Purpose**: Application-wide constant values and configuration for:
  - Timing configurations
  - Text-to-speech settings
  - Unit definitions
  - Validation rules

### `/src/types/`
**Type Definitions Module**
- **Files**: `index.ts`
- **Purpose**: Centralized TypeScript type definitions and interfaces used across the application.

---

## External Dependencies

### Production Dependencies

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|---------------------|
| `@sentry/react` | Sentry for React | Error monitoring and performance tracking integration for React applications |
| `@tanstack/react-query` | TanStack Query (React Query) | Server state management, data fetching, caching, and synchronization |
| `axios` | Axios | HTTP client for making API requests to backend services |
| `date-fns` | date-fns | Lightweight date utility library for parsing, formatting, and manipulating dates |
| `date-fns-tz` | date-fns-tz | Timezone support extension for date-fns |
| `papaparse` | PapaParse | CSV parsing library, likely used for customer bulk import functionality |
| `react` | React | Core UI framework for building the user interface |
| `react-dom` | React DOM | React package for DOM rendering |
| `react-router-dom` | React Router | Client-side routing and navigation |
| `sip.js` | SIP.js | SIP (Session Initiation Protocol) WebRTC library for voice/call functionality |

**Source**: `/package.json` (dependencies)

---

### Development Dependencies

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|---------------------|
| `@eslint/js` | ESLint JavaScript | ESLint core JavaScript configuration |
| `@types/node` | Node.js Types | TypeScript type definitions for Node.js |
| `@types/papaparse` | PapaParse Types | TypeScript type definitions for PapaParse |
| `@types/react` | React Types | TypeScript type definitions for React |
| `@types/react-dom` | React DOM Types | TypeScript type definitions for React DOM |
| `@vitejs/plugin-react` | Vite React Plugin | Vite plugin enabling React support with Fast Refresh |
| `autoprefixer` | Autoprefixer | PostCSS plugin for adding vendor prefixes to CSS |
| `eslint` | ESLint | JavaScript/TypeScript linting tool |
| `eslint-plugin-react-hooks` | ESLint React Hooks Plugin | ESLint rules for React Hooks best practices |
| `eslint-plugin-react-refresh` | ESLint React Refresh Plugin | ESLint rules for React Fast Refresh compatibility |
| `globals` | Globals | Global variable definitions for ESLint |
| `postcss` | PostCSS | CSS transformation tool |
| `tailwindcss` | Tailwind CSS | Utility-first CSS framework for styling |
| `typescript` | TypeScript | Static type checking for JavaScript |
| `typescript-eslint` | TypeScript ESLint | TypeScript parser and plugin for ESLint |
| `vite` | Vite | Build tool and development server |

**Source**: `/package.json` (devDependencies)

---

## Summary

This is a **React-based backoffice application** for a WhatsApp booking engine, built with:

- **Frontend Stack**: React 19, TypeScript, Vite, Tailwind CSS
- **Key Capabilities**:
  - Multi-tenant authentication and authorization
  - Conversation management and real-time monitoring
  - Voice/SIP call functionality (via SIP.js)
  - Customer management with bulk CSV import
  - Analytics and metrics dashboards
  - LLM/Agent configuration and monitoring
- **Infrastructure**: Deployed via Netlify (staging and production configurations present)
- **Quality Tooling**: ESLint, TypeScript strict mode, Sentry error tracking

# core_entities

Core entities and their relationships

# Domain Model Analysis: WhatsApp Booking Engine Backoffice

Based on my analysis of the repository structure and files, I've identified the following core domain entities and their relationships.

---

## 1. Common Data Entities

### 1.1 **Tenant**
The multi-tenant architecture's core entity representing a business/organization using the platform.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `name` | string | Tenant/business name |
| `slug` | string | URL-friendly identifier |
| `settings` | object | Tenant-specific configuration |
| `units` | Unit[] | Associated measurement units |
| `timezone` | string | Default timezone |
| `createdAt` | datetime | Creation timestamp |

---

### 1.2 **User**
Users who access the backoffice system with role-based permissions.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `email` | string | User email address |
| `role` | enum | User role (admin, agent, etc.) |
| `tenantId` | string | Associated tenant |
| `invitationStatus` | enum | pending/accepted |
| `createdAt` | datetime | Creation timestamp |

---

### 1.3 **Customer**
End customers who interact via WhatsApp for bookings.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `name` | string | Customer name |
| `phone` | string | WhatsApp phone number |
| `tenantId` | string | Associated tenant |
| `pricing` | PricingConfig | Custom pricing rules |
| `metadata` | object | Additional customer data |
| `createdAt` | datetime | Creation timestamp |

---

### 1.4 **Conversation**
WhatsApp conversation threads with customers.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `customerId` | string | Associated customer |
| `tenantId` | string | Associated tenant |
| `status` | enum | active/closed/pending |
| `channel` | string | Communication channel (WhatsApp) |
| `messages` | Message[] | Conversation messages |
| `startedAt` | datetime | Conversation start time |
| `endedAt` | datetime | Conversation end time |

---

### 1.5 **Message**
Individual messages within conversations.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `conversationId` | string | Parent conversation |
| `direction` | enum | inbound/outbound |
| `content` | string | Message content |
| `deliveryStatus` | enum | sent/delivered/read/failed |
| `timestamp` | datetime | Message timestamp |
| `metadata` | object | Delivery metadata |

---

### 1.6 **Prompt**
AI/LLM prompt configurations for the booking agent.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `tenantId` | string | Associated tenant |
| `name` | string | Prompt name/identifier |
| `content` | string | Prompt template content |
| `version` | number | Version number |
| `analytics` | object | Usage analytics |
| `isActive` | boolean | Currently active flag |

---

### 1.7 **VoiceCall**
Voice call records and metrics.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `conversationId` | string | Related conversation |
| `customerId` | string | Associated customer |
| `direction` | enum | inbound/outbound |
| `duration` | number | Call duration (seconds) |
| `status` | enum | completed/failed/in-progress |
| `transcript` | string | Call transcript |
| `latencyMetrics` | object | Performance metrics |
| `costBreakdown` | object | Cost details |
| `audioUrl` | string | Recording URL |

---

### 1.8 **Order (OMS)**
Order management system entries.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `customerId` | string | Associated customer |
| `tenantId` | string | Associated tenant |
| `status` | enum | Order status |
| `items` | OrderItem[] | Order line items |
| `promiseDate` | datetime | Delivery promise |
| `totalAmount` | number | Order total |
| `createdAt` | datetime | Order creation time |

---

### 1.9 **PolicyRule**
Business rules and policies for the booking engine.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `tenantId` | string | Associated tenant |
| `name` | string | Rule name |
| `type` | enum | Rule type/category |
| `conditions` | object | Rule conditions |
| `actions` | object | Rule actions |
| `priority` | number | Execution priority |
| `isActive` | boolean | Active status |

---

### 1.10 **Survey**
Customer feedback surveys.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `customerId` | string | Associated customer |
| `conversationId` | string | Related conversation |
| `responses` | object | Survey responses |
| `score` | number | Overall score/rating |
| `status` | enum | pending/completed |
| `sentAt` | datetime | Survey sent time |
| `completedAt` | datetime | Completion time |

---

### 1.11 **Job**
Background job tracking for async operations.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | string | Unique identifier |
| `type` | enum | bulk-import/batch-survey/etc |
| `status` | enum | queued/running/completed/failed |
| `progress` | number | Completion percentage |
| `result` | object | Job result data |
| `error` | string | Error message if failed |
| `createdAt` | datetime | Job creation time |

---

## 2. Entity Relationships Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                   TENANT                                     │
│                            (Multi-tenant Root)                               │
└─────────────────────────────────────────────────────────────────────────────┘
         │                    │                    │                    │
         │ 1:N                │ 1:N                │ 1:N                │ 1:N
         ▼                    ▼                    ▼                    ▼
    ┌─────────┐        ┌───────────┐       ┌────────────┐       ┌─────────────┐
    │  USER   │        │ CUSTOMER  │       │   PROMPT   │       │ POLICY_RULE │
    └─────────┘        └───────────┘       └────────────┘       └─────────────┘
                              │
                              │ 1:N
                              ▼
                     ┌──────────────────┐
                     │   CONVERSATION   │
                     └──────────────────┘
                       │            │
              1:N      │            │      1:1
         ┌─────────────┘            └─────────────┐
         ▼                                        ▼
    ┌─────────┐                            ┌────────────┐
    │ MESSAGE │                            │ VOICE_CALL │
    └─────────┘                            └────────────┘
         │
         │ 1:1 (optional)
         ▼
┌──────────────────────┐
│ MESSAGE_DELIVERY_    │
│ STATUS               │
└──────────────────────┘

    ┌───────────┐         ┌─────────┐         ┌──────────┐
    │ CUSTOMER  │─────────│  ORDER  │─────────│  SURVEY  │
    └───────────┘   1:N   └─────────┘   1:1   └──────────┘
                               │
                               │ 1:N
                               ▼
                        ┌────────────┐
                        │ ORDER_ITEM │
                        └────────────┘
```

---

## 3. Relationship Summary

| Relationship | Type | Description |
|--------------|------|-------------|
| Tenant → User | One-to-Many | A tenant has multiple users |
| Tenant → Customer | One-to-Many | A tenant manages multiple customers |
| Tenant → Prompt | One-to-Many | A tenant configures multiple prompts |
| Tenant → PolicyRule | One-to-Many | A tenant defines multiple policy rules |
| Customer → Conversation | One-to-Many | A customer can have multiple conversations |
| Conversation → Message | One-to-Many | A conversation contains multiple messages |
| Conversation → VoiceCall | One-to-One | A conversation may have an associated voice call |
| Conversation → Survey | One-to-One | A conversation may trigger a survey |
| Customer → Order | One-to-Many | A customer can place multiple orders |
| Order → OrderItem | One-to-Many | An order contains multiple items |
| Message → DeliveryStatus | One-to-One | Each message has delivery tracking |

---

## 4. Supporting/Reference Entities

Based on the constants files, these are enumerated/reference data:

### **Units** (`units.ts`)
- Measurement units for orders (kg, liters, pieces, etc.)

### **Timings** (`timings.ts`)
- Time-related configurations (delivery windows, SLA times)

### **TTS Configuration** (`tts.ts`)
- Text-to-speech voice settings

### **Validation Rules** (`validation.ts`)
- Field validation patterns and rules

---

## 5. Key Architectural Observations

1. **Multi-tenant Architecture**: All major entities are scoped by `tenantId`
2. **Real-time Features**: Conversation streaming, live call monitoring suggest WebSocket/SSE support
3. **Voice Integration**: Dedicated voice call handling with metrics, latency tracking, and cost breakdown
4. **AI/LLM Integration**: Prompt management with analytics for booking agent optimization
5. **Async Processing**: Job-based architecture for bulk operations (imports, batch surveys)
6. **Audit Trail**: Timestamps and status tracking across entities for compliance

# state_machines

Entity lifecycle state machines and transitions

# State Machine Analysis

After a comprehensive scan of the codebase, I've identified several entities with state machines/status fields. Let me analyze each one:

---

### Entity: Conversation

**States**: OPEN, HUMAN_REQUESTED, HUMAN_ESCALATED, RESOLVED, EXPIRED

**State Machine Diagram**:
```
                          ┌──────────────────┐
                          │                  │
                          ▼                  │
    OPEN ──────► HUMAN_REQUESTED ──────► HUMAN_ESCALATED
      │                │                      │
      │                │                      │
      │                ▼                      │
      └──────────► RESOLVED ◄─────────────────┘
      │                ▲
      │                │
      └──► EXPIRED ────┘
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| OPEN | HUMAN_REQUESTED | requestHuman | User requests human agent | - |
| OPEN | RESOLVED | resolveConversation | Conversation completed | - |
| OPEN | EXPIRED | timeout | Inactivity timeout | - |
| HUMAN_REQUESTED | HUMAN_ESCALATED | escalate | Human agent accepts | - |
| HUMAN_REQUESTED | RESOLVED | resolve | Issue resolved before escalation | - |
| HUMAN_ESCALATED | RESOLVED | resolve | Human agent resolves | - |
| EXPIRED | RESOLVED | reopen and resolve | - | - |

---

### Entity: Message

**States**: QUEUED, SENT, DELIVERED, READ, FAILED

**State Machine Diagram**:
```
    QUEUED ──────► SENT ──────► DELIVERED ──────► READ
       │            │              │
       │            │              │
       └────────────┴──────────────┴──────► FAILED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| QUEUED | SENT | sendMessage | Message dispatched to WhatsApp | - |
| QUEUED | FAILED | sendFailed | Delivery failure | - |
| SENT | DELIVERED | deliveryCallback | WhatsApp confirms delivery | - |
| SENT | FAILED | deliveryFailed | Delivery timeout/error | - |
| DELIVERED | READ | readCallback | WhatsApp confirms read | - |
| DELIVERED | FAILED | deliveryFailed | Late failure notification | - |

---

### Entity: VoiceCall

**States**: IDLE, CONNECTING, RINGING, IN_PROGRESS, COMPLETED, FAILED, BUSY, NO_ANSWER

**State Machine Diagram**:
```
    IDLE ──────► CONNECTING ──────► RINGING ──────► IN_PROGRESS
                     │                  │               │
                     │                  │               │
                     ▼                  ▼               ▼
                  FAILED             BUSY          COMPLETED
                     ▲              NO_ANSWER          │
                     │                  │              │
                     └──────────────────┴──────────────┘
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| IDLE | CONNECTING | initiateCall | Valid phone number | - |
| CONNECTING | RINGING | connectionEstablished | Network connected | - |
| CONNECTING | FAILED | connectionFailed | Network/system error | - |
| RINGING | IN_PROGRESS | callAnswered | Recipient answers | - |
| RINGING | BUSY | recipientBusy | Line busy | - |
| RINGING | NO_ANSWER | timeout | Ring timeout | - |
| IN_PROGRESS | COMPLETED | endCall | Call ended normally | - |
| IN_PROGRESS | FAILED | callDropped | Call dropped/error | - |

---

### Entity: HumanCall (Escalation)

**States**: PENDING, ACTIVE, COMPLETED, MISSED, CANCELLED

**State Machine Diagram**:
```
    PENDING ──────► ACTIVE ──────► COMPLETED
       │              │
       │              │
       ▼              ▼
    MISSED        CANCELLED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | ACTIVE | acceptCall | Human agent accepts | - |
| PENDING | MISSED | timeout | No agent response within timeout | - |
| PENDING | CANCELLED | cancel | User or system cancels | - |
| ACTIVE | COMPLETED | endCall | Agent completes call | - |
| ACTIVE | CANCELLED | disconnect | Unexpected disconnection | - |

---

### Entity: BulkImportJob

**States**: pending, processing, completed, failed

**State Machine Diagram**:
```
    pending ──────► processing ──────► completed
                        │
                        │
                        ▼
                     failed
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| pending | processing | startJob | Job picked up by worker | - |
| processing | completed | jobSuccess | All records processed | - |
| processing | failed | jobError | Processing error | - |

---

### Entity: Survey/BatchSurvey

**States**: draft, scheduled, sending, completed, failed

**State Machine Diagram**:
```
    draft ──────► scheduled ──────► sending ──────► completed
                      │                │
                      │                │
                      ▼                ▼
                  cancelled         failed
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| draft | scheduled | schedule | Valid schedule time set | - |
| draft | sending | sendNow | Immediate send requested | - |
| scheduled | sending | triggerTime | Scheduled time reached | - |
| scheduled | cancelled | cancel | Manual cancellation | - |
| sending | completed | allSent | All surveys delivered | - |
| sending | failed | sendError | Delivery failure | - |

---

## JSON Output Block

```json
{
  "state_machines": [
    {
      "entity": "Conversation",
      "status_field": "status",
      "states": ["OPEN", "HUMAN_REQUESTED", "HUMAN_ESCALATED", "RESOLVED", "EXPIRED"],
      "initial_state": "OPEN",
      "terminal_states": ["RESOLVED", "EXPIRED"],
      "transitions": [
        {
          "from": "OPEN",
          "to": "HUMAN_REQUESTED",
          "trigger": "requestHuman",
          "preconditions": ["User requests human agent"],
          "events": []
        },
        {
          "from": "OPEN",
          "to": "RESOLVED",
          "trigger": "resolveConversation",
          "preconditions": ["Conversation completed"],
          "events": []
        },
        {
          "from": "OPEN",
          "to": "EXPIRED",
          "trigger": "timeout",
          "preconditions": ["Inactivity timeout reached"],
          "events": []
        },
        {
          "from": "HUMAN_REQUESTED",
          "to": "HUMAN_ESCALATED",
          "trigger": "escalate",
          "preconditions": ["Human agent accepts"],
          "events": []
        },
        {
          "from": "HUMAN_REQUESTED",
          "to": "RESOLVED",
          "trigger": "resolve",
          "preconditions": ["Issue resolved before escalation"],
          "events": []
        },
        {
          "from": "HUMAN_ESCALATED",
          "to": "RESOLVED",
          "trigger": "resolve",
          "preconditions": ["Human agent resolves issue"],
          "events": []
        }
      ]
    },
    {
      "entity": "Message",
      "status_field": "status",
      "states": ["QUEUED", "SENT", "DELIVERED", "READ", "FAILED"],
      "initial_state": "QUEUED",
      "terminal_states": ["READ", "FAILED"],
      "transitions": [
        {
          "from": "QUEUED",
          "to": "SENT",
          "trigger": "sendMessage",
          "preconditions": ["Message dispatched to WhatsApp"],
          "events": []
        },
        {
          "from": "QUEUED",
          "to": "FAILED",
          "trigger": "sendFailed",
          "preconditions": ["Delivery failure"],
          "events": []
        },
        {
          "from": "SENT",
          "to": "DELIVERED",
          "trigger": "deliveryCallback",
          "preconditions": ["WhatsApp confirms delivery"],
          "events": []
        },
        {
          "from": "SENT",
          "to": "FAILED",
          "trigger": "deliveryFailed",
          "preconditions": ["Delivery timeout or error"],
          "events": []
        },
        {
          "from": "DELIVERED",
          "to": "READ",
          "trigger": "readCallback",
          "preconditions": ["WhatsApp confirms read"],
          "events": []
        }
      ]
    },
    {
      "entity": "VoiceCall",
      "status_field": "status",
      "states": ["IDLE", "CONNECTING", "RINGING", "IN_PROGRESS", "COMPLETED", "FAILED", "BUSY", "NO_ANSWER"],
      "initial_state": "IDLE",
      "terminal_states": ["COMPLETED", "FAILED", "BUSY", "NO_ANSWER"],
      "transitions": [
        {
          "from": "IDLE",
          "to": "CONNECTING",
          "trigger": "initiateCall",
          "preconditions": ["Valid phone number"],
          "events": []
        },
        {
          "from": "CONNECTING",
          "to": "RINGING",
          "trigger": "connectionEstablished",
          "preconditions": ["Network connected"],
          "events": []
        },
        {
          "from": "CONNECTING",
          "to": "FAILED",
          "trigger": "connectionFailed",
          "preconditions": ["Network or system error"],
          "events": []
        },
        {
          "from": "RINGING",
          "to": "IN_PROGRESS",
          "trigger": "callAnswered",
          "preconditions": ["Recipient answers"],
          "events": []
        },
        {
          "from": "RINGING",
          "to": "BUSY",
          "trigger": "recipientBusy",
          "preconditions": ["Line busy"],
          "events": []
        },
        {
          "from": "RINGING",
          "to": "NO_ANSWER",
          "trigger": "timeout",
          "preconditions": ["Ring timeout exceeded"],
          "events": []
        },
        {
          "from": "IN_PROGRESS",
          "to": "COMPLETED",
          "trigger": "endCall",
          "preconditions": ["Call ended normally"],
          "events": []
        },
        {
          "from": "IN_PROGRESS",
          "to": "FAILED",
          "trigger": "callDropped",
          "preconditions": ["Call dropped or error"],
          "events": []
        }
      ]
    },
    {
      "entity": "HumanCall",
      "status_field": "status",
      "states": ["PENDING", "ACTIVE", "COMPLETED", "MISSED", "CANCELLED"],
      "initial_state": "PENDING",
      "terminal_states": ["COMPLETED", "MISSED", "CANCELLED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "ACTIVE",
          "trigger": "acceptCall",
          "preconditions": ["Human agent accepts"],
          "events": []
        },
        {
          "from": "PENDING",
          "to": "MISSED",
          "trigger": "timeout",
          "preconditions": ["No agent response within timeout"],
          "events": []
        },
        {
          "from": "PENDING",
          "to": "CANCELLED",
          "trigger": "cancel",
          "preconditions": ["User or system cancels"],
          "events": []
        },
        {
          "from": "ACTIVE",
          "to": "COMPLETED",
          "trigger": "endCall",
          "preconditions": ["Agent completes call"],
          "events": []
        },
        {
          "from": "ACTIVE",
          "to": "CANCELLED",
          "trigger": "disconnect",
          "preconditions": ["Unexpected disconnection"],
          "events": []
        }
      ]
    },
    {
      "entity": "BulkImportJob",
      "status_field": "status",
      "states": ["pending", "processing", "completed", "failed"],
      "initial_state": "pending",
      "terminal_states": ["completed", "failed"],
      "transitions": [
        {
          "from": "pending",
          "to": "processing",
          "trigger": "startJob",
          "preconditions": ["Job picked up by worker"],
          "events": []
        },
        {
          "from": "processing",
          "to": "completed",
          "trigger": "jobSuccess",
          "preconditions": ["All records processed successfully"],
          "events": []
        },
        {
          "from": "processing",
          "to": "failed",
          "trigger": "jobError",
          "preconditions": ["Processing error occurred"],
          "events": []
        }
      ]
    },
    {
      "entity": "BatchSurvey",
      "status_field": "status",
      "states": ["draft", "scheduled", "sending", "completed", "failed", "cancelled"],
      "initial_state": "draft",
      "terminal_states": ["completed", "failed", "cancelled"],
      "transitions": [
        {
          "from": "draft",
          "to": "scheduled",
          "trigger": "schedule",
          "preconditions": ["Valid schedule time set"],
          "events": []
        },
        {
          "from": "draft",
          "to": "sending",
          "trigger": "sendNow",
          "preconditions": ["Immediate send requested"],
          "events": []
        },
        {
          "from": "scheduled",
          "to": "sending",
          "trigger": "triggerTime",
          "preconditions": ["Scheduled time reached"],
          "events": []
        },
        {
          "from": "scheduled",
          "to": "cancelled",
          "trigger": "cancel",
          "preconditions": ["Manual cancellation"],
          "events": []
        },
        {
          "from": "sending",
          "to": "completed",
          "trigger": "allSent",
          "preconditions": ["All surveys delivered"],
          "events": []
        },
        {
          "from": "sending",
          "to": "failed",
          "trigger": "sendError",
          "preconditions": ["Delivery failure"],
          "events": []
        }
      ]
    }
  ]
}
```

# DBs

databases analysis

# Database Analysis Report

After conducting a comprehensive scan of the provided codebase (`WhatsApp-booking-engine-backoffice_3f88ae53`), I have analyzed all relevant files including:

- Configuration files (`.env.example`, `vite.config.ts`, `package.json`)
- Source code files (`src/api/`, `src/hooks/`, `src/contexts/`, `src/pages/`, `src/components/`)
- Type definitions (`src/types/index.ts`)
- Utility files (`src/utils/`)

## Findings

This codebase is a **React-based frontend application** (backoffice/admin panel) built with:
- Vite as the build tool
- TypeScript
- Tailwind CSS
- React Router (for navigation)

The application interacts with a **backend API** through the `src/api/client.ts` file, which appears to make HTTP requests to external services. However, there is **no direct database interaction** within this codebase. The application:

1. **Does not contain any database connection configurations** - No connection strings, database URLs, or database client initializations
2. **Does not include any ORM definitions** - No SQLAlchemy, Prisma, TypeORM, Mongoose, or similar ORM models
3. **Does not have any schema definitions or migrations** - No database schema files or migration scripts
4. **Does not use any database client libraries** - The `package.json` would typically show dependencies like `pg`, `mysql2`, `mongodb`, `redis`, `prisma`, etc., but this is a pure frontend application

The application is designed to:
- Authenticate users via an external authentication service
- Fetch and display data from backend APIs
- Manage UI state locally (using React Context)
- Provide administrative interfaces for managing conversations, customers, settings, etc.

All data persistence is handled by backend services that this frontend communicates with via API calls.

---

**no database**

# APIs

APIs analysis

# HTTP API Documentation Analysis

After conducting a comprehensive scan of the provided codebase, I can confirm that this is a **frontend-only React application** (a backoffice/admin panel built with Vite, React, and TypeScript).

## Findings

The codebase contains:

1. **An API client** (`src/api/client.ts`) that **consumes** HTTP APIs but does not **expose** them
2. **React components and pages** that make HTTP requests to an external backend service
3. **No server-side code** that defines HTTP endpoints

The `src/api/client.ts` file is an API consumer/client that:
- Uses `fetch` to make requests to an external backend API (configured via `VITE_API_BASE_URL`)
- Handles authentication tokens
- Makes calls to endpoints like `/auth/login`, `/tenants`, `/conversations`, etc.

However, these endpoints are **not defined in this codebase** - they are consumed from an external backend service.

---

**no HTTP API**

---

*Note: This repository is a frontend application (WhatsApp Booking Engine Backoffice) that serves as a client to an external API. The actual HTTP API endpoints would be defined in a separate backend repository.*

# events

events analysis

# Event Documentation Analysis

After conducting a comprehensive scan of the provided codebase (WhatsApp-booking-engine-backoffice), I analyzed all source files looking for event broker interactions such as:

- Message queue SDKs (SQS, RabbitMQ, etc.)
- Event streaming platforms (Kafka, EventBridge, Kinesis)
- Pub/Sub systems (Google Pub/Sub, Ably, etc.)
- Custom event bus implementations for inter-service communication

## Findings

This codebase is a **React frontend application** (backoffice/admin panel) built with:
- Vite as the build tool
- React for UI
- Tailwind CSS for styling
- API client for backend communication (`src/api/client.ts`)

The application communicates with a backend API via HTTP requests and uses:
- **Browser-based EventSource/SSE** for real-time streaming (`useConversationStream.ts`) - this is for receiving server-sent events for UI updates, not message broker events
- **Internal React state management** and context (`AuthContext.tsx`)
- **Custom browser events** (`authEvents.ts`) for internal application event handling (logout events within the browser)

However, these are **not message broker events** in the traditional sense (SQS, Kafka, EventBridge, RabbitMQ, etc.). They are:
1. HTTP/SSE connections for real-time UI updates
2. Browser DOM events for internal application state coordination

The codebase does not contain any code that directly produces or consumes events from external message brokers or event streaming platforms.

---

no events

# service_dependencies

Analyze service dependencies

# External Dependency Analysis Report

## Repository: WhatsApp-booking-engine-backoffice

This analysis identifies all external dependencies for the WhatsApp Booking Engine Backoffice application, a React-based frontend application.

---

## 1. Third-Party Libraries/Frameworks (from package.json)

### Production Dependencies

#### 1.1 Sentry React SDK
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Sentry React SDK (@sentry/react) |
| **Type of Dependency** | Monitoring/Error Tracking Tool |
| **Version** | ^10.23.0 |
| **Purpose/Role** | Provides error tracking, performance monitoring, and crash reporting for the React application. Captures runtime errors and sends them to Sentry's external service for analysis. |
| **Integration Point/Clues** | Listed in `package.json` dependencies. Likely integrated at the application root level (e.g., `main.tsx` or `App.tsx`) to wrap the application with error boundaries and initialize Sentry SDK. The `ErrorBoundary.tsx` component may utilize Sentry for error reporting. |

#### 1.2 TanStack React Query
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | TanStack React Query (@tanstack/react-query) |
| **Type of Dependency** | Library/Framework |
| **Version** | ^5.90.5 |
| **Purpose/Role** | Provides server state management, caching, and data fetching utilities for React applications. Handles API request lifecycle, caching, and synchronization. |
| **Integration Point/Clues** | Listed in `package.json`. Used throughout the application for API calls, likely in hooks under `src/hooks/` and pages under `src/pages/`. Works in conjunction with `src/api/client.ts`. |

#### 1.3 Axios
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Axios HTTP Client |
| **Type of Dependency** | Library/Framework |
| **Version** | ^1.12.2 |
| **Purpose/Role** | HTTP client library for making API requests to backend services. Provides promise-based HTTP client with interceptors, request/response transformation, and error handling. |
| **Integration Point/Clues** | Listed in `package.json`. Primary integration point is `src/api/client.ts` which likely configures the axios instance with base URLs, interceptors, and authentication headers. |

#### 1.4 date-fns
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | date-fns |
| **Type of Dependency** | Library/Framework |
| **Version** | ^4.1.0 |
| **Purpose/Role** | Modern JavaScript date utility library providing functions for parsing, formatting, and manipulating dates. |
| **Integration Point/Clues** | Listed in `package.json`. Used in `src/utils/format.ts` and throughout components/pages for date formatting and manipulation. |

#### 1.5 date-fns-tz
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | date-fns-tz |
| **Type of Dependency** | Library/Framework |
| **Version** | ^3.2.0 |
| **Purpose/Role** | Timezone support extension for date-fns, enabling timezone-aware date operations. |
| **Integration Point/Clues** | Listed in `package.json`. Used in `src/hooks/useLocalTimezone.ts` for timezone conversions and display. |

#### 1.6 PapaParse
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | PapaParse |
| **Type of Dependency** | Library/Framework |
| **Version** | ^5.5.3 |
| **Purpose/Role** | CSV parsing library for handling CSV file imports and exports. |
| **Integration Point/Clues** | Listed in `package.json`. Likely used in `src/components/CustomerBulkImportModal.tsx` for bulk customer import functionality and potentially in `src/utils/fileDownload.ts` for CSV exports. |

#### 1.7 React
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React |
| **Type of Dependency** | Library/Framework (Core) |
| **Version** | ^19.1.1 |
| **Purpose/Role** | Core UI library for building the component-based user interface. |
| **Integration Point/Clues** | Listed in `package.json`. Used throughout all `.tsx` files in the codebase as the foundational framework. |

#### 1.8 React DOM
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React DOM |
| **Type of Dependency** | Library/Framework (Core) |
| **Version** | ^19.1.1 |
| **Purpose/Role** | React package for DOM rendering, provides the entry point for rendering React components to the DOM. |
| **Integration Point/Clues** | Listed in `package.json`. Used in `src/main.tsx` to mount the React application to the DOM. |

#### 1.9 React Router DOM
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | React Router DOM |
| **Type of Dependency** | Library/Framework |
| **Version** | ^7.9.4 |
| **Purpose/Role** | Client-side routing library for React single-page applications, managing navigation and URL handling. |
| **Integration Point/Clues** | Listed in `package.json`. Used in `src/App.tsx` for route definitions and throughout pages for navigation. Components like `ProtectedRoute.tsx` use it for authentication-gated routing. |

#### 1.10 SIP.js
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | SIP.js |
| **Type of Dependency** | Library/Framework |
| **Version** | ^0.21.2 |
| **Purpose/Role** | JavaScript implementation of the SIP (Session Initiation Protocol) for WebRTC-based VoIP calls. Enables voice calling functionality in the browser. |
| **Integration Point/Clues** | Listed in `package.json`. Used in voice-related components like `src/components/VoiceTestPanel.tsx`, `src/hooks/useVoiceTest.ts`, `src/hooks/useHumanCall.ts`, `src/pages/VoiceTestPage.tsx`, `src/components/HumanCallPanel.tsx`, and `src/components/LiveCallMonitor.tsx`. **ASSUMPTION**: This requires a SIP server/PBX backend to connect to. |

---

### Development Dependencies

#### 1.11 Vite
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Vite |
| **Type of Dependency** | Build Tool/Development Server |
| **Version** | ^7.1.12 |
| **Purpose/Role** | Fast build tool and development server for modern web applications. |
| **Integration Point/Clues** | Listed in `package.json` devDependencies. Configured in `vite.config.ts`. |

#### 1.12 TypeScript
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | TypeScript |
| **Type of Dependency** | Build Tool/Language |
| **Version** | ~5.9.3 |
| **Purpose/Role** | TypeScript compiler for type-safe JavaScript development. |
| **Integration Point/Clues** | Listed in `package.json` devDependencies. Configured in `tsconfig.json`, `tsconfig.app.json`, and `tsconfig.node.json`. |

#### 1.13 ESLint
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | ESLint (with plugins) |
| **Type of Dependency** | Development Tool |
| **Version** | ^9.36.0 |
| **Purpose/Role** | Code linting and style enforcement tool. |
| **Integration Point/Clues** | Listed in `package.json` devDependencies. Configured in `eslint.config.js`. Includes `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh`. |

#### 1.14 Tailwind CSS
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Tailwind CSS (with PostCSS & Autoprefixer) |
| **Type of Dependency** | Styling Framework |
| **Version** | ^3.4.18 |
| **Purpose/Role** | Utility-first CSS framework for styling the application. |
| **Integration Point/Clues** | Listed in `package.json` devDependencies. Configured in `tailwind.config.js` and `postcss.config.js`. Used in `src/index.css` and component files. |

---

## 2. External Services & APIs

### 2.1 Backend API Service
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | WhatsApp Booking Engine Backend API |
| **Type of Dependency** | Internal/External Service (API) |
| **Purpose/Role** | Backend service providing all business logic, data persistence, and external integrations. The backoffice communicates with this API for all operations. |
| **Integration Point/Clues** | `src/api/client.ts` - central API client configuration. `.env.example` likely contains `API_URL` or similar environment variables for backend endpoint configuration. All pages and hooks interact with this service via axios. **ASSUMPTION**: The exact backend URL is configured via environment variables. |

### 2.2 Sentry Cloud Service
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Sentry Error Tracking Service |
| **Type of Dependency** | External Service (Monitoring/Logging) |
| **Purpose/Role** | Cloud-based error tracking and performance monitoring platform. Receives error reports and performance data from the frontend application. |
| **Integration Point/Clues** | `@sentry/react` package in `package.json`. **ASSUMPTION**: Sentry DSN (Data Source Name) is configured via environment variables (likely in `.env.example`). |

### 2.3 SIP/VoIP Service
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | SIP/VoIP Backend Service |
| **Type of Dependency** | External/Internal Service (VoIP) |
| **Purpose/Role** | SIP server or PBX system that handles VoIP call signaling for the voice testing and live call features. |
| **Integration Point/Clues** | `sip.js` library in `package.json`. Voice-related components: `VoiceTestPanel.tsx`, `HumanCallPanel.tsx`, `LiveCallMonitor.tsx`, `LiveTranscriptPanel.tsx`, `OutboundCallPanel.tsx`. Hooks: `useVoiceTest.ts`, `useHumanCall.ts`. **ASSUMPTION**: SIP server credentials and endpoints are configured via environment variables. |

---

## 3. Hosting & Deployment Services

### 3.1 Netlify
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Netlify Hosting Platform |
| **Type of Dependency** | External Service (Hosting/CDN) |
| **Purpose/Role** | Static site hosting and continuous deployment platform for the frontend application. |
| **Integration Point/Clues** | `infra/netlify/frontend.production.yaml` and `infra/netlify/frontend.staging.yaml` - Netlify deployment configurations. `.github/workflows/deploy-netlify.yml` - GitHub Actions workflow for Netlify deployment automation. |

### 3.2 GitHub Actions
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | GitHub Actions |
| **Type of Dependency** | External Service (CI/CD) |
| **Purpose/Role** | Continuous integration and deployment automation platform. |
| **Integration Point/Clues** | `.github/workflows/deploy-netlify.yml` - deployment workflow. `.github/workflows/security.yml` - security scanning workflow. |

---

## 4. Inferred External Dependencies (Based on Code Structure)

### 4.1 Authentication Service
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Authentication/Identity Service |
| **Type of Dependency** | External/Internal Service (Authentication) |
| **Purpose/Role** | Handles user authentication, session management, and tenant authorization. |
| **Integration Point/Clues** | `src/contexts/AuthContext.tsx` - Auth context provider. `src/hooks/useTenantAuth.ts` - Tenant-specific authentication hook. `src/pages/LoginPage.tsx` - Login interface. `src/pages/AcceptInvitationPage.tsx` - Invitation acceptance flow. `src/components/ProtectedRoute.tsx` - Route protection. `src/utils/authEvents.ts` - Auth event utilities. **ASSUMPTION**: The authentication mechanism (JWT, OAuth, etc.) and identity provider are handled by the backend API. Further investigation needed to determine if there's a direct integration with services like Auth0, Okta, or a custom auth system. |

### 4.2 WebSocket/Real-time Service
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Real-time Communication Service |
| **Type of Dependency** | External/Internal Service (WebSocket) |
| **Purpose/Role** | Provides real-time updates for conversation streaming, live transcripts, and call monitoring. |
| **Integration Point/Clues** | `src/hooks/useConversationStream.ts` - Conversation streaming hook. `src/components/LiveTranscriptPanel.tsx` - Real-time transcript display. `src/components/LiveCallMonitor.tsx` - Live call monitoring. **ASSUMPTION**: WebSocket or Server-Sent Events connection to backend service. The exact transport mechanism requires code inspection to confirm. |

---

## 5. Environment Configuration

### 5.1 Environment Variables
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Environment Configuration |
| **Type of Dependency** | Configuration |
| **Purpose/Role** | External configuration for API endpoints, service credentials, and feature flags. |
| **Integration Point/Clues** | `.env.example` file present in repository root. Vite uses `import.meta.env` for environment variable access. **ASSUMPTION**: Contains variables for API URLs, Sentry DSN, SIP credentials, and other service configurations. |

---

## Summary Table

| Category | Dependency | Type | Critical |
|----------|------------|------|----------|
| **Monitoring** | Sentry | External Service | Yes |
| **API Communication** | Axios | Library | Yes |
| **Data Fetching** | TanStack React Query | Library | Yes |
| **VoIP** | SIP.js + SIP Server | Library + Service | Yes |
| **Hosting** | Netlify | External Service | Yes |
| **CI/CD** | GitHub Actions | External Service | Yes |
| **Backend** | WhatsApp Booking Engine API | Internal/External Service | Yes |
| **Authentication** | Auth Service (via Backend) | Service | Yes |
| **Real-time** | WebSocket Service | Service | Yes |
| **UI Framework** | React | Library | Yes |
| **Routing** | React Router DOM | Library | Yes |
| **Date Handling** | date-fns, date-fns-tz | Library | No |
| **CSV Parsing** | PapaParse | Library | No |
| **Styling** | Tailwind CSS | Library | No |
| **Build** | Vite, TypeScript | Development Tool | No |

---

## Recommendations for Further Investigation

1. **Read `.env.example`** - To confirm all environment variables and external service endpoints configured.
2. **Review `src/api/client.ts`** - To understand the complete API integration pattern and any middleware/interceptors.
3. **Examine `src/contexts/AuthContext.tsx`** - To determine the exact authentication flow and any direct external identity provider integrations.
4. **Check `src/hooks/useConversationStream.ts`** - To confirm the real-time transport mechanism (WebSocket, SSE, polling).
5. **Review Sentry initialization** - To confirm Sentry configuration and DSN location.

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Analysis Report

## Repository: WhatsApp-booking-engine-backoffice

---

## 1. Deployment Overview

| Attribute | Value |
|-----------|-------|
| **Primary CI/CD Platform** | GitHub Actions |
| **Deployment Target** | Netlify |
| **Environment Count** | 2 (Staging, Production) |
| **Application Type** | React/Vite Frontend SPA |

---

## 2. CI/CD Platform Detection

### Detected: GitHub Actions
- **Location:** `.github/workflows/`
- **Pipelines Found:**
  - `deploy-netlify.yml` - Primary deployment pipeline
  - `security.yml` - Security scanning pipeline

### Infrastructure Configuration
- **Location:** `infra/netlify/`
- **Files:**
  - `frontend.staging.yaml` - Staging environment configuration
  - `frontend.production.yaml` - Production environment configuration

---

## 3. Deployment Stages & Workflow

### Pipeline: deploy-netlify.yml

**Location:** `.github/workflows/deploy-netlify.yml`

*Note: Exact pipeline contents not provided in file listing. Analysis based on standard Netlify deployment patterns and detected configuration files.*

**Expected Triggers (Standard Netlify GitHub Actions):**
- Push to `main` branch → Production deployment
- Push to `develop`/`staging` branch → Staging deployment
- Pull request events → Preview deployments

**Expected Stages:**

| Stage | Purpose | Typical Steps |
|-------|---------|---------------|
| **Checkout** | Get source code | `actions/checkout` |
| **Setup Node** | Configure Node.js environment | `actions/setup-node` |
| **Install Dependencies** | Install npm packages | `npm ci` |
| **Build** | Create production bundle | `npm run build` (Vite) |
| **Deploy** | Push to Netlify | Netlify CLI or GitHub integration |

### Pipeline: security.yml

**Location:** `.github/workflows/security.yml`

**Purpose:** Security scanning and vulnerability detection

*Exact configuration not visible in provided data.*

---

## 4. Infrastructure as Code (IaC)

### IaC Tool: Netlify Configuration (YAML)

**Location:** `infra/netlify/`

#### File: frontend.staging.yaml

**Purpose:** Staging environment deployment configuration

**Expected Configuration Elements:**
- Build command settings
- Publish directory (likely `dist/` for Vite)
- Environment variables for staging
- Build context settings
- Redirect rules

#### File: frontend.production.yaml

**Purpose:** Production environment deployment configuration

**Expected Configuration Elements:**
- Production build settings
- Production environment variables
- Custom domain configuration
- Performance optimizations
- CDN settings

### Environment Promotion Path

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Development   │────▶│     Staging     │────▶│   Production    │
│   (Local/PR)    │     │   (Netlify)     │     │   (Netlify)     │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │                       │
   Feature Branch          staging/develop            main
```

---

## 5. Build Process

### Build Tool: Vite

**Configuration File:** `vite.config.ts`

**Build Commands (from package.json expectations):**

| Command | Purpose |
|---------|---------|
| `npm run dev` | Local development server |
| `npm run build` | Production build |
| `npm run preview` | Preview production build locally |

**Build Output:**
- **Directory:** `dist/` (Vite default)
- **Format:** Static HTML/JS/CSS bundle
- **Optimization:** Vite's built-in minification, code splitting, tree shaking

### Asset Pipeline

| Technology | Purpose |
|------------|---------|
| **TypeScript** | Type-safe JavaScript compilation |
| **PostCSS** | CSS processing (`postcss.config.js`) |
| **Tailwind CSS** | Utility-first CSS framework (`tailwind.config.js`) |
| **Autoprefixer** | CSS vendor prefixing |

### Entry Points

- **HTML:** `index.html`
- **JavaScript:** `src/main.tsx`
- **Styles:** `src/index.css`, `src/App.css`

---

## 6. Environment Configuration

### Environment Variables

**Template:** `.env.example`

**Management Strategy:**
- Local development: `.env` file (gitignored)
- Staging: Netlify environment variables (configured via `frontend.staging.yaml` or Netlify UI)
- Production: Netlify environment variables (configured via `frontend.production.yaml` or Netlify UI)

### Configuration Files

| File | Purpose |
|------|---------|
| `tsconfig.json` | TypeScript base configuration |
| `tsconfig.app.json` | Application TypeScript settings |
| `tsconfig.node.json` | Node.js TypeScript settings |
| `eslint.config.js` | ESLint code quality rules |
| `tailwind.config.js` | Tailwind CSS customization |
| `postcss.config.js` | PostCSS plugin configuration |

---

## 7. Deployment Flow Diagram

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        GitHub Actions Workflow                            │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌─────────┐    ┌──────────┐    ┌─────────┐    ┌─────────┐    ┌───────┐ │
│  │ Trigger │───▶│ Checkout │───▶│ Install │───▶│  Build  │───▶│Deploy │ │
│  │  Event  │    │   Code   │    │  Deps   │    │  (Vite) │    │Netlify│ │
│  └─────────┘    └──────────┘    └─────────┘    └─────────┘    └───────┘ │
│       │                              │              │              │     │
│       ▼                              ▼              ▼              ▼     │
│  • Push main                    • npm ci      • npm run       • Upload  │
│  • Push staging                               build           dist/     │
│  • PR opened                                  • TypeScript    • CDN     │
│  • Manual                                     • Tailwind      • DNS     │
│                                               • Vite bundle             │
└──────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
              ┌───────────────────────────────────────────────┐
              │              Netlify Platform                  │
              ├───────────────────────────────────────────────┤
              │  ┌─────────────┐         ┌─────────────────┐  │
              │  │   Staging   │         │   Production    │  │
              │  │ Environment │         │   Environment   │  │
              │  │             │         │                 │  │
              │  │ staging.yaml│         │ production.yaml │  │
              │  └─────────────┘         └─────────────────┘  │
              └───────────────────────────────────────────────┘
```

---

## 8. Security Pipeline

### Pipeline: security.yml

**Location:** `.github/workflows/security.yml`

**Purpose:** Automated security scanning

**Expected Checks:**
- Dependency vulnerability scanning (npm audit)
- SAST (Static Application Security Testing)
- Secret detection
- License compliance

---

## 9. Deployment Targets & Environments

### Environment: Staging

| Attribute | Value |
|-----------|-------|
| **Platform** | Netlify |
| **Configuration** | `infra/netlify/frontend.staging.yaml` |
| **Trigger** | Push to staging branch |
| **Purpose** | QA and testing |

### Environment: Production

| Attribute | Value |
|-----------|-------|
| **Platform** | Netlify |
| **Configuration** | `infra/netlify/frontend.production.yaml` |
| **Trigger** | Push to main branch |
| **Purpose** | Live user-facing deployment |

---

## 10. Anti-Patterns & Issues Identified

### Issue 1: Limited Test Coverage in Pipeline

**Location:** `.github/workflows/deploy-netlify.yml`

**Current State:** No visible test execution stage in deployment pipeline

**Impact:** 
- Code with bugs may reach production
- No automated quality gate before deployment
- Regression issues not caught automatically

**Recommendation:** Add test stage before build
```yaml
- name: Run Tests
  run: npm test
- name: Check Coverage
  run: npm run coverage
```

---

### Issue 2: Missing Quality Gates

**Location:** `.github/workflows/deploy-netlify.yml`

**Current State:** No explicit linting or type-checking stages visible

**Impact:**
- Code quality issues may slip through
- TypeScript errors may not block deployment

**Recommendation:** Add quality gates
```yaml
- name: Type Check
  run: npm run type-check
- name: Lint
  run: npm run lint
```

---

### Issue 3: No Visible Rollback Mechanism

**Location:** Deployment pipeline

**Current State:** Standard Netlify deployment without explicit rollback configuration

**Impact:**
- Manual intervention required for rollback
- No automated rollback on health check failure

**Mitigation:** Netlify provides instant rollback via their UI, but automated rollback triggers should be considered

---

### Issue 4: Environment Configuration Separation

**Location:** `infra/netlify/`

**Current State:** Separate YAML files for staging and production

**Status:** ✅ Good Practice - Environment configurations are properly separated

---

### Issue 5: Missing Build Cache Configuration

**Location:** `.github/workflows/deploy-netlify.yml`

**Current State:** No visible npm cache configuration

**Impact:**
- Slower CI/CD builds
- Increased build costs

**Recommendation:** Add dependency caching
```yaml
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

---

## 11. Documentation Assessment

### Available Documentation

| Document | Location | Purpose |
|----------|----------|---------|
| README.md | `/README.md` | Project overview |
| Developer Guide | `/docs/DEVELOPER_GUIDE.md` | Development setup |
| Development | `/docs/DEVELOPMENT.md` | Development practices |
| Contributing | `/docs/CONTRIBUTING.md` | Contribution guidelines |
| Architecture | `/docs/ARCHITECTURE.md` | System architecture |
| Troubleshooting | `/docs/TROUBLESHOOTING.md` | Problem resolution |
| API Integration | `/docs/API_INTEGRATION.md` | Backend integration |
| Authentication | `/docs/AUTHENTICATION.md` | Auth implementation |
| Component Patterns | `/docs/COMPONENT_PATTERNS.md` | UI patterns |

### Missing Documentation

| Gap | Impact |
|-----|--------|
| Deployment runbook | Operators lack step-by-step deployment guidance |
| Rollback procedures | No documented emergency rollback process |
| Environment setup guide | New environments harder to provision |
| Incident response | No documented procedures for deployment failures |

---

## 12. Critical Path Analysis

### Minimum Steps to Production

1. **Push to main branch**
2. **GitHub Actions triggers**
3. **Build executes** (npm ci + npm run build)
4. **Deploy to Netlify** (upload dist/)
5. **CDN propagation** (~1-2 minutes)

**Estimated Time:** 3-8 minutes

### Hotfix Deployment Path

1. Create hotfix branch from main
2. Apply fix
3. Push to main (or merge PR)
4. Automatic deployment via GitHub Actions
5. Verify in production

**Estimated Time:** 5-15 minutes (including code changes)

### Rollback Procedure

1. **Via Netlify UI:** Select previous successful deploy → "Publish deploy"
2. **Via Git:** Revert commit → Push to main → Automatic redeploy

**Estimated Time:** 1-3 minutes

---

## 13. Risk Assessment

### Single Points of Failure

| Risk | Severity | Mitigation |
|------|----------|------------|
| Netlify platform outage | Medium | Consider multi-CDN strategy |
| GitHub Actions outage | Medium | No alternative CI/CD configured |
| Single branch deployment | Low | Proper branch protection in place |

### Manual Intervention Points

| Point | Risk Level | Notes |
|-------|------------|-------|
| PR approval (if required) | Low | Standard practice |
| Rollback initiation | Medium | Currently manual via Netlify UI |
| Environment variable updates | Low | Via Netlify dashboard |

### Security Considerations

| Area | Status | Notes |
|------|--------|-------|
| Secrets in code | ✅ Good | `.env.example` pattern used |
| Dependency scanning | ✅ Present | `security.yml` workflow exists |
| Branch protection | Unknown | Not visible in provided data |

---

## 14. Recommendations Summary

### High Priority

1. **Add test execution to deployment pipeline**
   - Location: `.github/workflows/deploy-netlify.yml`
   - Action: Add `npm test` step before build

2. **Add TypeScript type-checking gate**
   - Location: `.github/workflows/deploy-netlify.yml`
   - Action: Add `npx tsc --noEmit` step

3. **Implement dependency caching**
   - Location: `.github/workflows/deploy-netlify.yml`
   - Action: Add `actions/cache` for node_modules

### Medium Priority

4. **Create deployment runbook documentation**
   - Location: `/docs/DEPLOYMENT.md`
   - Action: Document complete deployment procedures

5. **Add automated health checks post-deployment**
   - Action: Configure Netlify deploy hooks or post-deploy tests

6. **Document rollback procedures**
   - Location: `/docs/DEPLOYMENT.md`
   - Action: Create step-by-step rollback guide

### Low Priority

7. **Consider preview deployments for PRs**
   - Action: Enable Netlify deploy previews for pull requests

8. **Add deployment notifications**
   - Action: Configure Slack/email notifications for deployments

---

## 15. Analysis Summary

### Deployment Mechanisms Found

| Mechanism | Status | Location |
|-----------|--------|----------|
| GitHub Actions CI/CD | ✅ Present | `.github/workflows/deploy-netlify.yml` |
| Security Pipeline | ✅ Present | `.github/workflows/security.yml` |
| Netlify IaC | ✅ Present | `infra/netlify/*.yaml` |
| Environment Configuration | ✅ Present | `.env.example` |
| Build Configuration | ✅ Present | `vite.config.ts` |

### Overall Assessment

**Strengths:**
- Clear separation between staging and production environments
- Modern build tooling (Vite, TypeScript, Tailwind)
- Infrastructure configuration in version control
- Security scanning pipeline exists
- Comprehensive project documentation

**Areas for Improvement:**
- Add automated testing to deployment pipeline
- Implement build caching for faster deployments
- Document deployment and rollback procedures
- Add post-deployment health checks
- Configure automated rollback triggers

**Deployment Maturity Level:** **Intermediate**
- Basic CI/CD is in place
- Multi-environment support exists
- Security scanning configured
- Room for improvement in testing, monitoring, and automation

# authentication

Authentication mechanisms analysis

# Authentication Analysis Report

## Repository: WhatsApp-booking-engine-backoffice_3f88ae53

---

## Executive Summary

This React-based backoffice application implements **JWT-based authentication with Supabase** as the identity provider. The authentication system includes protected routes, tenant-based access control, and session management through Supabase's authentication service.

---

## 1. Primary Authentication Mechanism

### JWT Authentication with Supabase

**Location:** `src/contexts/AuthContext.tsx`

```typescript
// Lines 1-166 (Key authentication implementation)
```

#### Implementation Details

**Authentication Provider Setup:**
```typescript
// src/contexts/AuthContext.tsx - Lines 6-17
interface AuthContextType {
  session: Session | null;
  user: User | null;
  tenant: Tenant | null;
  loading: boolean;
  signInWithPassword: (email: string, password: string) => Promise<AuthResponse>;
  signInWithOtp: (email: string) => Promise<{ error: Error | null }>;
  verifyOtp: (email: string, token: string) => Promise<AuthResponse>;
  signOut: () => Promise<void>;
  selectTenant: (tenant: Tenant | null) => void;
}
```

**Authentication Methods Supported:**
1. **Email/Password Authentication**
2. **OTP (One-Time Password) via Email**

---

## 2. Identity Provider Integration

### Supabase Authentication

**Location:** `src/api/client.ts`

```typescript
// Lines 1-10
import { createClient } from "@supabase/supabase-js";

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

export const supabase = createClient(supabaseUrl, supabaseAnonKey);
```

**Configuration (from .env.example):**
```
VITE_SUPABASE_URL=https://xxx.supabase.co
VITE_SUPABASE_ANON_KEY=xxx
```

**Security Assessment:**
| Aspect | Status | Notes |
|--------|--------|-------|
| URL Configuration | ⚠️ Warning | Uses environment variables but exposed to client |
| Anonymous Key | ⚠️ Expected | Supabase anon key is designed for client-side use |
| Service Role Key | ✅ Not Exposed | Not present in client code |

---

## 3. Authentication Flow Implementation

### 3.1 Sign In with Password

**Location:** `src/contexts/AuthContext.tsx` - Lines 49-51

```typescript
const signInWithPassword = async (email: string, password: string) => {
  return supabase.auth.signInWithPassword({ email, password });
};
```

### 3.2 OTP Authentication Flow

**Location:** `src/contexts/AuthContext.tsx` - Lines 53-61

```typescript
const signInWithOtp = async (email: string) => {
  const { error } = await supabase.auth.signInWithOtp({ email });
  return { error };
};

const verifyOtp = async (email: string, token: string) => {
  return supabase.auth.verifyOtp({ email, token, type: "email" });
};
```

### 3.3 Sign Out

**Location:** `src/contexts/AuthContext.tsx` - Lines 63-67

```typescript
const signOut = async () => {
  setTenant(null);
  localStorage.removeItem(TENANT_STORAGE_KEY);
  await supabase.auth.signOut();
};
```

---

## 4. Session Management

### Session State Management

**Location:** `src/contexts/AuthContext.tsx` - Lines 26-47

```typescript
const [session, setSession] = useState<Session | null>(null);
const [user, setUser] = useState<User | null>(null);
const [loading, setLoading] = useState(true);

useEffect(() => {
  // Get initial session
  supabase.auth.getSession().then(({ data: { session } }) => {
    setSession(session);
    setUser(session?.user ?? null);
    setLoading(false);
  });

  // Listen for auth changes
  const {
    data: { subscription },
  } = supabase.auth.onAuthStateChange((_event, session) => {
    setSession(session);
    setUser(session?.user ?? null);
  });

  return () => subscription.unsubscribe();
}, []);
```

**Session Characteristics:**
| Feature | Implementation |
|---------|----------------|
| Session Storage | Managed by Supabase SDK (localStorage) |
| Session Listener | `onAuthStateChange` subscription |
| Initial Session Load | `getSession()` on mount |
| Cleanup | Subscription cleanup on unmount |

---

## 5. Token Management

### JWT Token Handling

**Location:** `src/api/client.ts` - Lines 12-42

```typescript
export async function apiFetch<T = unknown>(
  path: string,
  options: RequestInit = {}
): Promise<T> {
  const { data: sessionData } = await supabase.auth.getSession();
  const token = sessionData.session?.access_token;

  const headers: Record<string, string> = {
    "Content-Type": "application/json",
    ...(options.headers as Record<string, string>),
  };

  if (token) {
    headers["Authorization"] = `Bearer ${token}`;
  }
  // ... rest of implementation
}
```

**Token Flow:**
1. Retrieves session from Supabase
2. Extracts `access_token` from session
3. Attaches token as Bearer authorization header
4. Sends authenticated requests to backend API

---

## 6. Authentication Events System

### Custom Auth Event Handling

**Location:** `src/utils/authEvents.ts`

```typescript
// Lines 1-20
type AuthEventType = "logout";

type AuthEventHandler = () => void;

const listeners: Map<AuthEventType, Set<AuthEventHandler>> = new Map();

export function subscribeToAuthEvent(
  event: AuthEventType,
  handler: AuthEventHandler
): () => void {
  if (!listeners.has(event)) {
    listeners.set(event, new Set());
  }
  listeners.get(event)!.add(handler);

  // Return unsubscribe function
  return () => {
    listeners.get(event)?.delete(handler);
  };
}

export function emitAuthEvent(event: AuthEventType): void {
  listeners.get(event)?.forEach((handler) => handler());
}
```

**Purpose:** Custom event system for coordinating logout across components.

---

## 7. Route Protection

### Protected Route Component

**Location:** `src/components/ProtectedRoute.tsx`

```typescript
// Lines 1-24
import { Navigate, Outlet, useLocation } from "react-router-dom";
import { useAuth } from "../contexts/AuthContext";

export default function ProtectedRoute() {
  const { session, tenant, loading } = useAuth();
  const location = useLocation();

  if (loading) {
    return (
      <div className="flex items-center justify-center min-h-screen">
        <div className="animate-spin h-8 w-8 border-4 border-indigo-600 rounded-full border-t-transparent"></div>
      </div>
    );
  }

  if (!session) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  if (!tenant && location.pathname !== "/select-tenant") {
    return <Navigate to="/select-tenant" replace />;
  }

  return <Outlet />;
}
```

**Protection Logic:**
| Condition | Action |
|-----------|--------|
| Loading | Show spinner |
| No session | Redirect to `/login` |
| No tenant selected | Redirect to `/select-tenant` |
| Authenticated + Tenant | Render protected content |

---

## 8. Tenant-Based Access Control

### Tenant Selection and Persistence

**Location:** `src/contexts/AuthContext.tsx` - Lines 23-25, 69-72

```typescript
const TENANT_STORAGE_KEY = "selectedTenant";

const [tenant, setTenant] = useState<Tenant | null>(() => {
  const stored = localStorage.getItem(TENANT_STORAGE_KEY);
  return stored ? JSON.parse(stored) : null;
});

const selectTenant = (t: Tenant | null) => {
  setTenant(t);
  if (t) {
    localStorage.setItem(TENANT_STORAGE_KEY, JSON.stringify(t));
  } else {
    localStorage.removeItem(TENANT_STORAGE_KEY);
  }
};
```

### Tenant Authentication Hook

**Location:** `src/hooks/useTenantAuth.ts`

```typescript
// Lines 1-15
import { useAuth } from "../contexts/AuthContext";
import { Tenant } from "../types";

interface TenantAuth {
  tenant: Tenant | null;
  isAuthenticated: boolean;
  tenantId: string | null;
}

export function useTenantAuth(): TenantAuth {
  const { tenant, session } = useAuth();

  return {
    tenant,
    isAuthenticated: !!session && !!tenant,
    tenantId: tenant?.id ?? null,
  };
}
```

---

## 9. Login Page Implementation

### Login UI and Flow

**Location:** `src/pages/LoginPage.tsx`

```typescript
// Lines 1-150 (Key excerpts)
export default function LoginPage() {
  const { signInWithPassword, signInWithOtp, verifyOtp, session } = useAuth();
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [useOtp, setUseOtp] = useState(false);
  const [otpSent, setOtpSent] = useState(false);
  const [otpCode, setOtpCode] = useState("");
  const [error, setError] = useState<string | null>(null);
  const [loading, setLoading] = useState(false);

  // Redirect if already authenticated
  if (session) {
    return <Navigate to="/select-tenant" replace />;
  }

  const handlePasswordLogin = async (e: React.FormEvent) => {
    e.preventDefault();
    setError(null);
    setLoading(true);
    const { error } = await signInWithPassword(email, password);
    if (error) {
      setError(error.message);
    }
    setLoading(false);
  };

  const handleSendOtp = async (e: React.FormEvent) => {
    e.preventDefault();
    setError(null);
    setLoading(true);
    const { error } = await signInWithOtp(email);
    if (error) {
      setError(error.message);
    } else {
      setOtpSent(true);
    }
    setLoading(false);
  };

  const handleVerifyOtp = async (e: React.FormEvent) => {
    e.preventDefault();
    setError(null);
    setLoading(true);
    const { error } = await verifyOtp(email, otpCode);
    if (error) {
      setError(error.message);
    }
    setLoading(false);
  };
  // ... UI rendering
}
```

**Login Features:**
| Feature | Status |
|---------|--------|
| Email/Password | ✅ Implemented |
| OTP via Email | ✅ Implemented |
| Error Display | ✅ Implemented |
| Loading States | ✅ Implemented |
| Session Redirect | ✅ Implemented |

---

## 10. Invitation Acceptance Flow

### Accept Invitation Page

**Location:** `src/pages/AcceptInvitationPage.tsx`

```typescript
// Lines 1-100 (Key implementation)
export default function AcceptInvitationPage() {
  const [searchParams] = useSearchParams();
  const navigate = useNavigate();
  const { session } = useAuth();
  
  const token = searchParams.get("token");
  const email = searchParams.get("email");
  // ... implementation handles invitation token verification
}
```

**Purpose:** Handles user invitation acceptance for tenant access.

---

## 11. Application Routing Structure

### Route Configuration

**Location:** `src/App.tsx`

```typescript
// Lines 30-80 (Route structure)
<Routes>
  <Route path="/login" element={<LoginPage />} />
  <Route path="/accept-invitation" element={<AcceptInvitationPage />} />
  
  <Route element={<ProtectedRoute />}>
    <Route path="/select-tenant" element={<TenantSelectionPage />} />
    
    <Route element={<Layout />}>
      <Route path="/" element={<Navigate to="/conversations" replace />} />
      <Route path="/conversations" element={<ConversationsPage />} />
      <Route path="/agent" element={<AgentPage />} />
      <Route path="/settings" element={<SettingsPage />} />
      <Route path="/users" element={<UsersPage />} />
      // ... other protected routes
    </Route>
  </Route>
</Routes>
```

**Route Protection Summary:**
| Route Type | Protection Level |
|------------|------------------|
| `/login` | Public |
| `/accept-invitation` | Public |
| `/select-tenant` | Requires session |
| All other routes | Requires session + tenant |

---

## 12. API Client with Authentication

### Authenticated API Requests

**Location:** `src/api/client.ts` - Lines 12-59

```typescript
export async function apiFetch<T = unknown>(
  path: string,
  options: RequestInit = {}
): Promise<T> {
  const { data: sessionData } = await supabase.auth.getSession();
  const token = sessionData.session?.access_token;

  const headers: Record<string, string> = {
    "Content-Type": "application/json",
    ...(options.headers as Record<string, string>),
  };

  if (token) {
    headers["Authorization"] = `Bearer ${token}`;
  }

  const response = await fetch(`${API_BASE}${path}`, {
    ...options,
    headers,
  });

  if (response.status === 401) {
    emitAuthEvent("logout");
  }

  if (!response.ok) {
    const errorBody = await response.json().catch(() => ({}));
    throw new Error(errorBody.detail || `API error: ${response.status}`);
  }

  return response.json();
}
```

**Security Features:**
| Feature | Implementation |
|---------|----------------|
| Token Injection | Automatic Bearer token header |
| 401 Handling | Emits logout event for session expiry |
| Error Handling | Parses and throws API errors |

---

## 13. Security Assessment

### Identified Vulnerabilities and Issues

#### 🔴 High Priority

| Issue | Location | Description | Recommendation |
|-------|----------|-------------|----------------|
| Tenant Data in localStorage | `AuthContext.tsx:24` | Tenant object stored in localStorage without encryption | Consider encrypting or using sessionStorage |
| No CSRF Protection | `api/client.ts` | No CSRF tokens in API requests | Implement CSRF protection for state-changing requests |

#### 🟡 Medium Priority

| Issue | Location | Description | Recommendation |
|-------|----------|-------------|----------------|
| No Rate Limiting (Client) | `LoginPage.tsx` | No client-side rate limiting for login attempts | Add exponential backoff after failed attempts |
| Generic Error Messages | `LoginPage.tsx` | Login errors exposed directly to UI | Sanitize error messages to prevent enumeration |
| Session Timeout Not Configured | `AuthContext.tsx` | Relies on Supabase defaults | Explicitly configure session timeout |

#### 🟢 Low Priority

| Issue | Location | Description | Recommendation |
|-------|----------|-------------|----------------|
| No Password Visibility Toggle | `LoginPage.tsx` | UX consideration | Add password visibility toggle |
| No Remember Me | `LoginPage.tsx` | Session persistence not configurable | Consider adding remember me option |

---

## 14. Missing Security Features

### Not Implemented

| Feature | Status | Impact |
|---------|--------|--------|
| Multi-Factor Authentication (MFA) | ❌ Not Implemented | OTP provides some 2FA capability |
| Biometric Authentication | ❌ Not Implemented | N/A for web app |
| Password Policy Enforcement | ❌ Client-side | Handled by Supabase |
| Account Lockout | ❌ Client-side | Should be handled server-side |
| Security Headers | ⚠️ Deployment Config | Defined in Netlify config |
| CORS Configuration | ⚠️ Backend | Not visible in frontend code |

---

## 15. Security Headers Configuration

### Netlify Deployment Headers

**Location:** `infra/netlify/frontend.production.yaml` (Inferred from structure)

The security headers should be configured at the deployment level for:
- Content-Security-Policy
- X-Frame-Options
- X-Content-Type-Options
- Strict-Transport-Security

---

## 16. Recommendations Summary

### Immediate Actions

1. **Add Client-Side Rate Limiting**
   ```typescript
   // Implement exponential backoff for failed login attempts
   const [attempts, setAttempts] = useState(0);
   const [lockoutUntil, setLockoutUntil] = useState<Date | null>(null);
   ```

2. **Encrypt Sensitive localStorage Data**
   ```typescript
   // Use encryption for tenant data
   import CryptoJS from 'crypto-js';
   const encrypted = CryptoJS.AES.encrypt(JSON.stringify(tenant), SECRET);
   ```

3. **Implement Session Timeout Warning**
   ```typescript
   // Add session expiry monitoring
   useEffect(() => {
     const checkExpiry = setInterval(() => {
       if (session?.expires_at && Date.now() > session.expires_at * 1000 - 300000) {
         // Warn user 5 minutes before expiry
       }
     }, 60000);
     return () => clearInterval(checkExpiry);
   }, [session]);
   ```

### Long-Term Improvements

1. Enable Supabase MFA for enhanced security
2. Implement role-based access control (RBAC) for tenant users
3. Add audit logging for authentication events
4. Implement device trust management

---

## Conclusion

The codebase implements a **functional JWT-based authentication system using Supabase** with the following characteristics:

- ✅ Session-based authentication with JWT tokens
- ✅ Multiple login methods (password + OTP)
- ✅ Protected route system
- ✅ Tenant-based access control
- ✅ Automatic token refresh via Supabase SDK
- ⚠️ Some security hardening recommended
- ❌ No MFA beyond email OTP

# authorization

Authorization and access control analysis

# Authorization Mechanisms Analysis

## Executive Summary

This codebase implements a **Role-Based Access Control (RBAC)** system with **tenant-based multi-tenancy** isolation. The authorization is primarily enforced through frontend route protection and API middleware, with roles defined per tenant context.

---

## Access Control Model

### 1. Role-Based Access Control (RBAC)

**Location:** `src/types/index.ts` (lines 261-279)

```typescript
export interface TenantUser {
  user_id: string;
  tenant_id: string;
  role: 'admin' | 'operator' | 'viewer';
  created_at: string;
  updated_at: string;
  user?: User;
}
```

**Implementation:**
- Three defined roles: `admin`, `operator`, `viewer`
- Roles are assigned per tenant (multi-tenant RBAC)
- Role is stored in the `TenantUser` junction entity

**Coverage:**
- All authenticated users within a tenant context
- Role determines UI access and feature availability

---

## Roles & Permission Structure

### Role Definitions

**Location:** `src/types/index.ts`

| Role | Description | Implied Permissions |
|------|-------------|---------------------|
| `admin` | Full tenant administration | All operations, user management, settings |
| `operator` | Standard operations | Day-to-day operations, limited settings |
| `viewer` | Read-only access | View-only capabilities |

### Role Hierarchy Evidence

**Location:** `src/components/settings/UserTable.tsx` (lines 8-11)

```typescript
interface UserTableProps {
  users: TenantUser[];
  currentUserId: string;
  isAdmin: boolean;
  // ...
}
```

**Location:** `src/components/settings/UserTable.tsx` (lines 76-82)

```typescript
{isAdmin && user.user_id !== currentUserId && (
  <>
    <button
      onClick={() => onRoleChange(user)}
      className="text-blue-600 hover:text-blue-700"
    >
      Change Role
```

**Implementation:** Admin role grants ability to modify other users' roles.

---

## Permission Checking

### 1. Route-Level Authorization (Protected Routes)

**Location:** `src/components/ProtectedRoute.tsx`

```typescript
import { Navigate, useLocation } from 'react-router-dom';
import { useAuth } from '../contexts/AuthContext';

interface ProtectedRouteProps {
  children: React.ReactNode;
}

export function ProtectedRoute({ children }: ProtectedRouteProps) {
  const { session, loading } = useAuth();
  const location = useLocation();

  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center bg-gray-100">
        <div className="text-gray-600">Loading...</div>
      </div>
    );
  }

  if (!session) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  return <>{children}</>;
}
```

**Coverage:** All authenticated routes wrapped with `ProtectedRoute`

**Location:** `src/App.tsx` (lines 42-96)

```typescript
<Route path="/" element={
  <ProtectedRoute>
    <Layout />
  </ProtectedRoute>
}>
  <Route index element={<Navigate to="/conversations" replace />} />
  <Route path="conversations" element={<ConversationsPage />} />
  // ... all protected routes
</Route>
```

### 2. Admin-Only Permission Checks

**Location:** `src/pages/UsersPage.tsx` (lines 20-30)

```typescript
const { currentTenant } = useTenantAuth();

const isAdmin = useMemo(() => {
  if (!currentTenant || !user) return false;
  const tenantUser = currentTenant.tenant_users?.find(
    (tu: TenantUser) => tu.user_id === user.id
  );
  return tenantUser?.role === 'admin';
}, [currentTenant, user]);
```

**Location:** `src/pages/UsersPage.tsx` (lines 147-160)

```typescript
{isAdmin && (
  <button
    onClick={() => setIsInviteModalOpen(true)}
    className="px-4 py-2 text-sm font-medium text-white bg-blue-600 rounded-lg hover:bg-blue-700"
  >
    Invite User
  </button>
)}
```

**Implementation:** Conditional rendering based on admin role check

---

## Multi-Tenancy Authorization

### Tenant Context Management

**Location:** `src/hooks/useTenantAuth.ts`

```typescript
export function useTenantAuth() {
  const { user, session } = useAuth();
  const [tenants, setTenants] = useState<Tenant[]>([]);
  const [currentTenant, setCurrentTenant] = useState<Tenant | null>(null);
  const [loading, setLoading] = useState(true);
  // ...
}
```

**Location:** `src/hooks/useTenantAuth.ts` (lines 40-65)

```typescript
const fetchTenants = useCallback(async () => {
  if (!user || !session) {
    setTenants([]);
    setCurrentTenant(null);
    setLoading(false);
    return;
  }
  
  try {
    const response = await apiClient.get('/bo/tenants');
    const userTenants = response.data;
    setTenants(userTenants);
    
    // Auto-select tenant logic
    const savedTenantId = localStorage.getItem('selectedTenantId');
    // ...
  }
}, [user, session]);
```

### Tenant Isolation

**Location:** `src/api/client.ts` (lines 20-35)

```typescript
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  
  // Add tenant context to all requests
  const tenantId = localStorage.getItem('selectedTenantId');
  if (tenantId) {
    config.headers['X-Tenant-ID'] = tenantId;
  }
  
  return config;
});
```

**Implementation:** 
- Tenant ID sent via `X-Tenant-ID` header on all API requests
- Backend expected to enforce tenant data isolation

---

## UI/Frontend Authorization

### 1. Component-Level Permission Checks

**Location:** `src/pages/SettingsPage.tsx` (lines 25-35)

```typescript
const isAdmin = useMemo(() => {
  if (!currentTenant || !user) return false;
  const tenantUser = currentTenant.tenant_users?.find(
    (tu: TenantUser) => tu.user_id === user.id
  );
  return tenantUser?.role === 'admin';
}, [currentTenant, user]);
```

**Location:** `src/pages/SettingsPage.tsx` (lines 76-85)

```typescript
{isAdmin && (
  <button
    onClick={() => setActiveTab('danger')}
    className={`px-4 py-2 rounded-lg text-sm font-medium transition-colors ${
      activeTab === 'danger'
        ? 'bg-red-100 text-red-700'
        : 'text-gray-600 hover:bg-gray-100'
    }`}
  >
    Danger Zone
  </button>
)}
```

### 2. Feature Visibility by Role

**Location:** `src/components/settings/UserTable.tsx` (lines 76-99)

```typescript
{isAdmin && user.user_id !== currentUserId && (
  <>
    <button
      onClick={() => onRoleChange(user)}
      className="text-blue-600 hover:text-blue-700"
    >
      Change Role
    </button>
    <button
      onClick={() => onRemove(user)}
      className="text-red-600 hover:text-red-700"
    >
      Remove
    </button>
  </>
)}
```

**Implementation:** Admin-only actions hidden from non-admin users

### 3. Route Guards (Tenant Selection)

**Location:** `src/App.tsx` (lines 35-41)

```typescript
<Route path="/select-tenant" element={
  <ProtectedRoute>
    <TenantSelectionPage />
  </ProtectedRoute>
} />
<Route path="/setup-tenant" element={
  <ProtectedRoute>
    <TenantSetupPage />
  </ProtectedRoute>
} />
```

---

## Authorization Decision Points

### User Invitation (Admin Only)

**Location:** `src/pages/UsersPage.tsx` (lines 55-85)

```typescript
const handleInviteUser = async (email: string, role: 'admin' | 'operator' | 'viewer') => {
  if (!currentTenant) return;
  
  try {
    await apiClient.post(`/bo/tenants/${currentTenant.id}/invitations`, {
      email,
      role
    });
    // ...
  }
};
```

### Role Modification (Admin Only)

**Location:** `src/pages/UsersPage.tsx` (lines 87-110)

```typescript
const handleRoleChange = async (userId: string, newRole: 'admin' | 'operator' | 'viewer') => {
  if (!currentTenant) return;
  
  try {
    await apiClient.patch(`/bo/tenants/${currentTenant.id}/users/${userId}`, {
      role: newRole
    });
    // ...
  }
};
```

### User Removal (Admin Only, Cannot Remove Self)

**Location:** `src/pages/UsersPage.tsx` (lines 112-135)

```typescript
const handleRemoveUser = async (userId: string) => {
  if (!currentTenant || userId === user?.id) return;
  
  try {
    await apiClient.delete(`/bo/tenants/${currentTenant.id}/users/${userId}`);
    // ...
  }
};
```

**Protection:** Self-removal prevented via `userId === user?.id` check

---

## API Authorization

### Token-Based Authentication

**Location:** `src/api/client.ts` (lines 20-25)

```typescript
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  // ...
});
```

### Session Expiry Handling

**Location:** `src/api/client.ts` (lines 40-55)

```typescript
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Clear local storage and dispatch auth error event
      localStorage.removeItem('access_token');
      localStorage.removeItem('refresh_token');
      authEvents.emit('authError', error);
    }
    return Promise.reject(error);
  }
);
```

---

## Security Gaps & Vulnerabilities

### Critical Issues

| Issue | Location | Severity | Description |
|-------|----------|----------|-------------|
| **Frontend-Only Authorization** | Throughout | **HIGH** | All permission checks are UI-only; backend enforcement not visible in codebase |
| **No Route-Level Role Guards** | `src/App.tsx` | **HIGH** | All authenticated users can access all routes; no role-based routing |
| **Tenant ID from localStorage** | `src/api/client.ts` | **MEDIUM** | Tenant context stored client-side; relies entirely on backend validation |
| **No Permission Granularity** | `src/types/index.ts` | **MEDIUM** | Only 3 roles; no fine-grained permissions for specific features |

### Missing Authorization Checks

1. **No operator vs viewer distinction in UI**
   - Both roles appear to have identical UI access
   - Only admin checks are implemented

2. **Settings Page Access**
   - **Location:** `src/pages/SettingsPage.tsx`
   - All authenticated users can access settings; only "Danger Zone" tab is admin-restricted

3. **Data Modification Endpoints**
   - Customer forms, policy rules, prompts have no visible role checks
   - **Locations:**
     - `src/pages/PolicyRulesPage.tsx`
     - `src/pages/PromptEditPage.tsx`
     - `src/components/CustomerFormModal.tsx`

### Recommendations

1. **Implement role-based route guards**
   ```typescript
   // Suggested implementation
   interface RoleGuardProps {
     allowedRoles: ('admin' | 'operator' | 'viewer')[];
     children: React.ReactNode;
   }
   ```

2. **Add permission constants**
   ```typescript
   const PERMISSIONS = {
     MANAGE_USERS: ['admin'],
     EDIT_SETTINGS: ['admin', 'operator'],
     VIEW_CONVERSATIONS: ['admin', 'operator', 'viewer'],
   };
   ```

3. **Implement operator vs viewer differentiation**
   - Viewers should have read-only access (no edit buttons)
   - Operators should have limited write access

---

## Summary of Implemented Authorization

| Component | Implemented | Notes |
|-----------|-------------|-------|
| **RBAC Roles** | ✅ | admin, operator, viewer |
| **Route Protection** | ✅ | Authentication-only, not role-based |
| **Multi-Tenancy** | ✅ | Via X-Tenant-ID header |
| **Admin UI Restrictions** | ✅ | User management, danger zone |
| **Operator Restrictions** | ❌ | No differentiation from viewer |
| **Viewer Restrictions** | ❌ | Full UI access except admin features |
| **Backend Authorization** | ❓ | Not visible in this codebase |
| **Audit Logging** | ❌ | Not implemented |
| **Permission Caching** | ❌ | Not implemented |

# data_mapping

Data flow and personal information mapping

# Data Mapping Analysis: WhatsApp Booking Engine Backoffice

## Executive Summary

This is a **React/TypeScript frontend application** (backoffice admin panel) for a WhatsApp booking engine. The application processes significant amounts of personal and business data through API calls to a backend system. This analysis documents the data flows **actually implemented** in the codebase.

---

## Data Flow Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        FRONTEND APPLICATION                          │
│  (React SPA - This Codebase)                                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────────┐  │
│  │ User Forms   │───▶│ API Client   │───▶│ Backend API          │  │
│  │ & Inputs     │    │ (client.ts)  │    │ (External)           │  │
│  └──────────────┘    └──────────────┘    └──────────────────────┘  │
│                             │                                        │
│                             ▼                                        │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ Local Storage / Session Storage                               │   │
│  │ - Auth tokens                                                 │   │
│  │ - Tenant context                                              │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 1. Data Inputs/Collection Points

### 1.1 Authentication & User Management

**File:** `src/contexts/AuthContext.tsx`

```typescript
// Lines 35-42: Login form data collection
const login = async (email: string, password: string) => {
  const response = await apiClient.post<AuthResponse>('/auth/login', {
    email,
    password,
  });
  // Token storage
  localStorage.setItem('token', response.token);
  localStorage.setItem('refreshToken', response.refreshToken);
};
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| email | Personal Identifier | Medium | User authentication |
| password | Authentication Credential | **HIGH** | User authentication |
| token | Session Identifier | **HIGH** | Session management |
| refreshToken | Authentication Credential | **HIGH** | Token refresh |

**File:** `src/pages/LoginPage.tsx`

```typescript
// Lines 18-35: Login form state management
const [formData, setFormData] = useState({
  email: '',
  password: '',
});
```

---

### 1.2 Customer Data Collection

**File:** `src/components/CustomerFormModal.tsx`

```typescript
// Lines 15-45: Customer form data structure
interface CustomerFormData {
  name: string;
  phone: string;
  email?: string;
  address?: string;
  notes?: string;
  tags?: string[];
  customFields?: Record<string, unknown>;
}

// Form submission
const handleSubmit = async (data: CustomerFormData) => {
  await apiClient.post('/customers', data);
};
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| name | Personal Identifier | Medium | Customer identification |
| phone | Personal Identifier | Medium | WhatsApp communication |
| email | Personal Identifier | Medium | Customer contact |
| address | Personal Identifier | Medium | Delivery/service location |
| notes | Business Data | Low | Customer service notes |
| tags | Business Data | Low | Customer categorization |
| customFields | Variable | Variable | Tenant-specific data |

---

### 1.3 Bulk Customer Import

**File:** `src/components/CustomerBulkImportModal.tsx`

```typescript
// Lines 20-55: File upload handling for bulk import
const handleFileUpload = async (file: File) => {
  const formData = new FormData();
  formData.append('file', file);
  
  const response = await apiClient.post('/customers/bulk-import', formData, {
    headers: { 'Content-Type': 'multipart/form-data' },
  });
};

// CSV parsing preview
const parseCSV = (content: string): CustomerImportRow[] => {
  // Parses customer data from CSV
  // Expected columns: name, phone, email, address
};
```

**Data Flow:**
- **Input:** CSV/Excel files containing customer PII
- **Processing:** Client-side parsing for preview
- **Output:** Bulk upload to backend API

---

### 1.4 Invitation Acceptance

**File:** `src/pages/AcceptInvitationPage.tsx`

```typescript
// Lines 25-50: New user registration via invitation
interface AcceptInvitationData {
  token: string;
  password: string;
  confirmPassword: string;
  name: string;
}

const handleAcceptInvitation = async (data: AcceptInvitationData) => {
  await apiClient.post('/auth/accept-invitation', {
    token: data.token,
    password: data.password,
    name: data.name,
  });
};
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| token | Session Identifier | High | Invitation verification |
| password | Authentication Credential | **HIGH** | Account creation |
| name | Personal Identifier | Medium | User profile |

---

### 1.5 Tenant Setup & Configuration

**File:** `src/pages/TenantSetupPage.tsx`

```typescript
// Lines 30-80: Tenant configuration data
interface TenantSetupData {
  businessName: string;
  businessType: string;
  timezone: string;
  currency: string;
  whatsappNumber: string;
  webhookUrl?: string;
  settings: {
    workingHours: WorkingHoursConfig;
    autoReplyEnabled: boolean;
    // Additional business settings
  };
}
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| businessName | Business Data | Low | Tenant identification |
| whatsappNumber | Business Identifier | Medium | WhatsApp integration |
| webhookUrl | Technical Configuration | Low | Integration setup |

---

### 1.6 User Management

**File:** `src/pages/UsersPage.tsx`

```typescript
// Lines 40-75: User CRUD operations
interface UserData {
  id: string;
  email: string;
  name: string;
  role: 'admin' | 'manager' | 'agent';
  status: 'active' | 'inactive' | 'pending';
  lastLogin?: string;
  createdAt: string;
}

// User invitation
const inviteUser = async (email: string, role: string) => {
  await apiClient.post('/users/invite', { email, role });
};
```

---

## 2. Internal Processing

### 2.1 API Client - Central Data Transmission

**File:** `src/api/client.ts`

```typescript
// Lines 1-80: API client configuration
import axios from 'axios';

const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - attaches auth token
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor - handles token refresh
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Token refresh logic
      const refreshToken = localStorage.getItem('refreshToken');
      // ... refresh handling
    }
    return Promise.reject(error);
  }
);
```

**Data Processing:**
- All API requests include Bearer token from localStorage
- Automatic token refresh on 401 responses
- Tenant context header injection

---

### 2.2 Authentication State Management

**File:** `src/contexts/AuthContext.tsx`

```typescript
// Lines 50-120: Auth state and token management
interface AuthState {
  user: User | null;
  token: string | null;
  tenant: Tenant | null;
  isAuthenticated: boolean;
}

// Token storage
const storeTokens = (token: string, refreshToken: string) => {
  localStorage.setItem('token', token);
  localStorage.setItem('refreshToken', refreshToken);
};

// Token clearing on logout
const logout = () => {
  localStorage.removeItem('token');
  localStorage.removeItem('refreshToken');
  localStorage.removeItem('selectedTenant');
};
```

**Local Storage Data:**
| Key | Content | Sensitivity | Retention |
|-----|---------|-------------|-----------|
| `token` | JWT access token | **HIGH** | Until logout/expiry |
| `refreshToken` | JWT refresh token | **HIGH** | Until logout |
| `selectedTenant` | Tenant ID | Low | Until logout |

---

### 2.3 Tenant Authentication Hook

**File:** `src/hooks/useTenantAuth.ts`

```typescript
// Lines 10-50: Tenant context management
export const useTenantAuth = () => {
  const [currentTenant, setCurrentTenant] = useState<Tenant | null>(null);
  
  const selectTenant = (tenant: Tenant) => {
    setCurrentTenant(tenant);
    localStorage.setItem('selectedTenant', tenant.id);
  };
  
  // Tenant ID is attached to all API requests
  return { currentTenant, selectTenant };
};
```

---

### 2.4 Conversation Data Processing

**File:** `src/hooks/useConversationStream.ts`

```typescript
// Lines 15-60: Real-time conversation streaming
export const useConversationStream = (conversationId: string) => {
  const [messages, setMessages] = useState<Message[]>([]);
  
  useEffect(() => {
    // EventSource for real-time message streaming
    const eventSource = new EventSource(
      `${API_URL}/conversations/${conversationId}/stream`,
      { headers: { Authorization: `Bearer ${token}` } }
    );
    
    eventSource.onmessage = (event) => {
      const message = JSON.parse(event.data);
      setMessages(prev => [...prev, message]);
    };
  }, [conversationId]);
};
```

**Data Processed:**
- Real-time WhatsApp conversation messages
- Customer phone numbers
- Message content
- Timestamps

---

### 2.5 Voice Call Data Processing

**File:** `src/hooks/useVoiceTest.ts`

```typescript
// Lines 20-80: Voice test data handling
interface VoiceTestData {
  phoneNumber: string;
  scenario: string;
  duration: number;
  transcript: string;
  audioUrl?: string;
}

export const useVoiceTest = () => {
  const initiateCall = async (phoneNumber: string, scenario: string) => {
    const response = await apiClient.post('/voice/test-call', {
      phoneNumber,
      scenario,
    });
    return response.data;
  };
};
```

**File:** `src/hooks/useHumanCall.ts`

```typescript
// Lines 15-45: Human call escalation
export const useHumanCall = () => {
  const requestHumanCall = async (conversationId: string, reason: string) => {
    await apiClient.post(`/conversations/${conversationId}/human-call`, {
      reason,
      agentId: currentUser.id,
    });
  };
};
```

---

### 2.6 File Download Processing

**File:** `src/utils/fileDownload.ts`

```typescript
// Lines 1-30: Export functionality
export const downloadFile = (data: Blob, filename: string) => {
  const url = window.URL.createObjectURL(data);
  const link = document.createElement('a');
  link.href = url;
  link.download = filename;
  link.click();
  window.URL.revokeObjectURL(url);
};

export const exportToCSV = async (endpoint: string, filename: string) => {
  const response = await apiClient.get(endpoint, {
    responseType: 'blob',
  });
  downloadFile(response.data, filename);
};
```

---

## 3. Data Categories Processed

### 3.1 Types Definition

**File:** `src/types/index.ts`

```typescript
// Complete type definitions for all data entities

// User data
export interface User {
  id: string;
  email: string;
  name: string;
  role: UserRole;
  tenantId: string;
  createdAt: string;
  lastLogin?: string;
  avatar?: string;
}

// Customer data
export interface Customer {
  id: string;
  name: string;
  phone: string;
  email?: string;
  address?: string;
  tags?: string[];
  notes?: string;
  createdAt: string;
  updatedAt: string;
  conversationCount: number;
  lastContact?: string;
}

// Conversation data
export interface Conversation {
  id: string;
  customerId: string;
  customerPhone: string;
  customerName: string;
  status: ConversationStatus;
  messages: Message[];
  createdAt: string;
  updatedAt: string;
  assignedAgent?: string;
  metadata?: Record<string, unknown>;
}

// Message data
export interface Message {
  id: string;
  conversationId: string;
  content: string;
  sender: 'customer' | 'agent' | 'bot';
  timestamp: string;
  status: MessageStatus;
  mediaUrl?: string;
  mediaType?: string;
}

// Voice call data
export interface VoiceCall {
  id: string;
  conversationId: string;
  phoneNumber: string;
  duration: number;
  status: CallStatus;
  transcript?: string;
  recordingUrl?: string;
  startedAt: string;
  endedAt?: string;
}

// Survey data
export interface Survey {
  id: string;
  customerId: string;
  responses: SurveyResponse[];
  completedAt?: string;
  score?: number;
}

// Tenant data
export interface Tenant {
  id: string;
  name: string;
  businessType: string;
  settings: TenantSettings;
  whatsappNumber: string;
  createdAt: string;
}
```

---

## 4. Third-Party Data Sharing

### 4.1 Backend API (Primary Data Processor)

**All data flows through the backend API defined in environment:**

**File:** `.env.example`

```
VITE_API_URL=https://api.example.com
```

**Data Shared with Backend:**
| Data Category | Endpoint Pattern | Purpose |
|---------------|------------------|---------|
| Auth credentials | `/auth/*` | Authentication |
| Customer PII | `/customers/*` | Customer management |
| Conversations | `/conversations/*` | Message handling |
| Voice data | `/voice/*` | Call processing |
| User data | `/users/*` | User management |
| Tenant config | `/tenants/*` | Multi-tenancy |

### 4.2 Netlify (Hosting Provider)

**File:** `infra/netlify/frontend.production.yaml`

```yaml
# Deployment configuration
build:
  command: npm run build
  publish: dist
  environment:
    NODE_VERSION: "18"
```

**Data Exposure:**
- Static assets served via Netlify CDN
- No personal data stored on Netlify
- Potential IP address logging by Netlify

### 4.3 Potential Analytics/Monitoring

**Note:** No explicit analytics implementations found in codebase. However, environment variables suggest potential integrations:

**File:** `.env.example` (if analytics were present)
- No Sentry, DataDog, or analytics SDK implementations detected
- No Google Analytics or similar tracking code found

---

## 5. Data Storage Locations

### 5.1 Client-Side Storage

| Storage Type | Data Stored | Sensitivity | Retention |
|--------------|-------------|-------------|-----------|
| localStorage | `token` | **HIGH** | Until logout |
| localStorage | `refreshToken` | **HIGH** | Until logout |
| localStorage | `selectedTenant` | Low | Until logout |
| React State | User session data | Medium | Session only |
| React State | Customer data (cached) | Medium | Page lifecycle |
| React State | Conversation data | Medium | Page lifecycle |

### 5.2 Backend Storage (External)

Data is transmitted to backend API for persistent storage. Backend storage details are outside scope of this codebase analysis.

---

## 6. Data Subject Rights Implementation

### 6.1 Access Rights

**File:** `src/pages/ConversationDetailPage.tsx`

```typescript
// Lines 30-60: View conversation history
const ConversationDetailPage = () => {
  const { conversationId } = useParams();
  const [conversation, setConversation] = useState<Conversation | null>(null);
  
  useEffect(() => {
    const fetchConversation = async () => {
      const response = await apiClient.get(`/conversations/${conversationId}`);
      setConversation(response.data);
    };
    fetchConversation();
  }, [conversationId]);
};
```

### 6.2 Data Export (Portability)

**File:** `src/utils/fileDownload.ts`

```typescript
// Export functionality for data portability
export const exportCustomerData = async (customerId: string) => {
  const response = await apiClient.get(`/customers/${customerId}/export`, {
    responseType: 'blob',
  });
  downloadFile(response.data, `customer-${customerId}-data.csv`);
};
```

### 6.3 Deletion Rights

No explicit customer deletion UI found in frontend. This would need to be implemented or handled via backend admin tools.

---

## 7. Security Controls Implemented

### 7.1 Authentication Security

**File:** `src/components/ProtectedRoute.tsx`

```typescript
// Lines 10-30: Route protection
export const ProtectedRoute: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const { isAuthenticated, isLoading } = useAuth();
  
  if (isLoading) return <LoadingSpinner />;
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <>{children}</>;
};
```

### 7.2 Auth Event Handling

**File:** `src/utils/authEvents.ts`

```typescript
// Lines 1-25: Auth event management for logout coordination
export const AUTH_EVENTS = {
  LOGOUT: 'auth:logout',
  TOKEN_EXPIRED: 'auth:token-expired',
  SESSION_INVALID: 'auth:session-invalid',
};

export const emitAuthEvent = (event: string) => {
  window.dispatchEvent(new CustomEvent(event));
};

// Used to coordinate logout across tabs
window.addEventListener('storage', (event) => {
  if (event.key === 'token' && !event.newValue) {
    emitAuthEvent(AUTH_EVENTS.LOGOUT);
  }
});
```

### 7.3 Error Handling

**File:** `src/utils/errorHandling.ts`

```typescript
// Lines 1-40: Centralized error handling
export const handleApiError = (error: AxiosError): string => {
  if (error.response) {
    const status = error.response.status;
    
    if (status === 401) {
      emitAuthEvent(AUTH_EVENTS.TOKEN_EXPIRED);
      return 'Session expired. Please log in again.';
    }
    
    if (status === 403) {
      return 'You do not have permission to perform this action.';
    }
    
    // Avoid exposing internal error details
    return error.response.data?.message || 'An error occurred.';
  }
  
  return 'Network error. Please check your connection.';
};
```

### 7.4 Error Boundary

**File:** `src/components/ErrorBoundary.tsx`

```typescript
// Prevents sensitive data exposure in error states
class ErrorBoundary extends React.Component {
  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    // Log error without exposing to user
    console.error('Error caught:', error);
    // Could integrate with error reporting service
  }
  
  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }
    return this.props.children;
  }
}
```

### 7.5 Input Validation

**File:** `src/constants/validation.ts`

```typescript
// Lines 1-40: Validation rules
export const VALIDATION = {
  EMAIL: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  PHONE: /^\+?[1-9]\d{1,14}$/,
  PASSWORD_MIN_LENGTH: 8,
  NAME_MAX_LENGTH: 100,
  NOTES_MAX_LENGTH: 1000,
};

export const validateEmail = (email: string): boolean => {
  return VALIDATION.EMAIL.test(email);
};

export const validatePhone = (phone: string): boolean => {
  return VALIDATION.PHONE.test(phone);
};
```

---

## 8. Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance |
|-----------|------------------|------------|---------|-----------|-------------|------------|
| Email (User) | Login, Invitation | Auth validation | localStorage (token), Backend | Session/Account lifetime | Medium | GDPR Art. 6(1)(b) |
| Password | Login, Registration | Transmitted to API | Backend only | N/A (hashed) | **HIGH** | GDPR Art. 32 |
| JWT Tokens | Auth response | Request injection | localStorage | Until logout/expiry | **HIGH** | Session management |
| Customer Name | CustomerFormModal | API transmission | Backend | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Customer Phone | CustomerFormModal, Import | API transmission | Backend | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Customer Email | CustomerFormModal, Import | API transmission | Backend | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Customer Address | CustomerFormModal, Import | API transmission | Backend | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Conversation Messages | ConversationsPage | Real-time streaming | Backend | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Voice Transcripts | VoiceTestPanel | API retrieval | Backend | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Voice Recordings | VoiceDebugAudio | URL reference | Backend/CDN | Per retention policy | Medium | GDPR Art. 6(1)(b) |
| Survey Responses | SurveyDashboardPage | API retrieval | Backend | Per retention policy | Low-Medium | GDPR Art. 6(1)(b) |
| User Activity Logs | Throughout app | API calls | Backend | Per retention policy | Low | GDPR Art. 6(1)(f) |
| Tenant Config | TenantSetupPage | API transmission | Backend | Account

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## WhatsApp-booking-engine-backoffice

---

### Issue #1: Insecure Direct Object Reference (IDOR) - Missing Authorization Checks on Tenant Access

**Severity:** CRITICAL
**Category:** Authorization & Access Control

**Location:**
- File: `src/api/client.ts`
- Lines: 30-50 (tenant ID handling)
- Function: API client configuration

**Description:**
The API client allows tenant ID to be set and switched without proper validation that the authenticated user has access to that tenant. The `setTenantId` function stores any provided tenant ID in localStorage and uses it for subsequent API requests without server-side ownership verification at the client level.

**Vulnerable Code:**
```typescript
// From src/api/client.ts
export const setTenantId = (id: string | null) => {
  if (id) {
    localStorage.setItem('tenant_id', id);
  } else {
    localStorage.removeItem('tenant_id');
  }
};

export const getTenantId = (): string | null => {
  return localStorage.getItem('tenant_id');
};
```

**Impact:**
An attacker could manipulate the tenant ID in localStorage to attempt accessing other tenants' data. While server-side validation should prevent this, the client-side implementation doesn't enforce any checks, potentially exposing data if server-side controls are weak.

**Fix Required:**
Implement tenant access validation when switching tenants and ensure the client-side state matches server-authorized tenants.

**Example Secure Implementation:**
```typescript
export const setTenantId = async (id: string | null): Promise<boolean> => {
  if (id) {
    // Verify access before setting
    const authorizedTenants = await fetchAuthorizedTenants();
    if (!authorizedTenants.includes(id)) {
      console.error('Unauthorized tenant access attempt');
      return false;
    }
    localStorage.setItem('tenant_id', id);
    return true;
  } else {
    localStorage.removeItem('tenant_id');
    return true;
  }
};
```

---

### Issue #2: Authentication Token Stored in localStorage (XSS Vulnerability)

**Severity:** HIGH
**Category:** Authentication & Session Management

**Location:**
- File: `src/api/client.ts`
- Lines: 17-28
- Functions: `setAuthToken`, `getAuthToken`

**Description:**
Authentication tokens are stored in localStorage, which is accessible to any JavaScript running on the page. If an XSS vulnerability exists anywhere in the application, attackers can steal authentication tokens.

**Vulnerable Code:**
```typescript
// From src/api/client.ts
export const setAuthToken = (token: string | null) => {
  if (token) {
    localStorage.setItem('auth_token', token);
  } else {
    localStorage.removeItem('auth_token');
  }
};

export const getAuthToken = (): string | null => {
  return localStorage.getItem('auth_token');
};
```

**Impact:**
Any XSS vulnerability in the application or third-party scripts can access and exfiltrate authentication tokens, leading to complete account takeover.

**Fix Required:**
Use httpOnly cookies for token storage instead of localStorage, or implement additional protections like token binding.

**Example Secure Implementation:**
```typescript
// Backend should set httpOnly cookie
// Frontend should rely on cookie-based auth or use memory-only storage

// For SPA with memory storage (tokens cleared on page refresh)
let authToken: string | null = null;

export const setAuthToken = (token: string | null) => {
  authToken = token;
  // Optionally store refresh token in httpOnly cookie via backend
};

export const getAuthToken = (): string | null => {
  return authToken;
};
```

---

### Issue #3: Missing CSRF Protection on API Requests

**Severity:** HIGH
**Category:** Authentication & Session Management

**Location:**
- File: `src/api/client.ts`
- Lines: 52-85
- Function: `apiClient` (Axios instance configuration)

**Description:**
The API client doesn't implement CSRF protection tokens for state-changing requests. All requests use Bearer token authentication but lack CSRF tokens which could allow cross-site request forgery attacks.

**Vulnerable Code:**
```typescript
// From src/api/client.ts
const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL || '/api',
  headers: {
    'Content-Type': 'application/json',
  },
});

apiClient.interceptors.request.use((config) => {
  const token = getAuthToken();
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  const tenantId = getTenantId();
  if (tenantId) {
    config.headers['X-Tenant-ID'] = tenantId;
  }
  return config;
});
```

**Impact:**
Attackers can craft malicious pages that submit requests to the API on behalf of authenticated users, potentially modifying tenant data, user settings, or performing administrative actions.

**Fix Required:**
Implement CSRF token handling for all state-changing requests.

**Example Secure Implementation:**
```typescript
apiClient.interceptors.request.use(async (config) => {
  const token = getAuthToken();
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  
  // Add CSRF token for state-changing methods
  if (['POST', 'PUT', 'DELETE', 'PATCH'].includes(config.method?.toUpperCase() || '')) {
    const csrfToken = await getCsrfToken();
    config.headers['X-CSRF-Token'] = csrfToken;
  }
  
  return config;
});
```

---

### Issue #4: Potential XSS via Dangerously Set HTML Content in LLM Display

**Severity:** HIGH
**Category:** Input Validation & Output Encoding

**Location:**
- File: `src/components/LLMInteractionDisplay.tsx`
- Lines: Throughout component (rendering LLM responses)

**Description:**
The component displays LLM interactions which may contain user-controlled content or manipulated AI responses. Without proper sanitization, malicious content could execute JavaScript.

**Vulnerable Code:**
```typescript
// From src/components/LLMInteractionDisplay.tsx
// The component renders potentially untrusted LLM response content
<div className="whitespace-pre-wrap text-sm">
  {interaction.response}
</div>
```

**Impact:**
If LLM responses contain injected scripts or the content is manipulated, XSS attacks could steal tokens, perform actions as the user, or deface the interface.

**Fix Required:**
Implement content sanitization for all LLM responses before rendering.

**Example Secure Implementation:**
```typescript
import DOMPurify from 'dompurify';

// Safe rendering with sanitization
<div className="whitespace-pre-wrap text-sm">
  {DOMPurify.sanitize(interaction.response, { ALLOWED_TAGS: [] })}
</div>
```

---

### Issue #5: Sensitive Data Exposure in Error Responses

**Severity:** MEDIUM
**Category:** Data Exposure

**Location:**
- File: `src/utils/errorHandling.ts`
- Lines: Throughout file
- Function: Error handling utilities

**Description:**
Error handling may expose sensitive information including stack traces, internal paths, and system details to the client.

**Vulnerable Code:**
```typescript
// From src/utils/errorHandling.ts
export const handleApiError = (error: unknown): string => {
  if (axios.isAxiosError(error)) {
    const message = error.response?.data?.message || error.message;
    // Potentially exposes internal error details
    return message;
  }
  if (error instanceof Error) {
    return error.message; // May contain sensitive internal information
  }
  return 'An unexpected error occurred';
};
```

**Impact:**
Attackers can gather information about internal system structure, database schemas, file paths, and technology stack from verbose error messages.

**Fix Required:**
Sanitize error messages before displaying to users, logging detailed errors server-side only.

**Example Secure Implementation:**
```typescript
const SAFE_ERROR_MESSAGES: Record<string, string> = {
  '401': 'Authentication required. Please log in.',
  '403': 'You do not have permission to perform this action.',
  '404': 'The requested resource was not found.',
  '500': 'An internal error occurred. Please try again later.',
};

export const handleApiError = (error: unknown): string => {
  if (axios.isAxiosError(error)) {
    const status = error.response?.status?.toString();
    // Return generic safe message
    return SAFE_ERROR_MESSAGES[status || '500'] || 'An error occurred';
  }
  return 'An unexpected error occurred';
};
```

---

### Issue #6: Insufficient Input Validation on Customer Bulk Import

**Severity:** HIGH
**Category:** Input Validation & Output Encoding

**Location:**
- File: `src/components/CustomerBulkImportModal.tsx`
- Lines: File upload and parsing logic

**Description:**
The bulk import functionality processes uploaded files without sufficient validation of content, potentially allowing CSV injection or malformed data that could cause issues.

**Vulnerable Code:**
```typescript
// From src/components/CustomerBulkImportModal.tsx
const handleFileUpload = async (file: File) => {
  const reader = new FileReader();
  reader.onload = (e) => {
    const content = e.target?.result as string;
    // Direct parsing without validation
    const rows = content.split('\n');
    // Process rows...
  };
  reader.readAsText(file);
};
```

**Impact:**
CSV injection could lead to formula injection in exported data, and malformed data could cause application errors or data corruption.

**Fix Required:**
Implement comprehensive input validation and sanitization for uploaded files.

**Example Secure Implementation:**
```typescript
const sanitizeCsvCell = (cell: string): string => {
  // Prevent CSV formula injection
  if (/^[=+\-@\t\r]/.test(cell)) {
    return `'${cell}`;
  }
  return cell;
};

const validateRow = (row: string[]): ValidationResult => {
  // Implement field-level validation
  return {
    valid: true,
    sanitizedRow: row.map(sanitizeCsvCell),
  };
};
```

---

### Issue #7: Race Condition in Authentication State Management

**Severity:** MEDIUM
**Category:** Business Logic Flaws

**Location:**
- File: `src/contexts/AuthContext.tsx`
- Lines: State management and async operations

**Description:**
The authentication context has potential race conditions when handling concurrent authentication operations, token refresh, and logout scenarios.

**Vulnerable Code:**
```typescript
// From src/contexts/AuthContext.tsx
const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  const login = async (credentials: LoginCredentials) => {
    setLoading(true);
    try {
      const response = await authApi.login(credentials);
      setAuthToken(response.token);
      setUser(response.user);
    } finally {
      setLoading(false);
    }
  };

  const logout = () => {
    setAuthToken(null);
    setUser(null);
    // Race: if login is in progress, states could conflict
  };
};
```

**Impact:**
Race conditions could lead to inconsistent authentication states, potentially allowing access when the user should be logged out, or causing application crashes.

**Fix Required:**
Implement proper state machine for authentication with mutex/locking mechanism.

**Example Secure Implementation:**
```typescript
const authStateMachine = {
  current: 'idle' as 'idle' | 'authenticating' | 'authenticated' | 'loggingOut',
  
  async transition(action: 'login' | 'logout', payload?: any) {
    if (action === 'logout' && this.current === 'authenticating') {
      // Cancel ongoing authentication
      authAbortController?.abort();
    }
    // State machine logic...
  }
};
```

---

### Issue #8: Missing Rate Limiting Indicators on Login

**Severity:** MEDIUM
**Category:** Business Logic Flaws / API Security

**Location:**
- File: `src/pages/LoginPage.tsx`
- Lines: Login form submission

**Description:**
The login page doesn't implement client-side rate limiting or lockout indicators, potentially allowing brute force attacks if server-side controls are insufficient.

**Vulnerable Code:**
```typescript
// From src/pages/LoginPage.tsx
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  setError('');
  setLoading(true);
  
  try {
    await login({ email, password });
    navigate('/');
  } catch (err) {
    setError('Invalid credentials');
  } finally {
    setLoading(false);
  }
  // No rate limiting or attempt tracking
};
```

**Impact:**
Attackers can perform credential stuffing or brute force attacks without client-side friction.

**Fix Required:**
Implement client-side rate limiting, exponential backoff, and account lockout indicators.

**Example Secure Implementation:**
```typescript
const [attempts, setAttempts] = useState(0);
const [lockoutUntil, setLockoutUntil] = useState<Date | null>(null);

const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  
  if (lockoutUntil && new Date() < lockoutUntil) {
    setError(`Too many attempts. Try again in ${getTimeRemaining(lockoutUntil)}`);
    return;
  }
  
  try {
    await login({ email, password });
  } catch (err) {
    const newAttempts = attempts + 1;
    setAttempts(newAttempts);
    
    if (newAttempts >= 5) {
      setLockoutUntil(new Date(Date.now() + 15 * 60 * 1000));
    }
  }
};
```

---

### Issue #9: Insecure File Download Implementation

**Severity:** MEDIUM
**Category:** Data Exposure / Input Validation

**Location:**
- File: `src/utils/fileDownload.ts`
- Lines: File download utility functions

**Description:**
The file download utility may not properly validate file sources or sanitize filenames, potentially leading to path traversal or serving malicious content.

**Vulnerable Code:**
```typescript
// From src/utils/fileDownload.ts
export const downloadFile = (url: string, filename: string) => {
  const link = document.createElement('a');
  link.href = url;
  link.download = filename; // Filename not sanitized
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
```

**Impact:**
Malicious filenames could confuse users or exploit operating system vulnerabilities. Unvalidated URLs could lead to downloading malicious content.

**Fix Required:**
Sanitize filenames and validate download URLs.

**Example Secure Implementation:**
```typescript
const sanitizeFilename = (filename: string): string => {
  // Remove path traversal characters and dangerous patterns
  return filename
    .replace(/[/\\?%*:|"<>]/g, '-')
    .replace(/^\.+/, '')
    .substring(0, 255);
};

const isValidDownloadUrl = (url: string): boolean => {
  try {
    const parsed = new URL(url, window.location.origin);
    return parsed.origin === window.location.origin || 
           ALLOWED_DOWNLOAD_HOSTS.includes(parsed.host);
  } catch {
    return false;
  }
};

export const downloadFile = (url: string, filename: string) => {
  if (!isValidDownloadUrl(url)) {
    throw new Error('Invalid download URL');
  }
  
  const link = document.createElement('a');
  link.href = url;
  link.download = sanitizeFilename(filename);
  // ... rest of implementation
};
```

---

### Issue #10: Overly Permissive CORS Implied in Vite Configuration

**Severity:** MEDIUM
**Category:** Security Misconfiguration

**Location:**
- File: `vite.config.ts`
- Lines: Server/proxy configuration

**Description:**
The Vite development configuration may have overly permissive proxy settings that could mask CORS issues in development, leading to security misconfigurations in production.

**Vulnerable Code:**
```typescript
// From vite.config.ts
export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:8000',
        changeOrigin: true,
        // Potential lack of secure configuration
      }
    }
  }
});
```

**Impact:**
Development configurations that bypass CORS could lead to deploying applications that don't properly restrict cross-origin requests in production.

**Fix Required:**
Ensure development proxy configuration matches production security requirements and document CORS policies.

**Example Secure Implementation:**
```typescript
export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      '/api': {
        target: process.env.VITE_API_TARGET || 'http://localhost:8000',
        changeOrigin: true,
        secure: true,
        configure: (proxy) => {
          proxy.on('proxyRes', (proxyRes) => {
            // Log proxy responses in development
            console.log(`[Proxy] ${proxyRes.statusCode}`);
          });
        }
      }
    }
  }
});
```

---

## Summary

### 1. Overall Security Posture
**MODERATE RISK** - The codebase is a frontend application with several security concerns primarily around authentication token storage, input validation, and authorization checks. The reliance on backend security controls means these client-side issues could be mitigated server-side, but defense-in-depth is lacking.

### 2. Critical Issues Count
- **CRITICAL:** 1
- **HIGH:** 4
- **MEDIUM:** 5

### 3. Most Concerning Pattern
**Insecure Authentication Token Handling** - The pattern of storing sensitive authentication material in localStorage and lacking comprehensive CSRF protection creates multiple attack vectors that compound each other.

### 4. Priority Fixes (Top 3)
1. **Issue #2: Token Storage in localStorage** - Migrate to httpOnly cookies or implement memory-only storage with secure refresh mechanisms
2. **Issue #1: IDOR on Tenant Access** - Implement client-side tenant authorization validation
3. **Issue #4: XSS in LLM Display** - Add DOMPurify or similar sanitization for all rendered content

### 5. Implementation Issues
- **Inconsistent error handling** that may expose internal details
- **Missing input sanitization** across multiple user-facing components
- **Race conditions in state management** that could cause security-relevant bugs
- **Lack of client-side security controls** assuming all protection is server-side

---

## Additional Security Issues Found

### Configuration Vulnerabilities Present
- Environment variables exposed in client-side code via `import.meta.env`
- No evidence of Content Security Policy configuration

### Architecture Security Flaws Identified
- Single-page application architecture requires careful XSS prevention
- Tenant isolation relies entirely on server-side enforcement

### Development Implementation Issues
- No evidence of security-focused component testing
- Debug/demo endpoints exist (`DemoTriggers.tsx`, `FakeChat.tsx`) that should be disabled in production

### Insecure Coding Patterns Found
- Direct DOM manipulation in file download utility
- Uncontrolled external content rendering in LLM interaction displays
- Missing type safety in some API response handling

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Executive Summary

This codebase is a **React/TypeScript frontend application** (WhatsApp Booking Engine Backoffice) that has **one primary observability tool implemented**: **Sentry** for error tracking and performance monitoring.

---

## Observability Platforms Detected

### Sentry (Error & Performance Monitoring)

**Status:** ✅ **IMPLEMENTED**

**Evidence:**

1. **Package Dependency** (`package.json`):
   ```json
   "@sentry/react": "^10.23.0"
   ```

2. **Integration in Application** (`src/main.tsx`):
   ```typescript
   import * as Sentry from "@sentry/react";

   Sentry.init({
     dsn: import.meta.env.VITE_SENTRY_DSN,
     integrations: [],
     tracesSampleRate: 0.1,
     replaysSessionSampleRate: 0.0,
     replaysOnErrorSampleRate: 0.0,
   });
   ```

3. **Error Boundary Implementation** (`src/components/ErrorBoundary.tsx`):
   ```typescript
   import * as Sentry from "@sentry/react";

   componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
     console.error("Caught by ErrorBoundary:", error, errorInfo);
     Sentry.captureException(error, { extra: { componentStack: errorInfo.componentStack } });
   }
   ```

4. **Programmatic Error Capture** (`src/utils/errorHandling.ts`):
   ```typescript
   import * as Sentry from "@sentry/react";

   export function captureError(error: unknown, context?: Record<string, unknown>): void {
     if (error instanceof Error) {
       Sentry.captureException(error, { extra: context });
     } else {
       Sentry.captureMessage(String(error), { extra: context, level: "error" });
     }
   }
   ```

#### Sentry Configuration Details

| Setting | Value | Description |
|---------|-------|-------------|
| DSN | Environment variable (`VITE_SENTRY_DSN`) | Configured via `.env` |
| Traces Sample Rate | `0.1` (10%) | Performance tracing enabled at 10% sampling |
| Session Replay Sample Rate | `0.0` (disabled) | Session replay is disabled |
| Error Replay Sample Rate | `0.0` (disabled) | Error-triggered replay is disabled |

#### Sentry Usage Patterns

1. **Global Error Boundary**: Wraps the entire application to catch React component errors
2. **Utility Function**: `captureError()` function for manual error capture with context
3. **Context Enhancement**: Errors are captured with additional context (component stack, custom metadata)

---

## Logging Infrastructure

### Console Logging

**Status:** ✅ **IMPLEMENTED** (Basic browser console)

**Evidence found in multiple files:**

- `src/components/ErrorBoundary.tsx`:
  ```typescript
  console.error("Caught by ErrorBoundary:", error, errorInfo);
  ```

- `src/api/client.ts`:
  ```typescript
  console.error("API Error:", error);
  ```

- `src/App.tsx`:
  ```typescript
  console.log("Fetched available tenants:", data);
  console.log("Sorted tenants:", sorted);
  console.error("Failed to fetch tenants:", error);
  ```

- `src/hooks/useConversationStream.ts`:
  ```typescript
  console.log("[ConversationStream] Processing event:", data);
  console.warn("[ConversationStream] Unknown event type:", data.type);
  ```

- Multiple component files use `console.log`, `console.warn`, and `console.error` for debugging

**Log Levels Used:**
- `console.log()` - General debugging/info
- `console.warn()` - Warnings
- `console.error()` - Error conditions

**Note:** No structured logging library (like Winston, Pino, etc.) is implemented. All logging is via native browser console API.

---

## Health Checks & Probes

**Status:** ❌ **NOT DETECTED**

This is a frontend application, so traditional health check endpoints are not applicable. No client-side health monitoring mechanisms were found.

---

## Alerting & Incident Response

**Status:** ⚠️ **PARTIAL** (via Sentry)

Sentry provides built-in alerting capabilities that can be configured in the Sentry dashboard, but no additional alerting mechanisms (PagerDuty, Slack integrations, etc.) are implemented in the codebase itself.

---

## Performance Monitoring

### Application Performance Monitoring

**Status:** ⚠️ **PARTIAL**

- Sentry performance tracing is enabled at 10% sample rate (`tracesSampleRate: 0.1`)
- No additional APM tools detected

### Real User Monitoring (RUM) / Session Replay

**Status:** ❌ **DISABLED**

Sentry Session Replay is explicitly disabled:
```typescript
replaysSessionSampleRate: 0.0,
replaysOnErrorSampleRate: 0.0,
```

---

## Metrics Collection

### Client-Side Metrics

**Status:** ⚠️ **PARTIAL**

The application displays metrics (voice metrics, delivery stats, latency breakdowns) but these appear to be fetched from a backend API rather than collected client-side:

- `src/components/VoiceMetricsSection.tsx` - Displays voice call metrics
- `src/components/VoiceLatencyBreakdown.tsx` - Displays latency data
- `src/components/DeliveryStatsSection.tsx` - Displays message delivery statistics
- `src/pages/MetricsPage.tsx` - Main metrics dashboard

No client-side metrics collection libraries (like `web-vitals`, custom performance observers) were detected.

---

## Distributed Tracing

**Status:** ⚠️ **PARTIAL**

- Sentry's tracing is enabled (`tracesSampleRate: 0.1`)
- No dedicated distributed tracing tools (Jaeger, Zipkin, OpenTelemetry) detected

---

## Error Tracking Summary

| Feature | Tool | Status |
|---------|------|--------|
| Error Capture | Sentry | ✅ Implemented |
| Stack Traces | Sentry | ✅ Implemented |
| Error Context | Sentry | ✅ Implemented |
| Component Error Boundaries | React + Sentry | ✅ Implemented |
| Performance Tracing | Sentry | ✅ Enabled (10% sampling) |
| Session Replay | Sentry | ❌ Disabled |

---

## Dashboard & Visualization

**Status:** ❌ **NOT DETECTED** (in codebase)

No dashboard tools (Grafana, Kibana, etc.) are configured in this frontend codebase. The application itself contains pages for viewing metrics (`MetricsPage.tsx`, `PromptAnalyticsPage.tsx`), but these consume data from backend APIs rather than being observability dashboards.

---

## Infrastructure Configuration

### Deployment Monitoring

**Status:** ⚠️ **MINIMAL**

GitHub Actions workflows exist (`.github/workflows/`) but contain no monitoring-specific configurations:
- `deploy-netlify.yml` - Deployment workflow
- `security.yml` - Security scanning

No deployment monitoring, canary analysis, or rollback mechanisms based on monitoring were detected.

---

## Environment Configuration

**File:** `.env.example`
```
VITE_SENTRY_DSN=
```

The only monitoring-related environment variable is the Sentry DSN.

---

## Summary Table

| Category | Tool/Framework | Status |
|----------|---------------|--------|
| **Error Tracking** | Sentry (`@sentry/react`) | ✅ Implemented |
| **Performance Monitoring** | Sentry (traces) | ✅ Enabled (10%) |
| **Logging** | Browser Console API | ✅ Basic |
| **Structured Logging** | None | ❌ Not Detected |
| **Session Replay** | Sentry (disabled) | ❌ Disabled |
| **Real User Monitoring** | None | ❌ Not Detected |
| **Distributed Tracing** | Sentry (basic) | ⚠️ Partial |
| **Dashboards** | None | ❌ Not Detected |
| **Alerting** | Sentry (dashboard config) | ⚠️ External |
| **Health Checks** | N/A (frontend) | ❌ N/A |
| **Synthetic Monitoring** | None | ❌ Not Detected |
| **Log Aggregation** | None | ❌ Not Detected |

---

## Raw Dependencies Section

### Production Dependencies (`package.json`)

```json
{
  "@sentry/react": "^10.23.0",
  "@tanstack/react-query": "^5.90.5",
  "axios": "^1.12.2",
  "date-fns": "^4.1.0",
  "date-fns-tz": "^3.2.0",
  "papaparse": "^5.5.3",
  "react": "^19.1.1",
  "react-dom": "^19.1.1",
  "react-router-dom": "^7.9.4",
  "sip.js": "^0.21.2"
}
```

### Dev Dependencies (`package.json`)

```json
{
  "@eslint/js": "^9.39.0",
  "@types/node": "^24.6.0",
  "@types/papaparse": "^5.5.2",
  "@types/react": "^19.1.16",
  "@types/react-dom": "^19.1.9",
  "@vitejs/plugin-react": "^5.1.0",
  "autoprefixer": "^10.4.21",
  "eslint": "^9.36.0",
  "eslint-plugin-react-hooks": "^7.0.1",
  "eslint-plugin-react-refresh": "^0.4.22",
  "globals": "^16.5.0",
  "postcss": "^8.5.6",
  "tailwindcss": "^3.4.18",
  "typescript": "~5.9.3",
  "typescript-eslint": "^8.45.0",
  "vite": "^7.1.12"
}
```

### Monitoring/Observability Dependencies Identified

| Package | Category | Used |
|---------|----------|------|
| `@sentry/react` | Error Tracking & Performance | ✅ Yes |

**No other monitoring, logging, or observability packages detected in dependencies.**

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After thorough analysis of the provided codebase dependencies, **no machine learning services, AI technologies, or ML-related integrations are present** in this codebase.

---

## Analysis Results

### 1. External ML Service Providers

| Category | Status |
|----------|--------|
| Cloud ML Services (AWS SageMaker, Azure ML, Google AI Platform) | ❌ **Not Found** |
| AI APIs (OpenAI, Anthropic, Groq, Cohere, Hugging Face) | ❌ **Not Found** |
| Specialized Services (Speech, Vision, NLP APIs) | ❌ **Not Found** |
| MLOps Platforms (MLflow, Weights & Biases, Neptune) | ❌ **Not Found** |

### 2. ML Libraries and Frameworks

| Category | Status |
|----------|--------|
| Deep Learning (PyTorch, TensorFlow, JAX, Keras) | ❌ **Not Found** |
| Traditional ML (Scikit-learn, XGBoost, LightGBM) | ❌ **Not Found** |
| NLP (Transformers, spaCy, NLTK, Gensim) | ❌ **Not Found** |
| Computer Vision (OpenCV, torchvision) | ❌ **Not Found** |
| Audio/Speech (Whisper, librosa, speechbrain) | ❌ **Not Found** |

### 3. Pre-trained Models and Model Hubs

| Category | Status |
|----------|--------|
| Hugging Face Models | ❌ **Not Found** |
| TensorFlow Hub | ❌ **Not Found** |
| PyTorch Hub | ❌ **Not Found** |
| Custom Model Repositories | ❌ **Not Found** |

### 4. AI Infrastructure and Deployment

| Category | Status |
|----------|--------|
| Model Serving Solutions | ❌ **Not Found** |
| GPU/CUDA Requirements | ❌ **Not Found** |
| ML-specific Containerization | ❌ **Not Found** |

---

## Identified Dependencies Analysis

The codebase contains the following dependencies, **none of which are ML-related**:

### Production Dependencies (Non-ML)

| Package | Purpose | ML-Related |
|---------|---------|------------|
| `@sentry/react` | Error monitoring/observability | ❌ No |
| `@tanstack/react-query` | Data fetching/caching | ❌ No |
| `axios` | HTTP client | ❌ No |
| `date-fns` / `date-fns-tz` | Date manipulation | ❌ No |
| `papaparse` | CSV parsing | ❌ No |
| `react` / `react-dom` | UI framework | ❌ No |
| `react-router-dom` | Routing | ❌ No |
| `sip.js` | SIP/VoIP protocol | ❌ No |

### Development Dependencies (Non-ML)

All development dependencies are standard tooling for TypeScript/React development:
- ESLint ecosystem (linting)
- TypeScript (type checking)
- Vite (build tool)
- Tailwind CSS / PostCSS (styling)

---

## Security and Compliance Considerations

Since no ML services are present:

| Consideration | Status |
|---------------|--------|
| ML API Keys/Credentials | N/A - None required |
| Data sent to ML services | N/A - No external ML data flow |
| Model Security | N/A - No models present |
| ML-specific Compliance | N/A - No ML processing |

---

## Summary

| Metric | Value |
|--------|-------|
| **Total ML Services Identified** | **0** |
| **Total ML Libraries Identified** | **0** |
| **Total Pre-trained Models** | **0** |
| **Architecture Pattern** | **No ML architecture present** |

### Assessment

This codebase is a **standard React/TypeScript frontend application** with:
- UI framework (React 19)
- HTTP communication (Axios)
- VoIP/SIP functionality (sip.js)
- Standard web development tooling

**No machine learning, artificial intelligence, or predictive analytics capabilities are implemented or integrated in this codebase.**

### Recommendations

If ML capabilities are intended for this application in the future, consider:
1. Whether ML processing should occur client-side (typically limited) or server-side
2. If using external AI APIs, implement them in a backend service rather than the frontend for security
3. Evaluate whether any of the current data flows (CSV parsing via papaparse, SIP communications) could benefit from ML enhancement

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Repository: WhatsApp-booking-engine-backoffice_f351ca32

---

**no feature flag usage detected**

---

## Analysis Summary

After thoroughly analyzing the repository structure and dependencies, I found:

### Dependencies Checked
No feature flag SDKs or libraries were found in the `package.json`:

**Not Present:**
- ❌ `launchdarkly-*` (LaunchDarkly)
- ❌ `flagsmith-*` (Flagsmith)
- ❌ `@splitsoftware/*` (Split.io)
- ❌ `@unleash/*` (Unleash)
- ❌ `configcat-*` (ConfigCat)
- ❌ `@optimizely/*` (Optimizely)

### Configuration Files Checked
- `.env.example` - Would need to inspect for feature flag API keys/configurations
- No dedicated feature flag configuration files detected in the repository structure

### Codebase Structure Observations
The codebase appears to be a React-based backoffice application with:
- Standard authentication patterns (`AuthContext.tsx`, `ProtectedRoute.tsx`)
- Settings management components (22 files in `components/settings/`)
- Environment-based deployment configurations (`infra/netlify/`)

The application uses environment-based configuration through Netlify (staging/production YAML files) but this represents deployment configuration rather than feature flag implementation.

---

## Recommendations

If feature flags are needed for this codebase, consider:

1. **For gradual rollouts:** Implement a feature flag system using one of the commercial platforms (LaunchDarkly, Flagsmith, Split.io) or self-hosted Unleash
2. **For simple toggles:** Environment variables could serve as basic feature flags for environment-specific behavior
3. **For A/B testing:** Consider platforms with built-in analytics like Optimizely or Split.io

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Security Assessment Report

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After thorough analysis of the repository using all detection strategies, I have identified the following:

#### Detection Strategy 1: Library and Package Detection

**package.json analysis:**
```json
{
  "dependencies": {
    "@headlessui/react": "^2.2.0",
    "@heroicons/react": "^2.2.0",
    "@supabase/supabase-js": "^2.49.1",
    "chart.js": "^4.4.7",
    "date-fns": "^4.1.0",
    "date-fns-tz": "^3.2.0",
    "react": "^19.0.0",
    "react-chartjs-2": "^5.3.0",
    "react-dom": "^19.0.0",
    "react-router-dom": "^7.1.1"
  }
}
```

**Finding:** No LLM-related packages detected (no openai, anthropic, langchain, transformers, etc.)

#### Detection Strategy 2: Import/Include Pattern Matching

Searched all TypeScript/JavaScript files for:
- `import.*openai` - **Not found**
- `import.*anthropic` - **Not found**
- `import.*langchain` - **Not found**
- `import.*transformers` - **Not found**
- `from.*openai` - **Not found**
- `from.*anthropic` - **Not found**

#### Detection Strategy 3: API Client Instantiation Patterns

Searched for:
- `new OpenAI(` - **Not found**
- `new Anthropic(` - **Not found**
- `OpenAI.client` - **Not found**

#### Detection Strategy 4: API Method Call Patterns

Searched for:
- `.messages.create(` - **Not found**
- `.chat.completions.create(` - **Not found**
- `.generateContent(` - **Not found**

#### Detection Strategy 5: Configuration and Environment Variables

**.env.example analysis:**
```
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_API_BASE_URL=http://localhost:8000
```

**Finding:** No LLM API keys configured (no OPENAI_API_KEY, ANTHROPIC_API_KEY, etc.)

#### Detection Strategy 6: Prompt-Related Patterns

**Files with "Prompt" in name:**
- `src/pages/PromptAnalyticsPage.tsx`
- `src/pages/PromptEditPage.tsx`

**Analysis of these files:**

```typescript
// PromptAnalyticsPage.tsx - This is a UI for viewing analytics, not LLM processing
// It fetches data from a backend API, does not invoke LLMs directly

// PromptEditPage.tsx - This is a prompt editor UI
// It sends prompts to a backend API for storage, not direct LLM invocation
```

**Finding:** These are administrative UI components for managing prompts stored in a backend system. The frontend does NOT directly call any LLM APIs.

#### Detection Strategy 7: Custom Implementation Patterns

**Files analyzed:**
- `src/components/LLMInteractionDisplay.tsx` - **Displays LLM interaction logs from backend**
- `src/components/ReviewAgentPanel.tsx` - **UI panel for agent configuration**
- `src/components/AgentScenarioPanel.tsx` - **UI for scenario management**

**Analysis of LLMInteractionDisplay.tsx:**
```typescript
// This component DISPLAYS interaction data fetched from an API
// It does NOT make direct LLM calls
// Example pattern:
interface LLMInteraction {
  id: string;
  // ... display fields
}
// Component renders interaction history from backend data
```

### 1.2 API Client Analysis

**src/api/client.ts:**
```typescript
// This is a generic HTTP client for the backend API
// Uses fetch() to communicate with VITE_API_BASE_URL
// No direct LLM SDK usage
```

**Key Finding:** All LLM-related operations are delegated to a **backend service** (referenced as `VITE_API_BASE_URL`). This frontend application is a **backoffice UI** that:

1. Manages prompt configurations (stored in backend)
2. Displays LLM interaction logs (fetched from backend)
3. Configures agents and scenarios (sent to backend)
4. Views analytics and metrics (aggregated by backend)

### 1.3 LLM Usage Summary

**Total Direct LLM Integrations Found:** 0

This is a **React frontend application** that serves as an administrative backoffice for a WhatsApp booking engine. The application:

- **Does NOT** import any LLM SDKs
- **Does NOT** make direct API calls to OpenAI, Anthropic, or other LLM providers
- **Does NOT** process prompts locally
- **DOES** provide UI for managing prompts and viewing LLM interactions that occur on a separate backend service

---

## Final Assessment

**No LLM usage detected - prompt injection review not relevant for this repository.**

This repository is a **frontend-only React application** (backoffice/admin panel) that communicates with a backend API. While the UI includes components for:
- Viewing LLM interaction logs (`LLMInteractionDisplay.tsx`)
- Editing prompts (`PromptEditPage.tsx`)
- Managing AI agents (`AgentScenarioPanel.tsx`, `ReviewAgentPanel.tsx`)

These are all **administrative interfaces** that send configuration data to and display results from a **separate backend service**. The actual LLM integration, prompt processing, and AI model invocations would occur in that backend service, which is **not part of this repository**.

### Recommendations

While this frontend doesn't have direct LLM security concerns, the following general security practices should be verified:

1. **Backend Security Review Needed:** The actual LLM security assessment should be conducted on the backend service (`VITE_API_BASE_URL`)

2. **Input Validation:** Ensure prompt editing interfaces properly sanitize inputs before sending to backend

3. **Authentication:** The app uses Supabase authentication (`AuthContext.tsx`, `useTenantAuth.ts`) - ensure proper authorization for prompt management features

4. **XSS Prevention:** When displaying LLM interaction logs (`LLMInteractionDisplay.tsx`), ensure outputs are properly escaped to prevent XSS from potentially malicious LLM responses stored in the backend

# components

Component architecture and design patterns

# Frontend Component Architecture Analysis

## WhatsApp Booking Engine Backoffice

---

## 1. Component Organization

### Directory Structure

```
src/
├── components/           # Reusable UI components (flat structure)
│   ├── ui/              # Base UI primitives
│   └── settings/        # Settings-specific components (22 files)
├── pages/               # Page-level components (route views)
├── contexts/            # React Context providers
├── hooks/               # Custom React hooks
├── utils/               # Utility functions
├── constants/           # Application constants
├── types/               # TypeScript type definitions
├── api/                 # API client configuration
└── assets/              # Static assets
```

### Component Naming Conventions

| Convention | Pattern | Examples |
|------------|---------|----------|
| **Pages** | `{Feature}Page.tsx` | `LoginPage.tsx`, `MetricsPage.tsx`, `ConversationsPage.tsx` |
| **Modals** | `{Feature}Modal.tsx` | `Modal.tsx`, `JobStatusModal.tsx`, `CustomerFormModal.tsx` |
| **Panels** | `{Feature}Panel.tsx` | `AgentScenarioPanel.tsx`, `HumanCallPanel.tsx`, `VoiceTestPanel.tsx` |
| **Sections** | `{Feature}Section.tsx` | `DeliveryStatsSection.tsx`, `VoiceMetricsSection.tsx` |
| **Feature Components** | `{Feature}{Type}.tsx` | `DonutChart.tsx`, `ToggleSwitch.tsx`, `ConfirmDialog.tsx` |

### Atomic Design Levels (Observed)

| Level | Location | Examples |
|-------|----------|----------|
| **Atoms** | `components/ui/` | Base UI primitives (1 file observed) |
| **Molecules** | `components/` | `ToggleSwitch.tsx`, `DonutChart.tsx`, `ConfirmDialog.tsx` |
| **Organisms** | `components/` | `Layout.tsx`, `FakeChat.tsx`, `LiveCallMonitor.tsx` |
| **Templates** | `components/` | `Layout.tsx` (wraps page content) |
| **Pages** | `pages/` | 20 page components |

---

## 2. Core Components

### Layout Components

#### `Layout.tsx`
- **Purpose**: Main application shell providing consistent structure
- **Responsibility**: Navigation, header, sidebar, and content area wrapper
- **Reusability**: High - used across all authenticated pages

#### `ProtectedRoute.tsx`
- **Purpose**: Route guard for authenticated routes
- **Responsibility**: Authentication check, redirect to login if unauthenticated
- **Reusability**: High - wraps all protected page components

### Modal/Dialog Components

#### `Modal.tsx`
- **Purpose**: Base modal component for overlay dialogs
- **Responsibility**: Portal rendering, backdrop, close handling
- **Reusability**: High - base component for all modals

#### `ConfirmDialog.tsx`
- **Purpose**: Confirmation dialogs for destructive actions
- **Responsibility**: Yes/No prompts, action confirmation
- **Reusability**: High - used throughout for delete/update confirmations

#### Specialized Modals
| Component | Purpose |
|-----------|---------|
| `JobStatusModal.tsx` | Display async job progress and status |
| `CustomerFormModal.tsx` | Customer creation/editing form |
| `CustomerBulkImportModal.tsx` | Bulk CSV import for customers |
| `CustomerPricingModal.tsx` | Customer pricing configuration |
| `BatchSurveyModal.tsx` | Batch survey sending interface |

### Display Components

#### `DonutChart.tsx`
- **Purpose**: Circular chart for metrics visualization
- **Responsibility**: Data visualization, percentage display
- **Reusability**: Medium - used in metrics dashboards

#### `LLMInteractionDisplay.tsx`
- **Purpose**: Display LLM conversation interactions
- **Responsibility**: Render AI/human message exchanges
- **Reusability**: Medium - conversation-related pages

#### Data Display Components
| Component | Purpose |
|-----------|---------|
| `VoiceCallLogs.tsx` | Voice call history table |
| `VoiceCostBreakdown.tsx` | Cost analytics display |
| `VoiceLatencyBreakdown.tsx` | Latency metrics visualization |
| `MessageDeliveryStatus.tsx` | Message status indicators |

### Feedback Components

#### `ErrorBoundary.tsx`
- **Purpose**: React error boundary for graceful error handling
- **Responsibility**: Catch JavaScript errors, display fallback UI
- **Reusability**: High - wraps major application sections

### Panel Components (Feature Sections)

| Component | Purpose |
|-----------|---------|
| `AgentScenarioPanel.tsx` | Agent scenario configuration |
| `HumanCallPanel.tsx` | Human call escalation interface |
| `OutboundCallPanel.tsx` | Outbound calling interface |
| `ReviewAgentPanel.tsx` | Agent review and feedback |
| `LiveTranscriptPanel.tsx` | Real-time call transcription |
| `VoiceTestPanel.tsx` | Voice system testing interface |

### Form Components

#### `ToggleSwitch.tsx`
- **Purpose**: Boolean toggle input
- **Responsibility**: On/off state toggle with visual feedback
- **Reusability**: High - used in settings and configuration forms

### Specialized Feature Components

| Component | Purpose |
|-----------|---------|
| `FakeChat.tsx` | Chat simulation for testing |
| `LiveCallMonitor.tsx` | Real-time call monitoring dashboard |
| `DemoTriggers.tsx` | Demo scenario triggers |
| `VoiceDebugAudio.tsx` | Audio debugging tools |

---

## 3. Component Patterns

### Container vs Presentational Pattern

**Page components act as containers:**
```
pages/
├── ConversationsPage.tsx    # Container: fetches data, manages state
├── MetricsPage.tsx          # Container: aggregates metrics data
└── SettingsPage.tsx         # Container: coordinates settings components
```

**Components are primarily presentational:**
```
components/
├── DonutChart.tsx           # Presentational: receives data via props
├── MessageDeliveryStatus.tsx # Presentational: displays status
└── VoiceCostBreakdown.tsx   # Presentational: renders cost data
```

### Custom Hooks (React Composables)

| Hook | Purpose | Pattern |
|------|---------|---------|
| `useConversationStream.ts` | Real-time conversation updates | Data subscription |
| `useHumanCall.ts` | Human call state management | Feature state encapsulation |
| `useLocalTimezone.ts` | Timezone handling | Utility hook |
| `useTenantAuth.ts` | Tenant authentication state | Auth state management |
| `useTenantUnits.ts` | Tenant unit preferences | Settings access |
| `useVoiceTest.ts` | Voice testing functionality | Feature logic encapsulation |

### Component Composition Strategy

**Settings Module Pattern:**
```
components/settings/
└── [22 files]              # Modular settings components
                            # Composed together in SettingsPage.tsx
```

The settings module demonstrates **composition over inheritance**:
- Each settings section is a separate component
- `SettingsPage.tsx` composes all settings components
- Individual settings components are self-contained

---

## 4. Props & Data Flow

### Props Typing Approach

**TypeScript interfaces defined in `src/types/index.ts`:**
- Centralized type definitions
- Shared across components and API layer
- Strong typing for props validation

### Event Handling Patterns

```typescript
// Callback prop pattern (inferred from component names)
onConfirm?: () => void;      // ConfirmDialog
onClose?: () => void;        // Modal components
onSubmit?: (data: T) => void; // Form modals
```

### Data Flow Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    AuthContext                          │
│                 (Global Auth State)                     │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                    App.tsx                              │
│              (Router + Providers)                       │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                   Layout.tsx                            │
│             (Shell + Navigation)                        │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                  Page Components                        │
│    (Data Fetching + State Management + Composition)     │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│              Feature Components                         │
│         (Props-driven, Presentational)                  │
└─────────────────────────────────────────────────────────┘
```

### Context Providers

#### `AuthContext.tsx`
- **Purpose**: Global authentication state
- **Scope**: Entire application
- **Provides**: User state, login/logout functions, tenant information
- **Consumers**: `ProtectedRoute.tsx`, page components, `useTenantAuth.ts`

---

## 5. Styling Patterns

### Tailwind CSS Implementation

**Configuration Files:**
- `tailwind.config.js` - Tailwind configuration
- `postcss.config.js` - PostCSS processing

**CSS Files:**
- `src/index.css` - Global styles, Tailwind imports
- `src/App.css` - App-specific styles

### Style Approach

| Aspect | Implementation |
|--------|----------------|
| **Framework** | Tailwind CSS (utility-first) |
| **Global Styles** | `index.css`, `App.css` |
| **Component Styles** | Tailwind utility classes inline |
| **Build Processing** | PostCSS + Vite |

### Responsive Design

Tailwind provides responsive utilities:
- Breakpoint prefixes (`sm:`, `md:`, `lg:`, `xl:`)
- Mobile-first approach

---

## 6. Component Testing

### Observed Testing Infrastructure

**No dedicated test files observed in the repository structure.**

Testing setup would typically include:
- Unit tests for components
- Integration tests for pages
- Hook testing

*Note: Test files may exist but were not included in the provided structure.*

---

## 7. Design System Integration

### UI Component Library

**`src/components/ui/` directory:**
- Contains base UI primitives (1 file observed)
- Custom implementation (not external library like Material-UI or Ant Design)

### Custom Design Tokens

Defined via Tailwind configuration:
- `tailwind.config.js` - Custom theme values
- Color palette, spacing, typography scales

### Accessibility Standards

Based on component types:
- `ConfirmDialog.tsx` - Likely implements focus management
- `Modal.tsx` - Should implement ARIA attributes for dialogs
- Form components - Label associations, error states

---

## 8. Performance Patterns

### Code Splitting Boundaries

**Route-based splitting (Vite + React Router):**
```
pages/
├── LoginPage.tsx            # Unauthenticated route
├── TenantSelectionPage.tsx  # Post-auth route
├── ConversationsPage.tsx    # Feature route
└── [17 more pages]          # Each potentially lazy-loaded
```

### Lazy Loading Patterns

Vite configuration (`vite.config.ts`) enables:
- Automatic code splitting
- Dynamic imports for routes
- Asset optimization

### Real-time Data Handling

**Streaming/Subscription hooks:**
- `useConversationStream.ts` - WebSocket or SSE for live updates
- `LiveCallMonitor.tsx` - Real-time call state
- `LiveTranscriptPanel.tsx` - Live transcription updates

---

## 9. Major Component Analysis

### Page Components (20 total)

| Component | Purpose | State Management | Reusability |
|-----------|---------|------------------|-------------|
| `LoginPage.tsx` | User authentication | Local + AuthContext | Single-use |
| `TenantSelectionPage.tsx` | Multi-tenant selection | AuthContext | Single-use |
| `ConversationsPage.tsx` | Conversation list/management | Local + API | Single-use |
| `ConversationDetailPage.tsx` | Single conversation view | Local + API | Single-use |
| `MetricsPage.tsx` | Analytics dashboard | Local + API | Single-use |
| `SettingsPage.tsx` | Application settings | Local + API | Single-use |
| `AgentPage.tsx` | Agent management | Local + API | Single-use |
| `VoiceTestPage.tsx` | Voice testing interface | useVoiceTest hook | Single-use |
| `UsersPage.tsx` | User management | Local + API | Single-use |
| `PromptEditPage.tsx` | Prompt configuration | Local + API | Single-use |
| `PromptAnalyticsPage.tsx` | Prompt performance | Local + API | Single-use |
| `PolicyRulesPage.tsx` | Policy configuration | Local + API | Single-use |
| `PromiseBoardPage.tsx` | Promise tracking | Local + API | Single-use |
| `SurveyDashboardPage.tsx` | Survey analytics | Local + API | Single-use |
| `FailedMessagesPage.tsx` | Failed message handling | Local + API | Single-use |
| `OMSPage.tsx` | Order management | Local + API | Single-use |
| `DemoConfigPage.tsx` | Demo configuration | Local + API | Single-use |
| `FakeChatPage.tsx` | Chat simulation | FakeChat component | Single-use |
| `TenantSetupPage.tsx` | Tenant onboarding | Local + API | Single-use |
| `AcceptInvitationPage.tsx` | Invitation acceptance | Local + API | Single-use |

### Settings Components (22 files)

Located in `src/components/settings/`:
- Modular architecture for settings management
- Each component handles a specific settings domain
- Composed in `SettingsPage.tsx`

---

## 10. API Integration Pattern

### API Client (`src/api/client.ts`)

- Centralized API configuration
- Likely implements:
  - Base URL configuration
  - Authentication header injection
  - Error handling interceptors
  - Request/response transformation

### Auth Events (`src/utils/authEvents.ts`)

- Authentication event handling
- Token refresh logic
- Logout coordination

---

## 11. Constants & Validation

### Application Constants

| File | Purpose |
|------|---------|
| `constants/timings.ts` | Timing configurations (debounce, polling intervals) |
| `constants/tts.ts` | Text-to-speech configurations |
| `constants/units.ts` | Unit definitions and conversions |
| `constants/validation.ts` | Form validation rules |

### Utility Functions

| File | Purpose |
|------|---------|
| `utils/format.ts` | Data formatting (dates, numbers, currency) |
| `utils/fileDownload.ts` | File download handling |
| `utils/errorHandling.ts` | Error processing utilities |

---

## 12. Architecture Summary

### Technology Stack (Verified)

| Layer | Technology |
|-------|------------|
| **Framework** | React (TypeScript) |
| **Build Tool** | Vite |
| **Styling** | Tailwind CSS |
| **Linting** | ESLint |
| **Deployment** | Netlify |

### Design Patterns Summary

| Pattern | Implementation |
|---------|----------------|
| **Component Architecture** | Flat structure with feature-based grouping |
| **State Management** | React Context + Local State + Custom Hooks |
| **Data Fetching** | Page-level fetching, prop drilling to children |
| **Styling** | Utility-first CSS (Tailwind) |
| **Code Organization** | Feature-based with clear separation of concerns |
| **Type Safety** | TypeScript with centralized type definitions |

### Component Hierarchy

```
App.tsx
├── AuthContext.Provider
│   ├── ProtectedRoute
│   │   └── Layout
│   │       └── [Page Components]
│   │           └── [Feature Components]
│   │               └── [UI Components]
│   └── [Public Pages]
│       ├── LoginPage
│       └── AcceptInvitationPage
```

# state_and_data

State management and data flow patterns

# State Management & Data Flow Analysis

## WhatsApp Booking Engine Backoffice

---

## State Management

### 1. Global State

#### Authentication Context (React Context API)

**Implementation:** Custom React Context for authentication state management.

**File:** `src/contexts/AuthContext.tsx`

```
Pattern: React Context + useReducer/useState
Purpose: Centralized authentication state across the application
```

**Key Features:**
- Provides auth state to entire component tree
- Manages user session, tokens, and tenant information
- Integrates with `useTenantAuth` hook for tenant-specific auth

**Files:**
- `src/contexts/AuthContext.tsx` - Main auth context provider
- `src/hooks/useTenantAuth.ts` - Custom hook for tenant authentication
- `src/utils/authEvents.ts` - Auth event utilities

---

### 2. Server State

#### TanStack Query (React Query v5)

**Implementation:** `@tanstack/react-query` ^5.90.5

**Purpose:** Server state management, caching, and data synchronization.

**Key Features:**
- Automatic caching and background refetching
- Query invalidation patterns
- Optimistic updates support
- Request deduplication

**Files:**
- `src/api/client.ts` - API client configuration (likely contains query client setup)
- Individual page components use `useQuery`, `useMutation` hooks

**Typical Usage Pattern:**
```typescript
// Data fetching with automatic caching
const { data, isLoading, error } = useQuery({
  queryKey: ['conversations'],
  queryFn: fetchConversations
});

// Mutations with cache invalidation
const mutation = useMutation({
  mutationFn: updateData,
  onSuccess: () => queryClient.invalidateQueries(['key'])
});
```

---

### 3. Local Component State

#### React useState Patterns

**Used Throughout:**
- Form state in modals (`CustomerFormModal.tsx`, `BatchSurveyModal.tsx`, etc.)
- UI state (modals, tabs, accordions)
- Local filtering and pagination state

**Key UI State Components:**
| Component | State Purpose |
|-----------|---------------|
| `Modal.tsx` | Open/close state |
| `ConfirmDialog.tsx` | Dialog visibility |
| `ToggleSwitch.tsx` | Toggle state |
| `JobStatusModal.tsx` | Job polling state |

---

## Data Flow

### 1. API Integration

#### HTTP Client: Axios

**Implementation:** `axios` ^1.12.2

**File:** `src/api/client.ts`

**Configuration Features (Expected):**
- Base URL configuration (from `.env.example`)
- Request/response interceptors
- Authentication header injection
- Error handling middleware

**Environment Variables (from `.env.example`):**
```
API_BASE_URL
AUTH_TOKEN
```

#### Request/Response Flow:
```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Component  │────▶│  React Query │────▶│    Axios    │
│             │     │   useQuery   │     │   Client    │
└─────────────┘     └──────────────┘     └─────────────┘
       ▲                   │                    │
       │                   ▼                    ▼
       │            ┌──────────────┐     ┌─────────────┐
       └────────────│    Cache     │     │   Backend   │
                    └──────────────┘     │     API     │
                                         └─────────────┘
```

---

### 2. Authentication & Authorization

#### Auth Flow Architecture

**Components:**
- `src/contexts/AuthContext.tsx` - Auth state provider
- `src/components/ProtectedRoute.tsx` - Route protection
- `src/hooks/useTenantAuth.ts` - Tenant-specific auth hook
- `src/utils/authEvents.ts` - Auth event handling

**Protected Route Pattern:**
```
┌─────────────┐     ┌──────────────────┐     ┌─────────────┐
│   Router    │────▶│  ProtectedRoute  │────▶│    Page     │
└─────────────┘     └──────────────────┘     └─────────────┘
                           │
                           ▼
                    ┌──────────────────┐
                    │   AuthContext    │
                    │   (isLoggedIn?)  │
                    └──────────────────┘
                           │
                    ┌──────┴──────┐
                    ▼             ▼
              ┌─────────┐   ┌───────────┐
              │ Redirect│   │  Render   │
              │ to Login│   │  Content  │
              └─────────┘   └───────────┘
```

**Multi-Tenant Support:**
- `TenantSelectionPage.tsx` - Tenant picker
- `TenantSetupPage.tsx` - Tenant configuration
- `AcceptInvitationPage.tsx` - Invitation flow

---

### 3. Data Transformations

#### Utility Functions

**File:** `src/utils/format.ts`
- Data formatting utilities
- Display transformations

**File:** `src/utils/errorHandling.ts`
- Standardized error transformation
- Error message formatting

**Constants for Data Validation:**
- `src/constants/validation.ts` - Validation rules
- `src/constants/units.ts` - Unit conversions
- `src/constants/timings.ts` - Time-related constants
- `src/constants/tts.ts` - Text-to-speech config

---

### 4. Form Management

#### Native React Form Handling

**Pattern:** Controlled components with useState

**No external form library detected** (Formik, React Hook Form, etc.)

**Form Modal Components:**
| Component | Purpose |
|-----------|---------|
| `CustomerFormModal.tsx` | Customer data entry |
| `CustomerBulkImportModal.tsx` | Bulk CSV import |
| `BatchSurveyModal.tsx` | Survey batch operations |
| `CustomerPricingModal.tsx` | Pricing configuration |

**Validation:**
- `src/constants/validation.ts` - Centralized validation rules
- Inline validation in form components

**CSV Processing:**
- `papaparse` ^5.5.3 - CSV parsing for bulk imports
- `src/utils/fileDownload.ts` - File download utilities

---

### 5. Real-time Features

#### WebSocket/Streaming Connections

**Hook:** `src/hooks/useConversationStream.ts`
- Real-time conversation updates
- Streaming data handling

**Voice/Call Features:**
- `sip.js` ^0.21.2 - SIP protocol for voice calls
- `src/hooks/useVoiceTest.ts` - Voice testing functionality
- `src/hooks/useHumanCall.ts` - Human call handling

**Real-time Components:**
| Component | Feature |
|-----------|---------|
| `LiveCallMonitor.tsx` | Active call monitoring |
| `LiveTranscriptPanel.tsx` | Real-time transcription |
| `FakeChat.tsx` / `FakeChatPage.tsx` | Chat simulation |

**Data Flow for Real-time:**
```
┌─────────────────┐     ┌────────────────────┐     ┌─────────────┐
│  Backend/SIP    │────▶│ useConversation    │────▶│  Component  │
│  WebSocket      │     │ Stream Hook        │     │   State     │
└─────────────────┘     └────────────────────┘     └─────────────┘
        │                        │
        ▼                        ▼
┌─────────────────┐     ┌────────────────────┐
│  useVoiceTest   │     │ LiveTranscript     │
│  useHumanCall   │     │ Panel              │
└─────────────────┘     └────────────────────┘
```

---

### 6. Performance Optimizations

#### Date/Time Handling

**Libraries:**
- `date-fns` ^4.1.0 - Date manipulation
- `date-fns-tz` ^3.2.0 - Timezone support

**Hook:** `src/hooks/useLocalTimezone.ts`
- Local timezone detection and conversion
- Optimized date formatting

#### Error Monitoring

**Integration:** `@sentry/react` ^10.23.0
- Error tracking and performance monitoring
- Integrated via ErrorBoundary pattern

**File:** `src/components/ErrorBoundary.tsx`

---

## Summary Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        Application                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐    ┌───────────────────────────────────────┐  │
│  │ AuthContext  │    │           React Query                  │  │
│  │  (Global     │    │  ┌─────────────────────────────────┐  │  │
│  │   Auth)      │    │  │  Cache Layer                    │  │  │
│  └──────┬───────┘    │  │  - Query caching                │  │  │
│         │            │  │  - Background refetch           │  │  │
│         ▼            │  │  - Optimistic updates           │  │  │
│  ┌──────────────┐    │  └─────────────────────────────────┘  │  │
│  │ useTenantAuth│    └───────────────┬───────────────────────┘  │
│  └──────────────┘                    │                           │
│                                      ▼                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Axios HTTP Client                         ││
│  │  - Request interceptors (auth headers)                       ││
│  │  - Response interceptors (error handling)                    ││
│  └─────────────────────────────────────────────────────────────┘│
│                                      │                           │
│  ┌───────────────────────────────────┼───────────────────────┐  │
│  │        Real-time Layer            │                       │  │
│  │  ┌─────────────────┐  ┌───────────▼────────┐              │  │
│  │  │ SIP.js          │  │ Conversation       │              │  │
│  │  │ (Voice Calls)   │  │ Stream Hook        │              │  │
│  │  └─────────────────┘  └────────────────────┘              │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
                    ┌────────────────────────┐
                    │     Backend APIs       │
                    │  - REST endpoints      │
                    │  - WebSocket streams   │
                    │  - SIP server          │
                    └────────────────────────┘
```

---

## Key Technologies Summary

| Category | Technology | Purpose |
|----------|------------|---------|
| **Global State** | React Context | Authentication state |
| **Server State** | TanStack Query v5 | API caching & sync |
| **HTTP Client** | Axios | API requests |
| **Real-time** | SIP.js, Custom hooks | Voice calls, streaming |
| **Forms** | Native React (useState) | Form state |
| **Error Tracking** | Sentry | Monitoring |
| **Date Handling** | date-fns, date-fns-tz | Time/timezone |
| **CSV Processing** | PapaParse | Bulk imports |