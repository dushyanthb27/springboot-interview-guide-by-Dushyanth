# How do you implement fallback mechanisms? 🟡

## Question

How do you implement fallback mechanisms to provide alternative functionality when primary operations fail?

## Answer

Fallback mechanisms provide alternative functionality when primary operations fail. They can return cached data, default values, or call alternative services. Resilience4j integrates with circuit breakers to execute fallback logic when the circuit is open or when operations fail.

Fallback approaches include returning cached data, calling backup services, returning default responses, or degrading functionality gracefully. Fallbacks should be simple and reliable to avoid cascading failures.

## Code Example

```java
@Service
public class UserServiceClient {
    private final CircuitBreaker circuitBreaker;
    private final UserCacheService userCacheService;
    private final RestTemplate restTemplate;

    public User getUser(Long userId) {
        return circuitBreaker.executeSupplier(() ->
                restTemplate.getForObject("http://user-service/api/users/" + userId, User.class)
        ).recover(throwable -> {
            log.warn("User service unavailable, using cache", throwable);
            return userCacheService.getCachedUser(userId)
                    .orElseThrow(() -> new ServiceUnavailableException("User service unavailable and no cached data"));
        });
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of fallback strategies and your ability to implement graceful degradation.

## ⚠️ Common Mistake
Implementing complex fallback logic that can itself fail, creating additional failure points.

## ⚡ Follow-up Questions

1. What are different fallback strategies?
2. How do you ensure fallbacks are reliable?
3. What is the difference between fallback and retry?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Retry Patterns](./03-retry-patterns.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./05-rate-limiting.md) | [Next Question →](./07-timeout-patterns.md)
