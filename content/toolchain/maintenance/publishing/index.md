---
title: "Publishing & Releases"
params:
  menuPre: "<i class=\"fa-solid fa-upload\"></i> "
weight: 1
---

This guide describes the release versioning rules, repository configuration properties, and step-by-step processes for publishing toolchain artifacts.

## Versioning scheme

The MineKot toolchain uses Semantic Versioning (SemVer): `MAJOR.MINOR.PATCH` (for example, `1.2.0`).

- **MAJOR version** – Incremented for backwards-incompatible API changes, such as removing public classes or changing method signatures.
- **MINOR version** – Incremented for new, backwards-compatible features, such as adding new library modules or adding new Gradle tasks.
- **PATCH version** – Incremented for backwards-compatible bug fixes or minor implementation updates.
- **Development snapshots** – Active development builds must use the `-SNAPSHOT` suffix (for example, `1.2.1-SNAPSHOT`). Snapshot releases are published to our snapshot repository.

## Publishing properties

Publishing is controlled through standard Gradle properties. These can be set inside `gradle.properties` or passed directly to Gradle command invocations via the `-P` flag:

- `minekotToolchain.publishing.enabled=true` - Activates the Gradle maven-publish plugin configurations.
- `minekotToolchain.publishing.minekotRepository=true` - Configures the publishing tasks to target our hosted Maven repository.
- `minekotToolchain.publishing.releasesUrl` - The release artifact target repository URL (defaults to `https://maven.minekot.org/releases`).
- `minekotToolchain.publishing.snapshotsUrl` - The snapshot artifact target repository URL (defaults to `https://maven.minekot.org/snapshots`).
- `minekotToolchain.publishing.staticRepositoryDirectory` - An optional property specifying a local filesystem directory to write published artifacts to (useful for offline static repo hosting).

### Credential configuration

Publishing to our remote Maven repository requires active credentials. The publishing plugin searches for the following environment variables:

- `MINEKOT_MAVEN_USERNAME` - The username authorized to push to the repository.
- `MINEKOT_MAVEN_PASSWORD` - The associated password or access token.

You can also pass these programmatically as Gradle properties:

```shell
./gradlew publish -PminekotToolchain.publishing.username=admin -PminekotToolchain.publishing.password=secret
```

## Publishing targets

The build script supports two target environments:

1. **Local Maven cache** – Publishes all artifacts directly to your local maven cache (typically located at `~/.m2/repository`). Use this during development to test changes locally:
   ```shell
   ./gradlew publishToMavenLocal
   ```
2. **MineKot repository** – Publishes release or snapshot artifacts to the hosted repository depending on whether the current version contains the `-SNAPSHOT` suffix:
   ```shell
   ./gradlew publish
   ```

## Release process

Follow this checklist to perform a formal release of the MineKot toolchain:

1. **Verify build stability** – Run the verification tasks locally to ensure compile safety, style compliance, and smoke test resolution:
   ```shell
   ./gradlew clean check mineKotSmokeTest
   ```
2. **Update the changelog** - Open `CHANGELOG.md` and add a new section summarizing the changes, bug fixes, and feature expansions included in this version.
3. **Remove snapshot suffix** - Open `gradle.properties` at the root of the project and set the version property to the target release version:
   ```properties
   minekotToolchain.toolchainVersion=1.2.0
   ```
4. **Publish release artifacts** – Execute the `publish` task while passing credentials. The build system detects the absence of the `-SNAPSHOT` suffix and automatically targets the `releases` repository:
   ```shell
   ./gradlew publish -PminekotToolchain.publishing.username=$USER -PminekotToolchain.publishing.password=$PASSWORD
   ```
5. **Tag the release** – Commit your version changes and tag the commit in git:
   ```shell
   git commit -am "Release v1.2.0"
   git tag -a v1.2.0 -m "Version 1.2.0 release"
   git push origin master --tags
   ```
6. **Bump to next snapshot** - Update `gradle.properties` to point to the next active development version, incorporating the snapshot suffix:
   ```properties
   minekotToolchain.toolchainVersion=1.2.1-SNAPSHOT
   ```
7. **Commit snapshot bump** – Save and push the snapshot version change back to the repository:
   ```shell
   git commit -am "Prepare for next development cycle (v1.2.1-SNAPSHOT)"
   git push origin master
   ```
