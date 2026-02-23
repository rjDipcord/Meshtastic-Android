# CLAUDE.md — Meshtastic Android

> **Primary reference for AI agents:** See [`AGENTS.md`](AGENTS.md) for the full architecture guide, module map, and detailed development guidelines. This file provides the essential quick-reference for Claude Code sessions.

## Essential Commands

```bash
# Formatting (required before every commit)
./gradlew spotlessApply

# Linting
./gradlew detekt

# Unit & Robolectric tests
./gradlew test

# Specific module tests
./gradlew :feature:settings:testDebugUnitTest

# Instrumented tests (requires device/emulator)
./gradlew connectedAndroidTest

# Build a specific flavor
./gradlew assembleFdroidDebug
./gradlew assembleGoogleDebug

# Install pre-push formatting hook
./gradlew spotlessInstallGitPrePushHook --no-configuration-cache
```

## Project Structure

| Path | Purpose |
|------|---------|
| `app/` | Main application (`com.geeksville.mesh`) |
| `core/` | Shared library modules (`org.meshtastic.core.*`) |
| `feature/` | Feature modules (`org.meshtastic.feature.*`) |
| `build-logic/` | Custom Gradle convention plugins |
| `gradle/libs.versions.toml` | All dependency versions (edit here only) |
| `core/resources/` | Centralized strings & assets |
| `core/proto/` | Protobuf definitions for device comms |

## Key Conventions

- **Strings:** Never use `app/src/main/res/values/strings.xml`. Add to `core/resources/src/commonMain/composeResources/values/strings.xml` and access via `stringResource(Res.string.your_key)`.
- **Dependencies:** Never hardcode versions in `build.gradle.kts`. All versions go in `gradle/libs.versions.toml`.
- **ViewModels:** Always annotate with `@HiltViewModel` and use `@Inject constructor(...)`.
- **Dialogs:** Use `MeshtasticDialog` (see `core/ui/.../AlertDialogs.kt`).
- **Robolectric tests:** Annotate with `@Config(sdk = [34])` when using Java 17.
- **Build flavors:** `google` (Play Store + Firebase) vs `fdroid` (FOSS — no Crashlytics/Firebase).
- **Versioning:** Never manually edit `versionCode` or `versionName`; managed by CI.
- **Package structure:** Legacy `app/` code uses `com.geeksville.mesh`; new code uses `org.meshtastic.*`.

## Workflow Checklist

1. Read `gradle/libs.versions.toml` and the relevant `build.gradle.kts` before making changes.
2. Identify which `core/` or `feature/` module owns the change.
3. Implement the change following conventions above.
4. Run `./gradlew spotlessApply` (mandatory).
5. Run `./gradlew detekt` and fix any issues.
6. Run relevant tests.
