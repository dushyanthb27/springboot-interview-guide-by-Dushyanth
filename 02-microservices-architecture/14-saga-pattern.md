# What is the saga pattern and how does it handle distributed transactions? 🔴

## Question

Explain the saga pattern for managing distributed transactions in microservices and compare choreography vs orchestration approaches.

## Answer

The saga pattern is a sequence of local transactions where each local transaction updates data within a single service and publishes an event or message to trigger the next local transaction in the saga. If a step fails, compensating transactions are executed to undo the changes made by preceding transactions, ensuring data consistency across services without distributed locks.

There are two saga coordination approaches: choreography (each service produces and listens to events, no central coordinator) and orchestration (a central saga orchestrator coordinates the saga, invoking services and managing compensation). Choreography is simpler for small sagas but becomes complex with many participants. Orchestration provides better visibility and control but introduces a single point of coordination.

## Code Example

```java
// Saga Pattern - Orchestration Approach
@Service
public class OrderSagaOrchestrator {
    @Autowired
    private PaymentService paymentService;
    @Autowired
    private InventoryService inventoryService;
    @Autowired
    private ShippingService shippingService;
    @Autowired
    private OrderRepository orderRepository;

    @Transactional
    public void executeOrderSaga(Order order) {
        try {
            // Step 1: Process payment
            paymentService.processPayment(order);
            order.setStatus(OrderStatus.PAYMENT_PROCESSED);
            orderRepository.save(order);

            // Step 2: Reserve inventory
            inventoryService.reserveItems(order.getItems());
            order.setStatus(OrderStatus.INVENTORY_RESERVED);
            orderRepository.save(order);

            // Step 3: Create shipment
            shippingService.createShipment(order);
            order.setStatus(OrderStatus.SHIPMENT_CREATED);
            orderRepository.save(order);

            // Mark saga as completed
            order.setStatus(OrderStatus.COMPLETED);
            orderRepository.save(order);

        } catch (PaymentException e) {
            // Payment failed - no compensation needed
            order.setStatus(OrderStatus.PAYMENT_FAILED);
            orderRepository.save(order);
            throw e;

        } catch (InventoryException e) {
            // Inventory failed - compensate payment
            compensatePayment(order);
            order.setStatus(OrderStatus.INVENTORY_FAILED);
            orderRepository.save(order);
            throw e;

        } catch (ShippingException e) {
            // Shipping failed - compensate inventory and payment
            compensateInventory(order);
            compensatePayment(order);
            order.setStatus(OrderStatus.SHIPPING_FAILED);
            orderRepository.save(order);
            throw e;
        }
    }

    private void compensatePayment(Order order) {
        try {
            paymentService.refundPayment(order);
        } catch (Exception e) {
            // Log error, continue compensation
            log.error("Failed to compensate payment for order {}", order.getId(), e);
        }
    }

    private void compensateInventory(Order order) {
        try {
            inventoryService.releaseItems(order.getItems());
        } catch (Exception e) {
            // Log error, continue compensation
            log.error("Failed to compensate inventory for order {}", order.getId(), e);
        }
    }
}

// Saga Pattern - Choreography Approach
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    @Autowired
    private ApplicationEventPublisher eventPublisher;

    @Transactional
    public Order createOrder(Order order) {
        Order savedOrder = orderRepository.save(order);
        eventPublisher.publishEvent(new OrderCreatedEvent(savedOrder));
        return savedOrder;
    }

    @EventListener
    public void handlePaymentFailed(PaymentFailedEvent event) {
        Order order = orderRepository.findById(event.getOrderId())
                .orElseThrow(() -> new OrderNotFoundException(event.getOrderId()));
        order.setStatus(OrderStatus.PAYMENT_FAILED);
        orderRepository.save(order);
    }

    @EventListener
    public void handleInventoryFailed(InventoryFailedEvent event) {
        Order order = orderRepository.findById(event.getOrderId())
                .orElseThrow(() -> new OrderNotFoundException(event.getOrderId()));
        order.setStatus(OrderStatus.INVENTORY_FAILED);
        orderRepository.save(order);
        // Trigger payment compensation
        eventPublisher.publishEvent(new CompensatePaymentEvent(order));
    }
}

@Service
public class PaymentService {
    @EventListener
    @Async
    public void handleOrderCreated(OrderCreatedEvent event) {
        try {
            processPayment(event.getOrder());
            eventPublisher.publishEvent(new PaymentProcessedEvent(event.getOrder()));
        } catch (PaymentException e) {
            eventPublisher.publishEvent(new PaymentFailedEvent(event.getOrder()));
        }
    }

    @EventListener
    public void handleCompensatePayment(CompensatePaymentEvent event) {
        refundPayment(event.getOrder());
    }
}

// Saga State Management
@Entity
@Table(name = "saga_instances")
public class SagaInstance {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String sagaType;
    private String sagaId;
    private SagaStatus status;
    private String currentState;
    private String payload;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    // Getters and setters
}

public enum SagaStatus {
    STARTED, COMPENSATING, COMPLETED, FAILED
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed transaction patterns and your ability to design complex coordination logic. They're looking for awareness of the trade-offs between choreography and orchestration and practical implementation considerations.

## ⚠️ Common Mistake
Not implementing proper compensation logic for all failure scenarios, leading to inconsistent state. Another mistake is not managing saga state properly, making it difficult to track saga progress and handle failures.

## ⚡ Follow-up Questions

1. How do you handle saga timeouts and retries?
2. What are the advantages of choreography over orchestration?
3. How do you ensure exactly-once processing in saga steps?

## 📚 Related Topics

- [Data Consistency](./05-data-consistency.md)
- [Event-Driven Architecture](./10-event-driven-architecture.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/README.md)

## Navigation

[← Previous Question](./13-monitoring-observability.md) | [Next Question →](./15-failure-design.md)
