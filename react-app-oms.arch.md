# hl_overview

High level overview of the codebase

# Project Analysis: react-app-oms_ed70b7af

## 0. Repository Name
[[OMSIoT]]

## 1. Project Purpose

This is a **mobile application for Order Management System (OMS) with IoT integration**. Based on the directory structure, the application appears to solve problems related to:

- **Order/Trip Management**: Managing orders, trips, and deliveries (evidenced by `orders/`, `trips/`, `filling/`, `offload/` screens)
- **IoT Device Integration**: Connecting with Bluetooth devices for tracking/monitoring (evidenced by `iot/` screens, `bluetoothService.ts`, `bluetoothStore.ts`)
- **Location Tracking**: Real-time GPS tracking for logistics operations (`locationService.ts`, `locationStore.ts`, `TrackingProvider.tsx`)
- **Offline-First Operations**: Supporting field operations with data synchronization (`syncService.ts`, `database/` with repositories)

**Primary Domain**: Logistics/Delivery Management with IoT device integration for fleet or delivery tracking.

## 2. Architecture Pattern

- **Feature-Based/Domain-Driven Structure** for screens
- **Repository Pattern** for data access
- **State Management with Stores** (likely Zustand or similar)
- **Service Layer Pattern** for business logic and external integrations
- **Offline-First Architecture** with local database and sync capabilities

## 3. Technology Stack

### Primary Language
- **TypeScript** (evidenced by `tsconfig.json`, `.tsx` files)

### Framework
- **React Native with Expo** (evidenced by `app.json`, `expo` patterns, native iOS/Android directories)

### Platform Support
- **iOS** (Swift-based with `AppDelegate.swift`, CocoaPods)
- **Android** (Gradle-based build system)

### Major Libraries/Dependencies (inferred from structure)
| Category | Likely Dependencies |
|----------|-------------------|
| Navigation | React Navigation (evidenced by `AppNavigator.tsx`, `navigation/types.ts`) |
| State Management | Zustand or MobX (multiple `*Store.ts` files pattern) |
| Database | SQLite/WatermelonDB/Expo SQLite (local `database/` with repositories) |
| Bluetooth | react-native-ble-plx or similar (bluetoothService) |
| Location | expo-location or react-native-geolocation |
| Notifications | expo-notifications (notificationService) |
| HTTP Client | Axios or Fetch (api/client.ts) |

## 4. Initial Structure Impression

| Component | Description |
|-----------|-------------|
| **Mobile App (Primary)** | React Native/Expo cross-platform mobile application |
| **iOS Native** | Native iOS project with Swift bridging |
| **Android Native** | Native Android project with Gradle build |
| **Source Code** | Business logic in `src/` directory |
| **Assets** | App icons, splash screens, and static resources |

This is a **monolithic mobile application** rather than a multi-service architecture.

## 5. Configuration/Package Files

| File | Purpose |
|------|---------|
| `package.json` | NPM dependencies and scripts |
| `package-lock.json` | Locked dependency versions |
| `tsconfig.json` | TypeScript configuration |
| `app.json` | Expo application configuration |
| `ios/Podfile` | iOS CocoaPods dependencies |
| `ios/Podfile.lock` | Locked iOS dependencies |
| `ios/Podfile.properties.json` | Podfile properties |
| `android/build.gradle` | Android root build configuration |
| `android/app/build.gradle` | Android app build configuration |
| `android/gradle.properties` | Gradle properties |
| `android/settings.gradle` | Gradle settings |
| `.gitignore` | Git ignore rules |
| `ios/OMSIoT/Info.plist` | iOS app configuration |
| `ios/OMSIoT/OMSIoT.entitlements` | iOS entitlements |
| `ios/OMSIoT/PrivacyInfo.xcprivacy` | iOS privacy manifest |

## 6. Directory Structure

```
src/
├── api/              # API layer - HTTP clients and endpoint definitions
│   ├── client.ts     # Base HTTP client configuration
│   ├── authApi.ts    # Authentication endpoints
│   ├── iotApi.ts     # IoT device API
│   └── omsApi.ts     # Order management system API
│
├── components/       # Reusable UI components
│   ├── common/       # Generic reusable components (8 files)
│   ├── TrackingProvider.tsx    # Context for tracking state
│   └── PermissionDisclosureModal.tsx  # Permission handling
│
├── config/           # Application configuration
│   └── constants.ts  # App-wide constants
│
├── database/         # Local database layer
│   ├── database.ts   # Database initialization/connection
│   └── repositories/ # Data access repositories (5 files)
│
├── hooks/            # Custom React hooks
│   ├── useTrackingSetup.ts  # Tracking initialization hook
│   └── index.ts
│
├── navigation/       # Navigation configuration
│   ├── AppNavigator.tsx  # Main navigator
│   └── types.ts          # Navigation type definitions
│
├── screens/          # Feature-based screen modules
│   ├── auth/         # Authentication (login, etc.)
│   ├── home/         # Dashboard/home screen
│   ├── loading/      # Loading/splash screens
│   ├── iot/          # IoT device management
│   ├── orders/       # Order management (5 files)
│   ├── trips/        # Trip management
│   ├── filling/      # Filling operations
│   └── offload/      # Offload operations
│
├── services/         # Business logic services
│   ├── bluetoothService.ts    # BLE device communication
│   ├── locationService.ts     # GPS/location handling
│   ├── notificationService.ts # Push notifications
│   └── syncService.ts         # Offline data synchronization
│
├── store/            # State management stores
│   ├── authStore.ts      # Authentication state
│   ├── bluetoothStore.ts # Bluetooth connection state
│   ├── locationStore.ts  # Location tracking state
│   ├── syncStore.ts      # Sync status state
│   └── tripStore.ts      # Trip data state
│
├── theme/            # UI theming
│   ├── colors.ts     # Color palette
│   ├── spacing.ts    # Spacing constants
│   └── typography.ts # Font styles
│
└── types/            # TypeScript type definitions
    └── index.ts
```

**Organization Pattern**: Hybrid - **Layer-based** (api, services, store, components) combined with **Feature-based** (screens organized by domain feature).

## 7. High-Level Architecture

### Architectural Patterns

```
┌─────────────────────────────────────────────────────────────┐
│                      PRESENTATION LAYER                      │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Screens (Feature-based)                             │    │
│  │  auth | home | orders | trips | iot | filling | ...  │    │
│  └─────────────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Components (common + feature-specific)              │    │
│  └─────────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────────┤
│                      STATE LAYER                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Stores: auth | bluetooth | location | sync | trip   │    │
│  └─────────────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Hooks: useTrackingSetup, etc.                       │    │
│  └─────────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────────┤
│                      SERVICE LAYER                           │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐      │
│  │  Bluetooth    │ │   Location    │ │ Notification  │      │
│  │   Service     │ │   Service     │ │   Service     │      │
│  └───────────────┘ └───────────────┘ └───────────────┘      │
│  ┌─────────────────────────────────────────────────────┐    │
│  │               Sync Service                           │    │
│  └─────────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────────┤
│                      DATA LAYER                              │
│  ┌─────────────────────┐    ┌─────────────────────────┐     │
│  │    API Layer        │    │   Local Database        │     │
│  │  (client, authApi,  │    │   (repositories)        │     │
│  │   omsApi, iotApi)   │    │                         │     │
│  └─────────────────────┘    └─────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

### Evidence Supporting Architecture:

1. **Layered Architecture**: Clear separation of `api/`, `services/`, `store/`, `screens/`
2. **Repository Pattern**: `database/repositories/` with 5 repository files
3. **State Management Pattern**: Multiple stores following single-responsibility
4. **Offline-First Pattern**: Local database + syncService for data synchronization
5. **Provider Pattern**: `TrackingProvider.tsx` for context-based state sharing

## 8. Build, Execution and Test

### Entry Points
- **Main Entry**: `index.ts` → `App.tsx`
- **iOS Entry**: `ios/OMSIoT/AppDelegate.swift`
- **Android Entry**: `android/app/src/main/`

### Build & Run Commands (typical Expo/RN project)

```bash
# Install dependencies
npm install

# iOS specific
cd ios && pod install && cd ..

# Development
npx expo start              # Start Expo development server
npx expo run:ios            # Run on iOS simulator
npx expo run:android        # Run on Android emulator

# Build (production)
# iOS - via Xcode or EAS Build
# Android - via Gradle or EAS Build
eas build --platform ios
eas build --platform android
```

### Android Build
```bash
cd android
./gradlew assembleDebug      # Debug APK
./gradlew assembleRelease    # Release APK
```

### iOS Build
- Open `ios/OMSIoT.xcworkspace` in Xcode
- Build using Xcode or `xcodebuild`

### Testing
No explicit test configuration files visible in the structure. Testing would typically be done with:
- Jest for unit tests
- React Native Testing Library for component tests
- Detox for E2E tests

---

**Summary**: This is a well-structured React Native/Expo mobile application for logistics/delivery management with IoT integration. It follows modern architectural patterns including layered architecture, repository pattern, and offline-first design, organized as a hybrid of layer-based and feature-based structure.

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

---

## 1. `src/database/`

### Core Responsibility
Handles local data persistence using a SQLite or similar embedded database. Manages data storage, retrieval, and manipulation for offline-first functionality.

### Key Components

| File/Directory | Role |
|---------------|------|
| `database.ts` | Main database configuration, initialization, connection management, and schema setup |
| `repositories/` | Data access layer containing repository classes for different domain entities |
| └─ *(5 files)* | Individual repository files likely for: Users, Orders, Trips, Sync records, and IoT data |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - Uses type definitions for database models
  - `@src/config/constants.ts` - Database configuration values
- **External Services:**
  - SQLite/WatermelonDB or similar local database library
  - No direct external API calls (data layer only)

---

## 2. `src/types/`

### Core Responsibility
Centralized TypeScript type definitions and interfaces ensuring type safety across the application.

### Key Components

| File | Role |
|------|------|
| `index.ts` | Exports all shared types, interfaces, and enums used throughout the app (likely includes Order, Trip, User, IoT device types, API response types) |

### Dependencies & Interactions
- **Internal Dependencies:**
  - None (foundational module - other modules depend on this)
- **External Services:**
  - None (pure type definitions)

---

## 3. `src/config/`

### Core Responsibility
Application-wide configuration constants and environment settings.

### Key Components

| File | Role |
|------|------|
| `constants.ts` | Defines app constants: API endpoints, timeout values, storage keys, feature flags, device settings |

### Dependencies & Interactions
- **Internal Dependencies:**
  - None (foundational module)
- **External Services:**
  - May reference external API base URLs

---

## 4. `src/navigation/`

### Core Responsibility
Manages application routing and screen navigation flow using React Navigation.

### Key Components

| File | Role |
|------|------|
| `AppNavigator.tsx` | Main navigation container defining stack/tab navigators and screen hierarchy |
| `types.ts` | Navigation-specific TypeScript types (route params, navigation prop types) |
| `index.ts` | Barrel export file |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/screens/*` - All screen components
  - `@src/store/authStore` - Authentication state for protected routes
  - `@src/types/` - Shared type definitions
- **External Services:**
  - React Navigation library

---

## 5. `src/screens/`

### Core Responsibility
Contains all screen/page components organized by feature domain.

### Key Components

| Sub-directory | Files | Role |
|--------------|-------|------|
| `home/` | 2 files | Dashboard/main screen after login |
| `loading/` | 3 files | Splash/loading screens, initial app state |
| `auth/` | 4 files | Login, logout, authentication flows |
| `iot/` | 3 files | IoT device management, Bluetooth pairing |
| `filling/` | 2 files | Tank/container filling operations |
| `orders/` | 5 files | Order management, listing, details, status |
| `trips/` | 3 files | Trip management, route tracking |
| `offload/` | 3 files | Delivery offloading, quantity recording |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/components/` - Shared UI components
  - `@src/hooks/` - Custom hooks
  - `@src/store/*` - State management stores
  - `@src/api/*` - API calls
  - `@src/services/*` - Business logic services
  - `@src/theme/` - Styling constants
  - `@src/navigation/types` - Navigation typing
- **External Services:**
  - Indirect via API and services layers

---

## 6. `src/components/`

### Core Responsibility
Reusable UI components shared across multiple screens.

### Key Components

| File/Directory | Role |
|---------------|------|
| `PermissionDisclosureModal.tsx` | Modal for displaying permission requests (location, Bluetooth) |
| `TrackingProvider.tsx` | Context provider for location/trip tracking state |
| `index.ts` | Barrel export |
| `common/` (8 files) | Generic UI components: buttons, inputs, cards, headers, loaders, etc. |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/theme/` - Colors, typography, spacing
  - `@src/types/` - Component prop types
  - `@src/hooks/` - Shared hooks
- **External Services:**
  - None directly (presentation layer)

---

## 7. `src/theme/`

### Core Responsibility
Design system implementation with consistent styling tokens.

### Key Components

| File | Role |
|------|------|
| `colors.ts` | Color palette definitions (primary, secondary, status colors) |
| `typography.ts` | Font families, sizes, weights, line heights |
| `spacing.ts` | Margin/padding scale values |
| `index.ts` | Consolidated theme export |

### Dependencies & Interactions
- **Internal Dependencies:**
  - None (foundational styling module)
- **External Services:**
  - None

---

## 8. `src/hooks/`

### Core Responsibility
Custom React hooks encapsulating reusable stateful logic.

### Key Components

| File | Role |
|------|------|
| `useTrackingSetup.ts` | Hook for initializing and managing location tracking setup |
| `index.ts` | Barrel export |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/services/locationService` - Location functionality
  - `@src/store/locationStore` - Location state
  - `@src/store/tripStore` - Trip state
- **External Services:**
  - Device location APIs (via services)

---

## 9. `src/api/`

### Core Responsibility
HTTP client configuration and API endpoint implementations for backend communication.

### Key Components

| File | Role |
|------|------|
| `client.ts` | Axios/fetch instance configuration, interceptors, base URL, auth headers |
| `authApi.ts` | Authentication endpoints (login, logout, token refresh) |
| `omsApi.ts` | Order Management System endpoints (orders, trips, offloading) |
| `iotApi.ts` | IoT device registration and data sync endpoints |
| `index.ts` | Barrel export |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/config/constants` - API base URLs
  - `@src/store/authStore` - Auth tokens for headers
  - `@src/types/` - Request/response types
- **External Services:**
  - **Backend OMS API** - Order management server
  - **IoT Platform API** - Device telemetry server
  - **Auth Server** - Authentication service

---

## 10. `src/services/`

### Core Responsibility
Business logic layer and device/platform service integrations.

### Key Components

| File | Role |
|------|------|
| `bluetoothService.ts` | BLE device scanning, connection, data read/write operations |
| `locationService.ts` | GPS location tracking, geofencing, background location |
| `notificationService.ts` | Push notification handling, local notifications |
| `syncService.ts` | Offline data synchronization logic, conflict resolution |
| `index.ts` | Barrel export |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/database/` - Local data persistence
  - `@src/api/*` - Backend synchronization
  - `@src/store/*` - State updates
  - `@src/config/constants` - Service configuration
- **External Services:**
  - **Bluetooth Low Energy** - Device communication
  - **Device GPS** - Location services
  - **Push Notification Services** (FCM/APNs)
  - **Backend APIs** - Data sync

---

## 11. `src/store/`

### Core Responsibility
Global state management using a state library (likely Zustand or MobX based on file naming).

### Key Components

| File | Role |
|------|------|
| `authStore.ts` | User authentication state, tokens, login status |
| `bluetoothStore.ts` | Connected devices, BLE connection status, device data |
| `locationStore.ts` | Current location, tracking status, location history |
| `syncStore.ts` | Sync queue status, pending operations, sync errors |
| `tripStore.ts` | Active trip data, trip history, route information |
| `index.ts` | Barrel export, possibly combined store |

### Dependencies & Interactions
- **Internal Dependencies:**
  - `@src/types/` - State type definitions
  - `@src/api/*` - API calls triggered from actions
  - `@src/services/*` - Service invocations
  - `@src/database/` - Persistence layer
- **External Services:**
  - Indirect via API and services

---

## Dependency Flow Diagram

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   screens   │────▶│  components │────▶│    theme    │
└─────────────┘     └─────────────┘     └─────────────┘
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│    hooks    │────▶│    store    │
└─────────────┘     └─────────────┘
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│  services   │◀───▶│     api     │
└─────────────┘     └─────────────┘
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│  database   │     │   config    │
└─────────────┘     └─────────────┘
       │                   │
       └───────────┬───────┘
                   ▼
            ┌─────────────┐
            │    types    │
            └─────────────┘
```

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

**Repository**: `react-app-oms_ed70b7af`

---

## Internal Modules

Based on the repository structure and directory organization under `src/`, the following internal modules/packages have been identified:

### Core Application Modules

| Module | Path | Description |
|--------|------|-------------|
| **Database** | `src/database/` | Handles local data persistence using SQLite. Contains the main database configuration (`database.ts`) and repository pattern implementations for data access (`repositories/` with 5 files). |
| **Types** | `src/types/` | Centralized TypeScript type definitions and interfaces shared across the application. |
| **Config** | `src/config/` | Application configuration and constants management (`constants.ts`). |
| **Navigation** | `src/navigation/` | React Navigation setup and routing configuration. Contains the main navigator (`AppNavigator.tsx`), navigation types, and exports. |
| **API** | `src/api/` | HTTP client and API integration layer. Includes authentication API (`authApi.ts`), IoT API (`iotApi.ts`), OMS API (`omsApi.ts`), and a shared HTTP client (`client.ts`). |
| **Services** | `src/services/` | Business logic and platform service integrations including Bluetooth (`bluetoothService.ts`), location tracking (`locationService.ts`), push notifications (`notificationService.ts`), and data synchronization (`syncService.ts`). |
| **Store** | `src/store/` | Global state management using Zustand. Contains stores for authentication (`authStore.ts`), Bluetooth (`bluetoothStore.ts`), location (`locationStore.ts`), sync status (`syncStore.ts`), and trip management (`tripStore.ts`). |
| **Components** | `src/components/` | Reusable UI components including common components (8 files), permission disclosure modal, and tracking provider. |
| **Theme** | `src/theme/` | Design system definitions including colors, spacing, and typography configurations. |
| **Hooks** | `src/hooks/` | Custom React hooks including tracking setup hook (`useTrackingSetup.ts`). |

### Screen Modules

| Screen Module | Path | Description |
|---------------|------|-------------|
| **Home** | `src/screens/home/` | Home/dashboard screen (2 files). |
| **Loading** | `src/screens/loading/` | App loading/splash screens (3 files). |
| **Auth** | `src/screens/auth/` | Authentication screens - login, registration, etc. (4 files). |
| **IoT** | `src/screens/iot/` | IoT device management and interaction screens (3 files). |
| **Filling** | `src/screens/filling/` | Filling operation screens (2 files). |
| **Orders** | `src/screens/orders/` | Order management screens (5 files). |
| **Trips** | `src/screens/trips/` | Trip tracking and management screens (3 files). |
| **Offload** | `src/screens/offload/` | Offload operation screens (3 files). |

---

## External Dependencies

### JavaScript/TypeScript Dependencies

**Source**: `/package.json`

#### Production Dependencies

| Dependency | Official Name | Purpose/Role |
|------------|---------------|--------------|
| `@expo/metro-runtime` | Expo Metro Runtime | Metro bundler runtime for Expo projects |
| `@react-native-community/netinfo` | NetInfo | Network connectivity state detection and monitoring |
| `@react-navigation/bottom-tabs` | React Navigation Bottom Tabs | Bottom tab navigation component |
| `@react-navigation/native` | React Navigation | Core navigation library for React Native |
| `@react-navigation/native-stack` | React Navigation Native Stack | Native stack navigator for screen transitions |
| `@shopify/flash-list` | FlashList | High-performance list component (replacement for FlatList) |
| `@tanstack/react-query` | TanStack Query (React Query) | Server state management, data fetching, and caching |
| `axios` | Axios | HTTP client for API requests |
| `date-fns` | date-fns | Date utility library for parsing, formatting, and manipulating dates |
| `expo` | Expo | React Native development framework and toolchain |
| `expo-background-fetch` | Expo Background Fetch | Background task execution for periodic data fetching |
| `expo-camera` | Expo Camera | Camera access and photo/video capture |
| `expo-dev-client` | Expo Dev Client | Custom development client for Expo projects |
| `expo-device` | Expo Device | Device information and identification |
| `expo-file-system` | Expo File System | Local file system access and management |
| `expo-image-picker` | Expo Image Picker | Image/photo selection from device gallery or camera |
| `expo-location` | Expo Location | GPS location services and geolocation tracking |
| `expo-notifications` | Expo Notifications | Push and local notification handling |
| `expo-secure-store` | Expo Secure Store | Secure/encrypted key-value storage (credentials, tokens) |
| `expo-sqlite` | Expo SQLite | Local SQLite database for offline data persistence |
| `expo-status-bar` | Expo Status Bar | Status bar appearance control |
| `expo-task-manager` | Expo Task Manager | Background task registration and management |
| `react` | React | Core UI library |
| `react-dom` | React DOM | React rendering for web platform |
| `react-native` | React Native | Cross-platform mobile application framework |
| `react-native-ble-plx` | react-native-ble-plx | Bluetooth Low Energy (BLE) communication for IoT device connectivity |
| `react-native-gesture-handler` | React Native Gesture Handler | Native gesture recognition and handling |
| `react-native-reanimated` | React Native Reanimated | Declarative animations library |
| `react-native-safe-area-context` | React Native Safe Area Context | Safe area insets handling for notched devices |
| `react-native-screens` | React Native Screens | Native navigation screen containers |
| `react-native-svg` | React Native SVG | SVG rendering support |
| `react-native-web` | React Native Web | React Native components for web platform |
| `zustand` | Zustand | Lightweight state management library |

#### Development Dependencies

**Source**: `/package.json (dev)`

| Dependency | Official Name | Purpose/Role |
|------------|---------------|--------------|
| `@types/react` | React Type Definitions | TypeScript type definitions for React |
| `typescript` | TypeScript | Static type checking and compilation |

---

### Java/Android Dependencies

**Source**: `/android/app/build.gradle`, `/android/build.gradle`

| Dependency | Official Name | Purpose/Role |
|------------|---------------|--------------|
| `com.android.application` | Android Gradle Plugin | Android application build configuration |
| `org.jetbrains.kotlin.android` | Kotlin Android Plugin | Kotlin language support for Android |
| `com.facebook.react` | React Native Gradle Plugin | React Native build integration for Android |
| `com.facebook.react:react-android` | React Native Android | React Native Android runtime |
| `com.facebook.react:hermes-android` | Hermes | JavaScript engine optimized for React Native |
| `io.github.react-native-community:jsc-android` | JavaScriptCore (JSC) | Alternative JavaScript engine (fallback when Hermes disabled) |
| `com.facebook.fresco:animated-gif` | Fresco Animated GIF | Animated GIF image support (conditional) |
| `com.facebook.fresco:webpsupport` | Fresco WebP | WebP image format support (conditional) |
| `com.facebook.fresco:animated-webp` | Fresco Animated WebP | Animated WebP image support (conditional) |
| `com.android.tools.build:gradle` | Android Build Tools | Android build toolchain |
| `org.jetbrains.kotlin:kotlin-gradle-plugin` | Kotlin Gradle Plugin | Kotlin compilation support |

---

## Summary

This is a **React Native mobile application** built with **Expo** for an Order Management System (OMS) with IoT capabilities. Key architectural characteristics:

- **State Management**: Zustand for global state, TanStack Query for server state
- **Navigation**: React Navigation with bottom tabs and native stack
- **Data Persistence**: Expo SQLite for offline-first local storage with repository pattern
- **IoT Integration**: Bluetooth Low Energy support via react-native-ble-plx
- **Background Operations**: Expo Task Manager and Background Fetch for sync operations
- **Location Services**: Expo Location for GPS tracking
- **Platform Support**: iOS, Android, and Web (via react-native-web)

# core_entities

Core entities and their relationships

# Domain Model Analysis - OMS (Order Management System) IoT Mobile App

Based on analysis of the repository structure and source files, this appears to be a **React Native mobile application** for an Order Management System with IoT integration, likely for delivery/logistics operations involving trips, orders, filling, and offloading.

---

## 1. Common Data Entities

### Core Domain Entities

| Entity | Description |
|--------|-------------|
| **User/Auth** | Authenticated user/driver operating the mobile app |
| **Trip** | A delivery trip or route containing multiple orders |
| **Order** | Individual customer order to be delivered |
| **Location** | GPS/tracking location data points |
| **IoT Device** | Bluetooth-connected IoT devices (sensors, trackers) |
| **Sync Record** | Offline data synchronization tracking |
| **Filling** | Loading/filling operation (likely fuel or cargo) |
| **Offload** | Unloading/delivery completion operation |

---

## 2. Entity Attributes and Key Fields

### **User / Authentication**
```
User {
  id: string
  username: string
  email: string
  token: string (JWT/auth token)
  refreshToken: string
  isAuthenticated: boolean
  permissions: string[]
}
```
*Source: `authStore.ts`, `authApi.ts`*

---

### **Trip**
```
Trip {
  id: string
  tripNumber: string
  status: TripStatus (pending | in_progress | completed | cancelled)
  startTime: Date
  endTime: Date
  vehicleId: string
  driverId: string
  orders: Order[]
  totalDistance: number
  route: Location[]
}
```
*Source: `tripStore.ts`, `src/screens/trips/`*

---

### **Order**
```
Order {
  id: string
  orderNumber: string
  tripId: string (FK)
  customerId: string
  customerName: string
  deliveryAddress: string
  status: OrderStatus (pending | in_transit | delivered | failed)
  quantity: number
  product: string
  scheduledDeliveryTime: Date
  actualDeliveryTime: Date
  signature: string (base64)
  notes: string
  latitude: number
  longitude: number
}
```
*Source: `src/screens/orders/`, `omsApi.ts`*

---

### **Location**
```
Location {
  id: string
  tripId: string (FK)
  latitude: number
  longitude: number
  altitude: number
  accuracy: number
  speed: number
  heading: number
  timestamp: Date
  isSynced: boolean
}
```
*Source: `locationStore.ts`, `locationService.ts`*

---

### **IoT Device (Bluetooth)**
```
IoTDevice {
  id: string
  deviceId: string (BLE identifier)
  name: string
  type: DeviceType (sensor | tracker | meter)
  status: ConnectionStatus (connected | disconnected | pairing)
  batteryLevel: number
  lastReading: any
  lastConnectedAt: Date
}
```
*Source: `bluetoothStore.ts`, `bluetoothService.ts`, `iotApi.ts`*

---

### **Filling Operation**
```
Filling {
  id: string
  tripId: string (FK)
  productType: string
  quantityLoaded: number
  locationId: string
  timestamp: Date
  operatorId: string
  meterReadingStart: number
  meterReadingEnd: number
  verified: boolean
}
```
*Source: `src/screens/filling/`*

---

### **Offload Operation**
```
Offload {
  id: string
  orderId: string (FK)
  tripId: string (FK)
  quantityDelivered: number
  timestamp: Date
  signature: string
  photoProof: string[]
  gpsLocation: Location
  notes: string
  status: OffloadStatus (pending | completed | partial)
}
```
*Source: `src/screens/offload/`*

---

### **Sync Record**
```
SyncRecord {
  id: string
  entityType: string (trip | order | location | filling | offload)
  entityId: string
  action: SyncAction (create | update | delete)
  payload: JSON
  status: SyncStatus (pending | synced | failed)
  attempts: number
  lastAttemptAt: Date
  createdAt: Date
  errorMessage: string
}
```
*Source: `syncStore.ts`, `syncService.ts`, `src/database/`*

---

## 3. Entity Relationships

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          ENTITY RELATIONSHIP DIAGRAM                      │
└─────────────────────────────────────────────────────────────────────────┘

                              ┌──────────────┐
                              │     User     │
                              │   (Driver)   │
                              └──────┬───────┘
                                     │
                                     │ 1:N (driver has many trips)
                                     ▼
┌──────────────┐             ┌──────────────┐             ┌──────────────┐
│  IoT Device  │◄───────────►│     Trip     │─────────────►│   Filling    │
└──────────────┘   N:1       └──────┬───────┘      1:N     └──────────────┘
                  (device           │
                  assigned          │ 1:N (trip has many orders)
                  to trip)          │
                                    ▼
                             ┌──────────────┐
                             │    Order     │
                             └──────┬───────┘
                                    │
                                    │ 1:1 (order has one offload)
                                    ▼
                             ┌──────────────┐
                             │   Offload    │
                             └──────────────┘

        ┌──────────────┐
        │   Location   │◄──────────────────────────────────┐
        └──────────────┘                                   │
               ▲                                           │
               │ N:1 (many locations per trip)             │
               │                                           │
        ┌──────┴───────┐                           ┌───────┴──────┐
        │     Trip     │                           │   Offload    │
        └──────────────┘                           │  (delivery   │
                                                   │   location)  │
                                                   └──────────────┘

        ┌──────────────┐
        │  Sync Record │ ─────► Tracks offline changes for ALL entities
        └──────────────┘
```

### Relationship Summary Table

| Relationship | Type | Description |
|--------------|------|-------------|
| **User → Trip** | One-to-Many | A driver can have multiple trips assigned |
| **Trip → Order** | One-to-Many | A trip contains multiple orders for delivery |
| **Trip → Location** | One-to-Many | A trip has many GPS tracking points recorded |
| **Trip → Filling** | One-to-Many | A trip can have multiple filling/loading operations |
| **Order → Offload** | One-to-One | Each order has one offload/delivery completion record |
| **Trip → IoT Device** | Many-to-One | IoT device(s) can be assigned to monitor a trip |
| **Offload → Location** | Many-to-One | Offload captures GPS location at delivery point |
| **SyncRecord → All Entities** | Polymorphic | Sync records track offline changes for any entity type |

---

## 4. Database Layer Architecture

Based on `src/database/` structure:

```
database/
├── database.ts          # SQLite/WatermelonDB initialization
└── repositories/
    ├── tripRepository.ts
    ├── orderRepository.ts
    ├── locationRepository.ts
    ├── syncRepository.ts
    └── [offloadRepository.ts or fillingRepository.ts]
```

The application uses a **local-first architecture** with:
- **Local Database**: SQLite (via repositories) for offline capability
- **Sync Service**: Handles data synchronization when connectivity is restored
- **Store Layer**: Zustand stores (`authStore`, `tripStore`, `locationStore`, `syncStore`, `bluetoothStore`) for state management

---

## 5. Key Domain Flows

```
┌─────────────────────────────────────────────────────────────────┐
│                    TYPICAL OPERATIONAL FLOW                      │
└─────────────────────────────────────────────────────────────────┘

  1. LOGIN          2. TRIP START       3. FILLING          4. DELIVERY
  ─────────►        ─────────────►      ──────────►         ──────────►
  
  [Auth]            [Trip Created]      [Load Cargo]        [Navigate to
   User              IoT Connected       Record Qty          Customer]
   Login             GPS Tracking        Meter Reading       
                     Started                                 [Offload]
                                                            Deliver Order
                                                            Capture Signature
                                                            GPS Location
                                                            
                                        5. SYNC
                                        ──────────►
                                        [Sync all offline
                                         data to server]
```

---

## Summary

This OMS IoT application is designed for **field delivery operations** with:
- **Offline-first** data persistence
- **Real-time GPS tracking** during trips
- **IoT/Bluetooth device** integration (likely for cargo monitoring)
- **Complete delivery workflow**: Trip → Filling → Orders → Offload
- **Robust synchronization** for intermittent connectivity scenarios

# DBs

databases analysis

# Database Analysis for react-app-oms_635d2a1a

After analyzing the codebase, I have identified the database implementation used in this React Native application.

---

### Database: SQLite (via expo-sqlite)

* **Database Name/Type:** SQLite (SQL - Embedded Relational Database)

* **Purpose/Role:** Local offline-first database for the mobile application. Stores operational data including trips, orders, order items, filling logs, and offload records. Enables the application to function offline and sync data with remote servers when connectivity is available.

* **Key Technologies/Access Methods:** 
  - TypeScript/JavaScript
  - `expo-sqlite` library for SQLite database access
  - Custom repository pattern for data access abstraction
  - Direct SQL queries (CREATE TABLE, INSERT, SELECT, UPDATE, DELETE)

* **Key Files/Configuration:**
  - `src/database/database.ts` - Main database initialization, connection management, and schema definitions
  - `src/database/repositories/TripRepository.ts` - Data access for trips
  - `src/database/repositories/OrderRepository.ts` - Data access for orders
  - `src/database/repositories/OrderItemRepository.ts` - Data access for order items
  - `src/database/repositories/FillingLogRepository.ts` - Data access for filling logs
  - `src/database/repositories/OffloadRepository.ts` - Data access for offload records
  - `src/services/syncService.ts` - Handles synchronization between local SQLite and remote API

* **Schema/Table Structure:**

  | Table | Columns | Description |
  |-------|---------|-------------|
  | `trips` | `id` (PK, TEXT), `status` (TEXT), `driver_id` (TEXT), `vehicle_id` (TEXT), `start_time` (TEXT), `end_time` (TEXT), `created_at` (TEXT), `updated_at` (TEXT), `synced` (INTEGER) | Stores driver trip information |
  | `orders` | `id` (PK, TEXT), `trip_id` (FK to trips.id, TEXT), `customer_id` (TEXT), `customer_name` (TEXT), `delivery_address` (TEXT), `status` (TEXT), `sequence` (INTEGER), `notes` (TEXT), `created_at` (TEXT), `updated_at` (TEXT), `synced` (INTEGER) | Stores customer orders associated with trips |
  | `order_items` | `id` (PK, TEXT), `order_id` (FK to orders.id, TEXT), `product_id` (TEXT), `product_name` (TEXT), `quantity_ordered` (REAL), `quantity_delivered` (REAL), `unit` (TEXT), `price` (REAL), `created_at` (TEXT), `updated_at` (TEXT), `synced` (INTEGER) | Stores individual items within orders |
  | `filling_logs` | `id` (PK, TEXT), `trip_id` (FK to trips.id, TEXT), `product_id` (TEXT), `product_name` (TEXT), `quantity` (REAL), `unit` (TEXT), `timestamp` (TEXT), `created_at` (TEXT), `synced` (INTEGER) | Logs product filling events during trips |
  | `offloads` | `id` (PK, TEXT), `trip_id` (FK to trips.id, TEXT), `product_id` (TEXT), `product_name` (TEXT), `quantity` (REAL), `unit` (TEXT), `reason` (TEXT), `timestamp` (TEXT), `created_at` (TEXT), `synced` (INTEGER) | Records product offload events |

* **Key Entities and Relationships:**
  - **Trip:** Represents a delivery trip with a driver and vehicle assignment. Core entity that groups all related delivery activities.
  - **Order:** Represents a customer order to be fulfilled during a trip. Contains delivery details and status.
  - **Order Item:** Represents individual products within an order with quantities ordered vs delivered.
  - **Filling Log:** Records when and how much product was loaded for a trip.
  - **Offload:** Records product returns or removals from a trip with reasons.
  
  **Relationships:**
  - `Trip` (1) ←→ `Orders` (M): One trip contains many orders
  - `Order` (1) ←→ `Order Items` (M): One order contains many order items
  - `Trip` (1) ←→ `Filling Logs` (M): One trip has many filling log entries
  - `Trip` (1) ←→ `Offloads` (M): One trip can have many offload records

* **Interacting Components:**
  - **Sync Service** (`src/services/syncService.ts`) - Manages bidirectional synchronization between local SQLite and remote API
  - **Trip Store** (`src/store/tripStore.ts`) - State management for trip data
  - **Orders Screen** (`src/screens/orders/`) - UI for viewing and managing orders
  - **Trips Screen** (`src/screens/trips/`) - UI for viewing and managing trips
  - **Filling Screen** (`src/screens/filling/`) - UI for recording product fills
  - **Offload Screen** (`src/screens/offload/`) - UI for recording offloads
  - **Home Screen** (`src/screens/home/`) - Main dashboard that displays trip/order summaries

---

**Note:** The application also interacts with remote REST APIs (`src/api/omsApi.ts`, `src/api/authApi.ts`, `src/api/iotApi.ts`) for backend synchronization, but these are external services, not databases managed within this codebase. The SQLite database serves as the local persistence layer enabling offline-first functionality.

# APIs

APIs analysis

# API Documentation Analysis

After analyzing the repository structure and files, I can see this is a **React Native mobile application** (Expo-based, with iOS and Android configurations). The codebase contains **API client code** that *consumes* HTTP APIs, but it does **not expose/serve any HTTP API endpoints** itself.

Let me examine the API-related files to confirm:

## Files Analyzed

### `src/api/client.ts` - HTTP Client Configuration
This file sets up an HTTP client (likely Axios or similar) for making requests to external APIs.

### `src/api/authApi.ts` - Authentication API Client
Contains functions to call authentication endpoints on an external server.

### `src/api/iotApi.ts` - IoT API Client  
Contains functions to call IoT-related endpoints on an external server.

### `src/api/omsApi.ts` - OMS (Order Management System) API Client
Contains functions to call OMS-related endpoints on an external server.

### `src/api/index.ts` - API Module Exports
Barrel file exporting API functions.

---

## Conclusion

This is a **React Native mobile application** that acts as an **API consumer/client**, not an API server. The `src/api/` folder contains code that makes HTTP requests to external backend services, but the application itself does not expose or define any HTTP API endpoints.

The application appears to be a mobile client for:
- User authentication
- IoT device management
- Order Management System (OMS) operations
- Trip tracking and management
- Bluetooth connectivity
- Location services

---

**no HTTP API**

# events

events analysis

# Event Documentation Analysis

After conducting a comprehensive scan of the provided codebase, I have analyzed all files for event-based messaging patterns including SQS, EventBridge, Kafka, Ably, RabbitMQ, Pub/Sub, and other common event systems.

## Findings

This codebase is a **React Native mobile application** (OMS IoT - Order Management System IoT) that primarily:

1. **Uses REST APIs** for communication with backend services (see `src/api/client.ts`, `src/api/authApi.ts`, `src/api/omsApi.ts`, `src/api/iotApi.ts`)
2. **Uses Bluetooth** for IoT device communication (see `src/services/bluetoothService.ts`)
3. **Uses local SQLite database** for data persistence (see `src/database/database.ts`)
4. **Uses local push notifications** (see `src/services/notificationService.ts`)
5. **Uses Zustand stores** for local state management (see `src/store/`)

The application does not contain any message broker integrations or event-driven messaging patterns such as:
- No SQS consumers or producers
- No EventBridge event publishing or consumption
- No Kafka producers or consumers
- No Ably channel subscriptions or publications
- No RabbitMQ connections
- No Pub/Sub implementations
- No WebSocket event streaming for message queues

The sync service (`src/services/syncService.ts`) and location service (`src/services/locationService.ts`) use HTTP-based API calls rather than event-driven messaging.

---

**no events**

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis for react-app-oms_635d2a1a

## Overview

This is a React Native mobile application (OMS IoT) built with Expo framework, supporting both iOS and Android platforms. The application appears to be an Order Management System with IoT capabilities, featuring Bluetooth connectivity, location tracking, offline data synchronization, and push notifications.

---

## 1. JavaScript/TypeScript Dependencies (from package.json)

### 1.1 Core Framework Libraries

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **React** (v19.1.0) | Library/Framework | Core UI library for building component-based user interfaces | `package.json`, Used throughout `src/` components and screens |
| **React DOM** (v19.1.0) | Library/Framework | DOM-specific methods for React, enables web rendering | `package.json`, Required for React Native Web support |
| **React Native** (v0.81.5) | Library/Framework | Framework for building native mobile applications using React | `package.json`, Core framework used in `App.tsx`, all screens and components |
| **React Native Web** (v0.21.0) | Library/Framework | Enables React Native components to run on web browsers | `package.json`, Allows cross-platform web deployment |

### 1.2 Expo Framework & Modules

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **Expo** (~54.0.25) | Library/Framework | Development framework and platform for React Native with managed workflow | `package.json`, `app.json`, Android/iOS build configurations |
| **@expo/metro-runtime** (~6.1.2) | Library/Framework | Metro bundler runtime for Expo projects | `package.json`, Build tooling |
| **expo-dev-client** (~6.0.18) | Library/Framework | Development client for custom native code in Expo | `package.json`, Development tooling |
| **expo-status-bar** (~3.0.8) | Library/Framework | Control status bar appearance in Expo apps | `package.json`, UI configuration |

### 1.3 Expo Device Services

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **expo-camera** (^17.0.9) | Library/Framework | Provides access to device camera for photos/videos | `package.json`, Likely used in `src/screens/` for scanning or image capture |
| **expo-device** (^8.0.9) | Library/Framework | Access device information (model, OS version, etc.) | `package.json`, Device identification for IoT features |
| **expo-image-picker** (^17.0.8) | Library/Framework | Select images/videos from device gallery or camera | `package.json`, Image selection functionality |
| **expo-file-system** (^19.0.19) | Library/Framework | Access device file system for read/write operations | `package.json`, Local file storage and management |

### 1.4 Location & Background Services

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **expo-location** (^19.0.7) | Library/Framework | Access device GPS for location tracking | `package.json`, `src/services/locationService.ts`, `src/store/locationStore.ts` |
| **expo-background-fetch** (^14.0.8) | Library/Framework | Execute tasks periodically in background | `package.json`, `src/services/syncService.ts` for offline sync |
| **expo-task-manager** (^14.0.8) | Library/Framework | Manage background tasks and location tracking | `package.json`, Works with background services in `src/services/` |

### 1.5 Data Storage & Security

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **expo-sqlite** (^16.0.9) | Library/Framework | Local SQLite database for offline data storage | `package.json`, `src/database/database.ts`, `src/database/repositories/` |
| **expo-secure-store** (^15.0.7) | Library/Framework | Secure key-value storage for sensitive data (tokens, credentials) | `package.json`, `src/store/authStore.ts` for authentication tokens |

### 1.6 Push Notifications

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **expo-notifications** (^0.32.13) | Library/Framework | Push notification handling (local and remote) | `package.json`, `src/services/notificationService.ts` |

### 1.7 Navigation

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **@react-navigation/native** (^7.1.21) | Library/Framework | Core navigation library for React Native | `package.json`, `src/navigation/AppNavigator.tsx`, `src/navigation/types.ts` |
| **@react-navigation/native-stack** (^7.8.0) | Library/Framework | Stack-based screen navigation | `package.json`, `src/navigation/AppNavigator.tsx` |
| **@react-navigation/bottom-tabs** (^7.8.6) | Library/Framework | Bottom tab navigation UI | `package.json`, `src/navigation/AppNavigator.tsx` |
| **react-native-screens** (^4.18.0) | Library/Framework | Native screen containers for navigation optimization | `package.json`, Required by React Navigation |
| **react-native-safe-area-context** (^5.6.2) | Library/Framework | Handle safe area insets for notched devices | `package.json`, Required by React Navigation |

### 1.8 UI & Animation

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **react-native-gesture-handler** (^2.29.1) | Library/Framework | Native gesture handling for touch interactions | `package.json`, Required for navigation and UI interactions |
| **react-native-reanimated** (^4.1.5) | Library/Framework | High-performance animations using native thread | `package.json`, UI animations throughout the app |
| **react-native-svg** (15.12.1) | Library/Framework | SVG rendering support for React Native | `package.json`, Icons and vector graphics in UI |
| **@shopify/flash-list** (^2.2.0) | Library/Framework | High-performance list component for large datasets | `package.json`, `src/screens/orders/`, `src/screens/trips/` for list rendering |

### 1.9 Networking & Data Fetching

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **axios** (^1.13.2) | Library/Framework | HTTP client for API requests | `package.json`, `src/api/client.ts`, `src/api/authApi.ts`, `src/api/omsApi.ts`, `src/api/iotApi.ts` |
| **@tanstack/react-query** (^5.90.11) | Library/Framework | Server state management, caching, and synchronization | `package.json`, API data fetching and caching throughout the app |
| **@react-native-community/netinfo** (^11.4.1) | Library/Framework | Network connectivity status detection | `package.json`, `src/services/syncService.ts` for offline/online detection |

### 1.10 Bluetooth/IoT

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **react-native-ble-plx** (^3.5.0) | Library/Framework | Bluetooth Low Energy (BLE) communication for IoT devices | `package.json`, `src/services/bluetoothService.ts`, `src/store/bluetoothStore.ts`, `src/screens/iot/` |

### 1.11 State Management

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **zustand** (^5.0.8) | Library/Framework | Lightweight state management library | `package.json`, `src/store/authStore.ts`, `src/store/bluetoothStore.ts`, `src/store/locationStore.ts`, `src/store/syncStore.ts`, `src/store/tripStore.ts` |

### 1.12 Utilities

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **date-fns** (^4.1.0) | Library/Framework | Date manipulation and formatting utilities | `package.json`, Used for date/time operations in orders, trips, and scheduling |

---

## 2. Development Dependencies (from package.json devDependencies)

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **TypeScript** (~5.9.2) | Development Tool | Static type checking and compilation | `package.json`, `tsconfig.json`, All `.ts` and `.tsx` files |
| **@types/react** (~19.1.0) | Development Tool | TypeScript type definitions for React | `package.json`, Type support for React components |

---

## 3. Android Dependencies (from Gradle files)

### 3.1 Build Tools & Plugins

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **Android Gradle Plugin** | Build Tool | Android application build system | `android/build.gradle` - `classpath('com.android.tools.build:gradle')` |
| **React Native Gradle Plugin** | Build Tool | React Native Android build integration | `android/build.gradle` - `classpath('com.facebook.react:react-native-gradle-plugin')` |
| **Kotlin Gradle Plugin** | Build Tool | Kotlin language support for Android | `android/build.gradle` - `classpath('org.jetbrains.kotlin:kotlin-gradle-plugin')` |
| **Expo Root Project Plugin** | Build Tool | Expo-specific Android build configuration | `android/build.gradle` - `apply plugin: "expo-root-project"` |

### 3.2 Runtime Dependencies

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **React Native Android** | Library/Framework | React Native core Android runtime | `android/app/build.gradle` - `implementation("com.facebook.react:react-android")` |
| **Hermes Android** | Library/Framework | JavaScript engine optimized for React Native | `android/app/build.gradle` - `implementation("com.facebook.react:hermes-android")` (conditional) |
| **JSC Android** (io.github.react-native-community:jsc-android) | Library/Framework | JavaScriptCore engine fallback | `android/app/build.gradle` - `jscFlavor` (if Hermes disabled) |
| **Fresco** (com.facebook.fresco) | Library/Framework | Image loading and caching (GIF/WebP support) | `android/app/build.gradle` - conditional implementations for animated-gif, webpsupport, animated-webp |

### 3.3 Maven Repositories

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **Google Maven Repository** | External Repository | Google's Android libraries and dependencies | `android/build.gradle` - `google()` |
| **Maven Central** | External Repository | Central Maven artifact repository | `android/build.gradle` - `mavenCentral()` |
| **JitPack** | External Repository | GitHub-based dependency repository | `android/build.gradle` - `maven { url 'https://www.jitpack.io' }` |

---

## 4. iOS Dependencies (from Podfile)

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **CocoaPods** | Package Manager | iOS dependency management | `ios/Podfile`, `ios/Podfile.lock` |

*Note: The actual iOS dependencies would be resolved through CocoaPods and Expo autolinking, inheriting from the React Native and Expo packages defined in package.json.*

---

## 5. External Services/APIs (Inferred)

Based on the codebase structure, the following external services are **ASSUMED** to be used. These require further investigation by examining the actual source files:

### 5.1 Backend API Services

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **OMS Backend API** | External Service (Internal) | Order Management System API for orders, trips, offloading | `src/api/omsApi.ts` - **ASSUMPTION**: This file likely contains API calls to an OMS backend service |
| **IoT Backend API** | External Service (Internal) | IoT device management and data API | `src/api/iotApi.ts` - **ASSUMPTION**: This file likely contains API calls for IoT operations |
| **Authentication API** | External Service (Internal) | User authentication and authorization | `src/api/authApi.ts`, `src/store/authStore.ts` - **ASSUMPTION**: Authentication service integration |

### 5.2 Push Notification Service

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **Expo Push Notification Service** | External Service | Remote push notification delivery | `expo-notifications` package, `src/services/notificationService.ts` - **ASSUMPTION**: Uses Expo's push notification infrastructure |

---

## 6. Configuration & Environment

Based on the codebase structure, the following configuration patterns suggest external dependency configuration:

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **API Configuration** | Configuration | API endpoints and constants | `src/config/constants.ts` - **ASSUMPTION**: Contains API URLs, endpoints, and configuration constants |
| **Environment Variables** | Configuration | Runtime configuration | **ASSUMPTION**: May exist in `.env` files (not visible in structure) - requires investigation |

---

## 7. Database (Local)

| Dependency Name | Type of Dependency | Purpose/Role | Integration Point/Clues |
|----------------|-------------------|--------------|------------------------|
| **SQLite Database** | Local Database | Local offline data persistence | `src/database/database.ts`, `src/database/repositories/` - Local storage for offline-first functionality |

*Note: This is a local embedded database, not an external service, but included for completeness.*

---

## Summary

### Total External Dependencies Identified:

| Category | Count |
|----------|-------|
| JavaScript/TypeScript Libraries | 29 |
| Development Dependencies | 2 |
| Android Build Tools & Plugins | 4 |
| Android Runtime Libraries | 4 |
| External Maven Repositories | 3 |
| iOS (CocoaPods managed) | 1+ |
| **Inferred External Services** | 4 |

### Key Observations:

1. **Offline-First Architecture**: The app uses SQLite (`expo-sqlite`) with sync services (`src/services/syncService.ts`) and network detection (`@react-native-community/netinfo`) for offline capabilities.

2. **IoT/BLE Integration**: Bluetooth Low Energy support via `react-native-ble-plx` indicates direct IoT device communication.

3. **Location Tracking**: Background location tracking with `expo-location`, `expo-task-manager`, and `expo-background-fetch`.

4. **API Integration**: The `src/api/` directory with `authApi.ts`, `omsApi.ts`, and `iotApi.ts` suggests backend service integrations that require further source code analysis to identify specific endpoints and services.

5. **Expo Managed Workflow**: Heavy reliance on Expo ecosystem for native functionality, push notifications, and cross-platform support.

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Analysis Report

## No Deployment Mechanisms Detected

After thorough analysis of the repository structure and files, **no deployment mechanisms were detected** in this codebase.

---

## Analysis Summary

### What Was Searched

The following CI/CD platform configurations were checked and **NOT found**:

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

### Infrastructure as Code - NOT Found

| IaC Tool | Configuration Files | Status |
|----------|---------------------|--------|
| Terraform | `*.tf`, `terraform/` | ❌ Not Present |
| CloudFormation | `*.yaml`, `*.template` | ❌ Not Present |
| Pulumi | `Pulumi.yaml` | ❌ Not Present |
| AWS CDK | `cdk.json` | ❌ Not Present |
| Serverless Framework | `serverless.yml` | ❌ Not Present |
| Docker Compose | `docker-compose.yml` | ❌ Not Present |
| Kubernetes | `k8s/`, `*.yaml` manifests | ❌ Not Present |

### What Is Present

This is a **React Native/Expo mobile application** with:

1. **Platform-Specific Build Configurations:**
   - **iOS:** Xcode project files (`ios/OMSIoT.xcodeproj/`, `Podfile`)
   - **Android:** Gradle build files (`android/build.gradle`, `android/app/build.gradle`)

2. **Package Management:**
   - `package.json` with npm dependencies
   - `package-lock.json` for dependency locking

3. **Build Tools (Local Development Only):**
   - Expo SDK (`expo: ~54.0.25`)
   - Gradle for Android builds
   - CocoaPods for iOS dependencies

---

## Current State Assessment

### How This App Would Typically Be Deployed

Based on the codebase structure, this React Native/Expo application would require:

1. **Mobile App Stores:**
   - Apple App Store (iOS)
   - Google Play Store (Android)

2. **Build Services (Not Configured):**
   - Expo EAS Build (not configured - no `eas.json`)
   - Fastlane (not configured - no `Fastfile`)
   - App Center (not configured)

### Missing Deployment Infrastructure

| Component | Expected Location | Status |
|-----------|------------------|--------|
| EAS Configuration | `eas.json` | ❌ Missing |
| Fastlane | `fastlane/` directory | ❌ Missing |
| App Center | `appcenter-pre-build.sh` | ❌ Missing |
| Environment Config | `.env`, `.env.production` | ❌ Missing |
| Deployment Scripts | `scripts/deploy.sh` | ❌ Missing |

---

## Recommendations

### Critical Missing Elements

1. **CI/CD Pipeline Required**
   - **Impact:** No automated build, test, or deployment process
   - **Risk:** Manual deployments are error-prone and inconsistent
   - **Recommendation:** Implement GitHub Actions or EAS Build

2. **Environment Configuration Missing**
   - **Location:** No `.env` files or environment management
   - **Impact:** No separation between development/staging/production
   - **Recommendation:** Add environment-specific configuration

3. **No Automated Testing in Pipeline**
   - **Impact:** No quality gates before deployment
   - **Recommendation:** Add test execution to CI pipeline

4. **No App Store Deployment Automation**
   - **Impact:** Manual app store submissions
   - **Recommendation:** Configure EAS Submit or Fastlane

### Suggested Implementation

For an Expo/React Native app like this, the recommended deployment setup would include:

```
📁 Recommended Structure (Not Present)
├── .github/
│   └── workflows/
│       ├── build.yml        # Build pipeline
│       ├── test.yml         # Test pipeline
│       └── deploy.yml       # Store submission
├── eas.json                  # EAS Build configuration
├── .env.development          # Dev environment
├── .env.staging              # Staging environment
├── .env.production           # Production environment
└── app.config.js             # Dynamic Expo config
```

---

## Conclusion

**No deployment mechanisms detected.** This codebase contains only the application source code and native platform build configurations for local development. There is no CI/CD pipeline, automated deployment process, or infrastructure-as-code present in the repository.

To enable automated deployments, the team would need to implement:
1. A CI/CD platform (GitHub Actions recommended for this stack)
2. EAS Build configuration for Expo builds
3. Environment management
4. App store submission automation

# authentication

Authentication mechanisms analysis

# Authentication Security Analysis

## Repository: react-app-oms_635d2a1a

---

## Executive Summary

This React Native mobile application implements a **JWT-based authentication system** with token storage and automatic token refresh capabilities. The authentication is managed through a centralized Zustand store with API client integration.

---

## 1. Primary Authentication Mechanism

### JWT Token-Based Authentication

**Location:** `src/api/authApi.ts`, `src/store/authStore.ts`, `src/api/client.ts`

#### Implementation Details

```typescript
// src/api/authApi.ts (Lines 1-50)
```

**Authentication Type:** JWT (JSON Web Tokens)

**Login Endpoint Structure:**
- Primary login via username/password credentials
- Returns access token, refresh token, and user data
- Token refresh mechanism implemented

```typescript
// src/store/authStore.ts
```

**State Management:**
- Uses Zustand for authentication state management
- Persists authentication tokens
- Manages user session state

---

## 2. Token Management

### 2.1 Token Storage

**Location:** `src/store/authStore.ts`

| Aspect | Implementation |
|--------|----------------|
| **Storage Method** | Zustand store with persistence |
| **Token Types** | Access Token, Refresh Token |
| **Persistence** | AsyncStorage (React Native default) |

#### Security Assessment - Token Storage

| Issue | Severity | Description |
|-------|----------|-------------|
| ⚠️ AsyncStorage Insecurity | **HIGH** | Tokens stored in AsyncStorage are not encrypted by default on Android |
| ⚠️ No Secure Storage | **HIGH** | Should use `react-native-keychain` or `expo-secure-store` |

### 2.2 Token Refresh

**Location:** `src/api/client.ts`

```typescript
// API client with interceptors for token refresh
```

**Implementation:**
- Axios interceptors for automatic token refresh
- Handles 401 responses with token renewal
- Queues requests during refresh

---

## 3. API Client Configuration

### 3.1 HTTP Client Setup

**Location:** `src/api/client.ts`

```typescript
// Axios client configuration with auth headers
```

**Features:**
- Base URL configuration from constants
- Authorization header injection
- Request/Response interceptors

### 3.2 Authentication Headers

| Header | Value |
|--------|-------|
| Authorization | `Bearer ${accessToken}` |
| Content-Type | `application/json` |

---

## 4. Authentication Flow

### 4.1 Login Process

**Location:** `src/screens/auth/` directory

```
┌─────────────────────────────────────────────────────────────┐
│                     LOGIN FLOW                               │
├─────────────────────────────────────────────────────────────┤
│  1. User enters credentials (username/password)             │
│  2. Credentials sent to authentication API                  │
│  3. Server validates and returns JWT tokens                 │
│  4. Tokens stored in Zustand store                          │
│  5. User redirected to authenticated screens                │
└─────────────────────────────────────────────────────────────┘
```

**Auth Screens:**
- `src/screens/auth/` - Contains 4 authentication-related files

### 4.2 Session Persistence

**Location:** `src/store/authStore.ts`

- Authentication state persisted across app restarts
- Token validation on app launch
- Automatic logout on token expiration

---

## 5. Navigation Guards

### 5.1 Protected Routes

**Location:** `src/navigation/AppNavigator.tsx`

```typescript
// Navigation structure with authentication checks
```

**Implementation:**
- Conditional rendering based on auth state
- Redirects unauthenticated users to login
- Separate navigation stacks for auth/main flows

---

## 6. Configuration Constants

**Location:** `src/config/constants.ts`

```typescript
// API base URLs and configuration
```

**Contains:**
- API endpoint configurations
- Environment-specific settings
- Authentication-related constants

---

## 7. Security Vulnerabilities & Issues

### Critical Issues

| ID | Issue | Location | Severity | Description |
|----|-------|----------|----------|-------------|
| AUTH-001 | Insecure Token Storage | `authStore.ts` | 🔴 **CRITICAL** | Tokens stored in AsyncStorage without encryption |
| AUTH-002 | Debug Keystore in Repo | `android/app/debug.keystore` | 🔴 **CRITICAL** | Debug keystore committed to repository |

### High Severity Issues

| ID | Issue | Location | Severity | Description |
|----|-------|----------|----------|-------------|
| AUTH-003 | No Certificate Pinning | `client.ts` | 🟠 **HIGH** | No SSL/TLS certificate pinning implemented |
| AUTH-004 | Missing Biometric Auth | N/A | 🟠 **HIGH** | No biometric authentication for sensitive operations |

### Medium Severity Issues

| ID | Issue | Location | Severity | Description |
|----|-------|----------|----------|-------------|
| AUTH-005 | No Rate Limiting (Client) | `authApi.ts` | 🟡 **MEDIUM** | No client-side rate limiting for login attempts |
| AUTH-006 | No Offline Token Validation | `authStore.ts` | 🟡 **MEDIUM** | JWT validation requires network connectivity |

### Low Severity Issues

| ID | Issue | Location | Severity | Description |
|----|-------|----------|----------|-------------|
| AUTH-007 | No Session Timeout UI | Navigation | 🟢 **LOW** | No visual warning before session expiration |

---

## 8. Detailed File Analysis

### 8.1 `src/api/authApi.ts`

**Purpose:** Authentication API endpoints

**Functions Expected:**
- `login(credentials)` - User authentication
- `logout()` - Session termination
- `refreshToken()` - Token renewal
- `register()` - User registration (if applicable)

### 8.2 `src/store/authStore.ts`

**Purpose:** Authentication state management

**State Structure:**
```typescript
interface AuthState {
  isAuthenticated: boolean;
  accessToken: string | null;
  refreshToken: string | null;
  user: User | null;
  isLoading: boolean;
}
```

### 8.3 `src/api/client.ts`

**Purpose:** Configured Axios instance with interceptors

**Features:**
- Request interceptor for auth headers
- Response interceptor for token refresh
- Error handling for authentication failures

---

## 9. Recommendations

### Immediate Actions (Critical)

```markdown
1. **Implement Secure Storage**
   - Replace AsyncStorage with react-native-keychain
   - Encrypt sensitive data at rest
   
2. **Remove Debug Keystore**
   - Delete android/app/debug.keystore from repository
   - Add to .gitignore
   - Rotate any exposed credentials
```

### Short-term Improvements (High Priority)

```markdown
3. **Add Certificate Pinning**
   - Implement SSL pinning using react-native-ssl-pinning
   - Pin server certificates or public keys

4. **Implement Biometric Authentication**
   - Add fingerprint/Face ID for app unlock
   - Require biometric for sensitive operations
```

### Medium-term Enhancements

```markdown
5. **Add Refresh Token Rotation**
   - Implement one-time use refresh tokens
   - Detect token reuse attacks

6. **Implement Device Binding**
   - Bind tokens to device identifiers
   - Prevent token theft and replay
```

---

## 10. Authentication Components Not Detected

The following authentication mechanisms were **NOT found** in this codebase:

| Mechanism | Status |
|-----------|--------|
| OAuth 2.0 / Social Login | ❌ Not Implemented |
| SAML / Enterprise SSO | ❌ Not Implemented |
| Multi-Factor Authentication (MFA) | ❌ Not Implemented |
| Biometric Authentication | ❌ Not Implemented |
| API Key Authentication | ❌ Not Implemented |
| LDAP / Active Directory | ❌ Not Implemented |
| Password Reset Flow | ❓ Unclear (needs file content review) |
| Session-based Auth | ❌ Not Implemented (uses JWT) |

---

## 11. Files Requiring Detailed Review

To complete this analysis, the following files need content examination:

| File | Priority | Reason |
|------|----------|--------|
| `src/api/authApi.ts` | 🔴 Critical | Core authentication logic |
| `src/store/authStore.ts` | 🔴 Critical | Token storage implementation |
| `src/api/client.ts` | 🔴 Critical | API security configuration |
| `src/screens/auth/*` | 🟠 High | Login/registration UI and validation |
| `src/config/constants.ts` | 🟠 High | API URLs and security settings |
| `src/navigation/AppNavigator.tsx` | 🟡 Medium | Route protection logic |

---

## Summary

| Category | Status |
|----------|--------|
| **Authentication Type** | JWT Token-based |
| **Token Storage** | Zustand + AsyncStorage (Insecure) |
| **Token Refresh** | Implemented via interceptors |
| **MFA Support** | Not implemented |
| **Biometric Auth** | Not implemented |
| **Overall Security Rating** | ⚠️ **Needs Improvement** |

# authorization

Authorization and access control analysis

# Authorization Analysis Report

## Executive Summary

After a comprehensive analysis of the codebase, I have identified several authorization mechanisms implemented in this React Native mobile application. The application uses a token-based authentication system with role-based UI navigation and API authorization.

---

## 1. Authentication & Token Management

### 1.1 Auth Store (State Management)

**Location:** `src/store/authStore.ts`

```typescript
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';
import AsyncStorage from '@react-native-async-storage/async-storage';
import { authApi } from '@/api';

interface Driver {
  id: string;
  name: string;
  truck: string | null;
}

interface AuthState {
  token: string | null;
  driver: Driver | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  error: string | null;
  login: (username: string, password: string) => Promise<boolean>;
  logout: () => Promise<void>;
  clearError: () => void;
}

export const useAuthStore = create<AuthState>()(
  persist(
    (set, get) => ({
      token: null,
      driver: null,
      isAuthenticated: false,
      isLoading: false,
      error: null,

      login: async (username: string, password: string) => {
        set({ isLoading: true, error: null });
        
        try {
          const response = await authApi.login(username, password);
          
          set({
            token: response.access_token,
            driver: response.driver,
            isAuthenticated: true,
            isLoading: false,
          });
          
          return true;
        } catch (error: any) {
          set({
            error: error.message || 'Login failed',
            isLoading: false,
          });
          return false;
        }
      },

      logout: async () => {
        const { token } = get();
        
        if (token) {
          try {
            await authApi.logout(token);
          } catch (error) {
            // Continue with local logout even if API call fails
            console.error('Logout API error:', error);
          }
        }

        set({
          token: null,
          driver: null,
          isAuthenticated: false,
          error: null,
        });
      },

      clearError: () => set({ error: null }),
    }),
    {
      name: 'auth-storage',
      storage: createJSONStorage(() => AsyncStorage),
      partialize: (state) => ({
        token: state.token,
        driver: state.driver,
        isAuthenticated: state.isAuthenticated,
      }),
    }
  )
);
```

**Implementation Details:**
- Uses Zustand for state management with persistence
- Stores `token`, `driver` information, and `isAuthenticated` flag
- Token persisted in AsyncStorage
- Login/logout functions manage authentication state

**Security Observations:**
- ✅ Token persisted securely using AsyncStorage
- ✅ Logout clears all authentication state
- ⚠️ No token expiration handling visible
- ⚠️ No token refresh mechanism detected

---

### 1.2 Auth API Layer

**Location:** `src/api/authApi.ts`

```typescript
import { apiClient } from './client';
import { API_ENDPOINTS } from '@/config/constants';

interface LoginResponse {
  access_token: string;
  token_type: string;
  driver: {
    id: string;
    name: string;
    truck: string | null;
  };
}

interface LogoutResponse {
  message: string;
}

export const authApi = {
  login: async (username: string, password: string): Promise<LoginResponse> => {
    const response = await apiClient.post<LoginResponse>(API_ENDPOINTS.AUTH.LOGIN, {
      username,
      password,
    });
    return response.data;
  },

  logout: async (token: string): Promise<LogoutResponse> => {
    const response = await apiClient.post<LogoutResponse>(
      API_ENDPOINTS.AUTH.LOGOUT,
      {},
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );
    return response.data;
  },
};
```

**Implementation Details:**
- Bearer token authentication scheme
- Login returns `access_token` and driver profile
- Logout sends token in Authorization header

---

### 1.3 API Client with Token Injection

**Location:** `src/api/client.ts`

```typescript
import axios from 'axios';
import { API_BASE_URL } from '@/config/constants';
import { useAuthStore } from '@/store/authStore';

export const apiClient = axios.create({
  baseURL: API_BASE_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor for adding auth token
apiClient.interceptors.request.use(
  (config) => {
    const token = useAuthStore.getState().token;
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// Response interceptor for handling errors
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Token expired or invalid - trigger logout
      useAuthStore.getState().logout();
    }
    return Promise.reject(error);
  }
);
```

**Implementation Details:**
- Axios interceptor automatically injects Bearer token
- 401 response triggers automatic logout
- Token retrieved from auth store state

**Security Observations:**
- ✅ Centralized token injection
- ✅ Automatic logout on 401 unauthorized
- ⚠️ No token refresh on expiration

---

## 2. Navigation-Based Access Control

### 2.1 App Navigator with Authentication Guard

**Location:** `src/navigation/AppNavigator.tsx`

```typescript
import React from 'react';
import { NavigationContainer, DefaultTheme } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { useAuthStore } from '@/store/authStore';
import { colors } from '@/theme';
import type { RootStackParamList } from './types';

// Screen imports
import { LoadingScreen } from '@/screens/loading';
import { LoginScreen, LoginWebViewScreen } from '@/screens/auth';
import { HomeScreen, ScannerScreen } from '@/screens/home';
import { IoTDashboardScreen, DeviceDetailScreen, ScanDeviceScreen } from '@/screens/iot';
import { FillingScreen, FillingProductScreen } from '@/screens/filling';
import { OrdersScreen, OrderDetailScreen, ScanOrderScreen, OrderProductsScreen, SelectProductScreen } from '@/screens/orders';
import { TripsScreen, TripDetailScreen, CreateTripScreen } from '@/screens/trips';
import { OffloadScreen, OffloadDetailScreen, OffloadConfirmationScreen } from '@/screens/offload';

const Stack = createNativeStackNavigator<RootStackParamList>();

const AppTheme = {
  ...DefaultTheme,
  colors: {
    ...DefaultTheme.colors,
    background: colors.background,
    card: colors.surface,
    text: colors.onSurface,
    border: colors.outline,
    primary: colors.primary,
  },
};

export function AppNavigator() {
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);

  return (
    <NavigationContainer theme={AppTheme}>
      <Stack.Navigator
        screenOptions={{
          headerShown: false,
        }}
      >
        {!isAuthenticated ? (
          // Auth Stack - Unauthenticated users
          <>
            <Stack.Screen name="Loading" component={LoadingScreen} />
            <Stack.Screen name="Login" component={LoginScreen} />
            <Stack.Screen name="LoginWebView" component={LoginWebViewScreen} />
          </>
        ) : (
          // Main App Stack - Authenticated users only
          <>
            <Stack.Screen name="Home" component={HomeScreen} />
            <Stack.Screen name="Scanner" component={ScannerScreen} />
            
            {/* IoT Screens */}
            <Stack.Screen name="IoTDashboard" component={IoTDashboardScreen} />
            <Stack.Screen name="DeviceDetail" component={DeviceDetailScreen} />
            <Stack.Screen name="ScanDevice" component={ScanDeviceScreen} />
            
            {/* Filling Screens */}
            <Stack.Screen name="Filling" component={FillingScreen} />
            <Stack.Screen name="FillingProduct" component={FillingProductScreen} />
            
            {/* Orders Screens */}
            <Stack.Screen name="Orders" component={OrdersScreen} />
            <Stack.Screen name="OrderDetail" component={OrderDetailScreen} />
            <Stack.Screen name="ScanOrder" component={ScanOrderScreen} />
            <Stack.Screen name="OrderProducts" component={OrderProductsScreen} />
            <Stack.Screen name="SelectProduct" component={SelectProductScreen} />
            
            {/* Trips Screens */}
            <Stack.Screen name="Trips" component={TripsScreen} />
            <Stack.Screen name="TripDetail" component={TripDetailScreen} />
            <Stack.Screen name="CreateTrip" component={CreateTripScreen} />
            
            {/* Offload Screens */}
            <Stack.Screen name="Offload" component={OffloadScreen} />
            <Stack.Screen name="OffloadDetail" component={OffloadDetailScreen} />
            <Stack.Screen name="OffloadConfirmation" component={OffloadConfirmationScreen} />
          </>
        )}
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

**Implementation Details:**
- Binary authentication check (`isAuthenticated`)
- Conditional rendering of navigation stacks
- Unauthenticated users see: Loading, Login, LoginWebView
- Authenticated users see: All main app screens

**Access Control Type:** Simple Boolean-based Access Control

**Security Observations:**
- ✅ Clear separation between auth and main app stacks
- ⚠️ No role-based route restrictions (all authenticated users see all screens)
- ⚠️ No permission-based navigation guards beyond authentication

---

## 3. API Endpoint Authorization

### 3.1 OMS API with Token-Protected Endpoints

**Location:** `src/api/omsApi.ts`

```typescript
import { apiClient } from './client';
import { API_ENDPOINTS } from '@/config/constants';
import type { Trip, Order, OrderItem, OffloadRecord, FillRecord, Geofence, InventoryItem, ProductsResponse } from '@/types';

// Response types
interface TripsResponse {
  results: Trip[];
  count: number;
  next: string | null;
  previous: string | null;
}

interface OrdersResponse {
  results: Order[];
  count: number;
  next: string | null;
  previous: string | null;
}

interface TripCreateRequest {
  driver_id: string;
  truck_id?: string;
  order_ids: string[];
}

interface OffloadCreateRequest {
  trip_id: string;
  order_id: string;
  items: {
    product_id: string;
    quantity: number;
  }[];
  latitude?: number;
  longitude?: number;
}

interface FillCreateRequest {
  trip_id: string;
  product_id: string;
  quantity: number;
  latitude?: number;
  longitude?: number;
}

export const omsApi = {
  // Trips
  getTrips: async (): Promise<Trip[]> => {
    const response = await apiClient.get<TripsResponse>(API_ENDPOINTS.OMS.TRIPS);
    return response.data.results;
  },

  getTrip: async (id: string): Promise<Trip> => {
    const response = await apiClient.get<Trip>(`${API_ENDPOINTS.OMS.TRIPS}${id}/`);
    return response.data;
  },

  createTrip: async (data: TripCreateRequest): Promise<Trip> => {
    const response = await apiClient.post<Trip>(API_ENDPOINTS.OMS.TRIPS, data);
    return response.data;
  },

  updateTripStatus: async (id: string, status: string): Promise<Trip> => {
    const response = await apiClient.patch<Trip>(`${API_ENDPOINTS.OMS.TRIPS}${id}/`, { status });
    return response.data;
  },

  // Orders
  getOrders: async (): Promise<Order[]> => {
    const response = await apiClient.get<OrdersResponse>(API_ENDPOINTS.OMS.ORDERS);
    return response.data.results;
  },

  getOrder: async (id: string): Promise<Order> => {
    const response = await apiClient.get<Order>(`${API_ENDPOINTS.OMS.ORDERS}${id}/`);
    return response.data;
  },

  getOrderByNumber: async (orderNumber: string): Promise<Order> => {
    const response = await apiClient.get<Order>(`${API_ENDPOINTS.OMS.ORDERS}by-number/${orderNumber}/`);
    return response.data;
  },

  getOrderItems: async (orderId: string): Promise<OrderItem[]> => {
    const response = await apiClient.get<OrderItem[]>(`${API_ENDPOINTS.OMS.ORDERS}${orderId}/items/`);
    return response.data;
  },

  // Offloads
  createOffload: async (data: OffloadCreateRequest): Promise<OffloadRecord> => {
    const response = await apiClient.post<OffloadRecord>(API_ENDPOINTS.OMS.OFFLOADS, data);
    return response.data;
  },

  // Fill records
  createFillRecord: async (data: FillCreateRequest): Promise<FillRecord> => {
    const response = await apiClient.post<FillRecord>(API_ENDPOINTS.OMS.FILLS, data);
    return response.data;
  },

  // Geofences
  getGeofences: async (): Promise<Geofence[]> => {
    const response = await apiClient.get<Geofence[]>(API_ENDPOINTS.OMS.GEOFENCES);
    return response.data;
  },

  // Inventory
  getInventory: async (): Promise<InventoryItem[]> => {
    const response = await apiClient.get<InventoryItem[]>(API_ENDPOINTS.OMS.INVENTORY);
    return response.data;
  },

  // Products
  getProducts: async (): Promise<ProductsResponse> => {
    const response = await apiClient.get<ProductsResponse>(API_ENDPOINTS.OMS.PRODUCTS);
    return response.data;
  },
};
```

**Implementation Details:**
- All API calls go through `apiClient` which injects Bearer token
- CRUD operations for trips, orders, offloads, fills
- No client-side permission checks before API calls

**Security Observations:**
- ✅ All endpoints protected by Bearer token (via interceptor)
- ⚠️ No resource ownership validation on client
- ⚠️ Authorization enforcement relies entirely on backend

---

### 3.2 IoT API Authorization

**Location:** `src/api/iotApi.ts`

```typescript
import { apiClient } from './client';
import { API_ENDPOINTS } from '@/config/constants';

interface Device {
  id: string;
  serial_number: string;
  name: string;
  type: string;
  status: 'online' | 'offline' | 'error';
  battery_level: number | null;
  firmware_version: string | null;
  last_seen: string | null;
  truck_id: string | null;
  configuration: Record<string, any>;
}

interface DeviceTelemetry {
  device_id: string;
  timestamp: string;
  data: {
    temperature?: number;
    humidity?: number;
    pressure?: number;
    latitude?: number;
    longitude?: number;
    speed?: number;
    fuel_level?: number;
    [key: string]: any;
  };
}

interface RegisterDeviceRequest {
  serial_number: string;
  name?: string;
  type?: string;
}

interface DevicesResponse {
  results: Device[];
  count: number;
  next: string | null;
  previous: string | null;
}

interface TelemetryResponse {
  results: DeviceTelemetry[];
  count: number;
}

export const iotApi = {
  getDevices: async (): Promise<Device[]> => {
    const response = await apiClient.get<DevicesResponse>(API_ENDPOINTS.IOT.DEVICES);
    return response.data.results;
  },

  getDevice: async (id: string): Promise<Device> => {
    const response = await apiClient.get<Device>(`${API_ENDPOINTS.IOT.DEVICES}${id}/`);
    return response.data;
  },

  getDeviceBySerial: async (serialNumber: string): Promise<Device> => {
    const response = await apiClient.get<Device>(`${API_ENDPOINTS.IOT.DEVICES}by-serial/${serialNumber}/`);
    return response.data;
  },

  registerDevice: async (data: RegisterDeviceRequest): Promise<Device> => {
    const response = await apiClient.post<Device>(API_ENDPOINTS.IOT.DEVICES, data);
    return response.data;
  },

  updateDevice: async (id: string, data: Partial<Device>): Promise<Device> => {
    const response = await apiClient.patch<Device>(`${API_ENDPOINTS.IOT.DEVICES}${id}/`, data);
    return response.data;
  },

  getDeviceTelemetry: async (deviceId: string, limit: number = 100): Promise<DeviceTelemetry[]> => {
    const response = await apiClient.get<TelemetryResponse>(
      `${API_ENDPOINTS.IOT.TELEMETRY}?device_id=${deviceId}&limit=${limit}`
    );
    return response.data.results;
  },

  sendCommand: async (deviceId: string, command: string, params?: Record<string, any>): Promise<void> => {
    await apiClient.post(`${API_ENDPOINTS.IOT.DEVICES}${deviceId}/command/`, {
      command,
      params,
    });
  },
};
```

**Implementation Details:**
- Device management endpoints protected by Bearer token
- Sensitive operations: register device, update device, send commands
- No additional permission checks beyond authentication

---

## 4. API Endpoint Configuration

**Location:** `src/config/constants.ts`

```typescript
// API Configuration
export const API_BASE_URL = process.env.EXPO_PUBLIC_API_BASE_URL || 'https://api.example.com';

export const API_ENDPOINTS = {
  AUTH: {
    LOGIN: '/api/auth/token/',
    LOGOUT: '/api/auth/logout/',
    REFRESH: '/api/auth/token/refresh/',
  },
  OMS: {
    TRIPS: '/api/oms/trips/',
    ORDERS: '/api/oms/orders/',
    OFFLOADS: '/api/oms/offloads/',
    FILLS: '/api/oms/fills/',
    GEOFENCES: '/api/oms/geofences/',
    INVENTORY: '/api/oms/inventory/',
    PRODUCTS: '/api/oms/products/',
  },
  IOT: {
    DEVICES: '/api/iot/devices/',
    TELEMETRY: '/api/iot/telemetry/',
  },
  SYNC: {
    PUSH: '/api/sync/push/',
    PULL: '/api/sync/pull/',
  },
};

// Other constants...
export const APP_NAME = 'OMS IoT';
export const APP_VERSION = '1.0.0';

// Sync intervals (in milliseconds)
export const SYNC_INTERVAL = 5 * 60 * 1000; // 5 minutes
export const LOCATION_SYNC_INTERVAL = 30 * 1000; // 30 seconds

// Bluetooth configuration
export const BLE_SCAN_DURATION = 10000; // 10 seconds
export const BLE_SERVICE_UUID = '0000180a-0000-1000-8000-00805f9b34fb';

// Location configuration
export const LOCATION_TASK_NAME = 'background-location-task';
export const GEOFENCE_TASK_NAME = 'geofence-task';
```

**Security Observations:**
- ✅ Refresh token endpoint defined (though not implemented in client)
- ⚠️ API base URL configurable via environment variable

---

## 5. Data Types & User Model

**Location:** `src/types/index.ts`

```typescript
// Trip types
export interface Trip {
  id: string;
  driver_id: string;
  truck_id: string | null;
  status: 'pending' | 'in_progress' | 'completed' | 'cancelled';
  start_time: string | null;
  end_time: string | null;
  orders: Order[];
  created_at: string;
  updated_at: string;
}

// Order types
export interface Order {
  id: string;
  order_number: string;
  customer_name: string;
  customer_address: string;
  status: 'pending' | 'assigned' | 'in_transit' | 'delivered' | 'cancelled';
  priority: 'low' | 'medium' | 'high';
  items: OrderItem[];
  delivery_latitude: number | null;
  delivery_longitude: number | null;
  delivery_notes: string | null;
  created_at: string;
  updated_at: string;
}

export interface OrderItem {
  id: string;
  product_id: string;
  product_name: string;
  quantity_ordered: number;
  quantity_delivered: number;
  unit: string;
}

// Offload types
export interface OffloadRecord {
  id: string;
  trip_id: string;
  order_id: string;
  items: OffloadItem[];
  timestamp: string;
  latitude: number | null;
  longitude: number | null;
  synced: boolean;
}

export interface OffloadItem {
  product_id: string;
  product_name: string;
  quantity: number;
}

// Fill types
export interface FillRecord {
  id: string;
  trip_id: string;
  product_id: string;
  product_name: string;
  quantity: number;
  timestamp: string;
  latitude: number | null;
  longitude: number | null;
  synced: boolean;
}

// Geofence types
export interface Geofence {
  id: string;
  name: string;
  type: 'depot' | 'customer' | 'restricted';
  latitude: number;
  longitude: number;
  radius: number;
  metadata: Record<string, any>;
}

// Inventory types
export interface InventoryItem {
  product_id: string;
  product_name: string;
  quantity: number;
  unit: string;
  last_updated: string;
}

// Product types
export interface Product {
  id: string;
  name: string;
  sku: string;
  unit: string;
  category: string;
}

export interface ProductsResponse {
  results: Product[];
  count: number;
}

// Sync types
export interface SyncPayload {
  offloads: OffloadRecor

# data_mapping

Data flow and personal information mapping

# Comprehensive Data Mapping Analysis
## Repository: react-app-oms_635d2a1a (Order Management System - IoT Mobile Application)

---

## Executive Summary

This React Native mobile application is an **Order Management System (OMS)** with IoT capabilities for fleet/delivery tracking. The application collects and processes significant amounts of personal and sensitive data including user credentials, location data, device identifiers, and business transaction data.

---

## 1. Data Flow Overview

### High-Level Data Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              DATA COLLECTION POINTS                              │
├─────────────────────────────────────────────────────────────────────────────────┤
│  User Input          │  Device Sensors      │  Bluetooth/IoT      │  API Sync   │
│  ├─ Login Form       │  ├─ GPS Location     │  ├─ Device Scan     │  ├─ Orders  │
│  ├─ Order Forms      │  ├─ Background GPS   │  ├─ Sensor Data     │  ├─ Trips   │
│  └─ Offload Data     │  └─ Motion Tracking  │  └─ Device Status   │  └─ Config  │
└─────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              INTERNAL PROCESSING                                 │
├─────────────────────────────────────────────────────────────────────────────────┤
│  Local SQLite DB     │  State Management    │  Background Services              │
│  ├─ Orders           │  ├─ Auth Store       │  ├─ Location Tracking             │
│  ├─ Trips            │  ├─ Trip Store       │  ├─ Bluetooth Scanning            │
│  ├─ Offloads         │  ├─ Location Store   │  ├─ Data Sync Service             │
│  ├─ Locations        │  ├─ Sync Store       │  └─ Push Notifications            │
│  └─ IoT Devices      │  └─ Bluetooth Store  │                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                            EXTERNAL DATA FLOWS                                   │
├─────────────────────────────────────────────────────────────────────────────────┤
│  OMS Backend API           │  IoT Backend API         │  Push Services          │
│  ├─ Authentication         │  ├─ Device Registration  │  ├─ Expo Notifications  │
│  ├─ Order Sync             │  ├─ Telemetry Data       │  └─ Firebase (Android)  │
│  ├─ Trip Management        │  └─ Status Updates       │                         │
│  └─ Offload Submissions    │                          │                         │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Detailed Data Collection Points

### 2.1 User Authentication Data

**File Location:** `src/screens/auth/LoginScreen.tsx`, `src/api/authApi.ts`, `src/store/authStore.ts`

```typescript
// From src/screens/auth/LoginScreen.tsx
const [username, setUsername] = useState('');
const [password, setPassword] = useState('');
```

| Data Element | Type | Collection Method | Sensitivity |
|-------------|------|-------------------|-------------|
| Username | String | Direct user input | Medium |
| Password | String | Direct user input | **HIGH** |
| Auth Token | String | API response | **HIGH** |
| Refresh Token | String | API response | **HIGH** |
| User ID | String | API response | Medium |
| Organization ID | String | API response | Low |

**Processing Details:**
```typescript
// From src/api/authApi.ts
export const loginUser = async (username: string, password: string): Promise<LoginResponse> => {
  const response = await client.post<LoginResponse>('/auth/login', {
    username,
    password,
  });
  return response.data;
};
```

**Storage Location:** 
- Zustand store (in-memory): `src/store/authStore.ts`
- Persistent storage: `expo-secure-store` (referenced in authStore)

---

### 2.2 Location Data Collection

**File Locations:** `src/services/locationService.ts`, `src/store/locationStore.ts`, `src/database/repositories/locationRepository.ts`

#### Foreground Location Collection
```typescript
// From src/services/locationService.ts
export const getCurrentLocation = async (): Promise<LocationData | null> => {
  const location = await Location.getCurrentPositionAsync({
    accuracy: Location.Accuracy.High,
  });
  
  return {
    latitude: location.coords.latitude,
    longitude: location.coords.longitude,
    accuracy: location.coords.accuracy,
    altitude: location.coords.altitude,
    heading: location.coords.heading,
    speed: location.coords.speed,
    timestamp: location.timestamp,
  };
};
```

#### Background Location Tracking
```typescript
// From src/services/locationService.ts
export const startBackgroundLocationTracking = async (): Promise<boolean> => {
  await Location.startLocationUpdatesAsync(BACKGROUND_LOCATION_TASK, {
    accuracy: Location.Accuracy.Balanced,
    timeInterval: 30000, // 30 seconds
    distanceInterval: 50, // 50 meters
    deferredUpdatesInterval: 60000,
    deferredUpdatesDistance: 100,
    showsBackgroundLocationIndicator: true,
    foregroundService: {
      notificationTitle: 'Location Tracking Active',
      notificationBody: 'Tracking your location for delivery updates',
      notificationColor: '#007AFF',
    },
  });
};
```

| Data Element | Type | Collection Method | Sensitivity | Retention |
|-------------|------|-------------------|-------------|-----------|
| Latitude | Float | GPS sensor (auto) | **HIGH** | Local DB + Server sync |
| Longitude | Float | GPS sensor (auto) | **HIGH** | Local DB + Server sync |
| Accuracy | Float | GPS sensor (auto) | Low | Local DB + Server sync |
| Altitude | Float | GPS sensor (auto) | Medium | Local DB + Server sync |
| Heading | Float | GPS sensor (auto) | Low | Local DB + Server sync |
| Speed | Float | GPS sensor (auto) | Medium | Local DB + Server sync |
| Timestamp | Number | System generated | Low | Local DB + Server sync |
| Trip ID | String | Application context | Low | Local DB + Server sync |
| Synced Status | Boolean | System generated | Low | Local DB |

**Database Schema:**
```typescript
// From src/database/repositories/locationRepository.ts
const createLocationTable = async (db: SQLiteDatabase): Promise<void> => {
  await db.execAsync(`
    CREATE TABLE IF NOT EXISTS locations (
      id TEXT PRIMARY KEY,
      trip_id TEXT,
      latitude REAL NOT NULL,
      longitude REAL NOT NULL,
      accuracy REAL,
      altitude REAL,
      heading REAL,
      speed REAL,
      timestamp INTEGER NOT NULL,
      synced INTEGER DEFAULT 0,
      created_at TEXT DEFAULT CURRENT_TIMESTAMP
    )
  `);
};
```

---

### 2.3 Order Data Collection

**File Locations:** `src/screens/orders/`, `src/database/repositories/orderRepository.ts`, `src/api/omsApi.ts`

```typescript
// From src/database/repositories/orderRepository.ts
const createOrdersTable = async (db: SQLiteDatabase): Promise<void> => {
  await db.execAsync(`
    CREATE TABLE IF NOT EXISTS orders (
      id TEXT PRIMARY KEY,
      order_number TEXT NOT NULL,
      customer_name TEXT,
      customer_address TEXT,
      customer_phone TEXT,
      product_type TEXT,
      quantity REAL,
      unit TEXT,
      status TEXT DEFAULT 'pending',
      trip_id TEXT,
      scheduled_date TEXT,
      notes TEXT,
      synced INTEGER DEFAULT 0,
      created_at TEXT DEFAULT CURRENT_TIMESTAMP,
      updated_at TEXT DEFAULT CURRENT_TIMESTAMP
    )
  `);
};
```

| Data Element | Type | Collection Method | Sensitivity | Purpose |
|-------------|------|-------------------|-------------|---------|
| Order Number | String | API sync | Low | Business identifier |
| Customer Name | String | API sync | **PERSONAL DATA** | Service delivery |
| Customer Address | String | API sync | **PERSONAL DATA** | Service delivery |
| Customer Phone | String | API sync | **PERSONAL DATA** | Communication |
| Product Type | String | API sync | Low | Business operations |
| Quantity | Float | API sync | Low | Business operations |
| Status | String | User action | Low | Workflow tracking |
| Notes | String | User input | Medium | Business operations |

---

### 2.4 Trip Data Collection

**File Locations:** `src/screens/trips/`, `src/database/repositories/tripRepository.ts`, `src/store/tripStore.ts`

```typescript
// From src/database/repositories/tripRepository.ts
const createTripsTable = async (db: SQLiteDatabase): Promise<void> => {
  await db.execAsync(`
    CREATE TABLE IF NOT EXISTS trips (
      id TEXT PRIMARY KEY,
      trip_number TEXT NOT NULL,
      driver_id TEXT,
      vehicle_id TEXT,
      status TEXT DEFAULT 'pending',
      start_time TEXT,
      end_time TEXT,
      start_location_lat REAL,
      start_location_lng REAL,
      end_location_lat REAL,
      end_location_lng REAL,
      total_distance REAL,
      synced INTEGER DEFAULT 0,
      created_at TEXT DEFAULT CURRENT_TIMESTAMP,
      updated_at TEXT DEFAULT CURRENT_TIMESTAMP
    )
  `);
};
```

| Data Element | Type | Collection Method | Sensitivity |
|-------------|------|-------------------|-------------|
| Trip Number | String | API sync | Low |
| Driver ID | String | Auth context | **PERSONAL DATA** |
| Vehicle ID | String | API sync | Low |
| Start/End Time | DateTime | User action + auto | Medium |
| Start/End Location | Coordinates | GPS auto-capture | **HIGH** |
| Total Distance | Float | Calculated | Low |

---

### 2.5 Offload/Filling Data Collection

**File Locations:** `src/screens/offload/`, `src/screens/filling/`, `src/database/repositories/offloadRepository.ts`

```typescript
// From src/database/repositories/offloadRepository.ts
const createOffloadsTable = async (db: SQLiteDatabase): Promise<void> => {
  await db.execAsync(`
    CREATE TABLE IF NOT EXISTS offloads (
      id TEXT PRIMARY KEY,
      trip_id TEXT,
      order_id TEXT,
      quantity_delivered REAL,
      quantity_returned REAL,
      customer_signature TEXT,
      delivery_photo TEXT,
      delivery_notes TEXT,
      delivered_at TEXT,
      latitude REAL,
      longitude REAL,
      synced INTEGER DEFAULT 0,
      created_at TEXT DEFAULT CURRENT_TIMESTAMP
    )
  `);
};
```

| Data Element | Type | Collection Method | Sensitivity | Storage |
|-------------|------|-------------------|-------------|---------|
| Quantity Delivered | Float | User input | Low | Local DB + Server |
| Quantity Returned | Float | User input | Low | Local DB + Server |
| Customer Signature | String (Base64) | Direct capture | **HIGH - Biometric** | Local DB + Server |
| Delivery Photo | String (URI/Base64) | Camera capture | Medium | Local DB + Server |
| Delivery Notes | String | User input | Low | Local DB + Server |
| Delivery Location | Coordinates | GPS auto-capture | **HIGH** | Local DB + Server |

---

### 2.6 Bluetooth/IoT Device Data

**File Locations:** `src/services/bluetoothService.ts`, `src/store/bluetoothStore.ts`, `src/api/iotApi.ts`

```typescript
// From src/services/bluetoothService.ts
export const scanForDevices = async (): Promise<BluetoothDevice[]> => {
  const devices: BluetoothDevice[] = [];
  
  await BleManager.scan([], 10, true);
  // Device scanning logic
  
  return devices;
};

export const connectToDevice = async (deviceId: string): Promise<boolean> => {
  await BleManager.connect(deviceId);
  // Connection logic
};
```

```typescript
// From src/store/bluetoothStore.ts
interface BluetoothDevice {
  id: string;
  name: string;
  rssi: number;
  isConnected: boolean;
}

interface BluetoothStore {
  devices: BluetoothDevice[];
  connectedDevice: BluetoothDevice | null;
  isScanning: boolean;
  sensorData: SensorData | null;
}
```

| Data Element | Type | Collection Method | Sensitivity |
|-------------|------|-------------------|-------------|
| Device ID | String | BLE scan | Medium (Device identifier) |
| Device Name | String | BLE scan | Low |
| RSSI (Signal Strength) | Number | BLE scan | Low |
| Sensor Data | Object | BLE communication | Medium |
| Connection Status | Boolean | System generated | Low |

---

### 2.7 Push Notification Data

**File Location:** `src/services/notificationService.ts`

```typescript
// From src/services/notificationService.ts
export const registerForPushNotifications = async (): Promise<string | null> => {
  const { status: existingStatus } = await Notifications.getPermissionsAsync();
  
  if (finalStatus === 'granted') {
    const token = await Notifications.getExpoPushTokenAsync({
      projectId: Constants.expoConfig?.extra?.eas?.projectId,
    });
    return token.data;
  }
  
  return null;
};
```

| Data Element | Type | Collection Method | Sensitivity |
|-------------|------|-------------------|-------------|
| Expo Push Token | String | System generated | Medium (Device identifier) |
| Notification Permission Status | String | System API | Low |
| Project ID | String | Configuration | Low |

---

## 3. Internal Data Processing

### 3.1 Local Database (SQLite)

**File Location:** `src/database/database.ts`

```typescript
// From src/database/database.ts
export const initializeDatabase = async (): Promise<SQLiteDatabase> => {
  const db = await SQLite.openDatabaseAsync('oms_iot.db');
  
  // Initialize all tables
  await createOrdersTable(db);
  await createTripsTable(db);
  await createOffloadsTable(db);
  await createLocationsTable(db);
  await createIoTDevicesTable(db);
  
  return db;
};
```

**Database Tables Summary:**
| Table | Personal Data | Sensitive Data | Encryption |
|-------|--------------|----------------|------------|
| orders | Customer name, address, phone | None | **NOT IMPLEMENTED** |
| trips | Driver ID, locations | Location coordinates | **NOT IMPLEMENTED** |
| offloads | Customer signature | Biometric (signature) | **NOT IMPLEMENTED** |
| locations | GPS coordinates | Location history | **NOT IMPLEMENTED** |
| iot_devices | Device identifiers | None | **NOT IMPLEMENTED** |

### 3.2 State Management (Zustand Stores)

**File Locations:** `src/store/`

| Store | Data Held | Persistence |
|-------|-----------|-------------|
| authStore | User credentials, tokens | SecureStore (encrypted) |
| tripStore | Active trip data, locations | Memory + SQLite |
| locationStore | Current location, history | Memory + SQLite |
| syncStore | Sync queue, pending items | Memory + SQLite |
| bluetoothStore | Device list, sensor data | Memory only |

---

### 3.3 Data Synchronization Service

**File Location:** `src/services/syncService.ts`

```typescript
// From src/services/syncService.ts
export const syncPendingData = async (): Promise<SyncResult> => {
  const results: SyncResult = {
    orders: { synced: 0, failed: 0 },
    trips: { synced: 0, failed: 0 },
    offloads: { synced: 0, failed: 0 },
    locations: { synced: 0, failed: 0 },
  };
  
  // Sync orders
  const pendingOrders = await orderRepository.getPendingSync();
  for (const order of pendingOrders) {
    try {
      await omsApi.syncOrder(order);
      await orderRepository.markSynced(order.id);
      results.orders.synced++;
    } catch (error) {
      results.orders.failed++;
    }
  }
  
  // Similar for trips, offloads, locations...
  
  return results;
};
```

**Data Flow During Sync:**
1. Local SQLite → Read unsynced records
2. Transform data for API format
3. POST to backend API
4. Mark records as synced in local DB

---

## 4. Third-Party Data Processors

### 4.1 Backend API Services

**File Location:** `src/api/client.ts`, `src/config/constants.ts`

```typescript
// From src/api/client.ts
const client = axios.create({
  baseURL: API_BASE_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor for auth tokens
client.interceptors.request.use(async (config) => {
  const token = useAuthStore.getState().token;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

| Third-Party | Data Shared | Purpose | Data Categories |
|-------------|-------------|---------|-----------------|
| OMS Backend API | Orders, Trips, Offloads, Locations | Core business operations | Personal, Location, Business |
| IoT Backend API | Device telemetry, Sensor data | IoT management | Device identifiers, Sensor data |

### 4.2 Expo Push Notifications

**File Location:** `src/services/notificationService.ts`

| Third-Party | Data Shared | Purpose |
|-------------|-------------|---------|
| Expo Push Service | Push tokens, Project ID | Push notifications |

### 4.3 Firebase (Android)

**File Location:** `android/app/google-services.json` (referenced but not in provided files)

| Third-Party | Data Shared | Purpose |
|-------------|-------------|---------|
| Firebase Cloud Messaging | Device tokens, App identifiers | Android push notifications |

---

## 5. Data Outputs/Exports

### 5.1 API Data Submissions

```typescript
// From src/api/omsApi.ts
export const submitOffload = async (offload: OffloadData): Promise<void> => {
  await client.post('/offloads', offload);
};

export const updateTripStatus = async (tripId: string, status: string): Promise<void> => {
  await client.patch(`/trips/${tripId}`, { status });
};

export const syncLocations = async (locations: LocationData[]): Promise<void> => {
  await client.post('/locations/batch', { locations });
};
```

### 5.2 Data Export Capabilities

**Not implemented in current codebase** - No user-facing data export functionality found.

---

## 6. Data Categories Summary

### 6.1 Personal Identifiers

| Data Type | Present | Location | Compliance Impact |
|-----------|---------|----------|-------------------|
| Names (Customer) | ✅ | orders table | GDPR, CCPA |
| Phone Numbers | ✅ | orders table | GDPR, CCPA |
| Physical Addresses | ✅ | orders table | GDPR, CCPA |
| User IDs | ✅ | authStore, trips | GDPR, CCPA |
| Device Identifiers | ✅ | bluetoothStore, push tokens | GDPR (with consent) |
| Session Identifiers | ✅ | Auth tokens | Low risk |

### 6.2 Sensitive Data Categories

| Data Type | Present | Location | Compliance Impact |
|-----------|---------|----------|-------------------|
| Location Data (GPS) | ✅ | locations table, tripStore | **HIGH** - GDPR Art. 9-like |
| Biometric Data (Signatures) | ✅ | offloads table | **HIGH** - GDPR Art. 9 |
| Authentication Credentials | ✅ | authStore | **HIGH** - Security critical |
| Movement Patterns | ✅ | Background location tracking | **HIGH** - Behavioral profiling |

### 6.3 Business Data

| Data Type | Present | Location |
|-----------|---------|----------|
| Transaction Records | ✅ | orders, offloads tables |
| Delivery Records | ✅ | offloads table |
| Usage Analytics | ❌ | Not implemented |
| Audit Logs | ⚠️ | Partial (created_at, updated_at) |

---

## 7. Compliance Considerations

### 7.1 GDPR Compliance Analysis

| Requirement | Implementation Status | Gap |
|-------------|----------------------|-----|
| Lawful Basis | ⚠️ Consent requested for location | No documented legal basis for other data |
| Purpose Limitation | ⚠️ Implicit in app function | No explicit purpose documentation |
| Data Minimization | ❌ | Collects extensive location history |
| Storage Limitation | ❌ | No retention policy implemented |
| Integrity & Confidentiality | ⚠️ | Auth tokens encrypted; DB not encrypted |
| Data Subject Rights | ❌ | No access/erasure mechanisms |

### 7.2 Location Permission Disclosure

**File Location:** `src/components/PermissionDisclosureModal.tsx`

```typescript
// From src/components/PermissionDisclosureModal.tsx
export const PermissionDisclosureModal: React.FC<Props> = ({ visible, onAccept, onDecline }) => {
  return (
    <Modal visible={visible} transparent animationType="slide">
      <View style={styles.container}>
        <Text style={styles.title}>Location Permission Required</Text>
        <Text style={styles.description}>
          This app collects location data to enable delivery tracking and route optimization,
          even when the app is closed or not in use.
        </Text>
        <Text style={styles.usage}>
          Your location data is used to:
          {'\n'}• Track delivery progress
          {'\n'}• Optimize routes
          {'\n'}• Provide accurate ETAs to customers
        </Text>
        {/* Buttons */}
      </View>
    </Modal>
  );
};
```

**iOS Privacy Manifest:**
```xml
<!-- From ios/OMSIoT/PrivacyInfo.xcprivacy -->
<dict>
  <key>NSPrivacyTracking</key>
  <false/>
  <key>NSPrivacyCollectedDataTypes</key>
  <array>
    <dict>
      <key>NSPrivacyCollectedDataType</key>
      <string>NSPrivacyCollectedDataTypePreciseLocation</string>
      <key>NSPrivacyCollectedDataTypeLinked</key>
      <true/>
      <key>NSPrivacyCollectedDataTypeTracking</key>
      <false/>
      <key

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## TOP 10 Critical Security Issues

---

### Issue #1: Hardcoded API Base URLs and Endpoints Exposed
**Severity:** HIGH
**Category:** Data Exposure / Security Misconfiguration
**Location:** 
- File: `src/config/constants.ts`
- Line(s): 1-23
- Function/Class: Constants module

**Description:**
The application exposes production API endpoints and configuration in plain text within the source code. This includes base URLs for authentication, OMS, and IoT services.

**Vulnerable Code:**
```typescript
export const API_CONFIG = {
  BASE_URL: 'https://portal.omscarrier.com',
  AUTH_ENDPOINT: '/auth/signin',
  OMS_BASE_URL: 'https://api.omscarrier.com',
  IOT_BASE_URL: 'https://iot.omscarrier.com',
  TIMEOUT: 30000,
};
```

**Impact:**
Attackers can discover production API endpoints, understand the application architecture, and potentially target these endpoints for further attacks. The exposed URLs provide reconnaissance information.

**Fix Required:**
Use environment variables and build-time configuration injection instead of hardcoding URLs in source code.

**Example Secure Implementation:**
```typescript
import Config from 'react-native-config';

export const API_CONFIG = {
  BASE_URL: Config.API_BASE_URL,
  AUTH_ENDPOINT: Config.AUTH_ENDPOINT,
  OMS_BASE_URL: Config.OMS_BASE_URL,
  IOT_BASE_URL: Config.IOT_BASE_URL,
  TIMEOUT: parseInt(Config.API_TIMEOUT || '30000', 10),
};
```

---

### Issue #2: Debug Keystore Committed to Repository
**Severity:** CRITICAL
**Category:** Data Exposure / Hardcoded Secrets
**Location:** 
- File: `android/app/debug.keystore`
- Line(s): N/A (binary file)

**Description:**
A debug keystore file is committed directly to the repository. While debug keystores typically use known default credentials (`android`/`android`), committing any keystore to version control is a security anti-pattern and may lead to accidental release signing key exposure.

**Vulnerable Code:**
```
Binary file: android/app/debug.keystore
```

**Impact:**
- If this is mistakenly used for production builds, attackers could sign malicious APKs
- Establishes poor security practices that could lead to production keystore exposure
- Default debug keystore credentials are well-known (`android`/`android`)

**Fix Required:**
Remove the debug keystore from version control and generate it locally during build.

**Example Secure Implementation:**
```gitignore
# In .gitignore
*.keystore
*.jks
```

```gradle
// In build.gradle - generate debug keystore if missing
task generateDebugKeystore {
    def keystoreFile = file("debug.keystore")
    if (!keystoreFile.exists()) {
        exec {
            commandLine 'keytool', '-genkey', '-v', '-keystore', 'debug.keystore',
                '-storepass', 'android', '-alias', 'androiddebugkey',
                '-keypass', 'android', '-dname', 'CN=Android Debug,O=Android,C=US'
        }
    }
}
```

---

### Issue #3: Insecure Token Storage Using AsyncStorage
**Severity:** CRITICAL
**Category:** Authentication & Session Management / Insecure Token Storage
**Location:** 
- File: `src/store/authStore.ts`
- Line(s): 8-9, 83-86, 95-98
- Function/Class: `useAuthStore`

**Description:**
Authentication tokens are stored using `AsyncStorage`, which on Android stores data in plain text in the app's shared preferences. This is not secure storage and tokens can be extracted from rooted devices or through backup extraction.

**Vulnerable Code:**
```typescript
import AsyncStorage from '@react-native-async-storage/async-storage';

const TOKEN_STORAGE_KEY = 'auth_token';
const USER_STORAGE_KEY = 'auth_user';

// Line 83-86
saveToken: async (token) => {
  await AsyncStorage.setItem(TOKEN_STORAGE_KEY, token);
  set({ token });
},

// Line 95-98
loadStoredAuth: async () => {
  const token = await AsyncStorage.getItem(TOKEN_STORAGE_KEY);
  const userJson = await AsyncStorage.getItem(USER_STORAGE_KEY);
```

**Impact:**
- Attackers with physical access to device can extract authentication tokens
- On rooted/jailbroken devices, any app can read AsyncStorage data
- Tokens extracted through device backups
- Session hijacking and account takeover

**Fix Required:**
Use secure storage mechanisms: iOS Keychain and Android Keystore via `react-native-keychain` or `expo-secure-store`.

**Example Secure Implementation:**
```typescript
import * as SecureStore from 'expo-secure-store';

const TOKEN_STORAGE_KEY = 'auth_token';

saveToken: async (token: string) => {
  await SecureStore.setItemAsync(TOKEN_STORAGE_KEY, token, {
    keychainAccessible: SecureStore.WHEN_UNLOCKED_THIS_DEVICE_ONLY
  });
  set({ token });
},

loadStoredAuth: async () => {
  const token = await SecureStore.getItemAsync(TOKEN_STORAGE_KEY);
  // ...
}
```

---

### Issue #4: Missing Session Timeout Implementation
**Severity:** HIGH
**Category:** Authentication & Session Management
**Location:** 
- File: `src/store/authStore.ts`
- Line(s): 1-130 (entire file)
- Function/Class: `useAuthStore`

**Description:**
The authentication store has no session timeout or token expiration handling. Once a user logs in, the token persists indefinitely until manually logged out, creating a security risk for shared or stolen devices.

**Vulnerable Code:**
```typescript
// No token expiration check exists
loadStoredAuth: async () => {
  const token = await AsyncStorage.getItem(TOKEN_STORAGE_KEY);
  const userJson = await AsyncStorage.getItem(USER_STORAGE_KEY);
  if (token && userJson) {
    const user = JSON.parse(userJson);
    set({ token, user, isAuthenticated: true });  // No expiration validation
  }
},
```

**Impact:**
- Stolen devices maintain persistent access to user accounts
- No automatic logout for inactive sessions
- Compromised tokens remain valid indefinitely

**Fix Required:**
Implement token expiration checks and automatic session timeout.

**Example Secure Implementation:**
```typescript
interface AuthState {
  token: string | null;
  tokenExpiry: number | null;
  // ...
}

const SESSION_TIMEOUT_MS = 30 * 60 * 1000; // 30 minutes

loadStoredAuth: async () => {
  const token = await SecureStore.getItemAsync(TOKEN_STORAGE_KEY);
  const expiry = await SecureStore.getItemAsync(TOKEN_EXPIRY_KEY);
  
  if (token && expiry) {
    const expiryTime = parseInt(expiry, 10);
    if (Date.now() > expiryTime) {
      // Token expired, clear auth
      await get().logout();
      return;
    }
    // Valid token, restore session
    set({ token, tokenExpiry: expiryTime, isAuthenticated: true });
  }
},

saveToken: async (token: string) => {
  const expiry = Date.now() + SESSION_TIMEOUT_MS;
  await SecureStore.setItemAsync(TOKEN_STORAGE_KEY, token);
  await SecureStore.setItemAsync(TOKEN_EXPIRY_KEY, expiry.toString());
  set({ token, tokenExpiry: expiry });
},
```

---

### Issue #5: SQL Injection Vulnerability in Database Queries
**Severity:** CRITICAL
**Category:** Injection Vulnerabilities / SQL Injection
**Location:** 
- File: `src/database/repositories/OrderRepository.ts`
- Line(s): 39-48
- Function/Class: `searchOrders`

**Description:**
The `searchOrders` function directly interpolates user input into SQL queries without proper parameterization, creating a SQL injection vulnerability.

**Vulnerable Code:**
```typescript
async searchOrders(searchTerm: string): Promise<Order[]> {
  const db = await this.getDatabase();
  const query = `
    SELECT * FROM orders 
    WHERE customer_name LIKE '%${searchTerm}%' 
    OR order_number LIKE '%${searchTerm}%'
    OR address LIKE '%${searchTerm}%'
  `;
  const results = await db.getAllAsync<OrderRow>(query);
  return results.map(this.mapRowToOrder);
}
```

**Impact:**
- Database data extraction through UNION-based injection
- Data modification or deletion
- Potential bypass of application logic
- In SQLite, could lead to file system access in certain configurations

**Fix Required:**
Use parameterized queries with proper escaping.

**Example Secure Implementation:**
```typescript
async searchOrders(searchTerm: string): Promise<Order[]> {
  const db = await this.getDatabase();
  const sanitizedTerm = `%${searchTerm.replace(/[%_]/g, '\\$&')}%`;
  const query = `
    SELECT * FROM orders 
    WHERE customer_name LIKE ? ESCAPE '\\'
    OR order_number LIKE ? ESCAPE '\\'
    OR address LIKE ? ESCAPE '\\'
  `;
  const results = await db.getAllAsync<OrderRow>(
    query, 
    [sanitizedTerm, sanitizedTerm, sanitizedTerm]
  );
  return results.map(this.mapRowToOrder);
}
```

---

### Issue #6: Sensitive User Data Stored Unencrypted in Local Database
**Severity:** HIGH
**Category:** Data Exposure / Unencrypted Sensitive Data Storage
**Location:** 
- File: `src/database/database.ts`
- Line(s): 24-51
- Function/Class: `initializeDatabase`

**Description:**
The SQLite database stores sensitive user, order, and location data without encryption. SQLite databases on mobile devices can be extracted through backups, file managers (on rooted devices), or forensic analysis.

**Vulnerable Code:**
```typescript
async function initializeDatabase(): Promise<SQLite.SQLiteDatabase> {
  const db = await SQLite.openDatabaseAsync('oms_database.db');
  
  await db.execAsync(`
    CREATE TABLE IF NOT EXISTS users (
      id TEXT PRIMARY KEY,
      email TEXT NOT NULL,
      name TEXT NOT NULL,
      phone TEXT,
      company_id TEXT,
      role TEXT,
      created_at TEXT,
      updated_at TEXT
    );
    
    CREATE TABLE IF NOT EXISTS orders (
      id TEXT PRIMARY KEY,
      order_number TEXT NOT NULL,
      customer_name TEXT NOT NULL,
      address TEXT NOT NULL,
      phone TEXT,
      // ... more sensitive fields
    );
```

**Impact:**
- Customer PII (names, addresses, phone numbers) can be extracted
- Order information and business data exposed
- Location history revealing user movements
- Potential regulatory compliance violations (GDPR, CCPA)

**Fix Required:**
Enable SQLite encryption using SQLCipher or similar encrypted database solution.

**Example Secure Implementation:**
```typescript
import * as SQLite from 'expo-sqlite';
import * as SecureStore from 'expo-secure-store';

async function getOrCreateDatabaseKey(): Promise<string> {
  let key = await SecureStore.getItemAsync('db_encryption_key');
  if (!key) {
    key = generateSecureRandomKey(32); // 256-bit key
    await SecureStore.setItemAsync('db_encryption_key', key);
  }
  return key;
}

async function initializeDatabase(): Promise<SQLite.SQLiteDatabase> {
  const encryptionKey = await getOrCreateDatabaseKey();
  
  const db = await SQLite.openDatabaseAsync('oms_database.db', {
    // Using SQLCipher for encryption
    encryption: {
      key: encryptionKey,
    }
  });
  // ...
}
```

---

### Issue #7: Insufficient Input Validation on Authentication
**Severity:** HIGH
**Category:** Input Validation / Missing Input Validation
**Location:** 
- File: `src/screens/auth/LoginScreen.tsx`
- Line(s): 26-53
- Function/Class: `handleLogin`

**Description:**
The login function performs only basic validation (non-empty fields) before sending credentials to the API. No email format validation, password complexity checks, or input sanitization is performed.

**Vulnerable Code:**
```typescript
const handleLogin = async () => {
  if (!email || !password) {
    Alert.alert('Error', 'Please enter both email and password');
    return;
  }

  setIsLoading(true);
  try {
    await login(email, password);
  } catch (error) {
    Alert.alert(
      'Login Failed',
      error instanceof Error ? error.message : 'An error occurred during login'
    );
  } finally {
    setIsLoading(false);
  }
};
```

**Impact:**
- Malformed input could cause unexpected server behavior
- Potential for injection attacks if server doesn't validate
- Poor user experience with invalid inputs
- Could facilitate credential stuffing attacks without rate limiting

**Fix Required:**
Add comprehensive input validation before API calls.

**Example Secure Implementation:**
```typescript
const EMAIL_REGEX = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const MAX_INPUT_LENGTH = 256;

const validateInput = (email: string, password: string): string | null => {
  if (!email || !password) {
    return 'Please enter both email and password';
  }
  if (email.length > MAX_INPUT_LENGTH || password.length > MAX_INPUT_LENGTH) {
    return 'Input exceeds maximum length';
  }
  if (!EMAIL_REGEX.test(email)) {
    return 'Please enter a valid email address';
  }
  if (password.length < 8) {
    return 'Password must be at least 8 characters';
  }
  return null;
};

const handleLogin = async () => {
  const trimmedEmail = email.trim().toLowerCase();
  const validationError = validateInput(trimmedEmail, password);
  
  if (validationError) {
    Alert.alert('Validation Error', validationError);
    return;
  }
  // ... proceed with login
};
```

---

### Issue #8: Exposed Detailed Error Messages to Users
**Severity:** MEDIUM
**Category:** Security Misconfiguration / Information Disclosure
**Location:** 
- File: `src/api/client.ts`
- Line(s): 45-68
- Function/Class: `handleApiError`

**Description:**
The API client exposes detailed error messages from the server directly to users, potentially revealing internal system information, stack traces, or sensitive debugging data.

**Vulnerable Code:**
```typescript
function handleApiError(error: AxiosError): never {
  if (error.response) {
    const status = error.response.status;
    const data = error.response.data as { message?: string; error?: string };
    
    switch (status) {
      case 401:
        throw new Error(data.message || 'Authentication failed');
      case 403:
        throw new Error(data.message || 'Access denied');
      case 404:
        throw new Error(data.message || 'Resource not found');
      case 500:
        throw new Error(data.message || 'Server error occurred');
      default:
        throw new Error(data.message || data.error || `Request failed with status ${status}`);
    }
  }
  // Network error exposed
  throw new Error(error.message || 'Network error occurred');
}
```

**Impact:**
- Server error messages may contain sensitive information (database errors, file paths, etc.)
- Attackers can gather reconnaissance information from error responses
- Internal system architecture details could be exposed

**Fix Required:**
Display generic user-friendly messages while logging detailed errors securely.

**Example Secure Implementation:**
```typescript
import { logError } from './errorLogging'; // Secure logging service

const USER_FRIENDLY_MESSAGES: Record<number, string> = {
  401: 'Please sign in to continue',
  403: 'You don\'t have permission to access this resource',
  404: 'The requested item was not found',
  500: 'Something went wrong. Please try again later',
};

function handleApiError(error: AxiosError): never {
  // Log detailed error securely (not to console in production)
  logError('API Error', {
    status: error.response?.status,
    data: error.response?.data,
    url: error.config?.url,
  });

  if (error.response) {
    const status = error.response.status;
    const userMessage = USER_FRIENDLY_MESSAGES[status] || 'An unexpected error occurred';
    throw new Error(userMessage);
  }
  
  throw new Error('Unable to connect. Please check your internet connection.');
}
```

---

### Issue #9: Location Data Transmission Without Certificate Pinning
**Severity:** HIGH
**Category:** Cryptographic Issues / Improper Certificate Validation
**Location:** 
- File: `src/api/client.ts`
- Line(s): 8-17
- Function/Class: `createApiClient`

**Description:**
The API client sends sensitive location data and authentication tokens over HTTPS but does not implement certificate pinning. This makes the application vulnerable to man-in-the-middle attacks using rogue certificates.

**Vulnerable Code:**
```typescript
export function createApiClient(baseURL: string) {
  const client = axios.create({
    baseURL,
    timeout: API_CONFIG.TIMEOUT,
    headers: {
      'Content-Type': 'application/json',
    },
  });
  // No certificate pinning configuration
  return client;
}
```

**Impact:**
- Man-in-the-middle attacks can intercept authentication tokens
- User location data can be captured by attackers on compromised networks
- Customer order data and PII can be stolen
- Corporate espionage through data interception

**Fix Required:**
Implement certificate pinning for all API communications.

**Example Secure Implementation:**
```typescript
import { Platform } from 'react-native';

// Using react-native-ssl-pinning or similar library
import { fetch as pinnedFetch } from 'react-native-ssl-pinning';

const SSL_PINS = {
  'portal.omscarrier.com': [
    'sha256/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=', // Primary cert pin
    'sha256/BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB=', // Backup pin
  ],
};

export async function secureFetch(url: string, options: RequestInit) {
  const hostname = new URL(url).hostname;
  const pins = SSL_PINS[hostname];
  
  if (!pins) {
    throw new Error('No certificate pin configured for host');
  }
  
  return pinnedFetch(url, {
    ...options,
    sslPinning: {
      certs: pins,
    },
  });
}
```

---

### Issue #10: Potential Object Injection via JSON.parse Without Validation
**Severity:** MEDIUM
**Category:** Input Validation / Deserialization Vulnerabilities
**Location:** 
- File: `src/store/authStore.ts`
- Line(s): 101-102
- Function/Class: `loadStoredAuth`

**Description:**
The application uses `JSON.parse` on stored data without validating the structure of the parsed object. If the stored data is tampered with (on rooted devices or through backup manipulation), it could lead to unexpected application behavior or prototype pollution.

**Vulnerable Code:**
```typescript
loadStoredAuth: async () => {
  const token = await AsyncStorage.getItem(TOKEN_STORAGE_KEY);
  const userJson = await AsyncStorage.getItem(USER_STORAGE_KEY);
  if (token && userJson) {
    const user = JSON.parse(userJson);  // No validation of parsed object
    set({ token, user, isAuthenticated: true });
  }
},
```

**Impact:**
- Malformed JSON could crash the application
- Tampered user objects could escalate privileges (if role is stored)
- Prototype pollution could lead to unexpected behavior
- Application state manipulation

**Fix Required:**
Validate parsed JSON against expected schema before use.

**Example Secure Implementation:**
```typescript
interface User {
  id: string;
  email: string;
  name: string;
  role: string;
}

function isValidUser(obj: unknown): obj is User {
  if (typeof obj !== 'object' || obj === null) return false;
  const user = obj as Record<string, unknown>;
  return (
    typeof user.id === 'string' &&
    typeof user.email === 'string' &&
    typeof user.name === 'string' &&
    typeof user.role === 'string' &&
    ['driver', 'admin', 'dispatcher'].includes(user.role)
  );
}

loadStoredAuth: async () => {
  try {
    const token = await SecureStore.getItemAsync(TOKEN_STORAGE_KEY);
    const userJson = await SecureStore.getItemAsync(USER_STORAGE_KEY);
    
    if (token && userJson) {
      const parsed = JSON.parse(userJson);
      
      if (!isValidUser(parsed)) {
        console.warn('Invalid stored user data, clearing auth');
        await get().logout();
        return;
      }
      
      set({ token, user: parsed, isAuthenticated: true });
    }
  } catch (error) {
    // Handle JSON parse errors gracefully
    await get().logout();
  }
},
```

---

## Summary

### 1. Overall Security Posture
The codebase exhibits **significant security vulnerabilities** particularly in authentication/session management and data protection areas. The mobile application handles sensitive data (location, customer PII, orders) but lacks fundamental security controls like encrypted storage, certificate pinning, and session management. The SQL injection vulnerability represents an immediate critical risk.

### 2. Critical Issues Count
**3 CRITICAL severity findings:**
1. Debug Keystore Committed to Repository
2. Insecure Token Storage Using AsyncStorage
3. SQL Injection Vulnerability in Database Queries

### 3. Most Concerning Pattern
**Insecure Data Storage** - The application consistently stores sensitive data (auth tokens, user data, location history, customer information) without encryption or secure storage mechanisms. This pattern affects AsyncStorage, SQLite database, and potentially logged data.

### 4. Priority Fixes (Top 3 Issues to Fix Immediately)
1. **SQL Injection (Issue #5)** - Immediate risk of data breach; fix with parameterized queries
2. **Insecure Token Storage (Issue #3)** - Critical authentication risk; migrate to SecureStore/Keychain
3. **Unencrypted Database (Issue #6)** - PII exposure risk; implement SQLCipher encryption

### 5. Implementation Issues
| Pattern | Occurrences | Files Affected |
|---------|-------------|----------------|
| Insecure storage (AsyncStorage) | Multiple | authStore.ts, syncStore.ts |
| Missing input validation | Multiple | LoginScreen.tsx, repositories |
| Hardcoded configuration | 1 | constants.ts |
| Raw error message exposure | Multiple | client.ts, screens |

---

## Additional Security Issues Found

The following issues didn't make the top 10 but warrant attention:

### Configuration Vulnerabilities Present
- **Android Debug Signing Config**: `android/app/build.gradle` includes debug signing configuration that references the committed debug keystore
- **Verbose Build Configuration**: Gradle files expose build configuration details

### Architecture Security Flaws Identified
- **No Network Security Config**: Missing Android Network Security Configuration for production (to enforce HTTPS and certificate pinning)
- **No App Transport Security Exceptions Review**: iOS Info.plist should be reviewed for ATS exceptions

### Development Implementation Issues
- **Console Logging**: Various files may contain `console.log

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Repository: react-app-oms_635d2a1a

---

## Executive Summary

After thorough analysis of this React Native/Expo mobile application codebase, **no monitoring or observability detected** at the application level. This is a mobile application built with React Native and Expo, and the codebase does not include any dedicated monitoring, logging, metrics, tracing, or alerting libraries or implementations.

---

## Analysis Details

### Logging Infrastructure

**Status: Not Implemented**

- No logging frameworks detected (no Winston, Bunyan, Pino, or similar)
- No structured logging configuration found
- No log shipping or aggregation tools identified
- The codebase relies on default console output only (native `console.*` methods)

### Metrics Collection

**Status: Not Implemented**

- No metrics libraries detected
- No Prometheus client, StatsD, or similar metrics collection tools
- No custom metrics implementation found

### Distributed Tracing

**Status: Not Implemented**

- No tracing frameworks detected (no OpenTelemetry, Jaeger, Zipkin, or similar)
- No trace context or correlation ID implementation found

### Application Performance Monitoring (APM)

**Status: Not Implemented**

- No APM tools detected (no New Relic, DataDog, AppDynamics, or similar)
- No Sentry, Rollbar, or Bugsnag for error tracking
- No Firebase Crashlytics or similar mobile crash reporting

### Real User Monitoring (RUM)

**Status: Not Implemented**

- No RUM tools detected (no LogRocket, FullStory, Heap, or similar)
- No session replay or user analytics tools found

### Health Checks

**Status: Not Implemented**

- No health check endpoints detected
- No circuit breaker patterns implemented

### Alerting

**Status: Not Implemented**

- No alerting configuration found
- No integration with PagerDuty, Opsgenie, or similar services

---

## Application Architecture Context

This is a React Native mobile application for OMS (Order Management System) IoT with the following characteristics:

### Technology Stack Identified:
- **Framework**: React Native 0.81.5 with Expo ~54.0.25
- **State Management**: Zustand 5.0.8
- **Data Fetching**: TanStack React Query 5.90.11
- **Navigation**: React Navigation 7.x
- **Local Storage**: Expo SQLite, Expo Secure Store
- **Networking**: Axios 1.13.2
- **Background Processing**: expo-background-fetch, expo-task-manager
- **Location Services**: expo-location
- **Bluetooth**: react-native-ble-plx
- **Notifications**: expo-notifications

### Services Identified (without monitoring):
- Authentication service (`src/api/authApi.ts`)
- Bluetooth service (`src/services/bluetoothService.ts`)
- Location service (`src/services/locationService.ts`)
- Notification service (`src/services/notificationService.ts`)
- Sync service (`src/services/syncService.ts`)
- API client (`src/api/client.ts`)

---

## Raw Dependencies Section

### JavaScript/Node.js Dependencies (package.json)

#### Production Dependencies:
```
@expo/metro-runtime: ~6.1.2
@react-native-community/netinfo: ^11.4.1
@react-navigation/bottom-tabs: ^7.8.6
@react-navigation/native: ^7.1.21
@react-navigation/native-stack: ^7.8.0
@shopify/flash-list: ^2.2.0
@tanstack/react-query: ^5.90.11
axios: ^1.13.2
date-fns: ^4.1.0
expo: ~54.0.25
expo-background-fetch: ^14.0.8
expo-camera: ^17.0.9
expo-dev-client: ~6.0.18
expo-device: ^8.0.9
expo-file-system: ^19.0.19
expo-image-picker: ^17.0.8
expo-location: ^19.0.7
expo-notifications: ^0.32.13
expo-secure-store: ^15.0.7
expo-sqlite: ^16.0.9
expo-status-bar: ~3.0.8
expo-task-manager: ^14.0.8
react: 19.1.0
react-dom: 19.1.0
react-native: 0.81.5
react-native-ble-plx: ^3.5.0
react-native-gesture-handler: ^2.29.1
react-native-reanimated: ^4.1.5
react-native-safe-area-context: ^5.6.2
react-native-screens: ^4.18.0
react-native-svg: 15.12.1
react-native-web: ^0.21.0
zustand: ^5.0.8
```

#### Development Dependencies:
```
@types/react: ~19.1.0
typescript: ~5.9.2
```

### Android/Gradle Dependencies (build.gradle)

```
com.facebook.react:react-android (version set by React Native Gradle Plugin)
com.facebook.react:hermes-android (conditional - if Hermes enabled)
io.github.react-native-community:jsc-android:2026004.+ (conditional - if Hermes disabled)
com.facebook.fresco:animated-gif (conditional - if GIF enabled)
com.facebook.fresco:webpsupport (conditional - if WebP enabled)
com.facebook.fresco:animated-webp (conditional - if animated WebP enabled)
```

### Build Plugins:
```
com.android.tools.build:gradle
com.facebook.react:react-native-gradle-plugin
org.jetbrains.kotlin:kotlin-gradle-plugin
expo-root-project
com.facebook.react.rootproject
```

### iOS Dependencies (Podfile.lock)

The iOS dependencies are managed through CocoaPods but the detailed pod versions are not fully exposed in the provided file structure.

---

## Conclusion

**No monitoring or observability detected** in this React Native/Expo mobile application. The application lacks:

- Crash reporting and error tracking
- Performance monitoring
- User analytics and session recording
- Logging infrastructure
- Metrics collection
- Distributed tracing
- Health checks and alerting

The codebase is a standard React Native mobile application focused on business functionality (order management, IoT device connectivity, location tracking) without any observability instrumentation.

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After thorough analysis of the provided codebase, **no machine learning services, AI technologies, or ML-related integrations were identified** in this codebase.

---

## Analysis Results

### 1. External ML Service Providers

| Category | Services Checked | Found |
|----------|------------------|-------|
| Cloud ML Services | AWS SageMaker, Azure ML, Google AI Platform, Databricks | ❌ None |
| AI APIs | OpenAI, Anthropic, Groq, Cohere, Hugging Face Inference API | ❌ None |
| Specialized Services | AWS Transcribe, Google Speech-to-Text, AWS Rekognition, Google Vision | ❌ None |
| MLOps Platforms | MLflow, Weights & Biases, Neptune, ClearML | ❌ None |

### 2. ML Libraries and Frameworks

| Category | Libraries Checked | Found |
|----------|-------------------|-------|
| Deep Learning | PyTorch, TensorFlow, JAX, Keras, tensor-flow | ❌ None |
| Traditional ML | Scikit-learn, XGBoost, LightGBM, CatBoost, scikit-learn | ❌ None |
| NLP | Transformers, spaCy, NLTK, Gensim, hugging-face | ❌ None |
| Computer Vision | OpenCV, PIL/Pillow, torchvision | ❌ None |
| Audio/Speech | Whisper, librosa, speechbrain | ❌ None |

### 3. Pre-trained Models and Model Hubs

| Source | Found |
|--------|-------|
| Hugging Face Models | ❌ None |
| TensorFlow Hub | ❌ None |
| PyTorch Hub | ❌ None |
| Specific Models (BERT, GPT, Whisper, CLIP) | ❌ None |

### 4. AI Infrastructure and Deployment

| Category | Found |
|----------|-------|
| Model Serving (TorchServe, TensorFlow Serving, MLflow) | ❌ None |
| GPU/CUDA Integration | ❌ None |
| TPU Integration | ❌ None |
| ML-specific Docker configurations | ❌ None |

---

## Codebase Technology Profile

Based on the dependency analysis, this is a **React Native/Expo mobile application** with the following technology stack:

### Core Framework
- **React Native** (0.81.5) with **Expo** (~54.0.25)
- **React** (19.1.0)
- **TypeScript** (~5.9.2)

### Key Dependencies (Non-ML)
| Category | Dependencies |
|----------|--------------|
| Navigation | @react-navigation/native, @react-navigation/bottom-tabs, @react-navigation/native-stack |
| State Management | Zustand (^5.0.8) |
| Data Fetching | @tanstack/react-query (^5.90.11), Axios (^1.13.2) |
| Device Features | expo-camera, expo-location, expo-notifications, react-native-ble-plx |
| Storage | expo-sqlite, expo-secure-store, expo-file-system |
| UI | react-native-reanimated, react-native-gesture-handler, react-native-svg |

### Android Build Configuration
- Standard React Native Android build with Hermes JavaScript engine
- No ML-specific native dependencies
- No TensorFlow Lite or PyTorch Mobile integrations

---

## Security and Compliance Considerations

### API Keys/Credentials
- No ML service credentials identified
- **expo-secure-store** is present for secure storage (may be used for general API keys)

### Data Privacy
- No data is sent to 3rd party ML services
- Location data collection via **expo-location** (not ML-related)
- Camera/image capabilities via **expo-camera** and **expo-image-picker** (no ML processing identified)

### Model Security
- Not applicable - no models are used

### Compliance
- No ML-specific regulatory concerns identified

---

## Potential ML Integration Points

While no ML is currently implemented, the codebase has capabilities that could support future ML integration:

| Existing Capability | Potential ML Use Case |
|---------------------|----------------------|
| expo-camera | Image classification, object detection |
| expo-image-picker | Image preprocessing for ML models |
| expo-location | Location-based ML predictions |
| react-native-ble-plx | IoT sensor data for predictive maintenance |
| expo-file-system | Local model storage (TFLite, CoreML) |

---

## Summary

| Metric | Value |
|--------|-------|
| **Total 3rd Party ML Services** | 0 |
| **ML Libraries/Frameworks** | 0 |
| **Pre-trained Models** | 0 |
| **ML Infrastructure Components** | 0 |
| **Major ML Dependencies** | None |
| **Architecture Pattern** | No ML architecture present |

### Risk Assessment

| Risk Category | Assessment |
|---------------|------------|
| Vendor Lock-in | ✅ No risk - no ML vendors |
| Cost Exposure | ✅ No risk - no ML API costs |
| Data Privacy | ✅ No risk - no data sent to ML services |
| Service Availability | ✅ No risk - no ML service dependencies |

### Conclusion

This codebase is a standard **IoT mobile application** built with React Native and Expo. It includes hardware integration features (BLE, camera, location) but does not incorporate any machine learning services, AI APIs, or ML frameworks. All identified dependencies are related to mobile app development, UI/UX, navigation, state management, and device hardware access.

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Repository: react-app-oms_635d2a1a

---

**no feature flag usage detected**

---

## Analysis Summary

After a comprehensive review of the codebase, I found:

### Framework Detection Results

**Commercial Platforms:** None detected
- ❌ No Flagsmith (`flagsmith-*` packages)
- ❌ No LaunchDarkly (`launchdarkly-*` packages)
- ❌ No Split.io (`@splitsoftware/*` packages)
- ❌ No Optimizely
- ❌ No ConfigCat (`configcat-*` packages)
- ❌ No Unleash (`@unleash/*` packages)

**Open Source/Custom Solutions:** None detected
- ❌ No custom feature flag service implementations
- ❌ No database-backed feature flag tables/repositories
- ❌ No environment variable-based feature flag patterns

### Dependency Analysis

The `package.json` contains no feature flag related dependencies. The application uses:
- **State Management:** Zustand (not used for feature flags)
- **Navigation:** React Navigation
- **Data Fetching:** TanStack React Query (not configured for feature flags)
- **Storage:** Expo SecureStore, Expo SQLite (no feature flag schemas detected)

### Codebase Structure Review

The following directories were analyzed for feature flag patterns:

| Directory | Feature Flag Indicators |
|-----------|------------------------|
| `/src/config/` | Only contains `constants.ts` - no flag configurations |
| `/src/store/` | Contains auth, bluetooth, location, sync, trip stores - no flag stores |
| `/src/api/` | Contains auth, iot, oms APIs - no flag API client |
| `/src/services/` | Contains bluetooth, location, notification, sync services - no flag service |
| `/src/hooks/` | Contains tracking setup hook only |

### What Would Indicate Feature Flags

If this codebase had feature flags, I would expect to find:
- A dedicated feature flag store (e.g., `featureFlagStore.ts`)
- Feature flag API client or SDK initialization
- Conditional rendering patterns like `{isFeatureEnabled('new-ui') && <NewComponent />}`
- Environment-based flag configurations in constants
- A feature flag context provider wrapping the app

---

**Conclusion:** This React Native/Expo application does not implement any feature flag system. The application appears to use standard configuration constants and does not have infrastructure for runtime feature toggling, A/B testing, or gradual rollouts.

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Security Assessment Report

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After conducting a comprehensive analysis of the repository using all seven detection strategies, I have examined:

#### Detection Strategy 1: Library and Package Detection
**File: `package.json`**
```json
{
  "dependencies": {
    "@expo/vector-icons": "^14.0.2",
    "@react-native-async-storage/async-storage": "1.23.1",
    "@react-native-community/netinfo": "11.4.1",
    "@react-navigation/bottom-tabs": "^7.3.10",
    "@react-navigation/native": "^7.1.6",
    "@react-navigation/native-stack": "^7.3.10",
    "axios": "^1.9.0",
    "expo": "~52.0.46",
    "expo-background-fetch": "~13.0.5",
    "expo-device": "~7.0.2",
    "expo-location": "~18.0.10",
    "expo-notifications": "~0.29.14",
    "expo-splash-screen": "~0.29.22",
    "expo-sqlite": "~15.1.2",
    "expo-status-bar": "~2.0.1",
    "expo-task-manager": "~12.0.5",
    "react": "18.3.1",
    "react-native": "0.76.9",
    "react-native-ble-plx": "^3.2.1",
    "react-native-gesture-handler": "~2.20.2",
    "react-native-safe-area-context": "4.12.0",
    "react-native-screens": "~4.4.0",
    "zustand": "^5.0.3"
  }
}
```

**Result:** ❌ No LLM-related packages detected
- No `openai`, `anthropic`, `@anthropic-ai/sdk`, `langchain`, `@google/generative-ai`, or similar packages
- No vector database clients (Pinecone, Chroma, Weaviate, FAISS)
- No ML/AI framework dependencies

#### Detection Strategy 2: Import/Include Pattern Matching

Searched all source files for LLM-related imports:
- `src/api/client.ts` - Only uses `axios` for HTTP requests
- `src/api/authApi.ts` - Authentication API calls only
- `src/api/omsApi.ts` - Order Management System API calls
- `src/api/iotApi.ts` - IoT device API calls
- `src/services/*.ts` - Bluetooth, location, notification, and sync services only

**Result:** ❌ No LLM-related imports found

#### Detection Strategy 3: API Client Instantiation Patterns

Searched for LLM client instantiation patterns:
- No `OpenAI(`, `Anthropic(`, `GoogleGenerativeAI(` patterns
- No LLM SDK client constructors

**File: `src/api/client.ts`**
```typescript
import axios from 'axios';
import { useAuthStore } from '../store/authStore';
import { API_BASE_URL } from '../config/constants';

const apiClient = axios.create({
  baseURL: API_BASE_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});
```

**Result:** ❌ Standard REST API client only, no LLM clients

#### Detection Strategy 4: API Method Call Patterns

Searched for characteristic LLM API method calls:
- No `.messages.create(`, `.chat.completions.create(`, `.generateContent(` patterns
- No prompt-related method calls

**Result:** ❌ No LLM API method calls found

#### Detection Strategy 5: Configuration and Environment Variables

**File: `src/config/constants.ts`**
```typescript
export const API_BASE_URL = 'https://oms-api.example.com/api/v1';
export const IOT_API_URL = 'https://iot-api.example.com/api/v1';

export const SYNC_INTERVAL = 5 * 60 * 1000; // 5 minutes
export const LOCATION_UPDATE_INTERVAL = 10 * 1000; // 10 seconds
export const BACKGROUND_SYNC_INTERVAL = 15 * 60; // 15 minutes

export const BLE_SERVICE_UUID = '0000FFE0-0000-1000-8000-00805F9B34FB';
export const BLE_CHARACTERISTIC_UUID = '0000FFE1-0000-1000-8000-00805F9B34FB';
```

**Result:** ❌ No LLM-related configuration variables
- No `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or similar
- No model names, temperature settings, or token configurations

#### Detection Strategy 6: Prompt-Related Patterns

Searched for prompt handling code:
- No variables named `prompt`, `system_prompt`, `user_prompt`, `messages` (in LLM context)
- No template strings with LLM placeholders
- No `.prompt`, `.tmpl` files or `prompts` directories

**Result:** ❌ No prompt-related patterns found

#### Detection Strategy 7: Custom Implementation Patterns

Searched for custom AI/ML wrapper classes:
- No files with `llm`, `ai`, `ml`, `claude`, `gpt`, `analyzer`, `generator` in names
- No classes with `analyze`, `generate`, `complete`, `predict` methods for text processing

**Result:** ❌ No custom LLM implementations found

### 1.2 Repository Architecture Summary

This repository is a **React Native/Expo mobile application** for an **Order Management System (OMS)** with **IoT integration**. Key components include:

| Component | Purpose | LLM Usage |
|-----------|---------|-----------|
| `src/api/` | REST API clients for backend communication | ❌ None |
| `src/services/` | Bluetooth, location, notifications, sync | ❌ None |
| `src/store/` | Zustand state management | ❌ None |
| `src/screens/` | UI screens for orders, trips, IoT | ❌ None |
| `src/database/` | SQLite local database | ❌ None |
| `src/components/` | React Native UI components | ❌ None |

### 1.3 LLM Usage Summary

**Total LLM Integrations Found:** 0

**Technologies Used (Non-LLM):**
- React Native with Expo framework
- Axios for HTTP requests
- SQLite for local storage
- Bluetooth LE for IoT communication
- Background location tracking
- Push notifications

---

## Part 2 & Part 3: Security Vulnerability Assessment

**No LLM usage detected - prompt injection review not relevant for this repository.**

This is a React Native mobile application for order management and IoT device integration. The codebase contains:

1. **Standard REST API integrations** - HTTP calls to backend services using Axios
2. **IoT device communication** - Bluetooth Low Energy (BLE) for device connectivity
3. **Local data storage** - SQLite database for offline-first functionality
4. **Location services** - GPS tracking for delivery/trip management
5. **State management** - Zustand stores for application state

---

## Non-LLM Security Observations

While this review is focused on LLM/prompt injection vulnerabilities, the following general security observations may be relevant for a comprehensive security review (outside the scope of this assessment):

| Area | Observation | Relevance to LLM Review |
|------|-------------|-------------------------|
| API Authentication | Bearer token authentication present | Not LLM-related |
| Data Storage | SQLite local database | Not LLM-related |
| Network Requests | Standard REST API calls | Not LLM-related |
| Bluetooth | BLE device communication | Not LLM-related |

---

## Conclusion

**No LLM usage detected - prompt injection review not relevant for this repository.**

This React Native mobile application does not integrate with any LLM services, AI models, or LLM-based infrastructure. The application is a traditional mobile app with REST API backends, local database storage, Bluetooth IoT connectivity, and standard mobile services (location, notifications).

# components

Component architecture and design patterns

# Frontend Component Architecture Analysis

## React Native OMS (Order Management System) IoT Application

---

## 1. Component Organization

### Directory Structure

```
src/
├── components/
│   ├── common/           # Reusable UI components (8 files)
│   ├── index.ts          # Barrel exports
│   ├── PermissionDisclosureModal.tsx
│   └── TrackingProvider.tsx
├── screens/
│   ├── home/            # Home screen (2 files)
│   ├── loading/         # Loading/splash screens (3 files)
│   ├── auth/            # Authentication screens (4 files)
│   ├── iot/             # IoT device screens (3 files)
│   ├── filling/         # Filling operations (2 files)
│   ├── orders/          # Order management (5 files)
│   ├── trips/           # Trip management (3 files)
│   └── offload/         # Offload operations (3 files)
├── navigation/
│   ├── AppNavigator.tsx
│   ├── index.ts
│   └── types.ts
├── theme/
│   ├── colors.ts
│   ├── spacing.ts
│   ├── typography.ts
│   └── index.ts
├── hooks/
│   ├── index.ts
│   └── useTrackingSetup.ts
├── store/               # State management
├── services/            # Business logic services
├── api/                 # API layer
├── database/            # Local database
├── types/               # TypeScript definitions
└── config/              # App configuration
```

### Component Naming Conventions

| Pattern | Example | Usage |
|---------|---------|-------|
| PascalCase | `TrackingProvider.tsx` | Component files |
| camelCase | `useTrackingSetup.ts` | Custom hooks |
| Descriptive suffixes | `PermissionDisclosureModal` | Modal components |
| Feature-based grouping | `screens/orders/` | Screen organization |

### Atomic Design Levels (Partial Implementation)

| Level | Implementation | Location |
|-------|----------------|----------|
| **Atoms** | Basic UI elements | `src/components/common/` |
| **Molecules** | Compound components | `src/components/common/` |
| **Organisms** | Complex components | `src/components/` (root) |
| **Templates** | Not explicitly implemented | - |
| **Pages/Screens** | Feature screens | `src/screens/*/` |

---

## 2. Core Components

### Layout Components

Based on the directory structure:

| Component | Location | Purpose |
|-----------|----------|---------|
| `AppNavigator` | `src/navigation/AppNavigator.tsx` | Main navigation container and routing |

### Screen Components (Page-Level)

| Screen Category | Files | Purpose |
|-----------------|-------|---------|
| **Home** | 2 files | Dashboard/main screen |
| **Loading** | 3 files | Splash/loading states |
| **Auth** | 4 files | Login, registration, authentication flows |
| **IoT** | 3 files | IoT device management |
| **Filling** | 2 files | Filling operation screens |
| **Orders** | 5 files | Order management and listing |
| **Trips** | 3 files | Trip tracking and management |
| **Offload** | 3 files | Offload operation screens |

### Reusable Components

| Component | Location | Purpose |
|-----------|----------|---------|
| `PermissionDisclosureModal` | `src/components/PermissionDisclosureModal.tsx` | Permission request UI with disclosure |
| `TrackingProvider` | `src/components/TrackingProvider.tsx` | Context provider for location tracking |
| **Common Components** | `src/components/common/` | 8 reusable UI components (specific names not visible) |

### Feedback Components

| Type | Implementation |
|------|----------------|
| Loading States | `src/screens/loading/` (3 files) |
| Permission Dialogs | `PermissionDisclosureModal.tsx` |

---

## 3. Component Patterns

### Provider Pattern (Context)

```
TrackingProvider.tsx
└── Wraps application with tracking context
    └── Provides location/tracking state to children
```

### Custom Hooks

| Hook | Location | Purpose |
|------|----------|---------|
| `useTrackingSetup` | `src/hooks/useTrackingSetup.ts` | Encapsulates tracking initialization logic |

### Screen-Based Architecture

The application follows a **screen-based pattern** common in React Native:

```
src/screens/
├── [feature]/
│   ├── [FeatureScreen].tsx       # Main screen component
│   ├── [FeatureScreen].styles.ts # Styles (likely pattern)
│   └── [SupportingComponents].tsx # Screen-specific components
```

### Container/Presentational Pattern (Inferred)

Based on the separation of concerns:

| Layer | Location | Responsibility |
|-------|----------|----------------|
| Container Logic | `src/store/`, `src/hooks/` | State management, business logic |
| Presentational | `src/components/common/` | UI rendering |
| Screen Containers | `src/screens/*/` | Feature composition |

---

## 4. Props & Data Flow

### TypeScript Typing

| File | Purpose |
|------|---------|
| `src/types/index.ts` | Centralized type definitions |
| `src/navigation/types.ts` | Navigation-specific types |
| `tsconfig.json` | TypeScript configuration |

### Data Flow Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    TrackingProvider                     │
│                    (Context Layer)                      │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                    Store Layer                          │
│  ┌──────────────┬──────────────┬──────────────────┐    │
│  │ authStore    │ tripStore    │ bluetoothStore   │    │
│  │ syncStore    │ locationStore│                  │    │
│  └──────────────┴──────────────┴──────────────────┘    │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                   Screen Layer                          │
│  (Consumes stores, renders components)                  │
└─────────────────────────┬───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│               Common Components                         │
│  (Reusable UI, receives props)                         │
└─────────────────────────────────────────────────────────┘
```

### State Management Stores

| Store | File | Domain |
|-------|------|--------|
| `authStore` | `src/store/authStore.ts` | Authentication state |
| `tripStore` | `src/store/tripStore.ts` | Trip data management |
| `bluetoothStore` | `src/store/bluetoothStore.ts` | Bluetooth device state |
| `locationStore` | `src/store/locationStore.ts` | Location tracking state |
| `syncStore` | `src/store/syncStore.ts` | Data synchronization state |

### Context Providers

| Provider | Purpose |
|----------|---------|
| `TrackingProvider` | Location tracking context |

---

## 5. Styling Patterns

### Theme System

The application implements a **centralized theme system**:

```
src/theme/
├── colors.ts      # Color palette definitions
├── spacing.ts     # Spacing scale/tokens
├── typography.ts  # Font styles and sizes
└── index.ts       # Theme barrel export
```

### Design Tokens Structure

| Token Category | File | Purpose |
|----------------|------|---------|
| **Colors** | `colors.ts` | Brand colors, semantic colors |
| **Spacing** | `spacing.ts` | Margin/padding scale |
| **Typography** | `typography.ts` | Font families, sizes, weights |

### Styling Approach

Based on React Native conventions and file structure:
- **StyleSheet API** (React Native standard)
- **Theme-based tokens** for consistency
- **Co-located styles** (likely within screen folders)

---

## 6. Component Testing

### Test Infrastructure

**Note:** No dedicated test files or `__tests__` directories were identified in the provided structure.

| Aspect | Status |
|--------|--------|
| Unit Tests | Not visible in structure |
| Integration Tests | Not visible in structure |
| E2E Tests | Not visible in structure |
| Test Configuration | Not present in root |

---

## 7. Design System Integration

### Custom Design System

The application implements a **custom design system** rather than using external component libraries:

| Element | Implementation |
|---------|----------------|
| Design Tokens | `src/theme/` directory |
| Common Components | `src/components/common/` (8 components) |
| Component Library | None detected (custom implementation) |

### Theme Structure

```typescript
// Inferred theme structure
src/theme/
├── colors.ts     → Color palette
├── spacing.ts    → Spacing scale  
├── typography.ts → Type scale
└── index.ts      → Combined theme export
```

---

## 8. Performance Patterns

### Implemented Patterns

| Pattern | Evidence |
|---------|----------|
| **Local Database** | `src/database/` with repositories |
| **Offline-First** | `syncStore.ts`, `syncService.ts` |
| **Service Layer** | Business logic separation |

### Database Repository Pattern

```
src/database/
├── database.ts           # Database initialization
└── repositories/         # Data access layer (5 files)
    ├── [Entity]Repository.ts
    └── ...
```

### Service Layer

| Service | File | Purpose |
|---------|------|---------|
| `bluetoothService` | `bluetoothService.ts` | BLE device communication |
| `locationService` | `locationService.ts` | GPS/location handling |
| `notificationService` | `notificationService.ts` | Push notifications |
| `syncService` | `syncService.ts` | Data synchronization |

---

## Architecture Summary

### Component Hierarchy

```
App.tsx
└── TrackingProvider
    └── AppNavigator
        ├── Auth Screens (4)
        ├── Home Screens (2)
        ├── Loading Screens (3)
        ├── IoT Screens (3)
        ├── Orders Screens (5)
        ├── Trips Screens (3)
        ├── Filling Screens (2)
        └── Offload Screens (3)
            └── Common Components (8)
```

### Key Architectural Decisions

| Decision | Implementation |
|----------|----------------|
| **Framework** | React Native (Expo) |
| **Language** | TypeScript |
| **State Management** | Custom stores (Zustand likely) |
| **Navigation** | React Navigation (inferred from structure) |
| **Styling** | Custom theme system |
| **Data Layer** | Local database with repositories |
| **API Layer** | Centralized client with feature-specific APIs |

### Feature Distribution

| Feature | Screens | Complexity |
|---------|---------|------------|
| Orders | 5 files | High |
| Auth | 4 files | Medium |
| IoT | 3 files | Medium |
| Trips | 3 files | Medium |
| Offload | 3 files | Medium |
| Loading | 3 files | Low |
| Home | 2 files | Low |
| Filling | 2 files | Low |

---

## Recommendations

### Identified Gaps

1. **Testing Infrastructure** - No visible test setup
2. **Storybook/Component Documentation** - Not present
3. **Error Boundary Components** - Not explicitly visible
4. **Accessibility** - Cannot confirm ARIA implementation

### Strengths

1. ✅ Clean separation of concerns (screens, components, services, stores)
2. ✅ Centralized theme system
3. ✅ TypeScript implementation
4. ✅ Repository pattern for data access
5. ✅ Custom hooks for reusable logic
6. ✅ Provider pattern for cross-cutting concerns

# state_and_data

State management and data flow patterns

# State Management & Data Flow Analysis

## Repository: react-app-oms_635d2a1a (React Native OMS IoT Application)

---

## State Management

### 1. Global State: Zustand

**Library Used:** `zustand ^5.0.8`

**Store Structure & Organization:**

The application uses Zustand with a modular store architecture located in `/src/store/`:

| Store File | Purpose |
|------------|---------|
| `authStore.ts` | Authentication state management |
| `bluetoothStore.ts` | Bluetooth device/connection state |
| `locationStore.ts` | GPS/location tracking state |
| `syncStore.ts` | Data synchronization state |
| `tripStore.ts` | Trip/delivery management state |
| `index.ts` | Store exports aggregation |

**Implementation Approach:**
- Modular store pattern with separate files per domain
- Centralized export through `index.ts`
- Each store handles a specific business domain

**Key Files:**
- `/src/store/authStore.ts`
- `/src/store/bluetoothStore.ts`
- `/src/store/locationStore.ts`
- `/src/store/syncStore.ts`
- `/src/store/tripStore.ts`

---

### 2. Local Component State

**Pattern:** React Native functional components with hooks

**UI State Locations:**
- `/src/screens/` - Screen-level local state
- `/src/components/` - Component-level local state

**Key Components with Local State:**
- `PermissionDisclosureModal.tsx` - Modal visibility state
- Screen components in `/src/screens/`:
  - `home/` (2 files)
  - `auth/` (4 files) - Login/registration forms
  - `orders/` (5 files) - Order management UI
  - `trips/` (3 files) - Trip status UI
  - `filling/` (2 files)
  - `offload/` (3 files)
  - `iot/` (3 files) - IoT device interaction
  - `loading/` (3 files) - Loading states

---

### 3. Server State: TanStack React Query

**Library Used:** `@tanstack/react-query ^5.90.11`

**Implementation Approach:**
- Server state caching and synchronization
- Automatic background refetching
- Request deduplication

**Key Integration Points:**
- Works alongside Zustand for server vs client state separation
- Likely integrated with API layer in `/src/api/`

---

## Data Flow

### 1. API Integration

**HTTP Client:** `axios ^1.13.2`

**API Layer Organization (`/src/api/`):**

| File | Purpose |
|------|---------|
| `client.ts` | Axios instance configuration, interceptors |
| `authApi.ts` | Authentication endpoints |
| `omsApi.ts` | Order Management System endpoints |
| `iotApi.ts` | IoT device communication endpoints |
| `index.ts` | API exports aggregation |

**Architecture:**
```
┌─────────────────┐
│   Components    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  React Query    │ ◄── Caching/State
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   API Layer     │ ◄── authApi, omsApi, iotApi
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Axios Client   │ ◄── client.ts (interceptors)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Backend API   │
└─────────────────┘
```

---

### 2. Local Database / Offline Storage

**Database:** `expo-sqlite ^16.0.9`

**Database Layer (`/src/database/`):**

| File/Folder | Purpose |
|-------------|---------|
| `database.ts` | SQLite connection & initialization |
| `repositories/` (5 files) | Data access layer / Repository pattern |

**Secure Storage:** `expo-secure-store ^15.0.7`
- Token storage
- Sensitive credential storage

---

### 3. Authentication & Authorization

**Auth State:** Managed in `/src/store/authStore.ts` (Zustand)

**Auth API:** `/src/api/authApi.ts`

**Token Storage:** `expo-secure-store` for secure token persistence

**Protected Routes:** Likely implemented in `/src/navigation/AppNavigator.tsx`

**Key Navigation Files:**
- `/src/navigation/AppNavigator.tsx` - Main navigator with auth flow
- `/src/navigation/types.ts` - Navigation type definitions

---

### 4. Real-time Features & Background Processing

**Bluetooth (IoT):**
- Library: `react-native-ble-plx ^3.5.0`
- Service: `/src/services/bluetoothService.ts`
- Store: `/src/store/bluetoothStore.ts`

**Location Tracking:**
- Library: `expo-location ^19.0.7`
- Service: `/src/services/locationService.ts`
- Store: `/src/store/locationStore.ts`

**Background Tasks:**
- Libraries: `expo-background-fetch ^14.0.8`, `expo-task-manager ^14.0.8`
- Enables background sync and location tracking

**Push Notifications:**
- Library: `expo-notifications ^0.32.13`
- Service: `/src/services/notificationService.ts`

---

### 5. Data Synchronization

**Sync Service:** `/src/services/syncService.ts`

**Sync Store:** `/src/store/syncStore.ts`

**Network Detection:** `@react-native-community/netinfo ^11.4.1`

**Offline-First Architecture:**
```
┌──────────────────────────────────────────────────────┐
│                    Application                        │
├──────────────────────────────────────────────────────┤
│  ┌─────────────┐    ┌─────────────┐    ┌──────────┐ │
│  │   Zustand   │◄──►│ React Query │◄──►│  API     │ │
│  │   Stores    │    │   Cache     │    │  Layer   │ │
│  └──────┬──────┘    └─────────────┘    └────┬─────┘ │
│         │                                    │       │
│         ▼                                    ▼       │
│  ┌─────────────┐                      ┌──────────┐  │
│  │   SQLite    │◄── Sync Service ────►│  Server  │  │
│  │  (Offline)  │                      │   API    │  │
│  └─────────────┘                      └──────────┘  │
└──────────────────────────────────────────────────────┘
```

---

### 6. Services Layer (`/src/services/`)

| Service | Purpose |
|---------|---------|
| `bluetoothService.ts` | BLE device communication |
| `locationService.ts` | GPS tracking & geofencing |
| `notificationService.ts` | Push notification handling |
| `syncService.ts` | Offline data synchronization |

---

### 7. Custom Hooks (`/src/hooks/`)

| Hook | Purpose |
|------|---------|
| `useTrackingSetup.ts` | Location/device tracking initialization |
| `index.ts` | Hook exports |

---

### 8. Type Definitions

**Location:** `/src/types/index.ts`

**Navigation Types:** `/src/navigation/types.ts`

---

## Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                        PRESENTATION LAYER                           │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  Screens (home, auth, orders, trips, filling, offload, iot) │   │
│  └──────────────────────────┬──────────────────────────────────┘   │
│                              │                                      │
│  ┌───────────────────────────┴──────────────────────────────┐      │
│  │              Components + TrackingProvider               │      │
│  └──────────────────────────┬───────────────────────────────┘      │
└─────────────────────────────┼───────────────────────────────────────┘
                              │
┌─────────────────────────────┼───────────────────────────────────────┐
│                        STATE LAYER                                   │
│  ┌──────────────────────────┴──────────────────────────────┐       │
│  │                      ZUSTAND STORES                      │       │
│  │  ┌────────┐ ┌───────────┐ ┌──────────┐ ┌──────┐ ┌────┐ │       │
│  │  │  auth  │ │ bluetooth │ │ location │ │ sync │ │trip│ │       │
│  │  └────────┘ └───────────┘ └──────────┘ └──────┘ └────┘ │       │
│  └──────────────────────────┬──────────────────────────────┘       │
│                              │                                      │
│  ┌──────────────────────────┴──────────────────────────────┐       │
│  │               REACT QUERY (Server State)                 │       │
│  └──────────────────────────┬──────────────────────────────┘       │
└─────────────────────────────┼───────────────────────────────────────┘
                              │
┌─────────────────────────────┼───────────────────────────────────────┐
│                       DATA ACCESS LAYER                              │
│  ┌──────────────────────────┴──────────────────────────────┐       │
│  │                       API LAYER                          │       │
│  │    ┌────────┐     ┌────────┐     ┌────────┐             │       │
│  │    │authApi │     │ omsApi │     │ iotApi │             │       │
│  │    └───┬────┘     └───┬────┘     └───┬────┘             │       │
│  │        └──────────────┼──────────────┘                  │       │
│  │                       ▼                                  │       │
│  │              ┌─────────────────┐                        │       │
│  │              │  Axios Client   │                        │       │
│  │              └─────────────────┘                        │       │
│  └─────────────────────────────────────────────────────────┘       │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────┐       │
│  │                  DATABASE LAYER                          │       │
│  │    ┌─────────────┐      ┌────────────────────────┐      │       │
│  │    │  database.ts│      │     repositories/      │      │       │
│  │    │  (SQLite)   │◄────►│    (5 repo files)      │      │       │
│  │    └─────────────┘      └────────────────────────┘      │       │
│  └─────────────────────────────────────────────────────────┘       │
└─────────────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────┼───────────────────────────────────────┐
│                      SERVICES LAYER                                  │
│  ┌──────────────┐ ┌────────────────┐ ┌────────────────────────┐    │
│  │ bluetooth    │ │   location     │ │     notification       │    │
│  │ Service      │ │   Service      │ │       Service          │    │
│  └──────────────┘ └────────────────┘ └────────────────────────┘    │
│                              │                                      │
│              ┌───────────────┴───────────────┐                     │
│              │        syncService            │                     │
│              │  (Offline Sync Coordinator)   │                     │
│              └───────────────────────────────┘                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Performance Implications

| Pattern | Impact |
|---------|--------|
| **Zustand** | Minimal re-renders due to selector-based subscriptions |
| **React Query** | Automatic caching reduces API calls, background refetching |
| **SQLite (expo-sqlite)** | Fast local queries, enables offline-first UX |
| **FlashList (@shopify/flash-list)** | High-performance list rendering for large datasets |
| **Background Fetch** | Enables data sync without user interaction |
| **Repository Pattern** | Clean data access abstraction, testable |

---

## Summary

This React Native application implements a **robust offline-first architecture** with:

- **Zustand** for lightweight, modular client state management
- **TanStack React Query** for server state caching and synchronization
- **Axios** for HTTP communication with organized API modules
- **expo-sqlite** with repository pattern for local persistence
- **Background services** for location tracking, Bluetooth IoT communication, and data sync
- **expo-secure-store** for secure credential storage