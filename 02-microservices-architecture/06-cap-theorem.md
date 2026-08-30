# What is the CAP theorem and how does it apply to microservices? 🟠

## Question

Explain the CAP theorem and its implications for designing microservices architectures.

## Answer

The CAP theorem states that a distributed data store can only simultaneously provide two out of three guarantees: Consistency (every read receives the most recent write or an error), Availability (every request receives a response, without guaranteeing it's the most recent write), and Partition Tolerance (the system continues to operate despite network partitions). In microservices, you must choose between CP (consistency and partition tolerance) and AP (availability and partition tolerance) as partition tolerance is mandatory in distributed systems.

For microservices, this means you must decide between strong consistency (CP) where services might become unavailable during partitions, or eventual consistency (AP) where services remain available but may return stale data during partitions. The choice depends on business requirements - financial systems often choose CP for data accuracy, while social media platforms often choose AP for availability.

## Code Example

```java
// CP System - Prioritizing Consistency
@Configuration
public class ConsistentConfig {

    @Bean
    public CacheManager consistentCacheManager() {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ZERO) // No caching, always fresh data
                .disableCachingNullValues();

        return RedisCacheManager.builder()
                .cacheDefaults(config)
                .transactionAware() // Ensure cache consistency
                .build();
    }
}

@Service
public class ConsistentOrderService {
    @Autowired
    private OrderRepository orderRepository;

    @Transactional
    public Order getOrder(Long id) {
        // Will throw exception if cannot ensure consistency
        return orderRepository.findById(id)
                .orElseThrow(() -> new OrderNotFoundException(id));
    }
}

// AP System - Prioritizing Availability
@Configuration
public class AvailableConfig {

    @Bean
    public CacheManager availableCacheManager() {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(5)) // Accept stale data
                .disableCachingNullValues();

        return RedisCacheManager.builder()
                .cacheDefaults(config)
                .build();
    }
}

@Service
public class AvailableOrderService {
    @Autowired
    private OrderRepository orderRepository;
    @Autowired
    private CacheManager cacheManager;

    @Cacheable(value = "orders", key = "#id")
    public Order getOrder(Long id) {
        try {
            return orderRepository.findById(id)
                    .orElse(null);
        } catch (Exception e) {
            // Return cached data or null to maintain availability
            Cache cache = cacheManager.getCache("orders");
            return cache != null ? cache.get(id, Order.class) : null;
        }
    }
}

// Hybrid Approach - Different consistency per use case
@Service
public class HybridOrderService {
    @Autowired
    private OrderRepository orderRepository;

    // Critical operations - CP
    @Transactional
    public Order processPayment(Long orderId) {
        Order order = orderRepository.findById(orderId)
                .orElseThrow(() -> new OrderNotFoundException(orderId));
        // Ensure consistency for financial operations
        return order;
    }

    // Non-critical operations - AP
    @Cacheable(value = "order-views", key = "#id")
    public Order getOrderView(Long id) {
        return orderRepository.findById(id).orElse(null);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed systems theory and your ability to apply it to microservices architecture. They're looking for awareness that you must make trade-offs between consistency and availability in distributed systems.

## ⚠️ Common Mistake

Thinking you can have all three CAP properties simultaneously in a distributed system. Another mistake is not understanding that partition tolerance is not optional in distributed systems - you must choose between C and A.

## ⚡ Follow-up Questions

1. How do you decide between CP and AP for a given service?
2. What is eventual consistency and how does it relate to the CAP theorem?
3. How does the BASE pattern differ from ACID transactions?

## 📚 Related Topics

- [Data Consistency](./05-data-consistency.md)
- [Communication Patterns](./04-communication-patterns.md)
- [12. Database Patterns](../12-database-patterns/README.md)

## Navigation

[← Previous Question](./05-data-consistency.md) | [Next Question →](./07-service-boundaries.md)
