# What is event-driven architecture and when would you use it? 🟠

## Question

Explain event-driven architecture in microservices and discuss when it's appropriate to use this pattern.

## Answer

Event-driven architecture (EDA) is a pattern where services communicate through events (messages) rather than direct calls, enabling loose coupling, asynchronous processing, and scalability. In EDA, services produce events when state changes occur and other services consume these events to trigger their own operations. This pattern is ideal for scenarios requiring loose coupling, eventual consistency, high scalability, and real-time processing.

Key components include event producers (services that generate events), event consumers (services that process events), event channels (message brokers like Kafka, RabbitMQ), and event schemas (contracts defining event structure). EDA is particularly useful for order processing, user notifications, data synchronization, and real-time analytics.

## Code Example

```java
// Event Producer
@Service
public class OrderEventPublisher {
    private final ApplicationEventPublisher eventPublisher;

    public void publishOrderCreated(Order order) {
        eventPublisher.publishEvent(new OrderCreatedEvent(order));
    }

    public void publishOrderCancelled(Order order) {
        eventPublisher.publishEvent(new OrderCancelledEvent(order));
    }
}

// Event Definition
public class OrderCreatedEvent {
    private final Order order;
    private final LocalDateTime timestamp;

    public OrderCreatedEvent(Order order) {
        this.order = order;
        this.timestamp = LocalDateTime.now();
    }

    // Getters
    public Order getOrder() { return order; }
    public LocalDateTime getTimestamp() { return timestamp; }
}

// Event Consumer
@Service
public class InventoryService {
    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        Order order = event.getOrder();
        reserveItems(order.getItems());
    }

    @EventListener
    public void handleOrderCancelled(OrderCancelledEvent event) {
        Order order = event.getOrder();
        releaseItems(order.getItems());
    }
}

// Message Broker Integration (RabbitMQ)
@Service
public class OrderMessagePublisher {
    private final RabbitTemplate rabbitTemplate;

    public void publishOrderCreated(Order order) {
        rabbitTemplate.convertAndSend("order.exchange", "order.created", order);
    }
}

@RabbitListener(queues = "order.created.queue")
public class NotificationService {
    public void handleOrderCreated(Order order) {
        sendOrderConfirmation(order);
    }
}

// Kafka Integration
@Service
public class OrderKafkaProducer {
    private final KafkaTemplate<String, Order> kafkaTemplate;

    public void publishOrderCreated(Order order) {
        kafkaTemplate.send("order-created", order.getId().toString(), order);
    }
}

@KafkaListener(topics = "order-created")
public class AnalyticsService {
    public void handleOrderCreated(Order order) {
        updateOrderAnalytics(order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of event-driven patterns and your ability to identify when this architecture is appropriate. They're looking for awareness of the benefits and challenges of asynchronous communication.

## ⚠️ Common Mistake

Using event-driven architecture for simple request-response scenarios where synchronous communication would be simpler. Another mistake is not handling event ordering and duplicate processing properly.

## ⚡ Follow-up Questions

1. How do you handle event ordering in an event-driven system?
2. What is the difference between event notification and event-carried state transfer?
3. How do you ensure at-least-once or exactly-once message delivery?

## 📚 Related Topics

- [Communication Patterns](./04-communication-patterns.md)
- [Data Consistency](./05-data-consistency.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/README.md)

## Navigation

[← Previous Question](./09-cross-cutting-concerns.md) | [Next Question →](./11-service-authentication.md)
