---
title: "Testing & Validation"
params:
  menuPre: "<i class=\"fa-solid fa-vial-circle-check\"></i> "
weight: 4
---

We require comprehensive test coverage for all modifications to the MineKot toolchain. This guide outlines our testing strategies, available testing tools, and steps for validating your changes.

## Testing hierarchy

The testing framework is divided into two distinct levels of validation:

1. **Unit tests** – Fast, focused tests that verify the logic of specific functions, classes, and helper libraries.
2. **Smoke/integration tests** – Broad tests that apply the built Gradle plugin and code styles to a real sandbox project to ensure end-to-end compatibility.

## Running tests

You can execute tests from your terminal using the following Gradle commands:

- Run all unit tests:
  ```shell
  ./gradlew test
  ```
- Run unit tests for a specific module:
  ```shell
  ./gradlew :libraries:kotlin:common:test
  ```
- Run the smoke test suite:
  ```shell
  ./gradlew mineKotSmokeTest
  ```
- Run the full verification suite (compilation, Detekt linting, unit tests, and smoke tests):
  ```shell
  ./gradlew check
  ```

## Writing unit tests

Unit tests are located within the `src/test/kotlin` directory of each subproject. We use standard JUnit Jupiter assertions alongside our custom assertions library.

### Asserting on Result types

Because the codebase relies on functional error handling with the `Result` wrapper, you should use the assertions provided by the `:libraries:kotlin:testing` (`minekot-kt-testing`) module:

```kotlin
import org.minekot.testing.assertSuccess
import org.minekot.testing.assertFailure

// Test that a parser functions correctly
val parseResult = toMineKotUuidResult("invalid-uuid-format")
parseResult.assertFailure { exception ->
    exception.message.assertContains("Invalid UUID format")
}

val validResult = toMineKotUuidResult("00000000-0000-0000-0000-000000000000")
validResult.assertSuccess { uuid ->
    assertEquals(0L, uuid.mostSignificantBits)
}
```

### Temporary file management

When testing file IO or configuration loading, never write directly to system folders or project directories. Use the JUnit `@TempDir` annotation to automatically generate and delete sandbox environments:

```kotlin
import org.junit.jupiter.api.io.TempDir
import java.nio.file.Path

class ConfigurationTest {

    @Test
    fun testConfigLoading(@TempDir tempDir: Path) {
        val file = tempDir.resolve("config.json")
        // Run assertions on file contents
    }
}
```

## Programmatic Gradle testing

To test Gradle tasks and plugin configurations, the `minekot-kt-testing` module provides helper utilities for setting up mock Gradle builds using the Gradle Runner API.

These helpers let you:

- Write temporary `build.gradle.kts` files programmatically.
- Run tasks using the Gradle runner.
- Assert that build execution succeeded or failed.
- Check the terminal output logs for specific strings.

## Smoke test configuration

The `mineKotSmokeTest` task validates the toolchain plugin end-to-end. It performs the following steps:

1. Publishes the current build of the Gradle plugin and utility libraries to a temporary, local maven directory.
2. Launches a standalone Gradle project located in `samples/smoke`.
3. Injects the published plugin version into the smoke project's `settings.gradle.kts` and `build.gradle.kts`.
4. Executes the smoke project's verification tasks, ensuring that project initialization, configuration loading, linting, and Adventure formatting rules work correctly.

Any change to the Gradle plugin DSL, task registrations, or library dependencies must be validated by running the smoke test suite before submitting a pull request.
