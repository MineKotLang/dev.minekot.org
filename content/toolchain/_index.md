---
title: "MineKot Toolchain"
params:
  menuPre: "<i class=\"fa-solid fa-toolbox\"></i> "
weight: 2
---

Welcome to the MineKot Toolchain documentation. This section contains the complete technical guides for contributing to and maintaining the toolchain.

## Sections

The documentation is split into two primary areas:

- [Contributor guide]({{% relref "toolchain/contributions" %}}) – Guides on local setup, project structure, coding standards, and test suites.
- [Maintenance handbook]({{% relref "toolchain/maintenance" %}}) – Manuals for repository maintainers covering releases, builds, and custom linting logic.

## Overview

The MineKot toolchain provides the core infrastructure used by all MineKot development libraries and platform implementations. It includes:

- The standard Gradle plugin configuration.
- Custom Detekt rules to enforce strict coding patterns.
- Bundled platform-agnostic libraries for IO, coroutines, atomics, and Adventure rich-text formatting.
