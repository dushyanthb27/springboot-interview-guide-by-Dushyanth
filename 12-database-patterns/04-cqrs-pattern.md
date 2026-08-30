# How do you implement CQRS pattern? 🟠

## Question

How do you implement the Command Query Responsibility Segregation (CQRS) pattern?

## Answer

CQRS separates read and write operations into different models. Write operations use commands to modify data, while read operations use queries to retrieve data from optimized read models. This pattern improves performance by allowing different data structures for reads and writes.

Implementation involves creating separate command and query models, implementing command handlers for write operations, using event-driven updates to synchronize read models, and optimizing read models for query performance.

## Code Example

```java
// Command model
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String status;
    // Write-optimized fields
}

// Read model
@Entity
public class OrderView {
    @Id
    private Long id;
    private String status;
    private String userName; // Denormalized for reads
    private List<OrderItemView> items;
}

// Command handler
@Service
public class OrderCommandHandler {
    public void createOrder(CreateOrderCommand command) {
        Order order = new Order();
        order.setStatus("CREATED");
        orderRepository.save(order);
        eventPublisher.publishEvent(new OrderCreatedEvent(order));
    }
}

// Query handler
@Service
public class OrderQueryHandler {
    public OrderView getOrder(Long orderId) {
        return orderViewRepository.findById(orderId).orElseThrow();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of CQRS and your ability to implement separate read/write models.

## ⚠️ Common Mistake
Not properly synchronizing read models with write models, leading to stale data.

## ⚡ Follow-up Questions

1. When should you use CQRS?
2. How do you synchronize read and write models?
3. What are the trade-offs of CQRS?

## 📚 Related Topics

- [Event Sourcing](./05-event-sourcing.md)
- [Read Replicas](./07-read-replicas.md)
- [Caching Strategies](./10-caching-strategies.md)

## Navigation

[← Previous Question](./03-distributed-data-challenges.md) | [Next Question →](./05-event-sourcing.md)
