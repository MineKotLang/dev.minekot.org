---
title: "Style & Standards"
params:
  menuPre: "<i class=\"fa-solid fa-list-check\"></i> "
weight: 3
---

This document defines the mandatory code style, language standards, and programming patterns required for contributions to the MineKot toolchain.

## Language and formatting basics

All code identifiers, comments, documentation, and user-facing messages must be written in American English.

- **Kotlin code style** – We enforce a strict Kotlin format. Key elements include a 120-character column limit, mandatory trailing commas for parameters and collections, strict parameter wrapping (newline after opening parenthesis `(`, and closing parenthesis `)` on a new line), and requiring braces for all variable interpolations in string templates (use `${variable}`, never `$variable`).
- **Markdown style** – Markdown documents must follow the MineKot style guide. Files are encoded in UTF-8 without BOM and end with LF. Paragraph blocks must not have hard line breaks; instead, they must flow as single, continuous lines. Headings use sentence casing and must have one blank line before and after. List markers must be hyphens `-`, and nested list elements must be indented by exactly two spaces.

## The kotlinx mandate

To ensure portability and performance across different Minecraft platforms and runtime environments, we forbid raw JVM-centric utility usage in favor of Kotlin multiplatform alternatives:

- **Serialization** - Always use `kotlinx-serialization` for JSON, configuration, and data transport structures. Never use Gson, Jackson, or standard Java object serialization.
- **Input/Output** - Use `kotlinx-io` for all stream reading, writing, buffer pooling, and path resolutions. Avoid Java NIO `Files` or legacy `java.io` stream wrappers unless interacting with platform-specific Java bindings.
- **Concurrency** - Use `kotlinx-coroutines` for all asynchronous tasks, scheduler operations, timeout handling, and thread dispatches. Never spawn raw Java threads or configure `java.util.Timer` schedulers.

## Functional error handling

We follow functional programming principles for managing application flow and handling failures:

- **No raw try-catch blocks** – Do not write traditional `try { ... } catch (e: Exception) { ... }` blocks. The Detekt rule suite analyzes your code and rejects raw try-catch patterns.
- **Use runCatching** - Wrap throwable operations using `runCatching { ... }` to return a `Result` wrapper.
- **Result propagation** – Propagate errors up the call stack by returning `Result<T>` instead of throwing exceptions or returning nullable types.
- **Boilerplate reduction helpers** – Use the built-in parsing adapters inside `minekot-kt-common` to parse objects cleanly:
  - Convert strings to UUIDs using `toMineKotUuidResult()`.
  - Convert strings to enums using `toMineKotEnumResult()`.
  - Load and migrate JSON configurations using `readMineKotConfigResult()` and `migrateMineKotJsonConfig()`.

## User-facing text and formatting

All user-facing text, console logging, chat output, and documentation snippets must be formatted using Kyori Adventure's rich text system:

- **MiniMessage format** – Use the MiniMessage formatting standard (for example, `<yellow>Text</yellow>`) for styling and colors.
- **Legacy color rejection** – Do not use legacy color codes (such as `§` or `&` symbols). The Detekt rule suite checks and flags any legacy format codes.
- **Localization** – Keep text and format strings modular to prepare for future type-safe translation utilities.

## Built-in Detekt rules

The `:plugin:lint-rules` module executes automatically during the `./gradlew check` task. It checks for:

- Presence of braces around string template variables.
- Raw try-catch blocks.
- Usage of Bukkit legacy color formats.
- Public functions lacking proper KDoc documentation.
- Spawning threads instead of leveraging Kotlin coroutines.
- Missing `Result` error checks.
