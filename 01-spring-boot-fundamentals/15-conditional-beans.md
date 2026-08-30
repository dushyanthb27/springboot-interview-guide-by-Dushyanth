# How do you implement conditional bean loading in Spring Boot? 🟠

## Question

What are the different ways to conditionally load beans in Spring Boot and when would you use each approach?

## Answer

Spring Boot provides multiple conditional bean loading mechanisms: @ConditionalOnClass (loads bean if specified class is on classpath), @ConditionalOnMissingBean (loads bean if no matching bean exists), @ConditionalOnProperty (loads bean based on property values), @ConditionalOnResource (loads bean if resource exists), @ConditionalOnWebApplication (loads bean for web applications), @ConditionalOnNotWebApplication (loads bean for non-web applications), and custom @Conditional implementations.

These annotations are essential for creating flexible, auto-configurable applications that adapt to different environments and dependencies. They're commonly used in Spring Boot starters, auto-configuration classes, and feature toggles. Conditional bean loading helps avoid conflicts, reduces unnecessary bean creation, and enables modular application design.

## Code Example

```java
package com.example.demo.config;

import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.autoconfigure.condition.ConditionalOnResource;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.type.AnnotatedTypeMetadata;

// Conditional on class presence
@Configuration
public class DatabaseConfig {

    @Bean
    @ConditionalOnClass(name = "org.postgresql.Driver")
    public DataSource postgresDataSource() {
        return new HikariDataSource();
    }

    @Bean
    @ConditionalOnClass(name = "com.mysql.cj.jdbc.Driver")
    public DataSource mysqlDataSource() {
        return new HikariDataSource();
    }
}

// Conditional on missing bean
@Configuration
public class CacheConfig {

    @Bean
    @ConditionalOnMissingBean(CacheManager.class)
    public CacheManager simpleCacheManager() {
        return new SimpleCacheManager();
    }

    @Bean
    @ConditionalOnMissingBean(name = "redisCacheManager")
    @ConditionalOnClass(name = "org.springframework.data.redis.connection.RedisConnectionFactory")
    public CacheManager redisCacheManager() {
        return new RedisCacheManager();
    }
}

// Conditional on property
@Configuration
public class FeatureConfig {

    @Bean
    @ConditionalOnProperty(name = "feature.email.enabled", havingValue = "true")
    public EmailService emailService() {
        return new EmailService();
    }

    @Bean
    @ConditionalOnProperty(name = "feature.sms.enabled", matchIfMissing = false)
    public SmsService smsService() {
        return new SmsService();
    }
}

// Conditional on resource
@Configuration
public class ExternalConfig {

    @Bean
    @ConditionalOnResource(resources = "classpath:config/external-service.properties")
    public ExternalService externalService() {
        return new ExternalService();
    }
}

// Conditional on web application
@Configuration
public class WebConfig {

    @Bean
    @ConditionalOnWebApplication
    public WebMvcConfigurer webMvcConfigurer() {
        return new WebMvcConfigurer() {
            // Web-specific configuration
        };
    }

    @Bean
    @ConditionalOnNotWebApplication
    public CommandLineRunner commandLineRunner() {
        return args -> {
            System.out.println("Running in non-web mode");
        };
    }
}

// Custom conditional annotation
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Conditional(CustomCondition.class)
public @interface CustomConditional {
    String value() default "";
}

// Custom condition implementation
public class CustomCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        // Custom logic for condition evaluation
        String customValue = context.getEnvironment().getProperty("custom.condition.value");
        return "enabled".equals(customValue);
    }
}

// Using custom conditional
@Configuration
public class CustomConfig {

    @Bean
    @CustomConditional("enabled")
    public CustomService customService() {
        return new CustomService();
    }
}

// Complex conditional logic
@Configuration
public class ComplexConditionalConfig {

    @Bean
    @ConditionalOnClass(DataSource.class)
    @ConditionalOnMissingBean(DataSource.class)
    @ConditionalOnProperty(name = "database.auto-config", havingValue = "true", matchIfMissing = true)
    public DataSource defaultDataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of conditional bean loading and your ability to create flexible, environment-aware configurations. They're looking for knowledge of different conditional annotations and when to use custom conditions.

## ⚠️ Common Mistake

Not understanding the order of condition evaluation, leading to unexpected bean creation behavior. Another mistake is overusing conditional beans, making the application configuration complex and hard to debug.

## ⚡ Follow-up Questions

1. How do you debug conditional bean loading issues?
2. What is the difference between @ConditionalOnProperty havingValue and matchIfMissing?
3. How do you create a custom condition that evaluates multiple criteria?

## 📚 Related Topics

- [Auto-configuration](./02-auto-configuration.md)
- [Profiles](./10-profiles.md)
- [Bean Configuration](./13-bean-configuration.md)

## Navigation

[← Previous Question](./14-bean-scopes.md) | [Back to Section](./README.md)
