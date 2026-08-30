# How do you implement event sourcing? 🟠

## Question

How do you implement event sourcing for data persistence?

## Answer

Event sourcing stores the state of an entity as a sequence of events rather than its current state. To reconstruct the current state, all events are replayed. This provides a complete audit trail and enables temporal queries. Events are immutable and represent state changes.

Implementation involves creating event classes, implementing event store for persistence, creating aggregate roots that apply events, and implementing event handlers to update read models or trigger side effects.

## Code Example

```java
// Event
public class OrderCreatedEvent {
    private final Long orderId;
    private final Long userId;
    private final LocalDateTime createdAt;
}

// Aggregate root
@Entity
public class Order {
    @Id
    private Long id;
    private List<OrderEvent> events = new ArrayList<>();

    public void createOrder(Long userId) {
        OrderCreatedEvent event = new OrderCreatedEvent(id, userId, LocalDateTime.now());
        applyEvent(event);
        events.add(new OrderEvent(event));
    }

    private void applyEvent(OrderCreatedEvent event) {
        // Apply event to change state
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of event sourcing and your ability to implement event-based persistence.

## ⚠️ Common Mistake
Not implementing proper event versioning, leading to issues when event schemas change.

## ⚡ Follow-up Questions

1. What are the benefits of event sourcing?
2. How do you handle event versioning?
3. How do you optimize event replay performance?

## 📚 Related Topics

- [CQRS Pattern](./04-cqrs-pattern.md)
- [Saga Pattern](./06-saga-pattern.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/README.md)

## Navigation

[← Previous Question](./04-cqrs-pattern.md) | [Next Question →](./06-saga-pattern.md)
