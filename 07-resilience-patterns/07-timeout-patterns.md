# How do you implement timeout patterns? 🟡

## Question

How do you implement timeout patterns to prevent indefinite waiting for unresponsive services?

## Answer

Timeout patterns prevent operations from waiting indefinitely for unresponsive services. They set maximum wait times for operations, after which the operation is cancelled and an exception is thrown. Timeouts are essential for preventing resource exhaustion and maintaining system responsiveness.

Configuration includes setting connection timeouts, read timeouts, and overall operation timeouts. Timeouts should be set based on expected service response times and SLA requirements.

## Code Example

```java
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient() {
        HttpClient httpClient = HttpClient.create()
                .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 5000)
                .responseTimeout(Duration.ofSeconds(5));

        return WebClient.builder()
                .clientConnector(new ReactorClientHttpConnector(httpClient))
                .baseUrl("http://user-service")
                .build();
    }
}

@Service
public class OrderServiceClient {
    private final WebClient webClient;

    public Order getOrder(Long orderId) {
        return webClient.get()
                .uri("/api/orders/" + orderId)
                .retrieve()
                .bodyToMono(Order.class)
                .block(Duration.ofSeconds(5));
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of timeout configuration and your ability to prevent indefinite waiting.

## ⚠️ Common Mistake
Setting timeouts too short (causing unnecessary failures) or too long (defeating the purpose of timeouts).

## ⚡ Follow-up Questions

1. How do you determine appropriate timeout values?
2. What is the difference between connection timeout and read timeout?
3. How do you handle timeout exceptions?

## 📚 Related Topics

- [Retry Patterns](./03-retry-patterns.md)
- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./06-fallback-mechanisms.md) | [Next Question →](./08-monitoring-resilience.md)
