---
title: "Coding Standards & Style"
type: "default"
weight: 1
---

## Kotlin

Our [Kotlin Style Guide]({{% relref "codestyle/kotlin" %}}) defines the mandatory standards for all MineKot projects.

Key principles include:

- **Modern Standards:** Adherence to official Kotlin conventions with a 120-character column limit.
- **The KotlinX Mandate:** Mandatory use of `kotlinx-serialization`, `kotlinx-io`, and `kotlinx-coroutines`.
- **Functional Error Handling:** Preferring `runCatching` and `Result` over traditional `try-catch` blocks.
- **Universal Text:** Exclusive use of `MiniMessage` for all user-facing strings and logs.
- **Modular Design:** Strict separation between generic library code and core engine logic.

Refer to the full guide for detailed documentation on formatting, naming, and architectural requirements.
