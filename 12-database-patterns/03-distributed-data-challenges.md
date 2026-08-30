# What are the challenges of distributed data management? 🟡

## Question

What are the main challenges of managing data across multiple microservices?

## Answer

Distributed data management challenges include maintaining data consistency across services (eventual consistency vs strong consistency), implementing distributed transactions, handling queries that need data from multiple services, managing data synchronization, and dealing with network partitions and failures.

Other challenges include implementing compensating transactions for rollbacks, managing data versioning across services, and ensuring data integrity without shared database constraints.

## Code Example

```java
// Challenge: Querying data across services
// Bad: Direct database access (violates database per service)
// Good: API calls or messaging

@Service
public class OrderService {
    private final UserServiceClient userServiceClient;
    private final OrderRepository orderRepository;

    public OrderWithUser getOrderWithUser(Long orderId) {
        Order order = orderRepository.findById(orderId).orElseThrow();
        User user = userServiceClient.getUser(order.getUserId());
        return new OrderWithUser(order, user);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed data challenges and your ability to address them.

## ⚠️ Common Mistake
Trying to maintain strong consistency across services, which is difficult and often unnecessary.

## ⚡ Follow-up Questions

1. What is eventual consistency?
2. How do you implement distributed transactions?
3. What is the CAP theorem?

## 📚 Related Topics

- [Database per Service](./01-database-per-service.md)
- [Saga Pattern](./06-saga-pattern.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./02-implementing-database-per-service.md) | [Next Question →](./04-cqrs-pattern.md)
