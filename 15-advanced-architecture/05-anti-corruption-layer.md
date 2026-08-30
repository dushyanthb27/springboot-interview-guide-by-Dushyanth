# How do you implement the Anti-Corruption Layer? 🟠

## Question

How do you implement the Anti-Corruption Layer for integrating with legacy systems?

## Answer

The Anti-Corruption Layer (ACL) isolates a new system from legacy systems by translating between different domain models and APIs. It prevents the legacy system's design from contaminating the new system's design. The ACL can be implemented as a separate service or a facade layer.

Implementation involves creating a translation layer that converts between the legacy system's data model and the new system's domain model, implementing the ACL as a separate service or module, and ensuring the new system doesn't depend on legacy system APIs directly.

## Code Example

```java
@Service
public class LegacyOrderAdapter {
    private final LegacyOrderClient legacyClient;

    public Order convertLegacyOrder(LegacyOrder legacyOrder) {
        Order order = new Order();
        order.setId(legacyOrder.getOrderId());
        order.setAmount(legacyOrder.getTotalAmount());
        // Convert legacy data model to new domain model
        return order;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the Anti-Corruption Layer and your ability to isolate legacy systems.

## ⚠️ Common Mistake
Allowing legacy system design to influence the new system's domain model, creating tight coupling.

## ⚡ Follow-up Questions

1. When should you use an Anti-Corruption Layer?
2. How do you handle data model translation?
3. What are the alternatives to ACL?

## 📚 Related Topics

- [Strangler Fig](./02-strangler-fig.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)
- [12. Database Patterns](../12-database-patterns/README.md)

## Navigation

[← Previous Question](./04-ambassador-pattern.md) | [Next Question →](./06-scalability-design.md)
