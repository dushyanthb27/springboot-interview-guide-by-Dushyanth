# How do you implement configuration refresh? 🟠

## Question

How do you implement dynamic configuration refresh in Spring Cloud Config?

## Answer

Configuration refresh allows applications to reload configuration without restarting. Spring Cloud Config provides the @RefreshScope annotation for beans that need to be refreshed, and the /actuator/refresh endpoint to trigger refresh. The refresh can be triggered manually via HTTP call or automatically using Spring Cloud Bus with message brokers.

Implementation includes adding the spring-boot-starter-actuator dependency, enabling the refresh endpoint, annotating beans with @RefreshScope, and optionally integrating with Spring Cloud Bus for automatic refresh across all instances.

## Code Example

```java
// Enable refresh endpoint
management:
  endpoints:
    web:
      exposure:
        include: refresh,health,info

// Bean with refresh scope
@Component
@RefreshScope
public class EmailService {
    @Value("${email.sender}")
    private String sender;

    @Value("${email.enabled}")
    private boolean enabled;

    // Getters and setters
}

// Trigger refresh via HTTP
// POST http://localhost:8080/actuator/refresh
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of dynamic configuration updates and the refresh mechanism.

## ⚠️ Common Mistake
Not using @RefreshScope on beans that use @Value annotations, preventing configuration updates from taking effect.

## ⚡ Follow-up Questions

1. How does Spring Cloud Bus enable automatic refresh?
2. What is the difference between @RefreshScope and @ConfigurationProperties?
3. How do you handle configuration refresh in a clustered environment?

## 📚 Related Topics

- [Spring Cloud Bus](./11-spring-cloud-bus.md)
- [Environment Config](./06-environment-config.md)
- [Config Client Setup](./03-config-client-setup.md)

## Navigation

[← Previous Question](./04-configuration-sources.md) | [Next Question →](./06-environment-config.md)
