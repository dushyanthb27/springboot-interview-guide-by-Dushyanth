# How do you implement retry patterns with Resilience4j? 🟡

## Question

How do you implement retry patterns to handle transient failures?

## Answer

Retry patterns automatically retry failed operations with configurable backoff strategies. Resilience4j provides retry mechanisms with exponential backoff, max attempts, and retryable exception configuration. Retries should be used for transient failures like network timeouts, not for permanent errors like 404 responses.

Configuration includes setting max attempts, wait duration between retries, backoff multiplier, and specifying which exceptions should trigger retries. Exponential backoff prevents overwhelming the service during recovery.

## Code Example

```java
@Configuration
public class RetryConfig {
    @Bean
    public Retry retry() {
        return Retry.custom()
                .maxAttempts(3)
                .waitDuration(Duration.ofMillis(1000))
                .retryOnException(WebClientResponseException.class)
                .retryOnException(IOException.class)
                .build();
    }
}

@Service
public class OrderServiceClient {
    private final Retry retry;
    private final WebClient webClient;

    public Order getOrder(Long orderId) {
        return Retry.decorateCallable(retry, () ->
                webClient.get()
                        .uri("http://order-service/api/orders/" + orderId)
                        .retrieve()
                        .bodyToMono(Order.class)
                        .block()
        ).call();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of retry patterns and your ability to configure them appropriately.

## ⚠️ Common Mistake
Retrying non-transient errors like 404 or validation errors, wasting resources and potentially causing issues.

## ⚡ Follow-up Questions

1. What is exponential backoff?
2. How do you determine which exceptions are retryable?
3. What is the difference between retry and circuit breaker?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Timeout Patterns](./07-timeout-patterns.md)
- [Testing Resilience](./10-testing-resilience.md)

## Navigation

[← Previous Question](./02-circuit-breaker-pattern.md) | [Next Question →](./04-bulkhead-pattern.md)
