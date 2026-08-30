# How do you configure beans in Spring Boot? 🟡

## Question

What are the different ways to configure beans in Spring Boot and when would you use each approach?

## Answer

Spring Boot provides multiple bean configuration approaches: @Bean annotation in @Configuration classes (Java-based configuration, recommended), stereotype annotations (@Component, @Service, @Repository, @Controller) with component scanning, XML configuration (legacy, not recommended for new projects), and programmatic configuration using BeanFactory or ApplicationContext. Java-based configuration is preferred as it's type-safe, refactor-friendly, and leverages Java features.

@Bean methods in @Configuration classes allow for complex bean creation logic, dependency injection between beans, and conditional bean creation. Component scanning with stereotype annotations is simpler for most use cases. Programmatic configuration is useful for dynamic bean creation or integration with legacy systems.

## Code Example

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.Profile;
import org.springframework.beans.factory.annotation.Value;

@Configuration
public class AppConfig {

    // Simple bean configuration
    @Bean
    public UserService userService(UserRepository userRepository) {
        return new UserService(userRepository);
    }

    // Bean with custom initialization logic
    @Bean
    public DataSource dataSource(
            @Value("${spring.datasource.url}") String url,
            @Value("${spring.datasource.username}") String username,
            @Value("${spring.datasource.password}") String password) {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl(url);
        config.setUsername(username);
        config.setPassword(password);
        config.setMaximumPoolSize(10);
        return new HikariDataSource(config);
    }

    // Conditional bean based on profile
    @Bean
    @Profile("dev")
    public EmailService devEmailService() {
        return new DevEmailService();
    }

    @Bean
    @Profile("prod")
    public EmailService prodEmailService() {
        return new ProdEmailService();
    }

    // Primary bean when multiple beans of same type exist
    @Bean
    @Primary
    public PaymentService defaultPaymentService() {
        return new DefaultPaymentService();
    }

    @Bean
    public PaymentService stripePaymentService() {
        return new StripePaymentService();
    }

    // Bean with custom name
    @Bean(name = "customRestTemplate")
    public RestTemplate customRestTemplate() {
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.setMessageConverters(List.of(new MappingJackson2HttpMessageConverter()));
        return restTemplate;
    }

    // Bean with initialization and destruction methods
    @Bean(initMethod = "init", destroyMethod = "cleanup")
    public ConnectionManager connectionManager() {
        return new ConnectionManager();
    }
}

// Component scanning approach
@Service
public class OrderService {
    private final UserRepository userRepository;

    public OrderService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// Programmatic bean creation
@Component
public class DynamicBeanCreator implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
        // Dynamically register beans
        beanFactory.registerSingleton("dynamicBean", new DynamicBean());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of bean configuration approaches and your ability to choose the appropriate method for different scenarios. They're looking for knowledge of Java-based configuration best practices and when to use alternative approaches.

## ⚠️ Common Mistake

Using XML configuration in new projects instead of Java-based configuration. Another mistake is not understanding the difference between @Configuration classes and regular @Component classes, leading to incorrect bean proxying behavior.

## ⚡ Follow-up Questions

1. What is the difference between @Configuration and @Component?
2. How do you handle circular dependencies in bean configuration?
3. When would you use programmatic bean creation?

## 📚 Related Topics

- [Dependency Injection](./04-dependency-injection.md)
- [Bean Scopes](./14-bean-scopes.md)
- [Conditional Beans](./15-conditional-beans.md)

## Navigation

[← Previous Question](./12-springboot-application.md) | [Next Question →](./14-bean-scopes.md)
