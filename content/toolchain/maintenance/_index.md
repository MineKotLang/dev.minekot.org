---
title: "Maintenance Handbook"
params:
  menuPre: "<i class=\"fa-solid fa-screwdriver-wrench\"></i> "
weight: 2
---

Welcome to the maintenance handbook. This section contains reference guides and operational instructions for repository maintainers of the MineKot toolchain.

## Handbook chapters

The maintenance guide is divided into three primary reference pages:

- [Publishing and releases]({{% relref "toolchain/maintenance/publishing" %}}) – Explains how to update project versions, run validation checks, publish artifacts to Maven registries, and manage changelogs.
- [Continuous integration and deployment]({{% relref "toolchain/maintenance/ci-cd" %}}) – Details our build pipelines, smoke test automation, and Develocity build scan configurations.
- [Custom Detekt rules]({{% relref "toolchain/maintenance/lint-rules" %}}) – Instructions for maintaining and extending our custom static analysis lint rules.

## Core maintainer responsibilities

As a maintainer, you are responsible for keeping the toolchain healthy and secure:

- **Dependency updates** – Regularly monitor upstream dependencies (such as Kotlin, Gradle, kotlinx-serialization, kotlinx-io, and Kyori Adventure). Update versions in the Gradle version catalogs and verify that all tests pass.
- **Pull request reviews** – Ensure that incoming pull requests conform to the strict architectural boundaries, follow the functional error handling model, compile without warning flags, and provide complete unit test coverage.
- **Lint rules management** – As new Kotlin patterns emerge or downstream Minecraft platforms evolve, adjust lint rule tolerances and configuration settings.
