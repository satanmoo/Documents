# Version Catalog

```kotlin
// build.gradle.kts
plugins {
    alias(libs.plugins.kotlin.jvm)
}
```

```toml
[versions]
kotlin = "1.9.22"

[libraries]
kotlin-stdlib = { module = "org.jetbrains.kotlin:kotlin-stdlib", version.ref = "kotlin" }
kotlin-reflect = { module = "org.jetbrains.kotlin:kotlin-reflect", version.ref = "kotlin" }

[plugins]
kotlin-jvm = { id = "org.jetbrains.kotlin.jvm", version.ref = "kotlin" }
```

- In this example libs represents the `catalog`, and kotlin is a dependency available in it.

## Accessing a catalog

- In the `plugins {}` block of the build script, the `alias()` function can be used to access plugins from the catalog.

> To access items in a version catalog defined in the standard libs.versions.toml file located in the gradle directory,
> you use the libs object in your build scripts. For example, to reference a library, you can use libs.<alias>, and for
> a plugin, you can use libs.plugins.<alias>.
> 
> This section defines the plugins and their versions by mapping plugin IDs to version numbers. Just like libraries, you
> can define plugin versions using aliases from the [versions] section or directly specify the version.
> Which can be accessed in any project of the build using the plugins {} block. To refer to a plugin from the catalog,
> use the alias() function
> 
> [primary-source](https://docs.gradle.org/current/userguide/version_catalogs.html#sec:accessing-catalog)
