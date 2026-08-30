# How do you handle service instances going down? 🟠

## Question

How does Eureka handle service instances that go down and how do clients handle unavailable services?

## Answer

Eureka handles service instances going down through heartbeat monitoring and lease expiration. When a service stops sending heartbeats, Eureka waits for the lease expiration duration before removing the instance from the registry. Eureka also has a self-preservation mode to prevent mass removal during network partitions.

Clients handle unavailable services through retry mechanisms, circuit breakers, and fallback logic. Spring Cloud provides resilience patterns like Retry and Circuit Breaker to handle service unavailability gracefully.

## Code Example

```java
// Eureka self-preservation configuration
eureka:
  server:
    enable-self-preservation: true
    renewal-percent-threshold: 0.85

// Client-side resilience patterns
@Service
public class OrderService {
    private final UserServiceClient userServiceClient;
    private final CircuitBreaker circuitBreaker;

    public OrderService(UserServiceClient userServiceClient, CircuitBreaker circuitBreaker) {
        this.userServiceClient = userServiceClient;
        this.circuitBreaker = circuitBreaker;
    }

    public User getUser(Long userId) {
        return circuitBreaker.executeSupplier(() ->
                userServiceClient.getUser(userId)
        );
    }
}

// Retry configuration
@Configuration
public class RetryConfig {
    @Bean
    public Retry retry() {
        return Retry.custom()
                .maxAttempts(3)
                .waitDuration(Duration.ofMillis(1000))
                .retryOnException(ServiceUnavailableException.class)
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of fault tolerance in service discovery and your ability to implement resilience patterns.

## ⚠️ Common Mistake
Not implementing proper fallback mechanisms, leading to cascading failures when services become unavailable.

## ⚡ Follow-up Questions

1. What is self-preservation mode in Eureka?
2. How do circuit breakers protect against cascading failures?
3. What is the difference between retry and circuit breaker patterns?

## 📚 Related Topics

- [Health Checks](./08-health-checks.md)
- [07. Resilience Patterns](../07-resilience-patterns/README.md)
- [High Availability](./07-high-availability.md)

## Navigation

[← Previous Question](./08-health-checks.md) | [Next Question →](./10-security.md)
