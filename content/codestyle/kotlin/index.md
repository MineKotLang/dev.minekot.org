---
title: "Kotlin Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-kotlin-icon\"></i> "
weight: 1
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in the `Kotlin Programming Language`. A `Kotlin` source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide builds upon the official `Kotlin` coding conventions, enforcing architectural and stylistic overrides tailored for the MineKot ecosystem.

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

### 5.2 The Kotlinx mandate

The `kotlinx` suite of libraries is used for `core system functionality`:

1. `kotlinx-serialization` is used for all data parsing and JSON handling.
2. `kotlinx-io` is used for all file and stream operations.
3. `kotlinx-coroutines` is used for all concurrent and asynchronous operations.

### 5.3 Versioning

Projects target the latest stable releases of the `Kotlin compiler`, the `standard library`, and all internal and external dependencies.

### 5.4 Code style defaults

Kotlin code follows the official Kotlin code style defaults (`KOTLIN_OFFICIAL`) with MineKot-specific overrides as defined in this guide.

## 6 Programming practices

### 6.1 Error handling

Standard `try-catch` blocks are not used. Exception handling is managed functionally using `Kotlin's` `runCatching` API. The resulting `Result` type is handled using `onSuccess`, `onFailure`, `getOrElse`, or `getOrNull`.

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

`Kotlin Coroutines` are preferred over standard `threading loops` or legacy `server schedulers` (such as `Bukkit/Paper schedulers`) for all `asynchronous` and `delayed tasks`. `Database operations` and queries are executed asynchronously to prevent `thread blocking`.

### 6.4 Spacing

Spaces are used around `additive`, `assignment`, `equality`, `logical`, `multiplicative`, and `relational operators`. Spaces are used around the `elvis operator`, `function type` arrows, and `when` arrows. No spaces are used around the `range operator` or `unary operators`. A space is used before the `lambda arrow`. Spaces are used after `commas`, after `extend colons`, and after `type colons`. No space is used before `commas` or before `type colons`. A space is used before `extend colons`.

### 6.5 forEach loops

`forEach` is preferred over traditional `for` loops when iterating over collections, arrays, or sequences, provided the loop body does not require early termination (`break` or `continue`). `forEach` provides a more functional and idiomatic approach to iteration.

[//]: # (@formatter:off)
`````kotlin
// Correct
players.forEach { player ->
    player.sendMessage("Welcome!")
}

// Incorrect
for (player in players) {
    player.sendMessage("Welcome!")
}
`````
[//]: # (@formatter:on)

Traditional `for` loops are used when:

- Early termination with `break` or `continue` is required
- Index-based iteration is necessary
- The loop body is complex and would benefit from explicit control flow

## 7 Architecture and design

### 7.1 Data-driven design

The codebase is `data-driven`. Behavior, `configuration parameters`, and `structural logic` are abstracted into `data structures` or `serialization formats` rather than being baked directly into the execution flow.

### 7.2 Zero hardcoding

Hardcoding `user-facing strings`, `magic numbers`, or `strict logic paths` is not allowed. The implementation utilizes `configuration managers` and `data serialization`.

### 7.3 Modular decoupling

There is a rigid separation between `generic library code` and the core proprietary MineKot logic. This abstraction ensures the engine remains `maintainable`, `modular`, and accessible for `open-source community contributions`.

### 7.4 Internal extensions

To eliminate `boilerplate code`, `internal utility extension functions` are created and used.

### 7.5 Text and localization

`MiniMessage` is used exclusively for all `UI text`, `console output`, and `logging`, regardless of whether the project is `Minecraft-related` or not. Legacy `color codes` (e.g., `&` or `§`) and standard `string concatenations` are not used for user-facing text across all repositories.

## 8 Documentation and comments

### 8.1 Method documentation

Every method requires a complete `KDoc` block explaining its purpose, parameters (`@param`), and return type (`@return`).

### 8.2 Variable documentation

`KDoc` is required for "complicated" variables. This includes `collections` (`Map`, `List`, `Set`), `complex state holders`, and instances where the behavior is not immediately evident from the property name alone. Simple, self-evident primitives do not require `KDoc`.

### 8.3 Inline comments

The codebase prioritizes `self-documenting logic` through expressive naming and structure. `Inline comments` (`//`) are used only when the underlying `algorithm` or logic is inherently complex and requires a plain-text explanation to be understood by future maintainers.

### 8.4 Comment formatting

`Block comments` are indented to the same level as the surrounding code. A space is added after the opening `/*` marker and before the closing `*/` marker. `Line comments` are indented to the same level as the surrounding code. A space is added after the `//` marker.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
