# How does the Circuit Breaker pattern work? 🟡

## Question

Explain the Circuit Breaker pattern and how it prevents cascading failures.

## Answer

The Circuit Breaker pattern monitors calls to external services and trips (opens) when failures exceed a threshold. When open, subsequent calls fail immediately without attempting the operation, preventing cascading failures. After a timeout, the circuit breaker enters a half-open state to test if the service has recovered.

States include closed (normal operation), open (calls fail immediately), and half-open (limited calls to test recovery). This pattern prevents overwhelming failing services and allows them time to recover.

## Code Example

```java
@Configuration
public class CircuitBreakerConfig {
    @Bean
    public CircuitBreaker circuitBreaker() {
        CircuitBreakerConfig config = CircuitBreakerConfig.custom()
                .failureRateThreshold(50)
                .waitDurationInOpenState(Duration.ofMillis(10000))
                .slidingWindowSize(10)
                .build();
        return CircuitBreaker.of("user-service", config);
    }
}

@Service
public class UserServiceClient {
    private final CircuitBreaker circuitBreaker;
    private final RestTemplate restTemplate;

    public User getUser(Long userId) {
        return circuitBreaker.executeSupplier(() ->
                restTemplate.getForObject("http://user-service/api/users/" + userId, User.class)
        );
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the Circuit Breaker pattern and its role in preventing cascading failures.

## ⚠️ Common Mistake
Not configuring appropriate thresholds, leading to circuit breakers that trip too frequently or not frequently enough.

## ⚡ Follow-up Questions

1. What are the different circuit breaker states?
2. How do you determine the right failure rate threshold?
3. How does the half-open state work?

## 📚 Related Topics

- [Retry Patterns](./03-retry-patterns.md)
- [Fallback Mechanisms](./06-fallback-mechanisms.md)
- [Monitoring Resilience](./08-monitoring-resilience.md)

## Navigation

[← Previous Question](./01-what-are-resilience-patterns.md) | [Next Question →](./03-retry-patterns.md)
