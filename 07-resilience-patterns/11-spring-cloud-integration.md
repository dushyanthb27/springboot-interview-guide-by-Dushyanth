# How do you integrate Resilience4j with Spring Cloud Circuit Breaker? 🟡

## Question

How do you integrate Resilience4j with Spring Cloud Circuit Breaker annotations?

## Answer

Spring Cloud Circuit Breaker provides integration with Resilience4j through annotations like @CircuitBreaker, @Retry, and @Bulkhead. These annotations simplify resilience pattern implementation by automatically configuring the underlying Resilience4j components. The integration also provides Actuator endpoints for monitoring circuit breaker state.

Configuration includes adding the spring-cloud-starter-circuitbreaker-resilience4j dependency and using annotations on methods that need resilience. The annotations can be customized with properties to configure thresholds, timeouts, and fallback methods.

## Code Example

```java
// Dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>

// Usage with annotations
@Service
public class OrderService {
    @CircuitBreaker(name = "user-service", fallbackMethod = "getUserFallback")
    public User getUser(Long userId) {
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    }

    public User getUserFallback(Long userId, Exception e) {
        log.warn("User service unavailable, using fallback", e);
        return new User(userId, "Fallback User");
    }

    @Retry(name = "order-service", fallbackMethod = "createOrderFallback")
    public Order createOrder(Order order) {
        return restTemplate.postForObject("http://order-service/api/orders", order, Order.class);
    }

    public Order createOrderFallback(Order order, Exception e) {
        log.warn("Order service unavailable, using fallback", e);
        return new Order(order.getId(), OrderStatus.PENDING);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Cloud integration with Resilience4j and your ability to use annotations for simpler configuration.

## ⚠️ Common Mistake
Not understanding the fallback method signature requirements, leading to compilation errors.

## ⚡ Follow-up Questions

1. What are the different Spring Cloud Circuit Breaker annotations?
2. How do you customize annotation-based configuration?
3. What is the difference between programmatic and annotation-based configuration?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Retry Patterns](./03-retry-patterns.md)
- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)

## Navigation

[← Previous Question](./10-testing-resilience.md) | [Next Question →](./12-resilience4j-vs-hystrix.md)
