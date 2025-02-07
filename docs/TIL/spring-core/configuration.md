# Configuration

- [configuration](https://docs.spring.io/spring-framework/reference/core/beans/java/configuration-annotation.html#beans-java-injecting-dependencies)

## Further Information About How Java-based Configuration Works Internally

> All @Configuration classes are subclassed at startup-time with CGLIB. In the subclass, the child method checks the
> container first for any cached (scoped) beans before it calls the parent method and creates a new instance.

```kotlin
@Configuration
class AppConfig {
    @Bean
    fun clientDao(): ClientDao {
        return ClientDaoImpl()
    }
}
```

```kotlin
class AppConfig$$EnhancerBySpringCGLIB: AppConfig() {
    // subclass
    override fun clientDao(): ClientDao {
        if (container.contains("clientDao")) {
            return container.get("clientDao") // Returns existing bean
        }
        val newBean = super.clientDao() // Calls parent method to create a new object
        container.put("clientDao", newBean) // Stores it in the container

        return newBean
    }
}
```

- Spring creates a subclass (proxy) of the @Configuration class using CGLIB.
- "Child method" refers to overriden method inside the dynamically generated CGLIB subclass. It checks if a bean already
  exists in the container. If it exists, it returns the cached bean (ensuring Singleton behavior). Otherwise, it calls
  the Parent Method to create a new instance.
- "Parent Method" refers to method in original @Configuration class. This is the actual method that instantiate the
  object. 
