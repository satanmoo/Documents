# Lookup Method Injection

- [look-up-method-injection](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-method-injection.html#beans-factory-lookup-method-injection)

## Why lookup methods do not work with "factory method" or "@Bean methods in configuration classes"

> A further key limitation is that lookup methods do not work with factory methods and in particular not with @Bean
> methods in configuration classes, since, in that case, the container is not in charge of creating the instance and
> therefore cannot create a runtime-generated subclass on the fly.

- When using Factory Methods and @Bean methods, objects are created by the developer instead of being managed directly
  by Spring.
- Spring IoC container must create the objects itself in order to dynamically generate a subclass at runtime for Lookup
  Methods.
- However, since Factory Methods and @Bean methods involve manually creating objects, Lookup Methods cannot work.
