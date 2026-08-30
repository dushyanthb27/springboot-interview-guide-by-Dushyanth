# How do you design for failure in microservices architecture? 🔴

## Question

What are the key principles and patterns for designing resilient microservices that can handle failures gracefully?

## Answer

Designing for failure in microservices involves implementing resilience patterns including circuit breakers (prevent cascading failures by stopping calls to failing services), retries (handle transient failures with exponential backoff), bulkheads (isolate resources to prevent failures in one area from affecting others), timeouts (prevent indefinite waiting), and fallbacks (provide alternative functionality when primary service fails).

Additional patterns include graceful degradation (reduce functionality rather than complete failure), dead letter queues (handle failed messages), health checks (detect and remove unhealthy instances), and chaos engineering (proactively test failure scenarios). The goal is to build systems that are fault-tolerant, self-healing, and can continue operating even when individual components fail.

## Code Example

```java
// Circuit Breaker Pattern
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

// Retry Pattern with Exponential Backoff
@Configuration
public class RetryConfig {

    @Bean
    public Retry retry() {
        return Retry.custom()
                .maxAttempts(3)
                .waitDuration(Duration.ofMillis(1000))
                .retryOnException(Exception.class)
                .retryOnException(WebClientResponseException.class)
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

// Bulkhead Pattern
@Configuration
public class BulkheadConfig {

    @Bean
    public Bulkhead bulkhead() {
        BulkheadConfig config = BulkheadConfig.custom()
                .maxConcurrentCalls(10)
                .maxWaitDuration(Duration.ofMillis(500))
                .build();
        return Bulkhead.of("service-bulkhead", config);
    }
}

// Fallback Pattern
@Service
public class UserServiceClient {
    private final CircuitBreaker circuitBreaker;
    private final RestTemplate restTemplate;
    private final UserCacheService userCacheService;

    public User getUser(Long userId) {
        return circuitBreaker.executeSupplier(() -> {
            try {
                return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
            } catch (Exception e) {
                // Fallback to cache
                return userCacheService.getCachedUser(userId)
                        .orElseThrow(() -> new ServiceUnavailableException("User service unavailable"));
            }
        });
    }
}

// Timeout Configuration
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

// Health Check
@Component
public class CustomHealthIndicator implements HealthIndicator {
    @Autowired
    private UserServiceClient userServiceClient;

    @Override
    public Health health() {
        try {
            userServiceClient.healthCheck();
            return Health.up().withDetail("user-service", "Available").build();
        } catch (Exception e) {
            return Health.down(e).withDetail("user-service", "Unavailable").build();
        }
    }
}

// Dead Letter Queue
@RabbitListener(queues = "order.queue")
public class OrderProcessor {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    public void processOrder(Order order) {
        try {
            processOrder(order);
        } catch (Exception e) {
            // Send to dead letter queue
            rabbitTemplate.convertAndSend("order.dlq", order);
            log.error("Failed to process order {}, sent to DLQ", order.getId(), e);
        }
    }
}

// Graceful Degradation
@Service
public class RecommendationService {
    @Autowired
    private RecommendationClient recommendationClient;
    @Autowired
    private PopularItemsCache popularItemsCache;

    public List<Item> getRecommendations(Long userId) {
        try {
            return recommendationClient.getRecommendations(userId);
        } catch (Exception e) {
            log.warn("Recommendation service unavailable, returning popular items");
            return popularItemsCache.getPopularItems();
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of resilience patterns and your ability to design systems that can handle failures gracefully. They're looking for awareness of multiple resilience patterns and how they work together to create robust systems.

## ⚠️ Common Mistake
Implementing only one resilience pattern (like retries) without considering others (like circuit breakers), which can actually make things worse under heavy load. Another mistake is not setting appropriate timeouts and retry policies.

## ⚡ Follow-up Questions

1. How do you determine the right circuit breaker thresholds?
2. What is the difference between circuit breakers and bulkheads?
3. How do you test failure scenarios in production?

## 📚 Related Topics

- [07. Resilience Patterns](../07-resilience-patterns/README.md)
- [Monitoring and Observability](./13-monitoring-observability.md)
- [Communication Patterns](./04-communication-patterns.md)

## Navigation

[← Previous Question](./14-saga-pattern.md) | [Back to Section](./README.md)
