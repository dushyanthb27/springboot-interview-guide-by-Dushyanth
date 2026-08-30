# How do you handle configuration failures? 🟠

## Question

How do you handle situations where the Config Server is unavailable or configuration fetch fails?

## Answer

Configuration failure handling includes implementing fail-fast behavior, using fallback configurations, implementing retry logic, and providing default configurations. Spring Cloud Config can be configured to fail fast (prevent application startup if config server is unavailable) or continue with local configuration.

Resilience patterns include retry mechanisms with exponential backoff, circuit breakers to prevent cascading failures, and fallback to local configuration files. Applications can also implement health checks to monitor configuration status.

## Code Example

```java
// Fail-fast configuration
spring:
  cloud:
    config:
      fail-fast: true
      retry:
        initial-interval: 1000
        max-attempts: 3
        max-interval: 2000
        multiplier: 1.5

// Fallback configuration
@Configuration
public class FallbackConfig {
    @Bean
    @ConditionalOnMissingBean
    public DataSource defaultDataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of resilience patterns for configuration management and your ability to handle failures gracefully.

## ⚠️ Common Mistake
Not implementing proper fallback mechanisms, leading to application startup failures when the Config Server is unavailable.

## ⚡ Follow-up Questions

1. What is the difference between fail-fast and fallback behavior?
2. How do you implement circuit breaking for configuration fetch?
3. How do you monitor configuration health?

## 📚 Related Topics

- [Config Client Setup](./03-config-client-setup.md)
- [Configuration Refresh](./05-configuration-refresh.md)
- [07. Resilience Patterns](../07-resilience-patterns/README.md)

## Navigation

[← Previous Question](./08-configuration-versioning.md) | [Next Question →](./10-best-practices.md)
