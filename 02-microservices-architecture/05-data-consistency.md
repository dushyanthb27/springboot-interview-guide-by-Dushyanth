# How do you handle data consistency in distributed microservices? 🟠

## Question

What are the challenges of maintaining data consistency across microservices and what patterns can be used to address them?

## Answer

Data consistency in microservices is challenging because each service typically has its own database, making traditional ACID transactions across services impossible. Patterns to address this include eventual consistency (accepting that data may be temporarily inconsistent but will converge), Saga pattern (breaking transactions into local transactions with compensating actions), event sourcing (storing state changes as events), and CQRS (separating read and write models).

The Saga pattern is particularly important for distributed transactions, using either choreography (events trigger compensating transactions) or orchestration (a central coordinator manages the saga). Event sourcing with CQRS provides strong consistency guarantees for complex business logic but adds complexity. The choice depends on consistency requirements, business criticality, and complexity tolerance.

## Code Example

```java
// Saga Pattern - Choreography approach
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    @Autowired
    private ApplicationEventPublisher eventPublisher;

    @Transactional
    public Order createOrder(Order order) {
        // Local transaction for order
        Order savedOrder = orderRepository.save(order);
        // Publish event to trigger other services
        eventPublisher.publishEvent(new OrderCreatedEvent(savedOrder));
        return savedOrder;
    }
}

@EventListener
public class PaymentService {
    @Transactional
    public void handleOrderCreated(OrderCreatedEvent event) {
        try {
            processPayment(event.getOrder());
        } catch (PaymentException e) {
            // Publish compensation event
            eventPublisher.publishEvent(new PaymentFailedEvent(event.getOrder()));
        }
    }
}

@EventListener
public class InventoryService {
    @Transactional
    public void handleOrderCreated(OrderCreatedEvent event) {
        try {
            reserveItems(event.getOrder().getItems());
        } catch (InventoryException e) {
            eventPublisher.publishEvent(new InventoryFailedEvent(event.getOrder()));
        }
    }
}

// Saga Pattern - Orchestration approach
@Service
public class OrderSagaOrchestrator {
    @Autowired
    private PaymentService paymentService;
    @Autowired
    private InventoryService inventoryService;
    @Autowired
    private ShippingService shippingService;

    public void executeOrderSaga(Order order) {
        try {
            // Step 1: Process payment
            paymentService.processPayment(order);

            // Step 2: Reserve inventory
            inventoryService.reserveItems(order.getItems());

            // Step 3: Create shipment
            shippingService.createShipment(order);

            // Mark saga as completed
            order.setStatus(OrderStatus.COMPLETED);
        } catch (Exception e) {
            // Compensating transactions
            compensateOrder(order);
        }
    }

    private void compensateOrder(Order order) {
        try {
            shippingService.cancelShipment(order);
        } catch (Exception e) {
            // Log error, continue compensation
        }
        try {
            inventoryService.releaseItems(order.getItems());
        } catch (Exception e) {
            // Log error, continue compensation
        }
        try {
            paymentService.refundPayment(order);
        } catch (Exception e) {
            // Log error
        }
        order.setStatus(OrderStatus.FAILED);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed data consistency challenges and your knowledge of patterns to address them. They're looking for awareness that traditional ACID transactions don't work across microservices and familiarity with alternative approaches.

## ⚠️ Common Mistake

Trying to use distributed transactions (2PC) across microservices, which kills performance and availability. Another mistake is not understanding the difference between strong and eventual consistency and when each is appropriate.

## ⚡ Follow-up Questions

1. What is the difference between choreography and orchestration in the Saga pattern?
2. How do you handle compensation when a saga step fails?
3. What is event sourcing and how does it help with data consistency?

## 📚 Related Topics

- [Saga Pattern](./14-saga-pattern.md)
- [CAP Theorem](./06-cap-theorem.md)
- [Event-Driven Architecture](./10-event-driven-architecture.md)

## Navigation

[← Previous Question](./04-communication-patterns.md) | [Next Question →](./06-cap-theorem.md)
