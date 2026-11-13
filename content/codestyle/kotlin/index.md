---
title: "Kotlin Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-kotlin-icon\"></i> "
weight: 1
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in the `Kotlin Programming Language`. A `Kotlin` source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide builds upon the official `Kotlin` coding conventions, enforcing architectural and stylistic overrides tailored for the MineKot ecosystem. Any aspect not explicitly covered by this guide follows the official Kotlin coding conventions.

## 2 Source file basics

### 2.1 File encoding

Source files are encoded in UTF-8 without BOM.

### 2.2 Line endings

Source files end with an `LF (Line Feed)` line ending and insert a single final newline character at the end of the file.

### 2.3 Language standard

Code identifiers, documentation, and user-facing strings are written in American English.

## 3 Source file structure

### 3.1 Imports

Import statements are managed according to the following guidelines:

1. `Star imports`: `Wildcard (star) imports` are used when importing five or more names from a single package, or three or more members from a specific scope.
2. `On-demand packages`: Packages such as `java.util.*`, `kotlinx.android.synthetic.**`, and `io.ktor.**` are imported on demand.
3. `Nested classes` are not imported via `static import`.

## 4 Formatting

### 4.1 Column limit

`Kotlin` code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 4.2 Formatter control

Formatter control tags are enabled. Code regions can be excluded from formatting using `@formatter:off` and `@formatter:on` tags.

### 4.3 Indentation

The global default indent size is four spaces, and the tab width is four spaces. For `YAML` and `JSON` files (`*.yml`, `*.yaml`, `*.json`, `*.json5`, `*.jsonc`), the indent size is `four spaces`, and the tab width is `four spaces`. Continuation indent size is `eight spaces`.

### 4.4 Trailing commas

`Trailing commas` are allowed and enforced in multiple contexts, including `collections`, `parameter lists`, and `type arguments`. Trailing commas are enabled for `collection literals`, `context receiver lists`, `destructuring declarations`, `function literals`, `indices`, `type argument lists`, `type parameter lists`, `value argument lists`, `value parameter lists`, and `when entries`.

### 4.5 String templates

When embedding variables, properties, or methods within a string template, curly braces (`${}`) are required. Omission of curly braces for simple variables is not allowed.

[//]: # (@formatter:off)
`````kotlin
// Correct
val greeting = "Hello, ${name}!"

// Incorrect
val greeting = "Hello, $name!"
`````
[//]: # (@formatter:on)

### 4.6 Blank lines

No blank lines are kept before the `right braces` in code and declarations. There are no blank lines after `class headers` or around `block when branches`. One blank line is kept before declarations with comments or annotations on separate lines.

## 5 Language features and ecosystem

### 5.1 Library preference

If a `Kotlin standard library` function, class, or data structure exists, it is used instead of its `Java` equivalent (e.g., use `Kotlin collections` over `java.util` equivalents).

### 5.2 Kotlinx and MineKot APIs

Core system functionality uses APIs in this order:

1. Kotlin standard-library and `kotlinx` APIs.
2. MineKot helper APIs when Kotlin and `kotlinx` do not provide the required capability.
3. Platform or JDK APIs inside MineKot helper implementations and platform adapters.
4. Documented native escape hatches when a portable abstraction would lose required behavior.

`kotlinx-serialization` is used for serialization and supported data formats. `kotlinx-io` is used for portable file and stream operations. `kotlinx-coroutines` owns asynchronous lifecycles, cancellation, delays, and background concurrency. Missing portable capabilities are added to demand-driven MineKot helper libraries rather than repeatedly implemented by consumers.

### 5.3 Versioning

Each MineKot release pins and tests an exact Kotlin, Java, Gradle, and toolchain dependency baseline. Projects use the baseline declared by their MineKot Toolchain release. Baseline upgrades are deliberate compatibility changes rather than an open-ended requirement to track untested latest versions.

### 5.4 Code style defaults

Kotlin code follows the official Kotlin code style defaults (`KOTLIN_OFFICIAL`) with MineKot-specific overrides as defined in this guide.

## 6 Programming practices

### 6.1 Error handling

Recoverable failures are represented with `Result`. Consumer code uses typed MineKot catching helpers that convert only declared recoverable exception types to `Result.failure`, always preserve coroutine cancellation, and rethrow unrelated or fatal failures. Results are handled explicitly with transformations or terminal operations such as `onSuccess`, `onFailure`, `recover`, `getOrElse`, and `getOrNull`.

Structured `try-catch` and `try-finally` remain valid inside exception-boundary helpers, platform adapters, exception translation, and cleanup logic where Result chains cannot express the required semantics safely. Broad catches, swallowed failures, and cancellation conversion are not allowed.

### 6.2 Explicit scope resolution

When operating within `nested scopes`, `lambdas`, `coroutines`, or `builder DSLs`, relying on an implicit `this` to access properties or methods of an outer class is not allowed. The receiver is explicitly qualified using labeled `this` references to prevent `shadowing`, `context leaks`, and `ambiguity`.

[//]: # (@formatter:off)
`````kotlin
// Correct
class MineKotEngine {
    val engineName = "MineKot"

    fun initialize() {
        runCatching {
            this@MineKotEngine.logger.info("Starting ${this@MineKotEngine.engineName}")
        }
    }
}

// Incorrect
class MineKotEngine {
    val engineName = "MineKot"

    fun initialize() {
        runCatching {
            logger.info("Starting ${engineName}") // Ambiguous implicit `this`
        }
    }
}
`````
[//]: # (@formatter:on)

### 6.3 Concurrency

Kotlin coroutines own asynchronous lifecycles, cancellation, delays, and background concurrency. Portable code uses MineKot platform dispatch abstractions when work must enter a platform-owned thread. Direct Bukkit, Paper, Fabric, NeoForge, or Velocity scheduler access is restricted to platform adapters implementing those abstractions. Blocking database and IO operations run on appropriate dispatchers and never block platform event threads.

### 6.4 Spacing

Spaces are used around `additive`, `assignment`, `equality`, `logical`, `multiplicative`, and `relational operators`. Spaces are used around the `elvis operator`, `function type` arrows, and `when` arrows. No spaces are used around the `range operator` or `unary operators`. A space is used before the `lambda arrow`. Spaces are used after `commas`, after `extend colons`, and after `type colons`. No space is used before `commas` or before `type colons`. A space is used before `extend colons`.

### 6.5 Iteration

`for` loops and `forEach` are selected by semantics and readability; neither form is universally preferred. `forEach` suits simple side-effect iteration without non-local control flow. `for` loops suit `break` or `continue`, mutation-sensitive logic, indexed traversal, suspend/control-flow clarity, and cases where loop syntax is simpler.

[//]: # (@formatter:off)
`````kotlin
// Correct
players.forEach { player ->
    player.sendMessage("Welcome!")
}

// Also correct
for (player in players) {
    player.sendMessage("Welcome!")
}
`````
[//]: # (@formatter:on)

Automatic conversion between these forms is allowed only when receiver evaluation, iteration order, mutation behavior, labels, and control flow are proven equivalent.

### 6.6 Property assignment

Property assignment uses the `=` operator instead of `.set()` methods. The `=` syntax is more idiomatic, concise, and consistent with Kotlin's property system.

[//]: # (@formatter:off)
`````kotlin
// Correct
player.displayName = "New Name"
config.maxPlayers = 100

// Incorrect
player.setDisplayName("New Name")
config.setMaxPlayers(100)
`````
[//]: # (@formatter:on)

The `.set()` pattern is only used when required by a third-party API that does not provide property assignment syntax.

### 6.7 No hardcoding

User-facing text, messages, and labels are not hardcoded directly in source code. All localizable strings are defined in language files and referenced through localization keys. This enables internationalization, easy updates without code changes, and consistency across the codebase.

[//]: # (@formatter:off)
`````kotlin
// Correct
player.sendMessage(Component.text(translate("messages.welcome", player.locale)))

// Incorrect
player.sendMessage(Component.text("Welcome to the server!"))
`````
[//]: # (@formatter:on)

Language files are organized by locale (e.g., `en_us.lang`, `de_de.lang`) and stored in the appropriate 'resources' directory. Keys follow a hierarchical naming convention (e.g., `category.subcategory.message`) to maintain organization and prevent conflicts.

Exceptions to this rule include:

- Technical error messages intended for developers
- Debug or diagnostic output
- Protocol-defined constants and identifiers
- Generated or template-based content where localization is not applicable

## 7 Architecture and design

### 7.1 Data-driven design

The codebase is `data-driven`. Behavior, `configuration parameters`, and `structural logic` are abstracted into `data structures` or `serialization formats` rather than being baked directly into the execution flow.

### 7.2 Intent-based value extraction

Values are extracted into constants, localization, configuration, or serialized data when meaning, reuse, deployment variance, user customization, or localization requires it. Unexplained domain literals, duplicated user-facing text, credentials, environment-specific paths or endpoints, and behavior intended to vary between deployments are not embedded directly in execution flow.

Local structural syntax, protocol-defined constants, one-use obvious values, generated-source templates, named algorithm constants, and stable defaults may remain in source when extraction or configuration would not improve meaning, reuse, safety, or adaptability.

### 7.3 Modular decoupling

There is a rigid separation between `generic library code` and the core proprietary MineKot logic. This abstraction ensures the engine remains `maintainable`, `modular`, and accessible for `open-source community contributions`.

### 7.4 Internal extensions

To eliminate `boilerplate code`, `internal utility extension functions` are created and used.

### 7.5 Text and localization

MiniMessage is required for rich user-facing text on Adventure-capable surfaces such as chat, UI components, and interactive messages. Localizable text uses localization keys or templates with explicit placeholders. Legacy color codes such as `&` and `§` and string concatenation are not used to construct rich user-facing components.

Structured logs, exceptions, build output, diagnostics, protocol values, file formats, and machine-readable output remain plain or structured text unless their destination explicitly supports Adventure components.

## 8 Documentation and comments

### 8.1 API documentation

Public and protected APIs require KDoc that explains their consumer-visible contract. Internal declarations require KDoc when behavior, invariants, lifecycle, ownership, failure semantics, threading, or usage constraints are not obvious from names and types. Private declarations, self-evident internal declarations, and overrides that add no contract are exempt.

Parameter, property, and return tags are included only when their prose adds information beyond the declaration's names and types. Generated or placeholder documentation that merely restates a signature is not allowed.

### 8.2 Property documentation

Properties follow the same contract-based policy. Collections and state holders do not require KDoc solely because of their type; documentation is required when mutation, ownership, synchronization, lifecycle, units, bounds, or other non-obvious semantics matter.

### 8.3 Inline comments

The codebase prioritizes `self-documenting logic` through expressive naming and structure. `Inline comments` (`//`) are used only when the underlying `algorithm` or logic is inherently complex and requires a plain-text explanation to be understood by future maintainers.

### 8.4 Comment formatting

`Block comments` are indented to the same level as the surrounding code. A space is added after the opening `/*` marker and before the closing `*/` marker. `Line comments` are indented to the same level as the surrounding code. A space is added after the `//` marker.

### 8.5 Test comments

Comments are allowed and encouraged in test code. Test comments explain test intent, describe edge cases, document expected behavior, and clarify complex test scenarios. Unlike production code, test code benefits from explicit documentation of what is being tested and why.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
