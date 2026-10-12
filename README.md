# BillDog Android SDK

![Version](https://img.shields.io/badge/version-1.1.0--beta.3-blue)
![Platform](https://img.shields.io/badge/platform-Android-3DDC84?logo=android&logoColor=white)
![minSdk](https://img.shields.io/badge/minSdk-21-orange)
![Maven Central](https://img.shields.io/badge/Maven%20Central-io.billdog-blue?logo=apachemaven)
![License](https://img.shields.io/badge/license-MIT-green)

**BillDog** is an all-in-one **Monetization, Engagement, and Data** platform built for mobile applications. It combines **Paywalls & Billing**, **Product Analytics**, **In-App Messaging**, and **Surveys** into a single, modular package.

> The **full** BillDog SDK (compiled AAR artifacts, closed source) is published to **Maven Central** under the `io.billdog` group at version **`1.1.0-beta.3`** — no account or token required.
>
> Looking for the Eng edition? See **[billdog-android-eng](https://github.com/billdogai/billdog-android-eng)**.

---

## 🏗️ Architecture

BillDog uses a **highly modular architecture** — pull in only what you need to keep your app binary lean (e.g. `billdog-compose` for paywalls, or specific modules for surveys and analytics).

```text
billdog                  → Full-suite umbrella; bundles the core modules in one dependency
billdog-core             → Shared foundation, customer identity, and custom billing APIs
billdog-auth             → Native auth, hosted auth cards, and session management
billdog-paywall          → Paywall presentation engine and offer/product resolution
billdog-compose          → Jetpack Compose native paywall renderer
billdog-webview          → Fallback HTML/CSS paywall renderer
billdog-analytics        → Event tracking, autocapture, and user property updates
billdog-ab-test          → Experiment assignment and A/B testing
billdog-caching          → Offline caching of offerings and API responses
billdog-replay           → Screenshot-based session replay with privacy masking
billdog-survey           → Headless survey engine with branching logic
billdog-survey-compose   → Jetpack Compose survey UI
billdog-inappmessages    → Trigger-based in-app banners, dialogs, and modals
billdog-notifications    → Push notification manager & deep-link routing
billdog-virtual-currency → Server-synced wallet balance and drops
billdog-bom              → Bill of Materials for version alignment
```

---

## 🚀 Installation

### 1. Add the registry

The SDK is on **Maven Central** — no account, credentials, or token required. Just make sure `mavenCentral()` is in your `settings.gradle.kts` (it is by default in new Android Studio projects):

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
    }
}
```

### 2. Declare dependencies

```kotlin
dependencies {
    // Core + native Compose paywalls (pulls in billdog-core automatically)
    implementation("io.billdog:billdog-compose:1.1.0-beta.3")

    // Optional feature modules
    implementation("io.billdog:billdog-auth:1.1.0-beta.3")
    implementation("io.billdog:billdog-analytics:1.1.0-beta.3")
    implementation("io.billdog:billdog-replay:1.1.0-beta.3")
    implementation("io.billdog:billdog-survey-compose:1.1.0-beta.3")
    implementation("io.billdog:billdog-inappmessages:1.1.0-beta.3")
    implementation("io.billdog:billdog-notifications:1.1.0-beta.3")
    implementation("io.billdog:billdog-virtual-currency:1.1.0-beta.3")
}
```

### 3. (Optional) Use the BOM

Import the BOM to align all module versions and omit them on each dependency:

```kotlin
dependencies {
    implementation(platform("io.billdog:billdog-bom:1.1.0-beta.3"))
    implementation("io.billdog:billdog-compose")
    implementation("io.billdog:billdog-analytics")
}
```

<details>
<summary>Alternative: GitHub Packages</summary>
<br>

The same artifacts are also mirrored to this repo's GitHub Packages registry. It requires a GitHub Personal Access Token with `read:packages` even to download, so **Maven Central above is the recommended path**. If you specifically need the GitHub Packages mirror:

```kotlin
// settings.gradle.kts
maven {
    url = uri("https://maven.pkg.github.com/billdogai/billdog-android-full")
    credentials {
        username = providers.gradleProperty("gpr.user").orNull ?: System.getenv("GITHUB_ACTOR")
        password = providers.gradleProperty("gpr.key").orNull ?: System.getenv("GITHUB_TOKEN")
    }
}
```

with `gpr.user` / `gpr.key` (your PAT) in `~/.gradle/gradle.properties`.

</details>

---

## ⚡ Quick start

Initialize once, in your `Application`:

```kotlin
import android.app.Application
import io.billdog.core.BillDog

class MyApp : Application() {
    override fun onCreate() {
        super.onCreate()
        BillDog.configure(this, "YOUR_API_KEY")
    }
}
```

Then, anywhere in your app:

```kotlin
// Track a product / analytics event
BillDog.trackEvent("checkout_started", mapOf("plan" to "pro"))

// Read entitlements for a product
val entitlements = BillDog.entitlementsForProduct("premium_monthly")

// Preload paywalls for snappy presentation
BillDog.preloadPaywalls(listOf("onboarding", "settings_upsell"))

// End the session / clear identity
BillDog.logOut()
BillDog.reset()
```

Paywalls render through **`billdog-compose`** (the `PaywallScreen` composable / `presentPaywall(...)`) or the **`billdog-webview`** fallback renderer.

---

## ✅ Requirements

- **minSdk 21** (Android 5.0+)
- **Java 17** toolchain
- Jetpack Compose (for `billdog-compose` / `billdog-survey-compose`)

---

## 📦 Modules at a glance

| Artifact | Purpose |
|---|---|
| `billdog` | Full-suite umbrella |
| `billdog-core` | Identity + billing foundation |
| `billdog-auth` | Native authentication and session management |
| `billdog-paywall` | Paywall engine |
| `billdog-compose` | Compose paywall renderer |
| `billdog-webview` | HTML/CSS paywall renderer |
| `billdog-analytics` | Events + user properties |
| `billdog-ab-test` | A/B testing |
| `billdog-caching` | Offline caching |
| `billdog-replay` | Session replay |
| `billdog-survey` / `billdog-survey-compose` | Surveys (engine + UI) |
| `billdog-inappmessages` | In-app messaging |
| `billdog-notifications` | Push notifications |
| `billdog-virtual-currency` | Virtual currency wallet |
| `billdog-bom` | Version alignment |

---

## 🔗 Eng edition

The `io.billdog:billdog-eng` suite is published separately at **[billdog-android-eng](https://github.com/billdogai/billdog-android-eng)**. It resolves its shared dependencies from **this** registry, so Eng consumers add **both** Maven URLs. `billdog-eng` and the full `billdog` umbrella are mutually exclusive — use one or the other.

---

## 📄 License

MIT — see the published POM metadata. Artifacts are compiled AARs; **source is not distributed**.

<sub>© BillDog.io</sub>
