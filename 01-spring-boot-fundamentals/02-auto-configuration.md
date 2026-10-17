# How does Spring Boot auto-configuration work? 🟡

## Question

You are debugging a production Spring Boot service where a datasource or security bean is not being created as expected. Explain how Spring Boot decides what to auto-configure, how you would diagnose the issue, and how you would safely override or disable the default behavior.

## Answer

Spring Boot auto-configuration uses conditional annotations to automatically configure beans based on classpath dependencies, existing beans, and property settings. It examines the classpath for specific classes and configurations, then creates appropriate beans with sensible defaults.

**How auto-configuration works:**

1. **Trigger**: The `@EnableAutoConfiguration` annotation (part of `@SpringBootApplication`) enables auto-configuration
2. **Discovery**: Spring Boot loads auto-configuration classes from:
   - Spring Boot 2.x: `META-INF/spring.factories` files
   - Spring Boot 3.x: `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` files
3. **Condition evaluation**: Each auto-configuration class uses conditional annotations to determine if it should apply:
   - `@ConditionalOnClass`: Only activates if specified class is on classpath
   - `@ConditionalOnMissingBean`: Only creates bean if one doesn't already exist
   - `@ConditionalOnProperty`: Only activates if property has specific value
   - `@ConditionalOnWebApplication`: Only activates in web applications
   - `@ConditionalOnExpression`: Activates based on SpEL expression
4. **Bean creation**: When conditions are met, the configuration class creates beans with sensible defaults

**Customizing auto-configuration:**

1. **Exclude specific auto-configurations**:
   ```java
   @SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
   ```
   Or in `application.properties`:
   ```properties
   spring.autoconfigure.exclude=com.example.UnwantedAutoConfiguration
   ```

2. **Override beans manually**: Define your own bean to override the auto-configured one (auto-configuration uses `@ConditionalOnMissingBean`)

3. **Use properties**: Many auto-configurations use properties to customize behavior:
   ```properties
   server.port=8081
   spring.datasource.url=jdbc:mysql://localhost/mydb
   ```

4. **Debug auto-configuration**: Run with `--debug` flag to see which auto-configurations were applied and why

## Code Example

```java
package com.example.demo.config;

import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

// Custom auto-configuration class
@Configuration
@ConditionalOnClass(name = "com.example.ExternalService") // Only if class exists on classpath
@ConditionalOnProperty(name = "external.service.enabled", havingValue = "true", matchIfMissing = true)
public class CustomAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean // Only create if user hasn't defined their own
    public ExternalService externalService() {
        return new ExternalService();
    }
}

// Example of how Spring Boot's DataSource auto-configuration works internally
@Configuration
@ConditionalOnClass(DataSource.class) // Only if JDBC driver is present
@EnableConfigurationProperties(DataSourceProperties.class)
public class DatabaseAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean // User can override by defining their own DataSource
    public DataSource dataSource(DataSourceProperties properties) {
        return properties.initializeDataSourceBuilder().build();
    }
}

// Application properties to control auto-configuration
// application.properties
external.service.enabled=true
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Boot's core mechanism and your ability to work with auto-configuration. They're looking for knowledge of conditional annotations and how to customize default behavior.

## ⚠️ Common Mistake

Not understanding that auto-configuration is based on classpath presence and conditions, leading to confusion when beans aren't created as expected. Another mistake is not knowing how to exclude unwanted auto-configurations properly.

## ⚡ Follow-up Questions

1. How do you debug auto-configuration issues when a bean isn't being created as expected?
2. What is the difference between @ConditionalOnMissingBean and @ConditionalOnBean, and when would you use each?
3. How do you create your own Spring Boot starter with auto-configuration?
4. What is the order of auto-configuration execution, and how does Spring Boot handle dependencies between auto-configurations?
5. How does Spring Boot 3.x differ from 2.x in terms of auto-configuration discovery?

## 📚 Related Topics

- [Spring Boot Starters](./05-spring-boot-starters.md)
- [Conditional Beans](./15-conditional-beans.md)
- [Bean Configuration](./13-bean-configuration.md)

## Navigation

[← Previous Question](./01-what-is-spring-boot.md) | [Next Question →](./03-stereotype-annotations.md)
