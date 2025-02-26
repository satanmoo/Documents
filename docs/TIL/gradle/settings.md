# Settings

- The setting file is entry point of Gradle build.
- There is a one-to-one correspondence between a `Settings` instance and a `settings.gradle(.kts)` file.

- [primary-source](https://docs.gradle.org/current/userguide/writing_settings_files.html#settings_script_structure)

## Plugin Management

- For a detailed explanation of  `pluginManagement {}`, refer to the document below.
- [second-source](plugin-management-block.md)

### Configure Repository

- Repositories declared in `pluginManagement.repositories` are searched in order.
- First, Gradle will download and use the **plugin** from the `gradlePluginPortal` (Gradle’s official plugin
  repository),
  then from `mavenCentral`.

```kotlin
// settings.gradle.kts
pluginManagement {
    repositories {
        maven(url = file("./maven-repo"))
        gradlePluginPortal()
        ivy(url = file("./ivy-repo"))
    }
}
```

> This tells Gradle to first look in the Maven repository at ../maven-repo when resolving plugins and then to check the
> Gradle Plugin Portal if the plugins are not found in the Maven repository. If you don’t want the Gradle Plugin Portal
> to be searched, omit the gradlePluginPortal() line. Finally, the Ivy repository at ../ivy-repo will be checked.

- [primary-source](https://docs.gradle.org/current/userguide/plugins.html#sec:custom_plugin_repositories)

## Add Dependency Resolution Management

- This section configures settings related to dependency resolution across the entire project.
    - library dependency

```kotlin
// buildSrc/settings.gradle.kts

dependencyResolutionManagement {
    // 1. configure repository
    repositories {
        mavenCentral()
    }
    // 2. include version catlogs
    versionCatalogs {
        create("libs") {
            from(files("../gradle/libs.versions.toml"))
        }
    }
}
```

### 1. Configure Repository

- Specifies where the project should download the required **libraries**.

### 2. Include version catalogs

- You can write it as above to import the version catalog into buildSrc.

- [primary-source](https://docs.gradle.org/current/userguide/writing_settings_files.html#4_define_dependency_resolution_strategies)

## Apply Setting Plugins

```kotlin
// ../settings.gradle.kts

// 1. apply toolchain resolver
plugins {   
    id("org.gradle.toolchains.foojay-resolver-convention") version "0.9.0"
}
```

> The settings file can optionally apply plugins that are required for configuring the settings of the project. These
> are commonly the Develocity plugin and the Toolchain Resolver plugin in the example below.
> Plugins applied in the settings file only affect the Settings object.

- [primary-source](https://docs.gradle.org/current/userguide/writing_settings_files.html#2_apply_settings_plugins)

### 1. Apply Toolchain Resolver

- Automatically resolves the required JDK version.
- Ensure that the same JDK version in enforced across development and CI environments.

- [primary-source](https://docs.gradle.org/current/userguide/writing_settings_files.html#2_apply_settings_plugins)
