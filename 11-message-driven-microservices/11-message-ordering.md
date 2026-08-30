# How do you implement message ordering? 🟠

## Question

How do you ensure message ordering in message-driven microservices?

## Answer

Message ordering can be guaranteed using single-partition topics in Kafka or single consumer in RabbitMQ. Kafka guarantees ordering within a partition, so messages with the same key go to the same partition. RabbitMQ guarantees ordering within a single queue but not across multiple consumers.

Implementation involves using message keys for partitioning in Kafka, using single consumer for ordered processing, or implementing application-level ordering with sequence numbers.

## Code Example

```java
// Kafka: Use message key for ordering
@Service
public class OrderProducer {
    private final KafkaTemplate<String, Order> kafkaTemplate;

    public void sendOrder(Order order) {
        // Use user ID as key to ensure all orders for a user go to same partition
        kafkaTemplate.send("order-topic", order.getUserId().toString(), order);
    }
}

// RabbitMQ: Single consumer for ordered processing
@RabbitListener(queues = "order.queue", concurrency = "1")
public void handleOrder(Order order) {
    // Process order - single consumer ensures ordering
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of message ordering guarantees and your ability to implement ordered processing.

## ⚠️ Common Mistake
Assuming global ordering in Kafka, which only guarantees ordering within a partition.

## ⚡ Follow-up Questions

1. How does Kafka guarantee ordering within partitions?
2. What is the trade-off between ordering and throughput?
3. How do you implement application-level ordering?

## 📚 Related Topics

- [Kafka Implementation](./03-kafka-implementation.md)
- [Consumer Scaling](./12-consumer-scaling.md)
- [Message Patterns](./05-message-patterns.md)

## Navigation

[← Previous Question](./10-message-tracing.md) | [Next Question →](./12-consumer-scaling.md)
