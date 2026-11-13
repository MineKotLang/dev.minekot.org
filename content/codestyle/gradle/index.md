---
title: "Gradle Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-gradle\"></i> "
weight: 1
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for Gradle build scripts. All build scripts are written in `Gradle Kotlin DSL (*.gradle.kts)`. A Gradle source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide enforces precise architectural boundaries and stylistic consistency to support a scalable, maintainable, and community-driven ecosystem.

Gradle build scripts follow the Kotlin Style Guide with additional build-system-specific requirements and best practices.

## 2 Source file basics

### 2.1 File encoding

Source files are encoded in UTF-8 without BOM.

### 2.2 Line endings

Source files end with an `LF (Line Feed)` line ending and insert a single final newline character at the end of the file.

### 2.3 Language standard

Code identifiers, documentation, and user-facing strings are written in American English.

## 3 Source file structure

### 3.1 Plugin application

Plugins are applied using the plugins block with type-safe accessors:

[//]: # (@formatter:off)
`````kotlin
// Correct
plugins {
    kotlin("jvm") version "2.4.0"
    id("org.jetbrains.kotlinx.kover") version "0.7.3"
    id("minekot.module")
}

// Incorrect
apply(plugin = "kotlin")
apply(plugin = "org.jetbrains.kotlinx.kover")
`````
[//]: # (@formatter:on)

### 3.2 Build script organization

Build scripts are organized in the following sequence:

1. Plugin application block
2. Build script configuration (repositories, dependencies)
3. Project configuration (extensions, tasks)
4. Custom task definitions

### 3.3 Convention plugins

Convention plugins are used to share build logic across projects. Build logic is extracted to `build-logic` or included builds rather than duplicated across subprojects.

## 4 Formatting

### 4.1 Column limit

Gradle Kotlin DSL code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 4.2 Formatter control

Formatter control tags are enabled. Code regions can be excluded from formatting using `@formatter:off` and `@formatter:on` tags:

[//]: # (@formatter:off)
`````kotlin
// Correct
// @formatter:off
val matrix = intArrayOf(1,2,3,4,5,6,7,8,9)
// @formatter:on
`````

[//]: # (@formatter:on)

### 4.3 Indentation

The global default indent size is four spaces, and the tab width is four spaces. Continuation indent size is eight spaces.

### 4.4 Block formatting

Blocks (plugins, dependencies, repositories, tasks) use the following formatting:

[//]: # (@formatter:off)
`````kotlin
// Correct
plugins {
    kotlin("jvm") version "2.4.0"
    id("minekot.module")
}

dependencies {
    implementation("org.jetbrains.kotlin:kotlin-stdlib:2.4.0")
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.0")
}

// Incorrect
plugins { kotlin("jvm") version "2.4.0" }

dependencies { implementation("org.jetbrains.kotlin:kotlin-stdlib:2.4.0") }
`````
[//]: # (@formatter:on)

### 4.5 Dependency declarations

Dependency declarations must use version catalogs. Inline version strings are not allowed:

[//]: # (@formatter:off)
`````kotlin
// Correct - Version catalog
dependencies {
    implementation(libs.kotlin.stdlib)
    testImplementation(libs.junit.jupiter)
}

// Incorrect - Short notation
dependencies {
    implementation(kotlin("stdlib"))
}

// Incorrect - Inline version
dependencies {
    implementation("org.jetbrains.kotlin:kotlin-stdlib:2.4.0")
}
`````
[//]: # (@formatter:on)

## 5 Dependency management

### 5.1 Version catalogs

Version catalogs are required for all dependency management. The `libs.versions.toml` file defines all library versions, and build scripts must reference dependencies through the catalog rather than using inline version strings.

[//]: # (@formatter:off)
`````toml
[versions]
kotlin = "2.4.0"
junit = "5.10.0"

[libraries]
kotlin-stdlib = { module = "org.jetbrains.kotlin:kotlin-stdlib", version.ref = "kotlin" }
junit-jupiter = { module = "org.junit.jupiter:junit-jupiter", version.ref = "junit" }

[plugins]
kotlin-jvm = { id = "org.jetbrains.kotlin.jvm", version.ref = "kotlin" }
`````
[//]: # (@formatter:on)

### 5.2 Dependency scopes

Dependencies are declared with appropriate scopes:

1. **implementation:** For dependencies used at compile time and runtime
2. **api:** For dependencies exposed to consumers of the project
3. **compileOnly:** For dependencies used only at compile time
4. **runtimeOnly:** For dependencies used only at runtime
5. **testImplementation:** For dependencies used in tests
6. **testRuntimeOnly:** For dependencies used only at test runtime

### 5.3 Transitive dependencies

Transitive dependencies are explicitly excluded when necessary:

[//]: # (@formatter:off)
`````kotlin
// Correct
implementation("com.example:library:1.0.0") {
    exclude(group = "org.unwanted", module = "unwanted-module")
}
`````
[//]: # (@formatter:on)

## 6 Task configuration

### 6.1 Task naming

Task names use `camelCase` and describe the action performed:

[//]: # (@formatter:off)
`````kotlin
// Correct
tasks.register("generateDocumentation") {
    // Implementation
}

tasks.named("test") {
    // Configuration
}

// Incorrect
tasks.register("generate_documentation") {
    // Implementation
}
`````
[//]: # (@formatter:on)

### 6.2 Task dependencies

Task dependencies are declared explicitly using `dependsOn`:

[//]: # (@formatter:off)
`````kotlin
// Correct
tasks.named("check") {
    dependsOn("formatKotlin")
}

tasks.register("formatKotlin") {
    // Implementation
}

// Incorrect
tasks.named("check") {
    // Implicit dependency through task name
}
`````
[//]: # (@formatter:on)

### 6.3 Task configuration avoidance

Task configuration avoidance is used to improve build performance. `register` creates tasks lazily, `named` configures a known task lazily, and `withType().configureEach` configures every realized task of a type without eagerly realizing the collection:

[//]: # (@formatter:off)
`````kotlin
// Correct - Known task
tasks.named<Test>("test") {
    useJUnitPlatform()
}

// Correct - Every realized task of a type
tasks.withType<Test>().configureEach {
    useJUnitPlatform()
}

// Incorrect - Eager lookup
tasks.getByName<Test>("test").useJUnitPlatform()
`````
[//]: # (@formatter:on)

## 7 Repository configuration

### 7.1 Repository declaration

Repositories are declared in the repositories block:

[//]: # (@formatter:off)
`````kotlin
// Correct
repositories {
    mavenLocal()
    maven("https://maven.fabricmc.net/")
    mavenCentral()
}

// Incorrect
repositories {
    maven { url = uri("https://maven.fabricmc.net/") }
}
`````
[//]: # (@formatter:on)

### 7.2 Repository ordering

Repositories are ordered from the most specific to the most general:

1. Local Maven repository
2. Project-specific repositories
3. Maven Central
4. Other public repositories

## 8 Extension configuration

### 8.1 Type-safe accessors

Type-safe accessors are used for plugin extensions:

[//]: # (@formatter:off)
`````kotlin
// Correct
tasks.withType<KotlinJvmCompile>().configureEach {
    compilerOptions {
        jvmTarget.set(JvmTarget.JVM_21)
    }
}

// Incorrect
tasks.withType(KotlinCompile::class.java).configureEach {
    (kotlinOptions as KotlinJvmOptions).jvmTarget = "17"
}
`````
[//]: # (@formatter:on)

### 8.2 Extension blocks

Extension blocks are used for plugin configuration:

[//]: # (@formatter:off)
`````kotlin
// Correct
configure<KotlinJvmProjectExtension> {
    explicitApi()
}

// Incorrect
the<JavaPluginExtension>().sourceCompatibility = JavaVersion.VERSION_17
`````
[//]: # (@formatter:on)

## 9 Multi-project builds

### 9.1 Project structure

Multi-project builds use a clear hierarchical structure:

[//]: # (@formatter:off)
`````tree
- root | folder
  - build.gradle.kts | devicons devicons-kotlin-icon
  - settings.gradle.kts | devicons devicons-kotlin-icon
  - build-logic | folder
  - subproject-a | folder
    - build.gradle.kts | devicons devicons-kotlin-icon
  - subproject-b | folder
    - build.gradle.kts | devicons devicons-kotlin-icon
`````
[//]: # (@formatter:on)

### 9.2 Subproject configuration

Subproject configuration is done in individual build files or through convention plugins:

[//]: # (@formatter:off)
`````kotlin
// Correct - Convention plugin
plugins {
    id("minekot.library")
}

// Correct - Individual configuration
dependencies {
    implementation(projects.core)
}

// Also correct - allProjects/subProjects for cross-cutting configuration
allprojects {
    group = "org.minekot"
    version = "1.0.0"
}

subprojects {
    apply(plugin = "java")
}

// Incorrect - Overly specific configuration in subprojects
subprojects {
    dependencies {
        implementation(project(":core"))
    }
}
`````
[//]: # (@formatter:on)

`allProjects` and `subProjects` are allowed for cross-cutting configuration such as group, version, repository settings, and plugin application that applies uniformly across projects. Project-specific configuration remains in individual build files or convention plugins. With `NO_IMPLICIT_LOOKUP_IN_PARENT_PROJECTS` enabled, dependencies and configurations are not implicitly resolved from parent projects.

### 9.3 Project dependencies

Project dependencies use type-safe project accessors enabled by `TYPESAFE_PROJECT_ACCESSORS`:

[//]: # (@formatter:off)
`````kotlin
// Correct - Type-safe project accessor
dependencies {
    implementation(projects.core)
    testImplementation(projects.core.test)
}

// Incorrect - String-based project notation
dependencies {
    implementation(project(":core"))
    testImplementation(project(":core", "test"))
}

// Incorrect - File-based dependencies
dependencies {
    implementation(files("../core/build/libs/core.jar"))
}
`````
[//]: # (@formatter:on)

## 10 Build logic

### 10.1 Convention plugins

Build logic is extracted to convention plugins in `build-logic`:

[//]: # (@formatter:off)
`````kotlin
// build-logic/src/main/kotlin/minekot.library.gradle.kts
plugins {
    kotlin("jvm")
}

dependencies {
    implementation(libs.kotlin.stdlib)
}
`````
[//]: # (@formatter:on)

### 10.2 Custom tasks

Custom tasks are defined as classes in `buildSrc` or convention plugins:

[//]: # (@formatter:off)
`````kotlin
// Correct
abstract class GenerateDocumentation : DefaultTask() {
    @Input
    abstract val inputDir: RegularFileProperty

    @OutputDirectory
    abstract val outputDir: DirectoryProperty

    @TaskAction
    fun generate() {
        // Implementation
    }
}

// Also correct - Simple doFirst/doLast for task hooks
tasks.named("build") {
    doLast {
        println("Build completed")
    }
}

// Incorrect - Complex inline logic
tasks.register("generateDocumentation") {
    doLast {
        // Complex inline logic that should be a task class
    }
}
`````
[//]: # (@formatter:on)

`doFirst` and `doLast` blocks are allowed for simple task hooks, logging, and lightweight operations. Complex logic is extracted to task classes.

### 10.3 Build script compilation

Build scripts are compiled for type safety. Dynamic Groovy methods are forbidden. `GROOVY_COMPILATION_AVOIDANCE` is enabled to avoid unnecessary Groovy compilation overhead.

## 11 Naming conventions

### 11.1 Task names

Task names use `camelCase` and start with a verb:

[//]: # (@formatter:off)
`````kotlin
// Correct
tasks.register("generateDocumentation")
tasks.register("runIntegrationTests")
tasks.register("publishToMavenLocal")

// Incorrect
tasks.register("documentation_generator")
tasks.register("integration_tests")
`````
[//]: # (@formatter:on)

### 11.2 Extension names

Extension names use `camelCase`:

[//]: # (@formatter:off)
`````kotlin
// Correct
extensions.create("minekot", MineKotExtension::class)

// Incorrect
extensions.create("MineKot", MineKotExtension::class)
`````
[//]: # (@formatter:on)

### 11.3 Configuration names

Configuration names use `camelCase`:

[//]: # (@formatter:off)
`````kotlin
// Correct
configurations.create("integrationTestImplementation")
configurations.create("integrationTestRuntimeOnly")

// Incorrect
configurations.create("integration_test_implementation")
`````
[//]: # (@formatter:on)

## 12 Architecture and design

### 12.1 Pinned compatibility baseline

Each MineKot Toolchain release declares and tests exact Java, Gradle, Kotlin, and Detekt versions. Build scripts use that release baseline rather than assuming compatibility with arbitrary newer or older versions. Baseline changes are reviewed and tested as explicit compatibility work.

### 12.2 Declarative builds

Build scripts are declarative rather than imperative. Configuration is done through plugins and extensions rather than imperative logic.

### 12.3 Convention over configuration

Convention plugins are used to establish project standards. Individual projects override conventions only when necessary.

### 12.4 Build logic separation

Build logic is separated from project code. Complex build logic is implemented in convention plugins or custom task classes, not inline in build scripts.

### 12.5 Reproducible builds

Builds are configured to be reproducible:

1. Fixed dependency versions
2. No dynamic version ranges
3. Resolution rules for conflict resolution
4. Build cache enabled

### 12.6 Feature previews

Gradle feature previews are enabled in `settings.gradle.kts` to leverage stable, performance-improving, and type-safe features:

[//]: # (@formatter:off)
`````kotlin
// settings.gradle.kts
enableFeaturePreview("TYPESAFE_PROJECT_ACCESSORS")
enableFeaturePreview("STABLE_CONFIGURATION_CACHE")
enableFeaturePreview("GROOVY_COMPILATION_AVOIDANCE")
enableFeaturePreview("NO_IMPLICIT_LOOKUP_IN_PARENT_PROJECTS")
`````
[//]: # (@formatter:on)

The following feature previews are required:

1. **TYPESAFE_PROJECT_ACCESSORS**: Enables type-safe project accessors instead of string-based project references.
2. **STABLE_CONFIGURATION_CACHE**: Enables the configuration cache for faster builds.
3. **GROOVY_COMPILATION_AVOIDANCE**: Avoids unnecessary Groovy compilation.
4. **NO_IMPLICIT_LOOKUP_IN_PARENT_PROJECTS**: Prevents implicit lookups in parent projects, improving build script clarity and performance.

## 13 Documentation and comments

### 13.1 Build script documentation

Complex build logic is documented with KDoc:

[//]: # (@formatter:off)
`````kotlin
/**
 * Configures the Kotlin compilation for the project.
 * Sets the JVM target to 21 and enables explicit API mode.
 */
fun Project.configureKotlinCompilation() {
    tasks.withType<KotlinJvmCompile>().configureEach {
        compilerOptions {
            jvmTarget.set(JvmTarget.JVM_21)
        }
    }
}
`````
[//]: # (@formatter:on)

### 13.2 Task documentation

Custom tasks are documented with KDoc explaining purpose, inputs, and outputs:

[//]: # (@formatter:off)
`````kotlin
/**
 * Generates documentation from source files.
 *
 * @property inputDir The directory containing source files.
 * @property outputDir The directory where documentation will be generated.
 */
abstract class GenerateDocumentation : DefaultTask() {
    @Input
    abstract val inputDir: RegularFileProperty

    @OutputDirectory
    abstract val outputDir: DirectoryProperty

    @TaskAction
    fun generate() {
        // Implementation
    }
}
`````
[//]: # (@formatter:on)

### 13.3 Inline comments

Build scripts prioritize self-documenting structure through clear naming and organization. Inline comments are used only for complex build logic that cannot be made self-evident through refactoring.

## 14 Performance and optimization

### 14.1 Configuration avoidance

Task configuration avoidance is used to improve build performance. Tasks are configured lazily using `named()` or `register()`.

### 14.2 Build cache

The build cache is enabled for reproducible and faster builds. `STABLE_CONFIGURATION_CACHE` is enabled to cache configuration phase results:

[//]: # (@formatter:off)
`````kotlin
buildCache {
    local {
        directory = rootProject.layout.buildDirectory.dir("build-cache")
    }
}
`````
[//]: # (@formatter:on)

### 14.3 Parallel execution

Parallel execution is enabled for multi-project builds:

[//]: # (@formatter:off)
`````kotlin
// gradle.properties
org.gradle.parallel = true
org.gradle.caching = true
org.gradle.configureondemand = true
`````
[//]: # (@formatter:on)

### 14.4 Dependency resolution

Dependency resolution is configured for performance:

[//]: # (@formatter:off)
`````kotlin
configurations.all {
    resolutionStrategy {
        cacheDynamicVersionsFor(10, "minutes")
        cacheChangingModulesFor(0, "seconds")
    }
}
`````
[//]: # (@formatter:on)

## 15 Testing

### 15.1 Test configuration

Test tasks are configured with appropriate frameworks:

[//]: # (@formatter:off)
`````kotlin
tasks.named<Test>("test") {
    useJUnitPlatform()
    maxParallelForks = Runtime.getRuntime().availableProcessors()
}
`````
[//]: # (@formatter:on)

### 15.2 Test source sets

Additional test source sets are configured when needed:

[//]: # (@formatter:off)
`````kotlin
val integrationTest = sourceSets.create("integrationTest") {
    compileClasspath += sourceSets.main.get().output
    runtimeClasspath += sourceSets.main.get().output
}

configurations[integrationTest.implementationConfigurationName].extendsFrom(configurations.testImplementation.get())
`````
[//]: # (@formatter:on)

## Conclusion

Adherence to this style guide ensures that MineKot build scripts remain consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project's build infrastructure.
