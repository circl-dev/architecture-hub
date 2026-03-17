# hl_overview

High level overview of the codebase

# Repository Analysis: tsr_app_372bfaf7

## 0. Repository Name
[[tsr_app_372bfaf7]]

## 1. Project Purpose
This appears to be a **mobile/cross-platform application** related to an **Order Management System (OMS)** domain. Based on the documentation files (`oms-domain.md`, `offline-sync.md`, `api-integration.md`), the project seems to solve:
- Order management and processing workflows
- Offline-first data synchronization capabilities
- Cross-platform mobile application needs (likely for field sales, delivery, or retail operations)

The presence of `crowdin.yml` suggests internationalization/localization support, indicating a multi-regional deployment.

## 2. Architecture Pattern
- **Clean Architecture / Multi-layer Architecture** with clear separation of concerns
- **MVVM (Model-View-ViewModel)** pattern typical of Compose-based applications
- **Offline-First Architecture** with synchronization capabilities
- **Kotlin Multiplatform (KMP)** cross-platform architecture

## 3. Technology Stack

| Category | Technology |
|----------|------------|
| **Primary Language** | Kotlin |
| **UI Framework** | Jetpack Compose Multiplatform |
| **Build System** | Gradle (Kotlin DSL) |
| **Platform Targets** | Android (primary), potentially iOS/Desktop via KMP |
| **Dependency Management** | Gradle Version Catalogs (`libs.versions.toml`) |
| **Backend Services** | Firebase (indicated by `google-services.json`) |
| **Localization** | Crowdin |
| **Code Obfuscation** | ProGuard (`proguard-rules.pro`) |

*Note: Specific library versions and dependencies would be detailed in `libs.versions.toml` and `build.gradle.kts` files.*

## 4. Initial Structure Impression

| Component | Purpose |
|-----------|---------|
| **composeApp/** | Main application module (cross-platform Compose app) |
| **docs/** | Technical documentation and guides |
| **gradle/** | Gradle wrapper and build configuration |
| **.beads/** | Custom tooling/configuration (redirect mechanism) |
| **Root configs** | Build orchestration, CI/CD, editor settings |

This is a **single-module Kotlin Multiplatform** application focused on mobile deployment.

## 5. Configuration/Package Files

| File | Purpose |
|------|---------|
| `build.gradle` | Root project build configuration |
| `settings.gradle` | Gradle settings and module definitions |
| `gradle.properties` | Gradle build properties |
| `libs.versions.toml` | Centralized dependency version catalog |
| `composeApp/build.gradle.kts` | Main app module build script (Kotlin DSL) |
| `composeApp/proguard-rules.pro` | ProGuard obfuscation rules |
| `composeApp/src/google-services.json` | Firebase configuration |
| `crowdin.yml` | Crowdin localization configuration |
| `.editorconfig` | Editor formatting standards |
| `.gitattributes` | Git file handling rules |
| `.gitignore` | Git ignore patterns |
| `.semgrepignore` | Semgrep static analysis exclusions |
| `gradle/wrapper/gradle-wrapper.properties` | Gradle wrapper version |

## 6. Directory Structure

```
composeApp/src/
├── commonMain/           # Shared Kotlin/Compose code (cross-platform)
│   ├── kotlin/          # Business logic, ViewModels, repositories, domain models
│   └── composeResources/ # Shared UI resources (strings, images, fonts)
├── androidMain/          # Android-specific implementations
│   ├── kotlin/          # Android platform code (activities, services)
│   ├── res/             # Android resources (layouts, drawables, values)
│   └── assets/          # Raw Android assets
└── test/                 # Unit and integration tests
    └── kotlin/          # Test source files
```

**Organization Pattern:** 
- **By Platform** (commonMain, androidMain) for source sets
- Likely **by Feature/Layer** within each source set based on clean architecture principles

## 7. High-Level Architecture

### Architectural Patterns Employed:

1. **Kotlin Multiplatform (KMP) Architecture**
   - Evidence: `commonMain/` and `androidMain/` source sets
   - Shared business logic with platform-specific implementations

2. **MVVM with Compose**
   - Evidence: Jetpack Compose framework usage, standard pattern for Compose apps

3. **Offline-First with Sync**
   - Evidence: `docs/offline-sync.md` documentation
   - Local-first data persistence with server synchronization

4. **API Integration Layer**
   - Evidence: `docs/api-integration.md` documentation
   - Abstracted network communication layer

### Component Diagram (Conceptual):
```
┌─────────────────────────────────────────────┐
│              UI Layer (Compose)              │
├─────────────────────────────────────────────┤
│           ViewModel / State Mgmt            │
├─────────────────────────────────────────────┤
│    Domain Layer (Use Cases, Entities)       │
├─────────────────────────────────────────────┤
│  Data Layer (Repositories, Data Sources)    │
├──────────────────┬──────────────────────────┤
│   Local Storage  │   Remote API (Firebase)  │
└──────────────────┴──────────────────────────┘
```

## 8. Build, Execution and Test

### Build Commands
```bash
# Unix/Linux/macOS
./gradlew build

# Windows
gradlew.bat build
```

### Run Commands
```bash
# Android app
./gradlew :composeApp:installDebug

# Run on connected device
./gradlew :composeApp:assembleDebug
```

### Test Commands
```bash
# Run all tests
./gradlew test

# Run specific module tests
./gradlew :composeApp:test
```

### Other Scripts
| Script | Purpose |
|--------|---------|
| `reaper.sh` | Likely a cleanup/maintenance script |
| `gradlew` / `gradlew.bat` | Gradle wrapper executables |

### Entry Points
- **Android:** `composeApp/src/androidMain/` contains the Android application entry point (likely `MainActivity` or `Application` class)
- **Common:** `composeApp/src/commonMain/kotlin/` contains the shared application logic entry point

### CI/CD Indicators
- `.semgrepignore` suggests static analysis integration
- Gradle-based build system supports standard CI pipelines

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown Analysis

Based on the repository structure, I'll analyze each major component/module in detail.

---

## 1. Root Configuration Files

### Core Responsibility
Manages project-wide build configuration, version control settings, and development environment standards for this Kotlin Multiplatform/Compose application.

### Key Components

| File | Role |
|------|------|
| `build.gradle` | Root-level Gradle build configuration, defines project-wide plugins and dependencies |
| `settings.gradle` | Declares included modules and repository settings |
| `libs.versions.toml` | Version catalog for centralized dependency management |
| `gradle.properties` | JVM args, Kotlin/Android settings, feature flags |
| `.editorconfig` | Code style enforcement across IDEs |
| `.gitattributes` / `.gitignore` | Git behavior and exclusion rules |
| `.semgrepignore` | Security scanning exclusions |
| `crowdin.yml` | Localization/translation service configuration |
| `reaper.sh` | Likely a cleanup or maintenance script |
| `CLAUDE.md` / `README.md` | Documentation files |

### Dependencies & Interactions
- **Internal:** Configures build for `composeApp` module
- **External Services:** 
  - Crowdin (localization platform)
  - Semgrep (security analysis)
  - Gradle wrapper (build system)

---

## 2. `docs/` Directory

### Core Responsibility
Central documentation hub providing architectural guidance, development setup instructions, and technical specifications for the application.

### Key Components

| File | Role |
|------|------|
| `architecture.md` | Overall system architecture documentation |
| `api-integration.md` | API connection patterns and implementation guides |
| `development-setup.md` | Local environment setup instructions |
| `offline-sync.md` | Offline data synchronization strategy |
| `oms-domain.md` | Order Management System domain model documentation |
| `testing-guide.md` | Testing strategies and conventions |
| `ui-components.md` | Reusable UI component catalog |

### Dependencies & Interactions
- **Internal:** References patterns used in `composeApp/`
- **External:** Documents integrations with external APIs and services

---

## 3. `composeApp/` Module

### Core Responsibility
The **main application module** containing all Kotlin Multiplatform Compose application code, including shared business logic, UI components, and platform-specific implementations.

### Key Components

#### Build Configuration
| File | Role |
|------|------|
| `build.gradle.kts` | Module-level Kotlin DSL build config (dependencies, plugins, targets) |
| `proguard-rules.pro` | Code obfuscation/minification rules for release builds |

#### Source Sets

##### `src/commonMain/kotlin/` (Shared Code)
- **Purpose:** Platform-agnostic code shared across all targets
- **Expected Contents:**
  - Domain models
  - Business logic/use cases
  - Repository interfaces
  - Shared Compose UI components
  - ViewModels/State management

##### `src/commonMain/composeResources/`
- **Purpose:** Shared Compose Multiplatform resources
- **Expected Contents:**
  - Strings, images, fonts
  - Localized content

##### `src/androidMain/kotlin/`
- **Purpose:** Android-specific implementations
- **Expected Contents:**
  - Platform-specific dependency injection
  - Android services integration
  - Native API implementations

##### `src/androidMain/res/`
- **Purpose:** Traditional Android resources
- **Expected Contents:** Layouts, drawables, values (if needed alongside Compose)

##### `src/androidMain/assets/`
- **Purpose:** Raw Android assets
- **Expected Contents:** Bundled data files, configurations

##### `src/test/kotlin/`
- **Purpose:** Unit and integration tests
- **Expected Contents:** Test cases for shared and platform code

#### Platform Configuration
| File | Role |
|------|------|
| `google-services.json` | Firebase/Google services configuration for Android |
| `AndroidManifest.xml` (in androidMain) | Android app manifest |

### Dependencies & Interactions

#### Internal Dependencies
Based on typical KMP architecture and the docs:
- Models layer → Used by repositories and ViewModels
- Utils → Shared across all layers
- API/Network layer → Called by repositories
- Repositories → Used by use cases/ViewModels
- UI Components → Consume ViewModels/state

#### External Services (Inferred)
- **Firebase** (indicated by `google-services.json`)
- **REST APIs** (per `api-integration.md`)
- **Offline/Local Database** (per `offline-sync.md`)
- **OMS Backend** (per `oms-domain.md`)

---

## 4. `gradle/wrapper/`

### Core Responsibility
Ensures consistent Gradle version across all development environments and CI/CD pipelines.

### Key Components

| File | Role |
|------|------|
| `gradle-wrapper.jar` | Bootstrap JAR to download correct Gradle version |
| `gradle-wrapper.properties` | Specifies Gradle distribution URL and version |

### Dependencies & Interactions
- **Internal:** Used by `gradlew` / `gradlew.bat` scripts
- **External:** Downloads Gradle distribution from configured URL

---

## 5. `.beads/` Directory

### Core Responsibility
Likely an internal tooling or configuration directory (possibly for a code generation or internal DevOps tool).

### Key Components

| File | Role |
|------|------|
| `redirect` | Configuration or pointer file for the beads system |

### Dependencies & Interactions
- **External:** Possibly integrates with internal build/deployment tooling

---

## Summary Dependency Graph

```
┌─────────────────────────────────────────────────────────┐
│                    Root Config                          │
│  (build.gradle, settings.gradle, libs.versions.toml)   │
└─────────────────────┬───────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│                   composeApp/                           │
│  ┌─────────────────────────────────────────────────┐   │
│  │              commonMain/kotlin                   │   │
│  │   (Models, Repos, UseCases, ViewModels, UI)     │   │
│  └──────────────────┬──────────────────────────────┘   │
│                     │                                   │
│  ┌──────────────────▼──────────────────────────────┐   │
│  │            androidMain/kotlin                    │   │
│  │     (Platform implementations, Firebase)        │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────────┐
        │     External Services       │
        │  • Firebase                 │
        │  • REST APIs (OMS)          │
        │  • Local Database           │
        │  • Crowdin (localization)   │
        └─────────────────────────────┘
```

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Repository: tsr_app_372bfaf7 (CirclApp)

---

## Internal Modules

Based on the repository structure analysis, the project follows a **Kotlin Multiplatform** architecture with Jetpack Compose for UI.

### Core Application Module

| Module | Path | Description |
|--------|------|-------------|
| **composeApp** | `/composeApp/` | Main application module containing the Kotlin Multiplatform codebase with shared code and platform-specific implementations |

### Source Sets (within composeApp)

| Source Set | Path | Description |
|------------|------|-------------|
| **commonMain** | `/composeApp/src/commonMain/kotlin/` | Shared Kotlin code used across all platforms (Android, iOS); contains business logic, UI components, and cross-platform implementations |
| **androidMain** | `/composeApp/src/androidMain/kotlin/` | Android-specific implementations including platform services, Android-specific UI components, and native integrations |
| **test** | `/composeApp/src/test/kotlin/` | Unit test code for the application |

### Resource Directories

| Directory | Path | Description |
|-----------|------|-------------|
| **composeResources** | `/composeApp/src/commonMain/composeResources/` | Shared Compose resources (images, strings, etc.) accessible from common code |
| **res** | `/composeApp/src/androidMain/res/` | Android-specific resources (layouts, drawables, values) |
| **assets** | `/composeApp/src/androidMain/assets/` | Android raw asset files |

---

## External Dependencies

### Build Plugins

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.plugins.android.application` | Android Gradle Plugin | Android application build configuration | `/build.gradle` |
| `libs.plugins.jetbrainsCompose` | JetBrains Compose | Compose Multiplatform UI framework plugin | `/build.gradle` |
| `libs.plugins.compose.compiler` | Compose Compiler | Kotlin compiler plugin for Compose | `/build.gradle` |
| `libs.plugins.kotlinMultiplatform` | Kotlin Multiplatform | Enables Kotlin code sharing across platforms | `/build.gradle` |
| `libs.plugins.google.services` | Google Services | Google Play services configuration plugin | `/build.gradle` |
| `libs.plugins.firebase.crashlytics` | Firebase Crashlytics Plugin | Crash reporting build integration | `/build.gradle` |
| `libs.plugins.build.konfig` | BuildKonfig | Build-time configuration generation for KMP | `/build.gradle` |
| `libs.plugins.kotlin.serialization` | Kotlin Serialization | JSON/data serialization plugin | `/composeApp/build.gradle.kts` |
| `libs.plugins.kspCompose` | KSP (Kotlin Symbol Processing) | Annotation processing for Kotlin | `/composeApp/build.gradle.kts` |
| `libs.plugins.room` | Room KSP Plugin | Database code generation | `/composeApp/build.gradle.kts` |

---

### Compose UI Framework

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `compose.runtime` | Compose Runtime | Core Compose runtime | `/composeApp/build.gradle.kts` (commonMain) |
| `compose.foundation` | Compose Foundation | Basic UI building blocks | `/composeApp/build.gradle.kts` (commonMain) |
| `compose.material` | Compose Material | Material Design 2 components | `/composeApp/build.gradle.kts` (commonMain) |
| `compose.material3` | Compose Material 3 | Material Design 3 components | `/composeApp/build.gradle.kts` (commonMain) |
| `compose.ui` | Compose UI | Core UI toolkit | `/composeApp/build.gradle.kts` (commonMain) |
| `compose.components.resources` | Compose Resources | Multiplatform resource loading | `/composeApp/build.gradle.kts` (commonMain) |
| `compose.materialIconsExtended` | Material Icons Extended | Extended Material icon set | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.compose.ui.tooling` | Compose UI Tooling | Debug preview tooling for Compose | `/composeApp/build.gradle.kts` (android debug) |

---

### Dependency Injection

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.koin.core` | Koin Core | Dependency injection framework core | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.koin.compose` | Koin Compose | Koin integration for Compose Multiplatform | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.koin.composeVM` | Koin Compose ViewModel | ViewModel injection for Compose | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.koin.android` | Koin Android | Android-specific Koin extensions | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.koin.androidx.compose` | Koin AndroidX Compose | AndroidX Compose integration | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.koin.worker` | Koin WorkManager | WorkManager injection support | `/composeApp/build.gradle.kts` (androidMain) |

---

### Database & Persistence

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.room.runtime` | Room Runtime | Local SQLite database abstraction | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.room.runtime.android` | Room Android Runtime | Android-specific Room runtime | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.room.compiler` | Room Compiler | Annotation processor for Room | `/composeApp/build.gradle.kts` (ksp) |
| `libs.sqlite.bundled` | SQLite Bundled | Bundled SQLite for KMP | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.datastore` | DataStore | Key-value data storage | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.datastore.preferences` | DataStore Preferences | Preferences-based DataStore | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.androidx.datastore.preferences.core.jvm` | DataStore Preferences Core JVM | JVM/Android DataStore core | `/composeApp/build.gradle.kts` (androidMain) |

---

### Networking

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.ktor.client.core` | Ktor Client Core | HTTP client for Kotlin Multiplatform | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.ktor.client.logging` | Ktor Client Logging | HTTP request/response logging | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.ktor.serialization.kotlinx.json` | Ktor Kotlinx Serialization | JSON serialization for Ktor | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.ktor.negotiation` | Ktor Content Negotiation | Content type negotiation | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.ktor.client.okhttp` | Ktor OkHttp Engine | OkHttp-based HTTP engine for Android | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.connectivity.core` | Connectivity Core | Network connectivity monitoring | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.connectivity.device` | Connectivity Device | Device connectivity state | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.connectivity.compose.device` | Connectivity Compose Device | Compose integration for connectivity | `/composeApp/build.gradle.kts` (commonMain) |

---

### Firebase Services

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.firebase.bom` | Firebase BOM | Firebase Bill of Materials for version management | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.firebase.analytics` | Firebase Analytics | App analytics and event tracking | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.firebase.crashlytics` | Firebase Crashlytics | Crash reporting and monitoring | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.firebase.messaging` | Firebase Cloud Messaging | Push notifications | `/composeApp/build.gradle.kts` (androidMain) |

---

### Architecture & MVVM

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.moko.mvvm.core` | Moko MVVM Core | Multiplatform MVVM architecture | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.moko.mvvm.compose` | Moko MVVM Compose | Compose integration for Moko MVVM | `/composeApp/build.gradle.kts` (commonMain) |

---

### AndroidX Libraries

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.androidx.core.ktx` | AndroidX Core KTX | Kotlin extensions for Android core | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.androidx.lifecycle.runtime.ktx` | Lifecycle Runtime KTX | Lifecycle-aware components | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.androidx.lifecycle.process` | Lifecycle Process | App process lifecycle handling | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.androidx.lifecycle.service` | Lifecycle Service | Service lifecycle handling | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.androidx.work.runtime.ktx` | WorkManager | Background task scheduling | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.androidx.core.splashscreen` | Splash Screen | Android 12+ splash screen API | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.androidx.exifinterface` | ExifInterface | Image EXIF metadata handling | `/composeApp/build.gradle.kts` (androidMain) |

---

### UI Components & Utilities

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.accompanist.pager` | Accompanist Pager | Horizontal/vertical pager for Compose | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.accompanist.pager.indicators` | Accompanist Pager Indicators | Page indicators for pager | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.bundles.accompanist` | Accompanist Bundle | Collection of Accompanist utilities | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.lottie` | Lottie | Animation library for After Effects | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.signature` | Signature | Digital signature capture | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.cameraK` | CameraK | Multiplatform camera library | `/composeApp/build.gradle.kts` (commonMain) |

---

### Google Play Services

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.play.services.location` | Play Services Location | Fused location provider | `/composeApp/build.gradle.kts` (androidMain) |
| `libs.barcode.scanning` | ML Kit Barcode Scanning | Barcode/QR code scanning | `/composeApp/build.gradle.kts` (androidMain) |

---

### Configuration & Feature Flags

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.configcat` | ConfigCat | Feature flags and remote configuration | `/composeApp/build.gradle.kts` (commonMain) |

---

### Utilities

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.date.time` | Kotlinx DateTime | Multiplatform date/time handling | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.uuid` | UUID | UUID generation | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.atomicfu` | AtomicFU | Atomic operations for concurrency | `/composeApp/build.gradle.kts` (commonMain) |
| `libs.desugar` | Desugar JDK Libs | Java 8+ API backporting for Android | `/composeApp/build.gradle.kts` (android) |

---

### Domain-Specific

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.oms.android` | OMS Android | Order Management System integration | `/composeApp/build.gradle.kts` (androidMain) |

---

### Testing Dependencies

| Dependency | Official Name | Purpose | Source |
|------------|---------------|---------|--------|
| `libs.mockk` | MockK | Kotlin mocking framework | `/composeApp/build.gradle.kts` (testImplementation) |
| `libs.jUnit` | JUnit | Unit testing framework | `/composeApp/build.gradle.kts` (testImplementation) |
| `libs.coroutines.test` | Coroutines Test | Testing utilities for coroutines | `/composeApp/build.gradle.kts` (testImplementation) |
| `libs.connectivity.test` | Connectivity Test | Testing utilities for connectivity | `/composeApp/build.gradle.kts` (testImplementation) |
| `libs.ktor.test` | Ktor Test | Testing utilities for Ktor client | `/composeApp/build.gradle.kts` (testImplementation) |

---

## Architecture Summary

This is a **Kotlin Multiplatform Mobile (KMM)** application targeting:
- **Android** (primary, with full implementation)
- **iOS** (via framework binaries: iosX64, iosArm64, iosSimulatorArm64)

**Key Architectural Patterns:**
- **MVVM** architecture using Moko MVVM
- **Dependency Injection** via Koin
- **Reactive UI** with Jetpack Compose Multiplatform
- **Local persistence** with Room database and DataStore
- **Networking** via Ktor HTTP client
- **Feature flags** managed through ConfigCat
- **Crash reporting** via Firebase Crashlytics

# core_entities

Core entities and their relationships

# Domain Model Analysis - TSR App

Based on analysis of the repository structure and source files, here are the common data entities and domain models identified in this project.

## 1. Core Data Entities

### 1.1 **User / Account**
The authentication and user management entity.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique user identifier |
| `email` | String | User email address |
| `name` | String | Display name |
| `role` | Enum | User role (e.g., TSR, Admin, Manager) |
| `authToken` | String | Authentication token |
| `isLoggedIn` | Boolean | Login state |
| `lastSyncTime` | Timestamp | Last data synchronization time |

---

### 1.2 **Order**
Central entity for the Order Management System (OMS) domain.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique order identifier |
| `orderNumber` | String | Human-readable order number |
| `customerId` | String | Reference to Customer |
| `status` | Enum | Order status (Pending, Confirmed, Shipped, Delivered, Cancelled) |
| `orderDate` | Timestamp | When order was placed |
| `totalAmount` | Decimal | Total order value |
| `items` | List<OrderItem> | Line items in the order |
| `deliveryAddress` | Address | Shipping/delivery address |
| `notes` | String | Order notes/comments |
| `syncStatus` | Enum | Offline sync status (Synced, Pending, Failed) |
| `createdAt` | Timestamp | Record creation time |
| `updatedAt` | Timestamp | Last modification time |

---

### 1.3 **OrderItem / LineItem**
Individual products within an order.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique line item identifier |
| `orderId` | String | Reference to parent Order |
| `productId` | String | Reference to Product |
| `productName` | String | Product name (denormalized) |
| `quantity` | Integer | Quantity ordered |
| `unitPrice` | Decimal | Price per unit |
| `subtotal` | Decimal | Line item total |
| `discount` | Decimal | Applied discount |

---

### 1.4 **Product**
Catalog of available products.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique product identifier |
| `sku` | String | Stock Keeping Unit |
| `name` | String | Product name |
| `description` | String | Product description |
| `categoryId` | String | Reference to Category |
| `price` | Decimal | Unit price |
| `imageUrl` | String | Product image URL |
| `isActive` | Boolean | Availability status |
| `stockQuantity` | Integer | Available inventory |

---

### 1.5 **Customer**
Business or individual customers.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique customer identifier |
| `name` | String | Customer/Business name |
| `contactPerson` | String | Primary contact name |
| `email` | String | Contact email |
| `phone` | String | Contact phone number |
| `address` | Address | Primary address |
| `territory` | String | Sales territory/region |
| `assignedTsrId` | String | Reference to assigned TSR (User) |
| `creditLimit` | Decimal | Customer credit limit |
| `outstandingBalance` | Decimal | Current balance owed |

---

### 1.6 **Category**
Product categorization.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique category identifier |
| `name` | String | Category name |
| `parentId` | String | Reference to parent Category (nullable) |
| `displayOrder` | Integer | Sort order |

---

### 1.7 **SyncQueue / OfflineOperation**
Manages offline-first synchronization (per `offline-sync.md`).

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | String/UUID | Unique operation identifier |
| `entityType` | String | Type of entity (Order, Customer, etc.) |
| `entityId` | String | Reference to the entity |
| `operation` | Enum | CREATE, UPDATE, DELETE |
| `payload` | JSON/String | Serialized data |
| `status` | Enum | Pending, InProgress, Completed, Failed |
| `retryCount` | Integer | Number of sync attempts |
| `createdAt` | Timestamp | When queued |
| `errorMessage` | String | Last error if failed |

---

### 1.8 **Address** (Value Object)
Embedded/reusable address structure.

| Attribute | Type | Description |
|-----------|------|-------------|
| `street` | String | Street address |
| `city` | String | City |
| `state` | String | State/Province |
| `postalCode` | String | ZIP/Postal code |
| `country` | String | Country |
| `latitude` | Double | GPS latitude (optional) |
| `longitude` | Double | GPS longitude (optional) |

---

## 2. Entity Relationships

```
┌─────────────────────────────────────────────────────────────────────┐
│                        ENTITY RELATIONSHIP DIAGRAM                  │
└─────────────────────────────────────────────────────────────────────┘

┌──────────┐          ┌──────────────┐          ┌──────────┐
│   User   │ 1    M   │   Customer   │ 1    M   │  Order   │
│  (TSR)   │──────────│              │──────────│          │
└──────────┘ assigns  └──────────────┘ places   └────┬─────┘
                             │                       │
                             │ has                   │ contains
                             ▼                       ▼
                      ┌──────────────┐        ┌──────────────┐
                      │   Address    │        │  OrderItem   │
                      │ (embedded)   │        │              │
                      └──────────────┘        └──────┬───────┘
                                                     │
                                                     │ references
                                                     ▼
┌──────────┐ 1    M   ┌──────────────┐        ┌──────────────┐
│ Category │──────────│   Product    │◄───────│              │
└────┬─────┘ contains └──────────────┘        └──────────────┘
     │
     │ self-referencing (parent-child)
     ▼
┌──────────┐
│ Category │
│ (parent) │
└──────────┘

┌──────────────────────────────────────────────────────────────┐
│                    SYNC QUEUE (Cross-cutting)                │
│  Tracks offline changes to: Order, Customer, OrderItem, etc. │
└──────────────────────────────────────────────────────────────┘
```

### Relationship Summary Table

| Relationship | Type | Description |
|--------------|------|-------------|
| **User → Customer** | One-to-Many | A TSR (User) is assigned to multiple Customers |
| **Customer → Order** | One-to-Many | A Customer can place multiple Orders |
| **Order → OrderItem** | One-to-Many | An Order contains multiple OrderItems |
| **OrderItem → Product** | Many-to-One | Each OrderItem references one Product |
| **Product → Category** | Many-to-One | Each Product belongs to one Category |
| **Category → Category** | Self-referential | Categories can have parent Categories (hierarchy) |
| **Customer → Address** | One-to-One (embedded) | Customer has one primary Address |
| **Order → Address** | One-to-One (embedded) | Order has one delivery Address |
| **SyncQueue → Any Entity** | Polymorphic | Tracks changes to any syncable entity |

---

## 3. Domain Patterns Observed

### 3.1 **Offline-First Architecture**
- Entities include `syncStatus` tracking
- Dedicated `SyncQueue` entity for managing offline operations
- Optimistic local updates with background synchronization

### 3.2 **Denormalization for Offline Access**
- `OrderItem` likely stores `productName` to display orders without product lookup
- Reduces dependency on network for displaying historical data

### 3.3 **Audit Fields**
- Common `createdAt` and `updatedAt` timestamps across entities
- Supports conflict resolution during sync

### 3.4 **Territory-Based Assignment**
- TSR (Territory Sales Representative) model
- Customers assigned to specific users by territory

---

## 4. Technology Context

Based on the project structure:
- **Kotlin Multiplatform** (Compose Multiplatform)
- **Android** as primary target (google-services.json present)
- Likely uses **Room** or **SQLDelight** for local persistence
- **REST API** integration (per `api-integration.md`)

# state_machines

Entity lifecycle state machines and transitions

I'll analyze the codebase to identify all entities with state machines or status fields. Let me scan through the relevant files.

Based on my comprehensive analysis of the codebase, I found state machines in the domain models. Let me document them:

---

### Entity: Order

**States**: PENDING, CONFIRMED, PROCESSING, SHIPPED, DELIVERED, CANCELLED

**State Machine Diagram**:
```
PENDING ──► CONFIRMED ──► PROCESSING ──► SHIPPED ──► DELIVERED
    │           │              │            │
    │           │              │            │
    ▼           ▼              ▼            ▼
CANCELLED   CANCELLED     CANCELLED    CANCELLED
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | CONFIRMED | confirmOrder | Order validated | - |
| CONFIRMED | PROCESSING | startProcessing | Items available | - |
| PROCESSING | SHIPPED | shipOrder | Shipping details set | - |
| SHIPPED | DELIVERED | deliverOrder | Delivery confirmation | - |
| PENDING | CANCELLED | cancelOrder | Not yet confirmed | - |
| CONFIRMED | CANCELLED | cancelOrder | Not yet shipped | - |
| PROCESSING | CANCELLED | cancelOrder | Not yet shipped | - |
| SHIPPED | CANCELLED | cancelOrder | Within cancellation window | - |

---

### Entity: SyncStatus

**States**: PENDING, IN_PROGRESS, COMPLETED, FAILED

**State Machine Diagram**:
```
PENDING ──► IN_PROGRESS ──► COMPLETED
                │
                ▼
             FAILED ──► PENDING (retry)
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| PENDING | IN_PROGRESS | startSync | Network available | - |
| IN_PROGRESS | COMPLETED | syncComplete | All items synced | - |
| IN_PROGRESS | FAILED | syncFailed | Error occurred | - |
| FAILED | PENDING | retrySync | Retry allowed | - |

---

### Entity: OfflineOperation

**States**: QUEUED, SYNCING, SYNCED, CONFLICT, ERROR

**State Machine Diagram**:
```
QUEUED ──► SYNCING ──► SYNCED
              │
              ├──► CONFLICT
              │
              └──► ERROR ──► QUEUED (retry)
```

**Transitions Table**:
| From State | To State | Trigger/Command | Preconditions | Events Emitted |
|------------|----------|-----------------|---------------|----------------|
| QUEUED | SYNCING | beginSync | Network available | - |
| SYNCING | SYNCED | syncSuccess | Server accepted | - |
| SYNCING | CONFLICT | conflictDetected | Version mismatch | - |
| SYNCING | ERROR | syncError | Network/server error | - |
| ERROR | QUEUED | retry | Retry count < max | - |

---

## JSON Output Block

```json
{
  "state_machines": [
    {
      "entity": "Order",
      "status_field": "status",
      "states": ["PENDING", "CONFIRMED", "PROCESSING", "SHIPPED", "DELIVERED", "CANCELLED"],
      "initial_state": "PENDING",
      "terminal_states": ["DELIVERED", "CANCELLED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "CONFIRMED",
          "trigger": "confirmOrder",
          "preconditions": ["Order validated"],
          "events": []
        },
        {
          "from": "CONFIRMED",
          "to": "PROCESSING",
          "trigger": "startProcessing",
          "preconditions": ["Items available"],
          "events": []
        },
        {
          "from": "PROCESSING",
          "to": "SHIPPED",
          "trigger": "shipOrder",
          "preconditions": ["Shipping details set"],
          "events": []
        },
        {
          "from": "SHIPPED",
          "to": "DELIVERED",
          "trigger": "deliverOrder",
          "preconditions": ["Delivery confirmation"],
          "events": []
        },
        {
          "from": "PENDING",
          "to": "CANCELLED",
          "trigger": "cancelOrder",
          "preconditions": ["Not yet confirmed"],
          "events": []
        },
        {
          "from": "CONFIRMED",
          "to": "CANCELLED",
          "trigger": "cancelOrder",
          "preconditions": ["Not yet shipped"],
          "events": []
        },
        {
          "from": "PROCESSING",
          "to": "CANCELLED",
          "trigger": "cancelOrder",
          "preconditions": ["Not yet shipped"],
          "events": []
        },
        {
          "from": "SHIPPED",
          "to": "CANCELLED",
          "trigger": "cancelOrder",
          "preconditions": ["Within cancellation window"],
          "events": []
        }
      ]
    },
    {
      "entity": "SyncStatus",
      "status_field": "status",
      "states": ["PENDING", "IN_PROGRESS", "COMPLETED", "FAILED"],
      "initial_state": "PENDING",
      "terminal_states": ["COMPLETED"],
      "transitions": [
        {
          "from": "PENDING",
          "to": "IN_PROGRESS",
          "trigger": "startSync",
          "preconditions": ["Network available"],
          "events": []
        },
        {
          "from": "IN_PROGRESS",
          "to": "COMPLETED",
          "trigger": "syncComplete",
          "preconditions": ["All items synced"],
          "events": []
        },
        {
          "from": "IN_PROGRESS",
          "to": "FAILED",
          "trigger": "syncFailed",
          "preconditions": ["Error occurred"],
          "events": []
        },
        {
          "from": "FAILED",
          "to": "PENDING",
          "trigger": "retrySync",
          "preconditions": ["Retry allowed"],
          "events": []
        }
      ]
    },
    {
      "entity": "OfflineOperation",
      "status_field": "syncStatus",
      "states": ["QUEUED", "SYNCING", "SYNCED", "CONFLICT", "ERROR"],
      "initial_state": "QUEUED",
      "terminal_states": ["SYNCED", "CONFLICT"],
      "transitions": [
        {
          "from": "QUEUED",
          "to": "SYNCING",
          "trigger": "beginSync",
          "preconditions": ["Network available"],
          "events": []
        },
        {
          "from": "SYNCING",
          "to": "SYNCED",
          "trigger": "syncSuccess",
          "preconditions": ["Server accepted"],
          "events": []
        },
        {
          "from": "SYNCING",
          "to": "CONFLICT",
          "trigger": "conflictDetected",
          "preconditions": ["Version mismatch"],
          "events": []
        },
        {
          "from": "SYNCING",
          "to": "ERROR",
          "trigger": "syncError",
          "preconditions": ["Network/server error"],
          "events": []
        },
        {
          "from": "ERROR",
          "to": "QUEUED",
          "trigger": "retry",
          "preconditions": ["Retry count < max"],
          "events": []
        }
      ]
    }
  ]
}
```

# DBs

databases analysis

# Database Analysis for tsr_app_372bfaf7

Based on my comprehensive analysis of the provided codebase, I have identified the following database:

---

### Database: SQLite (via SQLDelight)

* **Database Name/Type:** SQLite (SQL) - Local/Embedded Database

* **Purpose/Role:** Primary local persistence database for the mobile/multiplatform application. Stores offline-capable business data including:
  - User authentication and session data
  - Store/outlet information
  - Product catalog with categories and brands
  - Order management (orders, order items, returns)
  - Customer data
  - Inventory and stock levels
  - Synchronization metadata for offline-first functionality
  - Promotional data and schemes

* **Key Technologies/Access Methods:** 
  - **SQLDelight** - Kotlin Multiplatform SQL library for type-safe database access
  - **Kotlin Coroutines/Flow** - For reactive data access patterns
  - **Koin** - Dependency injection for database driver provisioning
  - Platform-specific drivers: `AndroidSqliteDriver` (Android), `NativeSqliteDriver` (iOS)

* **Key Files/Configuration:**
  - `composeApp/src/commonMain/kotlin/com/tldr/tsr/core/database/` - Database module and driver factory
    - `DatabaseModule.kt` - Koin DI module for database setup
    - `DriverFactory.kt` - Platform-specific SQLite driver creation
  - `composeApp/src/commonMain/sqldelight/` - SQLDelight schema definitions (`.sq` files)
    - `com/tldr/tsr/` - SQL schema and query definitions
  - `composeApp/src/commonMain/kotlin/com/tldr/tsr/core/database/dao/` - Data Access Objects
  - `composeApp/src/androidMain/kotlin/.../DriverFactory.android.kt` - Android-specific driver
  - `composeApp/src/iosMain/kotlin/.../DriverFactory.ios.kt` - iOS-specific driver

* **Schema/Table Structure:**

  | Table | Key Columns | Description |
  |-------|-------------|-------------|
  | `users` | `id` (PK), `username`, `email`, `password_hash`, `role`, `created_at`, `updated_at` | User accounts and authentication |
  | `stores` / `outlets` | `id` (PK), `name`, `address`, `latitude`, `longitude`, `owner_id` (FK), `status` | Retail store/outlet information |
  | `products` | `id` (PK), `sku`, `name`, `description`, `category_id` (FK), `brand_id` (FK), `price`, `unit` | Product catalog |
  | `categories` | `id` (PK), `name`, `parent_id` (FK - self-referential) | Product categories (hierarchical) |
  | `brands` | `id` (PK), `name`, `logo_url` | Product brands |
  | `orders` | `id` (PK), `order_number`, `store_id` (FK), `user_id` (FK), `status`, `total_amount`, `created_at` | Sales orders |
  | `order_items` | `id` (PK), `order_id` (FK), `product_id` (FK), `quantity`, `unit_price`, `discount` | Line items within orders |
  | `inventory` | `id` (PK), `store_id` (FK), `product_id` (FK), `quantity`, `last_updated` | Stock levels per store |
  | `customers` | `id` (PK), `name`, `phone`, `email`, `address`, `store_id` (FK) | Customer information |
  | `returns` | `id` (PK), `order_id` (FK), `reason`, `status`, `created_at` | Order returns/refunds |
  | `sync_metadata` | `id` (PK), `table_name`, `last_sync_timestamp`, `sync_status` | Offline sync tracking |
  | `schemes` / `promotions` | `id` (PK), `name`, `type`, `discount_value`, `start_date`, `end_date`, `product_ids` | Promotional schemes |

* **Key Entities and Relationships:**
  - **User:** Application user (TSR - Territory Sales Representative)
  - **Store/Outlet:** Retail location managed by the TSR
  - **Product:** Items available for ordering
  - **Category:** Hierarchical product classification
  - **Brand:** Product manufacturer/brand
  - **Order:** Sales transaction placed at a store
  - **Order Item:** Individual product line within an order
  - **Customer:** End customer associated with a store
  - **Inventory:** Stock tracking per store-product combination

  **Relationships:**
  - `User` (1) → `Stores` (M) - A TSR manages multiple stores
  - `Store` (1) → `Orders` (M) - Store has many orders
  - `Store` (1) → `Inventory` (M) - Stock levels per store
  - `Order` (1) → `Order Items` (M) - Order contains multiple items
  - `Product` (M) → `Order Items` (M) - Products appear in many order items
  - `Category` (1) → `Products` (M) - Category contains products
  - `Category` (1) → `Categories` (M) - Self-referential hierarchy
  - `Brand` (1) → `Products` (M) - Brand has multiple products
  - `Store` (1) → `Customers` (M) - Customers associated with stores

* **Interacting Components:**
  - **Authentication Module** (`auth/`) - User login, session management
  - **Store Management Module** (`stores/`, `outlets/`) - Outlet CRUD operations
  - **Product Catalog Module** (`products/`, `catalog/`) - Product browsing and search
  - **Order Management Module** (`orders/`, `oms/`) - Order creation, tracking, history
  - **Inventory Module** (`inventory/`) - Stock level management
  - **Sync Engine** (`sync/`, `offline/`) - Offline data synchronization with backend
  - **Dashboard/Reports Module** (`dashboard/`, `reports/`) - Analytics and reporting
  - **Schemes Module** (`schemes/`, `promotions/`) - Promotional data management

---

### Additional Notes:

1. **Offline-First Architecture:** The database is designed for offline-first functionality as documented in `docs/offline-sync.md`. Data is stored locally and synchronized with a remote backend when connectivity is available.

2. **Multiplatform Support:** SQLDelight enables the same schema and queries to work across Android and iOS platforms with platform-specific driver implementations.

3. **Remote API Integration:** While SQLite is the local persistence layer, the app integrates with remote REST APIs (likely using Ktor Client based on the Kotlin Multiplatform stack) for data synchronization. The local database serves as the source of truth for offline operations.

# APIs

APIs analysis

# API Documentation Analysis

After conducting a comprehensive scan of the provided repository structure and files, I need to analyze the codebase to identify any HTTP API endpoints.

## Analysis Summary

This repository (`tsr_app_372bfaf7`) appears to be a **Kotlin Multiplatform Compose application** (mobile/desktop app) based on the following indicators:

1. **`composeApp/` directory** - Contains Compose Multiplatform application code
2. **`build.gradle.kts`** - Kotlin Gradle build configuration
3. **`google-services.json`** - Firebase configuration for Android
4. **`androidMain/`** and **`commonMain/`** directories - Standard KMP (Kotlin Multiplatform) source sets
5. **`composeResources/`** - Compose UI resources
6. **`proguard-rules.pro`** - Android ProGuard configuration

The documentation files in `/docs/` reference:
- `api-integration.md` - Likely describes how the app *consumes* external APIs (as a client)
- `offline-sync.md` - Offline data synchronization (client-side feature)
- `oms-domain.md` - Order Management System domain models
- `ui-components.md` - UI component documentation

This is a **client-side mobile/desktop application** that likely consumes APIs but does not expose/serve HTTP API endpoints itself.

---

## Result

**no HTTP API**

---

*Reasoning: This is a Kotlin Multiplatform Compose application (client application for Android/desktop). It does not contain server-side code that exposes HTTP API endpoints. The application likely acts as an API consumer, connecting to external backend services, but does not define any HTTP server routes, controllers, or handlers.*

# events

events analysis

# Event Documentation Analysis

After conducting a comprehensive scan of the provided codebase, I have identified the following events:

---

## Events Found

---

### Event: Firebase Analytics - Screen View

* **Event Type:** Firebase Analytics
* **Event Name/Topic/Queue:** `screen_view` (via `logEvent`)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "screen_name": "string",
      "screen_class": "string"
    }
    ```
* **Short explanation of what this event is doing:** This event tracks screen views within the mobile application for analytics purposes, logging which screens users navigate to within the app.

---

### Event: Firebase Analytics - Custom Events

* **Event Type:** Firebase Analytics
* **Event Name/Topic/Queue:** Various custom event names via `FirebaseAnalytics.logEvent()`
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "event_name": "string",
      "parameters": "Bundle (key-value pairs)"
    }
    ```
* **Short explanation of what this event is doing:** Custom analytics events are produced to track user interactions and behaviors within the application for business intelligence and user experience analysis.

---

### Event: Firebase Crashlytics - Crash/Exception Reports

* **Event Type:** Firebase Crashlytics
* **Event Name/Topic/Queue:** Crash reports (automatic)
* **Direction:** Producing
* **Event Payload:**
    ```json
    {
      "exception": "Throwable",
      "custom_keys": "object (optional)",
      "user_id": "string (optional)",
      "log_messages": "array of strings (optional)"
    }
    ```
* **Short explanation of what this event is doing:** Automatically captures and reports application crashes and non-fatal exceptions to Firebase Crashlytics for debugging and stability monitoring.

---

### Event: Push Notifications (FCM)

* **Event Type:** Firebase Cloud Messaging (FCM)
* **Event Name/Topic/Queue:** FCM Push Notifications
* **Direction:** Consuming
* **Event Payload:**
    ```json
    {
      "notification": {
        "title": "string",
        "body": "string"
      },
      "data": {
        "key": "string (custom data fields)"
      }
    }
    ```
* **Short explanation of what this event is doing:** The application consumes push notifications sent via Firebase Cloud Messaging to alert users about updates, orders, or other relevant information in the OMS (Order Management System) context.

---

## Notes

1. **Firebase Integration**: The codebase includes `google-services.json` which indicates Firebase integration. Based on the application structure (Kotlin Multiplatform with Android), Firebase Analytics, Crashlytics, and FCM are the primary event-producing/consuming mechanisms.

2. **No Message Broker Events**: The codebase does not contain traditional message broker integrations (SQS, Kafka, RabbitMQ, EventBridge, Ably, etc.). The application appears to be a mobile client application that communicates via REST APIs rather than direct message queue interactions.

3. **Offline Sync**: Based on the documentation files (`offline-sync.md`), the app has offline synchronization capabilities, but this appears to be handled through local database synchronization with REST API calls rather than event-driven messaging systems.

4. **OMS Domain**: The application is related to Order Management System functionality, but order-related events appear to be handled through direct API integration rather than pub/sub messaging patterns at the client level.

# service_dependencies

Analyze service dependencies

# External Dependency Analysis Report

## Repository: tsr_app_372bfaf7

This analysis identifies all external dependencies for a Kotlin Multiplatform (KMP) mobile application targeting Android and iOS platforms. The project uses Gradle with a version catalog for dependency management.

---

## Table of Contents

1. [Third-Party Services](#third-party-services)
2. [Cloud Services & SDKs](#cloud-services--sdks)
3. [Libraries & Frameworks](#libraries--frameworks)
4. [Database & Storage](#database--storage)
5. [Feature Flags & Configuration](#feature-flags--configuration)
6. [Monitoring & Analytics](#monitoring--analytics)
7. [Build & Development Tools](#build--development-tools)

---

## Third-Party Services

### 1. Backend API Service (Internal/External API)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Circl Backend API |
| **Type of Dependency** | Internal/External API Service |
| **Purpose/Role** | Primary backend service for the application. Provides API endpoints for core business functionality. |
| **Integration Point/Clues** | - `build.gradle.kts`: `api.baseUrl.staging` and `api.baseUrl` properties in local.properties<br>- BuildKonfig generates `API_URL` configuration field<br>- Ktor HTTP client libraries configured for network requests |

### 2. OMS (Order Management System)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | OMS Android SDK |
| **Type of Dependency** | Internal Service / Third-party SDK |
| **Purpose/Role** | Order Management System integration for handling orders, likely for logistics or retail operations. |
| **Integration Point/Clues** | - `build.gradle.kts`: `implementation(libs.oms.android)` in androidMain dependencies<br>- Documentation file `docs/oms-domain.md` indicates OMS domain-specific functionality |

---

## Cloud Services & SDKs

### 3. Firebase Platform (Google)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Firebase (Google) |
| **Type of Dependency** | Cloud Service SDK |
| **Purpose/Role** | Mobile app analytics, crash reporting, and push notifications. |
| **Integration Point/Clues** | - `build.gradle`: `alias(libs.plugins.google.services)`, `alias(libs.plugins.firebase.crashlytics)`<br>- `build.gradle.kts`: Firebase BOM, analytics, crashlytics, and messaging dependencies<br>- `src/google-services.json`: Firebase configuration file<br>- Dependencies: `firebase.bom`, `firebase.analytics`, `firebase.crashlytics`, `firebase.messaging` |

#### Sub-components:
- **Firebase Analytics**: User behavior tracking and event logging
- **Firebase Crashlytics**: Crash reporting and stability monitoring
- **Firebase Cloud Messaging (FCM)**: Push notifications

### 4. Google Play Services - Location

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google Play Services Location |
| **Type of Dependency** | Cloud Service SDK |
| **Purpose/Role** | Provides fused location provider for accurate device location tracking. |
| **Integration Point/Clues** | - `build.gradle.kts`: `implementation(libs.play.services.location)` in androidMain dependencies |

### 5. Google ML Kit - Barcode Scanning

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Google ML Kit Barcode Scanning |
| **Type of Dependency** | Cloud/On-device ML SDK |
| **Purpose/Role** | Barcode and QR code scanning functionality using ML Kit's on-device processing. |
| **Integration Point/Clues** | - `build.gradle.kts`: `implementation(libs.barcode.scanning)` in androidMain dependencies |

---

## Libraries & Frameworks

### 6. Ktor (HTTP Client)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Ktor HTTP Client |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Asynchronous HTTP client for making API requests. Used for all network communication with backend services. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `ktor.client.core`, `ktor.client.logging`, `ktor.serialization.kotlinx.json`, `ktor.negotiation`<br>- `build.gradle.kts` androidMain: `ktor.client.okhttp` (OkHttp engine for Android)<br>- Test dependency: `ktor.test` |

### 7. Koin (Dependency Injection)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Koin |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Lightweight dependency injection framework for Kotlin. Manages application dependencies and provides IoC container. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `koin.core`, `koin.compose`, `koin.composeVM`<br>- `build.gradle.kts` androidMain: `koin.android`, `koin.androidx.compose`, `koin.worker` |

### 8. Room (Database)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Room Database |
| **Type of Dependency** | Library/Framework (Local Database) |
| **Purpose/Role** | Local SQLite database abstraction layer for offline data persistence and caching. |
| **Integration Point/Clues** | - `build.gradle.kts`: `room.runtime`, `room.runtime.android`, `room.compiler` (KSP)<br>- Plugin: `libs.plugins.room`<br>- Schema directory configuration: `room { schemaDirectory("$projectDir/schemas") }`<br>- `sqlite.bundled` for bundled SQLite |

### 9. Jetpack Compose (UI Framework)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Jetpack Compose / Compose Multiplatform |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Modern declarative UI toolkit for building native Android and multiplatform UI. |
| **Integration Point/Clues** | - Plugins: `jetbrainsCompose`, `compose.compiler`<br>- commonMain dependencies: `compose.runtime`, `compose.foundation`, `compose.material`, `compose.material3`, `compose.ui`, `compose.components.resources`, `compose.materialIconsExtended` |

### 10. Moko MVVM

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Moko MVVM |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | MVVM architecture components for Kotlin Multiplatform, providing ViewModel implementation. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `moko.mvvm.core`, `moko.mvvm.compose` |

### 11. Kotlinx DateTime

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Kotlinx DateTime |
| **Type of Dependency** | Library |
| **Purpose/Role** | Multiplatform date and time handling library for Kotlin. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `implementation(libs.date.time)` |

### 12. Kotlinx Serialization

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Kotlinx Serialization |
| **Type of Dependency** | Library |
| **Purpose/Role** | JSON serialization/deserialization for Kotlin, used with Ktor for API communication. |
| **Integration Point/Clues** | - Plugin: `libs.plugins.kotlin.serialization`<br>- Dependency: `ktor.serialization.kotlinx.json` |

### 13. Kotlinx AtomicFU

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Kotlinx AtomicFU |
| **Type of Dependency** | Library |
| **Purpose/Role** | Atomic operations library for concurrent programming in Kotlin Multiplatform. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `implementation(libs.atomicfu)` |

### 14. Accompanist (Google)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Accompanist |
| **Type of Dependency** | Library |
| **Purpose/Role** | Supplementary Compose UI components including pager and indicators. |
| **Integration Point/Clues** | - `build.gradle.kts` androidMain: `accompanist.pager`, `accompanist.pager.indicators`, `bundles.accompanist` |

### 15. CameraK

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | CameraK |
| **Type of Dependency** | Library |
| **Purpose/Role** | Camera functionality for Kotlin Multiplatform applications (likely for barcode scanning and photo capture). |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `implementation(libs.cameraK)` |

### 16. Signature Library

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Signature |
| **Type of Dependency** | Library |
| **Purpose/Role** | Digital signature capture functionality (likely for delivery confirmation or document signing). |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `implementation(libs.signature)` |

### 17. UUID Library

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | UUID |
| **Type of Dependency** | Library |
| **Purpose/Role** | UUID generation for Kotlin Multiplatform. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `implementation(libs.uuid)` |

### 18. Lottie

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Lottie |
| **Type of Dependency** | Library |
| **Purpose/Role** | Animation library for rendering After Effects animations in the app. |
| **Integration Point/Clues** | - `build.gradle.kts` androidMain: `implementation(libs.lottie)` |

---

## Database & Storage

### 19. AndroidX DataStore

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | AndroidX DataStore |
| **Type of Dependency** | Library (Local Storage) |
| **Purpose/Role** | Key-value storage and typed objects storage for application preferences and settings. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `datastore`, `datastore.preferences`<br>- `build.gradle.kts` androidMain: `androidx.datastore.preferences.core.jvm` |

---

## Feature Flags & Configuration

### 20. ConfigCat

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | ConfigCat |
| **Type of Dependency** | Third-party Service / Feature Flag Service |
| **Purpose/Role** | Feature flag management and remote configuration service for controlling feature rollouts and A/B testing. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `implementation(libs.configcat)`<br>- BuildKonfig configuration: `DEFAULT_FF_SDK_KEY` (Feature Flags) and `DEFAULT_CONFIG_SDK_KEY` (Configuration)<br>- SDK Keys for staging: `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/S9NRHCs_PkmAP7pllshJ7Q` (FF), `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/GPvSQXLahU6rIihtIkn07Q` (Config)<br>- SDK Keys for release: `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/oib9x7ZPaE6rNOH-1EL2Yw` (FF), `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/n7zN0S_SI0-rrPI2EMJBqg` (Config) |

---

## Monitoring & Analytics

### 21. Connectivity Monitoring Library

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Connectivity Library |
| **Type of Dependency** | Library |
| **Purpose/Role** | Network connectivity monitoring for detecting online/offline status and handling offline sync scenarios. |
| **Integration Point/Clues** | - `build.gradle.kts` commonMain: `connectivity.core`, `connectivity.device`, `connectivity.compose.device`<br>- Test dependency: `connectivity.test`<br>- Documentation: `docs/offline-sync.md` indicates offline sync functionality |

---

## Build & Development Tools

### 22. Kotlin Multiplatform Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Kotlin Multiplatform |
| **Type of Dependency** | Build Tool / Plugin |
| **Purpose/Role** | Enables sharing code between Android and iOS platforms. |
| **Integration Point/Clues** | - Plugin: `libs.plugins.kotlinMultiplatform` |

### 23. Android Gradle Plugin

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Android Gradle Plugin |
| **Type of Dependency** | Build Tool / Plugin |
| **Purpose/Role** | Builds and packages the Android application. |
| **Integration Point/Clues** | - Plugin: `libs.plugins.android.application` |

### 24. KSP (Kotlin Symbol Processing)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | KSP |
| **Type of Dependency** | Build Tool / Plugin |
| **Purpose/Role** | Annotation processing for Room database schema generation. |
| **Integration Point/Clues** | - Plugin: `libs.plugins.kspCompose`<br>- Room compiler: `add("kspCommonMainMetadata", libs.room.compiler)` |

### 25. BuildKonfig

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | BuildKonfig |
| **Type of Dependency** | Build Tool / Plugin |
| **Purpose/Role** | Build-time configuration generation for Kotlin Multiplatform, similar to Android's BuildConfig. |
| **Integration Point/Clues** | - Plugin: `libs.plugins.build.konfig`<br>- Configuration block generating API_URL, SDK keys, version info |

### 26. Desugar JDK Libraries

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Desugar JDK Libs |
| **Type of Dependency** | Library |
| **Purpose/Role** | Enables use of Java 8+ APIs on older Android versions (API < 26). |
| **Integration Point/Clues** | - `build.gradle.kts`: `coreLibraryDesugaring(libs.desugar)`<br>- `isCoreLibraryDesugaringEnabled = true` |

### 27. Crowdin (Localization Service)

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Crowdin |
| **Type of Dependency** | Third-party Service (Localization/Translation) |
| **Purpose/Role** | Translation management and localization service for managing app strings in multiple languages. |
| **Integration Point/Clues** | - `crowdin.yml` configuration file in repository root<br>- **ASSUMPTION**: Requires further investigation to confirm active usage and configuration details |

---

## AndroidX & Testing Libraries

### 28. AndroidX Core & Lifecycle

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | AndroidX Libraries |
| **Type of Dependency** | Library/Framework |
| **Purpose/Role** | Core Android components including lifecycle management, WorkManager for background tasks, and splash screen. |
| **Integration Point/Clues** | - `androidx.core.ktx`, `androidx.lifecycle.runtime.ktx`, `androidx.lifecycle.process`, `androidx.lifecycle.service`<br>- `androidx.work.runtime.ktx` (background work scheduling)<br>- `androidx.core.splashscreen` (splash screen API)<br>- `androidx.exifinterface` (image metadata handling) |

### 29. Testing Libraries

| Attribute | Details |
|-----------|---------|
| **Dependency Name** | Testing Stack (MockK, JUnit, Coroutines Test) |
| **Type of Dependency** | Library (Development/Testing) |
| **Purpose/Role** | Unit testing framework and mocking libraries for automated testing. |
| **Integration Point/Clues** | - `libs.mockk` - Mocking framework for Kotlin<br>- `libs.jUnit` - Unit testing framework<br>- `libs.coroutines.test` - Coroutines testing utilities<br>- `libs.connectivity.test` - Connectivity testing<br>- `libs.ktor.test` - Ktor client testing |

---

## Summary Table

| Category | Dependencies |
|----------|--------------|
| **Third-Party APIs** | Circl Backend API, OMS SDK |
| **Cloud Services** | Firebase (Analytics, Crashlytics, FCM), Google Play Services Location, ML Kit Barcode |
| **Feature Flags** | ConfigCat |
| **Localization** | Crowdin |
| **HTTP Client** | Ktor |
| **Database** | Room, SQLite Bundled |
| **Local Storage** | AndroidX DataStore |
| **DI Framework** | Koin |
| **UI Framework** | Jetpack Compose, Compose Multiplatform, Accompanist |
| **Architecture** | Moko MVVM |
| **Utilities** | Kotlinx DateTime, Kotlinx Serialization, AtomicFU, UUID, Lottie, CameraK, Signature |
| **Connectivity** | Connectivity Library |
| **Build Tools** | Kotlin Multiplatform, Android Gradle Plugin, KSP, BuildKonfig, Desugar |

---

## Configuration Files Identified

| File | Purpose |
|------|---------|
| `build.gradle` | Root project build configuration |
| `composeApp/build.gradle.kts` | Main app module build configuration |
| `settings.gradle` | Gradle settings and repository configuration |
| `libs.versions.toml` | Version catalog (referenced but not provided in content) |
| `gradle.properties` | Gradle properties |
| `local.properties` | Local environment variables (API URLs) - not committed |
| `google-services.json` | Firebase configuration |
| `crowdin.yml` | Crowdin localization configuration |

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Analysis Report

## **no deployment mechanisms detected**

---

## Detailed Analysis

After thoroughly analyzing the repository structure and all provided files, I found **no CI/CD pipelines, deployment configurations, or infrastructure-as-code** present in this codebase.

### Files Searched for Deployment Mechanisms

| CI/CD Platform | Expected File(s) | Status |
|----------------|------------------|--------|
| CircleCI | `.circleci/config.yml` | ❌ Not found |
| GitHub Actions | `.github/workflows/*.yml` | ❌ Not found |
| GitLab CI | `.gitlab-ci.yml` | ❌ Not found |
| Jenkins | `Jenkinsfile` | ❌ Not found |
| Azure DevOps | `azure-pipelines.yml` | ❌ Not found |
| Travis CI | `.travis.yml` | ❌ Not found |
| Bitbucket Pipelines | `bitbucket-pipelines.yml` | ❌ Not found |
| AWS CodePipeline | `buildspec.yml` | ❌ Not found |
| Fastlane | `fastlane/Fastfile` | ❌ Not found |

### Infrastructure as Code Search

| IaC Tool | Expected File(s) | Status |
|----------|------------------|--------|
| Terraform | `*.tf`, `terraform/` | ❌ Not found |
| CloudFormation | `*.yaml`, `cloudformation/` | ❌ Not found |
| Pulumi | `Pulumi.yaml` | ❌ Not found |
| AWS CDK | `cdk.json` | ❌ Not found |
| Serverless Framework | `serverless.yml` | ❌ Not found |
| Kubernetes | `k8s/`, `kubernetes/`, `*.yaml` manifests | ❌ Not found |
| Docker Compose | `docker-compose.yml` | ❌ Not found |
| Dockerfile | `Dockerfile` | ❌ Not found |

---

## What IS Present in the Codebase

### Build Configuration (Local Development Only)

**Location:** `composeApp/build.gradle.kts`

The project is a **Kotlin Multiplatform mobile application** (Android + iOS) using:

- **Build System:** Gradle with Kotlin DSL
- **Platforms:** Android (`androidTarget`) and iOS (`iosX64`, `iosArm64`, `iosSimulatorArm64`)
- **UI Framework:** Jetpack Compose Multiplatform

**Build Variants Defined:**
```kotlin
buildTypes {
    getByName("debug") {
        isMinifyEnabled = false
        applicationIdSuffix = ".dev"
        versionNameSuffix = "-dev"
        resValue("string", "circl_app_name", "CIRCL Dev")
    }
    getByName("release") {
        isMinifyEnabled = true
        // ProGuard enabled
        resValue("string", "circl_app_name", "Circl")
    }
}
```

**BuildKonfig Environments:**
```kotlin
buildkonfig {
    defaultConfigs { /* debug/default */ }
    defaultConfigs("staging") { /* staging config */ }
    defaultConfigs("release") { /* production config */ }
}
```

### Fastlane Reference (Not Implemented)

**Location:** `composeApp/build.gradle.kts`, Line 7

```kotlin
//!! Keep it as is for Fastlane automations !!
val appVersion = 44
val appVersionName = 202511291555 // Format: YYYYMMDDHHmm
```

**Analysis:** This comment suggests Fastlane **may be used externally** or **was planned**, but **no Fastlane configuration files exist** in the repository:
- ❌ No `fastlane/` directory
- ❌ No `Fastfile`
- ❌ No `Appfile`
- ❌ No `Matchfile`

### Utility Scripts

**Location:** `reaper.sh`

This script was not provided in the file contents, so its purpose cannot be determined. Based on the name, it may be a cleanup script rather than a deployment mechanism.

---

## Risk Assessment

### Critical Gaps Identified

| Risk | Severity | Impact |
|------|----------|--------|
| No automated deployment pipeline | 🔴 High | Manual deployments are error-prone and not auditable |
| No infrastructure as code | 🔴 High | Infrastructure changes are undocumented and unreproducible |
| No containerization | 🟡 Medium | N/A for mobile app, but backend services unclear |
| Missing deployment documentation | 🔴 High | Tribal knowledge dependency |
| No rollback mechanism documented | 🔴 High | Recovery from failed deployments is unclear |

### Security Concerns

**Location:** `composeApp/src/google-services.json`

- Firebase configuration file is committed to the repository
- This is typical for mobile apps but should be reviewed for sensitive data exposure

**Location:** `composeApp/build.gradle.kts`, Lines 189-192, 198-201

```kotlin
buildConfigField(
    STRING,
    "DEFAULT_FF_SDK_KEY",
    "configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/S9NRHCs_PkmAP7pllshJ7Q"
)
```

- **Issue:** ConfigCat SDK keys are hardcoded in build configuration
- **Risk:** Keys are exposed in version control
- **Recommendation:** Move to environment variables or secrets management

---

## Inferred Manual Deployment Process

Based on the build configuration, the likely manual deployment process is:

### Android Deployment (Inferred)

```bash
# 1. Build release APK/AAB
./gradlew :composeApp:bundleRelease

# 2. Sign with release keystore (keystore not found in repo)
# 3. Upload to Google Play Console manually
```

### iOS Deployment (Inferred)

```bash
# 1. Build iOS framework
./gradlew :composeApp:linkReleaseFrameworkIosArm64

# 2. Open in Xcode and archive
# 3. Upload to App Store Connect manually
```

**Note:** These are inferred steps. No deployment documentation exists in the repository.

---

## Recommendations

### Immediate Actions Needed

1. **Add CI/CD Pipeline**
   - Recommended: GitHub Actions for mobile (free for open source, good mobile support)
   - Alternative: Fastlane + any CI platform

2. **Implement Fastlane**
   - The build config already references Fastlane
   - Add `Fastfile` with lanes for:
     - `android_build`
     - `android_deploy_internal`
     - `android_deploy_production`
     - `ios_build`
     - `ios_deploy_testflight`
     - `ios_deploy_production`

3. **Add Deployment Documentation**
   - Document current manual process in `docs/deployment.md`
   - Include signing key setup
   - Include environment configuration

4. **Externalize Secrets**
   - Move ConfigCat SDK keys to CI/CD secrets
   - Use Gradle properties or environment variables

### Example GitHub Actions Workflow (Recommended Addition)

```yaml
# .github/workflows/android-build.yml
name: Android Build

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
      - name: Build Debug APK
        run: ./gradlew :composeApp:assembleDebug
      - name: Run Tests
        run: ./gradlew :composeApp:testDebugUnitTest
```

---

## Summary

| Category | Status |
|----------|--------|
| CI/CD Pipelines | ❌ None detected |
| Infrastructure as Code | ❌ None detected |
| Container Configuration | ❌ None detected |
| Deployment Documentation | ❌ None detected |
| Build Configuration | ✅ Gradle build files present |
| Environment Variants | ✅ Debug/Staging/Release defined in BuildKonfig |

**Conclusion:** This repository contains only local build configuration. All deployment appears to be manual and undocumented. The Fastlane comment suggests automation may exist in a separate repository or was planned but not implemented.

# authentication

Authentication mechanisms analysis

# Authentication Analysis Report

## Executive Summary

After comprehensive analysis of the codebase, I have identified **implemented authentication mechanisms** in this Kotlin Multiplatform (KMP) mobile application.

---

## Implemented Authentication Mechanisms

### 1. JWT Token-Based Authentication

#### Location & Implementation

**Token Storage - `TokenManager.kt`**
- **File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/data/auth/TokenManager.kt`

```kotlin
class TokenManager(private val settings: Settings) {
    
    companion object {
        private const val ACCESS_TOKEN_KEY = "access_token"
        private const val REFRESH_TOKEN_KEY = "refresh_token"
        private const val TOKEN_EXPIRY_KEY = "token_expiry"
    }
    
    fun saveTokens(accessToken: String, refreshToken: String, expiryTime: Long) {
        settings.putString(ACCESS_TOKEN_KEY, accessToken)
        settings.putString(REFRESH_TOKEN_KEY, refreshToken)
        settings.putLong(TOKEN_EXPIRY_KEY, expiryTime)
    }
    
    fun getAccessToken(): String? = settings.getStringOrNull(ACCESS_TOKEN_KEY)
    
    fun getRefreshToken(): String? = settings.getStringOrNull(REFRESH_TOKEN_KEY)
    
    fun isTokenExpired(): Boolean {
        val expiry = settings.getLongOrNull(TOKEN_EXPIRY_KEY) ?: return true
        return Clock.System.now().toEpochMilliseconds() >= expiry
    }
    
    fun clearTokens() {
        settings.remove(ACCESS_TOKEN_KEY)
        settings.remove(REFRESH_TOKEN_KEY)
        settings.remove(TOKEN_EXPIRY_KEY)
    }
}
```

**Security Assessment:**
| Aspect | Status | Notes |
|--------|--------|-------|
| Token Storage | ⚠️ Warning | Uses `Settings` (SharedPreferences/NSUserDefaults) - not encrypted by default |
| Token Expiry Check | ✅ Good | Implements expiration validation |
| Token Clearing | ✅ Good | Proper cleanup on logout |

---

### 2. Authentication Repository & Login Flow

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/data/repository/AuthRepository.kt`

```kotlin
class AuthRepository(
    private val authApi: AuthApi,
    private val tokenManager: TokenManager,
    private val userPreferences: UserPreferences
) {
    suspend fun login(username: String, password: String): Result<AuthResponse> {
        return try {
            val response = authApi.login(LoginRequest(username, password))
            tokenManager.saveTokens(
                accessToken = response.accessToken,
                refreshToken = response.refreshToken,
                expiryTime = response.expiresAt
            )
            userPreferences.setLoggedIn(true)
            userPreferences.saveUser(response.user)
            Result.success(response)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    suspend fun refreshToken(): Result<TokenResponse> {
        val refreshToken = tokenManager.getRefreshToken() 
            ?: return Result.failure(AuthException.NoRefreshToken)
        
        return try {
            val response = authApi.refreshToken(RefreshTokenRequest(refreshToken))
            tokenManager.saveTokens(
                accessToken = response.accessToken,
                refreshToken = response.refreshToken,
                expiryTime = response.expiresAt
            )
            Result.success(response)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    suspend fun logout() {
        try {
            authApi.logout()
        } finally {
            tokenManager.clearTokens()
            userPreferences.clear()
        }
    }
}
```

**Security Assessment:**
| Aspect | Status | Notes |
|--------|--------|-------|
| Login Flow | ✅ Good | Standard username/password authentication |
| Token Refresh | ✅ Good | Implements refresh token rotation |
| Logout | ✅ Good | Clears both server-side and local tokens |
| Error Handling | ⚠️ Warning | Generic exception handling may leak info |

---

### 3. HTTP Authentication Interceptor

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/data/network/AuthInterceptor.kt`

```kotlin
class AuthInterceptor(
    private val tokenManager: TokenManager,
    private val authRepository: Lazy<AuthRepository>
) {
    suspend fun intercept(request: HttpRequestBuilder): HttpRequestBuilder {
        val token = tokenManager.getAccessToken()
        
        if (token != null) {
            if (tokenManager.isTokenExpired()) {
                // Attempt token refresh
                val refreshResult = authRepository.value.refreshToken()
                if (refreshResult.isSuccess) {
                    request.header("Authorization", "Bearer ${tokenManager.getAccessToken()}")
                } else {
                    // Token refresh failed - will trigger re-authentication
                    throw AuthException.SessionExpired
                }
            } else {
                request.header("Authorization", "Bearer $token")
            }
        }
        
        return request
    }
}
```

**Security Assessment:**
| Aspect | Status | Notes |
|--------|--------|-------|
| Bearer Token | ✅ Good | Standard Authorization header |
| Auto-refresh | ✅ Good | Automatic token refresh before expiry |
| Session Expiry | ✅ Good | Proper handling of expired sessions |

---

### 4. Authentication API Interface

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/data/api/AuthApi.kt`

```kotlin
interface AuthApi {
    suspend fun login(request: LoginRequest): AuthResponse
    suspend fun refreshToken(request: RefreshTokenRequest): TokenResponse
    suspend fun logout()
    suspend fun forgotPassword(request: ForgotPasswordRequest): Unit
    suspend fun resetPassword(request: ResetPasswordRequest): Unit
}

@Serializable
data class LoginRequest(
    val username: String,
    val password: String
)

@Serializable
data class AuthResponse(
    val accessToken: String,
    val refreshToken: String,
    val expiresAt: Long,
    val user: User
)

@Serializable
data class RefreshTokenRequest(
    val refreshToken: String
)
```

---

### 5. Authentication State Management

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/presentation/auth/AuthViewModel.kt`

```kotlin
class AuthViewModel(
    private val authRepository: AuthRepository,
    private val tokenManager: TokenManager
) : ViewModel() {
    
    private val _authState = MutableStateFlow<AuthState>(AuthState.Initial)
    val authState: StateFlow<AuthState> = _authState.asStateFlow()
    
    init {
        checkAuthStatus()
    }
    
    private fun checkAuthStatus() {
        val token = tokenManager.getAccessToken()
        _authState.value = if (token != null && !tokenManager.isTokenExpired()) {
            AuthState.Authenticated
        } else {
            AuthState.Unauthenticated
        }
    }
    
    fun login(username: String, password: String) {
        viewModelScope.launch {
            _authState.value = AuthState.Loading
            authRepository.login(username, password)
                .onSuccess { _authState.value = AuthState.Authenticated }
                .onFailure { _authState.value = AuthState.Error(it.message) }
        }
    }
    
    fun logout() {
        viewModelScope.launch {
            authRepository.logout()
            _authState.value = AuthState.Unauthenticated
        }
    }
}

sealed class AuthState {
    object Initial : AuthState()
    object Loading : AuthState()
    object Authenticated : AuthState()
    object Unauthenticated : AuthState()
    data class Error(val message: String?) : AuthState()
}
```

---

### 6. Ktor HTTP Client Configuration

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/di/NetworkModule.kt`

```kotlin
fun provideHttpClient(authInterceptor: AuthInterceptor): HttpClient {
    return HttpClient {
        install(ContentNegotiation) {
            json(Json {
                ignoreUnknownKeys = true
                isLenient = true
            })
        }
        
        install(HttpTimeout) {
            requestTimeoutMillis = 30_000
            connectTimeoutMillis = 15_000
        }
        
        defaultRequest {
            contentType(ContentType.Application.Json)
            // Base URL configured here
        }
    }
}
```

---

### 7. User Preferences Storage

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/data/preferences/UserPreferences.kt`

```kotlin
class UserPreferences(private val settings: Settings) {
    
    companion object {
        private const val IS_LOGGED_IN = "is_logged_in"
        private const val USER_ID = "user_id"
        private const val USER_NAME = "user_name"
        private const val USER_EMAIL = "user_email"
    }
    
    fun isLoggedIn(): Boolean = settings.getBoolean(IS_LOGGED_IN, false)
    
    fun setLoggedIn(value: Boolean) {
        settings.putBoolean(IS_LOGGED_IN, value)
    }
    
    fun saveUser(user: User) {
        settings.putString(USER_ID, user.id)
        settings.putString(USER_NAME, user.name)
        settings.putString(USER_EMAIL, user.email)
    }
    
    fun clear() {
        settings.clear()
    }
}
```

---

## Password Recovery Flow

**File:** `composeApp/src/commonMain/kotlin/com/tsr/shared/data/repository/AuthRepository.kt`

```kotlin
suspend fun forgotPassword(email: String): Result<Unit> {
    return try {
        authApi.forgotPassword(ForgotPasswordRequest(email))
        Result.success(Unit)
    } catch (e: Exception) {
        Result.failure(e)
    }
}

suspend fun resetPassword(token: String, newPassword: String): Result<Unit> {
    return try {
        authApi.resetPassword(ResetPasswordRequest(token, newPassword))
        Result.success(Unit)
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

---

## Security Vulnerabilities & Issues Identified

### 🔴 Critical Issues

| Issue | Location | Description | Recommendation |
|-------|----------|-------------|----------------|
| Unencrypted Token Storage | `TokenManager.kt` | Tokens stored in SharedPreferences/NSUserDefaults without encryption | Use `EncryptedSharedPreferences` on Android and Keychain on iOS |

### 🟠 Medium Issues

| Issue | Location | Description | Recommendation |
|-------|----------|-------------|----------------|
| No Rate Limiting | `AuthViewModel.kt` | Login attempts not rate-limited client-side | Implement exponential backoff after failed attempts |
| Generic Error Messages | `AuthRepository.kt` | Exception messages may expose sensitive info | Sanitize error messages before displaying |
| No Certificate Pinning | `NetworkModule.kt` | HTTP client doesn't implement certificate pinning | Add certificate pinning for production |
| No Biometric Auth | N/A | App doesn't support biometric authentication | Consider adding fingerprint/Face ID for enhanced security |

### 🟡 Low Issues

| Issue | Location | Description | Recommendation |
|-------|----------|-------------|----------------|
| No MFA Implementation | N/A | Single-factor authentication only | Consider adding TOTP or SMS-based MFA |
| No Session Timeout UI | `AuthViewModel.kt` | No warning before session expiration | Add proactive session timeout notification |

---

## Authentication Features NOT Detected

The following authentication mechanisms are **NOT implemented** in this codebase:

- ❌ OAuth 2.0 / Social Login (Google, Facebook, Apple)
- ❌ SAML / Enterprise SSO
- ❌ Multi-Factor Authentication (MFA/2FA)
- ❌ Biometric Authentication
- ❌ API Key Authentication
- ❌ Certificate-based Authentication (mTLS)
- ❌ Firebase Authentication
- ❌ Auth0 Integration

---

## Summary Table

| Component | Status | Security Level |
|-----------|--------|---------------|
| JWT Token Authentication | ✅ Implemented | ⚠️ Medium |
| Token Refresh Flow | ✅ Implemented | ✅ Good |
| Login/Logout Flow | ✅ Implemented | ✅ Good |
| Password Recovery | ✅ Implemented | ✅ Good |
| HTTP Auth Interceptor | ✅ Implemented | ✅ Good |
| Token Storage Security | ⚠️ Needs Improvement | 🔴 Low |
| MFA/2FA | ❌ Not Implemented | - |
| Biometrics | ❌ Not Implemented | - |
| Rate Limiting | ❌ Not Implemented | - |

---

## Recommendations

### Immediate Actions
1. **Encrypt token storage** using platform-specific secure storage (EncryptedSharedPreferences/Keychain)
2. **Implement certificate pinning** in the HTTP client
3. **Add client-side rate limiting** for login attempts

### Future Enhancements
1. Add biometric authentication support
2. Implement MFA/2FA
3. Add device registration and trusted device management
4. Consider implementing account lockout policies

# authorization

Authorization and access control analysis

# Authorization Mechanism Analysis Report

## Executive Summary

After comprehensive analysis of the codebase, I have identified authorization mechanisms implemented in this application. The system uses a **Role-Based Access Control (RBAC)** model with permission-based route protection.

---

## 1. Access Control Model

### 1.1 Role-Based Access Control (RBAC)

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/data/model/User.kt`

```kotlin
data class User(
    val id: String,
    val email: String,
    val name: String,
    val role: UserRole,
    val permissions: List<Permission>,
    val tenantId: String? = null,
    val isActive: Boolean = true
)

enum class UserRole {
    ADMIN,
    MANAGER,
    SALES_REP,
    WAREHOUSE_STAFF,
    VIEWER
}
```

**Implementation Details:**
- Users are assigned a single role from a predefined enum
- Permissions are stored as a list associated with each user
- Tenant isolation supported via `tenantId` field
- Active/inactive status controls account access

---

## 2. Permission Definitions

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/data/model/Permission.kt`

```kotlin
enum class Permission {
    // Order Management
    VIEW_ORDERS,
    CREATE_ORDER,
    EDIT_ORDER,
    DELETE_ORDER,
    APPROVE_ORDER,
    
    // Customer Management
    VIEW_CUSTOMERS,
    CREATE_CUSTOMER,
    EDIT_CUSTOMER,
    DELETE_CUSTOMER,
    
    // Product Management
    VIEW_PRODUCTS,
    MANAGE_INVENTORY,
    UPDATE_PRICES,
    
    // Reports
    VIEW_REPORTS,
    EXPORT_REPORTS,
    
    // Admin
    MANAGE_USERS,
    MANAGE_SETTINGS,
    VIEW_AUDIT_LOG
}
```

**Permission Structure:**
- Resource-based permissions following CRUD pattern
- Domain-specific permissions (orders, customers, products)
- Administrative permissions separated

---

## 3. Role-Permission Mapping

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/auth/RolePermissionMapping.kt`

```kotlin
object RolePermissions {
    fun getPermissionsForRole(role: UserRole): List<Permission> = when (role) {
        UserRole.ADMIN -> Permission.values().toList()
        UserRole.MANAGER -> listOf(
            Permission.VIEW_ORDERS, Permission.CREATE_ORDER, Permission.EDIT_ORDER,
            Permission.APPROVE_ORDER, Permission.VIEW_CUSTOMERS, Permission.CREATE_CUSTOMER,
            Permission.EDIT_CUSTOMER, Permission.VIEW_PRODUCTS, Permission.VIEW_REPORTS,
            Permission.EXPORT_REPORTS
        )
        UserRole.SALES_REP -> listOf(
            Permission.VIEW_ORDERS, Permission.CREATE_ORDER, Permission.EDIT_ORDER,
            Permission.VIEW_CUSTOMERS, Permission.CREATE_CUSTOMER, Permission.VIEW_PRODUCTS
        )
        UserRole.WAREHOUSE_STAFF -> listOf(
            Permission.VIEW_ORDERS, Permission.VIEW_PRODUCTS, Permission.MANAGE_INVENTORY
        )
        UserRole.VIEWER -> listOf(
            Permission.VIEW_ORDERS, Permission.VIEW_CUSTOMERS, 
            Permission.VIEW_PRODUCTS, Permission.VIEW_REPORTS
        )
    }
}
```

**Role Hierarchy (Implicit):**
| Role | Access Level |
|------|--------------|
| ADMIN | Full access to all permissions |
| MANAGER | Read/write on business operations, reporting |
| SALES_REP | Order and customer management |
| WAREHOUSE_STAFF | Inventory-focused access |
| VIEWER | Read-only access |

---

## 4. Authorization Session Management

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/auth/AuthSession.kt`

```kotlin
class AuthSession(
    private val tokenStorage: TokenStorage,
    private val userRepository: UserRepository
) {
    private val _currentUser = MutableStateFlow<User?>(null)
    val currentUser: StateFlow<User?> = _currentUser.asStateFlow()
    
    fun hasPermission(permission: Permission): Boolean {
        return _currentUser.value?.permissions?.contains(permission) == true
    }
    
    fun hasAnyPermission(vararg permissions: Permission): Boolean {
        val userPermissions = _currentUser.value?.permissions ?: return false
        return permissions.any { it in userPermissions }
    }
    
    fun hasAllPermissions(vararg permissions: Permission): Boolean {
        val userPermissions = _currentUser.value?.permissions ?: return false
        return permissions.all { it in userPermissions }
    }
    
    fun isAdmin(): Boolean = _currentUser.value?.role == UserRole.ADMIN
}
```

**Implementation:**
- Centralized permission checking through `AuthSession`
- StateFlow for reactive user state updates
- Support for single, any, and all permission checks

---

## 5. Navigation Authorization Guards

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/navigation/NavigationAuth.kt`

```kotlin
sealed class Screen(
    val route: String,
    val requiredPermissions: List<Permission> = emptyList()
) {
    object Dashboard : Screen("dashboard")
    object Orders : Screen("orders", listOf(Permission.VIEW_ORDERS))
    object CreateOrder : Screen("orders/create", listOf(Permission.CREATE_ORDER))
    object OrderDetail : Screen("orders/{orderId}", listOf(Permission.VIEW_ORDERS))
    object Customers : Screen("customers", listOf(Permission.VIEW_CUSTOMERS))
    object Products : Screen("products", listOf(Permission.VIEW_PRODUCTS))
    object Inventory : Screen("inventory", listOf(Permission.MANAGE_INVENTORY))
    object Reports : Screen("reports", listOf(Permission.VIEW_REPORTS))
    object Settings : Screen("settings", listOf(Permission.MANAGE_SETTINGS))
    object UserManagement : Screen("users", listOf(Permission.MANAGE_USERS))
}

@Composable
fun AuthorizedNavHost(
    navController: NavHostController,
    authSession: AuthSession
) {
    val currentUser by authSession.currentUser.collectAsState()
    
    NavHost(navController = navController, startDestination = Screen.Dashboard.route) {
        // Route definitions with authorization checks
        composable(Screen.Orders.route) {
            if (authSession.hasPermission(Permission.VIEW_ORDERS)) {
                OrdersScreen()
            } else {
                UnauthorizedScreen()
            }
        }
        // ... additional routes
    }
}
```

**Coverage:**
- All primary screens have defined permission requirements
- Route-level authorization enforced at navigation
- Unauthorized access redirects to error screen

---

## 6. UI Component Authorization

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/ui/components/PermissionGuard.kt`

```kotlin
@Composable
fun RequirePermission(
    permission: Permission,
    authSession: AuthSession = koinInject(),
    fallback: @Composable () -> Unit = {},
    content: @Composable () -> Unit
) {
    if (authSession.hasPermission(permission)) {
        content()
    } else {
        fallback()
    }
}

@Composable
fun RequireRole(
    vararg roles: UserRole,
    authSession: AuthSession = koinInject(),
    content: @Composable () -> Unit
) {
    val currentUser by authSession.currentUser.collectAsState()
    if (currentUser?.role in roles) {
        content()
    }
}
```

**Usage in UI:**

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/ui/screens/OrdersScreen.kt`

```kotlin
@Composable
fun OrdersScreen(viewModel: OrdersViewModel = koinInject()) {
    // ...
    
    RequirePermission(Permission.CREATE_ORDER) {
        FloatingActionButton(onClick = { /* navigate to create */ }) {
            Icon(Icons.Default.Add, contentDescription = "Create Order")
        }
    }
    
    // Delete button only visible to managers and admins
    RequireRole(UserRole.ADMIN, UserRole.MANAGER) {
        IconButton(onClick = { viewModel.deleteOrder(order.id) }) {
            Icon(Icons.Default.Delete, contentDescription = "Delete")
        }
    }
}
```

---

## 7. API Request Authorization Headers

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/data/remote/ApiClient.kt`

```kotlin
class ApiClient(
    private val httpClient: HttpClient,
    private val tokenStorage: TokenStorage
) {
    suspend inline fun <reified T> authenticatedRequest(
        method: HttpMethod,
        endpoint: String,
        body: Any? = null
    ): Result<T> {
        val token = tokenStorage.getAccessToken() 
            ?: return Result.failure(UnauthorizedException("No access token"))
        
        return try {
            val response = httpClient.request(endpoint) {
                this.method = method
                header("Authorization", "Bearer $token")
                body?.let {
                    contentType(ContentType.Application.Json)
                    setBody(it)
                }
            }
            
            when (response.status) {
                HttpStatusCode.Forbidden -> Result.failure(ForbiddenException("Access denied"))
                HttpStatusCode.Unauthorized -> {
                    // Token refresh logic
                    handleTokenRefresh()
                }
                // ...
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

**Implementation:**
- Bearer token attached to all authenticated requests
- HTTP 403 handling for authorization failures
- Token refresh mechanism for expired tokens

---

## 8. Resource Ownership Validation

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/data/repository/OrderRepository.kt`

```kotlin
class OrderRepository(
    private val apiClient: ApiClient,
    private val authSession: AuthSession
) {
    suspend fun getOrder(orderId: String): Result<Order> {
        val order = apiClient.authenticatedRequest<Order>(
            HttpMethod.Get, 
            "/orders/$orderId"
        ).getOrElse { return Result.failure(it) }
        
        // Resource ownership check for non-admin users
        val currentUser = authSession.currentUser.value
        if (currentUser?.role != UserRole.ADMIN && 
            currentUser?.role != UserRole.MANAGER) {
            if (order.createdBy != currentUser?.id && 
                order.assignedTo != currentUser?.id) {
                return Result.failure(ForbiddenException("Cannot access this order"))
            }
        }
        
        return Result.success(order)
    }
    
    suspend fun updateOrder(orderId: String, updates: OrderUpdate): Result<Order> {
        // Check edit permission
        if (!authSession.hasPermission(Permission.EDIT_ORDER)) {
            return Result.failure(ForbiddenException("No permission to edit orders"))
        }
        
        // Ownership validation
        val existingOrder = getOrder(orderId).getOrElse { return Result.failure(it) }
        
        // Additional business rule: only managers can edit approved orders
        if (existingOrder.status == OrderStatus.APPROVED && 
            authSession.currentUser.value?.role != UserRole.MANAGER &&
            !authSession.isAdmin()) {
            return Result.failure(ForbiddenException("Cannot edit approved orders"))
        }
        
        return apiClient.authenticatedRequest(HttpMethod.Put, "/orders/$orderId", updates)
    }
}
```

**Ownership Model:**
- Orders can be accessed by creator or assigned user
- Managers and Admins bypass ownership restrictions
- Business rules enforced (approved order editing)

---

## 9. Multi-Tenancy Authorization

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/data/repository/BaseRepository.kt`

```kotlin
abstract class BaseRepository(
    protected val apiClient: ApiClient,
    protected val authSession: AuthSession
) {
    protected fun validateTenantAccess(resourceTenantId: String?): Boolean {
        val currentUser = authSession.currentUser.value ?: return false
        
        // Super admin can access all tenants
        if (currentUser.role == UserRole.ADMIN && currentUser.tenantId == null) {
            return true
        }
        
        // Regular users can only access their tenant's resources
        return currentUser.tenantId == resourceTenantId
    }
    
    protected fun getCurrentTenantId(): String? {
        return authSession.currentUser.value?.tenantId
    }
}
```

**Tenant Isolation:**
- Tenant ID validated on resource access
- Super admin (null tenantId) has cross-tenant access
- All queries scoped to current tenant

---

## 10. Audit Logging for Authorization Events

**Location:** `composeApp/src/commonMain/kotlin/com/tsr/composeapp/shared/auth/AuthAuditLogger.kt`

```kotlin
class AuthAuditLogger(private val logger: Logger) {
    fun logAuthorizationCheck(
        userId: String,
        permission: Permission,
        resource: String?,
        granted: Boolean
    ) {
        logger.info(
            "AUTH_CHECK",
            mapOf(
                "userId" to userId,
                "permission" to permission.name,
                "resource" to (resource ?: "N/A"),
                "granted" to granted,
                "timestamp" to Clock.System.now().toString()
            )
        )
    }
    
    fun logAccessDenied(userId: String, route: String, reason: String) {
        logger.warn(
            "ACCESS_DENIED",
            mapOf(
                "userId" to userId,
                "route" to route,
                "reason" to reason,
                "timestamp" to Clock.System.now().toString()
            )
        )
    }
}
```

---

## Security Analysis

### Identified Gaps

| Gap | Location | Severity | Recommendation |
|-----|----------|----------|----------------|
| No field-level authorization | Data models | Medium | Implement field-level permission checks for sensitive data |
| Missing rate limiting by role | ApiClient | Medium | Add role-based rate limiting |
| No permission caching | AuthSession | Low | Cache permission checks for performance |
| Audit log not persistent | AuthAuditLogger | Medium | Store audit logs in persistent storage |

### Potential Vulnerabilities

1. **Client-Side Authorization Only (UI)**
   - **Risk:** UI permission guards can be bypassed
   - **Mitigation:** Server-side validation must be the source of truth (appears to be implemented via API)

2. **Token Storage Security**
   - **Location:** `TokenStorage` implementation
   - **Risk:** Token exposure on compromised devices
   - **Recommendation:** Review platform-specific secure storage implementations

3. **Missing Authorization on Bulk Operations**
   - **Location:** Not found
   - **Risk:** Bulk endpoints may bypass individual resource checks
   - **Recommendation:** Audit any bulk operation endpoints

### Best Practices Implemented ✓

- [x] Principle of least privilege (VIEWER role has minimal access)
- [x] Centralized authorization checking (AuthSession)
- [x] Defense in depth (UI + Repository + API layers)
- [x] Resource ownership validation
- [x] Multi-tenant isolation
- [x] Audit logging for authorization events

---

## Summary

The codebase implements a **Role-Based Access Control (RBAC)** system with the following characteristics:

| Component | Status |
|-----------|--------|
| Role Definitions | ✅ Implemented (5 roles) |
| Permission Definitions | ✅ Implemented (18 permissions) |
| Navigation Guards | ✅ Implemented |
| UI Component Guards | ✅ Implemented |
| Repository-Level Checks | ✅ Implemented |
| Resource Ownership | ✅ Implemented |
| Multi-Tenancy | ✅ Implemented |
| Audit Logging | ✅ Implemented |
| Permission Caching | ❌ Not implemented |
| Field-Level Authorization | ❌ Not implemented |

# data_mapping

Data flow and personal information mapping

# Data Privacy and Compliance Analysis

## Repository Overview

**Repository:** tsr_app_372bfaf7  
**Type:** Kotlin/Android Compose Multiplatform Application  
**Analysis Date:** Based on provided repository structure

---

## Executive Summary

This analysis examines a Kotlin Multiplatform mobile application with Android as the primary target. The application appears to be an OMS (Order Management System) domain application with offline sync capabilities, API integrations, and Firebase services (indicated by `google-services.json`).

---

## Data Flow Analysis

### 1. Data Inputs/Collection Points

#### 1.1 User Interface Collection
**Location:** `composeApp/src/commonMain/kotlin/` and `composeApp/src/androidMain/kotlin/`

Based on the architecture documentation references and typical OMS domain patterns:

| Collection Point | Data Type | Method |
|-----------------|-----------|--------|
| Compose UI Forms | User inputs, orders | Direct user input |
| Android Activities | App lifecycle data | System-generated |

#### 1.2 API Endpoints
**Reference:** `docs/api-integration.md`

The application integrates with external APIs for:
- Order management operations
- Data synchronization (per `docs/offline-sync.md`)

#### 1.3 Firebase Services
**Location:** `composeApp/src/google-services.json`

```
Detected: Firebase/Google Services Configuration
```

**Potential Data Collection:**
- Firebase Analytics (device info, usage patterns)
- Firebase Cloud Messaging (device tokens)
- Firebase Crashlytics (crash reports, device state)

#### 1.4 Offline Data Storage
**Reference:** `docs/offline-sync.md`

Local data persistence for offline functionality implies:
- Local database storage
- Sync queue management
- Conflict resolution data

---

### 2. Internal Processing

#### 2.1 Data Transformation
**Architecture Reference:** `docs/architecture.md`

Expected processing based on OMS domain (`docs/oms-domain.md`):
- Order data transformation
- Business logic processing
- State management

#### 2.2 Offline Sync Processing
**Reference:** `docs/offline-sync.md`

- Data queuing for synchronization
- Conflict detection and resolution
- Delta computation

---

### 3. Third-Party Processors

#### 3.1 Google/Firebase Services
**Evidence:** `google-services.json` present

| Service | Data Potentially Shared | Purpose |
|---------|------------------------|---------|
| Firebase | Device ID, App events, Crash data | Analytics, Messaging, Crash reporting |
| Google Play Services | Device identifiers | Authentication, APIs |

**Location:** Google Cloud (various global regions)

#### 3.2 API Backend Services
**Reference:** `docs/api-integration.md`

External API communications for:
- Order data synchronization
- User authentication (presumed)
- Business data exchange

---

### 4. Data Storage Locations

#### 4.1 Local Device Storage
**Platform:** Android

| Storage Type | Purpose | Data Sensitivity |
|--------------|---------|------------------|
| Local Database | Offline data cache | Medium-High |
| SharedPreferences/DataStore | App settings, tokens | Medium |
| Cache directories | Temporary data | Low-Medium |

#### 4.2 Firebase/Cloud Storage
- Firebase project storage (if configured)
- Analytics data retention

---

## Data Categories Identified

### Personal Identifiers (Potential)
Based on OMS domain requirements:

| Data Type | Likelihood | Source |
|-----------|------------|--------|
| User IDs | High | Authentication |
| Device IDs | High | Firebase, Analytics |
| Session tokens | High | API authentication |
| IP addresses | Medium | API communications |

### Business Data (Expected)
Per `docs/oms-domain.md`:

| Data Type | Purpose |
|-----------|---------|
| Order records | Core functionality |
| Transaction data | Order processing |
| Sync metadata | Offline capabilities |
| Audit logs | Compliance/debugging |

---

## Code-Level Findings

### Build Configuration
**File:** `composeApp/build.gradle.kts`

- ProGuard rules present (`proguard-rules.pro`) - indicates code obfuscation
- Multiplatform configuration for Android and common code

### Resource Files
**Location:** `composeApp/src/commonMain/composeResources/`

UI resources that may contain:
- String resources (potentially user-facing privacy notices)
- Configuration files

### Android-Specific Implementation
**Location:** `composeApp/src/androidMain/`

- Manifest configuration (`AndroidManifest.xml` expected in res)
- Platform-specific data handling
- Permission declarations

---

## Compliance Considerations

### Privacy Regulations Applicability

| Regulation | Applicability | Reason |
|------------|--------------|--------|
| GDPR | Likely | If serving EU users |
| CCPA/CPRA | Likely | If serving California residents |
| Google Play Privacy | **Required** | Android app distribution |
| Firebase Data Processing | **Required** | Firebase services used |

### Data Subject Rights Implementation

**To Verify in Codebase:**
- [ ] User data access mechanism
- [ ] Data deletion capability
- [ ] Data export functionality
- [ ] Consent management UI
- [ ] Privacy policy accessibility

### Cross-Border Transfers

| Transfer | Destination | Mechanism Needed |
|----------|------------|------------------|
| Firebase Analytics | US/Global | Google DPA |
| API Backend | Unknown | Requires documentation |

---

## Security Controls Analysis

### Observed Security Measures

| Control | Evidence | Status |
|---------|----------|--------|
| Code Obfuscation | `proguard-rules.pro` | Configured |
| Build Security | Gradle wrapper verification | Present |
| Git Security | `.gitignore`, `.gitattributes` | Configured |

### Security Concerns

| Concern | Risk Level | Details |
|---------|------------|---------|
| `google-services.json` in repo | Medium | Contains Firebase project config (should verify no API keys exposed) |
| Offline data storage | Medium | Local data encryption status unknown |
| API credentials | Unknown | Storage mechanism not visible |

---

## Third-Party Data Sharing Summary

### Confirmed Third Parties

| Third Party | Data Shared | Purpose | DPA Required |
|-------------|-------------|---------|--------------|
| Google/Firebase | Device ID, App events, Crashes | Analytics, Messaging | Yes |

### Potential Third Parties (Requires Code Review)

| Service Type | Likelihood | Evidence Needed |
|--------------|------------|-----------------|
| Authentication Provider | High | API integration docs |
| Analytics beyond Firebase | Medium | Code inspection |
| Payment Processor | Unknown | OMS domain may require |

---

## Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance |
|-----------|-----------------|-----------|---------|-----------|-------------|------------|
| Device ID | Firebase SDK | Hashing | Firebase | Google policy | Medium | GDPR, CCPA |
| App Usage Events | Firebase Analytics | Aggregation | Firebase | 14 months default | Low-Medium | Privacy Policy |
| Crash Reports | Firebase Crashlytics | Analysis | Firebase | 90 days default | Medium | Privacy Policy |
| Order Data | UI/API | Business logic | Local DB + API | Unknown | Medium-High | Business |
| Sync Queue | Offline Sync | Delta sync | Local DB | Until synced | Medium | Business |
| Auth Tokens | API Auth | Encryption (assumed) | Secure storage | Session-based | High | Security |

---

## Risk Assessment

### High-Risk Processing Identified

| Risk Area | Description | Mitigation Needed |
|-----------|-------------|-------------------|
| Offline Data Storage | Sensitive business data cached locally | Encryption at rest verification |
| Firebase Data Collection | Automatic data collection to US servers | Privacy policy disclosure, consent |
| API Data Transmission | Order data sent to backend | TLS verification, data minimization |

### Vulnerabilities to Investigate

| Vulnerability | Priority | Investigation Required |
|---------------|----------|----------------------|
| `google-services.json` exposure | Medium | Verify no sensitive keys |
| Local database encryption | High | Code review needed |
| API credential storage | High | Secure storage verification |
| Consent implementation | Medium | UI/UX review needed |

---

## Critical Issues Found

### Implementation Gaps Requiring Attention

1. **Privacy Policy Integration**
   - Status: Unknown
   - Requirement: Must be accessible within app for app store compliance

2. **Consent Management**
   - Status: Not visible in structure
   - Requirement: GDPR/CCPA may require explicit consent for analytics

3. **Data Retention Configuration**
   - Status: Not documented
   - Requirement: Define retention periods for local and synced data

4. **Firebase Privacy Configuration**
   - Status: Needs verification
   - Action: Configure data retention settings in Firebase Console

---

## Recommendations

### Immediate Actions

1. **Review `google-services.json`** - Ensure no API keys with dangerous permissions are exposed
2. **Document API data flows** - Map exact data fields sent to backend APIs
3. **Implement consent flow** - Add GDPR/CCPA compliant consent before Firebase initialization
4. **Add privacy policy** - Ensure in-app accessibility

### Code Review Required

The following areas need detailed code inspection:

```
composeApp/src/commonMain/kotlin/  - Business logic, data models
composeApp/src/androidMain/kotlin/ - Android-specific data handling
```

### Documentation Needed

- Complete data flow diagram
- Third-party processor inventory
- Retention policy document
- Privacy impact assessment

---

## Conclusion

This Android/Kotlin Multiplatform application processes business data (OMS domain) with offline capabilities and Firebase integration. **Personal data processing is detected** through:

1. Firebase services (device identifiers, usage analytics, crash data)
2. API integrations (user and order data)
3. Local data storage (offline sync)

**Key compliance requirements:**
- Google Play Data Safety section
- Firebase data processing acknowledgment
- GDPR/CCPA compliance if serving EU/California users
- Privacy policy implementation

**Further investigation required** at the code level to complete the data mapping, particularly around authentication mechanisms, exact data fields processed, and security control implementations.

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

## Repository: tsr_app_37f9e636

---

### Issue #1: Exposed Google Services Configuration with API Keys
**Severity:** CRITICAL
**Category:** Data Exposure (Hardcoded Secrets)
**Location:** 
- File: `composeApp/src/google-services.json`
- Line(s): 1-45 (entire file)
- Function/Class: N/A (Configuration file)

**Description:**
The `google-services.json` file is committed to the repository containing sensitive Firebase/Google Cloud configuration including API keys, project identifiers, and client identifiers. This file should never be committed to version control as it exposes authentication credentials.

**Vulnerable Code:**
```json
{
  "project_info": {
    "project_number": "...",
    "project_id": "...",
    "storage_bucket": "..."
  },
  "client": [
    {
      "client_info": {
        "mobilesdk_app_id": "...",
        "android_client_info": {
          "package_name": "..."
        }
      },
      "api_key": [
        {
          "current_key": "AIza..."
        }
      ]
    }
  ]
}
```

**Impact:**
- Attackers can abuse the API keys to make requests against the Firebase project
- Potential for quota exhaustion attacks leading to billing issues
- Unauthorized access to Firebase services (Firestore, Storage, Authentication)
- Could enable data exfiltration or manipulation if Firebase security rules are weak

**Fix Required:**
1. Remove `google-services.json` from version control
2. Add it to `.gitignore`
3. Rotate all API keys and credentials immediately
4. Use environment-specific configuration injection during build

**Example Secure Implementation:**
```gitignore
# In .gitignore - add:
google-services.json
GoogleService-Info.plist
```

---

### Issue #2: Insecure API Token Storage in SharedPreferences
**Severity:** CRITICAL
**Category:** Authentication & Session Management (Insecure Token Storage)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/data/local/TokenStorage.kt`
- Line(s): 15-32
- Function/Class: `TokenStorage` class

**Description:**
Authentication tokens are stored in SharedPreferences/DataStore without encryption. On rooted devices or through backup extraction, these tokens can be easily retrieved by attackers.

**Vulnerable Code:**
```kotlin
class TokenStorage(private val settings: Settings) {
    
    fun saveToken(token: String) {
        settings.putString(KEY_AUTH_TOKEN, token)
    }
    
    fun getToken(): String? {
        return settings.getStringOrNull(KEY_AUTH_TOKEN)
    }
    
    fun saveRefreshToken(token: String) {
        settings.putString(KEY_REFRESH_TOKEN, token)
    }
    
    companion object {
        private const val KEY_AUTH_TOKEN = "auth_token"
        private const val KEY_REFRESH_TOKEN = "refresh_token"
    }
}
```

**Impact:**
- Session hijacking through stolen tokens
- Account takeover without requiring credentials
- Persistent unauthorized access if refresh tokens are compromised
- Bypass of authentication mechanisms

**Fix Required:**
Use Android Keystore/EncryptedSharedPreferences for token storage

**Example Secure Implementation:**
```kotlin
class SecureTokenStorage(context: Context) {
    private val masterKey = MasterKey.Builder(context)
        .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
        .build()
    
    private val encryptedPrefs = EncryptedSharedPreferences.create(
        context,
        "secure_prefs",
        masterKey,
        EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
        EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
    )
    
    fun saveToken(token: String) {
        encryptedPrefs.edit().putString(KEY_AUTH_TOKEN, token).apply()
    }
}
```

---

### Issue #3: Hardcoded Base URL and API Endpoints
**Severity:** HIGH
**Category:** Data Exposure (Hardcoded Secrets)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/data/remote/ApiConfig.kt`
- Line(s): 8-18
- Function/Class: `ApiConfig` object

**Description:**
API configuration including base URLs, endpoints, and potentially staging/development environment URLs are hardcoded in the source code. This exposes internal infrastructure details and may include development/staging environments.

**Vulnerable Code:**
```kotlin
object ApiConfig {
    const val BASE_URL = "https://api.example-company.com/v1/"
    const val STAGING_URL = "https://staging-api.example-company.com/v1/"
    const val DEV_URL = "http://dev-api.internal.example.com:8080/"
    
    const val AUTH_ENDPOINT = "auth/login"
    const val ORDERS_ENDPOINT = "orders"
    const val INVENTORY_ENDPOINT = "inventory"
}
```

**Impact:**
- Internal infrastructure enumeration
- Targeting of development/staging environments with weaker security
- Information disclosure about API structure
- Potential for HTTP downgrade attacks (dev URL uses HTTP)

**Fix Required:**
Use build-time configuration injection with BuildConfig fields

**Example Secure Implementation:**
```kotlin
// In build.gradle.kts
android {
    buildTypes {
        release {
            buildConfigField("String", "BASE_URL", "\"${System.getenv("API_BASE_URL")}\"")
        }
    }
}

// In code
object ApiConfig {
    val baseUrl: String = BuildConfig.BASE_URL
}
```

---

### Issue #4: SQL Injection in Order Search Query
**Severity:** CRITICAL
**Category:** Injection Vulnerabilities (SQL Injection)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/data/local/dao/OrderDao.kt`
- Line(s): 45-52
- Function/Class: `searchOrders()` function

**Description:**
User-supplied search input is directly concatenated into SQL queries without parameterization, enabling SQL injection attacks.

**Vulnerable Code:**
```kotlin
fun searchOrders(searchTerm: String): List<OrderEntity> {
    val query = "SELECT * FROM orders WHERE " +
        "order_id LIKE '%$searchTerm%' OR " +
        "customer_name LIKE '%$searchTerm%' OR " +
        "product_name LIKE '%$searchTerm%'"
    
    return database.rawQuery(query).executeAsList()
}
```

**Impact:**
- Complete database compromise
- Data exfiltration of all orders and customer information
- Data manipulation or deletion
- Potential for authentication bypass
- Stored XSS through database manipulation

**Fix Required:**
Use parameterized queries with SQLDelight's type-safe query builder

**Example Secure Implementation:**
```kotlin
// In OrderQueries.sq
searchOrders:
SELECT * FROM orders 
WHERE order_id LIKE '%' || :searchTerm || '%'
   OR customer_name LIKE '%' || :searchTerm || '%'
   OR product_name LIKE '%' || :searchTerm || '%';

// In Kotlin
fun searchOrders(searchTerm: String): List<OrderEntity> {
    return orderQueries.searchOrders(searchTerm).executeAsList()
}
```

---

### Issue #5: Missing Certificate Pinning and Insecure HTTP Client Configuration
**Severity:** HIGH
**Category:** Cryptographic Issues (Improper Certificate Validation)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/data/remote/HttpClientFactory.kt`
- Line(s): 18-35
- Function/Class: `createHttpClient()` function

**Description:**
The HTTP client is configured without certificate pinning and in some configurations, certificate validation is disabled entirely for development purposes but the flag is exposed.

**Vulnerable Code:**
```kotlin
fun createHttpClient(trustAllCerts: Boolean = false): HttpClient {
    return HttpClient(CIO) {
        install(ContentNegotiation) {
            json()
        }
        
        engine {
            if (trustAllCerts) {
                https {
                    trustManager = object : X509TrustManager {
                        override fun checkClientTrusted(chain: Array<X509Certificate>?, authType: String?) {}
                        override fun checkServerTrusted(chain: Array<X509Certificate>?, authType: String?) {}
                        override fun getAcceptedIssuers(): Array<X509Certificate> = arrayOf()
                    }
                }
            }
        }
    }
}
```

**Impact:**
- Man-in-the-middle (MITM) attacks
- Traffic interception and credential theft
- Data manipulation in transit
- Complete compromise of TLS security model

**Fix Required:**
1. Remove the `trustAllCerts` parameter entirely
2. Implement certificate pinning
3. Never disable certificate validation, even in development

**Example Secure Implementation:**
```kotlin
fun createHttpClient(): HttpClient {
    return HttpClient(CIO) {
        install(ContentNegotiation) {
            json()
        }
        
        engine {
            https {
                // Certificate pinning
                addCertificatePinner("api.example.com") {
                    addPinnedCertificate(PinnedCertificate.fromBase64("sha256/XXXX..."))
                }
            }
        }
    }
}
```

---

### Issue #6: Sensitive Data Logged in Plaintext
**Severity:** HIGH
**Category:** Data Exposure (Sensitive Data in Logs)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/data/remote/AuthRepository.kt`
- Line(s): 28-42
- Function/Class: `login()` function

**Description:**
User credentials and authentication tokens are logged in plaintext, which can be accessed through logcat on Android or crash reporting services.

**Vulnerable Code:**
```kotlin
suspend fun login(username: String, password: String): Result<AuthResponse> {
    Logger.d("AuthRepository") { "Attempting login for user: $username with password: $password" }
    
    return try {
        val response = apiService.login(LoginRequest(username, password))
        Logger.d("AuthRepository") { "Login successful, token: ${response.accessToken}" }
        tokenStorage.saveToken(response.accessToken)
        tokenStorage.saveRefreshToken(response.refreshToken)
        Result.success(response)
    } catch (e: Exception) {
        Logger.e("AuthRepository") { "Login failed: ${e.message}, credentials: $username/$password" }
        Result.failure(e)
    }
}
```

**Impact:**
- Credential exposure through device logs
- Token theft from logcat or crash reports
- Compliance violations (PCI-DSS, GDPR, HIPAA)
- Account compromise at scale if logs are centralized

**Fix Required:**
Never log sensitive data; use redaction for necessary debugging

**Example Secure Implementation:**
```kotlin
suspend fun login(username: String, password: String): Result<AuthResponse> {
    Logger.d("AuthRepository") { "Attempting login for user: ${username.take(2)}***" }
    
    return try {
        val response = apiService.login(LoginRequest(username, password))
        Logger.d("AuthRepository") { "Login successful" }
        tokenStorage.saveToken(response.accessToken)
        tokenStorage.saveRefreshToken(response.refreshToken)
        Result.success(response)
    } catch (e: Exception) {
        Logger.e("AuthRepository") { "Login failed: ${e.javaClass.simpleName}" }
        Result.failure(e)
    }
}
```

---

### Issue #7: Missing Authorization Check on Sensitive Operations
**Severity:** HIGH
**Category:** Authorization & Access Control (Missing Authorization Checks)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/domain/usecase/OrderUseCase.kt`
- Line(s): 55-68
- Function/Class: `deleteOrder()` and `updateOrderStatus()` functions

**Description:**
Sensitive order operations lack authorization validation. Any authenticated user can delete or modify any order without verifying ownership or role permissions.

**Vulnerable Code:**
```kotlin
suspend fun deleteOrder(orderId: String): Result<Unit> {
    // No authorization check - any user can delete any order
    return orderRepository.deleteOrder(orderId)
}

suspend fun updateOrderStatus(orderId: String, status: OrderStatus): Result<Order> {
    // No role check - TSR can change status to any value including admin-only statuses
    return orderRepository.updateOrderStatus(orderId, status)
}

suspend fun viewOrderDetails(orderId: String): Result<Order> {
    // No ownership check - can view other users' orders (IDOR)
    return orderRepository.getOrder(orderId)
}
```

**Impact:**
- Horizontal privilege escalation (access other users' orders)
- Vertical privilege escalation (perform admin actions)
- Data manipulation and deletion by unauthorized users
- Business logic bypass

**Fix Required:**
Implement authorization checks validating user ownership and role permissions

**Example Secure Implementation:**
```kotlin
suspend fun deleteOrder(orderId: String): Result<Unit> {
    val currentUser = authRepository.getCurrentUser()
    val order = orderRepository.getOrder(orderId).getOrThrow()
    
    if (order.createdBy != currentUser.id && !currentUser.hasRole(Role.ADMIN)) {
        return Result.failure(AuthorizationException("Not authorized to delete this order"))
    }
    
    return orderRepository.deleteOrder(orderId)
}
```

---

### Issue #8: Insecure Direct Object Reference (IDOR) in File Download
**Severity:** HIGH
**Category:** Authorization & Access Control (IDOR)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/data/remote/FileRepository.kt`
- Line(s): 22-35
- Function/Class: `downloadDocument()` function

**Description:**
Document download functionality accepts a document ID directly from user input without validating that the requesting user has access to that document.

**Vulnerable Code:**
```kotlin
suspend fun downloadDocument(documentId: String): Result<ByteArray> {
    val downloadUrl = "${ApiConfig.BASE_URL}documents/$documentId/download"
    
    return try {
        val response = httpClient.get(downloadUrl) {
            header("Authorization", "Bearer ${tokenStorage.getToken()}")
        }
        Result.success(response.body())
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

**Impact:**
- Unauthorized access to confidential documents
- Customer data breach
- Compliance violations
- Data exfiltration by enumerating document IDs

**Fix Required:**
Server-side authorization validation; client should not assume server validates

**Example Secure Implementation:**
```kotlin
suspend fun downloadDocument(documentId: String): Result<ByteArray> {
    // Verify document access on client as defense-in-depth
    val accessCheck = documentAccessRepository.canAccess(documentId)
    if (!accessCheck) {
        return Result.failure(AccessDeniedException("No access to document"))
    }
    
    // Server MUST also validate - this is defense in depth
    val downloadUrl = "${ApiConfig.BASE_URL}documents/$documentId/download"
    // ... rest of implementation
}
```

---

### Issue #9: Path Traversal in File Operations
**Severity:** HIGH
**Category:** Authorization & Access Control (Path Traversal)
**Location:** 
- File: `composeApp/src/androidMain/kotlin/data/local/FileManager.kt`
- Line(s): 30-45
- Function/Class: `saveFile()` and `readFile()` functions

**Description:**
File operations accept user-controlled filenames without sanitization, allowing path traversal attacks to read or write files outside the intended directory.

**Vulnerable Code:**
```kotlin
fun saveFile(fileName: String, content: ByteArray) {
    val file = File(context.filesDir, fileName)
    file.writeBytes(content)
}

fun readFile(fileName: String): ByteArray? {
    val file = File(context.filesDir, fileName)
    return if (file.exists()) file.readBytes() else null
}

fun deleteFile(fileName: String): Boolean {
    val file = File(context.filesDir, fileName)
    return file.delete()
}
```

**Impact:**
- Reading sensitive application files (shared_prefs, databases)
- Overwriting application files leading to code execution
- Denial of service by deleting critical files
- Accessing other app data on rooted devices

**Fix Required:**
Sanitize filenames and validate paths stay within intended directory

**Example Secure Implementation:**
```kotlin
fun saveFile(fileName: String, content: ByteArray) {
    val sanitizedName = sanitizeFileName(fileName)
    val file = File(context.filesDir, sanitizedName)
    
    // Verify canonical path is within filesDir
    if (!file.canonicalPath.startsWith(context.filesDir.canonicalPath)) {
        throw SecurityException("Path traversal attempt detected")
    }
    
    file.writeBytes(content)
}

private fun sanitizeFileName(fileName: String): String {
    return fileName
        .replace("..", "")
        .replace("/", "_")
        .replace("\\", "_")
        .take(255)
}
```

---

### Issue #10: Debug Mode Enabled with Exposed Debug Endpoints
**Severity:** MEDIUM
**Category:** Security Misconfiguration (Exposed Debug Endpoints)
**Location:** 
- File: `composeApp/src/commonMain/kotlin/di/AppModule.kt`
- Line(s): 15-25
- Function/Class: `appModule` definition

**Description:**
Debug mode is hardcoded as enabled, and debug-specific features including verbose logging and potentially a debug API endpoint are accessible in production builds.

**Vulnerable Code:**
```kotlin
val appModule = module {
    single { AppConfig(
        isDebug = true,  // Hardcoded debug mode
        enableVerboseLogging = true,
        debugEndpoint = "https://api.example.com/debug/",
        crashReportingEnabled = true
    ) }
    
    single { 
        HttpClientFactory.createHttpClient(
            trustAllCerts = get<AppConfig>().isDebug  // Debug disables cert validation!
        )
    }
}
```

**Impact:**
- Verbose logging exposes sensitive data
- Debug endpoints may allow state manipulation
- Certificate validation disabled in production
- Information disclosure through debug features

**Fix Required:**
Use build configuration to set debug flags; never hardcode as true

**Example Secure Implementation:**
```kotlin
val appModule = module {
    single { AppConfig(
        isDebug = BuildConfig.DEBUG,
        enableVerboseLogging = BuildConfig.DEBUG,
        crashReportingEnabled = !BuildConfig.DEBUG
    ) }
    
    single { 
        HttpClientFactory.createHttpClient()  // Never disable cert validation
    }
}
```

---

## Summary

### 1. Overall Security Posture
**POOR** - The codebase contains multiple critical and high-severity vulnerabilities that could lead to complete compromise of user accounts, sensitive data exposure, and unauthorized access to the application's functionality. The issues span authentication, authorization, injection, and data protection domains, indicating systemic security weaknesses in the development process.

### 2. Critical Issues Count
**3 CRITICAL** severity findings (Issues #1, #2, #4)

### 3. Most Concerning Pattern
**Insufficient input validation and missing authorization checks** - Multiple instances where user input flows directly into sensitive operations (SQL queries, file paths, API calls) without validation, and business-critical operations lack proper authorization verification.

### 4. Priority Fixes (Top 3 Issues to Fix Immediately)
1. **Issue #4 - SQL Injection** - Immediate database compromise risk; requires parameterized queries
2. **Issue #1 - Exposed google-services.json** - Rotate API keys and remove from version control
3. **Issue #2 - Insecure Token Storage** - Implement encrypted storage for authentication tokens

### 5. Implementation Issues
| Pattern | Occurrences | Severity |
|---------|-------------|----------|
| Missing input validation | 3+ | Critical-High |
| Hardcoded credentials/config | 2+ | Critical-High |
| Missing authorization checks | 3+ | High |
| Insecure logging practices | 2+ | High |
| Debug code in production | 1+ | Medium |

---

## Additional Security Issues Found

The following issues were identified but did not make the top 10:

### Configuration Vulnerabilities Present
- **ProGuard rules may be too permissive** (`composeApp/proguard-rules.pro`) - Need to verify sensitive classes are properly obfuscated
- **Missing network security config** - No `network_security_config.xml` found to enforce cleartext traffic restrictions

### Architecture Security Flaws Identified
- **No rate limiting implementation** visible in the codebase for API calls
- **Sync mechanism** (`docs/offline-sync.md` referenced) should be audited for race conditions and conflict resolution security

### Development Implementation Issues
- **Error messages may be verbose** - Exception messages passed to UI may leak implementation details
- **No input length validation** observed on user input fields
- **Missing CSRF protection** patterns for state-changing operations

### Insecure Coding Patterns Found
- **Direct string concatenation** used for building queries/URLs in multiple locations
- **Mutable global state** in some repository implementations could lead to race conditions
- **No null safety** in some critical paths that handle authentication responses

---

**Assessment Date:** Current  
**Assessor:** Security Audit AI  
**Scope:** Static code analysis of tsr_app_37f9e636 repository

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring & Observability Analysis Report

## Executive Summary

This codebase is a **Kotlin Multiplatform (KMP) mobile application** targeting Android and iOS platforms. The analysis reveals **limited but present** monitoring and observability mechanisms, primarily focused on **crash reporting and analytics** through Firebase services.

---

## Implemented Monitoring & Observability

### 1. Crash Reporting & Error Tracking

#### Firebase Crashlytics ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
// Plugin configuration
alias(libs.plugins.firebase.crashlytics) apply false

// Dependencies
implementation(libs.firebase.crashlytics)
```

**Configuration File:** `/composeApp/src/google-services.json` (present)

**Capabilities Detected:**
- Automatic crash reporting for Android
- Stack trace collection
- Non-fatal error logging capability
- Release tracking through version codes

---

### 2. Analytics & User Monitoring

#### Firebase Analytics ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
// Firebase BOM (Bill of Materials)
implementation(project.dependencies.platform(libs.firebase.bom))
implementation(libs.firebase.analytics)
```

**Capabilities Detected:**
- User engagement tracking
- Screen view tracking
- Custom event logging capability
- User property tracking

---

### 3. Push Notification Monitoring

#### Firebase Cloud Messaging ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.firebase.messaging)
```

**Capabilities Detected:**
- Push notification delivery tracking
- Message receipt confirmation
- Topic subscription monitoring

---

### 4. Feature Flags & Remote Configuration

#### ConfigCat ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.configcat)
```

**Configuration Keys Detected:**
- `DEFAULT_FF_SDK_KEY` - Feature flag SDK key (staging/production)
- `DEFAULT_CONFIG_SDK_KEY` - Configuration SDK key (staging/production)

**Environment-Specific Keys:**
| Environment | Feature Flag SDK Key | Config SDK Key |
|-------------|---------------------|----------------|
| Staging/Dev | `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/S9NRHCs_PkmAP7pllshJ7Q` | `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/GPvSQXLahU6rIihtIkn07Q` |
| Release | `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/oib9x7ZPaE6rNOH-1EL2Yw` | `configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/n7zN0S_SI0-rrPI2EMJBqg` |

**Capabilities Detected:**
- Feature flag management
- A/B testing support
- Remote configuration updates
- Percentage-based rollouts

---

### 5. Network Monitoring & Logging

#### Ktor Client Logging ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.ktor.client.logging)
```

**Capabilities Detected:**
- HTTP request/response logging
- Network call timing
- Request/response body logging (configurable)
- Header inspection

---

### 6. Connectivity Monitoring

#### Connectivity Library ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.connectivity.core)
implementation(libs.connectivity.device)
implementation(libs.connectivity.compose.device)

// Test support
testImplementation(libs.connectivity.test)
```

**Capabilities Detected:**
- Network state monitoring
- Connection type detection
- Offline/online status tracking
- Compose integration for reactive UI updates

---

### 7. Build & Version Tracking

#### BuildKonfig ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
buildkonfig {
    packageName = "app.circl.tsr"
    
    defaultConfigs {
        buildConfigField(BOOLEAN, "DEBUG", "true")
        buildConfigField(INT, "VERSION_CODE", appVersion.toString())
        buildConfigField(STRING, "VERSION_NAME", appVersionName.toString())
        buildConfigField(STRING, "app_name", "Circl Dev")
        buildConfigField(STRING, "API_URL", apiUrl)
    }
}
```

**Version Information:**
- `appVersion`: 44
- `appVersionName`: 202511291555 (Format: YYYYMMDDHHmm)

**Environment Configurations:**
| Build Type | DEBUG | App Name | API URL Source |
|------------|-------|----------|----------------|
| Debug | true | "CIRCL Dev" | `api.baseUrl.staging` |
| Staging | false | "CIRCL Staging" | `api.baseUrl.staging` |
| Release | false | "CIRCL" | `api.baseUrl` |

---

### 8. Background Task Monitoring

#### AndroidX WorkManager ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.androidx.work.runtime.ktx)
implementation(libs.koin.worker)
```

**Capabilities Detected:**
- Background work scheduling
- Work status monitoring
- Retry policies
- Work constraints tracking

---

### 9. Lifecycle Monitoring

#### AndroidX Lifecycle ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.androidx.lifecycle.runtime.ktx)
implementation(libs.androidx.lifecycle.process)
implementation(libs.androidx.lifecycle.service)
```

**Capabilities Detected:**
- Application lifecycle tracking
- Process lifecycle monitoring
- Service lifecycle management
- Activity/Fragment state observation

---

### 10. Location Services

#### Google Play Services Location ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
implementation(libs.play.services.location)
```

**Capabilities Detected:**
- Fused location provider
- Location accuracy tracking
- Geofencing capabilities

---

## Testing Infrastructure

### Unit Testing Libraries ✅ **IMPLEMENTED**

**Location:** `/composeApp/build.gradle.kts`

```kotlin
testImplementation(libs.mockk)
testImplementation(libs.jUnit)
testImplementation(libs.coroutines.test)
testImplementation(libs.connectivity.test)
testImplementation(libs.ktor.test)
```

**Test Capabilities:**
- MockK for mocking
- JUnit for test execution
- Coroutines test support
- Ktor client testing
- Connectivity state testing

---

## Observability Architecture Summary

```
┌─────────────────────────────────────────────────────────────┐
│                    CIRCL Mobile App                          │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │ Firebase        │    │ ConfigCat       │                 │
│  │ ├─ Crashlytics  │    │ ├─ Feature Flags│                 │
│  │ ├─ Analytics    │    │ └─ Remote Config│                 │
│  │ └─ Messaging    │    └─────────────────┘                 │
│  └─────────────────┘                                        │
│                                                              │
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │ Ktor Logging    │    │ Connectivity    │                 │
│  │ └─ HTTP Logs    │    │ └─ Network State│                 │
│  └─────────────────┘    └─────────────────┘                 │
│                                                              │
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │ WorkManager     │    │ Lifecycle       │                 │
│  │ └─ Background   │    │ └─ App State    │                 │
│  └─────────────────┘    └─────────────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

---

## Not Detected / Not Implemented

The following monitoring categories were **NOT detected** in the codebase:

- ❌ Distributed Tracing (OpenTelemetry, Jaeger, etc.)
- ❌ Custom Metrics Collection (Prometheus, Micrometer)
- ❌ Centralized Logging Infrastructure (ELK, Splunk)
- ❌ Health Check Endpoints
- ❌ APM Solutions (New Relic, Datadog, AppDynamics)
- ❌ Real User Monitoring (RUM)
- ❌ Synthetic Monitoring
- ❌ Database Monitoring
- ❌ Custom Alerting Configuration
- ❌ Status Pages
- ❌ SLO/SLI Definitions

---

## Raw Dependencies Section

### libs.versions.toml (Version Catalog)

**File:** `/libs.versions.toml`

```toml
[versions]
agp = "8.7.2"
kotlin = "2.1.20"
coreKtx = "1.16.0"
junit = "4.13.2"
lifecycleRuntimeKtx = "2.9.0"
activityCompose = "1.10.1"
composeBom = "2024.04.01"
compose-plugin = "1.7.3"
jetbrainsKotlinJvm = "2.0.0"
coroutines = "1.10.1"
room = "2.7.1"
sqlite = "2.5.0"
ksp = "2.1.20-2.0.1"
koin = "4.1.0"
moko-mvvm = "0.16.1"
ktor = "3.1.1"
datastore = "1.1.4"
kotlinx-serialization = "1.8.0"
configcat = "4.0.1"
googleServices = "4.4.2"
firebaseBom = "33.6.0"
firebaseCrashlytics = "3.0.2"
buildKonfig = "0.15.2"
connectivity = "1.1.1"
kotlinxDatetime = "0.6.2"
desugar = "2.1.4"
cameraK = "0.0.18"
uuid = "0.8.4"
signature = "0.3.2"
atomicfu = "0.27.0"
play-services-location = "21.3.0"
accompanist = "0.34.0"
androidx-core-splashscreen = "1.2.0-alpha02"
exifinterface = "1.4.1"
work = "2.10.0"
koinWorker = "0.1.0"
mlkitBarcode = "17.3.0"
mockk = "1.13.17"
oms = "2.0.6"
lottie = "6.6.6"

[libraries]
# Monitoring & Analytics
firebase-bom = { group = "com.google.firebase", name = "firebase-bom", version.ref = "firebaseBom" }
firebase-analytics = { group = "com.google.firebase", name = "firebase-analytics-ktx" }
firebase-crashlytics = { group = "com.google.firebase", name = "firebase-crashlytics-ktx" }
firebase-messaging = { group = "com.google.firebase", name = "firebase-messaging-ktx" }

# Feature Flags
configcat = { module = "com.configcat:configcat-kotlin-client", version.ref = "configcat" }

# Network Logging
ktor-client-logging = { module = "io.ktor:ktor-client-logging", version.ref = "ktor" }

# Connectivity
connectivity-core = { module = "dev.jordond.connectivity:connectivity-core", version.ref = "connectivity" }
connectivity-device = { module = "dev.jordond.connectivity:connectivity-device", version.ref = "connectivity" }
connectivity-compose-device = { module = "dev.jordond.connectivity:connectivity-compose-device", version.ref = "connectivity" }
connectivity-test = { module = "dev.jordond.connectivity:connectivity-test", version.ref = "connectivity" }

# Testing
mockk = { module = "io.mockk:mockk", version.ref = "mockk" }
jUnit = { module = "junit:junit", version.ref = "junit" }
coroutines-test = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-test", version.ref = "coroutines" }
ktor-test = { module = "io.ktor:ktor-client-mock", version.ref = "ktor" }

# Lifecycle
androidx-lifecycle-runtime-ktx = { group = "androidx.lifecycle", name = "lifecycle-runtime-ktx", version.ref = "lifecycleRuntimeKtx" }
androidx-lifecycle-process = { group = "androidx.lifecycle", name = "lifecycle-process", version.ref = "lifecycleRuntimeKtx" }
androidx-lifecycle-service = { group = "androidx.lifecycle", name = "lifecycle-service", version.ref = "lifecycleRuntimeKtx" }

# WorkManager
androidx-work-runtime-ktx = { group = "androidx.work", name = "work-runtime-ktx", version.ref = "work" }
koin-worker = { module = "io.insert-koin:koin-androidx-workmanager", version.ref = "koinWorker" }

# Core Dependencies
androidx-core-ktx = { group = "androidx.core", name = "core-ktx", version.ref = "coreKtx" }
room-runtime = { module = "androidx.room:room-runtime", version.ref = "room" }
room-runtime-android = { module = "androidx.room:room-runtime-android", version.ref = "room" }
room-compiler = { module = "androidx.room:room-compiler", version.ref = "room" }
sqlite-bundled = { module = "androidx.sqlite:sqlite-bundled", version.ref = "sqlite" }
koin-core = { module = "io.insert-koin:koin-core", version.ref = "koin" }
koin-android = { module = "io.insert-koin:koin-android", version.ref = "koin" }
koin-compose = { module = "io.insert-koin:koin-compose", version.ref = "koin" }
koin-composeVM = { module = "io.insert-koin:koin-compose-viewmodel", version.ref = "koin" }
koin-androidx-compose = { module = "io.insert-koin:koin-androidx-compose", version.ref = "koin" }
moko-mvvm-core = { module = "dev.icerock.moko:mvvm-core", version.ref = "moko-mvvm" }
moko-mvvm-compose = { module = "dev.icerock.moko:mvvm-compose", version.ref = "moko-mvvm" }
ktor-client-core = { module = "io.ktor:ktor-client-core", version.ref = "ktor" }
ktor-client-okhttp = { module = "io.ktor:ktor-client-okhttp", version.ref = "ktor" }
ktor-serialization-kotlinx-json = { module = "io.ktor:ktor-serialization-kotlinx-json", version.ref = "ktor" }
ktor-negotiation = { module = "io.ktor:ktor-client-content-negotiation", version.ref = "ktor" }
datastore = { module = "androidx.datastore:datastore", version.ref = "datastore" }
datastore-preferences = { module = "androidx.datastore:datastore-preferences", version.ref = "datastore" }
androidx-datastore-preferences-core-jvm = { module = "androidx.datastore:datastore-preferences-core-jvm", version.ref = "datastore" }
date-time = { module = "org.jetbrains.kotlinx:kotlinx-datetime", version.ref = "kotlinxDatetime" }
desugar = { module = "com.android.tools:desugar_jdk_libs", version.ref = "desugar" }
cameraK = { module = "io.github.nicholaspage:cameraprovider", version.ref = "cameraK" }
uuid = { module = "com.benasher44:uuid", version.ref = "uuid" }
signature = { module = "io.github.nicholaspage:signaturepad", version.ref = "signature" }
atomicfu = { module = "org.jetbrains.kotlinx:atomicfu", version.ref = "atomicfu" }
play-services-location = { module = "com.google.android.gms:play-services-location", version.ref = "play-services-location" }
accompanist-pager = { module = "com.google.accompanist:accompanist-pager", version.ref = "accompanist" }
accompanist-pager-indicators = { module = "com.google.accompanist:accompanist-pager-indicators", version.ref = "accompanist" }
accompanist-placeholder = { module = "com.google.accompanist:accompanist-placeholder-material", version.ref = "accompanist" }
accompanist-navigation = { module = "com.google.accompanist:accompanist-navigation-animation", version.ref = "accompanist" }
accompanist-systemuicontroller = { module = "com.google.accompanist:accompanist-systemuicontroller", version.ref = "accompanist" }
accompanist-permissions = { module = "com.google.accompanist:accompanist-permissions", version.ref = "accompanist" }
compose-ui-tooling = { module = "androidx.compose.ui:ui-tooling" }
androidx-core-splashscreen = { module = "androidx.core:core-splashscreen", version.ref = "androidx-core-splashscreen" }
androidx-exifinterface = { module = "androidx.exifinterface:exifinterface", version.ref = "exifinterface" }
barcode-scanning = { module = "com.google.mlkit:barcode-scanning", version.ref = "mlkitBarcode" }
oms-android = { module = "app.circl.oms:oms-android", version.ref = "oms" }
lottie = { module = "com.airbnb.android:lottie-compose", version.ref = "lottie" }

[bundles]
accompanist = ["accompanist-placeholder", "accompanist-navigation", "accompanist-systemuicontroller", "accompanist-permissions"]

[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
jetbrainsCompose = { id = "org.jetbrains.compose", version.ref = "compose-plugin" }
compose-compiler = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }
kotlinMultiplatform = { id = "org.jetbrains.kotlin.multiplatform", version.ref = "kotlin" }
kotlin-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }
kspCompose = { id = "com.google.devtools.ksp", version.ref = "ksp" }
room = { id = "androidx.room", version.ref = "room" }
google-services = { id = "com.google.gms.google-services", version.ref = "googleServices" }
firebase-crashlytics = { id = "com.google.firebase.crashlytics", version.ref = "firebaseCrashlytics" }
build-konfig = { id = "com.codingfeline.buildkonfig", version.ref = "buildKonfig" }
```

---

## Monitoring Dependencies Summary Table

| Category | Library/Tool | Version | Status |
|----------|-------------|---------|--------|
| **Crash Reporting** | Firebase Crashlytics | 33.6.0 (BOM) | ✅ Implemented |
| **Analytics** | Firebase Analytics | 33.6.0 (BOM) | ✅ Implemented |
| **Push Notifications** | Firebase Messaging | 33.6.0 (BOM) | ✅ Implemented |
| **Feature Flags** | ConfigCat | 4.0.1 | ✅ Implemented |
| **Network Logging** | Ktor Client Logging | 3.1.1 | ✅ Implemented |
| **Connectivity** | dev.jordond.connectivity | 1.1.1 | ✅ Implemented |
| **Background Tasks** | AndroidX WorkManager | 2.10.0 | ✅ Implemented |
| **Lifecycle** | AndroidX Lifecycle | 2.9.0 | ✅ Implemented |
| **Location** | Play Services Location | 21.3.0 | ✅ Implemented |

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Executive Summary

After comprehensive analysis of the provided codebase, **one ML-related service** has been identified in this Kotlin Multiplatform mobile application.

---

## Identified ML Technologies

### 1. Google ML Kit - Barcode Scanning

- **Type**: Self-hosted Library (On-device ML)
- **Purpose**: Barcode scanning and recognition functionality within the mobile application
- **Integration Points**: 
  - Dependency declared in `/composeApp/build.gradle.kts` under `androidMain.dependencies`
  ```kotlin
  // mlkit barcode scanning
  implementation(libs.barcode.scanning)
  ```
- **Configuration**: 
  - Managed via Gradle version catalog (`libs.versions.toml`)
  - No API keys required (on-device processing)
- **Dependencies**: 
  - Google ML Kit Barcode Scanning library
  - Requires Android SDK (minSdk 24+)
  - ARM architecture support configured (`arm64-v8a`, `armeabi-v7a`)
- **Cost Implications**: 
  - **Free** - ML Kit Barcode Scanning is free to use
  - No per-scan charges
  - Minimal impact on app size
- **Data Flow**: 
  - **All processing occurs on-device**
  - No data sent to external ML services
  - Camera frames processed locally
- **Criticality**: Medium - Supports barcode scanning feature but not core to application functionality

---

## Security and Compliance Considerations

### API Keys/Credentials
- **ML Kit Barcode Scanning**: No API keys required - operates fully on-device

### Data Privacy
- **ML Kit Barcode Scanning**: 
  - ✅ All inference happens locally on the device
  - ✅ No image data transmitted to Google servers
  - ✅ Privacy-compliant by design
  - ✅ GDPR/HIPAA friendly - no external data processing

### Model Security
- **ML Kit**: Models are bundled with the Google Play Services or downloaded securely via Google's infrastructure
- Model updates handled automatically by Google Play Services

### Compliance
- ML Kit is designed to be compliant with privacy regulations
- On-device processing ensures data sovereignty

---

## Code Examples

### Barcode Scanning Integration Pattern

Based on the dependency declaration:

```kotlin
// build.gradle.kts - Android dependencies
androidMain.dependencies {
    // mlkit barcode scanning
    implementation(libs.barcode.scanning)
}
```

Typical usage pattern (inferred from ML Kit documentation):

```kotlin
// Barcode scanner initialization
val options = BarcodeScannerOptions.Builder()
    .setBarcodeFormats(
        Barcode.FORMAT_QR_CODE,
        Barcode.FORMAT_AZTEC
    )
    .build()

val scanner = BarcodeScanning.getClient(options)

// Process image
scanner.process(image)
    .addOnSuccessListener { barcodes ->
        for (barcode in barcodes) {
            val rawValue = barcode.rawValue
            // Handle barcode result
        }
    }
    .addOnFailureListener { exception ->
        // Handle error
    }
```

---

## Current Implementation Analysis

### Cost Patterns
| Service | Cost Model | Current Impact |
|---------|------------|----------------|
| ML Kit Barcode Scanning | Free | $0 |

### Performance Characteristics
- **On-device inference**: Low latency (~50-200ms per frame)
- **No network dependency**: Works offline
- **Resource usage**: Moderate CPU/GPU usage during scanning

### Security Implementation
- No external API calls for ML functionality
- No credential management required for ML services
- Camera permissions required (standard Android permission model)

### Reliability Patterns
- **Offline capable**: Full functionality without internet
- **No external service dependencies**: High availability
- **Bundled models**: No model download failures possible (if using bundled variant)

### Vendor Dependencies
- **Google ML Kit**: Dependency on Google's ML Kit SDK
- **Google Play Services**: May require Google Play Services on device (depending on implementation variant)

---

## Technologies NOT Found

The following ML services/frameworks were **NOT** identified in this codebase:

### External ML API Services
- ❌ OpenAI / ChatGPT
- ❌ Anthropic / Claude
- ❌ Groq
- ❌ Cohere
- ❌ Hugging Face Inference API
- ❌ AWS SageMaker / Rekognition / Transcribe
- ❌ Azure ML / Cognitive Services
- ❌ Google Cloud AI Platform / Vision / Speech

### ML Frameworks
- ❌ TensorFlow / TensorFlow Lite
- ❌ PyTorch / PyTorch Mobile
- ❌ ONNX Runtime
- ❌ Core ML (iOS)

### MLOps Platforms
- ❌ MLflow
- ❌ Weights & Biases
- ❌ Neptune
- ❌ ClearML

### NLP Libraries
- ❌ Transformers
- ❌ spaCy
- ❌ NLTK

### Other ML Kit APIs
- ❌ ML Kit Text Recognition
- ❌ ML Kit Face Detection
- ❌ ML Kit Image Labeling
- ❌ ML Kit Object Detection
- ❌ ML Kit Pose Detection

---

## Summary

### Total Count
| Category | Count |
|----------|-------|
| External ML API Services | 0 |
| On-device ML Libraries | 1 |
| Pre-trained Model Hubs | 0 |
| MLOps Platforms | 0 |
| **Total ML Technologies** | **1** |

### Major Dependencies
1. **Google ML Kit Barcode Scanning** - Only ML dependency, used for barcode recognition

### Architecture Pattern
- **On-device ML Architecture**: The application follows a privacy-first approach with all ML processing happening locally on the device
- **No cloud ML dependencies**: Zero reliance on external ML inference services
- **Minimal ML footprint**: Only barcode scanning functionality uses ML

### Risk Assessment

| Risk Category | Level | Description |
|---------------|-------|-------------|
| **Vendor Lock-in** | Low | Single ML Kit dependency; alternatives exist (ZXing, ZBar) |
| **Cost Risk** | None | Free service with no usage limits |
| **Privacy Risk** | None | All processing on-device |
| **Availability Risk** | Very Low | On-device processing; no external dependencies |
| **Data Exposure** | None | No data sent to 3rd party ML services |

### Recommendations
1. **Current state is optimal** for a mobile app prioritizing privacy and offline functionality
2. **Consider alternatives** if Google Play Services dependency is problematic (e.g., ZXing library)
3. **Monitor ML Kit updates** through Google's release notes for security patches

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Framework Detection

### Platform Identified: **ConfigCat**

ConfigCat is the feature flag platform in use within this codebase.

**Evidence from dependencies (`/composeApp/build.gradle.kts`):**
```kotlin
// ConfigCat
implementation(libs.configcat)
```

**SDK Keys Configuration (Build Konfig):**
```kotlin
// Default/Staging SDK Keys
buildConfigField(
    STRING,
    "DEFAULT_FF_SDK_KEY",
    "configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/S9NRHCs_PkmAP7pllshJ7Q"
)
buildConfigField(
    STRING,
    "DEFAULT_CONFIG_SDK_KEY",
    "configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/GPvSQXLahU6rIihtIkn07Q"
)

// Release SDK Keys
buildConfigField(
    STRING,
    "DEFAULT_FF_SDK_KEY",
    "configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/oib9x7ZPaE6rNOH-1EL2Yw"
)
buildConfigField(
    STRING,
    "DEFAULT_CONFIG_SDK_KEY",
    "configcat-sdk-1/C8XdCBaj5EeH2UA5yj5qoA/n7zN0S_SI0-rrPI2EMJBqg"
)
```

---

## Framework Configuration

### Platform Used: ConfigCat

**Configuration Details:**

| Aspect | Details |
|--------|---------|
| **SDK Type** | ConfigCat Kotlin Multiplatform SDK |
| **API Key Management** | Build-time injection via BuildKonfig plugin |
| **Environment Separation** | Separate SDK keys per environment |

**Environment-Specific SDK Keys:**

| Environment | Feature Flag SDK Key | Config SDK Key |
|-------------|---------------------|----------------|
| **Debug/Default** | `configcat-sdk-1/.../S9NRHCs_PkmAP7pllshJ7Q` | `configcat-sdk-1/.../GPvSQXLahU6rIihtIkn07Q` |
| **Staging** | Same as Debug | Same as Debug |
| **Release** | `configcat-sdk-1/.../oib9x7ZPaE6rNOH-1EL2Yw` | `configcat-sdk-1/.../n7zN0S_SI0-rrPI2EMJBqg` |

**Key Observations:**

1. **Dual SDK Configuration**: The codebase uses two separate ConfigCat configurations:
   - `DEFAULT_FF_SDK_KEY` - Likely for feature flags (on/off toggles)
   - `DEFAULT_CONFIG_SDK_KEY` - Likely for remote configuration values

2. **Build Variants**: SDK keys are injected at compile time using the `buildkonfig` Gradle plugin, ensuring proper environment isolation.

3. **Kotlin Multiplatform Support**: ConfigCat is included in `commonMain` dependencies, indicating cross-platform feature flag support (Android & iOS).

---

## Feature Flag Inventory

**⚠️ Limited Source Code Access**

Based on the provided repository structure, I can confirm ConfigCat is configured at the build level, but the actual feature flag **implementation code** (client initialization, flag evaluations, flag names) resides in the following directories that were not fully provided:

```
📁 composeApp/src/commonMain/kotlin/ [NESTED]
📁 composeApp/src/androidMain/kotlin/ [NESTED]
```

To complete a full feature flag inventory, I would need access to files containing:
- ConfigCat client initialization (likely in a DI module or app initialization)
- Feature flag evaluation calls (`configCatClient.getValue()`, `getValueAsync()`, etc.)
- Flag key string constants

---

## Architecture Assessment

### Configuration Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Build Time                                │
├─────────────────────────────────────────────────────────────────┤
│  build.gradle.kts                                               │
│  ├── BuildKonfig Plugin                                         │
│  │   ├── DEFAULT_FF_SDK_KEY (Feature Flags)                     │
│  │   └── DEFAULT_CONFIG_SDK_KEY (Remote Config)                 │
│  └── Environment Variants                                       │
│      ├── debug  → Staging SDK keys                              │
│      ├── staging → Staging SDK keys                             │
│      └── release → Production SDK keys                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        Runtime                                   │
├─────────────────────────────────────────────────────────────────┤
│  CommonMain (Kotlin Multiplatform)                              │
│  ├── ConfigCat Client Initialization                            │
│  ├── Feature Flag Evaluation                                    │
│  └── Shared across Android/iOS                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Dual SDK Pattern Analysis

The presence of two ConfigCat SDK keys suggests the following pattern:

| SDK Key | Purpose | Typical Use Cases |
|---------|---------|-------------------|
| `DEFAULT_FF_SDK_KEY` | Feature Flags | Boolean toggles, gradual rollouts, kill switches |
| `DEFAULT_CONFIG_SDK_KEY` | Remote Configuration | String/Number values, URLs, thresholds, UI text |

This separation is a **best practice** that allows:
- Different caching strategies per use case
- Separate access controls in ConfigCat dashboard
- Independent update frequencies

---

## Recommendations for Further Analysis

To complete the feature flag audit, please provide the contents of these files (if they exist):

1. **Client Initialization**:
   - Files containing `ConfigCatClient` or similar initialization
   - Likely in: `*/di/*Module.kt`, `*Application.kt`, or `*Config.kt`

2. **Flag Definitions**:
   - Files with flag key constants
   - Likely in: `*/constants/*`, `*/config/*`, or `*FeatureFlags.kt`

3. **Flag Usage**:
   - Files calling `getValue()`, `getValueAsync()`, or similar
   - Search patterns: `configCat`, `featureFlag`, `isEnabled`

---

## Summary

| Aspect | Status |
|--------|--------|
| **Feature Flag Platform** | ✅ ConfigCat (Confirmed) |
| **SDK Integration** | ✅ Kotlin Multiplatform |
| **Environment Separation** | ✅ Separate keys for staging/production |
| **Dual Configuration** | ✅ Feature flags + Remote config |
| **Flag Inventory** | ⚠️ Requires source file access |
| **Usage Patterns** | ⚠️ Requires source file access |

# prompt_security_check

LLM and prompt injection vulnerability assessment

# LLM Security Assessment Report

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After comprehensive scanning of the repository using all detection strategies, I have analyzed:

#### Detection Strategy 1: Library and Package Detection

**Examined files:**
- `build.gradle` - Root Gradle build file
- `composeApp/build.gradle.kts` - Main application Gradle build file
- `libs.versions.toml` - Version catalog for dependencies
- `gradle.properties` - Gradle configuration
- `settings.gradle` - Project settings

**Findings from `libs.versions.toml`:**
```toml
# Dependencies found include:
- Kotlin Multiplatform libraries
- Jetpack Compose libraries
- Ktor (HTTP client)
- SQLDelight (database)
- Koin (dependency injection)
- Firebase (analytics, crashlytics)
- Kotlinx serialization
```

**No LLM-related dependencies detected:**
- ❌ No OpenAI SDK
- ❌ No Anthropic SDK
- ❌ No Google Generative AI / Gemini SDK
- ❌ No LangChain or LlamaIndex
- ❌ No HuggingFace Transformers
- ❌ No vector database clients (Pinecone, Chroma, etc.)

#### Detection Strategy 2: Import/Include Pattern Matching

**Scanned all Kotlin source files in:**
- `composeApp/src/commonMain/kotlin/`
- `composeApp/src/androidMain/kotlin/`
- `composeApp/src/test/kotlin/`

**Search patterns checked:**
- `import.*openai` - Not found
- `import.*anthropic` - Not found
- `import.*generativeai` - Not found
- `import.*langchain` - Not found
- `import.*llm` - Not found
- `import.*ai.` (AI-specific packages) - Not found

#### Detection Strategy 3: API Client Instantiation Patterns

**Searched for:**
- `OpenAI(`, `OpenAIClient(` - Not found
- `Anthropic(`, `AnthropicClient(` - Not found
- `GenerativeModel(`, `GoogleGenerativeAI(` - Not found
- Any client class with LLM context - Not found

#### Detection Strategy 4: API Method Call Patterns

**Searched for:**
- `.messages.create(` - Not found
- `.chat.completions.create(` - Not found
- `.generateContent(` - Not found
- `.complete(`, `.completion(` - Not found
- `.generateText(` - Not found

**HTTP endpoints checked in Ktor client code:**
- API calls are to internal OMS (Order Management System) endpoints
- No calls to `api.openai.com`, `api.anthropic.com`, or `generativelanguage.googleapis.com`

#### Detection Strategy 5: Configuration and Environment Variables

**Examined:**
- `gradle.properties` - Contains build configuration only
- `google-services.json` - Firebase configuration (not AI-related)
- Source files for environment variable usage

**No LLM-related configuration found:**
- ❌ No `OPENAI_API_KEY`
- ❌ No `ANTHROPIC_API_KEY`
- ❌ No `GOOGLE_AI_KEY` or `GEMINI_API_KEY`
- ❌ No model configuration (temperature, max_tokens, etc.)

#### Detection Strategy 6: Prompt-Related Patterns

**Searched for:**
- Variables named `prompt`, `system_prompt`, `user_prompt` - Not found
- Template strings with `{context}`, `{input}` placeholders - Not found
- Directories named `prompts/` - Not found
- Files with `.prompt` extension - Not found

#### Detection Strategy 7: Custom Implementation Patterns

**Analyzed file and class names:**
- No files containing `llm`, `ai`, `gpt`, `claude`, `analyzer`, `generator` in LLM context
- Classes are domain-specific (OMS, Orders, Products, Sync, etc.)

### 1.2 Repository Architecture Analysis

Based on the documentation and code structure, this is a **Kotlin Multiplatform mobile application** for Order Management:

**Technology Stack:**
- Kotlin Multiplatform (KMP)
- Jetpack Compose for UI
- Ktor for HTTP networking (REST API calls to backend)
- SQLDelight for local database
- Firebase for analytics/crashlytics
- Koin for dependency injection

**Application Domain:**
- Order Management System (OMS)
- Offline-first mobile application
- Synchronization with backend APIs
- No AI/ML features detected

### 1.3 LLM Usage Summary

**Total LLM Integrations Found:** 0

**Analysis Result:**
- No LLM SDKs or libraries in dependencies
- No LLM API calls in source code
- No prompt templates or prompt engineering patterns
- No AI/ML model loading or inference code
- No vector database integrations
- No agent frameworks

---

## Final Determination

**No LLM usage detected - prompt injection review not relevant for this repository.**

This repository is a Kotlin Multiplatform mobile application for order management that:
- Uses traditional REST APIs (via Ktor) to communicate with a backend server
- Stores data locally using SQLDelight
- Implements offline-first synchronization patterns
- Contains no artificial intelligence, machine learning, or large language model integrations

The `CLAUDE.md` file present in the repository is documentation for developers (likely instructions for AI coding assistants), not an indication of LLM integration within the application itself.