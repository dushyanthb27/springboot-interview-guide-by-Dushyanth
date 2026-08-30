# How do you implement message tracing? 🟡

## Question

How do you implement distributed tracing for message-driven microservices?

## Answer

Message tracing is implemented using Spring Cloud Sleuth, which automatically adds trace context to messages. Sleuth propagates trace IDs and span IDs through message headers, enabling end-to-end tracing across message boundaries. This helps track message flow through the system.

Configuration includes enabling Sleuth, configuring the message broker integration, and ensuring trace context headers are preserved. Sleuth automatically instruments RabbitMQ and Kafka without additional code.

## Code Example

```java
// Automatic tracing with Sleuth - no code changes needed
@Service
public class OrderProducer {
    private final RabbitTemplate rabbitTemplate;

    public void sendOrder(Order order) {
        // Sleuth automatically adds trace headers
        rabbitTemplate.convertAndSend("order.exchange", "order.created", order);
    }
}

@Service
public class OrderConsumer {
    @RabbitListener(queues = "order.queue")
    public void handleOrder(Order order) {
        // Sleuth automatically extracts trace context
        // and continues the trace
        processOrder(order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of message tracing and your ability to track message flow.

## ⚠️ Common Mistake
Removing trace headers during message transformation, breaking the trace continuity.

## ⚡ Follow-up Questions

1. How does Sleuth add trace context to messages?
2. What headers are used for message tracing?
3. How do you view message traces in Zipkin?

## 📚 Related Topics

- [10. Distributed Tracing](../10-distributed-tracing/README.md)
- [Message Patterns](./05-message-patterns.md)
- [Best Practices](./14-best-practices.md)

## Navigation

[← Previous Question](./09-idempotency.md) | [Next Question →](./11-message-ordering.md)
