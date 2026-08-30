# What is the saga pattern for data consistency? 🟠

## Question

How does the saga pattern maintain data consistency across multiple services?

## Answer

The saga pattern implements distributed transactions through a sequence of local transactions, each accompanied by a compensating transaction. If a step fails, compensating transactions undo the previous steps. Sagas can be choreography-based (events trigger next steps) or orchestration-based (central coordinator controls flow).

Implementation involves defining saga steps with compensating actions, publishing events for step completion, and implementing event listeners to trigger next steps or compensations.

## Code Example

```java
@Service
public class OrderSaga {
    @Transactional
    public void createOrder(Order order) {
        // Step 1: Create order
        orderRepository.save(order);
        eventPublisher.publishEvent(new OrderCreatedEvent(order));

        // Step 2: Reserve inventory
        try {
            inventoryService.reserveInventory(order);
            eventPublisher.publishEvent(new InventoryReservedEvent(order));
        } catch (Exception e) {
            // Compensate: Cancel order
            orderRepository.delete(order);
            throw new SagaException("Failed to reserve inventory", e);
        }
    }

    @EventListener
    public void handleInventoryReserved(InventoryReservedEvent event) {
        // Step 3: Process payment
        try {
            paymentService.processPayment(event.getOrder());
        } catch (Exception e) {
            // Compensate: Release inventory
            inventoryService.releaseInventory(event.getOrder());
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the saga pattern and your ability to implement distributed transactions.

## ⚠️ Common Mistake
Not implementing proper compensating transactions, leading to inconsistent state when saga steps fail.

## ⚡ Follow-up Questions

1. What is the difference between choreography and orchestration?
2. How do you handle saga timeouts?
3. What are the challenges of saga pattern?

## 📚 Related Topics

- [Distributed Data Challenges](./03-distributed-data-challenges.md)
- [Event Sourcing](./05-event-sourcing.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/13-saga-pattern.md)

## Navigation

[← Previous Question](./05-event-sourcing.md) | [Next Question →](./07-read-replicas.md)
