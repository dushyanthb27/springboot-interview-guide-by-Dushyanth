# How does Spring Boot auto-configuration work? 🟡

## Question

Explain how Spring Boot's auto-configuration mechanism works and how you can customize or disable it.

## Answer

Spring Boot auto-configuration uses conditional annotations to automatically configure beans based on classpath dependencies, existing beans, and property settings. It examines the classpath for specific classes and configurations, then creates appropriate beans with sensible defaults. The @EnableAutoConfiguration annotation triggers this process, which relies on spring.factories files (in Spring Boot 2.x) or spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports files (in Spring Boot 3.x) to load configuration classes.

You can customize auto-configuration using @ConditionalOnClass, @ConditionalOnMissingBean, @ConditionalOnProperty, and similar annotations. To disable specific auto-configurations, use @EnableAutoConfiguration(exclude = {ConfigurationClass.class}) or set spring.autoconfigure.exclude in application.properties.

## Code Example

```java
package com.example.demo.config;

import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class CustomAutoConfiguration {

    @Bean
    @ConditionalOnClass(name = "com.example.ExternalService")
    @ConditionalOnMissingBean
    @ConditionalOnProperty(name = "custom.service.enabled", havingValue = "true")
    public ExternalService externalService() {
        return new ExternalService();
    }
}

// Custom auto-configuration class
@Configuration
@ConditionalOnClass(DataSource.class)
@EnableConfigurationProperties(DataSourceProperties.class)
public class DatabaseAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public DataSource dataSource(DataSourceProperties properties) {
        return properties.initializeDataSourceBuilder().build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Boot's core mechanism and your ability to work with auto-configuration. They're looking for knowledge of conditional annotations and how to customize default behavior.

## ⚠️ Common Mistake

Not understanding that auto-configuration is based on classpath presence and conditions, leading to confusion when beans aren't created as expected. Another mistake is not knowing how to exclude unwanted auto-configurations properly.

## ⚡ Follow-up Questions

1. How do you debug auto-configuration issues?
2. What is the difference between @ConditionalOnMissingBean and @ConditionalOnBean?
3. How do you create your own Spring Boot starter with auto-configuration?

## 📚 Related Topics

- [Spring Boot Starters](./05-spring-boot-starters.md)
- [Conditional Beans](./15-conditional-beans.md)
- [Bean Configuration](./13-bean-configuration.md)

## Navigation

[← Previous Question](./01-what-is-spring-boot.md) | [Next Question →](./03-stereotype-annotations.md)
