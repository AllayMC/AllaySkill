---
name: allaymc-plugin-dev
description: Build, update, and troubleshoot AllayMC plugins in Java or other JVM languages. Use when creating a new AllayMC plugin, migrating an existing plugin to a new Allay API version, wiring commands/events/tasks/config, or setting up Gradle and plugin metadata (plugin.json or AllayGradle plugin block).
license: LGPL-2.1
metadata:
  author: AllayMC
---

# AllayMC Plugin Development

## Overview

Create AllayMC plugins using the official Java template and the Allay API. Keep the workflow aligned with the latest Allay API and docs from the bundled references and default to the template's Java 21 toolchain unless the user requests otherwise.

## Null-safety policy

AllayMC currently does not use annotations such as JSpecify's @Nullable/@NonNull. Unless a method's Javadoc explicitly states that a parameter or return value may be null, treat it as non-null.

## Workflow

### 1) Pick the starting point

- Use the official template at `references/JavaPluginTemplate` for new plugins.
- If updating an existing plugin, diff its `build.gradle.kts` and plugin main class against the template.

### 2) Align Gradle and plugin metadata

- Update `group`, `description`, and `version` in `build.gradle.kts`.
- Keep `group` aligned with the package of the plugin main class.
- Keep the Java toolchain consistent with the template unless the user needs a different version.
- In the `allay {}` block:
  - Set `api` to the target Allay API version.
  - Set `plugin.entrance` to the fully qualified main class (or short suffix as used in the template).
  - Update `authors` and `website`.
- If the project does not use the AllayGradle plugin, create or update `plugin.json` per the docs in `references/Allay/docs/tutorials/create-your-first-plugin.md`.

### 3) Implement the plugin entry class

- Extend `org.allaymc.api.plugin.Plugin`.
- Override lifecycle methods as needed:
  - `onLoad` for lightweight setup.
  - `onEnable` for registrations and runtime wiring.
  - `onDisable` for cleanup.
- If reloadable behavior is required, override `isReloadable` and implement `reload`.
- Keep the class name and `plugin.entrance`/`plugin.json` entrance consistent.

### 4) Add core features (choose only what is needed)

- Commands: follow `references/Allay/docs/tutorials/register-commands.md`.
- Events: follow `references/Allay/docs/tutorials/register-event-listeners.md`.
- Tasks: follow `references/Allay/docs/tutorials/schedule-tasks.md`.
- Config: follow `references/Allay/docs/tutorials/use-config.md`.
- Permissions: follow `references/Allay/docs/tutorials/use-permission.md`.
- I18n: follow `references/Allay/docs/tutorials/use-i18n.md`.
- Forms/UI: follow `references/Allay/docs/tutorials/use-forms.md`.
- In-game object PDC: follow `references/Allay/docs/tutorials/persistent-data-container.md`.
- Blocks/items: follow `references/Allay/docs/tutorials/block-api.md` and `references/Allay/docs/tutorials/item-api.md`.
- Custom blocks/items: follow `references/Allay/docs/advanced/custom-block-api.md` and `references/Allay/docs/advanced/custom-item-api.md`.

### 5) Build and run

- Use `./gradlew runServer` for local testing when the AllayGradle plugin is configured.
- Use `./gradlew shadowJar` to build the shaded jar.

## Reference map (load on demand)

- Template project: `references/JavaPluginTemplate`
  - `build.gradle.kts` for Gradle + AllayGradle conventions
  - `README.md` for template initialization steps
  - `src/main/java/.../JavaPluginTemplate.java` for lifecycle structure
- Allay source and API: `references/Allay`
  - API entry points: `api/src/main/java/org/allaymc/api`
  - Docs: `docs/tutorials/*.md` and docs/advanced/*.md`
- AllayGradle: `references/AllayGradle`
  - Gradle plugin sources and configuration patterns

## Output expectations

- Keep Gradle config, plugin metadata, and main class in sync.
- Target the requested Allay API version and reflect it in Gradle metadata.
- Prefer the template conventions unless the user explicitly wants a custom structure.
