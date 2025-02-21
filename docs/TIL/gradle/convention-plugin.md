# Convention plugins

- A document on how to create convention plugins.

## What They Are and Why Precompiled Plugins Are a Good Choice

- A convention plugin is a plugin that defines conventions for a project.
    - It is useful for setting Java versions, Kotlin configurations, or establishing project-wide standards (convention
      applied across the project).
- Convention plugins are mostly implemented using precompiled plugins.
    - This [table](https://docs.gradle.org/current/userguide/custom_plugins.html#custom_plugins_2) provides a good
      comparison of the benefits of different plugin types.
- [The official Kotlin documentation]((https://kotlinlang.org/docs/gradle-best-practices.html#use-kotlin-dsl))
  recommends using Kotlin DSL, and precompiled plugins can be implemented using Kotlin
  DSL.

> A convention plugin is a plugin that normally configures existing core and community plugins with your own
> conventions (i.e. default values) such as setting the Java version by using java.toolchain.languageVersion =
> JavaLanguageVersion.of(17). Convention plugins are also used to enforce project standards and help streamline the
> build process. They can apply and configure plugins, create new tasks and extensions, set dependencies, and much more.
>
> [source](https://docs.gradle.org/current/userguide/implementing_gradle_plugins_precompiled.html)

> A convention plugin is typically a precompiled script plugin that configures existing core and community plugins with
> your own conventions (i.e. default values) such as setting the Java version by using java.toolchain.languageVersion =
> JavaLanguageVersion.of(17). Convention plugins are also used to enforce project standards and help streamline the
> build process. They can apply and configure plugins, create new tasks and extensions, set dependencies, and much more.
>
> [source](https://docs.gradle.org/current/userguide/custom_plugins.html#sec:convention_plugins)

## Using `buildSrc` to Share Common Build Logic

- The Gradle documentation recommends placing convention plugins inside `buildSrc`.

- Follow these steps:
    1. Create a convention plugin inside the `buildSrc` directory using Kotlin DSL.
    2. Write a build script inside the `buildSrc` folder to compile the convention plugin (ensuring it uses Kotlin DSL).
    3. Apply the convention plugin in the build scripts of other modules using the `plugins {}` block.

> We can write a plugin that encapsulates the build logic common to several subprojects in a project.
> This kind of plugin is called a convention plugin.
> While writing plugins is outside the scope of this section, the recommended way to build a Gradle project is to put
> common build logic in a convention plugin located in the buildSrc.
>
> [source](https://docs.gradle.org/current/userguide/sharing_build_logic_between_subprojects.html#sec:sharing_logic_via_convention_plugins)

> To develop a convention plugin, we recommend using buildSrc – which represents a completely separate Gradle build.
> buildSrc has its own settings file to define where dependencies of this build are located.
>
> [source](https://docs.gradle.org/current/userguide/custom_plugins.html#sec:convention_plugins)

## Using Convention Plugins with a Version Catalog

```toml
# gradle/libs.versions.toml
[versions]
kotlin = "1.9.0"

[plugins]
kotlin-jvm = { id = "org.jetbrains.kotlin.jvm", version.ref = "kotlin" }
application = { id = "application" }

[libraries]
kotlin-stdlib = { module = "org.jetbrains.kotlin:kotlin-stdlib", version.ref = "kotlin" }
```

```kotlin
// buildSrc/src/main/kotlin/my-kotlin-application.gradle.kts
// (Precompiled Script Plugin)
plugins {
    alias(libs.plugins.kotlin.jvm) // apply Kotlin JVM plugin
    alias(libs.plugins.application) // apply application plugin
}

repositories {
    mavenCentral()
}

dependencies {
    implementation(libs.kotlin.stdlib) // Kotlin Standard Library
}
```

- For details on using version catalogs, refer to this [document](version-catalog.md).

## Additional References

- [Gradle Kotlin 컨벤션 플러그인으로 효율적으로 멀티 모듈 관리하기](https://www.slideshare.net/slideshow/gradle-kotlin/261178195)
- [Using Version Catalogs with Precompiled Script Plugins](https://discuss.gradle.org/t/using-version-catalogs-with-precompiled-script-plugins/45417)
