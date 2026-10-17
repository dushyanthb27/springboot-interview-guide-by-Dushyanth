# What are resilience patterns and why are they important? 🟢

## Question

What are resilience patterns and why are they important in microservices architecture? Explain the most common resilience patterns and when to use each one.

## Answer

Resilience patterns are design patterns that help distributed systems handle failures gracefully and maintain availability despite component failures. They prevent cascading failures, improve system reliability, and provide better user experience during partial outages.

**Why resilience patterns are critical in microservices:**

Distributed systems have more failure points than monoliths: network latency, service failures, resource exhaustion, partial outages, and distributed coordination issues. Without resilience patterns, a single failing service can cascade through the system, causing widespread outages. Resilience patterns help systems degrade gracefully rather than fail completely.

**Common resilience patterns:**

1. **Circuit Breaker**: Prevents cascading failures by stopping calls to a failing service after a threshold of failures. When the circuit is open, calls fail fast immediately rather than timing out. Used when calling unreliable external services.

2. **Retry**: Automatically retries failed requests with exponential backoff to handle transient failures. Used for network calls that may fail temporarily due to timeouts or temporary overload.

3. **Bulkhead**: Isolates resources (thread pools, connections) to prevent one component from consuming all resources. Used when multiple services share resources and you want to prevent one from starving others.

4. **Timeout**: Sets a maximum time for operations to complete, preventing threads from hanging indefinitely. Used for all external calls to prevent resource exhaustion.

5. **Fallback**: Provides alternative behavior when a service fails (e.g., cached data, default values, alternative service). Used when you need to maintain functionality even when dependencies are down.

6. **Rate Limiting**: Limits the rate of requests to prevent overwhelming services. Used to protect your service from being overwhelmed by client requests or to respect API rate limits.

7. **Backpressure**: Signals to upstream components to slow down when downstream components can't keep up. Used in streaming systems to prevent buffer overflow.

## Code Example

```java
// Circuit Breaker pattern using Resilience4j
@Service
public class OrderService {

    private final UserServiceClient userServiceClient;
    private final CircuitBreaker circuitBreaker;

    public OrderService(UserServiceClient userServiceClient) {
        this.userServiceClient = userServiceClient;
        this.circuitBreaker = CircuitBreaker.ofDefaults("userService");
    }

    public Order createOrder(Order order) {
        User user = circuitBreaker.executeSupplier(() ->
            userServiceClient.getUser(order.getUserId())
        );
        // Process order with user data
        return processOrder(order, user);
    }
}

// Retry pattern with exponential backoff
@Service
public class PaymentService {

    @Retry(
        name = "paymentRetry",
        retryOn = PaymentException.class,
        maxAttempts = 3,
        backoff = @Backoff(delay = 1000, multiplier = 2)
    )
    public PaymentResult processPayment(Payment payment) {
        // May fail temporarily due to network issues
        return paymentGateway.charge(payment);
    }
}

// Bulkhead pattern - isolates thread pools
@Service
public class InventoryService {

    @Bulkhead(name = "inventory", type = Bulkhead.Type.THREADPOOL)
    public Inventory checkInventory(String productId) {
        // Uses isolated thread pool to prevent resource exhaustion
        return inventoryRepository.findByProductId(productId);
    }
}

// Fallback pattern
@Service
public class OrderService {

    @CircuitBreaker(fallbackMethod = "getUserFromCache")
    public User getUser(Long userId) {
        return userServiceClient.getUser(userId);
    }

    private User getUserFromCache(Long userId, Exception e) {
        // Fallback to cached data when service is down
        return userCache.get(userId);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of resilience patterns and their importance in distributed systems. They're looking for knowledge of specific patterns and when to apply them. A strong answer demonstrates that you understand that resilience is about graceful degradation rather than preventing failures entirely.

## ⚠️ Common Mistake

Confusing resilience with fault tolerance. Resilience is about graceful degradation—continuing to function (perhaps in a degraded state) despite failures. Fault tolerance is about preventing failures from affecting the system at all. Another mistake is implementing resilience patterns without understanding the trade-offs (e.g., retries can amplify load on a struggling service).

## ⚡ Follow-up Questions

1. How does the circuit breaker pattern prevent cascading failures?
2. What is the difference between resilience and fault tolerance?
3. When would you use retries vs circuit breakers?
4. How do you test resilience patterns in your application?
5. What are the trade-offs of implementing resilience patterns (e.g., increased complexity, latency)?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Retry Patterns](./03-retry-patterns.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-circuit-breaker-pattern.md)
