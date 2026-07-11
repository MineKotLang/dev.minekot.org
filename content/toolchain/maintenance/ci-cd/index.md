---
title: "CI/CD & Pipelines"
params:
  menuPre: "<i class=\"fa-solid fa-gears\"></i> "
weight: 2
---

This document outlines the build infrastructure, continuous integration pipelines, smoke test automation, and Develocity build scan configurations for the MineKot toolchain.

## CI pipeline phases

Our continuous integration workflow runs on GitHub Actions (configured inside `.github/workflows/build.yml`) and triggers on every push and pull request targeting the `master` branch. The pipeline executes the following phases:

1. **Environment setup** – Configures a Linux runner, checks out the codebase, and sets up a JDK 21 environment.
2. **Cache restoration** – Restores cached Gradle directories (including wrappers, dependencies, and build caches) to speed up compile times.
3. **Compilation and check** - Runs `./gradlew check`. This compiles all library and plugin source files, checks for code style compliance, and runs the entire JUnit test suite.
4. **Smoke test validation** - Runs `./gradlew mineKotSmokeTest`. This compiles the current plugin draft, publishes it to a local folder, applies it to the test project in `samples/smoke`, and executes Gradle verification tasks inside the sample project.
5. **Snapshot deployment** - On successful merges to `master`, if the version is a snapshot, it publishes the snapshot artifacts to our hosted Maven repository.

## Gradle configuration cache

To optimize development speed, the build system supports the Gradle configuration cache. When executing tasks, ensure your changes do not break configuration cache compatibility.

You can verify that the configuration cache functions correctly by running:

```shell
./gradlew check --configuration-cache
```

If any plugin tasks access the mutating project state at execution time, they will throw configuration cache violations. Maintainers must resolve these violations using Gradle provider APIs.

## Gradle Develocity integration

We use Develocity to record build scans. This provides insights into task durations, cache efficiency, dependency resolution, and test failures.

Develocity is applied in `settings.gradle.kts`:

```kotlin
plugins {
    id("com.gradle.develocity") version "4.5.0"
}

develocity {
    buildScan {
        termsOfUseUrl = "https://gradle.com/help/legal-terms-of-use"
        termsOfUseAgree = "yes"
        publishing.onlyIf { true }
    }
}
```

Every build executed on the CI runner automatically publishes a build scan. The scan link is printed in the build log outputs for easy debugging.

## CI troubleshooting and optimization

If builds fail or compile slowly, maintainers should review the following settings:

- **Cache keys** – The CI cache actions use hashing of the `gradle/libs.versions.toml` and build scripts to generate cache keys. If caches fail to update on dependency changes, check the cache key definitions in the workflow file.
- **Gradle daemon** - CI builds execute with the `--no-daemon` flag to prevent background processes from consuming runner memory.
- **Smoke test logs** – If the smoke test suite fails, look at the workspace folder inside `samples/smoke/build/` to inspect project logs or test runner reports.
