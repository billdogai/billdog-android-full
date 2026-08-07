# BillDog Android SDK — Full

All-in-one **Monetization, Engagement, and Data** platform for Android. This registry hosts the **compiled AAR artifacts** (no source) for the full BillDog SDK, distributed via **GitHub Packages (Maven)**.

> **Version:** `1.0.0-beta.2`  ·  **Group:** `io.billdog`

---

## 1. Repository setup

GitHub Packages requires authentication **even to download**. Create a GitHub **Personal Access Token** with the `read:packages` scope, then add the registry to your `settings.gradle.kts` (or `settings.gradle`):

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven {
            url = uri("https://maven.pkg.github.com/billdogai/billdog-android-full")
            credentials {
                username = providers.gradleProperty("gpr.user").orNull ?: System.getenv("GITHUB_ACTOR")
                password = providers.gradleProperty("gpr.key").orNull ?: System.getenv("GITHUB_TOKEN")
            }
        }
    }
}
```

Put your credentials in `~/.gradle/gradle.properties` (never commit them):

```properties
gpr.user=YOUR_GITHUB_USERNAME
gpr.key=YOUR_GITHUB_PAT_WITH_read:packages
```

## 2. Add dependencies

```kotlin
dependencies {
    // Core + native Compose paywalls (pulls in billdog-core automatically)
    implementation("io.billdog:billdog-compose:1.0.0-beta.2")

    // Optional feature modules
    implementation("io.billdog:billdog-analytics:1.0.0-beta.2")
    implementation("io.billdog:billdog-replay:1.0.0-beta.2")
    implementation("io.billdog:billdog-survey-compose:1.0.0-beta.2")
    implementation("io.billdog:billdog-inappmessages:1.0.0-beta.2")
    implementation("io.billdog:billdog-notifications:1.0.0-beta.2")
    implementation("io.billdog:billdog-virtual-currency:1.0.0-beta.2")
}
```

Prefer centralized versions? Import the BOM and omit versions on the modules:

```kotlin
dependencies {
    implementation(platform("io.billdog:billdog-bom:1.0.0-beta.2"))
    implementation("io.billdog:billdog-compose")
    implementation("io.billdog:billdog-analytics")
}
```

---

## Available modules

| Artifact | Description |
|---|---|
| `billdog` | Full-suite umbrella (bundles the core modules) |
| `billdog-core` | Foundational identity + billing APIs |
| `billdog-paywall` | Paywall engine |
| `billdog-compose` | Native Jetpack Compose paywall renderer |
| `billdog-webview` | HTML/CSS fallback renderer |
| `billdog-analytics` | Event tracking + user properties |
| `billdog-ab-test` | Experiments / A-B testing |
| `billdog-caching` | Offline + response caching |
| `billdog-replay` | Session replay with privacy masking |
| `billdog-survey` / `billdog-survey-compose` | Survey engine + Compose UI |
| `billdog-inappmessages` | Trigger-based in-app messaging |
| `billdog-notifications` | Push notification management |
| `billdog-virtual-currency` | Server-synced wallet |
| `billdog-bom` | Bill of Materials (version alignment) |

> **Eng edition?** The `io.billdog:billdog-eng` suite lives in a separate registry:
> [`billdog-android-eng`](https://github.com/billdogai/billdog-android-eng). It resolves its
> shared dependencies from **this** repository, so eng consumers add **both** Maven URLs.

---

_Artifacts are compiled AARs; source is not distributed. © BillDog._
