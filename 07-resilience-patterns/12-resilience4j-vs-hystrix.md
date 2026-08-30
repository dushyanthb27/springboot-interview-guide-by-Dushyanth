# What are the differences between Resilience4j and Hystrix? 🟡

## Question

What are the differences between Resilience4j and Netflix Hystrix, and why was Hystrix deprecated?

## Answer

Resilience4j is the recommended replacement for Netflix Hystrix, which is now in maintenance mode. Key differences include: Resilience4j is lightweight and modular (you only include what you need), while Hystrix is monolithic. Resilience4j provides more customization options and better performance. Hystrix was deprecated due to lack of maintenance and the rise of better alternatives.

Resilience4j supports circuit breakers, retries, bulkheads, rate limiters, and time limiters as separate modules. Hystrix combined all functionality into a single library. Resilience4j also provides better integration with modern reactive frameworks like Spring WebFlux.

## Code Example

```java
// Hystrix (deprecated)
@HystrixCommand(fallbackMethod = "getUserFallback")
public User getUser(Long userId) {
    return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
}

// Resilience4j (recommended)
@CircuitBreaker(name = "user-service", fallbackMethod = "getUserFallback")
public User getUser(Long userId) {
    return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the evolution of resilience libraries and your knowledge of why Hystrix was replaced.

## ⚠️ Common Mistake
Using deprecated Hystrix in new projects instead of Resilience4j.

## ⚡ Follow-up Questions

1. Why was Hystrix deprecated?
2. What are the advantages of Resilience4j over Hystrix?
3. How do you migrate from Hystrix to Resilience4j?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Spring Cloud Integration](./11-spring-cloud-integration.md)
- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)

## Navigation

[← Previous Question](./11-spring-cloud-integration.md) | [Back to Section](./README.md)
