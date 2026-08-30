# How do you handle database transactions in microservices? 🟠

## Question

How do you implement transactions across multiple microservices?

## Answer

Distributed transactions in microservices are challenging due to the lack of ACID guarantees across service boundaries. Approaches include the saga pattern (compensating transactions), two-phase commit (2PC) with XA transactions, and eventual consistency with event-driven updates. The saga pattern is the most common approach for microservices.

Implementation involves defining transaction steps with compensating actions, using messaging to coordinate steps, and implementing proper error handling and rollback mechanisms.

## Code Example

```java
@Service
public class OrderSaga {
    @Transactional
    public void createOrder(Order order) {
        // Local transaction for order creation
        orderRepository.save(order);
        
        // Compensating transaction if payment fails
        try {
            paymentService.processPayment(order);
        } catch (PaymentException e) {
            // Compensate: Cancel order
            orderRepository.delete(order);
            throw new SagaException("Payment failed", e);
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed transaction challenges and your ability to implement alternative approaches.

## ⚠️ Common Mistake
Trying to use traditional ACID transactions across service boundaries, which doesn't work in distributed systems.

## ⚡ Follow-up Questions

1. Why are ACID transactions difficult in microservices?
2. What is two-phase commit?
3. How does saga pattern ensure consistency?

## 📚 Related Topics

- [Saga Pattern](./06-saga-pattern.md)
- [Distributed Data Challenges](./03-distributed-data-challenges.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./11-multi-tenancy.md) | [Next Question →](./13-database-best-practices.md)
