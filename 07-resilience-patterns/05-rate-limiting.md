# How do you implement rate limiting with Resilience4j? 🟠

## Question

How do you implement rate limiting to protect services from being overwhelmed?

## Answer

Rate limiting restricts the number of operations within a time window to prevent service overload. Resilience4j provides rate limiter implementations with configurable limits and refresh periods. It can be used to limit calls to external services or to protect internal resources from being overwhelmed.

Configuration includes setting the limit for period, refresh duration, and timeout duration. Rate limiting can be applied per service, per user, or per IP address depending on the key resolver used.

## Code Example

```java
@Configuration
public class RateLimiterConfig {
    @Bean
    public RateLimiter rateLimiter() {
        return RateLimiter.custom()
                .limitForPeriod(100)
                .limitRefreshPeriod(Duration.ofSeconds(1))
                .timeoutDuration(Duration.ofMillis(500))
                .build();
    }
}

@Service
public class OrderServiceClient {
    private final RateLimiter rateLimiter;
    private final WebClient webClient;

    public Order createOrder(Order order) {
        return rateLimiter.executeSupplier(() ->
                webClient.post()
                        .uri("http://order-service/api/orders")
                        .bodyValue(order, objectMapper)
                        .retrieve()
                        .bodyToMono(Order.class)
                        .block()
        );
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of rate limiting implementation and your ability to protect services from overload.

## ⚠️ Common Mistake
Not setting appropriate rate limits, either too restrictive (poor performance) or too permissive (no protection).

## ⚡ Follow-up Questions

1. How do you implement per-user rate limiting?
2. What is the difference between rate limiting and circuit breaker?
3. How do you handle rate limit exceeded scenarios?

## 📚 Related Topics

- [Bulkhead Pattern](./04-bulkhead-pattern.md)
- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)

## Navigation

[← Previous Question](./04-bulkhead-pattern.md) | [Next Question →](./06-fallback-mechanisms.md)
