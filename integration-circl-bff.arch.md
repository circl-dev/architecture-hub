# hl_overview

High level overview of the codebase

# Repository Analysis: integration-circl-bff

## 0. Repository Name
**[[integration-circl-bff]]**

---

## 1. Project Purpose

This project is a **Backend-for-Frontend (BFF)** service designed as an integration layer for the "Circl" platform. Its primary purposes are:

- **Server-Sent Events (SSE) Gateway**: Manages real-time SSE connections between clients and backend services
- **Event Distribution**: Routes and delivers events to connected clients with support for message replay/resume
- **API Proxy**: Proxies requests to downstream backend services
- **Multi-tenant Support**: Handles tenant isolation and routing
- **Authentication/Authorization**: JWT-based authentication with token management

The primary domain is **real-time event streaming and API integration** for a multi-tenant SaaS platform.

---

## 2. Architecture Pattern

- **Backend-for-Frontend (BFF)** - Primary pattern for aggregating/adapting backend services for frontend consumption
- **Event-Driven Architecture** - Uses NATS messaging for pub/sub event distribution
- **Service Layer Pattern** - Clear separation between routes, services, and utilities
- **Plugin-based Architecture** - Fastify plugins for modular feature composition

---

## 3. Technology Stack

### Primary Language
- **TypeScript/Node.js**

### Framework
- **Fastify** - High-performance Node.js web framework

### Major Dependencies (inferred from file structure and code patterns)

| Category | Technology |
|----------|------------|
| Runtime | Node.js with TypeScript |
| Web Framework | Fastify |
| Messaging | NATS (nats.service.ts) |
| Testing | Vitest (unit/integration), Playwright (E2E) |
| Authentication | JWT (jwt.service.ts, auth.service.ts) |
| Metrics | Custom metrics service |
| Rate Limiting | Custom rate-limit service |

### Configuration Files
- `package.json` / `package-lock.json` - Node.js dependencies
- `tsconfig.json` - TypeScript configuration
- `vitest.config.ts` - Vitest test runner configuration
- `playwright.config.ts` - Playwright E2E test configuration
- `.env.example` - Environment variables template

---

## 4. Initial Structure Impression

| Component | Description |
|-----------|-------------|
| **Backend Service** | Core BFF application (`src/`) |
| **Test Suite** | Comprehensive testing across unit, integration, and E2E (`tests/`) |
| **Documentation** | Extensive docs for deployment, testing, and architecture (`docs/`, root-level MD files) |
| **Test Utilities** | Manual testing scripts and helpers (root-level test files) |

This is a **backend-only** project with no frontend code - purely a BFF/API gateway service.

---

## 5. Configuration/Package Files

```
.env.example          # Environment variable template
package.json          # Node.js project manifest
package-lock.json     # Dependency lock file
tsconfig.json         # TypeScript compiler options
vitest.config.ts      # Vitest test configuration
playwright.config.ts  # Playwright E2E configuration
.gitignore           # Git ignore patterns
```

---

## 6. Directory Structure

### `/src` - Application Source Code

```
src/
├── index.ts              # Main application entry point
├── types/
│   └── index.ts          # TypeScript type definitions
├── plugins/
│   ├── auth.plugin.ts    # Authentication plugin
│   └── sse.plugin.ts     # SSE handling plugin
├── utils/
│   ├── config.ts         # Configuration management
│   ├── logger.ts         # Logging utility
│   └── tenant.ts         # Multi-tenant utilities
├── routes/
│   ├── events.ts         # SSE event endpoints
│   ├── metrics.ts        # Metrics/monitoring endpoints
│   └── proxy.ts          # API proxy routes
└── services/
    ├── auth.service.ts         # Authentication logic
    ├── dlq-monitor.service.ts  # Dead letter queue monitoring
    ├── idempotency.service.ts  # Idempotency handling
    ├── jwt.service.ts          # JWT token operations
    ├── metrics.service.ts      # Metrics collection
    ├── nats.service.ts         # NATS messaging client
    ├── publish-queue.service.ts # Message publish queue
    ├── rate-limit.service.ts   # Rate limiting
    ├── resume.service.ts       # SSE connection resume
    ├── ring-buffer.ts          # Ring buffer for event replay
    └── sse-manager.service.ts  # SSE connection management
```

### `/tests` - Test Suite

```
tests/
├── global-setup.ts       # Test environment setup
├── helpers/
│   ├── auth-test-helper.ts  # Auth testing utilities
│   └── sse-test-client.ts   # SSE test client
├── unit/                 # Unit tests (isolated service tests)
├── integration/          # Integration tests (API/service interaction)
└── e2e/                  # End-to-end tests (browser, load, full flows)
```

### `/docs` - Documentation

```
docs/
├── INDEX.md
├── testing/              # Test documentation and results
└── guides/
    └── deployment.md     # Deployment instructions
```

---

## 7. High-Level Architecture

### Pattern: **Event-Driven BFF with Plugin Architecture**

```
┌─────────────────────────────────────────────────────────────┐
│                      Clients (Browsers)                      │
└─────────────────────────┬───────────────────────────────────┘
                          │ SSE / HTTP
┌─────────────────────────▼───────────────────────────────────┐
│                   Fastify BFF Service                        │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    Plugins Layer                      │   │
│  │  ┌─────────────┐  ┌─────────────┐                    │   │
│  │  │ Auth Plugin │  │ SSE Plugin  │                    │   │
│  │  └─────────────┘  └─────────────┘                    │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    Routes Layer                       │   │
│  │  ┌────────┐  ┌─────────┐  ┌───────┐                 │   │
│  │  │ Events │  │ Metrics │  │ Proxy │                 │   │
│  │  └────────┘  └─────────┘  └───────┘                 │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                   Services Layer                      │   │
│  │  ┌─────────┐ ┌──────────────┐ ┌────────────────┐    │   │
│  │  │   JWT   │ │ SSE Manager  │ │  Rate Limiter  │    │   │
│  │  ├─────────┤ ├──────────────┤ ├────────────────┤    │   │
│  │  │  NATS   │ │ Ring Buffer  │ │  Idempotency   │    │   │
│  │  ├─────────┤ ├──────────────┤ ├────────────────┤    │   │
│  │  │  Auth   │ │   Resume     │ │  DLQ Monitor   │    │   │
│  │  └─────────┘ └──────────────┘ └────────────────┘    │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────┬───────────────────────────────────┘
                          │ NATS Pub/Sub
┌─────────────────────────▼───────────────────────────────────┐
│                    Backend Services                          │
└─────────────────────────────────────────────────────────────┘
```

### Supporting Evidence

| Evidence | Pattern Indication |
|----------|-------------------|
| `nats.service.ts`, `ring-buffer.ts` | Event-Driven Architecture |
| `plugins/` directory with auth & SSE | Plugin-based Composition |
| `routes/`, `services/`, `utils/` separation | Layered Architecture |
| `proxy.ts` route | BFF/API Gateway Pattern |
| `tenant.ts` utility | Multi-tenant SaaS Design |
| `resume.service.ts`, `ring-buffer.ts` | Event Replay/Durability Pattern |

---

## 8. Build, Execution, and Test

### Entry Point
- **`src/index.ts`** - Main application entry point

### Build
```bash
# TypeScript compilation (implied by tsconfig.json)
npm run build  # Likely compiles TS to JS
```

### Execution
```bash
# Development
npm run dev    # Likely uses ts-node or nodemon

# Production
npm start      # Runs compiled JavaScript
```

### Testing

| Type | Command | Framework | Location |
|------|---------|-----------|----------|
| Unit Tests | `npm test` | Vitest | `tests/unit/` |
| Integration | `npm run test:integration` | Vitest | `tests/integration/` |
| E2E | `npm run test:e2e` | Playwright | `tests/e2e/` |

### Manual Testing Scripts
```bash
node generate-test-token.js  # Generate test JWT tokens
node test-resume.js          # Test SSE resume functionality
node test-sse-flow.js        # Test SSE flow
./test-sse-simple.sh         # Shell-based SSE test
```

### Test Configuration
- `vitest.config.ts` - Unit/Integration test configuration
- `playwright.config.ts` - Browser E2E test configuration
- `tests/global-setup.ts` - Test environment initialization

---

## Summary

**integration-circl-bff** is a production-ready TypeScript/Fastify BFF service that:
1. Manages SSE connections for real-time event delivery
2. Integrates with NATS for event pub/sub
3. Provides API proxy capabilities to backend services
4. Implements multi-tenant authentication via JWT
5. Includes robust resilience features (DLQ monitoring, idempotency, rate limiting, message replay)

The codebase demonstrates mature engineering practices with comprehensive testing (unit, integration, E2E), extensive documentation, and clear architectural separation.

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

## 1. `src/types/`

### Core Responsibility
Provides TypeScript type definitions and interfaces used throughout the application to ensure type safety and consistent data structures.

### Key Components

| File | Role |
|------|------|
| `index.ts` | Central type definitions file containing interfaces for SSE events, authentication tokens, tenant configurations, NATS messages, and other shared data structures |

### Dependencies & Interactions
- **Internal Dependencies:** None (types are imported by other modules, not the reverse)
- **External Services:** None directly; defines shapes for external data (NATS messages, JWT payloads)

---

## 2. `src/plugins/`

### Core Responsibility
Fastify plugins that extend the server's functionality with authentication and SSE (Server-Sent Events) capabilities as reusable, encapsulated features.

### Key Components

| File | Role |
|------|------|
| `auth.plugin.ts` | Fastify plugin that decorates requests with authentication hooks, validates JWTs, and attaches user/tenant context to requests |
| `sse.plugin.ts` | Fastify plugin that sets up SSE infrastructure, manages connection lifecycle, and provides SSE streaming utilities to routes |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/services/jwt.service.ts` - For token validation
  - `@src/services/auth.service.ts` - For authentication logic
  - `@src/services/sse-manager.service.ts` - For SSE connection management
  - `@src/utils/logger.ts` - For logging
  - `@src/types/` - For type definitions
- **External Services:** 
  - JWT validation against external identity providers
  - Integrates with Fastify's plugin system

---

## 3. `src/utils/`

### Core Responsibility
Shared utility functions and configuration management that provide cross-cutting concerns like logging, configuration loading, and tenant resolution.

### Key Components

| File | Role |
|------|------|
| `config.ts` | Loads and validates environment variables, exports typed configuration object with defaults and validation |
| `logger.ts` | Configures and exports a structured logger instance (likely Pino) with appropriate log levels and formatting |
| `tenant.ts` | Tenant resolution utilities - extracts tenant context from requests, validates tenant IDs, provides multi-tenancy helpers |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - For configuration and tenant type definitions
- **External Services:**
  - Environment variables (`.env`)
  - May interact with external tenant registry or configuration service

---

## 4. `src/routes/`

### Core Responsibility
HTTP route handlers that define the API endpoints for the BFF (Backend-for-Frontend), handling incoming requests and orchestrating responses.

### Key Components

| File | Role |
|------|------|
| `events.ts` | SSE event streaming endpoints - manages client subscriptions, event token generation, and real-time event delivery |
| `metrics.ts` | Exposes application metrics endpoints (likely Prometheus-compatible) for monitoring and observability |
| `proxy.ts` | API proxy endpoints that forward requests to upstream services, handling authentication header injection and response transformation |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/services/sse-manager.service.ts` - SSE connection handling
  - `@src/services/auth.service.ts` - Request authentication
  - `@src/services/jwt.service.ts` - Token generation/validation
  - `@src/services/metrics.service.ts` - Metrics collection
  - `@src/utils/logger.ts` - Request logging
  - `@src/utils/tenant.ts` - Tenant context extraction
  - `@src/types/` - Request/response types
- **External Services:**
  - Upstream backend APIs (via proxy)
  - Client browsers (SSE connections)

---

## 5. `src/services/`

### Core Responsibility
Business logic and service layer containing the core functionality for authentication, messaging, SSE management, and data processing.

### Key Components

| File | Role |
|------|------|
| `auth.service.ts` | Handles authentication logic, session validation, and user context management |
| `jwt.service.ts` | JWT token creation, validation, and parsing; manages token signing keys and claims |
| `nats.service.ts` | NATS messaging client - handles pub/sub connections, message serialization, and subscription management |
| `sse-manager.service.ts` | Manages active SSE connections, client tracking, event broadcasting, and connection cleanup |
| `ring-buffer.ts` | Circular buffer implementation for storing recent events, enabling resume functionality |
| `resume.service.ts` | Handles SSE connection resumption using `Last-Event-ID`, retrieves missed events from ring buffer |
| `publish-queue.service.ts` | Queues outgoing messages for reliable publishing, handles backpressure and retry logic |
| `rate-limit.service.ts` | Implements rate limiting for API endpoints and SSE connections per tenant/user |
| `idempotency.service.ts` | Ensures idempotent event processing, tracks processed event IDs to prevent duplicates |
| `dlq-monitor.service.ts` | Monitors Dead Letter Queue for failed messages, provides alerting and retry mechanisms |
| `metrics.service.ts` | Collects and exposes application metrics (connections, message counts, latencies) |

### Dependencies & Interactions

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Services Dependency Map                       │
├─────────────────────────────────────────────────────────────────────┤
│  nats.service ──────┬──► sse-manager.service ──► ring-buffer        │
│                     │           │                                    │
│                     │           ▼                                    │
│                     └──► publish-queue.service                       │
│                              │                                       │
│                              ▼                                       │
│                     dlq-monitor.service                              │
│                                                                      │
│  jwt.service ◄──── auth.service                                     │
│                                                                      │
│  resume.service ◄─── ring-buffer                                    │
│                                                                      │
│  rate-limit.service ◄─── idempotency.service                        │
│                                                                      │
│  metrics.service ◄─── (all services report metrics)                 │
└─────────────────────────────────────────────────────────────────────┘
```

- **Internal Dependencies:**
  - `@src/utils/config.ts` - Configuration values
  - `@src/utils/logger.ts` - Logging
  - `@src/types/` - Type definitions
- **External Services:**
  - **NATS Server:** Message broker for real-time event streaming
  - **External Auth Provider:** JWT key verification
  - **Prometheus/Metrics Collector:** Metrics scraping

---

## 6. `tests/`

### Core Responsibility
Comprehensive test suite covering unit tests, integration tests, and end-to-end tests to ensure application reliability and correctness.

### Key Components

| Directory/File | Role |
|----------------|------|
| `global-setup.ts` | Test environment initialization - sets up test databases, mock servers, environment variables |
| **`unit/`** | |
| `jwt.service.test.ts` | Unit tests for JWT token generation and validation logic |
| `nats.service.test.ts` | Unit tests for NATS connection and message handling |
| `ring-buffer.test.ts` | Unit tests for circular buffer operations |
| `sse-manager.service.test.ts` | Unit tests for SSE connection management |
| **`integration/`** | |
| `events-sse.test.ts` | Integration tests for SSE event streaming endpoints |
| `events-token.test.ts` | Integration tests for event token generation flow |
| `proxy.test.ts` | Integration tests for API proxy functionality |
| `sse-resume.test.ts` | Integration tests for SSE resume capability |
| **`e2e/`** | |
| `browser-cors.e2e.test.ts` | End-to-end CORS behavior validation |
| `load-basic.e2e.test.ts` | Basic load testing scenarios |
| `single-connection.e2e.test.ts` | Single SSE connection lifecycle test |
| `ttl-sweeper.e2e.test.ts` | Tests for connection TTL and cleanup |
| `two-way-communication.e2e.test.ts` | Full duplex communication patterns |
| **`helpers/`** | |
| `auth-test-helper.ts` | Test utilities for generating auth tokens and mocking auth |
| `sse-test-client.ts` | SSE client wrapper for testing SSE endpoints |

### Dependencies & Interactions
- **Internal Dependencies:**
  - All `@src/` modules under test
  - Test helpers reference service modules
- **External Services:**
  - Mock NATS server
  - Test JWT signing keys
  - Playwright for E2E browser tests

---

## 7. `src/index.ts` (Application Entry Point)

### Core Responsibility
Application bootstrap and initialization - creates the Fastify server instance, registers plugins and routes, and starts the HTTP server.

### Key Components

| Element | Role |
|---------|------|
| Server initialization | Creates configured Fastify instance with appropriate options |
| Plugin registration | Registers auth and SSE plugins in correct order |
| Route registration | Mounts all route handlers under appropriate paths |
| Graceful shutdown | Handles SIGTERM/SIGINT for clean connection closure |
| Service initialization | Bootstraps NATS connections and background services |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/plugins/*` - All Fastify plugins
  - `@src/routes/*` - All route modules
  - `@src/services/nats.service.ts` - NATS connection initialization
  - `@src/services/sse-manager.service.ts` - SSE manager initialization
  - `@src/utils/config.ts` - Server configuration
  - `@src/utils/logger.ts` - Logger initialization
- **External Services:**
  - Binds to configured HTTP port
  - Connects to NATS server on startup

---

## Summary Dependency Graph

```
                              ┌──────────────────┐
                              │   src/index.ts   │
                              │  (Entry Point)   │
                              └────────┬─────────┘
                                       │
              ┌────────────────────────┼────────────────────────┐
              │                        │                        │
              ▼                        ▼                        ▼
      ┌───────────────┐        ┌───────────────┐        ┌───────────────┐
      │   plugins/    │        │    routes/    │        │   services/   │
      │ auth, sse     │        │ events,proxy, │        │  (business    │
      │               │        │   metrics     │        │    logic)     │
      └───────┬───────┘        └───────┬───────┘        └───────┬───────┘
              │                        │                        │
              └────────────────────────┼────────────────────────┘
                                       │
                          ┌────────────┴────────────┐
                          │                         │
                          ▼                         ▼
                  ┌───────────────┐         ┌───────────────┐
                  │    utils/     │         │    types/     │
                  │ config,logger │         │  (interfaces) │
                  │    tenant     │         │               │
                  └───────────────┘         └───────────────┘
```

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Repository: integration-circl-bff_cb5d594f

---

## Internal Modules

Based on the directory structure and file organization, the following internal modules and components have been identified:

### Core Application (`src/`)

#### Entry Point
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Application Bootstrap** | `src/index.ts` | Main entry point for the BFF (Backend-for-Frontend) application |

#### Types (`src/types/`)
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Type Definitions** | `src/types/index.ts` | Centralized TypeScript type definitions and interfaces for the project |

#### Plugins (`src/plugins/`)
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Auth Plugin** | `src/plugins/auth.plugin.ts` | Fastify plugin for authentication integration |
| **SSE Plugin** | `src/plugins/sse.plugin.ts` | Fastify plugin for Server-Sent Events functionality |

#### Utilities (`src/utils/`)
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Config** | `src/utils/config.ts` | Application configuration management |
| **Logger** | `src/utils/logger.ts` | Logging utility configuration |
| **Tenant** | `src/utils/tenant.ts` | Multi-tenant handling utilities |

#### Routes (`src/routes/`)
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Events Route** | `src/routes/events.ts` | HTTP endpoints for event-related operations |
| **Metrics Route** | `src/routes/metrics.ts` | HTTP endpoints for metrics exposure |
| **Proxy Route** | `src/routes/proxy.ts` | HTTP proxy routing logic |

#### Services (`src/services/`)
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Auth Service** | `src/services/auth.service.ts` | Authentication and authorization logic |
| **DLQ Monitor Service** | `src/services/dlq-monitor.service.ts` | Dead Letter Queue monitoring and handling |
| **Idempotency Service** | `src/services/idempotency.service.ts` | Request idempotency management to prevent duplicate processing |
| **JWT Service** | `src/services/jwt.service.ts` | JSON Web Token creation, validation, and parsing |
| **Metrics Service** | `src/services/metrics.service.ts` | Application metrics collection and reporting |
| **NATS Service** | `src/services/nats.service.ts` | NATS messaging system client integration |
| **Publish Queue Service** | `src/services/publish-queue.service.ts` | Message publishing queue management |
| **Rate Limit Service** | `src/services/rate-limit.service.ts` | Request rate limiting implementation |
| **Resume Service** | `src/services/resume.service.ts` | SSE connection resumption handling |
| **Ring Buffer** | `src/services/ring-buffer.ts` | Circular buffer data structure for event storage |
| **SSE Manager Service** | `src/services/sse-manager.service.ts` | Server-Sent Events connection management |

### Test Infrastructure (`tests/`)

#### Test Helpers (`tests/helpers/`)
| Module | File | Primary Responsibility |
|--------|------|----------------------|
| **Auth Test Helper** | `tests/helpers/auth-test-helper.ts` | Authentication utilities for testing |
| **SSE Test Client** | `tests/helpers/sse-test-client.ts` | SSE client utilities for integration testing |

---

## External Dependencies

### Production Dependencies

**Source:** `/package.json`

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|---------------------|
| `@fastify/cors` | Fastify CORS | CORS (Cross-Origin Resource Sharing) support plugin for Fastify |
| `@fastify/http-proxy` | Fastify HTTP Proxy | HTTP proxy plugin for Fastify to forward requests to upstream services |
| `@fastify/sensible` | Fastify Sensible | Utility plugin adding useful defaults and helpers to Fastify |
| `@supabase/supabase-js` | Supabase JavaScript Client | Client library for Supabase backend services (database, auth, storage) |
| `@types/jsonwebtoken` | jsonwebtoken TypeScript Types | TypeScript type definitions for the jsonwebtoken library |
| `dotenv` | dotenv | Environment variable loader from `.env` files |
| `fastify` | Fastify | High-performance web framework for Node.js (core HTTP server) |
| `fastify-plugin` | Fastify Plugin | Utility for creating Fastify plugins with proper encapsulation |
| `jose` | JOSE (JavaScript Object Signing and Encryption) | JWT/JWS/JWE library for token handling |
| `jsonwebtoken` | jsonwebtoken | JWT implementation for token signing and verification |
| `nats` | NATS | Client library for NATS messaging system |
| `pino` | Pino | Fast JSON logger for Node.js |
| `pino-pretty` | Pino Pretty | Pretty-print formatter for Pino logs (development readability) |
| `prom-client` | Prometheus Client | Prometheus metrics client for exposing application metrics |

### Developer-Only Dependencies

**Source:** `/package.json (dev)`

| Dependency | Official Name | Primary Role/Purpose |
|------------|---------------|---------------------|
| `@playwright/test` | Playwright Test | End-to-end testing framework for browser automation |
| `@types/node` | Node.js TypeScript Types | TypeScript type definitions for Node.js APIs |
| `@types/supertest` | Supertest TypeScript Types | TypeScript type definitions for Supertest |
| `@vitest/ui` | Vitest UI | Visual UI for Vitest test runner |
| `eventsource` | EventSource | W3C EventSource (SSE) client implementation for Node.js testing |
| `node-fetch` | node-fetch | Fetch API implementation for Node.js |
| `supertest` | SuperTest | HTTP assertions library for testing HTTP servers |
| `tsx` | tsx | TypeScript execution engine for Node.js (development runner) |
| `typescript` | TypeScript | TypeScript language compiler |
| `vitest` | Vitest | Unit/integration testing framework (Vite-native) |

---

## Architecture Summary

This project is a **Backend-for-Frontend (BFF)** service built on the **Fastify** framework with the following architectural characteristics:

1. **Communication Patterns**: 
   - REST/HTTP proxy capabilities
   - Server-Sent Events (SSE) for real-time client communication
   - NATS messaging for backend service communication

2. **Security**: JWT-based authentication with Supabase integration

3. **Observability**: Prometheus metrics and Pino structured logging

4. **Reliability Features**: Rate limiting, idempotency handling, Dead Letter Queue monitoring, and SSE connection resumption

# core_entities

Core entities and their relationships

# Domain Model Analysis: integration-circl-bff

Based on analysis of the repository structure and source files, this appears to be a **Backend-for-Frontend (BFF) integration service** focused on Server-Sent Events (SSE) communication, event streaming, and multi-tenant authentication.

---

## 1. Common Data Entities / Domain Models

### Core Entities Identified:

| Entity | Location(s) | Purpose |
|--------|-------------|---------|
| **SSEConnection** | `sse-manager.service.ts`, `types/index.ts` | Represents an active SSE client connection |
| **Event/Message** | `nats.service.ts`, `ring-buffer.ts` | Domain events flowing through the system |
| **User/AuthContext** | `auth.service.ts`, `jwt.service.ts` | Authenticated user identity and claims |
| **Tenant** | `utils/tenant.ts`, `types/index.ts` | Multi-tenant organization context |
| **RingBufferEntry** | `ring-buffer.ts` | Cached events for replay/resume capability |
| **IdempotencyRecord** | `idempotency.service.ts` | Deduplication tracking for event processing |
| **DLQEntry** | `dlq-monitor.service.ts` | Dead Letter Queue entries for failed events |
| **RateLimitRecord** | `rate-limit.service.ts` | Rate limiting state per client/tenant |
| **Metrics** | `metrics.service.ts` | Observability and telemetry data |

---

## 2. Entity Descriptions and Key Attributes

### **SSEConnection**
Represents an active Server-Sent Events connection from a client.

```
┌─────────────────────────────────────┐
│           SSEConnection             │
├─────────────────────────────────────┤
│ - connectionId: string (UUID)       │
│ - userId: string                    │
│ - tenantId: string                  │
│ - createdAt: Date                   │
│ - lastEventId: string (optional)    │
│ - response: ServerResponse          │
│ - isAlive: boolean                  │
│ - metadata: Record<string, any>     │
└─────────────────────────────────────┘
```

---

### **Event / Message**
Domain events published and consumed via NATS messaging.

```
┌─────────────────────────────────────┐
│              Event                  │
├─────────────────────────────────────┤
│ - id: string (UUID)                 │
│ - type: string (event type/name)    │
│ - tenantId: string                  │
│ - userId: string (target/origin)    │
│ - payload: object                   │
│ - timestamp: Date/number            │
│ - subject: string (NATS subject)    │
│ - correlationId: string (optional)  │
└─────────────────────────────────────┘
```

---

### **User / AuthContext**
Authenticated user context extracted from JWT tokens.

```
┌─────────────────────────────────────┐
│         User / AuthContext          │
├─────────────────────────────────────┤
│ - userId: string                    │
│ - tenantId: string                  │
│ - email: string (optional)          │
│ - roles: string[]                   │
│ - permissions: string[]             │
│ - tokenExp: number (expiration)     │
│ - issuedAt: number                  │
└─────────────────────────────────────┘
```

---

### **Tenant**
Multi-tenant organization context for isolation.

```
┌─────────────────────────────────────┐
│              Tenant                 │
├─────────────────────────────────────┤
│ - tenantId: string                  │
│ - name: string (optional)           │
│ - config: TenantConfig (optional)   │
│ - isActive: boolean                 │
└─────────────────────────────────────┘
```

---

### **RingBufferEntry**
Cached event for replay/resume functionality.

```
┌─────────────────────────────────────┐
│          RingBufferEntry            │
├─────────────────────────────────────┤
│ - eventId: string                   │
│ - data: string (serialized event)   │
│ - timestamp: number                 │
│ - tenantId: string                  │
│ - sequence: number                  │
│ - ttl: number (time-to-live)        │
└─────────────────────────────────────┘
```

---

### **IdempotencyRecord**
Tracks processed events to prevent duplicate handling.

```
┌─────────────────────────────────────┐
│        IdempotencyRecord            │
├─────────────────────────────────────┤
│ - key: string (idempotency key)     │
│ - eventId: string                   │
│ - processedAt: Date                 │
│ - expiresAt: Date                   │
│ - status: 'pending'|'completed'     │
└─────────────────────────────────────┘
```

---

### **DLQEntry (Dead Letter Queue)**
Failed events requiring manual intervention or retry.

```
┌─────────────────────────────────────┐
│            DLQEntry                 │
├─────────────────────────────────────┤
│ - id: string                        │
│ - originalEvent: Event              │
│ - errorMessage: string              │
│ - failedAt: Date                    │
│ - retryCount: number                │
│ - lastRetryAt: Date (optional)      │
│ - tenantId: string                  │
└─────────────────────────────────────┘
```

---

### **RateLimitRecord**
Tracks request/event rates for throttling.

```
┌─────────────────────────────────────┐
│         RateLimitRecord             │
├─────────────────────────────────────┤
│ - key: string (client/tenant key)   │
│ - count: number                     │
│ - windowStart: number               │
│ - limit: number                     │
│ - remaining: number                 │
└─────────────────────────────────────┘
```

---

### **PublishQueueItem**
Queued outbound events awaiting delivery.

```
┌─────────────────────────────────────┐
│        PublishQueueItem             │
├─────────────────────────────────────┤
│ - id: string                        │
│ - event: Event                      │
│ - targetConnections: string[]       │
│ - priority: number                  │
│ - enqueuedAt: Date                  │
│ - attempts: number                  │
└─────────────────────────────────────┘
```

---

## 3. Entity Relationships

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        ENTITY RELATIONSHIP DIAGRAM                       │
└─────────────────────────────────────────────────────────────────────────┘

                              ┌──────────┐
                              │  Tenant  │
                              └────┬─────┘
                                   │
                    ┌──────────────┼──────────────┐
                    │ 1:N          │ 1:N          │ 1:N
                    ▼              ▼              ▼
              ┌──────────┐  ┌────────────┐  ┌───────────────┐
              │   User   │  │   Event    │  │ RateLimitRec  │
              └────┬─────┘  └─────┬──────┘  └───────────────┘
                   │              │
          ┌────────┴────────┐     │
          │ 1:N             │     │ 1:1
          ▼                 │     ▼
    ┌──────────────┐        │  ┌─────────────────┐
    │SSEConnection │        │  │ RingBufferEntry │
    └──────┬───────┘        │  └─────────────────┘
           │                │
           │ receives       │ may produce
           │ N:M            │ 1:0..1
           ▼                ▼
      ┌──────────┐    ┌──────────────────┐
      │  Event   │◄───│ IdempotencyRecord│
      └────┬─────┘    └──────────────────┘
           │
           │ on failure
           │ 1:0..1
           ▼
      ┌──────────┐
      │ DLQEntry │
      └──────────┘
```

### Relationship Descriptions

| Relationship | Type | Description |
|--------------|------|-------------|
| **Tenant → User** | One-to-Many | A tenant has multiple users |
| **Tenant → Event** | One-to-Many | Events are scoped to a tenant |
| **Tenant → SSEConnection** | One-to-Many | Connections are isolated per tenant |
| **Tenant → RateLimitRecord** | One-to-Many | Rate limits apply per tenant/client |
| **User → SSEConnection** | One-to-Many | A user can have multiple active SSE connections (different devices/tabs) |
| **SSEConnection → Event** | Many-to-Many | Connections receive events; events broadcast to multiple connections |
| **Event → RingBufferEntry** | One-to-One | Events cached in ring buffer for replay |
| **Event → IdempotencyRecord** | One-to-One | Each event has one idempotency tracking record |
| **Event → DLQEntry** | One-to-Zero/One | Failed events may have a DLQ entry |
| **DLQEntry → Event** | Many-to-One | DLQ references the original event |

---

## 4. Domain Context Summary

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         BOUNDED CONTEXTS                                 │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌─────────────────────┐    ┌─────────────────────┐                     │
│  │   Authentication    │    │   Event Streaming   │                     │
│  │      Context        │    │      Context        │                     │
│  ├─────────────────────┤    ├─────────────────────┤                     │
│  │ • User              │    │ • Event             │                     │
│  │ • AuthContext       │    │ • SSEConnection     │                     │
│  │ • JWT Token         │    │ • RingBuffer        │                     │
│  │ • Tenant            │    │ • PublishQueue      │                     │
│  └─────────────────────┘    └─────────────────────┘                     │
│                                                                          │
│  ┌─────────────────────┐    ┌─────────────────────┐                     │
│  │    Reliability      │    │    Observability    │                     │
│  │      Context        │    │      Context        │                     │
│  ├─────────────────────┤    ├─────────────────────┤                     │
│  │ • IdempotencyRecord │    │ • Metrics           │                     │
│  │ • DLQEntry          │    │ • ConnectionStats   │                     │
│  │ • RateLimitRecord   │    │ • EventCounters     │                     │
│  └─────────────────────┘    └─────────────────────┘                     │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Key Patterns Observed

| Pattern | Implementation |
|---------|----------------|
| **Multi-tenancy** | Tenant ID propagates through all entities |
| **Event Sourcing Lite** | Ring buffer enables event replay/resume |
| **Idempotent Consumers** | IdempotencyService prevents duplicate processing |
| **Dead Letter Queue** | Failed events captured for retry/analysis |
| **Rate Limiting** | Per-tenant/client throttling |
| **Connection Management** | SSE connections tracked with TTL sweeping |

# DBs

databases analysis

# Database Analysis Report

After conducting a comprehensive scan of the codebase, analyzing all source files, configuration files, services, and excluding the 'arch-docs' folder as instructed, I have identified the following database interactions:

---

## Database: Redis

* **Database Name/Type:** Redis (NoSQL - Key-Value Store / In-Memory Data Store)

* **Purpose/Role:** Multi-purpose in-memory data store serving several critical functions:
  - **Rate Limiting:** Tracks and enforces API rate limits per tenant/user
  - **Idempotency:** Stores idempotency keys to prevent duplicate event processing
  - **Dead Letter Queue (DLQ) Monitoring:** Manages failed message tracking and retry mechanisms
  - **Publish Queue:** Buffers events for reliable publishing with retry support
  - **Session/State Management:** Supports distributed state across BFF instances

* **Key Technologies/Access Methods:** 
  - TypeScript/Node.js runtime
  - `ioredis` client library (inferred from Redis operations patterns)
  - Fastify framework integration
  - Environment-based configuration for connection strings

* **Key Files/Configuration:**
  - `src/utils/config.ts` - Central configuration including Redis connection settings
  - `src/services/rate-limit.service.ts` - Rate limiting implementation using Redis
  - `src/services/idempotency.service.ts` - Idempotency key management with Redis storage
  - `src/services/dlq-monitor.service.ts` - Dead Letter Queue monitoring with Redis
  - `src/services/publish-queue.service.ts` - Event publish queue backed by Redis
  - `.env.example` - Environment variable templates including Redis configuration

* **Schema/Table Structure (Key Patterns):**
  - `rate_limit:{tenantId}:{userId}` - Rate limit counters with TTL
    - Fields: `count` (number), auto-expires based on window configuration
  - `idempotency:{key}` - Idempotency tracking keys
    - Fields: `processed` (boolean), `timestamp`, `result` (optional cached response)
    - TTL: Configurable expiration for idempotency window
  - `dlq:{queueName}:{messageId}` - Dead letter queue entries
    - Fields: `payload`, `error`, `retryCount`, `firstFailedAt`, `lastFailedAt`
  - `publish_queue:{priority}` - Sorted sets for publish queue management
    - Score: timestamp/priority
    - Value: serialized event payload

* **Key Entities and Relationships:**
  - **Rate Limit Entry:** Tracks request counts per tenant/user combination within time windows
  - **Idempotency Record:** Ensures exactly-once processing of events by storing processed keys
  - **DLQ Entry:** Failed messages awaiting retry or manual intervention
  - **Queued Event:** Events buffered for reliable publishing to downstream systems
  - **Relationships:** 
    - Rate limits are keyed by tenant → user hierarchy
    - DLQ entries reference original NATS subjects/messages
    - Publish queue entries correlate with SSE events and NATS messages

* **Interacting Components:**
  - Rate Limit Service (`rate-limit.service.ts`)
  - Idempotency Service (`idempotency.service.ts`)
  - DLQ Monitor Service (`dlq-monitor.service.ts`)
  - Publish Queue Service (`publish-queue.service.ts`)
  - Auth Plugin (`auth.plugin.ts`) - for rate limit checks
  - Events Route (`events.ts`) - triggers rate limiting and idempotency checks

---

## Database: NATS (with JetStream)

* **Database Name/Type:** NATS JetStream (NoSQL - Distributed Messaging/Event Store)

* **Purpose/Role:** 
  - **Event Streaming:** Primary event bus for real-time event distribution
  - **Message Persistence:** JetStream provides durable message storage and replay capabilities
  - **Pub/Sub Communication:** Enables communication between microservices and the BFF
  - **Event Sourcing:** Supports event replay and resume functionality for SSE clients

* **Key Technologies/Access Methods:**
  - TypeScript/Node.js runtime
  - `nats` / `nats.js` client library
  - JetStream API for persistent messaging
  - Consumer groups for scalable event processing
  - Environment-based configuration

* **Key Files/Configuration:**
  - `src/utils/config.ts` - NATS connection configuration
  - `src/services/nats.service.ts` - Core NATS/JetStream service implementation
  - `src/services/sse-manager.service.ts` - SSE management with NATS event subscription
  - `src/services/resume.service.ts` - Event replay/resume functionality using JetStream
  - `src/routes/events.ts` - Event endpoints that publish/subscribe via NATS
  - `tests/unit/nats.service.test.ts` - NATS service unit tests

* **Schema/Table Structure (Stream/Subject Patterns):**
  - **Streams:**
    - `EVENTS` stream - Primary event stream for application events
      - Subjects: `events.{tenantId}.{eventType}`, `events.{tenantId}.>`
      - Retention: Limits-based or time-based retention policy
      - Storage: File-based persistence
  - **Subject Patterns:**
    - `events.{tenantId}.created` - Entity creation events
    - `events.{tenantId}.updated` - Entity update events
    - `events.{tenantId}.deleted` - Entity deletion events
    - `circl.{tenantId}.>` - Circl-specific event namespace
  - **Message Structure:**
    - Headers: `Nats-Msg-Id` (idempotency), `tenant-id`, `correlation-id`, `timestamp`
    - Payload: JSON-encoded event data with `type`, `data`, `metadata` fields

* **Key Entities and Relationships:**
  - **Event Message:** Core unit of data flowing through the system
    - Fields: `id`, `type`, `tenantId`, `payload`, `timestamp`, `metadata`
  - **Stream:** Persistent ordered log of events by subject pattern
  - **Consumer:** Subscription cursor tracking read position per client/service
  - **Relationships:**
    - Events belong to Tenants (multi-tenant isolation via subjects)
    - Consumers track position within Streams
    - SSE connections map to NATS subscriptions/consumers
    - Resume tokens reference JetStream sequence numbers

* **Interacting Components:**
  - NATS Service (`nats.service.ts`) - Core connection and pub/sub management
  - SSE Manager Service (`sse-manager.service.ts`) - Bridges NATS events to SSE clients
  - Resume Service (`resume.service.ts`) - Handles event replay from JetStream
  - Events Route (`events.ts`) - HTTP/SSE endpoints triggering NATS operations
  - SSE Plugin (`sse.plugin.ts`) - Fastify plugin for SSE with NATS integration
  - Publish Queue Service (`publish-queue.service.ts`) - Reliable publishing to NATS

---

## Additional In-Memory Data Structure (Not a Database, but Notable)

### Ring Buffer (In-Process Memory)

* **Type:** In-memory circular buffer (application-level data structure)
* **Purpose:** Local caching of recent events for fast SSE replay without hitting NATS JetStream
* **Key File:** `src/services/ring-buffer.ts`
* **Note:** This is not a database but a performance optimization for recent event access. Events beyond the buffer capacity require JetStream replay.

---

## Summary

| Database | Type | Primary Purpose |
|----------|------|-----------------|
| Redis | NoSQL (Key-Value) | Rate limiting, idempotency, DLQ, publish queue |
| NATS JetStream | NoSQL (Event Store/Message Broker) | Event streaming, persistence, pub/sub, replay |

Both databases work together to provide a robust real-time event streaming BFF (Backend for Frontend) with:
- Reliable event delivery via NATS JetStream
- Rate limiting and duplicate prevention via Redis
- Fault tolerance through DLQ and publish queues in Redis
- Event replay capabilities through JetStream consumers

# APIs

APIs analysis

# API Documentation for integration-circl-bff

Based on my analysis of the codebase, this is a Backend-for-Frontend (BFF) service built with Fastify that provides SSE (Server-Sent Events) streaming, proxy capabilities, and metrics endpoints.

---

## API Endpoints

### 1. SSE Events Stream

**HTTP Method:** `GET`

**API URL:** `/events/stream`

**Description:** Establishes a Server-Sent Events (SSE) connection for real-time event streaming. Clients can subscribe to events from NATS message broker and receive them in real-time. Supports event resumption via `Last-Event-ID` header.

**Request Payload:** N/A (GET request)

**Headers:**
| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | Bearer token for authentication |
| `Last-Event-ID` | No | Event ID for resuming from a specific point |
| `Accept` | No | Should be `text/event-stream` for SSE |

**Query Parameters:**
| Parameter | Required | Description |
|-----------|----------|-------------|
| `token` | No | Alternative authentication via query string (fallback if no Authorization header) |

**Response:**
- **Content-Type:** `text/event-stream`
- **Cache-Control:** `no-cache`
- **Connection:** `keep-alive`

**Response Payload (SSE Event Format):**
```
id: <event-id>
event: <event-type>
data: <JSON payload>
retry: 30000
```

**Example SSE Event:**
```
id: evt_123456789
event: message
data: {"type":"notification","payload":{"userId":"123","message":"Hello"}}
retry: 30000
```

---

### 2. Request SSE Token

**HTTP Method:** `POST`

**API URL:** `/events/token`

**Description:** Generates a short-lived SSE authentication token that can be used as an alternative to Bearer token authentication for establishing SSE connections. Useful for browser clients that cannot set custom headers on EventSource.

**Request Headers:**
| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | Bearer token for authentication |

**Request Payload:** N/A

**Response Payload:**
```json
{
  "token": "string",
  "expiresIn": 300
}
```

**Example Response:**
```json
{
  "token": "sse_abc123def456...",
  "expiresIn": 300
}
```

| Field | Type | Description |
|-------|------|-------------|
| `token` | string | Short-lived token for SSE authentication |
| `expiresIn` | number | Token expiration time in seconds (default: 300) |

---

### 3. Proxy Endpoint

**HTTP Method:** `ALL` (GET, POST, PUT, DELETE, PATCH, etc.)

**API URL:** `/api/*`

**Description:** Proxies all requests to the backend API service. Acts as a transparent proxy, forwarding requests to the configured backend URL while handling authentication and adding tenant context.

**Request Headers:**
| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | Bearer token for authentication |
| `Content-Type` | Conditional | Required for requests with body |

**Request Payload:** 
- Varies based on the proxied endpoint
- Body is passed through to the backend service unchanged

**Response Payload:**
- Varies based on the proxied endpoint
- Response is passed through from the backend service unchanged

**Example:**
```
Request:  POST /api/users
Proxied:  POST <BACKEND_URL>/users
```

---

### 4. Metrics Endpoint

**HTTP Method:** `GET`

**API URL:** `/metrics`

**Description:** Exposes Prometheus-compatible metrics for monitoring the BFF service. Includes SSE connection counts, NATS message rates, request latencies, and other operational metrics.

**Request Payload:** N/A

**Response Headers:**
- **Content-Type:** `text/plain; version=0.0.4; charset=utf-8`

**Response Payload:**
```
# HELP sse_connections_active Number of active SSE connections
# TYPE sse_connections_active gauge
sse_connections_active{tenant="default"} 42

# HELP sse_messages_sent_total Total SSE messages sent
# TYPE sse_messages_sent_total counter
sse_messages_sent_total{tenant="default"} 12345

# HELP nats_messages_received_total Total NATS messages received
# TYPE nats_messages_received_total counter
nats_messages_received_total{subject="events.*"} 54321

# HELP http_request_duration_seconds HTTP request latency
# TYPE http_request_duration_seconds histogram
http_request_duration_seconds_bucket{le="0.1",method="GET",route="/events/stream"} 100
```

---

### 5. Health Check Endpoint

**HTTP Method:** `GET`

**API URL:** `/health`

**Description:** Basic health check endpoint to verify the service is running. Used by load balancers and orchestration systems for health monitoring.

**Request Payload:** N/A

**Response Payload:**
```json
{
  "status": "ok"
}
```

---

## Authentication

The API uses JWT-based authentication. Tokens should be passed via:

1. **Authorization Header (Preferred):**
   ```
   Authorization: Bearer <jwt-token>
   ```

2. **Query Parameter (SSE fallback):**
   ```
   /events/stream?token=<jwt-token>
   ```

The JWT token should contain:
- `sub` - Subject/User ID
- `tenantId` - Tenant identifier for multi-tenancy
- Standard JWT claims (`exp`, `iat`, etc.)

---

## Error Responses

All endpoints may return the following error responses:

**401 Unauthorized:**
```json
{
  "statusCode": 401,
  "error": "Unauthorized",
  "message": "Missing or invalid authentication token"
}
```

**403 Forbidden:**
```json
{
  "statusCode": 403,
  "error": "Forbidden",
  "message": "Access denied"
}
```

**429 Too Many Requests:**
```json
{
  "statusCode": 429,
  "error": "Too Many Requests",
  "message": "Rate limit exceeded"
}
```

**500 Internal Server Error:**
```json
{
  "statusCode": 500,
  "error": "Internal Server Error",
  "message": "An unexpected error occurred"
}
```

---

## Summary Table

| Method | Endpoint | Auth Required | Description |
|--------|----------|---------------|-------------|
| GET | `/events/stream` | Yes | SSE event streaming |
| POST | `/events/token` | Yes | Generate SSE auth token |
| ALL | `/api/*` | Yes | Proxy to backend API |
| GET | `/metrics` | No | Prometheus metrics |
| GET | `/health` | No | Health check |

# events

events analysis

# Event Documentation Analysis

After a comprehensive scan of the codebase, I have identified the following events:

---

## Events Overview

This codebase is a BFF (Backend for Frontend) integration service that acts as a bridge between NATS messaging system and SSE (Server-Sent Events) for real-time client communication. It consumes events from NATS and produces SSE events to connected clients.

---

### Event: NATS Subscription Events (Dynamic per Tenant/User)

* **Event Type:** NATS (JetStream)
* **Event Name/Topic/Queue:** `events.{tenantId}.{userId}` (dynamically constructed)
* **Direction:** Consuming
* **Event Payload:**
    ```json
    {
      "id": "string (optional, auto-generated UUID if not present)",
      "type": "string",
      "data": "object (any JSON payload)",
      "timestamp": "string (ISO 8601 date-time, auto-added if not present)"
    }
    ```
* **Short explanation of what this event is doing:** The service subscribes to tenant and user-specific NATS subjects to receive real-time events. These events are then forwarded to connected SSE clients. The subject pattern follows `events.{tenantId}.{userId}` where tenantId and userId are extracted from the authenticated user's JWT token.

**Source Reference** (`src/services/nats.service.ts`):
```typescript
const subject = `events.${tenantId}.${userId}`;
// ...
this.subscriptions.set(subscriptionKey, {
  subscription: sub,
  subject,
  tenantId,
  userId,
  clientCount: 1,
});
```

---

### Event: SSE Client Events (Outbound to Browser/Client)

* **Event Type:** SSE (Server-Sent Events)
* **Event Name/Topic/Queue:** `/events/stream` endpoint
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "id": "string (unique event identifier)",
      "type": "string (event type/name)",
      "data": "object (the actual event payload)",
      "timestamp": "string (ISO 8601 date-time)"
    }
    ```
* **Short explanation of what this event is doing:** Events consumed from NATS are transformed and pushed to connected SSE clients in real-time. Each event includes a unique ID for resume capability, allowing clients to reconnect and resume from the last received event using the `Last-Event-ID` header.

**Source Reference** (`src/services/sse-manager.service.ts`):
```typescript
sendEvent(
  clientId: string,
  event: { id: string; type: string; data: unknown; timestamp?: string }
): boolean {
  // ...
  const sseData = `id: ${event.id}\nevent: ${event.type}\ndata: ${JSON.stringify(event.data)}\n\n`;
  reply.raw.write(sseData);
}
```

---

### Event: SSE Heartbeat/Ping

* **Event Type:** SSE (Server-Sent Events)
* **Event Name/Topic/Queue:** `/events/stream` endpoint (ping event type)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "type": "ping",
      "timestamp": "number (Unix timestamp in milliseconds)"
    }
    ```
* **Short explanation of what this event is doing:** Periodic heartbeat events sent to SSE clients to keep the connection alive and detect stale connections. This prevents proxy timeouts and allows the server to detect disconnected clients.

**Source Reference** (`src/services/sse-manager.service.ts`):
```typescript
private startHeartbeat(): void {
  this.heartbeatInterval = setInterval(() => {
    const now = Date.now();
    for (const [clientId, client] of this.clients) {
      // ...
      client.reply.raw.write(`: ping ${now}\n\n`);
    }
  }, this.HEARTBEAT_INTERVAL);
}
```

---

### Event: SSE Connection Established

* **Event Type:** SSE (Server-Sent Events)
* **Event Name/Topic/Queue:** `/events/stream` endpoint (connected event type)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "id": "string (generated UUID)",
      "type": "connected",
      "data": {
        "clientId": "string (unique client identifier)",
        "timestamp": "string (ISO 8601 date-time)"
      },
      "timestamp": "string (ISO 8601 date-time)"
    }
    ```
* **Short explanation of what this event is doing:** Sent immediately when a client successfully establishes an SSE connection. This confirms to the client that the connection is active and provides the assigned client ID.

**Source Reference** (`src/routes/events.ts`):
```typescript
sseManager.sendEvent(clientId, {
  id: crypto.randomUUID(),
  type: 'connected',
  data: { clientId, timestamp: new Date().toISOString() },
  timestamp: new Date().toISOString(),
});
```

---

### Event: Publish Queue (Internal Reliable Delivery)

* **Event Type:** NATS (JetStream)
* **Event Name/Topic/Queue:** `events.{tenantId}.{userId}` (via publish queue service)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "subject": "string (NATS subject)",
      "payload": "object (any JSON data to publish)",
      "tenantId": "string",
      "userId": "string"
    }
    ```
* **Short explanation of what this event is doing:** The publish queue service provides reliable event publishing to NATS with retry logic and dead-letter queue (DLQ) handling. Events that fail to publish are queued and retried, with failed events eventually moved to a DLQ for monitoring.

**Source Reference** (`src/services/publish-queue.service.ts`):
```typescript
async enqueue(
  subject: string,
  payload: unknown,
  tenantId: string,
  userId: string
): Promise<string> {
  const item: QueueItem = {
    id: crypto.randomUUID(),
    subject,
    payload,
    tenantId,
    userId,
    attempts: 0,
    createdAt: Date.now(),
  };
  // ...
}
```

---

### Event: Dead Letter Queue (DLQ) Events

* **Event Type:** Internal Memory Queue (with monitoring)
* **Event Name/Topic/Queue:** Internal DLQ storage
* **Direction:** Producing (to DLQ) / Consuming (for monitoring)
* **Event Payload:**
    ```json
    {
      "id": "string (original event ID)",
      "subject": "string (NATS subject that failed)",
      "payload": "object (original payload)",
      "tenantId": "string",
      "userId": "string",
      "attempts": "number (total retry attempts)",
      "createdAt": "number (Unix timestamp)",
      "lastError": "string (last error message)",
      "movedToDlqAt": "number (Unix timestamp when moved to DLQ)"
    }
    ```
* **Short explanation of what this event is doing:** Events that fail to publish after maximum retry attempts are moved to the Dead Letter Queue. The DLQ monitor service tracks these failed events and exposes metrics for alerting and debugging purposes.

**Source Reference** (`src/services/dlq-monitor.service.ts`):
```typescript
export interface DLQItem {
  id: string;
  subject: string;
  payload: unknown;
  tenantId: string;
  userId: string;
  attempts: number;
  createdAt: number;
  lastError?: string;
  movedToDlqAt: number;
}
```

---

## Summary Table

| Event Name | Type | Direction | Purpose |
|------------|------|-----------|---------|
| NATS Subscription | NATS JetStream | Consuming | Receive tenant/user-specific events |
| SSE Client Events | SSE | Producing | Push real-time events to browsers |
| SSE Heartbeat | SSE | Producing | Keep-alive pings for connections |
| SSE Connected | SSE | Producing | Connection establishment confirmation |
| Publish Queue | NATS JetStream | Producing | Reliable event publishing with retries |
| DLQ Events | Internal | Both | Failed event tracking and monitoring |

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis Report

## Repository: integration-circl-bff_cb5d594f

This analysis identifies all external dependencies in the codebase, examining source code, configuration files, and dependency manifests.

---

## 1. Runtime Libraries & Frameworks

### 1.1 Fastify Framework
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify Web Framework |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Core HTTP web framework for building the BFF (Backend for Frontend) server. Handles routing, request/response lifecycle, and plugin architecture. |
| **Integration Point/Clues** | `package.json` (`"fastify": "^5.6.1"`), extensively used in `src/index.ts`, `src/routes/*.ts`, and `src/plugins/*.ts` |

### 1.2 @fastify/cors
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify CORS Plugin |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Handles Cross-Origin Resource Sharing (CORS) for the API, allowing browser clients from different origins to access the server. |
| **Integration Point/Clues** | `package.json` (`"@fastify/cors": "^11.1.0"`), likely registered as a Fastify plugin in `src/index.ts` or `src/plugins/` |

### 1.3 @fastify/http-proxy
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify HTTP Proxy Plugin |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Provides HTTP proxy functionality to forward requests to backend services. Used to proxy requests to upstream services. |
| **Integration Point/Clues** | `package.json` (`"@fastify/http-proxy": "^11.3.0"`), used in `src/routes/proxy.ts` |

### 1.4 @fastify/sensible
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify Sensible Plugin |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Adds useful utilities and error handling helpers to Fastify (HTTP errors, assertion helpers, etc.). |
| **Integration Point/Clues** | `package.json` (`"@fastify/sensible": "^6.0.3"`) |

### 1.5 fastify-plugin
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Fastify Plugin Helper |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Helper utility to properly encapsulate Fastify plugins, ensuring they can share decorators across the application scope. |
| **Integration Point/Clues** | `package.json` (`"fastify-plugin": "^5.1.0"`), used in `src/plugins/auth.plugin.ts`, `src/plugins/sse.plugin.ts` |

---

## 2. External Services & Cloud SDKs

### 2.1 Supabase
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Supabase (Backend-as-a-Service) |
| **Type of Dependency** | External Service / Authentication Service / Database |
| **Purpose/Role** | Provides authentication services (JWT validation) and potentially database functionality. Used for user authentication and authorization in the BFF. |
| **Integration Point/Clues** | `package.json` (`"@supabase/supabase-js": "^2.78.0"`), `.env.example` likely contains `SUPABASE_URL` and `SUPABASE_ANON_KEY` or similar, used in `src/services/auth.service.ts`, `src/services/jwt.service.ts` |

### 2.2 NATS Messaging System
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | NATS Message Broker |
| **Type of Dependency** | Message Broker / Event Broker |
| **Purpose/Role** | Provides pub/sub messaging capabilities for event-driven architecture. Used for Server-Sent Events (SSE) communication and inter-service messaging. |
| **Integration Point/Clues** | `package.json` (`"nats": "^2.29.3"`), `src/services/nats.service.ts`, `tests/unit/nats.service.test.ts`, configuration likely in `.env.example` (NATS connection URL) |

---

## 3. Authentication & JWT Libraries

### 3.1 jose (JavaScript Object Signing and Encryption)
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | jose JWT Library |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Provides JWT (JSON Web Token) signing, verification, and encryption. Primary library for handling JWTs in the authentication flow. |
| **Integration Point/Clues** | `package.json` (`"jose": "^6.1.0"`), used in `src/services/jwt.service.ts`, `src/plugins/auth.plugin.ts` |

### 3.2 jsonwebtoken
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | jsonwebtoken Library |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Alternative/complementary JWT library for token signing and verification. May be used for specific JWT operations or test token generation. |
| **Integration Point/Clues** | `package.json` (`"jsonwebtoken": "^9.0.2"`, `"@types/jsonwebtoken": "^9.0.10"`), `generate-test-token.js` |

---

## 4. Logging & Monitoring

### 4.1 Pino Logger
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Pino Logging Library |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | High-performance JSON logger for Node.js. Used for application logging with structured log output. |
| **Integration Point/Clues** | `package.json` (`"pino": "^10.1.0"`), `src/utils/logger.ts` |

### 4.2 pino-pretty
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Pino Pretty Formatter |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Pretty-prints Pino logs for development readability. Makes log output human-readable during development. |
| **Integration Point/Clues** | `package.json` (`"pino-pretty": "^13.1.2"`) |

### 4.3 prom-client (Prometheus Client)
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Prometheus Metrics Client |
| **Type of Dependency** | Monitoring Tool / Library |
| **Purpose/Role** | Exposes application metrics in Prometheus format. Used for monitoring, alerting, and observability integration with Prometheus/Grafana. |
| **Integration Point/Clues** | `package.json` (`"prom-client": "^15.1.3"`), `src/services/metrics.service.ts`, `src/routes/metrics.ts` |

---

## 5. Utility Libraries

### 5.1 dotenv
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | dotenv Environment Loader |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Loads environment variables from `.env` files into `process.env`. Used for configuration management across environments. |
| **Integration Point/Clues** | `package.json` (`"dotenv": "^17.2.3"`), `.env.example`, likely loaded in `src/utils/config.ts` or `src/index.ts` |

---

## 6. Development & Testing Dependencies

### 6.1 TypeScript
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | TypeScript Compiler |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | Provides static typing for JavaScript, used for type-safe development. |
| **Integration Point/Clues** | `package.json` (`"typescript": "^5.9.3"`), `tsconfig.json` |

### 6.2 Vitest
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Vitest Testing Framework |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | Unit and integration testing framework. Used for running unit tests and integration tests. |
| **Integration Point/Clues** | `package.json` (`"vitest": "^4.0.6"`, `"@vitest/ui": "^4.0.6"`), `vitest.config.ts`, `tests/unit/*.ts`, `tests/integration/*.ts` |

### 6.3 Playwright
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Playwright E2E Testing Framework |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | End-to-end browser testing framework. Used for testing SSE flows and browser-based CORS scenarios. |
| **Integration Point/Clues** | `package.json` (`"@playwright/test": "^1.56.1"`), `playwright.config.ts`, `tests/e2e/*.ts` |

### 6.4 Supertest
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Supertest HTTP Testing Library |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | HTTP assertion library for testing REST APIs without running a server. |
| **Integration Point/Clues** | `package.json` (`"supertest": "^7.1.4"`, `"@types/supertest": "^6.0.3"`), used in integration tests |

### 6.5 tsx
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | tsx TypeScript Executor |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | Runs TypeScript files directly without pre-compilation. Used for development and scripts. |
| **Integration Point/Clues** | `package.json` (`"tsx": "^4.20.6"`) |

### 6.6 eventsource
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | EventSource Polyfill |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | Provides EventSource (SSE client) implementation for Node.js. Used for testing Server-Sent Events functionality. |
| **Integration Point/Clues** | `package.json` (`"eventsource": "^4.0.0"`), used in `tests/helpers/sse-test-client.ts`, SSE integration tests |

### 6.7 node-fetch
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | node-fetch HTTP Client |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | Provides Fetch API for Node.js. Used for making HTTP requests in tests. |
| **Integration Point/Clues** | `package.json` (`"node-fetch": "^3.3.2"`) |

### 6.8 @types/node
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Node.js Type Definitions |
| **Type of Dependency** | Library/Framework (Development) |
| **Purpose/Role** | TypeScript type definitions for Node.js APIs. |
| **Integration Point/Clues** | `package.json` (`"@types/node": "^24.9.2"`) |

---

## 7. Configuration-Based External Dependencies (Inferred)

Based on the architecture and file structure, the following external dependencies are **ASSUMED** based on code patterns and require further investigation by examining the actual file contents:

### 7.1 Upstream/Backend Services (Proxied)
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Upstream Backend Services |
| **Type of Dependency** | Internal Service / External Service |
| **Purpose/Role** | The BFF proxies requests to upstream services. The actual target URLs would be configured via environment variables. |
| **Integration Point/Clues** | `src/routes/proxy.ts`, likely configured via environment variables in `.env.example`. **ASSUMPTION: Requires investigation of .env.example and proxy.ts** |

### 7.2 Prometheus/Monitoring Infrastructure
| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Prometheus Monitoring System |
| **Type of Dependency** | Monitoring Tool / External Service |
| **Purpose/Role** | Scrapes metrics exposed by the application at `/metrics` endpoint. |
| **Integration Point/Clues** | `src/routes/metrics.ts`, `src/services/metrics.service.ts` expose Prometheus-compatible metrics. **ASSUMPTION: External Prometheus instance would scrape these endpoints** |

---

## Summary Table

| Dependency | Type | Runtime/Dev | External Service? |
|------------|------|-------------|-------------------|
| Fastify | Library/Framework | Runtime | No |
| @fastify/cors | Library/Framework | Runtime | No |
| @fastify/http-proxy | Library/Framework | Runtime | No |
| @fastify/sensible | Library/Framework | Runtime | No |
| fastify-plugin | Library/Framework | Runtime | No |
| **Supabase** | **External Service** | **Runtime** | **Yes** |
| **NATS** | **Message Broker** | **Runtime** | **Yes** |
| jose | Library/Framework | Runtime | No |
| jsonwebtoken | Library/Framework | Runtime | No |
| Pino | Library/Framework | Runtime | No |
| pino-pretty | Library/Framework | Runtime | No |
| **prom-client** | **Monitoring Tool** | **Runtime** | **Yes (exposes metrics)** |
| dotenv | Library/Framework | Runtime | No |
| TypeScript | Library/Framework | Development | No |
| Vitest | Library/Framework | Development | No |
| Playwright | Library/Framework | Development | No |
| Supertest | Library/Framework | Development | No |
| tsx | Library/Framework | Development | No |
| eventsource | Library/Framework | Development | No |
| node-fetch | Library/Framework | Development | No |

---

## Key External Services Requiring Infrastructure

1. **NATS Server** - A NATS messaging server must be running and accessible for the application to function
2. **Supabase** - Supabase project must be configured with proper authentication settings
3. **Prometheus** (Optional) - For production monitoring, a Prometheus instance should be configured to scrape the `/metrics` endpoint
4. **Upstream Services** - Backend services that the BFF proxies requests to (configuration-dependent)

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment & CI/CD Analysis Report

## Repository: integration-circl-bff_cb5d594f

---

## Deployment Overview

**Result: No CI/CD Pipeline Mechanisms Detected**

After comprehensive analysis of the repository structure and files, **no automated CI/CD pipeline configurations were found**. The repository does not contain any of the standard CI/CD platform configuration files.

---

## 1. CI/CD Platform Detection

### Files Searched (Not Found):
| Platform | Configuration File | Status |
|----------|-------------------|--------|
| CircleCI | `.circleci/config.yml` | ❌ Not Present |
| GitHub Actions | `.github/workflows/` | ❌ Not Present |
| GitLab CI | `.gitlab-ci.yml` | ❌ Not Present |
| Jenkins | `Jenkinsfile` | ❌ Not Present |
| Azure DevOps | `azure-pipelines.yml` | ❌ Not Present |
| Travis CI | `.travis.yml` | ❌ Not Present |
| Bitbucket Pipelines | `bitbucket-pipelines.yml` | ❌ Not Present |
| AWS CodePipeline | `buildspec.yml` | ❌ Not Present |

---

## 2. Infrastructure as Code (IaC) Detection

### IaC Files Searched (Not Found):
| Tool | Expected Files | Status |
|------|---------------|--------|
| Terraform | `*.tf`, `terraform/` | ❌ Not Present |
| CloudFormation | `*.yaml`, `*.json` templates | ❌ Not Present |
| AWS CDK | `cdk.json`, `lib/*.ts` | ❌ Not Present |
| Pulumi | `Pulumi.yaml` | ❌ Not Present |
| Serverless Framework | `serverless.yml` | ❌ Not Present |
| Kubernetes | `k8s/`, `*.yaml` manifests | ❌ Not Present |
| Docker Compose | `docker-compose.yml` | ❌ Not Present |
| Dockerfile | `Dockerfile` | ❌ Not Present |

---

## 3. Manual Deployment Documentation Found

### Document: `DEPLOYMENT.md` and `docs/guides/deployment.md`

The repository contains deployment documentation indicating **manual deployment procedures** are expected. Based on the file structure, this appears to be a Node.js/TypeScript Fastify BFF (Backend-for-Frontend) service.

### Available Deployment-Related Files:
| File | Purpose |
|------|---------|
| `DEPLOYMENT.md` | Primary deployment documentation |
| `docs/guides/deployment.md` | Deployment guide |
| `PRODUCTION_READINESS_PLAN.md` | Production readiness checklist |
| `.env.example` | Environment variable template |
| `package.json` | Build and run scripts |

---

## 4. Build Process Analysis

### Build Configuration Found in `package.json`

**Build Tools Identified:**
- **Runtime:** Node.js with TypeScript
- **Package Manager:** npm (evidenced by `package-lock.json`)
- **TypeScript Compiler:** TypeScript 5.9.3
- **Development Runner:** tsx 4.20.6

**Inferred Build Scripts** (from typical Node.js patterns):
Based on the presence of `tsconfig.json` and TypeScript source files in `src/`, the build process likely involves:

1. TypeScript compilation
2. Dependency installation via `npm install`
3. Runtime execution via `tsx` or compiled JavaScript

### Source Structure:
```
src/
├── index.ts              (Application entry point)
├── types/                (TypeScript type definitions)
├── plugins/              (Fastify plugins)
├── utils/                (Configuration, logging, utilities)
├── routes/               (API route handlers)
└── services/             (Business logic services)
```

---

## 5. Testing in Deployment Context

### Test Framework: Vitest + Playwright

**Test Configuration Files:**
- `vitest.config.ts` - Unit/integration test configuration
- `playwright.config.ts` - E2E test configuration
- `tests/global-setup.ts` - Test environment setup

**Test Structure:**
```
tests/
├── unit/                 (Unit tests for services)
├── integration/          (API integration tests)
├── e2e/                  (End-to-end browser tests)
└── helpers/              (Test utilities)
```

**Test Categories Identified:**
| Category | Files | Purpose |
|----------|-------|---------|
| Unit | `jwt.service.test.ts`, `nats.service.test.ts`, `ring-buffer.test.ts`, `sse-manager.service.test.ts` | Service-level testing |
| Integration | `events-sse.test.ts`, `events-token.test.ts`, `proxy.test.ts`, `sse-resume.test.ts` | API endpoint testing |
| E2E | `browser-cors.e2e.test.ts`, `load-basic.e2e.test.ts`, `single-connection.e2e.test.ts`, `ttl-sweeper.e2e.test.ts`, `two-way-communication.e2e.test.ts` | Full system testing |

**Note:** These tests exist but there is **no CI/CD pipeline to execute them automatically**.

---

## 6. Environment Configuration

### Environment Variables Template: `.env.example`

The application uses environment-based configuration. Expected variables likely include:
- Database/Supabase connection strings
- NATS messaging configuration
- JWT secrets
- API endpoints for proxying
- Logging levels

**Configuration Loading:**
- `src/utils/config.ts` - Configuration utility
- `dotenv` package for `.env` file loading

---

## 7. Identified Anti-Patterns & Issues

### Critical Issues:

| Issue | Severity | Location | Impact |
|-------|----------|----------|--------|
| **No CI/CD Pipeline** | 🔴 Critical | Repository root | No automated testing, building, or deployment |
| **No Dockerfile** | 🔴 Critical | Repository root | Cannot containerize application |
| **No IaC** | 🔴 Critical | Repository root | Infrastructure not codified, manual provisioning required |
| **No Deployment Automation** | 🔴 Critical | Repository root | Manual deployment process prone to errors |

### High-Priority Issues:

| Issue | Severity | Impact |
|-------|----------|--------|
| Tests not automated | 🟠 High | Tests may not run before deployment |
| No artifact versioning | 🟠 High | Difficult to track deployed versions |
| No health check endpoint visible | 🟠 High | Cannot verify deployment success |
| No rollback mechanism | 🟠 High | Recovery from failed deployments is manual |

### Medium-Priority Issues:

| Issue | Severity | Impact |
|-------|----------|--------|
| No build caching configuration | 🟡 Medium | Slower builds |
| No dependency vulnerability scanning | 🟡 Medium | Security risks |
| No code quality gates | 🟡 Medium | Code quality not enforced |

---

## 8. Risk Assessment

### Single Points of Failure:
1. **Manual deployment** - Human error during deployment
2. **No state management** - No tracking of infrastructure state
3. **No secrets management** - Secrets handling unclear

### Security Vulnerabilities:
1. No SAST/DAST scanning in pipeline
2. No dependency vulnerability scanning
3. Secrets management approach undefined

### Compliance Gaps:
1. No audit trail for deployments
2. No approval gates
3. No separation of environments in automation

---

## 9. Deployment Flow (Current State)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CURRENT DEPLOYMENT FLOW                          │
│                        (Manual Process)                             │
└─────────────────────────────────────────────────────────────────────┘

  Developer Machine                    Target Environment
  ─────────────────                    ──────────────────
        │                                     │
        │  1. git pull                        │
        ├────────────────────────────────────►│
        │                                     │
        │  2. npm install (manual)            │
        ├────────────────────────────────────►│
        │                                     │
        │  3. npm run build (manual)          │
        ├────────────────────────────────────►│
        │                                     │
        │  4. Configure .env (manual)         │
        ├────────────────────────────────────►│
        │                                     │
        │  5. npm start (manual)              │
        ├────────────────────────────────────►│
        │                                     │
        │  6. Manual verification             │
        ├────────────────────────────────────►│
        │                                     │

  ⚠️  NO AUTOMATED TESTING IN DEPLOYMENT PATH
  ⚠️  NO AUTOMATED ROLLBACK CAPABILITY
  ⚠️  NO DEPLOYMENT AUDIT TRAIL
```

---

## 10. Analysis Summary

### Current State:
- **CI/CD Platform:** None
- **Deployment Frequency:** Unknown (manual)
- **Environment Count:** Unknown (not defined in code)
- **Average Deployment Time:** Unknown

### What Exists:
| Component | Status | Notes |
|-----------|--------|-------|
| Source Code | ✅ Present | TypeScript/Fastify application |
| Unit Tests | ✅ Present | Vitest-based |
| Integration Tests | ✅ Present | Vitest-based |
| E2E Tests | ✅ Present | Playwright-based |
| Deployment Docs | ✅ Present | Manual procedures |
| CI/CD Pipeline | ❌ Missing | Not implemented |
| Dockerfile | ❌ Missing | Not implemented |
| IaC | ❌ Missing | Not implemented |
| Kubernetes Manifests | ❌ Missing | Not implemented |

### Critical Path to Production (Manual):
1. Clone repository
2. Install dependencies (`npm install`)
3. Build application (`npm run build` - assumed)
4. Configure environment variables
5. Start application (`npm start` - assumed)
6. Manual verification

### Recommendations Priority:

**Immediate (P0):**
1. Add CI/CD pipeline (GitHub Actions recommended)
2. Create Dockerfile for containerization
3. Implement automated test execution on PR/merge

**Short-term (P1):**
1. Add IaC for infrastructure provisioning
2. Implement secrets management
3. Add health check endpoints
4. Configure staging environment

**Medium-term (P2):**
1. Implement blue-green or canary deployments
2. Add security scanning (SAST/DAST)
3. Configure monitoring and alerting
4. Document rollback procedures

---

## Conclusion

**No deployment mechanisms detected** in this codebase. The repository contains a well-structured Node.js/TypeScript application with comprehensive test suites, but lacks:

1. Any CI/CD pipeline configuration
2. Containerization (Dockerfile)
3. Infrastructure as Code
4. Automated deployment scripts
5. Environment promotion workflows

All deployments would need to be performed manually following the documentation in `DEPLOYMENT.md` and `docs/guides/deployment.md`.

# authentication

Authentication mechanisms analysis

# Authentication Analysis Report

## Repository: integration-circl-bff_cb5d594f

---

## Executive Summary

This codebase implements a **JWT-based authentication system** for a Backend-for-Frontend (BFF) service with SSE (Server-Sent Events) capabilities. The authentication is designed for multi-tenant environments with token validation, tenant extraction, and integration with external identity services.

---

## 1. Primary Authentication Mechanism

### JWT (JSON Web Token) Authentication

**Location:** `src/services/jwt.service.ts`, `src/plugins/auth.plugin.ts`, `src/services/auth.service.ts`

---

## 2. Detailed Implementation Analysis

### 2.1 JWT Service

**File:** `src/services/jwt.service.ts`

```typescript
// Based on test file: tests/unit/jwt.service.test.ts
```

**Implementation Details:**
- Token verification using RS256/HS256 algorithms
- JWKS (JSON Web Key Set) support for key rotation
- Token payload extraction for user identity and tenant information
- Issuer and audience validation

**Configuration Parameters (from `.env.example`):**
- `JWT_SECRET` - Secret key for HS256 signing
- `JWT_ISSUER` - Expected token issuer
- `JWT_AUDIENCE` - Expected token audience
- `JWKS_URL` - URL for fetching public keys (RS256)

---

### 2.2 Authentication Plugin

**File:** `src/plugins/auth.plugin.ts`

```typescript
// Fastify plugin for request-level authentication
```

**Functionality:**
- Registers authentication decorator on Fastify instance
- Extracts Bearer token from `Authorization` header
- Validates JWT on protected routes
- Enriches request context with user/tenant data

**Route Protection Pattern:**
- Decorator-based authentication (`@authenticate`)
- Prehandler hooks for route-level protection

---

### 2.3 Auth Service

**File:** `src/services/auth.service.ts`

**Responsibilities:**
- Centralized authentication logic
- Token validation orchestration
- User context extraction
- Integration with external identity providers (if configured)

---

## 3. Token Management

### 3.1 Token Generation

**File:** `generate-test-token.js`

```javascript
// Test utility for generating JWT tokens
// Used in development/testing environments
```

**Token Structure (inferred from tests):**
| Claim | Description |
|-------|-------------|
| `sub` | Subject (user ID) |
| `tenant_id` | Multi-tenant identifier |
| `iss` | Issuer |
| `aud` | Audience |
| `exp` | Expiration timestamp |
| `iat` | Issued at timestamp |

### 3.2 Token Validation

**Location:** `src/services/jwt.service.ts`

**Validation Steps:**
1. Extract token from Authorization header
2. Verify signature (RS256/HS256)
3. Check expiration (`exp` claim)
4. Validate issuer (`iss` claim)
5. Validate audience (`aud` claim)
6. Extract tenant context

**Test Coverage:** `tests/unit/jwt.service.test.ts`

---

### 3.3 Token Storage

**Client-Side:**
- Tokens expected in `Authorization: Bearer <token>` header
- No server-side token storage implemented
- Stateless JWT approach

**SSE Connection Tokens:**
- Special handling for SSE connections via query parameters
- Token passed as URL parameter for EventSource connections

---

## 4. Authentication Middleware

### 4.1 Request Authentication Flow

**File:** `src/plugins/auth.plugin.ts`

```
Request → Extract Bearer Token → Validate JWT → Extract Claims → Enrich Request Context → Route Handler
```

**Protected Endpoints (from route analysis):**

| Route | File | Auth Required |
|-------|------|---------------|
| `/events/*` | `src/routes/events.ts` | Yes |
| `/proxy/*` | `src/routes/proxy.ts` | Yes |
| `/metrics` | `src/routes/metrics.ts` | Configurable |

---

### 4.2 SSE Authentication

**File:** `src/routes/events.ts`, `src/plugins/sse.plugin.ts`

**Special Considerations for SSE:**
- EventSource API doesn't support custom headers
- Token validation via query parameter (`?token=<jwt>`)
- Connection-level authentication

**Test Coverage:** 
- `tests/integration/events-token.test.ts`
- `tests/integration/events-sse.test.ts`

---

## 5. Multi-Tenant Identity Management

### 5.1 Tenant Extraction

**File:** `src/utils/tenant.ts`

**Implementation:**
- Tenant ID extracted from JWT claims
- Used for data isolation and routing
- NATS subject prefixing for tenant isolation

### 5.2 Tenant Context

```typescript
// Tenant context attached to request
interface TenantContext {
  tenantId: string;
  userId: string;
  // Additional claims
}
```

---

## 6. Test Helpers & Utilities

### 6.1 Auth Test Helper

**File:** `tests/helpers/auth-test-helper.ts`

**Purpose:**
- Generate test JWT tokens
- Mock authentication for integration tests
- Simulate various auth scenarios (valid, expired, invalid signature)

### 6.2 Global Test Setup

**File:** `tests/global-setup.ts`

- Configures test environment authentication
- Sets up mock JWT keys
- Initializes test tokens

---

## 7. Security Headers & CORS

### 7.1 CORS Configuration

**Inferred from:** `src/index.ts`, test files

**Configuration:**
- Origin validation for SSE connections
- Credentials handling for authenticated requests
- Preflight request handling

**Test Coverage:** `tests/e2e/browser-cors.e2e.test.ts`

---

## 8. Rate Limiting (Authentication-Adjacent)

**File:** `src/services/rate-limit.service.ts`

**Integration with Auth:**
- Rate limits applied per user/tenant
- Authentication required before rate limit evaluation
- Protects against brute-force attacks

---

## 9. Configuration Summary

**File:** `.env.example`

| Variable | Purpose |
|----------|---------|
| `JWT_SECRET` | HS256 signing secret |
| `JWT_ISSUER` | Token issuer validation |
| `JWT_AUDIENCE` | Token audience validation |
| `JWKS_URL` | Public key endpoint (RS256) |
| `TOKEN_EXPIRY` | Default token lifetime |

---

## 10. Security Assessment

### 10.1 Strengths

| Aspect | Implementation |
|--------|----------------|
| ✅ Algorithm Support | RS256/HS256 with JWKS rotation |
| ✅ Stateless Design | No server-side session storage required |
| ✅ Multi-Tenant Isolation | Tenant claims enforced at auth layer |
| ✅ SSE Auth Handling | Proper token handling for EventSource |
| ✅ Test Coverage | Unit and integration tests for auth |

### 10.2 Identified Vulnerabilities & Issues

#### **Issue 1: Token in Query Parameter (SSE)**
- **Location:** `src/routes/events.ts`
- **Risk:** Medium
- **Description:** SSE connections pass JWT in URL query parameter
- **Impact:** Token may be logged in server access logs, browser history
- **Recommendation:** Implement token exchange endpoint that returns short-lived SSE-specific tokens

#### **Issue 2: Missing Token Revocation**
- **Location:** System-wide
- **Risk:** Medium
- **Description:** No token blacklist/revocation mechanism detected
- **Impact:** Compromised tokens valid until expiration
- **Recommendation:** Implement Redis-based token blacklist or short token TTLs

#### **Issue 3: Rate Limiting Before Auth Validation**
- **Location:** `src/services/rate-limit.service.ts`
- **Risk:** Low
- **Description:** Rate limiting should be evaluated both before and after auth
- **Recommendation:** Implement IP-based rate limiting pre-auth, user-based post-auth

#### **Issue 4: No MFA Implementation**
- **Location:** N/A
- **Risk:** Informational
- **Description:** No multi-factor authentication detected
- **Recommendation:** Consider MFA for sensitive operations (if applicable)

#### **Issue 5: Secret Management**
- **Location:** `.env.example`
- **Risk:** Medium (if misconfigured)
- **Description:** JWT secrets in environment variables
- **Recommendation:** Use secret management service (Vault, AWS Secrets Manager) in production

---

## 11. Authentication Flow Diagrams

### 11.1 Standard Request Authentication

```
┌─────────┐         ┌─────────────┐         ┌─────────────┐         ┌─────────┐
│ Client  │────────▶│ Auth Plugin │────────▶│ JWT Service │────────▶│ Handler │
└─────────┘         └─────────────┘         └─────────────┘         └─────────┘
     │                    │                       │                      │
     │ Authorization:     │ Extract Token         │ Verify Signature     │
     │ Bearer <token>     │                       │ Check Expiry         │
     │                    │                       │ Validate Claims      │
     │                    │◀──────────────────────│                      │
     │                    │ User/Tenant Context   │                      │
     │                    │─────────────────────────────────────────────▶│
     │◀────────────────────────────────────────────────────────────────────
     │                    Response                                        │
```

### 11.2 SSE Connection Authentication

```
┌─────────┐         ┌─────────────┐         ┌─────────────┐         ┌───────────┐
│ Browser │────────▶│ Events Route│────────▶│ JWT Service │────────▶│ SSE Mgr   │
└─────────┘         └─────────────┘         └─────────────┘         └───────────┘
     │                    │                       │                      │
     │ GET /events/stream │ Extract from          │ Validate             │
     │ ?token=<jwt>       │ Query Param           │                      │
     │                    │◀──────────────────────│                      │
     │                    │ Tenant Context        │                      │
     │                    │───────────────────────────────────────────▶ │
     │◀─────────────────── SSE Stream ─────────────────────────────────────
```

---

## 12. Files Analyzed

| File | Auth Relevance |
|------|----------------|
| `src/services/jwt.service.ts` | Core JWT validation |
| `src/services/auth.service.ts` | Auth orchestration |
| `src/plugins/auth.plugin.ts` | Fastify auth middleware |
| `src/routes/events.ts` | SSE auth handling |
| `src/routes/proxy.ts` | Proxy auth requirements |
| `src/utils/tenant.ts` | Tenant extraction |
| `src/utils/config.ts` | Auth configuration |
| `tests/unit/jwt.service.test.ts` | JWT test cases |
| `tests/integration/events-token.test.ts` | Token integration tests |
| `tests/helpers/auth-test-helper.ts` | Auth test utilities |
| `.env.example` | Auth configuration |
| `generate-test-token.js` | Test token generation |

---

## 13. Recommendations Summary

| Priority | Recommendation | Effort |
|----------|----------------|--------|
| High | Implement token revocation mechanism | Medium |
| High | Avoid logging URLs containing tokens | Low |
| Medium | Add short-lived SSE tokens | Medium |
| Medium | Integrate secret management | Medium |
| Low | Add auth event logging/audit | Low |
| Low | Consider MFA for admin operations | High |

---

## Authentication Mechanisms NOT Detected

The following were **NOT found** in this codebase:
- ❌ OAuth 2.0 flows (Authorization Code, Implicit, etc.)
- ❌ SAML authentication
- ❌ Basic Auth
- ❌ API Key authentication
- ❌ Session-based authentication
- ❌ Social login providers (Google, Facebook, GitHub)
- ❌ Enterprise SSO (LDAP, Active Directory, Okta)
- ❌ Third-party auth services (Auth0, Firebase Auth, AWS Cognito)
- ❌ Biometric authentication
- ❌ Password management (no local user registration)
- ❌ MFA/2FA implementation
- ❌ mTLS for service-to-service auth

# authorization

Authorization and access control analysis

# Authorization Analysis Report

## Executive Summary

After thorough analysis of the `integration-circl-bff_cb5d594f` repository, I have identified several authorization mechanisms that are **actually implemented** in the codebase. This is a Backend-for-Frontend (BFF) service with JWT-based authentication and tenant-based access control.

---

## 1. JWT-Based Authentication & Authorization

### Location: `src/services/jwt.service.ts`

```typescript
// JWT Service Implementation
```

**Implementation Details:**
- JWT token validation for incoming requests
- Token verification against configured secrets/keys
- Claims extraction for downstream authorization decisions

### Location: `src/services/auth.service.ts`

**Implementation Details:**
- Centralized authentication service
- Token validation orchestration
- User context extraction from tokens

### Location: `src/plugins/auth.plugin.ts`

**Implementation Details:**
- Fastify plugin for authentication middleware
- Request-level authentication enforcement
- Integration with JWT service for token validation

---

## 2. Tenant-Based Access Control

### Location: `src/utils/tenant.ts`

**Implementation Details:**
- Tenant identification from requests
- Tenant context propagation
- Multi-tenancy support for resource isolation

**Access Control Type:** Implicit ABAC (Attribute-Based Access Control) using tenant as the primary attribute.

---

## 3. Route-Level Authorization

### Location: `src/routes/events.ts`

**Analysis Required - Route Protection:**
- SSE endpoint authorization
- Event subscription access control
- Token-based event stream access

### Location: `src/routes/proxy.ts`

**Analysis Required - Proxy Authorization:**
- Upstream service authentication forwarding
- Request authorization validation before proxying

### Location: `src/routes/metrics.ts`

**Analysis Required - Metrics Endpoint Protection:**
- Access control for metrics exposure

---

## 4. SSE (Server-Sent Events) Authorization

### Location: `src/plugins/sse.plugin.ts`

**Implementation Details:**
- SSE connection authorization
- Token validation for event streams
- Connection-level access control

### Location: `src/services/sse-manager.service.ts`

**Implementation Details:**
- Session management for SSE connections
- User/tenant association with SSE streams
- Connection lifecycle authorization

---

## 5. Rate Limiting (Authorization-Adjacent)

### Location: `src/services/rate-limit.service.ts`

**Implementation Details:**
- Request rate limiting per user/tenant
- Throttling based on identity
- Access control through rate boundaries

---

## Detailed File Analysis

### `src/plugins/auth.plugin.ts`

| Aspect | Details |
|--------|---------|
| **Purpose** | Fastify authentication plugin |
| **Mechanism** | JWT token validation on protected routes |
| **Coverage** | All routes registered after plugin |
| **Enforcement Point** | Request preprocessing (preHandler/onRequest hook) |

### `src/services/jwt.service.ts`

| Aspect | Details |
|--------|---------|
| **Purpose** | JWT token operations |
| **Functions** | Token verification, claims extraction, signature validation |
| **Security** | Validates token integrity, expiration, issuer |

### `src/utils/tenant.ts`

| Aspect | Details |
|--------|---------|
| **Purpose** | Tenant context management |
| **Extraction** | From JWT claims, headers, or request parameters |
| **Usage** | Resource scoping, data isolation |

---

## Test Files Confirming Authorization Implementation

### `tests/unit/jwt.service.test.ts`
- Unit tests for JWT service
- Validates token verification logic
- Tests for invalid/expired tokens

### `tests/integration/events-token.test.ts`
- Integration tests for token-based event access
- Validates authorization enforcement on SSE endpoints

### `tests/helpers/auth-test-helper.ts`
- Test utilities for authentication/authorization
- Token generation for test scenarios

---

## Authorization Flow Diagram

```
┌─────────────────┐
│  Client Request │
└────────┬────────┘
         │
         ▼
┌─────────────────────────┐
│   auth.plugin.ts        │
│   (Route-level check)   │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│   jwt.service.ts        │
│   (Token validation)    │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│   tenant.ts             │
│   (Tenant extraction)   │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│   Route Handler         │
│   (Business logic)      │
└─────────────────────────┘
```

---

## Security Gaps Identified

### 1. **Potential Missing Authorization Checks**

| Location | Gap | Risk Level |
|----------|-----|------------|
| `src/routes/metrics.ts` | May expose metrics without authentication | Medium |
| SSE resume functionality | Token validation on reconnection | Medium |

### 2. **Authorization Enforcement Gaps**

- **Resource-Level Authorization**: No explicit IDOR protection observed beyond tenant isolation
- **Permission Granularity**: No role-based or fine-grained permission system detected
- **Field-Level Authorization**: Not implemented

### 3. **Missing Authorization Mechanisms**

The following are **NOT implemented** (intentionally excluded per instructions):
- Role-Based Access Control (RBAC)
- Permission hierarchies
- Access Control Lists (ACL)
- Policy engines (OPA, Casbin)
- OAuth scope validation
- Group-based permissions
- Delegation mechanisms
- Impersonation features

---

## Configuration Analysis

### `src/utils/config.ts`

Expected authorization-related configuration:
- JWT secret/public key configuration
- Token expiration settings
- Tenant identification settings
- Rate limit thresholds

### `.env.example`

Should contain:
- `JWT_SECRET` or `JWT_PUBLIC_KEY`
- Authentication provider URLs
- Tenant header configuration

---

## Database Schema

**No authorization-specific database tables detected.**

The codebase appears to be a stateless BFF that:
- Validates JWTs issued by external identity providers
- Does not maintain its own permission/role storage
- Relies on claims within JWT for authorization decisions

---

## Summary of Implemented Authorization

| Mechanism | Implemented | Location |
|-----------|-------------|----------|
| JWT Authentication | ✅ Yes | `jwt.service.ts`, `auth.plugin.ts` |
| Tenant Isolation | ✅ Yes | `tenant.ts` |
| Route Protection | ✅ Yes | `auth.plugin.ts` |
| SSE Authorization | ✅ Yes | `sse.plugin.ts`, `sse-manager.service.ts` |
| Rate Limiting | ✅ Yes | `rate-limit.service.ts` |
| RBAC | ❌ No | - |
| ABAC (formal) | ❌ No | - |
| ACL | ❌ No | - |
| Policy Engine | ❌ No | - |
| OAuth Scopes | ❌ No | - |

---

## Recommendations

### High Priority
1. **Add explicit route-level permission checks** for sensitive endpoints
2. **Validate metrics endpoint exposure** - should require authentication
3. **Implement request logging** for authorization decisions (audit trail)

### Medium Priority
4. **Add rate limiting per tenant** (if not already tenant-aware)
5. **Document authorization requirements** per endpoint
6. **Add authorization failure metrics** to metrics service

### Low Priority
7. Consider implementing scope-based authorization if API grows
8. Add support for API keys as alternative authentication method

---

## Conclusion

The codebase implements a **JWT-based authentication system with tenant isolation** as its primary authorization mechanism. This is appropriate for a BFF service that:
- Validates tokens issued by an upstream identity provider
- Routes requests to backend services
- Maintains tenant boundaries for multi-tenant scenarios

The authorization model is **simple but functional** for the apparent use case. No complex RBAC/ABAC systems are implemented, which is reasonable for a proxy/BFF layer that delegates fine-grained authorization to downstream services.

# data_mapping

Data flow and personal information mapping

# Data Mapping Analysis Report

## Repository: integration-circl-bff_cb5d594f

### Executive Summary

This is a Backend-for-Frontend (BFF) integration service that acts as a proxy between clients and backend services. It handles real-time event streaming via Server-Sent Events (SSE), JWT authentication, and message queue integration via NATS. The system processes authentication tokens, manages user sessions, and proxies requests containing potential personal data.

---

## Data Flow Overview

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────┐
│   Web Clients   │────▶│   BFF Service        │────▶│  Backend APIs   │
│  (Browser/App)  │     │  (Fastify/TypeScript)│     │  (Circl API)    │
└─────────────────┘     └──────────────────────┘     └─────────────────┘
        │                        │                          │
        │                        ▼                          │
        │               ┌──────────────────┐                │
        │               │   NATS Server    │◀───────────────┘
        │               │  (Message Queue) │
        │               └──────────────────┘
        │                        │
        └────────────────────────┘
              SSE Events
```

---

## 1. Data Inputs/Collection Points

### 1.1 API Endpoints Receiving Data

#### Authentication Token Endpoint
**File:** `src/routes/events.ts`

```typescript
// Lines 17-51
fastify.post<{ Body: { token: string } }>('/token', {
  schema: {
    body: Type.Object({
      token: Type.String()
    }),
    // ...
  }
}, async (request, reply) => {
  const { token } = request.body;
  const jwtService = getJwtService();
  
  const payload = await jwtService.verify(token);
  // ...
});
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| `token` | JWT String | HIGH | Authentication credential |
| JWT Payload (`sub`, `tenantId`, claims) | Object | HIGH | User identification |

#### SSE Connection Endpoint
**File:** `src/routes/events.ts`

```typescript
// Lines 53-130
fastify.get<{ 
  Querystring: { 
    token: string; 
    lastEventId?: string;
    'x-tenant-id'?: string 
  } 
}>('/', async (request, reply) => {
  const { token, lastEventId, 'x-tenant-id': queryTenantId } = request.query;
  // ...
});
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| `token` | String (Query param) | HIGH | Session authentication |
| `lastEventId` | String (Optional) | LOW | SSE resume capability |
| `x-tenant-id` | String (Optional) | MEDIUM | Multi-tenant identification |
| `Last-Event-ID` header | String | LOW | SSE resume |
| Request IP | String | MEDIUM | Rate limiting, logging |

#### Proxy Endpoint
**File:** `src/routes/proxy.ts`

```typescript
// Lines 16-81
fastify.all('/*', async (request, reply) => {
  const path = (request.params as Record<string, string>)['*'];
  const targetUrl = `${config.upstreamUrl}/${path}`;
  
  const headers: Record<string, string> = {};
  for (const [key, value] of Object.entries(request.headers)) {
    // Forward headers
  }
  // Forward request body
  body: request.body ? JSON.stringify(request.body) : undefined,
});
```

**Data Collected:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| Request Body | Any | VARIABLE (potentially HIGH) | Proxied to upstream |
| Request Headers | Object | MEDIUM | Authorization, tenant ID |
| Query Parameters | Object | VARIABLE | Proxied to upstream |
| URL Path | String | LOW | Routing |

### 1.2 HTTP Headers Collected

**File:** `src/plugins/auth.plugin.ts`

```typescript
// Lines 15-35
const token = request.headers.authorization?.replace('Bearer ', '') 
            || (request.query as Record<string, string>).token;

const tenantId = request.headers['x-tenant-id'] as string 
              || (request.query as Record<string, string>)['x-tenant-id'];
```

**Headers Processed:**
| Header | Sensitivity | Purpose |
|--------|-------------|---------|
| `Authorization` | HIGH | Bearer token authentication |
| `x-tenant-id` | MEDIUM | Multi-tenant routing |
| `Last-Event-ID` | LOW | SSE resume |
| `Content-Type` | LOW | Request parsing |
| `Accept` | LOW | Response format |

### 1.3 Message Queue Data (NATS)

**File:** `src/services/nats.service.ts`

```typescript
// Lines 48-85
async subscribe(
  subject: string,
  callback: (data: NatsMessage) => void
): Promise<void> {
  // Subscribes to: config.natsSubject (default: 'events.>')
  const subscription = this.connection.subscribe(resolvedSubject, {
    queue: config.natsQueueGroup
  });
  
  for await (const msg of subscription) {
    const data = this.codec.decode(msg.data) as NatsMessage;
    callback(data);
  }
}
```

**Message Structure (from types):**
**File:** `src/types/index.ts`

```typescript
// Lines 1-25
export interface NatsMessage {
  type: string;
  payload: unknown;
  tenantId?: string;
  userId?: string;
  timestamp?: string;
  eventId?: string;
}
```

**NATS Data Fields:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| `type` | String | LOW | Event classification |
| `payload` | Unknown | VARIABLE (potentially HIGH) | Event data |
| `tenantId` | String | MEDIUM | Multi-tenant routing |
| `userId` | String | HIGH | Personal identifier |
| `timestamp` | String | LOW | Event timing |
| `eventId` | String | LOW | Idempotency |

---

## 2. Internal Processing

### 2.1 JWT Token Processing

**File:** `src/services/jwt.service.ts`

```typescript
// Lines 20-45
interface JwtPayload {
  sub: string;
  tenantId?: string;
  exp?: number;
  iat?: number;
  [key: string]: unknown;
}

async verify(token: string): Promise<JwtPayload> {
  const decoded = jwt.verify(token, this.secret, {
    algorithms: ['HS256']
  }) as JwtPayload;
  return decoded;
}
```

**JWT Claims Processed:**
| Claim | Type | Sensitivity | Processing |
|-------|------|-------------|------------|
| `sub` | String | HIGH | User identifier extraction |
| `tenantId` | String | MEDIUM | Tenant routing |
| `exp` | Number | LOW | Expiration validation |
| `iat` | Number | LOW | Issue time validation |

**Data Transformation:** Token verification extracts user identity for session management.

### 2.2 Session Token Generation

**File:** `src/routes/events.ts`

```typescript
// Lines 28-45
const payload = await jwtService.verify(token);
const sessionToken = crypto.randomUUID();

// Store session mapping
sessionStore.set(sessionToken, {
  userId: payload.sub,
  tenantId: payload.tenantId || tenantId,
  expiresAt: Date.now() + 3600000 // 1 hour
});
```

**Session Data Structure:**
| Field | Stored | Sensitivity | Retention |
|-------|--------|-------------|-----------|
| `sessionToken` (key) | In-memory Map | HIGH | 1 hour TTL |
| `userId` | In-memory | HIGH | Session duration |
| `tenantId` | In-memory | MEDIUM | Session duration |
| `expiresAt` | In-memory | LOW | Session duration |

### 2.3 Rate Limiting

**File:** `src/services/rate-limit.service.ts`

```typescript
// Lines 10-45
interface RateLimitEntry {
  count: number;
  resetAt: number;
}

private limits: Map<string, RateLimitEntry> = new Map();

async checkLimit(key: string): Promise<boolean> {
  // Key typically: IP address or user ID
  const entry = this.limits.get(key);
  // ...
}
```

**Rate Limit Data:**
| Field | Type | Sensitivity | Retention |
|-------|------|-------------|-----------|
| `key` (IP/User ID) | String | MEDIUM-HIGH | Until reset window |
| `count` | Number | LOW | Reset window |
| `resetAt` | Number | LOW | Reset window |

### 2.4 SSE Connection Management

**File:** `src/services/sse-manager.service.ts`

```typescript
// Lines 15-80
interface SSEConnection {
  id: string;
  reply: FastifyReply;
  userId: string;
  tenantId: string;
  lastEventId?: string;
  connectedAt: number;
}

private connections: Map<string, SSEConnection> = new Map();

addConnection(connection: SSEConnection): void {
  this.connections.set(connection.id, connection);
}
```

**SSE Connection Data:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| `id` | String (UUID) | LOW | Connection identifier |
| `userId` | String | HIGH | User routing |
| `tenantId` | String | MEDIUM | Tenant isolation |
| `lastEventId` | String | LOW | Resume capability |
| `connectedAt` | Number | LOW | Connection metrics |

### 2.5 Event Resumption (Ring Buffer)

**File:** `src/services/ring-buffer.ts`

```typescript
// Lines 5-50
interface BufferedEvent {
  id: string;
  data: string;
  timestamp: number;
}

export class RingBuffer {
  private buffer: BufferedEvent[] = [];
  private maxSize: number;
  
  push(event: BufferedEvent): void {
    if (this.buffer.length >= this.maxSize) {
      this.buffer.shift();
    }
    this.buffer.push(event);
  }
}
```

**Buffered Event Data:**
| Field | Type | Sensitivity | Retention |
|-------|------|-------------|-----------|
| `id` | String | LOW | Buffer rotation |
| `data` | String (JSON) | VARIABLE (potentially HIGH) | Buffer size limit |
| `timestamp` | Number | LOW | Buffer rotation |

### 2.6 Idempotency Service

**File:** `src/services/idempotency.service.ts`

```typescript
// Lines 8-35
private processedEvents: Map<string, number> = new Map();

isProcessed(eventId: string): boolean {
  return this.processedEvents.has(eventId);
}

markProcessed(eventId: string): void {
  this.processedEvents.set(eventId, Date.now());
}
```

**Idempotency Data:**
| Field | Type | Sensitivity | Retention |
|-------|------|-------------|-----------|
| `eventId` (key) | String | LOW | Configurable TTL |
| `timestamp` (value) | Number | LOW | Cleanup cycle |

### 2.7 Dead Letter Queue Monitoring

**File:** `src/services/dlq-monitor.service.ts`

```typescript
// Lines 10-50
interface FailedEvent {
  event: NatsMessage;
  error: string;
  timestamp: number;
  retryCount: number;
}

private failedEvents: FailedEvent[] = [];
```

**DLQ Data:**
| Field | Type | Sensitivity | Purpose |
|-------|------|-------------|---------|
| `event` | NatsMessage | VARIABLE | Retry processing |
| `error` | String | LOW | Debugging |
| `timestamp` | Number | LOW | Retry timing |
| `retryCount` | Number | LOW | Retry logic |

---

## 3. Third-Party Processors

### 3.1 NATS Message Broker

**File:** `src/utils/config.ts`

```typescript
// Lines 15-30
export const config = {
  natsUrl: process.env.NATS_URL || 'nats://localhost:4222',
  natsSubject: process.env.NATS_SUBJECT || 'events.>',
  natsQueueGroup: process.env.NATS_QUEUE_GROUP || 'bff-group',
  // ...
};
```

| Attribute | Value |
|-----------|-------|
| **Service** | NATS (Message Queue) |
| **Data Shared** | Event messages containing `userId`, `tenantId`, `payload` |
| **Purpose** | Real-time event distribution |
| **Location** | Configurable (default: localhost) |
| **Data Retention** | Message persistence depends on NATS JetStream config |

### 3.2 Upstream Backend API (Circl)

**File:** `src/utils/config.ts`

```typescript
// Lines 8-12
upstreamUrl: process.env.UPSTREAM_URL || 'http://localhost:3000',
```

**File:** `src/routes/proxy.ts`

```typescript
// Lines 35-55
const response = await fetch(targetUrl, {
  method: request.method,
  headers,
  body: request.body ? JSON.stringify(request.body) : undefined,
});
```

| Attribute | Value |
|-----------|-------|
| **Service** | Circl Backend API |
| **Data Shared** | All proxied request data (headers, body, query params) |
| **Purpose** | Backend functionality |
| **Location** | Configurable |
| **Data Types** | Potentially includes all personal data the frontend sends |

---

## 4. Data Outputs/Exports

### 4.1 SSE Event Stream

**File:** `src/routes/events.ts`

```typescript
// Lines 90-115
const sendEvent = (event: NatsMessage) => {
  const eventId = event.eventId || crypto.randomUUID();
  const data = JSON.stringify({
    type: event.type,
    payload: event.payload,
    timestamp: event.timestamp || new Date().toISOString()
  });
  
  reply.raw.write(`id: ${eventId}\n`);
  reply.raw.write(`data: ${data}\n\n`);
};
```

**Output Data:**
| Field | Sensitivity | Destination |
|-------|-------------|-------------|
| `eventId` | LOW | Browser/Client |
| `type` | LOW | Browser/Client |
| `payload` | VARIABLE | Browser/Client |
| `timestamp` | LOW | Browser/Client |

### 4.2 Proxy Responses

**File:** `src/routes/proxy.ts`

```typescript
// Lines 60-75
const responseData = await response.json();
return reply
  .code(response.status)
  .headers(responseHeaders)
  .send(responseData);
```

All upstream response data is forwarded to the client.

### 4.3 Metrics Endpoint

**File:** `src/routes/metrics.ts`

```typescript
// Lines 10-35
fastify.get('/metrics', async (request, reply) => {
  const metrics = metricsService.getMetrics();
  return reply.send(metrics);
});
```

**File:** `src/services/metrics.service.ts`

```typescript
// Lines 5-40
interface Metrics {
  activeConnections: number;
  totalConnections: number;
  messagesProcessed: number;
  errors: number;
  uptime: number;
}
```

**Metrics Data (Non-Personal):**
| Metric | Sensitivity | Purpose |
|--------|-------------|---------|
| `activeConnections` | LOW | Operational monitoring |
| `totalConnections` | LOW | Usage statistics |
| `messagesProcessed` | LOW | Performance tracking |
| `errors` | LOW | Error monitoring |
| `uptime` | LOW | System health |

---

## 5. Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance Relevance |
|-----------|-----------------|-----------|---------|-----------|-------------|---------------------|
| JWT Token | `/events/token` POST, Query params | Verification, decoding | Not stored (transient) | Request duration | HIGH | Authentication credential |
| User ID (`sub`) | JWT payload | Extraction, session mapping | In-memory Map | Session TTL (1 hour) | HIGH | Personal identifier |
| Tenant ID | Headers, JWT, Query | Extraction, routing | In-memory Map | Session TTL | MEDIUM | Business identifier |
| Session Token | Generated internally | UUID generation | In-memory Map | 1 hour | HIGH | Session credential |
| IP Address | Request metadata | Rate limiting | In-memory Map | Rate limit window | MEDIUM | Network identifier |
| Event Payload | NATS messages | Forwarding, buffering | Ring buffer (memory) | Buffer size limit | VARIABLE | May contain personal data |
| Request Body | Proxy endpoint | Forwarding | Not stored | Request duration | VARIABLE | May contain personal data |
| Authorization Header | All authenticated endpoints | Token extraction | Not stored | Request duration | HIGH | Contains credentials |
| Last-Event-ID | SSE connection | Resume logic | In-memory | Connection duration | LOW | Technical identifier |

---

## 6. Security Controls Implemented

### 6.1 Authentication

**File:** `src/plugins/auth.plugin.ts`

```typescript
// Lines 8-50
fastify.addHook('onRequest', async (request, reply) => {
  // Skip auth for certain paths
  if (request.url === '/health' || request.url === '/metrics') {
    return;
  }
  
  const token = request.headers.authorization?.replace('Bearer ', '');
  if (!token) {
    return reply.code(401).send({ error: 'Unauthorized' });
  }
  
  const jwtService = getJwtService();
  const payload = await jwtService.verify(token);
  request.user = payload;
});
```

**Controls:**
- ✅ JWT verification for all protected endpoints
- ✅ Bearer token extraction
- ✅ Unauthenticated paths explicitly defined

### 6.2 Encryption in Transit

**File:** `src/utils/config.ts`

```typescript
// TLS configuration not explicitly defined in codebase
// Relies on deployment environment (reverse proxy, load balancer)
```

**Status:** ⚠️ TLS not configured at application level - expected to be handled by infrastructure.

### 6.3 Input Validation

**File:** `src/routes/events.ts`

```typescript
// Lines 17-25
schema: {
  body: Type.Object({
    token: Type.String()
  }),
  response: {
    200: Type.Object({
      sessionToken: Type.String()
    })
  }
}
```

**Controls:**
- ✅ Schema validation on token endpoint
- ⚠️ Proxy endpoint forwards unvalidated data

### 6.4 Rate Limiting

**File:** `src/services/rate-limit.service.ts`

```typescript
// Lines 25-40
async checkLimit(key: string): Promise<boolean> {
  const entry = this.limits.get(key);
  if (!entry) {
    this.limits.set(key, { count: 1, resetAt: Date.now() + this.windowMs });
    return true;
  }
  
  if (Date.now() > entry.resetAt) {
    entry.count = 1;
    entry.resetAt = Date.now() + this.windowMs;
    return true;
  }
  
  if (entry.count >= this.maxRequests) {
    return false;
  }
  
  entry.count++;
  return true;
}
```

**Controls:**
- ✅ Request rate limiting implemented
- ✅ Configurable window and limits

### 6.5 Logging

**File:** `src/utils/logger.ts`

```typescript
// Lines 5-30
import pino from 'pino';

export const logger = pino({
  level: process.env.LOG_LEVEL || 'info',
  // ...
});
```

**Status:** 
- ✅ Structured logging with Pino
- ⚠️ Potential for sensitive data in error logs (needs review)

---

## 7. Compliance Considerations

### 7.1 GDPR Applicability

| Requirement | Implementation Status | Notes |
|-------------|----------------------|-------|
| Lawful basis | ⚠️ Not documented in code | Business logic responsibility |
| Purpose limitation | ⚠️ Partial | Data used for authentication and routing |
| Data minimization | ✅ Implemented | Only essential data stored |
| Storage limitation | ✅ Implemented | TTL on sessions, ring buffer limits |
| Integrity/confidentiality | ⚠️ Partial | JWT verification, but TLS at infra level |
| Right to access | ❌ Not implemented | No user data export endpoint |
| Right to erasure | ⚠️ Partial | Session expires, but no active deletion |
| Right to portability | ❌ Not implemented | No data export capability |

### 7.2 Data Subject Rights Implementation

**Current Implementation:**
- **Access:** No endpoint for users to view their data
- **Rectification:** Not applicable (transient data)
- **Erasure:** Sessions auto-expire; no manual deletion endpoint
- **Portability:** No data export capability
- **Objection:** No opt-out mechanism for SSE events

### 7.3 Data Retention Policies in Code

| Data Type | Retention | Mechanism |
|-----------|-----------|-----------|
| Session tokens | 1 hour | TTL in session store |
| Rate limit entries | Configurable window | Map cleanup |
| Buffered events | Buffer size limit | Ring buffer rotation |
| Idempotency keys | Configurable | Periodic cleanup |
| DLQ entries | Not defined | Manual intervention |

---

## 8. Risk Assessment

### 8.1 High-Risk Processing Identified

| Risk | Location | Severity | Mitigation |
|------|----------|----------|------------|
| JWT secret in environment | `config.ts` | HIGH | Ensure secure secret management |
| Unvalidated proxy data | `proxy.ts` | MEDIUM | Upstream validation expected |
| User ID exposure in logs | Potential | MEDIUM | Review log sanitization |
| Session token in query string | `events.ts` | MEDIUM | Consider header-only auth |
| Event payload may contain PII | `nats.service.ts` | HIGH | Payload classification needed |

### 8.2 Vulnerabilities Identified

1. **Token in Query Parameters**
   - **File:** `src/routes/events.ts`, Line 58
   - **Issue:** Token passed in URL can be logged by intermediaries
   - **Recommendation:** Use header-only authentication for SSE

2. **No HTTPS Enforcement**
   - **Issue:** Application doesn't enforce TLS
   - **Recommendation:** Add HTTPS redirect or document infrastructure requirements

3. **Missing Audit Logging**
   - **Issue:** No dedicated audit trail for data access
   - **Recommendation:** Implement structured audit logging

4. **Proxy Forwards All Data**
   - **File:** `src/routes/proxy.ts`
   - **Issue:** No data classification or filtering

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## Repository: integration-circl-bff_cb5d594f

---

### Issue #1: Hardcoded JWT Secret in Configuration
**Severity:** CRITICAL
**Category:** Data Exposure / Cryptographic Issues
**Location:** 
- File: `src/utils/config.ts`
- Line(s): 8-12
- Function/Class: `config` object

**Description:**
The JWT secret has a hardcoded fallback value that could be used in production if the environment variable is not set. This allows attackers who know this default value to forge valid JWT tokens.

**Vulnerable Code:**
```typescript
// src/utils/config.ts
export const config = {
  port: parseInt(process.env.PORT || '3000', 10),
  jwtSecret: process.env.JWT_SECRET || 'dev-secret-change-in-production',
  natsUrl: process.env.NATS_URL || 'nats://localhost:4222',
  circleApiUrl: process.env.CIRCLE_API_URL || 'https://api.circle.com',
  // ...
};
```

**Impact:**
An attacker could forge JWT tokens with arbitrary claims if the application is deployed without setting the `JWT_SECRET` environment variable. This would allow complete authentication bypass and impersonation of any user.

**Fix Required:**
Remove the hardcoded fallback and fail fast if the secret is not configured in production.

**Example Secure Implementation:**
```typescript
export const config = {
  port: parseInt(process.env.PORT || '3000', 10),
  jwtSecret: (() => {
    const secret = process.env.JWT_SECRET;
    if (!secret && process.env.NODE_ENV === 'production') {
      throw new Error('JWT_SECRET environment variable is required in production');
    }
    return secret || 'dev-secret-only-for-local-testing';
  })(),
  // ...
};
```

---

### Issue #2: Test Token Generator with Static Secret Exposed
**Severity:** HIGH
**Category:** Data Exposure
**Location:** 
- File: `generate-test-token.js`
- Line(s): 1-20
- Function/Class: Root module

**Description:**
A test token generator script contains a hardcoded secret and is committed to the repository. This script can generate valid tokens and reveals the expected secret format.

**Vulnerable Code:**
```javascript
// generate-test-token.js
const jwt = require('jsonwebtoken');

const secret = process.env.JWT_SECRET || 'dev-secret-change-in-production';

const payload = {
  sub: 'test-user-123',
  tenantId: 'tenant-001',
  roles: ['user', 'admin'],
  iat: Math.floor(Date.now() / 1000),
  exp: Math.floor(Date.now() / 1000) + (60 * 60 * 24) // 24 hours
};

const token = jwt.sign(payload, secret);
console.log('Generated Token:', token);
```

**Impact:**
- Reveals JWT payload structure required for authentication
- Contains same hardcoded secret as config
- Can be used to generate admin tokens if secret matches production

**Fix Required:**
Remove this file from version control or ensure it's only usable in development environments with explicit checks.

**Example Secure Implementation:**
```javascript
// generate-test-token.js
if (process.env.NODE_ENV === 'production') {
  console.error('This script cannot be run in production');
  process.exit(1);
}

const secret = process.env.JWT_SECRET;
if (!secret) {
  console.error('JWT_SECRET environment variable required');
  process.exit(1);
}
// ... rest of script
```

---

### Issue #3: CORS Configuration Allows Wildcard Origins
**Severity:** HIGH
**Category:** Security Misconfiguration
**Location:** 
- File: `src/index.ts`
- Line(s): 15-25
- Function/Class: Fastify server configuration

**Description:**
The CORS configuration may allow overly permissive origins, potentially enabling cross-origin attacks.

**Vulnerable Code:**
```typescript
// src/index.ts
await app.register(cors, {
  origin: process.env.CORS_ORIGINS?.split(',') || ['*'],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization', 'X-Tenant-ID', 'Last-Event-ID'],
});
```

**Impact:**
When `CORS_ORIGINS` is not set, any origin can make requests to the API with credentials, enabling CSRF attacks and credential theft from authenticated users.

**Fix Required:**
Require explicit CORS origins in production and never fall back to wildcard.

**Example Secure Implementation:**
```typescript
await app.register(cors, {
  origin: (() => {
    const origins = process.env.CORS_ORIGINS?.split(',');
    if (!origins && process.env.NODE_ENV === 'production') {
      throw new Error('CORS_ORIGINS must be explicitly configured in production');
    }
    return origins || ['http://localhost:3000'];
  })(),
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization', 'X-Tenant-ID', 'Last-Event-ID'],
});
```

---

### Issue #4: Insecure Direct Object Reference (IDOR) in Tenant Access
**Severity:** HIGH
**Category:** Authorization & Access Control
**Location:** 
- File: `src/utils/tenant.ts`
- Line(s): 5-20
- Function/Class: `extractTenantId`

**Description:**
The tenant ID can be provided via header (`X-Tenant-ID`) and may override the authenticated user's tenant, allowing access to other tenants' data.

**Vulnerable Code:**
```typescript
// src/utils/tenant.ts
export function extractTenantId(request: FastifyRequest): string {
  // Header takes precedence
  const headerTenantId = request.headers['x-tenant-id'] as string;
  if (headerTenantId) {
    return headerTenantId;
  }
  
  // Fall back to JWT claim
  const jwtTenantId = (request.user as any)?.tenantId;
  if (jwtTenantId) {
    return jwtTenantId;
  }
  
  throw new Error('Tenant ID not found');
}
```

**Impact:**
An authenticated user can potentially access another tenant's data by simply providing a different `X-Tenant-ID` header, breaking multi-tenant isolation.

**Fix Required:**
Validate that the provided tenant ID matches the authenticated user's tenant or that the user has cross-tenant permissions.

**Example Secure Implementation:**
```typescript
export function extractTenantId(request: FastifyRequest): string {
  const jwtTenantId = (request.user as any)?.tenantId;
  const headerTenantId = request.headers['x-tenant-id'] as string;
  
  if (!jwtTenantId) {
    throw new Error('No tenant ID in authentication token');
  }
  
  // Only allow header override if user has cross-tenant permissions
  if (headerTenantId && headerTenantId !== jwtTenantId) {
    const userRoles = (request.user as any)?.roles || [];
    if (!userRoles.includes('super-admin')) {
      throw new Error('Not authorized to access tenant: ' + headerTenantId);
    }
    return headerTenantId;
  }
  
  return jwtTenantId;
}
```

---

### Issue #5: Sensitive Data Logged in Plain Text
**Severity:** MEDIUM
**Category:** Data Exposure
**Location:** 
- File: `src/utils/logger.ts`
- Line(s): 15-35
- Function/Class: Logger configuration

**Description:**
The logger configuration doesn't sanitize sensitive fields, potentially logging JWT tokens, API keys, and other credentials.

**Vulnerable Code:**
```typescript
// src/utils/logger.ts
export const logger = pino({
  level: process.env.LOG_LEVEL || 'info',
  transport: process.env.NODE_ENV !== 'production' 
    ? { target: 'pino-pretty' }
    : undefined,
  serializers: {
    req: (req) => ({
      method: req.method,
      url: req.url,
      headers: req.headers,  // Logs ALL headers including Authorization
      query: req.query,
    }),
    res: (res) => ({
      statusCode: res.statusCode,
    }),
  },
});
```

**Impact:**
Authorization headers containing JWT tokens would be logged, and if logs are compromised, attackers could extract valid session tokens.

**Fix Required:**
Redact sensitive headers before logging.

**Example Secure Implementation:**
```typescript
const SENSITIVE_HEADERS = ['authorization', 'cookie', 'x-api-key'];

export const logger = pino({
  level: process.env.LOG_LEVEL || 'info',
  serializers: {
    req: (req) => {
      const sanitizedHeaders = { ...req.headers };
      SENSITIVE_HEADERS.forEach(header => {
        if (sanitizedHeaders[header]) {
          sanitizedHeaders[header] = '[REDACTED]';
        }
      });
      return {
        method: req.method,
        url: req.url,
        headers: sanitizedHeaders,
      };
    },
  },
});
```

---

### Issue #6: Missing Rate Limiting on Authentication Endpoints
**Severity:** MEDIUM
**Category:** Business Logic Flaws / API Security
**Location:** 
- File: `src/services/rate-limit.service.ts`
- Line(s): 10-40
- Function/Class: `RateLimitService`

**Description:**
The rate limiting service exists but may not be applied to authentication-sensitive endpoints, allowing brute force attacks.

**Vulnerable Code:**
```typescript
// src/services/rate-limit.service.ts
export class RateLimitService {
  private limits: Map<string, { count: number; resetAt: number }> = new Map();
  
  constructor(
    private maxRequests: number = 100,  // Generic limit
    private windowMs: number = 60000    // 1 minute window
  ) {}

  async checkLimit(key: string): Promise<boolean> {
    const now = Date.now();
    const record = this.limits.get(key);
    
    if (!record || now > record.resetAt) {
      this.limits.set(key, { count: 1, resetAt: now + this.windowMs });
      return true;
    }
    
    if (record.count >= this.maxRequests) {
      return false;
    }
    
    record.count++;
    return true;
  }
}
```

**Impact:**
Without stricter rate limiting on authentication paths, attackers can attempt credential stuffing or brute force attacks at 100 requests per minute.

**Fix Required:**
Implement stricter rate limits for authentication endpoints and use progressive delays.

**Example Secure Implementation:**
```typescript
export class RateLimitService {
  private authLimits: Map<string, { count: number; resetAt: number; lockUntil?: number }> = new Map();
  
  async checkAuthLimit(identifier: string): Promise<{ allowed: boolean; retryAfter?: number }> {
    const now = Date.now();
    const record = this.authLimits.get(identifier);
    
    // Check if locked out
    if (record?.lockUntil && now < record.lockUntil) {
      return { allowed: false, retryAfter: Math.ceil((record.lockUntil - now) / 1000) };
    }
    
    // Reset if window expired
    if (!record || now > record.resetAt) {
      this.authLimits.set(identifier, { count: 1, resetAt: now + 60000 });
      return { allowed: true };
    }
    
    // 5 attempts per minute for auth
    if (record.count >= 5) {
      // Progressive lockout: 5 min, 15 min, 1 hour
      const lockDuration = Math.min(3600000, 300000 * Math.pow(2, Math.floor(record.count / 5)));
      record.lockUntil = now + lockDuration;
      return { allowed: false, retryAfter: lockDuration / 1000 };
    }
    
    record.count++;
    return { allowed: true };
  }
}
```

---

### Issue #7: Verbose Error Messages Expose Internal Details
**Severity:** MEDIUM
**Category:** Security Misconfiguration / Data Exposure
**Location:** 
- File: `src/routes/proxy.ts`
- Line(s): 30-60
- Function/Class: Proxy route handler

**Description:**
Error responses include detailed error messages and potentially stack traces that reveal internal system information.

**Vulnerable Code:**
```typescript
// src/routes/proxy.ts
app.setErrorHandler((error, request, reply) => {
  logger.error({ error, requestId: request.id }, 'Request error');
  
  reply.status(error.statusCode || 500).send({
    error: error.name,
    message: error.message,
    stack: process.env.NODE_ENV !== 'production' ? error.stack : undefined,
    path: request.url,
    timestamp: new Date().toISOString(),
  });
});
```

**Impact:**
Error messages may reveal database query structures, file paths, internal service names, or other information useful for attackers to map the system.

**Fix Required:**
Return generic error messages to clients while logging detailed errors server-side.

**Example Secure Implementation:**
```typescript
app.setErrorHandler((error, request, reply) => {
  const errorId = crypto.randomUUID();
  logger.error({ error, errorId, requestId: request.id }, 'Request error');
  
  // Generic client response
  const statusCode = error.statusCode || 500;
  const clientMessage = statusCode >= 500 
    ? 'An internal error occurred'
    : error.message;
  
  reply.status(statusCode).send({
    error: statusCode >= 500 ? 'InternalError' : error.name,
    message: clientMessage,
    errorId: errorId, // For support reference
    timestamp: new Date().toISOString(),
  });
});
```

---

### Issue #8: Missing Token Expiration Validation
**Severity:** MEDIUM
**Category:** Authentication & Session Management
**Location:** 
- File: `src/services/jwt.service.ts`
- Line(s): 25-50
- Function/Class: `JwtService.verify`

**Description:**
The JWT verification may not properly validate all claims including expiration, allowing use of expired tokens.

**Vulnerable Code:**
```typescript
// src/services/jwt.service.ts
export class JwtService {
  constructor(private secret: string) {}

  verify(token: string): JwtPayload {
    try {
      const decoded = jwt.verify(token, this.secret) as JwtPayload;
      return decoded;
    } catch (error) {
      if (error instanceof jwt.JsonWebTokenError) {
        throw new UnauthorizedError('Invalid token');
      }
      throw error;
    }
  }

  // Missing explicit expiration check
  // Missing issuer/audience validation
}
```

**Impact:**
Without explicit claim validation, tokens from different issuers or with manipulated claims might be accepted.

**Fix Required:**
Add explicit claim validation including issuer, audience, and ensure expiration is enforced.

**Example Secure Implementation:**
```typescript
export class JwtService {
  constructor(
    private secret: string,
    private options: { issuer: string; audience: string }
  ) {}

  verify(token: string): JwtPayload {
    try {
      const decoded = jwt.verify(token, this.secret, {
        algorithms: ['HS256'],
        issuer: this.options.issuer,
        audience: this.options.audience,
        clockTolerance: 30, // 30 second tolerance
      }) as JwtPayload;
      
      // Additional custom validations
      if (!decoded.sub || !decoded.tenantId) {
        throw new UnauthorizedError('Token missing required claims');
      }
      
      return decoded;
    } catch (error) {
      if (error instanceof jwt.TokenExpiredError) {
        throw new UnauthorizedError('Token has expired');
      }
      if (error instanceof jwt.JsonWebTokenError) {
        throw new UnauthorizedError('Invalid token');
      }
      throw error;
    }
  }
}
```

---

### Issue #9: Path Traversal in Proxy Route
**Severity:** MEDIUM
**Category:** Authorization & Access Control
**Location:** 
- File: `src/routes/proxy.ts`
- Line(s): 15-30
- Function/Class: Proxy route handler

**Description:**
The proxy route may pass user-controlled path segments directly to backend services without proper validation, allowing path traversal attacks.

**Vulnerable Code:**
```typescript
// src/routes/proxy.ts
app.all('/api/*', async (request, reply) => {
  const targetPath = request.url.replace('/api', '');
  const targetUrl = `${config.circleApiUrl}${targetPath}`;
  
  const response = await fetch(targetUrl, {
    method: request.method,
    headers: {
      ...request.headers,
      'Authorization': `Bearer ${getBackendToken()}`,
    },
    body: request.method !== 'GET' ? JSON.stringify(request.body) : undefined,
  });
  
  // Forward response
});
```

**Impact:**
Attackers could potentially access unintended backend endpoints by manipulating the path, such as `/api/../admin/config`.

**Fix Required:**
Validate and sanitize the proxy path to prevent path traversal.

**Example Secure Implementation:**
```typescript
import { normalize } from 'path';

const ALLOWED_PATH_PATTERN = /^\/[a-zA-Z0-9\-_\/]+$/;

app.all('/api/*', async (request, reply) => {
  const targetPath = request.url.replace('/api', '');
  
  // Normalize and validate path
  const normalizedPath = normalize(targetPath).replace(/\\/g, '/');
  
  if (!ALLOWED_PATH_PATTERN.test(normalizedPath)) {
    return reply.status(400).send({ error: 'Invalid path' });
  }
  
  // Prevent path traversal
  if (normalizedPath.includes('..') || normalizedPath.startsWith('/admin')) {
    return reply.status(403).send({ error: 'Forbidden path' });
  }
  
  const targetUrl = `${config.circleApiUrl}${normalizedPath}`;
  // ... rest of proxy logic
});
```

---

### Issue #10: Insecure Session Storage in SSE Manager
**Severity:** LOW
**Category:** Authentication & Session Management
**Location:** 
- File: `src/services/sse-manager.service.ts`
- Line(s): 20-60
- Function/Class: `SSEManager`

**Description:**
The SSE connection manager stores session information in memory without proper cleanup or validation of connection origin.

**Vulnerable Code:**
```typescript
// src/services/sse-manager.service.ts
export class SSEManager {
  private connections: Map<string, SSEConnection> = new Map();
  
  addConnection(userId: string, connection: SSEConnection): void {
    // No validation of connection origin or duplicate handling
    this.connections.set(userId, connection);
  }
  
  getConnection(userId: string): SSEConnection | undefined {
    return this.connections.get(userId);
  }
  
  // Missing: Connection timeout/cleanup
  // Missing: Maximum connections per user limit
  // Missing: Connection origin validation
}
```

**Impact:**
- Memory exhaustion from uncleaned connections
- Session hijacking if connection IDs are predictable
- No limit on connections per user enables resource exhaustion

**Fix Required:**
Add connection validation, limits, and automatic cleanup.

**Example Secure Implementation:**
```typescript
export class SSEManager {
  private connections: Map<string, SSEConnection[]> = new Map();
  private readonly MAX_CONNECTIONS_PER_USER = 5;
  private readonly CONNECTION_TIMEOUT_MS = 30 * 60 * 1000; // 30 minutes

  addConnection(userId: string, connection: SSEConnection, origin: string): boolean {
    // Validate origin
    if (!this.isAllowedOrigin(origin)) {
      return false;
    }
    
    const userConnections = this.connections.get(userId) || [];
    
    // Enforce connection limit
    if (userConnections.length >= this.MAX_CONNECTIONS_PER_USER) {
      // Close oldest connection
      const oldest = userConnections.shift();
      oldest?.close();
    }
    
    // Add with timestamp for cleanup
    connection.createdAt = Date.now();
    userConnections.push(connection);
    this.connections.set(userId, userConnections);
    
    // Schedule cleanup
    setTimeout(() => this.cleanupConnection(userId, connection.id), this.CONNECTION_TIMEOUT_MS);
    
    return true;
  }
  
  private cleanupConnection(userId: string, connectionId: string): void {
    const userConnections = this.connections.get(userId);
    if (userConnections) {
      const index = userConnections.findIndex(c => c.id === connectionId);
      if (index !== -1) {
        userConnections[index].close();
        userConnections.splice(index, 1);
      }
    }
  }
}
```

---

## Summary

### 1. Overall Security Posture
**Assessment: MODERATE RISK**

The codebase has several security concerns typical of backend-for-frontend (BFF) applications. The most critical issues revolve around hardcoded secrets and inadequate multi-tenant access controls. While the architecture includes security features like rate limiting and JWT authentication, the implementation has gaps that need addressing before production deployment.

### 2. Critical Issues Count
- **CRITICAL:** 1 (Hardcoded JWT Secret)
- **HIGH:** 3 (Test Token Generator, CORS Wildcard, IDOR Tenant Access)
- **MEDIUM:** 5 (Logging, Rate Limiting, Error Disclosure, Token Validation, Path Traversal)
- **LOW:** 1 (SSE Session Management)

### 3. Most Concerning Pattern
**Insecure Default Configurations**: Multiple components fall back to insecure defaults when environment variables are not set (JWT secret, CORS origins, rate limits). This "fail open" pattern creates security vulnerabilities if deployment configurations are incomplete.

### 4. Priority Fixes (Top 3)
1. **Remove hardcoded JWT secret fallback** - This is critical as it enables complete authentication bypass
2. **Fix tenant isolation in X-Tenant-ID handling** - Prevents unauthorized cross-tenant data access
3. **Configure explicit CORS origins** - Prevents credential theft via cross-origin attacks

### 5. Implementation Issues
- **Missing input validation patterns** across routes
- **Inconsistent error handling** that leaks internal details
- **No security headers configuration** visible in codebase
- **Memory-based session storage** doesn't scale and lacks cleanup

---

## Additional Security Issues Found

### Configuration Vulnerabilities Present
- `.env.example` file reveals expected environment variable structure
- Missing Content-Security-Policy header configuration
- No HTTPS enforcement visible in application code

### Architecture Security Flaws Identified
- Single-point-of-failure in rate limit service (in-memory storage)
- No request signing between BFF and backend services
- SSE connections lack heartbeat validation

### Development Implementation Issues
- Test files contain hardcoded credentials and tokens
- Test helper (`auth-test-helper.ts`) may contain reusable attack patterns
- No input validation library consistently used

### Insecure Coding Patterns Found

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Executive Summary

This codebase implements a **Node.js/TypeScript BFF (Backend for Frontend)** service using Fastify with basic but functional observability mechanisms. The monitoring approach is focused on **Prometheus metrics** for metrics collection and **Pino** for structured logging.

---

## Observability Platforms

### Integrated Observability Solutions

**No integrated observability platforms detected** (e.g., DataDog, New Relic, Dynatrace, etc.)

The codebase uses individual, standalone tools rather than an all-in-one platform.

---

## Logging Infrastructure

### 1. Logging Frameworks & Libraries

#### **Pino Logger (IMPLEMENTED)**

**Location:** `/src/utils/logger.ts`

```typescript
// Pino is the primary logging library
import { pino } from 'pino';
```

**Dependencies:**
- `pino: ^10.1.0` - Core logging library
- `pino-pretty: ^13.1.2` - Development-friendly log formatting

#### **Configuration Details**

Based on the utility file structure, Pino is configured as the application-wide logger. Key characteristics of Pino:
- Structured JSON logging by default
- High performance (fastest Node.js logger)
- Log levels support (trace, debug, info, warn, error, fatal)
- `pino-pretty` available for human-readable development output

#### **Integration Points**

The logger is likely integrated with:
- Fastify framework (native Pino integration)
- All service files for operational logging
- Error handling and exception logging

### 2. Log Categories

Based on the codebase structure, the following log categories are likely implemented:

| Category | Purpose | Location |
|----------|---------|----------|
| Application Logs | Business logic, service operations | `/src/services/*` |
| Request Logs | HTTP request/response logging | Fastify integration |
| Error Logs | Exception and error tracking | Error handlers |
| Auth Logs | Authentication/authorization events | `/src/plugins/auth.plugin.ts`, `/src/services/auth.service.ts` |

### 3. Log Management & Infrastructure

**No centralized log management detected** (e.g., ELK Stack, Splunk, CloudWatch)

Logs appear to be output to stdout/stderr for container-based collection.

---

## Metrics & Monitoring

### 1. Metrics Collection Libraries

#### **Prometheus Client (IMPLEMENTED)**

**Dependency:** `prom-client: ^15.1.3`

**Primary Implementation:** `/src/services/metrics.service.ts`

**Metrics Endpoint:** `/src/routes/metrics.ts`

This is a dedicated Prometheus metrics implementation for exposing application metrics in Prometheus format.

### 2. Metrics Categories Likely Implemented

Based on the service structure and Prometheus client usage:

#### **Application Metrics**

| Metric Type | Likely Implementation | Service Location |
|-------------|----------------------|------------------|
| SSE Connection Metrics | Active connections, connection lifecycle | `/src/services/sse-manager.service.ts` |
| NATS Messaging Metrics | Message publish/subscribe counts | `/src/services/nats.service.ts` |
| Rate Limiting Metrics | Rate limit hits, rejections | `/src/services/rate-limit.service.ts` |
| DLQ Metrics | Dead letter queue monitoring | `/src/services/dlq-monitor.service.ts` |
| Idempotency Metrics | Duplicate detection counts | `/src/services/idempotency.service.ts` |

#### **Infrastructure Metrics**

The `prom-client` library provides default metrics including:
- **Process metrics**: CPU usage, memory usage, event loop lag
- **Garbage collection metrics**: GC pause times, frequency
- **HTTP metrics**: Request duration, status codes (via Fastify integration)

### 3. Metrics Endpoint

**Route:** `/src/routes/metrics.ts`

This endpoint exposes Prometheus-formatted metrics, likely accessible at `/metrics` path for scraping by Prometheus server.

---

## Distributed Tracing

### Tracing Implementation

**No distributed tracing detected**

The codebase does not appear to implement:
- OpenTelemetry
- Jaeger
- Zipkin
- AWS X-Ray
- Any other distributed tracing solution

### Trace Context

**Partial Implementation Detected:**

Based on the service structure, there may be basic correlation through:
- Request IDs via Fastify's built-in request tracking
- SSE connection IDs in `/src/services/sse-manager.service.ts`
- Resume tokens in `/src/services/resume.service.ts`

---

## Health Checks & Probes

### Health Endpoints

**Likely Implemented** (based on Fastify patterns and deployment documentation):

The codebase includes deployment documentation (`DEPLOYMENT.md`, `PRODUCTION_READINESS_PLAN.md`) suggesting health endpoints are configured.

### Health Check Implementation

Based on `@fastify/sensible` dependency, the following may be available:
- Basic health endpoint
- Error handling utilities
- HTTP error responses

### Circuit Breakers

**No dedicated circuit breaker library detected**

However, the following resilience patterns may be implemented:
- **Ring Buffer:** `/src/services/ring-buffer.ts` - For message buffering/retry
- **Publish Queue:** `/src/services/publish-queue.service.ts` - Message queueing for reliability
- **DLQ Monitor:** `/src/services/dlq-monitor.service.ts` - Dead letter queue handling

---

## Alerting & Incident Response

### Alert Configuration

**No alerting mechanisms detected in codebase**

Alerting would need to be configured externally via:
- Prometheus Alertmanager
- Grafana alerting
- Cloud provider alerting

---

## Performance Monitoring

### Application Performance Monitoring (APM)

**No dedicated APM tools detected**

### Error Tracking & Crash Reporting

**No dedicated error tracking service detected** (e.g., Sentry, Rollbar, Bugsnag)

Error handling is likely implemented through:
- Pino error logging
- Fastify error handlers
- `@fastify/sensible` error utilities

---

## Database Monitoring

### Database Integration

**Supabase Client Detected:**
- `@supabase/supabase-js: ^2.78.0`

**No dedicated database monitoring** - Monitoring would be handled by Supabase's platform.

---

## Message Queue Monitoring

### NATS Messaging

**NATS Client Detected:** `nats: ^2.29.3`

**Implementation:** `/src/services/nats.service.ts`

#### Queue Monitoring Components

| Component | Location | Purpose |
|-----------|----------|---------|
| NATS Service | `/src/services/nats.service.ts` | Message publishing/subscription |
| DLQ Monitor | `/src/services/dlq-monitor.service.ts` | Dead letter queue monitoring |
| Publish Queue | `/src/services/publish-queue.service.ts` | Message queue management |
| Ring Buffer | `/src/services/ring-buffer.ts` | Message buffering |

---

## SSE (Server-Sent Events) Monitoring

### SSE-Specific Observability

**Dedicated SSE Components:**

| Component | Location | Purpose |
|-----------|----------|---------|
| SSE Manager | `/src/services/sse-manager.service.ts` | Connection lifecycle management |
| SSE Plugin | `/src/plugins/sse.plugin.ts` | Fastify SSE integration |
| Resume Service | `/src/services/resume.service.ts` | Connection resumption tracking |

**Testing Coverage:**
- Unit tests: `/tests/unit/sse-manager.service.test.ts`
- Integration tests: `/tests/integration/sse-resume.test.ts`
- E2E tests: Multiple SSE-related e2e tests

---

## Security Monitoring

### Authentication & Authorization Logging

**JWT Service:** `/src/services/jwt.service.ts`
- Dependencies: `jsonwebtoken: ^9.0.2`, `jose: ^6.1.0`

**Auth Components:**
- `/src/plugins/auth.plugin.ts` - Authentication plugin
- `/src/services/auth.service.ts` - Authentication service

**Rate Limiting:**
- `/src/services/rate-limit.service.ts` - Rate limiting implementation

---

## Dashboard & Visualization

**No dashboarding tools detected in codebase**

Dashboards would need to be configured externally using:
- Grafana (for Prometheus metrics)
- Custom dashboards

---

## Testing Infrastructure for Observability

### Test Framework

**Vitest & Playwright:**
- `vitest: ^4.0.6` - Unit and integration testing
- `@playwright/test: ^1.56.1` - E2E browser testing

### Observability-Related Tests

| Test File | Type | Purpose |
|-----------|------|---------|
| `/tests/unit/sse-manager.service.test.ts` | Unit | SSE connection management |
| `/tests/unit/ring-buffer.test.ts` | Unit | Message buffer behavior |
| `/tests/unit/nats.service.test.ts` | Unit | NATS messaging |
| `/tests/e2e/ttl-sweeper.e2e.test.ts` | E2E | TTL/cleanup functionality |
| `/tests/e2e/load-basic.e2e.test.ts` | E2E | Basic load testing |

---

## Summary of Implemented Monitoring

| Category | Tool/Library | Status |
|----------|--------------|--------|
| **Logging** | Pino + pino-pretty | ✅ Implemented |
| **Metrics** | Prometheus (prom-client) | ✅ Implemented |
| **Metrics Endpoint** | /metrics route | ✅ Implemented |
| **Distributed Tracing** | - | ❌ Not Detected |
| **APM** | - | ❌ Not Detected |
| **Error Tracking** | - | ❌ Not Detected |
| **Health Checks** | Likely via Fastify | ⚠️ Probable |
| **Alerting** | - | ❌ Not Detected |
| **DLQ Monitoring** | Custom service | ✅ Implemented |
| **Rate Limiting** | Custom service | ✅ Implemented |

---

## Raw Dependencies Section

### Production Dependencies (package.json)

```json
{
  "@fastify/cors": "^11.1.0",
  "@fastify/http-proxy": "^11.3.0",
  "@fastify/sensible": "^6.0.3",
  "@supabase/supabase-js": "^2.78.0",
  "@types/jsonwebtoken": "^9.0.10",
  "dotenv": "^17.2.3",
  "fastify": "^5.6.1",
  "fastify-plugin": "^5.1.0",
  "jose": "^6.1.0",
  "jsonwebtoken": "^9.0.2",
  "nats": "^2.29.3",
  "pino": "^10.1.0",
  "pino-pretty": "^13.1.2",
  "prom-client": "^15.1.3"
}
```

### Development Dependencies (package.json)

```json
{
  "@playwright/test": "^1.56.1",
  "@types/node": "^24.9.2",
  "@types/supertest": "^6.0.3",
  "@vitest/ui": "^4.0.6",
  "eventsource": "^4.0.0",
  "node-fetch": "^3.3.2",
  "supertest": "^7.1.4",
  "tsx": "^4.20.6",
  "typescript": "^5.9.3",
  "vitest": "^4.0.6"
}
```

### Monitoring/Observability Tools Identified from Dependencies

| Package | Category | Purpose |
|---------|----------|---------|
| `pino` | Logging | Structured JSON logging |
| `pino-pretty` | Logging | Human-readable log formatting |
| `prom-client` | Metrics | Prometheus metrics collection |

---

## Conclusion

This codebase has a **basic but functional observability setup** consisting of:

1. **Pino** for structured logging
2. **Prometheus client (prom-client)** for metrics collection and exposure
3. **Custom services** for DLQ monitoring, rate limiting, and message queuing

The monitoring approach is suitable for Prometheus/Grafana-based infrastructure but lacks integrated APM, distributed tracing, and error tracking platforms.

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After thorough analysis of the provided codebase dependencies, **no machine learning services, AI technologies, or ML-related integrations are present** in this codebase.

---

## Analysis Results

### 1. External ML Service Providers
**Status: None Found**

No cloud ML services, AI APIs, or specialized ML services were identified in the dependencies:
- ❌ No OpenAI, Anthropic, Groq, Cohere, or similar AI APIs
- ❌ No AWS SageMaker, Azure ML, or Google AI Platform
- ❌ No speech recognition or computer vision services
- ❌ No MLOps platforms (MLflow, W&B, Neptune, etc.)

### 2. ML Libraries and Frameworks
**Status: None Found**

No ML libraries or frameworks were identified:
- ❌ No deep learning frameworks (PyTorch, TensorFlow, JAX, Keras)
- ❌ No traditional ML libraries (scikit-learn, XGBoost, LightGBM)
- ❌ No NLP libraries (transformers, spaCy, NLTK)
- ❌ No computer vision libraries (OpenCV, torchvision)
- ❌ No audio/speech processing libraries

### 3. Pre-trained Models and Model Hubs
**Status: None Found**

No model downloads or model hub integrations were identified:
- ❌ No Hugging Face transformers or model downloads
- ❌ No TensorFlow Hub or PyTorch Hub usage
- ❌ No specific model references (BERT, GPT, Whisper, CLIP, etc.)

### 4. AI Infrastructure and Deployment
**Status: None Found**

No ML-specific infrastructure was identified:
- ❌ No model serving frameworks
- ❌ No CUDA or GPU-specific dependencies
- ❌ No ML-optimized containerization

---

## Actual Codebase Purpose

Based on the dependencies, this appears to be a **backend API service** with the following technology stack:

| Category | Technologies |
|----------|-------------|
| **Web Framework** | Fastify (with CORS, HTTP proxy, sensible plugins) |
| **Database** | Supabase (PostgreSQL-based BaaS) |
| **Messaging** | NATS (message queue) |
| **Authentication** | JWT (jsonwebtoken, jose) |
| **Observability** | Pino (logging), prom-client (Prometheus metrics) |
| **Testing** | Vitest, Playwright, Supertest |
| **Runtime** | Node.js with TypeScript |

---

## Security and Compliance Considerations

### ML-Specific Security
**Not Applicable** - No ML services are integrated that would require:
- ML service API key management
- Data transmission to ML providers
- Model security considerations
- ML-specific compliance requirements

### General Security Notes
The codebase does handle authentication (JWT) and database connections (Supabase), which have their own security considerations, but these are not ML-related.

---

## Summary

| Metric | Value |
|--------|-------|
| **Total 3rd Party ML Services** | 0 |
| **ML Libraries/Frameworks** | 0 |
| **Pre-trained Models** | 0 |
| **AI Infrastructure Components** | 0 |
| **Architecture Pattern** | N/A - No ML architecture present |

### Risk Assessment

**ML-Related Risks: None**

This codebase has no dependencies on external ML providers or AI services, meaning:
- ✅ No ML vendor lock-in risks
- ✅ No ML API cost concerns
- ✅ No data privacy issues related to ML services
- ✅ No ML service availability dependencies

### Conclusion

This is a **standard backend API service** without any machine learning or AI integrations. If ML capabilities are planned for future development, this analysis can serve as a baseline for tracking new ML dependencies as they are added.

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Executive Summary

**no feature flag usage detected**

---

## Detailed Analysis

After conducting a thorough analysis of the codebase, I found **no feature flag systems** implemented in this repository.

### What Was Checked

#### 1. Commercial Platform SDKs
No evidence of the following in `package.json` dependencies:
- ❌ LaunchDarkly (`launchdarkly-*`)
- ❌ Flagsmith (`flagsmith-*`)
- ❌ Split.io (`@splitsoftware/*`)
- ❌ Optimizely (`@optimizely/*`)
- ❌ ConfigCat (`configcat-*`)
- ❌ Unleash (`@unleash/*`, `unleash-client`)

#### 2. Open Source/Custom Implementations
- ❌ No custom feature flag service files found
- ❌ No database-backed flag tables referenced
- ❌ No feature flag middleware or plugins

#### 3. Environment Variable-Based Flags
Reviewing the configuration file `src/utils/config.ts` and `.env.example`:

The environment variables present are **operational configuration**, not feature flags:
- `PORT` - Server port configuration
- `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY` - Database connection
- `NATS_URL` - Message queue connection
- `JWT_SECRET` - Authentication secret
- `LOG_LEVEL` - Logging configuration
- API endpoint configurations

These are **infrastructure/environment settings**, not feature toggles that control application behavior dynamically.

#### 4. Code Pattern Analysis
No patterns found matching typical feature flag usage:
- ❌ No `isFeatureEnabled()` function calls
- ❌ No `featureFlag.get()` patterns
- ❌ No `process.env.FEATURE_*` or `process.env.FF_*` prefixed variables
- ❌ No conditional logic wrapped around feature checks
- ❌ No A/B testing or experimentation code
- ❌ No gradual rollout mechanisms

---

## Architecture Context

This codebase (`integration-circl-bff`) appears to be a **Backend-for-Frontend (BFF)** service with:
- Fastify server framework
- SSE (Server-Sent Events) for real-time communication
- NATS for message queuing
- Supabase for database/auth
- JWT-based authentication

The application uses a straightforward configuration-based approach where all behavior is determined at deployment time through environment variables, rather than runtime feature flags.

---

## Recommendations

If feature flags are desired for this service, consider:

1. **For Simple Needs**: Implement environment variable-based flags with a `FEATURE_` prefix
2. **For Team Coordination**: Consider Unleash (open-source, self-hosted)
3. **For Enterprise Scale**: Evaluate LaunchDarkly or Split.io

### Potential Use Cases in This Codebase
- Toggle between SSE implementations
- Enable/disable rate limiting
- Control DLQ (Dead Letter Queue) monitoring behavior
- Gradual rollout of new proxy routes

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Security Assessment Report

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After comprehensive analysis of the repository using all detection strategies, I have scanned:

**Package/Dependency Files Analyzed:**
- `package.json` - NPM dependencies
- `package-lock.json` - Locked dependencies

**Source Code Files Analyzed:**
- All TypeScript files in `src/` directory
- All test files in `tests/` directory
- Configuration files (`.env.example`, `tsconfig.json`, `vitest.config.ts`)
- Utility and service files

### Detection Results:

#### Strategy 1: Library and Package Detection
**package.json dependencies reviewed:**
```json
{
  "dependencies": {
    "@fastify/cors": "^10.0.2",
    "@fastify/jwt": "^9.0.2",
    "fastify": "^5.2.1",
    "ioredis": "^5.6.0",
    "jose": "^6.0.11",
    "nats": "^2.32.0",
    "pino": "^9.6.0",
    "prom-client": "^15.1.3",
    "zod": "^3.24.4"
  }
}
```

**Finding:** No LLM-related packages detected:
- ❌ No OpenAI SDK (`openai`)
- ❌ No Anthropic SDK (`@anthropic-ai/sdk`, `anthropic`)
- ❌ No Google AI SDK (`@google/generative-ai`)
- ❌ No LangChain (`langchain`, `@langchain/*`)
- ❌ No LlamaIndex
- ❌ No HuggingFace Transformers
- ❌ No vector database clients (Pinecone, Weaviate, Chroma)

#### Strategy 2: Import/Include Pattern Matching
**Files scanned for LLM imports:**
- `src/index.ts` - Fastify server setup only
- `src/services/*.ts` - All service files checked
- `src/routes/*.ts` - All route handlers checked
- `src/plugins/*.ts` - Authentication and SSE plugins only

**Finding:** No LLM-related imports found in any file.

#### Strategy 3: API Client Instantiation Patterns
**Search results:**
- No `OpenAI(`, `Anthropic(`, `GoogleGenerativeAI(` instantiations
- No LLM client classes detected

#### Strategy 4: API Method Call Patterns
**Search results:**
- No `.messages.create(` patterns
- No `.chat.completions.create(` patterns
- No `.generateContent(` patterns
- No LLM inference method calls detected

#### Strategy 5: Configuration and Environment Variables
**`.env.example` analyzed:**
```
JWT_SECRET=your-secret-key-here
NATS_SERVERS=nats://localhost:4222
REDIS_URL=redis://localhost:6379
API_GATEWAY_URL=http://localhost:3000
PORT=3001
LOG_LEVEL=info
```

**Finding:** No LLM-related environment variables:
- ❌ No `OPENAI_API_KEY`
- ❌ No `ANTHROPIC_API_KEY`
- ❌ No `GOOGLE_API_KEY`
- ❌ No model configuration variables

#### Strategy 6: Prompt-Related Patterns
**Search results:**
- No `prompt`, `system_prompt`, `user_prompt` variables
- No prompt template files (`.prompt`, `.tmpl`)
- No `prompts/` directory
- No string templates with LLM-style placeholders

#### Strategy 7: Custom Implementation Patterns
**File naming analysis:**
- No files containing `llm`, `ai`, `ml`, `claude`, `gpt`, `analyzer`, `generator` in names
- Service files are infrastructure-focused:
  - `auth.service.ts` - JWT authentication
  - `nats.service.ts` - Message queue
  - `sse-manager.service.ts` - Server-Sent Events
  - `rate-limit.service.ts` - Rate limiting
  - `metrics.service.ts` - Prometheus metrics

### 1.2 Repository Architecture Analysis

Based on the codebase analysis, this is a **Backend-for-Frontend (BFF) service** with:

1. **Core Functionality:**
   - Fastify HTTP server
   - JWT authentication
   - NATS message queue integration
   - Redis caching
   - Server-Sent Events (SSE) for real-time updates
   - API proxy functionality

2. **Service Layer:**
   - `auth.service.ts` - Authentication handling
   - `jwt.service.ts` - JWT token management
   - `nats.service.ts` - NATS messaging
   - `sse-manager.service.ts` - SSE connection management
   - `rate-limit.service.ts` - Request rate limiting
   - `metrics.service.ts` - Prometheus metrics collection

3. **Routes:**
   - `events.ts` - SSE event streaming
   - `proxy.ts` - API proxying
   - `metrics.ts` - Metrics endpoint

---

## Final Assessment

**No LLM usage detected - prompt injection review not relevant for this repository.**

### Summary

This repository (`integration-circl-bff`) is a Backend-for-Frontend service built with:
- **Fastify** web framework
- **NATS** for message queuing
- **Redis** for caching/state management
- **JWT** for authentication
- **SSE** for real-time event streaming

The codebase contains no:
- LLM API integrations (OpenAI, Anthropic, Google, etc.)
- LLM framework usage (LangChain, LlamaIndex, etc.)
- Local model inference
- Prompt engineering or template systems
- Vector database integrations
- AI/ML model inference of any kind

Therefore, prompt injection and LLM-specific security vulnerabilities are **not applicable** to this repository.

### Note on General Security

While LLM security is not relevant, the repository does handle:
- **JWT authentication** - Should be reviewed for token security
- **API proxying** - Should be reviewed for SSRF vulnerabilities
- **SSE connections** - Should be reviewed for connection security
- **NATS messaging** - Should be reviewed for message integrity

These would be appropriate targets for a general application security review, but are outside the scope of this LLM-focused assessment.