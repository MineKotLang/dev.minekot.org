---
title: "Contributor Guide"
params:
  menuPre: "<i class=\"fa-solid fa-users-cog\"></i> "
weight: 1
---

Thank you for your interest in contributing to the MineKot toolchain. This guide contains everything you need to know to get started writing code, tests, and documentation.

## Guide sections

To keep the documentation organized, we split the contributor handbook into several specialized pages:

- [Local setup]({{% relref "toolchain/contributions/setup" %}}) – Instructions for installing prerequisites, cloning the repository, and building the project locally.
- [Module architecture]({{% relref "toolchain/contributions/architecture" %}}) – Deep dive into how subprojects are divided between Gradle plugins, lint rules, and helper libraries.
- [Code style and standards]({{% relref "toolchain/contributions/style-guide" %}}) – Mandatory practices including our Kotlinx mandate, MiniMessage usage, and functional error handling.
- [Testing and validation]({{% relref "toolchain/contributions/testing" %}}) – How to write unit tests, verify changes using the smoke test suite, and run assertions.

## Contribution workflow

We follow a standard fork-and-pull model for contributions. Here is the general lifecycle of a contribution:

1. File or find an issue tracking the bug or feature you want to address.
2. Fork the repository and create a new branch from `master`.
3. Set up your local workspace and verify the build passes before making changes.
4. Implement your changes, following the codebase architecture and style guidelines.
5. Add unit tests for your changes and run the smoke test suite.
6. Verify detekt passes by running the local checks.
7. Submit a pull request detailing the changes and linking to the related issue.
