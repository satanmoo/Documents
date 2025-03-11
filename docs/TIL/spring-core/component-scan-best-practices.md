# Component Scan Best Practices: Leveraging scanBasePackageClasses

- 컴포넌트 스캔 범위를 지정할 때, 패키지 이름 타이핑 오류로 시간 낭비를 한 계기로 쓰는 글 ㄱ-

## 1. Use scanBasePackageClasses Instead of scanBasePackages

- When configuring component scanning in Spring Boot, it’s better to use scanBasePackageClasses rather than manually typing package names with scanBasePackages. Here’s why:
    - Type Safety & Maintenance:
        - By passing classes to scanBasePackageClasses, Spring Boot extracts the package name from each class automatically. This approach prevents typos and reduces the need for manual updates if package names change.
    - Eliminates Hardcoding:
        - Instead of hardcoding package names as strings, you provide a reference class, which makes the configuration resilient to refactoring.

## 2. Use a PackageMarker

- A common technique is to create a no-op marker interface or class that serves solely as a reference for the base package. For example:

```kotlin
package com.my.root

interface PackageMarker {
}
```

```kotlin
package com.my.root

import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication(scanBasePackageClasses = [PackageMarker::class])
class MyApplication

fun main(args: Array<String>) {
    runApplication<MyApplication>(*args)
}
```

- This ensures that Spring Boot scans the com.my.root package and all its subpackages. Using a PackageMarker makes your configuration type-safe and easier to maintain.

- [source](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/autoconfigure/SpringBootApplication.html#scanBasePackageClasses())

> Type-safe alternative to scanBasePackages for specifying the packages to scan for annotated components. The package of each class specified will be scanned.
> Consider creating a special no-op marker class or interface in each package that serves no purpose other than being referenced by this attribute.
> Note: this setting is an alias for @ComponentScan only. It has no effect on @Entity scanning or Spring Data Repository scanning. For those you should add @EntityScan and @Enable...Repositories annotations.

## 3. How scanBasePackageClasses Works

- The scanBasePackageClasses attribute operates as follows:
    - It takes one or more classes as arguments.
    - For each provided class, Spring calls `ClassUtils.getPackageName(clazz)` to extract the package name.
    - It then configures the component scanner to scan that package and all of its subpackages.

```java
// package org.springframework.context.annotation
// Class ComponentScanAnnotationParser

public Set<BeanDefinitionHolder> parse(AnnotationAttributes componentScan, String declaringClass) {
		ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(this.registry,
				componentScan.getBoolean("useDefaultFilters"), this.environment, this.resourceLoader);

		Class<? extends BeanNameGenerator> generatorClass = componentScan.getClass("nameGenerator");
		boolean useInheritedGenerator = (BeanNameGenerator.class == generatorClass);
		scanner.setBeanNameGenerator(useInheritedGenerator ? this.beanNameGenerator :
				BeanUtils.instantiateClass(generatorClass));

		ScopedProxyMode scopedProxyMode = componentScan.getEnum("scopedProxy");
		if (scopedProxyMode != ScopedProxyMode.DEFAULT) {
			scanner.setScopedProxyMode(scopedProxyMode);
		}
		else {
			Class<? extends ScopeMetadataResolver> resolverClass = componentScan.getClass("scopeResolver");
			scanner.setScopeMetadataResolver(BeanUtils.instantiateClass(resolverClass));
		}

		scanner.setResourcePattern(componentScan.getString("resourcePattern"));

		for (AnnotationAttributes includeFilterAttributes : componentScan.getAnnotationArray("includeFilters")) {
			List<TypeFilter> typeFilters = TypeFilterUtils.createTypeFiltersFor(includeFilterAttributes, this.environment,
					this.resourceLoader, this.registry);
			for (TypeFilter typeFilter : typeFilters) {
				scanner.addIncludeFilter(typeFilter);
			}
		}
		for (AnnotationAttributes excludeFilterAttributes : componentScan.getAnnotationArray("excludeFilters")) {
			List<TypeFilter> typeFilters = TypeFilterUtils.createTypeFiltersFor(excludeFilterAttributes, this.environment,
				this.resourceLoader, this.registry);
			for (TypeFilter typeFilter : typeFilters) {
				scanner.addExcludeFilter(typeFilter);
			}
		}

		boolean lazyInit = componentScan.getBoolean("lazyInit");
		if (lazyInit) {
			scanner.getBeanDefinitionDefaults().setLazyInit(true);
		}

		Set<String> basePackages = new LinkedHashSet<>();
		String[] basePackagesArray = componentScan.getStringArray("basePackages");
		for (String pkg : basePackagesArray) {
			String[] tokenized = StringUtils.tokenizeToStringArray(this.environment.resolvePlaceholders(pkg),
					ConfigurableApplicationContext.CONFIG_LOCATION_DELIMITERS);
			Collections.addAll(basePackages, tokenized);
		}
		for (Class<?> clazz : componentScan.getClassArray("basePackageClasses")) {
			basePackages.add(ClassUtils.getPackageName(clazz));
		}
		if (basePackages.isEmpty()) {
			basePackages.add(ClassUtils.getPackageName(declaringClass));
		}

		scanner.addExcludeFilter(new AbstractTypeHierarchyTraversingFilter(false, false) {
			@Override
			protected boolean matchClassName(String className) {
				return declaringClass.equals(className);
			}
		});
		return scanner.doScan(StringUtils.toStringArray(basePackages));
	}
```

- This approach eliminates the need to manually type package names and ensures that the scanner always uses the correct, up-to-date package information.

## 4. Differences Between Packages and Gradle Projects (Modules)

- Understanding the distinction between packages and projects (or modules) is crucial:

### Gradle Projects (Modules)

- Definition:
    - In a Gradle multi-project build, each project (often called a module) is an independent build unit with its own build script, dependencies, compilation, testing, and packaging.
- Role:
    - Build & Dependency Management:
        - Each module is compiled and packaged separately. During runtime, the compiled classes from all modules are merged onto a single classpath.
    - Physical Separation:
        - Modules are physically organized in separate directories and can produce separate artifacts (JARs, etc.).
- [source](https://docs.gradle.org/current/userguide/gradle_basics.html#projects)

### Packages

- Definition:
    - A packaging is a logical grouping of classes within Java/Kotlin defined by the `package` statement in source files.
- Role:
    - Namespace Management:
        - Packages help organize classes, avoid naming conflicts, and are used by Spring to perform component scanning.
    - Mapping Folder Structure:
        - The directory structure under source root(e.g. `src/main/kotlin`) maps directly to the package names. For example, files under `src/main/kotlin/com/my/root` are part of `com.my.root` package.

### How They Interact

- Even though modules(projects) are built independently in a multi-project Gradle build, if all source files declare the same package (e.g., `com.my.root`), then at runtime, these classes are merged into a single namespace. This allows Spring’s component scan to find components across different projects(modules) as long as they’re on the classpath.
