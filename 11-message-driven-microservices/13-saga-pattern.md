# How do you implement saga pattern with messaging? 🟠

## Question

How do you implement the saga pattern for distributed transactions using messaging?

## Answer

The saga pattern implements distributed transactions through a sequence of local transactions, each accompanied by a compensating transaction. In message-driven systems, sagas are implemented using choreography (events trigger next steps) or orchestration (central coordinator controls flow). Messaging enables asynchronous execution of saga steps.

Implementation involves defining saga steps with compensating actions, publishing events for each step completion, and listening for events to trigger next steps or compensations.

## Code Example

```java
@Service
public class OrderSaga {
    @RabbitListener(queues = "order.created")
    public void handleOrderCreated(Order order) {
        try {
            // Step 1: Reserve inventory
            inventoryService.reserveInventory(order);
            rabbitTemplate.convertAndSend("inventory.reserved", order);

            // Step 2: Process payment
            paymentService.processPayment(order);
            rabbitTemplate.convertAndSend("payment.processed", order);

            // Step 3: Confirm order
            orderService.confirmOrder(order);
        } catch (Exception e) {
            // Compensating transactions
            rabbitTemplate.convertAndSend("order.failed", order);
        }
    }

    @RabbitListener(queues = "order.failed")
    public void handleOrderFailed(Order order) {
        // Compensate: Release inventory
        inventoryService.releaseInventory(order);
        // Compensate: Refund payment
        paymentService.refundPayment(order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the saga pattern and your ability to implement distributed transactions with messaging.

## ⚠️ Common Mistake
Not implementing proper compensating transactions, leading to inconsistent state when saga steps fail.

## ⚡ Follow-up Questions

1. What is the difference between choreography and orchestration?
2. How do you handle saga timeouts?
3. What are the challenges of saga pattern?

## 📚 Related Topics

- [02. Microservices Architecture](../02-microservices-architecture/14-saga-pattern.md)
- [Idempotency](./09-idempotency.md)
- [Error Handling](./07-error-handling.md)

## Navigation

[← Previous Question](./12-consumer-scaling.md) | [Next Question →](./14-best-practices.md)
