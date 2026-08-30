# What is the Bulkhead pattern and how do you implement it? 🟠

## Question

Explain the Bulkhead pattern and how it prevents resource exhaustion.

## Answer

The Bulkhead pattern isolates resources (like thread pools, connections) to prevent failures in one component from affecting others. It limits the number of concurrent operations for specific services, preventing resource exhaustion. Resilience4j provides bulkhead implementations for thread pool isolation and semaphore isolation.

Thread pool bulkhead uses a separate thread pool for operations, while semaphore bulkhead limits concurrent operations using a semaphore. This prevents a slow or failing service from consuming all available resources.

## Code Example

```java
@Configuration
public class BulkheadConfig {
    @Bean
    public Bulkhead bulkhead() {
        BulkheadConfig config = BulkheadConfig.custom()
                .maxConcurrentCalls(10)
                .maxWaitDuration(Duration.ofMillis(500))
                .build();
        return Bulkhead.of("user-service", config);
    }
}

@Service
public class UserServiceClient {
    private final Bulkhead bulkhead;
    private final RestTemplate restTemplate;

    public User getUser(Long userId) {
        return bulkhead.executeSupplier(() ->
                restTemplate.getForObject("http://user-service/api/users/" + userId, User.class)
        );
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of resource isolation patterns and your ability to prevent resource exhaustion.

## ⚠️ Common Mistake
Not setting appropriate concurrency limits, leading to either resource exhaustion or poor performance.

## ⚡ Follow-up Questions

1. What is the difference between thread pool and semaphore bulkhead?
2. How do you determine the right concurrency limit?
3. How does bulkhead differ from circuit breaker?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Rate Limiting](./05-rate-limiting.md)
- [Monitoring Resilience](./08-monitoring-resilience.md)

## Navigation

[← Previous Question](./03-retry-patterns.md) | [Next Question →](./05-rate-limiting.md)
