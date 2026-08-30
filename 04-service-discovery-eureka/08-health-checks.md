# How do you implement health checks in Eureka? 🟡

## Question

How does Eureka monitor service health and how do you configure health checks?

## Answer

Eureka monitors service health through periodic heartbeats sent by Eureka clients. By default, Eureka uses the Spring Boot Actuator health endpoint for health checks. If a service fails to send heartbeats for a configured period (default 90 seconds), Eureka removes it from the registry. Health checks can be customized using Spring Boot Actuator health indicators.

Configuration includes setting the heartbeat interval, lease expiration duration, and health check URL path. Services can also implement custom health indicators to provide domain-specific health information.

## Code Example

```java
// Enable health checks
// pom.xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

// application.yml
eureka:
  client:
    healthcheck:
      enabled: true
  instance:
    health-check-url-path: /actuator/health
    lease-renewal-interval-in-seconds: 30
    lease-expiration-duration-in-seconds: 90

// Custom health indicator
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    @Autowired
    private DataSource dataSource;

    @Override
    public Health health() {
        try {
            try (Connection connection = dataSource.getConnection()) {
                if (connection.isValid(1)) {
                    return Health.up()
                            .withDetail("database", "PostgreSQL")
                            .build();
                }
            }
        } catch (SQLException e) {
            return Health.down(e).build();
        }
        return Health.down().build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of how Eureka maintains service availability information and your ability to configure health monitoring.

## ⚠️ Common Mistake
Not enabling health checks properly, leading to services being removed from the registry even when they're healthy.

## ⚡ Follow-up Questions

1. How do you customize the heartbeat interval?
2. What is the self-preservation mode in Eureka?
3. How do you implement custom health indicators?

## 📚 Related Topics

- [Service Registration](./04-service-registration.md)
- [Service Down Handling](./09-service-down-handling.md)
- [High Availability](./07-high-availability.md)

## Navigation

[← Previous Question](./07-high-availability.md) | [Next Question →](./09-service-down-handling.md)
