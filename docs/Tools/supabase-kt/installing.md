# Installing

- [installing](https://supabase.com/docs/reference/kotlin/installing)

## DATABASE

- [database](https://github.com/supabase-community/supabase-kt/tree/master/Postgrest)

1. Add dependency to build file using BOM
2. Add Ktor Client Engine to each of your Kotlin targets (required)
3. Add Serialization tool

```build.gradle.kt
plugins {
    kotlin("jvm") version "1.9.25"
    kotlin("plugin.serialization") version "1.9.25" // Kotlin version
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

repositories {
    mavenCentral()
}

val supabaseKtBomVersion = "3.1.1"
val ktorVersion = "3.0.3"

dependencies {
    implementation(platform("io.github.jan-tennert.supabase:bom:$supabaseKtBomVersion"))
    implementation("io.github.jan-tennert.supabase:postgrest-kt")
    implementation("io.ktor:ktor-client-java:$ktorVersion")
}

kotlin {
    compilerOptions {
        freeCompilerArgs.addAll("-Xjsr305=strict")
    }
}
```
