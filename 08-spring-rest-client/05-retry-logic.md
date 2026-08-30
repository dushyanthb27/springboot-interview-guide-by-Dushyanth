# How do you implement retry logic in REST clients? 🟠

## Question

How do you implement retry logic for transient failures in REST clients?

## Answer

Retry logic can be implemented using WebClient's retry() operator or Spring Retry with RestTemplate. Retry should be used for transient failures like network timeouts, not for permanent errors like 404. WebClient provides built-in retry support with configurable backoff strategies.

Configuration includes setting max attempts, backoff duration, and which exceptions should trigger retries. Exponential backoff prevents overwhelming the service during recovery.

## Code Example

```java
public Mono<User> getUser(Long userId) {
    return webClient.get()
            .uri("/api/users/{id}", userId)
            .retrieve()
            .bodyToMono(User.class)
            .retryWhen(Retry.backoff(3, Duration.ofMillis(1000)));
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of retry implementation and your ability to handle transient failures appropriately.

## ⚠️ Common Mistake
Retrying non-transient errors like 404, wasting resources and potentially causing issues.

## ⚡ Follow-up Questions

1. What is exponential backoff?
2. How do you determine which exceptions are retryable?
3. How does retry differ from circuit breaker?

## 📚 Related Topics

- [Error Handling](./04-error-handling.md)
- [07. Resilience Patterns](../07-resilience-patterns/README.md)
- [WebClient Configuration](./03-webclient-configuration.md)

## Navigation

[← Previous Question](./04-error-handling.md) | [Next Question →](./06-resttemplate-discovery.md)
