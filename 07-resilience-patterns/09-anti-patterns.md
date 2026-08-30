# What are the anti-patterns in resilience implementation? 🟠

## Question

What are common anti-patterns when implementing resilience patterns and how do you avoid them?

## Answer

Common anti-patterns include retrying non-transient errors (like 404 or validation errors), setting inappropriate circuit breaker thresholds, implementing complex fallback logic that can fail, not monitoring resilience patterns, and over-engineering resilience for simple use cases. These anti-patterns can make systems less reliable rather than more reliable.

Avoidance strategies include understanding which errors are transient, setting appropriate thresholds based on observed failure rates, keeping fallback logic simple, implementing comprehensive monitoring, and applying resilience patterns only where they add value.

## Code Example

```java
// Anti-pattern: Retrying non-transient errors
@Retry(exception = {Exception.class}) // Bad: retries everything
public User getUser(Long userId) {
    return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
}

// Correct: Retry only transient errors
@Retry(exception = {IOException.class, TimeoutException.class})
public User getUser(Long userId) {
    return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
}

// Anti-pattern: Complex fallback logic
public User getUserWithFallback(Long userId) {
    try {
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    } catch (Exception e) {
        // Complex logic that can fail
        return complexFallbackLogic(userId);
    }
}

// Correct: Simple fallback
public User getUserWithFallback(Long userId) {
    try {
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    } catch (Exception e) {
        return userCacheService.getCachedUser(userId).orElse(null);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of common mistakes in resilience implementation and your ability to avoid them.

## ⚠️ Common Mistake
Applying resilience patterns universally without considering whether they add value for the specific use case.

## ⚡ Follow-up Questions

1. How do you determine if a resilience pattern is needed?
2. What are the signs of over-engineered resilience?
3. How do you balance resilience with performance?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Retry Patterns](./03-retry-patterns.md)
- [Best Practices](../06-configuration-management/10-best-practices.md)

## Navigation

[← Previous Question](./08-monitoring-resilience.md) | [Next Question →](./10-testing-resilience.md)
