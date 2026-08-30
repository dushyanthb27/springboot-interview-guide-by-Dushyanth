# How do you implement read replicas? 🟡

## Question

How do you implement read replicas to scale database read operations?

## Answer

Read replicas are copies of the primary database that handle read operations, reducing load on the primary. In Spring Boot, read replicas can be configured using a routing datasource that directs read operations to replicas and write operations to the primary. This improves performance and scalability for read-heavy applications.

Configuration involves setting up multiple datasources (primary and replicas), implementing a routing datasource based on transaction type (read-only vs read-write), and configuring replication in the database server.

## Code Example

```java
@Configuration
public class DataSourceConfig {
    @Bean
    @Primary
    public DataSource routingDataSource(
            @Qualifier("primaryDataSource") DataSource primary,
            @Qualifier("replicaDataSource") DataSource replica) {
        Map<Object, Object> targetDataSources = new HashMap<>();
        targetDataSources.put("PRIMARY", primary);
        targetDataSources.put("REPLICA", replica);

        RoutingDataSource routingDataSource = new RoutingDataSource();
        routingDataSource.setTargetDataSources(targetDataSources);
        routingDataSource.setDefaultTargetDataSource(primary);
        return routingDataSource;
    }
}

// Use @Transactional(readOnly = true) for read operations
@Service
public class OrderService {
    @Transactional(readOnly = true)
    public Order getOrder(Long orderId) {
        // Routes to replica
        return orderRepository.findById(orderId).orElseThrow();
    }

    @Transactional
    public Order createOrder(Order order) {
        // Routes to primary
        return orderRepository.save(order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of read replicas and your ability to configure routing datasources.

## ⚠️ Common Mistake
Not configuring proper replication lag handling, leading to stale data reads.

## ⚡ Follow-up Questions

1. How do you handle replication lag?
2. What is the difference between read replicas and sharding?
3. How do you monitor replica health?

## 📚 Related Topics

- [CQRS Pattern](./04-cqrs-pattern.md)
- [Caching Strategies](./10-caching-strategies.md)
- [Database Sharding](./09-database-sharding.md)

## Navigation

[← Previous Question](./06-saga-pattern.md) | [Next Question →](./08-database-migrations.md)
