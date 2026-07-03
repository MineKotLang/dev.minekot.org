---
title: "Coding Standards & Style"
params:
  menuPre: "<i class=\"fa-solid fa-file-code\"></i> "
weight: 1
---

## Overview

MineKot enforces comprehensive coding standards across all languages and file types used in the ecosystem. These standards ensure consistency, readability, and maintainability across all projects.

## CI Enforcement

All coding standards are enforced through the MineKot Toolchain for Gradle. The toolchain provides automated linting, formatting, and validation during the build process. Non-compliant code will fail CI checks, ensuring that all committed code adheres to these standards.

## Language-Specific Guides

### Kotlin

Our [Kotlin Style Guide]({{% relref "codestyle/kotlin" %}}) defines the mandatory standards for all MineKot projects.

Key principles include:

- **Modern Standards:** Adherence to official Kotlin conventions with a 120-character column limit.
- **The Kotlinx Mandate:** Mandatory use of `kotlinx-serialization`, `kotlinx-io`, and `kotlinx-coroutines`.
- **Functional Error Handling:** Preferring `runCatching` and `Result` over traditional `try-catch` blocks.
- **Universal Text:** Exclusive use of `MiniMessage` for all user-facing strings and logs.
- **Modular Design:** Strict separation between generic library code and core engine logic.

### TypeScript

The [TypeScript Style Guide]({{% relref "codestyle/typescript" %}}) extends JavaScript standards with type system requirements.

Key principles include:

- **Strict Type Safety:** Mandatory strict mode with explicit type annotations.
- **Modern TypeScript:** ES6+ features, utility types, and discriminated unions.
- **Interface vs Type:** Consistent use of interfaces for extensible shapes and types for unions.
- **No `Any`:** Strict prohibition of the `any` type in favor of `unknown`.

### JavaScript

The [JavaScript Style Guide]({{% relref "codestyle/javascript" %}}) defines standards for JavaScript source code.

Key principles include:

- **ES6+ Features:** Modern JavaScript with const/let, arrow functions, and template literals.
- **ESM Modules:** Mandatory use of ES6 modules over CommonJS.
- **Async/Await:** Asynchronous operations using async/await paradigms.
- **Strict Equality:** Mandatory use of `===` and `!==`.

### Gradle Kotlin DSL

The [Gradle Kotlin DSL Style Guide]({{% relref "codestyle/gradle" %}}) defines standards for build scripts.

Key principles include:

- **Type-Safe Accessors:** Use of type-safe plugin and extension accessors.
- **Version Catalogs:** Centralized dependency management via version catalogs.
- **Convention Plugins:** Build logic extracted to convention plugins for reusability.
- **Declarative Builds:** Preference for declarative configuration over imperative logic.

### HTML

The [HTML Style Guide]({{% relref "codestyle/html" %}}) defines standards for markup files.

Key principles include:

- **Semantic HTML:** Use of semantic elements for accessibility and SEO.
- **Accessibility:** ARIA attributes, alt text, and keyboard navigation support.
- **Zero Hardcoding:** All user-facing text is managed through localization systems.
- **Structural Decoupling:** Separation of structure, style, and behavior.

### CSS

The [CSS Style Guide]({{% relref "codestyle/css" %}}) defines standards for stylesheets.

Key principles include:

- **BEM Methodology:** Block-Element-Modifier naming convention for components.
- **Mobile-First:** Responsive design with a mobile-first approach.
- **CSS Custom Properties:** Use of CSS variables for theming and configuration.
- **Low Specificity:** Simple selectors to avoid specificity wars.

### XML

The [XML Style Guide]({{% relref "codestyle/xml" %}}) defines standards for XML documents.

Key principles include:

- **Schema Validation:** All XML documents must validate against declared schemas.
- **Namespace Management:** Consistent namespace declarations and prefixes.
- **Attribute Ordering:** Immutable hierarchical ordering scheme for attributes.
- **Security:** Avoidance of external entities to prevent XXE attacks.

### YAML

The [YAML Style Guide]({{% relref "codestyle/yaml" %}}) defines standards for configuration files.

Key principles include:

- **Data-Driven:** Configuration as data-driven control maps for the engine.
- **Anchors and Aliases:** Use of anchors and aliases to avoid duplication.
- **Security:** Sensitive data referenced via environment variables.
- **Hierarchical Structure:** Clear hierarchy with minimal nesting.

### JSON

The [JSON Style Guide]({{% relref "codestyle/json" %}}) defines standards for JSON data structures.

Key principles include:

- **Key Naming:** Consistent `kebab-case` naming for keys.
- **Alphabetical Ordering:** Keys ordered alphabetically for consistency.
- **ISO 8601 Dates:** Standard date format for temporal data.
- **Schema Validation:** JSON schemas for documentation and validation.

### Markdown

The [Markdown Style Guide]({{% relref "codestyle/markdown" %}}) defines standards for documentation.

Key principles include:

- **Heading Hierarchy:** Logical heading structure without skipping levels.
- **Code Blocks:** Fenced code blocks with language identifiers.
- **Descriptive Links:** Descriptive link text indicating destination.
- **Document Structure:** Clear organization with introduction, content, and conclusion.

## Tooling Configuration

### Prettier

A [Prettier configuration](/.prettierrc) is provided for JavaScript, TypeScript, CSS, HTML, JSON, and Markdown formatting. The configuration enforces:

- 120-character column limit (999 for Markdown)
- 2-space indentation
- Double quotes
- LF line endings
- No trailing commas in standard JSON

### EditorConfig

An [EditorConfig](/.editorconfig) file is provided to maintain consistent coding styles across different editors and IDEs. The configuration enforces:

- UTF-8 encoding
- LF line endings
- Consistent indentation per file type
- Trailing newline

## Cross-Language Principles

### Data-Driven Design

All configurations, options, and operational constants are defined in data structures or serialization formats rather than hardcoded in the execution flow.

### Zero Hardcoding

Hardcoding of user-facing strings, magic numbers, or strict logic paths is prohibited. Configuration managers and data serialization are used instead.

### Modular Decoupling

Strict separation between generic library code and core proprietary logic ensures maintainability and accessibility for open-source community contributions.

### MiniMessage Mandate

MiniMessage is used exclusively for all UI text, console output, and logging, regardless of whether the project is Minecraft-related.

## Conclusion

Adherence to these style guides ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
