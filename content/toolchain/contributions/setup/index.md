---
title: "Local Setup"
params:
  menuPre: "<i class=\"fa-solid fa-laptop-code\"></i> "
weight: 1
---

This guide walks you through setting up a local development environment to contribute to the MineKot toolchain.

## Prerequisites

Before cloning and building the project, ensure your development machine has the following tools installed:

- **Java Development Kit (JDK) 21** – The toolchain compiles with Java 21. We recommend using Eclipse Temurin or another standard OpenJDK distribution. Verify your active Java version using `java -version`.
- **Git** - Version control tool for managing branch checkouts and submitting changes.
- **IntelliJ IDEA** – The recommended IDE for developing Kotlin and Gradle plugins. We configure IDE-specific settings (like actions-on-save and formatter options) automatically.

## Getting started

Follow these steps to clone the repository and build it for the first time:

1. Clone the repository to your local machine:
   ```shell
   git clone https://github.com/MineKotLang/minekot-toolchain.git
   cd minekot-toolchain
   ```
2. Run the initialization task to ensure all required configuration files and codestyle files are generated and applied:
   ```shell
   ./gradlew mineKotInitializeProject
   ```
3. Generate the IntelliJ IDEA settings and Detekt configurations:
   ```shell
   ./gradlew writeMineKotCodestyle
   ```
4. Build the project and run the complete test suite to confirm everything compiles and passes on your environment:
   ```shell
   ./gradlew build
   ```

## Common Gradle tasks

The Gradle build defines several tasks that you will use frequently during development:

- `./gradlew mineKotInitializeProject` - Discovers and writes missing project files (such as `.gitattributes`, `LICENSE`, `NOTICE`, `README.md`, and `CHANGELOG.md`) from template resources inside `plugin/toolchain/src/main/resources/project`. It will not overwrite files you have already customized.
- `./gradlew writeMineKotCodestyle` - Writes the Detekt rules file (`config/detekt/minekot.yml`), imports IntelliJ IDEA code style XML settings, and configures workspace defaults to automatically optimize imports and run cleanup actions on save.
- `./gradlew check` - Runs all verification tasks including Detekt static analysis and unit tests across all libraries and plugin modules.
- `./gradlew test` - Runs the unit test suite for all libraries and the plugin.
- `./gradlew mineKotSmokeTest` - Runs the integration/smoke test suite using the sample project under `samples/smoke` to verify the toolchain works in a simulated consumer environment.

## Configuring your IDE

We strongly recommend using IntelliJ IDEA. To import the project:

1. Open IntelliJ IDEA.
2. Select **Open** and choose the `minekot-toolchain` directory (where `settings.gradle.kts` is located).
3. Wait for the Gradle import process to complete.
4. After importing, execute `./gradlew writeMineKotCodestyle`. This task updates `.idea/workspace.xml` to apply code formatter rules, turn on automatic import optimization, and configure actions-on-save.
5. In IntelliJ preferences, navigate to **Build, Execution, Deployment → Build Tools → Gradle** and verify that the Gradle JVM is set to JDK 21.

## Troubleshooting

Here are typical errors you might encounter and how to resolve them:

- **Java version mismatch** – If Gradle reports class file version compatibility errors, ensure your `JAVA_HOME` environment variable points to a JDK 21 installation. You can check this by running `echo $JAVA_HOME` in your shell.
- **Detekt failures after modifying files** - If `./gradlew check` fails on Detekt lint checks, you can try enabling Detekt autocorrection. Run `./gradlew check -PminekotToolchain.lint.autoCorrect=true` to allow Detekt to fix formatting nits automatically.
