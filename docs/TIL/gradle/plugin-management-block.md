# Plugin Management

- This document covers the pluginManagement {} block in the settings.gradle.kts file.
- The `pluginManagement {}` block is only used in settings.gradle.

> The pluginManagement{} block is used to configure repositories for plugin resolution and to define version constraints
> for plugins that are applied in the build scripts.

> The pluginManagement{} block can be used in a settings.gradle(.kts) file, where it must be the first block in the
> file.

- [primary-source-1](https://docs.gradle.org/current/userguide/plugins.html#sec:plugin_management)

## `pluginManagement.plugins {}` in `settings.gradle`

1. Allows managing plugin versions in a single location.
    - This does not explicitly declare the usage of a plugin.
    - It only configures the plugin version.

> A plugins{} block inside pluginManagement{} allows all plugin versions for the build to be defined in a single
> location. Plugins can then be applied by id to any build script via the plugins{} block.

2. Can dynamically retrieve version values using `gradle.properties`.

```kotlin
// settings.gradle.kts
pluginManagement {
    val helloPluginVersion: String by settings
    plugins {
        id("com.example.hello") version "${helloPluginVersion}"
    }
}
```

```kotlin
// build.gradle.kts
plugins {
    id("com.example.hello")
}
```

```text
// gradle.properties
helloPluginVersion=1.0.0
```

- [primary source](https://docs.gradle.org/current/userguide/plugins.html#sec:plugin_version_management)

### The `Settings Object` Can Dynamically Read Values from `gradle.properties` and Contain Additional Read-Only Properties

> In addition to the properties of this interface, the Settings object makes some additional read-only properties
> available to the settings script. This includes properties from the following sources:
> - Defined in the gradle.properties file located in the settings directory of the build.
> - Defined the gradle.properties file located in the user's .gradle directory.
> - Provided on the command-line using the -P option.

- [primary-source-1](https://docs.gradle.org/current/dsl/org.gradle.api.initialization.Settings.html#N19905)
- [primary-source-2](https://docs.gradle.org/current/userguide/build_environment.html#the_gradle_properties_file)

## `plugin {}` in `setting.gradle`

- Actually use the plugins.
- Resolving plugin
    - The process of finding the JAR file and adding it to the script’s classpath.
- applying plugin
    - Using the plugin version specified in `pluginManagement.plugins {}` by referencing it with an `id`.

> To use the build logic encapsulated in a plugin, Gradle needs to perform two steps. First, it needs to resolve the
> plugin, and then it needs to apply the plugin to the target, usually a Project.
>
> Resolving a plugin means finding the correct version of the JAR that contains a given plugin and adding it to the
> script classpath. Once a plugin is resolved, its API can be used in a build script. Script plugins are self-resolving
> in that they are resolved from the specific file path or URL provided when applying them. Core binary plugins provided
> as part of the Gradle distribution are automatically resolved.
>
> Applying a plugin means executing the plugin’s Plugin.apply(T) on a project.

- [primary-source](https://docs.gradle.org/current/userguide/plugins.html#sec:using_plugins)

## Avoid Using `pluginManagement.plugins {}` in `settings.gradle`

- [The official Kotlin documentation](https://kotlinlang.org/docs/gradle-best-practices.html#use-a-version-catalog)
  recommends using the Version Catalog feature as a best practice.
- [reference](https://stackoverflow.com/questions/77073596/whats-the-difference-between-plugins-and-pluginmanagement-plugins-in)
